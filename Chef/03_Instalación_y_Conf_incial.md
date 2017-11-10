Puesta en marcha del laboratorio
================================

Primera ejecución y configuración del servidor _Chef_
-----------------------------------------------------
Una vez hayamos descargado e instalado el paquete "chef-server-core" para nuestra distribución, ejecutamos el comando 
      chef-server-ctl reconfigure
Una vez que el comando de configuración termine, podemos empezar a personalizar la instalación. Uno de los primeros pasos es crear un usuario, para lo que podemos usar el siguiente comando.

      chef-server-ctl user-create username firstname lastname email 'password' --filename user-rsa.key
La opción `--filename` nos deja especificar un fichero donde se guardará la clave RSA del usuario que acabamos de crear.
A continuación creamos una organización. Una organización es donde se almacenarán todas las recetas, cookbooks, nodos, etc. que generemos.

      chef-server-ctl org-create short_name 'Full org name.' --association_user username --filename orgname-validator.pem
A la hora de nombrar la organización, tener en cuenta que es necesario que el nombre comience por un número o por minúscula y que ha de ser menor de 256 caracteres.
Para controlar el servidor, instalamos la interfaz web con el comando:

      chef-server-ctl install chef-manage
Una vez instalado, lo reconfiguramos con el comando:

      chef-manage-ctl reconfigure
Una vez hecho esto podemos conectarnos a la web de administración de chef en http://servername/login

Configuración de una worstation
-------------------------------
Una vez nos hemos creado el chef server accedemos a la consola web.  Seleccionamos la organización deseada dentro de administración, _en un principio sólo habrá una_, en el panel de la izquierda pinchamos en _"Starter Kit"_. Ahí encontramos un botón desde el que nos podemos descargar el starter kit.
Una vez descargado, lo subimos al equipo que queramos usar como workstation y lo descomprimimos; se nos creará un árbol de directorios dentro de una carpeta llamada "chef-repo".
En `chef-repo/.chef` tenemos el archivo con nuestra clave privada, y el fichero `knife.rb` que contiene la configuración de la herramienta de administración de chef _knife_.
Para poder usar la herramienta knife, antes tenemos que importar sus certificados con el comando:

      sudo knife ssl fetch
Una vez configurada la herramienta la podemos empezar a usar. La herramienta knife la usaremos cuando queramos comunicarnos con el chef server, como por ejemplo para subir un cookbook al server:

      knife cookbook upload cookbookName

Configuración inicial de un nodo
--------------------------------
Una vez que tenemos nuestro chef server y nuestra workstation configuradas, podemos empezar a configurar nodos desde nuestra workstation.
Para esto usaremos el siguiente comando desde la carpeta donde hemos descomprimido el starter kit:

      [chef-repo]$ knife bootstrap ip.addr -N nombreNodo --ssh-user username --sudo
Si no usamos `-N nombreNodo` para poner nombre al nodo, y nos estamos conectando usando el "organization-validator.key" por defecto el nombre del nodo será el hostname del nodo. Si no nos estamos conectando con el "organization-validator.key" el comando nos dará error y nos obligará a usar -N.
El modificador `--ssh-user` especifica el usuario mediante el que nos conectaremos, y el comando `--sudo` indica a knife que el usuario que usamos necesita sudo para obtener privilegios de administrador.
