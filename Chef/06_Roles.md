Chef Roles
==========

Introducción
------------
Un rol en Chef es básicamente una colección de recipes, _o incluso otros roles,_ que podemos asignar a un grupo de equipos. Esto es útil por ejemplo para definir un rol `bd_mysql_appA` y asignarlo a todos los servidores que vayan a dar soporte a la _app A_ y vayan a ser ser servidores de bd con mysql.
Un rol puede ser asignado dentro de otro rol. Eso es útil para por ejemplo definir roles muy básicos como backup, loging, monitorización, etc. dentro de recipes, o roles y asignarlos a todos los tipos de servidores que sean necesarios. 
Para asignar un rol a una máquina, se lo añadimos a su run-list. Por ejemplo desde knife:

    knife node run_list set nodename "role[roleName]"

Si queremos ejecutar el rol, podemos, o esperar a que se auto-ejecute, _si está configurado,_ conectarnos a la máquina en cuestión y ejecutar el rol, o ejecutarlo de forma remota con el comando `knife ssh` por ejemplo:

    knife ssh "role:roleName" "sudo chef-client" -x user -P password

El anterior comando funciona de forma similar a las búsquedas, en este caso ejecuta el comando especificado en todas las máquinas que tengan el rol _roleName_ asignado. Si antes de ejecutar el comando queremos comprobar en qué máquinas se ejecutará simplemente usaríamos el comando _knife search_ del siguiente modo: `knife search 'role:rolename'`

Creación de un rol
------------------
Para crear un rol, nos vamos a la carpeta de nuestra workstation donde tenemos configurado el repositorio de Chef y ejecutamos el siguiente comando:

    knife role create roleName
Esto nos crea la plantilla del rol y nos la abre en el editor que tengamos definido por defecto. En la plantilla inicialmente sólo vamos a editar el run-list, y con eso ya podremos asignar el rol a los equipos que deseemos.
Ejemplo de plantilla de rol:

``` json
{
  "name": "roleName",
  "description": "",
  "json_class": "Chef::Role",
  "default_attributes": {

  },
  "override_attributes": {

  },
  "chef_type": "role",
  "run_list": [
        "recipe[cookbook]",
        "recipe[cookbook::recipe1]",
        "recipe[cookbook::recipe2]"
  ],
  "env_run_lists": {

  }
}
```
