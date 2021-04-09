---
date: 2011-04-10T00:00:00Z
tags:
- php
- rabbitmq
title: PHP a RabbitMQ
type: post
url: /2011/04/10/php-a-rabbitmq/
---

V poslední době se objevilo hodně článků o <a href="https://www.rabbitmq.com/">RabbitMQ</a> a připravuje se <a href="https://manning.com/videla/">kniha</a> kde většina příkladů je v PHP.  Připravil jsem malou demonstraci jak se message queue dobře využit. RabbitMQ je napsaný v Erlangu podobně jako CouchDB a hodí ke zpracování dávkových úloh. V demonstraci využívám knihovnu <a href="https://code.google.com/p/wkhtmltopdf/">wkhtmltopdf</a> která umí zpracovat html stránku na PDF, používá k tomu webkit jádro.
<h2>Design</h2>
Malý design aplikace jsem zvolil takto:  <img class="aligncenter" title="rabbitmq_design" src="https://github.com/abtris/php-rabbitmq-wkhtmltox-demo/raw/master/docs/design.png" alt="" width="600" />
<h2>Kód</h2>
Základ aplikace jsou dva úlohy producer a consumer. Nejdříve je potřeba pustit consumer.
<pre class="code">error_reporting(E_ALL);
/**
 * Application path
 */
define('APPLICATION_PATH', realpath(dirname(__FILE__) . '/../application'));
/**
 * Application enviroment
 */
define('APPLICATION_ENV', 'development');

require_once APPLICATION_PATH . '/models/Rabbit.php';
/**
 * /Ensure library/ is on include_path
 */
set_include_path(implode(PATH_SEPARATOR, array(
    realpath(APPLICATION_PATH . '/../library'),
    get_include_path()
)));

/** Zend_Application */
require_once 'Zend/Application.php';

// Create application, bootstrap, and run
$application = new Zend_Application(
    APPLICATION_ENV,
    APPLICATION_PATH . '/configs/application.ini'
);
$application-&gt;getBootstrap()-&gt;bootstrap();

$config = new Zend_Config_Ini(APPLICATION_PATH . '/configs/application.ini', APPLICATION_ENV);

$r = new Application_Model_Rabbit($config-&gt;rabbitmq);
$r-&gt;consumer();</pre>
Tento script musí běžet na serveru, kde se zpracovává vlastní požadavek. Model potom volá metodu, která se vykoná.
<pre class="code">        $conn = new AMQPConnection($this-&gt;options['host'],
                                   $this-&gt;options['port'],
                                   $this-&gt;options['user'],
                                   $this-&gt;options['pass'],
                                   $this-&gt;options['vhost']
        );

        $channel = $conn-&gt;channel();
        /**
         * $exchange, $type,$passive=false,$durable=false,$auto_delete=true,
         */
        $channel-&gt;exchange_declare(self::EXCHANGE, 'direct', false, true, false);
        $channel-&gt;queue_declare(self::QUEUE);
        $channel-&gt;queue_bind(self::QUEUE, self::EXCHANGE);

        $consumer = function($msg) {
          $msg-&gt;delivery_info['channel']-&gt;basic_cancel($msg-&gt;delivery_info['delivery_tag']);
          if ($msg-&gt;body == 'quit') {
              $msg-&gt;delivery_info['channel']-&gt;basic_cancel($msg-&gt;delivery_info['consumer_tag']);
          } else {
              if (!empty($msg-&gt;body))  {
                  // make PDF
                  Application_Model_Wkhtmltopdf::proceed($msg-&gt;body, APPLICATION_PATH . '/../output/');
                  // notify user
                  system("growlnotify -n \"Rabbit demo\" -m \"PDF CREATED\"");
              }
          }
        };

        $channel-&gt;basic_consume(self::QUEUE,
                                self::CONSUMER_TAG,
                                false,
                                false,
                                false,
                                false,
                                $consumer);
        while (count($channel-&gt;callbacks)) {
            $channel-&gt;wait();
        }

        $channel-&gt;close();
        $conn-&gt;close();</pre>
Vlastní aplikace je potom jednoduchá
<pre class="code">    public function indexAction()
    {
        $r = new Application_Model_Rabbit($this-&gt;_config-&gt;rabbitmq);

        $this-&gt;view-&gt;form = $form = new Application_Form_AddUrl();
        // process form
        if ($this-&gt;getRequest()-&gt;isPost()) {
            $formData = $this-&gt;getRequest()-&gt;getParams();
            if ($form-&gt;isValid($formData)) {
                $r-&gt;setUrl($form-&gt;url-&gt;getValue());
                $r-&gt;run();
            } else {
                $form-&gt;populate($formData);
            }
        }

        $this-&gt;view-&gt;url = $r-&gt;getUrl();
    }</pre>
a volá se producer, který invokuje RabbitMQ
<pre class="code">        $conn = new AMQPConnection($this-&gt;options['host'],
                                   $this-&gt;options['port'],
                                   $this-&gt;options['user'],
                                   $this-&gt;options['pass'],
                                   $this-&gt;options['vhost']
        );
        $channel = $conn-&gt;channel();
        $channel-&gt;exchange_declare(self::EXCHANGE,
                                   'direct',
                                   false,
                                   true,
                                   false);
        $msg = new AMQPMessage($string, array('content-type' =&gt; 'text/plain'));
        $channel-&gt;basic_publish($msg, self::EXCHANGE);
        $channel-&gt;close();
        $conn-&gt;close();</pre>
Demo u mne funguje velmi dobře, nevím jak na jiných platformách, zkoušel jsem to jen na Macu. Na linuxu by to mělo fungovat obdobně. Jen pro webovou aplikaci by bylo vhodné použít jiný systém notifikace pro webovou aplikaci. Nenašel jsem jak například předávat notifikace přes RabbitMQ, ale pokud někdo víte jak to elegatně udělat přidejte to do komentářů.

Veškerý zdrojový kód je <a href="https://github.com/abtris/php-rabbitmq-wkhtmltox-demo">dostupný na githubu</a>.

Zvolil jsem jednoduché jednosměrné řešení bez implementace RPC, kde by se dala pro notifikaci použít reply fronta. Ale myslím, že to celkem stačí pro vetšinu dávkových aplikací jako jsou logovaní, upload souborů i generovaní PDF. Nic nebrání nahradit moji notifikaci například posláním linku ke stažení výsledného PDF apod. Příklad by šel pomocí RPC vylepšit na notifikaci přímo v aplikaci. Pro implementaci RPC lze například použít <a href="https://github.com/tnc/Thumper">Thumper</a>.
