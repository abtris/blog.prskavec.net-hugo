---
date: 2009-09-01T00:00:00Z
tags:
- rest
- php
- zend
- zend-framework
title: Zend_Rest_Route a  Zend_Rest_Controller v ZF 1.9
type: post
url: /2009/09/01/zend_rest_route-a-zend_rest_controller-v-zf-1-9/
---

<h3>Proč REST a co to obnáší?</h3>
V Zend Frameworku 1.9 byla přidána pro používání <a href="https://en.wikipedia.org/wiki/Representational_State_Transfer">REST</a> v url a v controlleru.

REST je v módě a proto nám ho přidali i do ZF. Ne vážně samozřejmě každý teď dělá do REST. Implementace RESTu existuje v každém větším frameworku. V ZF už delší dobu je REST klient i server. Klienta můžete využít pro práci s mnohými službami na internetu (twitter, flickr, ...).

V tabulce je dobře vidět jak se využije HTTP protokol. Metody PUT, DELETE se běžně nevyužívají.
<table border="0">
<tbody>
<tr>
<th>Akce</th>
<th>SQL</th>
<th>HTTP</th>
</tr>
<tr>
<td>Create</td>
<td>Insert</td>
<td>PUT</td>
</tr>
<tr>
<td>Read</td>
<td>Select</td>
<td>GET</td>
</tr>
<tr>
<td>Update</td>
<td>Update</td>
<td>POST</td>
</tr>
<tr>
<td>Delete</td>
<td>Delete</td>
<td>DELETE</td>
</tr>
</tbody></table>
REST API je součástí mnoha z nich. Pokud máte aplikaci RESTful, není problém potom provozovat i REST api.
<ul>
	<li>do ZF byli přidány dvě nové třídy Zend_Rest_Route, Zend_Rest_Controller</li>
	<li>přinese nám to využití HTTP protokolu (GET, POST, PUT, DELETE)</li>
	<li>pro používání RESTu změníme controller, budeme používat Zend_Rest_Controller místo Zend_Action_Controller</li>
	<li>v controlleru je potřeba potřeba definovat tyto metody
<ul>
	<li> indexAction()</li>
	<li>getAction()</li>
	<li>postAction()</li>
	<li>putAction()</li>
	<li>deleteAction()</li>
</ul>
</li>
	<li>GET je get a všechny destruktivní akce jsou přes POST</li>
</ul>
Vzal jsem <a href="https://akrabat.com/zend-framework-tutorial/">základní tutorial z akrabatu</a> a modifikoval jsem ho pro použití s REST. Zdrojové kódy jsou k <a href="https://bitbucket.org/abtris/zf-tutorial-rest/">dispozici na bitbucketu</a>.

Do bootstrapu je potřeba přidat definici pro Zend_Rest_Route

<pre name="code" class="php">
    function _initRestRoute()
    {
        $this-&gt;bootstrap('Request');
        $front = $this-&gt;getResource('FrontController');
        $restRoute = new Zend_Rest_Route($front, array(), array(
            'default' =&gt; array('albums')
                ));
        $front-&gt;getRouter()-&gt;addRoute('rest', $restRoute);
    }

    function _initRequest()
    {
        $this-&gt;bootstrap('FrontController');
        $front = $this-&gt;getResource('FrontController');
        $request = $front-&gt;getRequest();
        if (null === $front-&gt;getRequest()) {
            $request = new Zend_Controller_Request_Http();
            $front-&gt;setRequest($request);
        }
        return $request;
    }

</pre>

a je potřeba modifikovat controller, aby využíval celký HTTP protokol.

<pre name="code" class="php">
class AlbumsController extends Zend_Rest_Controller
{
    private $_albums;
    private $_form;


    public function init()
    {
        /* Initialize action controller here */
        $this-&gt;_albums = new Zend_Db_Table('albums');
     	$this-&gt;_form = new Form_Album();
    }

    public function indexAction()
    {
        $this-&gt;view-&gt;title = "My Albums";
        $this-&gt;view-&gt;headTitle($this-&gt;view-&gt;title, 'PREPEND');
        $this-&gt;view-&gt;albums = $this-&gt;_albums-&gt;fetchAll();
    }

    public function listAction()
    {
        $this-&gt;_forward('index');
    }

    public function getAction()
    {
        $this-&gt;view-&gt;album = $this-&gt;_albums-&gt;find($this-&gt;_getParam('id'))-&gt;current();
    }


    public function putAction()
    {
        $album = $this-&gt;_albums-&gt;find($this-&gt;_getParam('id'))-&gt;current();
        if ($this-&gt;_form-&gt;isValid($this-&gt;_request-&gt;getParams())) {
            $album-&gt;setFromArray($this-&gt;_form-&gt;getValues())-&gt;save();
            $this-&gt;_redirect('albums');
        } else {
            $this-&gt;view-&gt;album = $album;
            $this-&gt;view-&gt;form = $this-&gt;_form;
            $this-&gt;render('edit');
        }

    }

    public function postAction()
    {
        if ($this-&gt;_form-&gt;isValid($this-&gt;_request-&gt;getParams())) {
	       $this-&gt;_albums-&gt;createRow($this-&gt;_form-&gt;getValues())-&gt;save();
           $this-&gt;_redirect('albums');
        } else {
            $this-&gt;view-&gt;form = $this-&gt;_form;
            $this-&gt;view-&gt;title = "Add new album";
           $this-&gt;render('new');
        }

    }


    public function newAction()
    {
        $this-&gt;view-&gt;title = "Add new album";
        $this-&gt;view-&gt;headTitle($this-&gt;view-&gt;title, 'PREPEND');

        $this-&gt;view-&gt;form = $this-&gt;_form;

    }

    public function editAction()
    {
        $this-&gt;view-&gt;title = "Edit album";
        $this-&gt;view-&gt;headTitle($this-&gt;view-&gt;title, 'PREPEND');
        $album = $this-&gt;_albums-&gt;find($this-&gt;_getParam('edit'))-&gt;current();
	    if ($album) {
    	    $this-&gt;_form-&gt;populate($album-&gt;toArray());
	    }
    	$this-&gt;view-&gt;form = $this-&gt;_form;
    	$this-&gt;view-&gt;album = $album;

    }

    public function deleteAction()
    {
        $this-&gt;view-&gt;title = "Delete album";
        $this-&gt;view-&gt;headTitle($this-&gt;view-&gt;title, 'PREPEND');

    	$album = $this-&gt;_albums-&gt;find($this-&gt;_getParam('id'))-&gt;current();
    	$album-&gt;delete();
        $this-&gt;_redirect('albums');

    }
}

</pre>
