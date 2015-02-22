Installing LiteSpeed and FastCGI for Catalyst
=============================================

* Date: 9 Mar 2006

Table of Contents
-----------------

1. [Overview] (#overview)
2. [Installing LiteSpeed] (#install)
3. [Configuring LiteSpeed] (#config)
  1. [Configuring a Virtual Host] (#config-vhost)
  2. [Configuring the Catalyst FastCGI App] (#config-catalyst-fastcgi)
  3. [Configuring the Catalyst FastCGI App Context] (#config-catalyst-fastcgi-context)
4. [Restarting LiteSpeed] (#restart)
5. [Discussion] (#discuss)
6. [Meta] (#meta)

<a name="overview"></a>
Overview
--------

LiteSpeed (http://litespeedtech.com/), along with lighttpd (http://www.lighttpd.net) and Zeus (http://www.zeus.com), are among the more popular event-driven web servers that are known for their high performance. Lighttpd is open source and Zeus is a fully commercial while LiteSpeed fits somewhere in the middle. LiteSpeed is a commercial web server that includes a free as in beer Standard offering as well as a Professional offering. In published benchmarks, it seems that LiteSpeed is somewhat faster than lighttpd.

This article runs through a simple installation to get a Catalyst (http://www.catalystframework.org) app running on LiteSpeed with FastCGI. It is tested with Catalyst 5.65, LiteSpeed 2.1.12 Standard and CentOS 4.2. It only covers using Catalyst with a stand-alone FastCGI server using sockets and TCP. The static-mode configuration wasn't tested. LiteSpeed's Persistent FastCGI connection, which is known to have problems with Ruby, also was not tested. At this time, it's unknown if LiteSpeed can connect to multiple stand-alone FastCGI servers such as lighttpd's load-balancing configuration.

<a name="install"></a>
Installing LiteSpeed
--------------------

You can download the LiteSpeed Standard edition for free from http://litespeedtech.com/download.html. To install LiteSpeed, simply unpack the tarball and run install.sh as a superuser.

```
# gtar xvzf lsws-2.1.12-std-i386-linux.tar.gz
# cd lsws-2.1.12
# ./install.sh
```

The installation script will ask you to select a port for the Admin server as well as an admin username and password. It will also install a startup script in /etc/init.d if you choose. For Red Hat/CentOS, the service is installed as lsws. By default, the binary is installed in /opt/lsws/bin/lshttpd

<a name="config"></a>
Configuring LiteSpeed
---------------------

The default Admin server port is 7080. If you used that you can use your web browser to connect to http://localhost:7080 and login.

<a name="config-vhost"></a>
### Configuring a Virtual Host

* Click on "Server Configuration" on the admin server home page.
* There is a Listener called Default enabled already. To add a virtual host to that listener, in the left nav column, click on the 'Default' link under the 'Listeners' link. This will bring up a page with the "Address Settings" and "Virtual Host Mappings". There's a virtual host called Example already configured.
* To use the Example virtual host, click on 'Edit' and add the domains you'd like to use. Use commas to separate multiple domains like [myapp.com, www.myapp.com]. Click 'Save' and then 'Apply Changes' which is in the upper left corner and red now. The 'Add' link here doesn't seem to work as it brings you to the Example virtual host. To add a vitual host click on the 'Virtual Hosts' link in the left nav column and then click 'Add.' We won't do this for this quick setup.

<a name="config-catalyst-fastcgi"</a>
### Configuring the Catalyst FastCGI App

Steps:

* While in "Server Configuration" click 'Server' in the left nav column and then click the 'External Apps' tab on top of the page body. There shouldn't be any apps loaded yet.
* Click 'Add' in the upper right corner of the table and then select 'Fast CGI App' from the drop down before clicking 'Next'.
* This brings you to the "FastCGI App Definition" page where you'll tell LiteSpeed how to run the FastCGI app. The FastCGI app can be a stand-alone server (listing via *NIX sockets or TCP) or it can be a local "static" FastCGI started by the server. A stand-alone FastCGI server has been tested successfully with sockets and TCP. There is a setting called 'Persistent Connection' to make a persistent connection to the FastCGI server. Ruby is known to have problems with this. It's unknown if Perl has problems with LiteSpeed's persistent FastCGI connection. To configure this page use the following settings and then click 'Save' and 'Apply Changes'. The 'Name' is just an internal reference and can be anything.

```
Name: MyApp
Address: UDS://tmp/myapp.socket
Max Connections: 10
Initial Request Timeout: 1
Retry Timeout: 0
Memory Soft Limit: 80M
Memory Hard Limit: 100M
Process Soft Limit: 200
Process Hard Limit: 200
```

To use TCP, use the something like the following for 'Address':

```
Address: 10.0.0.2:1030
```

Make sure the FastCGI server is running on the port you select with something like either of the following depending on whether you're using sockets or TCP:

```
MyApp/script/myapp_fastcgi.pl -l /tmp/myapp.socket -n 5 -d
MyApp/script/myapp_fastcgi.pl -l 1030 -n 5 -d
```

<a name="config-catalyst-fastcgi-context"></a>
### Configuring the Catalyst FastCGI App Context

Now that the FastCGI app is configured, we need to let LiteSpeed know what URLs it will respond to. This is done by setting a LiteSpeed 'Context'.

* Go back to the "Server Configuration" page and click on the 'Example' virtual host link at the bottom of the left nav column.
* Click on the 'Context' tab that appears above the page body and you will see a number of mapped URLs. Click 'Add' and on the next page select 'Fast CGI' from the drop down before clicking 'Next'.
* This brings you to the "Static Context Definition" page. To have the Catalyst app handle the root directory enter / for URI and make sure 'FastCGI App' is set to the name you configured when setting the 'External App'. Click 'Save' and then 'Apply Changes'
* You're done. Now just restart the webserver.

<a name="restart"></a>
Restarting LiteSpeed
--------------------

Steps to restart LiteSpeed:

* Click on "Home" in the admin console and then click "Service Manager".
* Click "Graceful Restart" and your Catalyst app should now be accessible.

You can also restart LiteSpeed from the command line. In Red Hat use:

```
# service lsws restart
```

<a name="discuss"></a>
Discuss
-------

Discussion on this recipe can be found on the LiteSpeed forums:

* [Tutorial in installing LiteSpeed with FastCGI and Catalyst] (http://www.litespeedtech.com/support/forum/threads/tutorial-in-installing-litespeed-with-fastcgi-and-catalyst.500/)

<a name="meta"></a>
Meta
----

* Author: John Wang
* Date: 9 Mar 2006
* License: CC-BY-SA [https://creativecommons.org/licenses/by/3.0/us/](https://creativecommons.org/licenses/by/3.0/us/)

END