---
date: 2008-05-05T00:00:00Z
published: true
status: publish
tags:
- php
- zend-framework
title: Zkušenosti s Zend_Auth_Adapter_Ldap
type: post
url: /2008/05/05/zkuenosti-s-zend_auth_adapter_ldap/
---

V nové verzi Zend Framework 1.5 byla do Zend_Auth přidána podpora pro LDAP. Protože u mě v práci se bez toho neobejde ani ta nejjednoduší aplikace, zkusil jsem ho a seznámím vás s problémy na které jsem narazil a jak jsem je obešel.

Nejprve standarní řešení přes Ldap modul v PHP. Něco o našem LDAPu, používáme port 1636 a pro bind vlastní DN, které kopíruje naší strukturu. Pro bind nepotřebuje aplikace žádného vlastního uživatele použije se jméno a heslo toho kdo se hlásí. Část kódu, která je podstatná pro naše porovnání.
<pre name='code' class="php">/**
     * Autentizace proti OpenLDAP Usermap (SSU)
     * @param string $uid
     * @param string $pass
     * @return bool
     */
    private function Autentizace_LDAP($uid,$pass)
    {
        $ds = ldap_connect($this-&gt;_config-&gt;ldaphost);
        @$r = ldap_bind($ds, "cvutLoginName=$uid,ou=People,ou=usermap,o=cvut,c=cz",$pass);
        ldap_close($ds);
        if ($r)
        {
            $this-&gt;logger-&gt;info("Uspesna autentizace k LDAP ($uid)");
            return true;
        }
        else
        {
            $this-&gt;logger-&gt;error("Chyba autentizace k LDAP ($uid)");
            return false;
        }
    }</pre>
V Zendu se to řeší pomocí tohoto kódu, který je odvozen od toho v <a href="https://framework.zend.com/manual/en/zend.ldap.html">manuálu</a>, ale musel projít úpravou v sekci $options, protože LDAP modul v Auth nějak nepočítá s tím, že nemáte uživatele pro přístup k Ldapu, což je pokud vím dost běžná situace.
<pre name='code' class="php">class AuthController extends Zend_Controller_Action
{
    function indexAction()
    {
      $username = $this-&gt;_request-&gt;getParam('username');
      $password = $this-&gt;_request-&gt;getParam('password');
$auth = Zend_Auth::getInstance();
   $registry = Zend_Registry::getInstance();
      $config=$registry-&gt;get('config');
      $log_path = $config-&gt;ldap-&gt;log_path;
      $options = $config-&gt;ldap-&gt;toArray();
     // modifikovani options
      $options['server1']['username'] = "cvutloginname=$username,ou=People,ou=usermap,o=cvut,c=cz";
      $options['server1']['password'] = $password;
      unset($options['log_path']);
      $adapter = new Zend_Auth_Adapter_Ldap($options, $username, $password);
      $result = $auth-&gt;authenticate($adapter);
      if ($log_path)
      {
          $messages = $result-&gt;getMessages();
          $logger = new Zend_Log();
          $logger-&gt;addWriter(new Zend_Log_Writer_Stream($log_path));
          //$filter = new Zend_Log_Filter_Priority(Zend_Log::DEBUG);
          $filter = new Zend_Log_Filter_Priority(Zend_Log::INFO);
          $logger-&gt;addFilter($filter);
          foreach ($messages as $i =&gt; $message)
          {
              if ($i-- &gt; 1) { // $messages[2] and up are log messages
                  $message = str_replace("\n", "\n  ", $message);
                  $logger-&gt;log("Ldap: $i: $message", Zend_Log::DEBUG);
              }
          }
      }
      // vypis
      if ($result-&gt;isValid())
      {
      $this-&gt;view-&gt;status = "You are logged-in as " . $auth-&gt;getIdentity() . "&lt;br&gt;\n";
      }
      else
      {
      $this-&gt;view-&gt;status = "Error. You are not logged. &lt;a href='../../'&gt;Please login again&lt;/a&gt;.";
      }
    }
}</pre>
ještě moje options:
<pre name='code' class="php">[stage]
ldap.log_path = ../logs/ldap.log;
Typical options for OpenLDAP
ldap.server1.host = usermap.cvut.cz
ldap.server1.port = 1636
ldap.server1.useSsl = true
ldap.server1.baseDn = "ou=People,ou=usermap,o=cvut,c=cz"
ldap.server1.bindRequiresDn = true
ldap.server1.accountFilterFormat = "(&amp;(objectClass=person)(uid=%s))"</pre>
v další fázi se obvykle ještě snažíme vytáhnout některá data jako je osobní číslo podle kterého se potom pracuje s aplikací, ale to bude dobře možné až pomocí <a href="https://framework.zend.com/wiki/display/ZFPROP/Zend_Ldap_Ext+Proposal">Zend_Ldap_Ext</a>, která je zatím ve vývoji, ale můžete samozřejmě použít stávající Ldap funkce v php.
