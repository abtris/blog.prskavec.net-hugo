---
date: 2011-01-28T00:00:00Z
meta:
  _encloseme: "1"
  _pingme: "1"
published: true
status: publish
tags:
- mongodb
- nosql
- zend framework
title: Zend Framework a NoSQL pro ukládání logů ve webových aplikací, díl druhý o
  MongoDb
type: post
url: /2011/01/28/zend-framework-a-nosql-pro-ukladani-logu-ve-webovych-aplikaci-druhy-dil-o-mongodb/
---

Jak už jsem psal v <a href="http://blog.prskavec.net/2010/08/zend-framework-a-nosql-pro-ukladani-logu-ve-webovych-aplikaci-dil-1-couchdb/">minulém díle o CouchDb</a> není žádný problém v použití dokumentových databází na logy. Výhodou je že se nemusíte starat o schema, což se u aplikace tohoto druhu opravdu hodí. 

Pro napojení Zend Frameworku na logování do MongoDb musíte mít nainstalovanou MongoDb extenzi do PHP. Bez ní se bohužel neobejdete. Log writer si vytvoříte snadno pomocí extenze Zend_Log_Writer_Abstract a provedete drobné úpravy pro práci s MongoDb jak obsahuje ukázka.

<pre class="code php">
/** Zend_Log_Writer_Abstract */
require_once 'Zend/Log/Writer/Abstract.php'; 
/**
 * ZendLogWriterCouchDb
 * @throws Zend_Log_Exception
 */
class App_Log_Writer_MongoDb extends Zend_Log_Writer_Abstract
{
    /**
     * Db
     * @var Mongo
     */
    private $_db;
    /**
     * Db name
     * @var string
     */
    private $_dbname;
    /**
     * Collection
     * @var string
     */
    private $_collection;
    /**
     *
     * @param array $params
     * @return void
     */
    public function __construct($options)
    {
        if (!extension_loaded('mongo')) {
            throw new Exception('The MongoDB extension must be loaded for using this logger !');
        }

        $options = $options->toArray();
        if (is_null($options)) {
            $options['host'] = "localhost";
            $options['port'] = "27017";
            $options['db'] = $dbname;
        }
        if (isset($options['user']) && isset($options['pass'])) {
            $conn = "{$options['user']}:{$options['pass']}@";
        } else {
            $conn = "";
        }
        $this->_db = new Mongo("mongodb://$conn{$options['host']}:{$options['port']}/{$options['db']}");
        $this->_dbname = $options['db'];
        $this->_collection = $options['collection'];
    }
    /**
     * @static
     * @param  $config
     * @return
     */
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
        $m = $this->_db;
        $m->connect();
        $c = $m->selectCollection($this->_dbname, $this->_collection);
        $c->insert($event);
    }

    
}
</pre>

a tímto jste vlastně hotovy. Celý kód najdete na <a href='https://github.com/abtris/phplogger-mongodb'>githubu</a>. V ukázce je ještě samozřejmě upravený controller pro zobrazování a obsahuje možnost vytvářet testovací data. 

MongoDb můžete nainstalovat lokálně nebo použít hosting <a href='http://mongohq.com'>mongohq.com</a>, kde základní databáze, která k testování stačí je zdarma. Potom se mi docela hodila utilita na Macu <a href="http://mongohub.todayclose.com/">Mongohub</a>, která poskytuje trochu konfortnější konzoli než webová konzole obsahující lokální instalaci. Na hostingu MongoHQ mají pěknou webovou administrační konzoli.

V PHP se kromě vlastní extenze dá ještě dobře použít pro Zend Framework knihovna <a href="https://github.com/coen-hyde/Shanty-Mongo">Shanty Mongo</a>, která zjednodušuje práci s Mongo Db na maximum.

Pokud budete chtít dělat složitější dotazy do MongoDb hodí se 
<a href="http://www.mongodb.org/display/DOCS/SQL+to+Mongo+Mapping+Chart">SQL to Mongo Mapping Chart</a>.

Případné další tipy se nebojte napsat do komentářů.
