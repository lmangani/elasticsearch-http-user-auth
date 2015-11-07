![](https://travis-ci.org/lmangani/elasticsearch-http-user-auth.svg)

# Elasticsearch HTTP Basic User Auth plugin

Elasticsearch user authentication plugin with http basic auth.

This plugin provides user authentication APIs and a User management we console. 

## Installation 
<pre>
bin/plugin --url https://raw.githubusercontent.com/TomSearch/elasticsearch-http-user-auth/master/jar/http-user-auth-plugin-1.0-SNAPSHOT.jar --install http-user-auth-plugin
</pre>

#### Build with Maven
<pre>
mvn package clean
bin/plugin --url file:///path/to/repo/jar/http-user-auth-plugin-1.0-SNAPSHOT.zip --install http-user-auth-plugin
</pre>

## Configuration
Add following lines to elasticsearch.yml:
<pre>
http.user.auth.disabled: false
http.user.auth.root.password: rootpassword
</pre>

If you set `http.user.auth.disabled` to `true`, Elasticsearch doesn't load this plugin.  
`http.user.auth.root.password` sets root user's password literally.  
**Only the root user can access ES's root APIs (like /_cat, /_cluster) and all indices.**  
Other users can access URLs under their own indices that are specified with this plugin's API.  

## Add username and password on HTTP requests
The authentication method of this plugin is Basic Authentication. Therefore, you should add your username and password on URL string. For example: 

<pre>
http://root:rootpassword@your.elasticsearch.hostname:9200/
</pre>

###### CURL
<pre>
curl -u root:rootpassword http://your.elasticsearch.hostname:9200/
</pre>
```javascript
{
  "status" : 200,
  "name" : "Piranha",
  "cluster_name" : "elastic1",
  "version" : {
    "number" : "1.7.3",
    "build_hash" : "05d4530971ef0ea46d0f4fa6ee64dbc8df659682",
    "build_timestamp" : "2015-10-15T09:14:17Z",
    "build_snapshot" : false,
    "lucene_version" : "4.10.4"
  },
  "tagline" : "You Know, for Search"
}
```

Plugins using ES's REST API also have to be set root password in their configurations.

The ways of configuring Marvel and Kibana 4 are below: 

#### Marvel 
elasticsearch.yml:
<pre>
marvel.agent.exporter.es.hosts: ["root:rootpassword@127.0.0.1:9200"]
</pre>

#### Kibana 4
kibana.yml:
<pre>
elasticsearch.username: root
elasticsearch.password: rootpassword
</pre>



## User Management Console

This plugin provides a web console which manages users. 
<pre>
http://your.elasticsearch.hostname:9200/_plugin/http-user-auth-plugin/index.html
</pre>

## User Management API
This plugin provides a web API to manage users and permissions.
![](http://i.imgur.com/r26mGAl.png)

##### Add User:
<pre>
http://your.elasticsearch.hostname:9200/_httpuserauth?mode=adduser&username=admin&password=somepass
</pre>

##### Add Index Permissions:
<pre>
http://your.elasticsearch.hostname:9200/_httpuserauth?mode=addindex&username=admin&password=somepass&index=index*
</pre>

##### Update Index Permissions:
<pre>
http://your.elasticsearch.hostname:9200/_httpuserauth?mode=updateindex&username=admin&index=index-*
</pre>

##### Delete User:
<pre>
http://your.elasticsearch.hostname:9200/_httpuserauth?mode=deleteuser&username=admin
</pre>

##### List User(s):
<pre>
http://your.elasticsearch.hostname:9200/_httpuserauth?mode=list
</pre>
<pre>
[{"username":"admin","password":"7080bfe27990021c562398e79823h920e9a38aa5d3b10c5ff5d8c498305","indices":["/index-*"],"created":"2015-11-06T21:57:21+0100"}]
</pre>
