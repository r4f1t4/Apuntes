Apuntes Chef
============
Instalamos el chef development kit en una máquina Linux. Podemos descargarlo de [chef](https://downloads.chef.io)

Una vez instalado lo arrancamos con el comando:

      chef-client --local-mode

Chef usa "recipes" para configurar las máquinas. Las recipes están compuestas por bloques de código en Ruby y cada uno de esos bloques realiza unas tareas especificas.
