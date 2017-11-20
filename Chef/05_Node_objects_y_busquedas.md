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

Búsquedas en Chef
=================

Tipos de índices
----------------
En chef hay 5 típos de índices _(indexes)_
- `client`
- `data Bag` Los _data bags_ son variables globales almacendas en formato JSON y accesibles desde un servidor Chef. Pueden ser cargadas por una receta y accedidas durante una búsqueda.
- `environment`
- `node`
- `role`

Para hacer una búsqueda desde knife usamos el siguiente comando

    knife search [INDEX] "key:patron"

- El campo `INDEX` es opcional, si no lo pasamos por defecto se usa el índice "node".
- `Key` es uno de los campos índice como los que podemos encontrar en la salida de JSON que nos muestra el comando `ohai`.
- El patrón de búsqueda puede contener expresiones regulares.

Ejemplos de búsquedas:
- `knife search node 'platform_family:debian'`
- `knife search node 'recipes:nginx\:\:default'` En este caso el `::` que separa el nombre del cookbook del de la receta ha tenido que ser escapado con sendas `\`.
- `knife search node 'ipaddress:192.168.1.* or ipaddress 192.168.2.*` Podemos buscar por más de una key o patrón usando `or`, para obtener los resultados que concuerden con una de las opciones, o `and` para filtrar por los resultados que contengan ambas opciones.
- `knife search node 'platform*:*buntu'` Podemos usar expresiones regulares para realizar la búsqueda filtrando tanto las keys como las máscaras.

Knife search tiene los siguientes modificadores
- `-i` Nos devuelve los node ID de los nodos que hayan concordado con la búsqueda.
- `-a attribute` nos muestra el atributo seleccionado de los nodos que concuerdan con la búsqueda.
- `-r` Nos muestra las run-lists.
Por ejemplo, `knife search '*:*' -r` es equivalente a `knife search '*:*' -a run_list`
