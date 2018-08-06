---
date: 2010-08-24T00:00:00Z
published: true
status: publish
tags:
- couchdb
- nosql
- zend-framework
- php
title: Zend Framework a NoSQL pro ukládání logů ve webových aplikací, díl prvnío CouchDb
type: post
url: /2010/08/24/zend-framework-a-nosql-pro-ukladani-logu-ve-webovych-aplikaci-dil-1-couchdb/
---

Použití databáze pro ukládání logů se používá často pro analýzu logů. Technicky napojit databázi RBMS (MySQL) pomocí Zend_Log_Writer_Db není žádný problém. Ale vidím tu nevýhodu v tom, že musíte mít schema podle toho co ukládáte za logy. Pokud se rozhodnete použít NOSQL databázi (CouchDb, MongoDb) nemusíte se o schema starat.

V tomto článku si napsat vlastní Zend_Log_Writer pro CouchDb a jak si potom lehce zobrazíme příslušné logy. Napíšeme si také jednoduchou map funkci pro view v CouchDb, kterou v aplikaci použijeme.

Nejprve si projedeme jak se používá <a href="https://framework.zend.com/manual/en/zend.log.writers.html">Zend_Log_Writer_Db, syntaxi najdete v manuálu</a>. Tabulka v db musí mít pevně dané schéma, která namapujete v konfiguraci writeru.

Implementaci vlastního App_Log_Writer_CouchDb provedeme třeba takto:
<pre class="code php">class App_Log_Writer_CouchDb extends Zend_Log_Writer_Abstract
{
    /**
     * Db
     * @var Phly_Couch
     */
    private $_db;
    /**
     * CouchDb host localhost default
     * @var string
     */
    private $_host;
    /**
     * Couchdb port 3184 default
     * @var int
     */
    private $_port;
    /**
     *
     * @param array $params
     * @return void
     */
    public function __construct($dbname, $options = null)
    {
        if (is_null($options)) {
            $options['host'] = "localhost";
            $options['port'] = "5984";
            $options['db'] = $dbname;
        }
        $this-&gt;_db = new Phly_Couch($options);
    }

    static public function factory($config)
    {
        $config = self::_parseConfig($config);
        return $config;
    }
    /**
     * @param array $event
     * @return void
     */
    protected function _write($event)
    {
        // action body
        $doc = new Phly_Couch_Document($event);
        $result = $this-&gt;_db-&gt;docSave($doc);
        // return array ok, id, rev
        $info = $result-&gt;getInfo();
        if (!$info['ok']) {
            throw new Zend_Log_Exception("Error in save to CouchDb");
        }
    }

}</pre>
Není na tom jak vidíte nic složitého, implementuje funkce __constructor, factory a _write funkci a je to.

Samozřejmě je to kus Zend Frameworku, který potrebuje konfiguraci.
<pre class="code">autoloaderNamespaces.phly = "Phly_"
autoloaderNamespaces.app = "App_"
; Options for CouchDb
couchdb.host = prskavec.couchone.com
couchdb.port = 80
couchdb.db = "test-log"
</pre>
Pro správu CouchDb jsem použil vestavěný Futon.

<a href="https://blog.prskavec.net/wp-content/uploads/2010/08/Screen-shot-2010-08-22-at-8.04.37.png"><img class="aligncenter size-medium wp-image-2613" title="CouchDb" src="https://blog.prskavec.net/wp-content/uploads/2010/08/Screen-shot-2010-08-22-at-8.04.37-300x187.png" alt="" width="300" height="187" /></a>

Vytvořil jsem testovací db <strong>"test-log"</strong> a v ní pohled, který v kódu potom používám. Vytvoříte temporary_view, které potom při ukládání ložíte do příslušného designu a view. U mě to bylo použité <code>logger/log_by_prior</code>.
<pre class="jush">  function(doc) {
    if (doc.priority) {
       emit(doc.priority, [doc.priorityName, doc.timestamp, doc.message, doc.module, doc.controller]);
    }
  }
</pre>
Toto view potom volá controller, který načítá data pro zobrazení dat z databáze.
<pre class="code">class IndexController extends Zend_Controller_Action
{
    protected $_config;

    public function preDispatch()
    {
            $this-&gt;_config = new Zend_Config_Ini('../application/configs/'.
                    'application.ini', APPLICATION_ENV);
    }

    public function indexAction()
    {
         $db = new Phly_Couch($this-&gt;_config-&gt;couchdb);

         $this-&gt;view-&gt;form = $form = new App_Form_Filter();
         if ($this-&gt;getRequest()-&gt;isPost()) {
             $formData = $this-&gt;getRequest()-&gt;getPost();
             if ($form-&gt;isValid($formData)) {
                 $filterValue = $form-&gt;getValue('filter');
                 Zend_Debug::dump($filterValue);
                 if (empty($filterValue) &amp;&amp; $filterValue===0) $filterValue = null;
                 $result = $db-&gt;view('logger','log_by_prior', $filterValue, array("db"=&gt;$this-&gt;_config-&gt;couchdb-&gt;db));
             } else {
             $form-&gt;populate($formData);
             }
         } else {
            $result = $db-&gt;view('logger','log_by_prior', null, array("db"=&gt;$this-&gt;_config-&gt;couchdb-&gt;db));
         }
         $this-&gt;view-&gt;docs = $result-&gt;toArray();
         $this-&gt;view-&gt;messages = $this-&gt;_helper-&gt;flashMessenger-&gt;getMessages();

         $logger = new Zend_Log();
         $r = new ReflectionClass($logger);
         $this-&gt;view-&gt;priorities = array_flip($r-&gt;getConstants());

    }

    public function logAction()
    {
          $id = $this-&gt;_request-&gt;getParam('id', 0);

          $logger = new Zend_Log();
          $format = '%timestamp% %priorityName% (%priority%): '.
            '[%module%] [%controller%] %message%';
          $formatter = new Zend_Log_Formatter_Simple($format);

          $writer = new App_Log_Writer_CouchDb($this-&gt;_config-&gt;couchdb-&gt;db, $this-&gt;_config-&gt;couchdb);
          $writer-&gt;setFormatter($formatter);

          $logger-&gt;addWriter($writer);
          $logger-&gt;setEventItem('module', $this-&gt;getRequest()-&gt;getModuleName());
          $logger-&gt;setEventItem('controller', $this-&gt;getRequest()-&gt;getControllerName());
          $logger-&gt;log("Testovani chyba", $id);
          $this-&gt;_helper-&gt;flashMessenger-&gt;addMessage('Log item saved');
          $this-&gt;_helper-&gt;redirector('index');
    }

}
</pre>
Controller obsahuje dvě akce a to vlastní zobrazení v indexAction a metodu, která záznamy vytváří logAction. V logAction je vidět výhoda neexistence schématu, protože jsem si přidal další informace bez potřeby měnit schéma databáze.
<h3>PHP a CouchDb</h3>
Použil jsem knihovnu <a href="https://weierophinney.net/phly/">Phly_Couch pro práci s CouchDb</a>, jen jsem musel funkci lehce aktualizovat pro současnou verzi CouchDb 1.0.1, kterou jsem použil.

Aktualizovanou verzi Phly_CouchDb najdete v mém <a href="https://github.com/abtris/phly">forku na githubu</a>, celou ukázkovou aplikaci <a href="https://github.com/abtris/phplogger-couchdb">php_couchdb_logger</a> také.

Pro přístup ke CouchDb můžete použít i jiné knihovny, například <a href="https://arbitracker.org/phpillow.html">PHPillow</a>, která vypadá celkem aktualizovaně. Já jsem použil knihovnu, kterou napsal Matthew Weier O'Phinney hlavně proto, že je psaná přímo pro Zend Framework. Má přehledný a dobře napsaný kód, kde jsem si upravil jen to co jsem se potřeboval.
<h3>CouchDb</h3>
Na Mac OS X si můžete nainstalovat CouchDb jako na linuxu ze zdrojáků nebo použít balík, který mi přijde ideální. Podrobné informace o <a href="https://wiki.apache.org/couchdb/Installation">instalaci jsou ve wiki</a>, případně se podívejte tam.
<h3>Závěr</h3>
Myslím, že to je jednoduchý příklad jak například použít CouchDb, který se pro začátek práce s CouchDb může hodit. Pokud chcete dělat náročnějši vyhledavání na logy, je potřeba použit <a href="https://github.com/rnewson/couchdb-lucene/">CouchDb Lucene</a>. Pokud se chcete dozvědět více o CouchDb tak doporučuji jít na přednášku <a href="https://webexpo.cz/prednasky/development/">Karla Minaříka na letošním Webexpu</a>.

V dalším pokračování si ukážeme jak by se to dělalo v MongoDb.
