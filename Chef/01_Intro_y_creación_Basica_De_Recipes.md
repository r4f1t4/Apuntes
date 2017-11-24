Apuntes Chef
============

Descarga e instalación
----------------------
Instalamos el chef development kit en una máquina Linux. Podemos descargarlo de https://downloads.chef.io

Una vez instalado lo arrancamos con el comando:

      chef-client --local-mode

Chef usa "recipes" para configurar las máquinas. Las recipes están compuestas por bloques de código en Ruby y cada uno de esos bloques realiza unas tareas especificas.

Sección Package
---------------
Desde la sección package se gestiona la instalación y configuración del software instalado. Un ejemplo sería:

```ruby
package 'httpd' do
	action :install
end
```

La acción por defecto es `:install` por lo que el ejemplo anterior sin la línea de en medio tendría idéntico resultado.
Las acciones de los paquetes son las siguientes:
- `:install` (es la opción por defecto para el bloque package)
- `:nothing` No se realiza nada hasta que otro recurso le notifica que realice alguna acción.
- `:purge` Borra los ficheros de configuración y el paquete (sólo Debian)
- `:reconfig` Reconfigura el paquete.
- `:remove` Borra el paquete
- `:upgrade` Instala el paquete, y si ya está instalado lo actualiza a la última versión disponible.

Sección service
===============
La sección service nos permite definir como se va a comportar el software instalado. Por ejemplo:

```ruby
service 'httpd' do
	action [:enable, :start]
end
```

El anterior bloque habilita el servidor web para que arranque automáticamente al iniciar el servidor, y lo arranca si está parado. El bloque que está entre [] es un array en Ruby, y nos permite pasar múltiples opciones dentro de un mismo bloque.

Otro ejemplo:

```ruby
service 'httpd' do
	service_name 'httpd'
	action [:enable, :start]
end
```

En este ejemplo hemos añadido la línea service name. Esto nos permite tener un nombre de servicio distinto al nombre de recurso. En ese caso particular, las acciones se ejecutan sobre el proceso 'httpd' en vez de sobre el proceso apache. Si no se incluye, Chef entiende que el "resource name" y el "service name" son idénticos.
Las acciones disponibles para los servicios son las siguientes:
- `:disable` deshabilita el arranque del servicio durante el arranque del SO
- `:enable` habilita el arranque del servicio durante el arranque del SO
- `:nothing` No se realiza nada hasta que otro recurso le notifica que realice alguna acción. (Esta es la acción por defecto para el bloque service.)
- `:reload` Recarga la configuración del servicio.
- `:start` Arranca el servicio.
- `:restart` reinicia el servicio.
- `:stop` Para el serivicio.

Sección file y notifies
-----------------------
Para notificar a otro recurso en la recipe de que se tiene que realizar una acción, está el evento `notifies` por ejemplo:

```ruby
file '/etc/httpd/vhost.conf' do
	content 'fake vhost file'
	notifies :restart, 'service[httpd]'
end
```

El anterior bloque notifica al recurso de tipo service con nombre 'httpd' de que ha de ejecutar el evento `:restart`. Ojo, en este caso, 'httpd' es el nombre del recurso, no el servicio.
La acción por defecto para file es `:create`

Sección execute
---------------
Otro de los tipos de bloque disponibles es el bloque de **execute**, que nos permite ejecutar comandos arbitrarios. Además estos bloques tienen las funciones `not_if` y `only_if` que nos permiten controlar su ejecución. Esto es porque la filosofía de chef es que todos los pasos de la receta se ejecutan o no para obtener un resultado deseado. Por ejemplo. Antes de ejecutarse la acción `:install` dentro  de un bloque "package" chef comprueba si el paquete está ya instalado y no se ejecuta si ya lo está.

Ejemplos de execute:

```ruby
execute 'not-if-example' do
	command '/usr/bin/echo "10.0.2.1 webserver01" >> /etc/hosts'
	not_if 'test -z $(grep "10.0.2.1 webserver01" /etc/hosts)'
end

execute 'only-if-example' do
	command '/usr/bin/echo "10.0.2.1 webserver01" >> /etc/hosts'
	only_if 'test -z $(grep "10.0.2.1 webserver01" /etc/hosts)'
end

execute 'only-if-with-ruby-code' do
	command 'echo "test" >> /tmp/testfile'
	only_if { ::File.exists?('/tmp/testfile') }
end
```

Actuar sobre más de un ítem en un bloque.
-----------------------------------------
El siguiente código actúa sobre los paquetes httpd, vim, tree y emacs al mismo tiempo realizando la misma acción sin necesidad de escribir cuatro bloques de código casi idénticos.

```ruby
%w{httpd vim tree emacs}.each do |pkg|
	package pkg do
		action :upgrade
	end
end
```

`%w{}` es la forma de definir un array de "word" en ruby. Sería equivalente a escribir `['httpd', 'vim', 'tree', 'emacs']` sólo que un poco más cómodo.

Ejecutando recipes localmente
-----------------------------
La recipe la escribimos en un fichero _.rb_ y a continuación ejecutamos el comando `ruby -c recipe.rb` Esto comprueba la sintaxis de ruby, pero no la de chef.
El siguiente punto es ejecutar `foodcritic recipe.rb` este comando sí que comprueba la sintaxis de chef.
Finalmente ejecutamos la recipe con `chef-client --local-mode recipe.rb`

Las ordenes _notifies_ y _subscribe_
-----------------------------------
La orden `notifies`, se comunica con otro recurso y le manda la orden que ha de ejecutar. Por ejemplo:

```ruby
service "httpd" do
end

cookbook_file "/etc/httpd/conf/httpd.conf" do
	ownner 'root'
	group 'root'
	mode '0644'
	source 'httpd.conf'
	notifies :restart, "service[httpd]"
end
```

En el bloque de código anterior, el recurso de tipo `service` no tiene acción definida, y la acción por defecto de service es `nothing` que no hace nada hasta que otro recurso le notifica lo que ha de hacer. En este caso la notificación vendrá del recurso "cookbook_file", que se ejecuta cuando el contenido del fichero que tiene definido como source es diferente del fichero que monitoriza. Al ejecutarse la orden **notifies** le manda el comando `:restart` al recurso indicado, en este caso `"service[httpd]"`

El funcionamiento de `subscribes` es el inverso, en este caso, cuando un recurso contiene la orden "subscribes" en un principio no ejecuta la acción que tiene definida, sino que monitoriza la ejecución de otro recurso distinto y ejecuta la acción sobre sí mismo cuando el otro recurso es ejecutado.

En este ejemplo, el recurso service monitoriza el recurso cookbook_file, y en el caso de que haya diferencias en el fichero source, el recurso service recarga su configuración `:reload`

```ruby
service "httpd" do
	subscribes :reload, "cookbook_file[/etc/httpd/conf/httpd.conf]"
end

cookbook_file "/etc/httpd/conf/httpd.conf" do
	ownner 'root'
	group 'root'
	mode '0644'
	source 'httpd.conf'
end
```

Eliminar recursos de una recipe
-------------------------------
Si simplemente borramos un recurso de una receta, la próxima vez que se ejecute la receta Chef no controlará el recurso. Esto no significa que vaya a desinstalarse si es un paquete, o pararse si es un servicio; lo que significa es que se quedará en el último estado configurado hasta que algo exterior a Chef lo modifique. Si deseamos quitar un recurso porque ya no va a ser usado, podemos, o borrarlo de la configuración de Chef y desinstalarlo/deshabilitarlo manualmente, o marcar como estado deseado en Chef el estado contrario al que veníamos usando.
