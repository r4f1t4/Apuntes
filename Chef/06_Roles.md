Chef Roles
==========
Un rol en Chef es básicamente una colección de recipes, _o incluso otros roles,_ que podemos asignar a un grupo de equipos. Esto es útil por ejemplo para definir un rol `bd_mysql_appA` y asignarlo a todos los servidores que vayan a dar soporte a la _app A_ y vayan a ser ser servidores de bd con mysql.
Un rol puede ser asignado dentro de otro rol. Eso es útil para por ejemplo definir roles muy básicos como backup, loging, monitorización, etc. dentro de recipes, o roles y asignarlos a todos los tipos de servidores que sean necesarios. 
Para asignar un rol a una máquina, se lo añadimos a su run-list. Por ejemplo desde knife:

    knife node run_list set nodename "role[roleName]"

Si queremos ejecutar el rol, podemos, o esperar a que se auto-ejecute, _si está configurado_ conectarnos a la máquina en cuestión y ejecutar el rol, o ejecutarlo de forma remota con el comando `knife ssh` por ejemplo:

    knife ssh "role:web" "sudo chef-client" -x user -P password

El anterior comando 
