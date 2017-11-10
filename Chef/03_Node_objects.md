Node Objects
============
Node objects are made of groups of attributes and run-lists. Attributes, in this context means the actual characteristics of the host, such as CPU and memory information, IP address, etc.

Node objects are rebuilt by the client after it's initial download from the Chef server, in fact that is the first thing that the chef-client does each time is run. This is because __attributes can be used by our recipes in order to filter what to be done.__

Attributes are collected by a tool called _ohai_ and is merged with the node's run-list into a __node object__ on a _JSON_ file stored on the server.

Working with ohai and Node Attributes
-------------------------------------
Executing `ohai` on the console actually dumps all the information that it has on the server, but we also can filter the results, for example

    $ ohai ipaddress

will output the following:

    [
      "172.31.96.245"
    ]

We can use the command in this way to test for key on which to filter in our recipes in the following way:

```ruby
if node['platform_family'] == "rhel"
	package = "httpd"
elsif node['platform_family'] == "debian"
	package = "apache2"
end
```

