nginx + tomcat + memcached
============================

####1.download & install
  * nginx [download](http://nginx.org/en/download.html) [install](http://wiki.nginx.org/Install)
  * tomcat [donwload](http://tomcat.apache.org/) [install](http://tomcat.apache.org/tomcat-7.0-doc/appdev/installation.html)
  * memcached-session-manager [download](http://code.google.com/p/memcached-session-manager/)
  * memcached [download](http://memcached.org/downloads)

####2.configuration
  
**nginx**
 
   conf/nginx.conf
 
```
 user  nobody;

worker_processes  4;

error_log  logs/error.log;

events{

   worker_connections  1024;

}


http {

   include       mime.types;

   default_type  application/octet-stream;

   sendfile        on;

   keepalive_timeout  65;

   gzip on;

   upstream _www.****.com_  {

   server  _192.168.1.11:8080_;

   server  _192.168.1.101:8080_;

   }

   server {

       listen       80;

       server_name  _www.****.com_;

       charset utf-8;

       location / {

           root   html;

           index  index.html index.htm;

           proxy_pass        _http://www.****.com_;

           proxy_set_header  X-Real-IP $remote_addr;

           client_max_body_size  100m;

       }


       location ~ ^/(WEB-INF)/ {

           deny all;

       }


       error_page   500 502 503 504  /50x.html;

       location = /50x.html {

           root   html;

       }


   }

}

```

**tomcat**

   conf/server.xml
   
```
<ContextdocBase="/var/www/html"path="" reloadable="true">

 <Manager className="de.javakaffee.web.msm.MemcachedBackupSessionManager"memcachedNodes="n1:localhost:11211"requestUriIgnorePatte

rn=".*\.(png|gif|jpg|css|js)$"sessionBackupAsync="false" sessionBackupTimeout="100"transcoderFactoryClass="de.javakaffee.web.msm.s

erializer.javolution.JavolutionTranscoderFactory"copyCollectionsForSerialization="false" />

 </Context>
```

 
