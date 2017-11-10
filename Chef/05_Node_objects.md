Node Objects
============
Los _node objects_ son grupos de atributos y run lists. En este contexto son atributos las características de los nodos clientes, como que tipo de CPU tienen, que versión del SO, memoria, IP etc.

Los Node objects son reconstruidos por el clente cada vez que el comando chef-client se ejecuta. En la práctica significa que se generan cada vez que el nodo se comunica con el servidor. Además es lo primero que hace. esto es porque **los atributos pueden ser usados por las recetas para filtrar las acciones que se van a llevar a cabo.**

La herramienta que recolecta todos los atributos se llama _ohai_. Los node objects son almacenados en el servidor de Chef en formato _JSON_

Trabajando con _ohai_ y los Node Attributes
-------------------------------------------
Si ejecutamos `ohai` en la consola obtendremos toda la información que tiene del servidor. Si nos fijamos en esta salida vemos que nos recuerda a la que obtenemos desde la workstation con el comando `knife node show -l node.name`.

También podemos filtrar la salida, por ejemplo, el siguiente comando:

    $ ohai ipaddress

Nos dará la siguietne salida:

```
[
    "172.31.96.245"
]
```

Podemos usar el comando ohai para buscar y testar keys que podremos usar luego en nuestras recetas, como por ejemplo de la siguiente manera:

```ruby
if node['platform_family'] == "rhel"
	package = "httpd"
elsif node['platform_family'] == "debian"
	package = "apache2"
end
```

