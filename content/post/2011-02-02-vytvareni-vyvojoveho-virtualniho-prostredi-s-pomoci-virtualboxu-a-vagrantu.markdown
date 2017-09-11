---
date: 2011-02-02T00:00:00Z
meta:
  _edit_last: "1"
  _encloseme: "1"
  _pingme: "1"
published: true
status: publish
tags:
- virtualizace
title: Vytváření vývojového Virtuálního prostředí pomocí VirtualBoxu a Vagrantu
type: post
url: /2011/02/02/vytvareni-vyvojoveho-virtualniho-prostredi-s-pomoci-virtualboxu-a-vagrantu/
---

<a href="http://www.virtualbox.org/">Oracle VirtualBox</a> je známé virtualizační prostředí pro platformy linux, mac a windows. Já VirtualBox používám na linux, mám na něm Ubuntu, které používám na školení Subversion nebo na vývoj webových aplikací jako server. Do nedávna jsem to používal na Macu nebo Linux pro běh Windows apod. O tomto používání nechci dnes mluvit.

<a href="http://www.vagrantup.com">Vagrant</a> je nástroj napsaný v Ruby, který nám umožňuje modifikovat virtualní stroj podle našich představ pomocí nějakého předpisu, který nám udělá co chceme. Ukážeme si to na příkladu, že připravím linuxový server pro webový vývoj s Apache, PHP5, MySQL, CouchDB.

Začneme instalací Vagrantu
<pre class="code">sudo gem install vagrant
vagrant box add base http://files.vagrantup.com/lucid32.box</pre>
Vytvoříme si ukázkový projekt.
<pre class="code">mkdir vagrant-lamp
cd vagrant-lamp
vagrant init</pre>
Musíme upravit Vagrantfile podle následujícího předpisu (pro Vagrant 0.7).
<pre class="code">Vagrant::Config.run do |config|
  # All Vagrant configuration is done here. For a detailed explanation
  # and listing of configuration options, please view the documentation
  # online.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "base"

  config.vm.forward_port("http", 80, 8080)
  config.vm.forward_port("mysql", 3306, 3306)
  config.vm.forward_port("couchdb", 5984, 5984)

  config.vm.provision :chef_solo do |chef|
     chef.cookbooks_path = "cookbooks"
  	 chef.json.merge!({
     	:mysql =&gt; {
     		:server_root_password =&gt; "root"
	 	}
  	 })
	 chef.add_recipe("vagrant_main")
  end

end</pre>
Vytvoříme si adresáře pro recepty a pro web.
<pre class="code">mkdir cookbooks
mkdir public</pre>
Do cookbooks je potřeba nakopírovat recepty pro Chef. Doporučuji si vzít z Githubu celou cookbooks repository.
git clone <a href="https://github.com/opscode/cookbooks.git">https://github.com/opscode/cookbooks.git</a>

protože receptů je hodně, doporučuji si to dám někam mimo a nakopírovat jen potřebné recepty. Pro nás jsou to ty, které jsou v úvodu default.rb a některé navíc jako je třeba erlang pro couchdb.

do adresáře cookbooks jsem si nakopíroval tyto recepty z cookbooks repository.
<pre>apache2
apt
couchdb
erlang
git
mysql
openssl
php</pre>
Potom si vytvoříme vlastní recept, který celou akci <a href="http://www.opscode.com/chef">Chefa</a> v Vagrantu bude řídit.
<pre>mkdir -p cookbooks/vagrant_main/recipes
vim default.rb</pre>
Přidáme potřebné recepty
<pre>require_recipe "apt"
require_recipe "apache2"
require_recipe "mysql::server"
require_recipe "php::php5"
require_recipe "git"
require_recipe "couchdb"</pre>
Kromě instalace z receptů provede instalaci některý dalších balíčků v Ubuntu
<pre># Some neat package (subversion is needed for "subversion" chef ressource)
%w{ debconf php5-xdebug subversion mc htop curl }.each do |a_package|
  package a_package
end</pre>
Vytvoříme si testovací vývojový web.
<pre>s = "dev-site"
site = {
  :name =&gt; s,
  :host =&gt; "www.#{s}.com",
  :aliases =&gt; ["#{s}.com", "dev.#{s}-static.com"]
}</pre>
použije se template uložena v vagrant_main/templates/default/sites.conf.erb a příslušné proměnné
<pre># Configure the development site
web_app site[:name] do
  template "sites.conf.erb"
  server_name site[:host]
  server_aliases site[:aliases]
  docroot "/vagrant/public/"
end</pre>
modifikujeme si hosty
<pre># Add site info in /etc/hosts
bash "info_in_etc_hosts" do
  code "echo 127.0.0.1 #{site[:host]} #{site[:aliases]} &gt;&gt; /etc/hosts"
end</pre>
přidáme některé knihovny přímo z repository Subversion
<pre># Retrieve webgrind for xdebug trace analysis
subversion "Webgrind" do
  repository "http://webgrind.googlecode.com/svn/trunk/"
  revision "HEAD"
  destination "/var/www/webgrind"
  action :sync
end</pre>
nebo Git
<pre># Retrieve adminer
git "Adminer" do
  repository "https://github.com/vrana/adminer.git"
  revision "HEAD"
  destination "/var/www/adminer/"
  action :sync
end</pre>
a další
<pre># Latest Zend Framework version
subversion "Zend" do
  # repository "http://framework.zend.com/svn/framework/standard/trunk/library/"
  repository "http://framework.zend.com/svn/framework/standard/tags/release-1.11.3/library/"
  revision "HEAD"
  destination "/srv/lib/php/"
  action :sync
end</pre>
Potom je tu script, kterým se dá modifikovat práva pro MySQL, není problém si podobně udělat jakékoliv úpravy.
<pre># Add an admin user to mysql
execute "add-admin-user" do
  command "/usr/bin/mysql -u root -p#{node[:mysql][:server_root_password]} -e \"" +
      "CREATE USER 'myadmin'@'localhost' IDENTIFIED BY 'myadmin';" +
      "GRANT ALL PRIVILEGES ON *.* TO 'myadmin'@'localhost' WITH GRANT OPTION;" +
      "CREATE USER 'myadmin'@'%' IDENTIFIED BY 'myadmin';" +
      "GRANT ALL PRIVILEGES ON *.* TO 'myadmin'@'%' WITH GRANT OPTION;\" " +
      "mysql"
  action :run
  ignore_failure true
end</pre>
pro funkční port forwarding u CouchDb modifikovat konfiguraci a potřeba dát po prvním spuštění dát vargant reload aby se tato poslední změna promítla do nastavení virtuálu.
<pre># Replace 127.0.0.1 bind port with 0.0.0.0 is necessary for port forwarding
execute "port-forward-couchdb" do
  command "sudo sed -i 's/127.0.0.1/0.0.0.0/g' /etc/couchdb/default.ini"
  action :run
end</pre>
První spuštění pomocí
<pre>vagrant up</pre>
potom je potřeba pro CouchDb
<pre>vagrant reload</pre>
Kompletní repositář s tímto ukázkovým prostředím jsou dostupné na <a href="https://github.com/abtris/vagrant-lamp">Githubu</a>.

Pokud si rozbijete virtuál tak není problém začít znovu. Dáte <code>vagrant destroy</code> a potom znovu pustíte celý proces pomocí <code>vagrant up</code>.

Pokud se chcete přihlásit na virtuál použijte <code>vagrant ssh</code>.
