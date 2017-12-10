Ansible Ad-Hoc Commands
=======================

Ejecutando comandos arbitrarios con _Ansible_
----------------------------------------------

_Ansible_ nos permite lanzar comandos contra grupos de máquinas sin necesidad de más configuraciones. El comando a usar sería el siguiente:

    ansible group -b -K -a "/path/to/command/ arguments"

Donde:

  - `-b` o `--become` le dice a _ansible_ que ha de usar _sudo_ para ejecutar el comando.
  - `-K` o `--ask-become-pass` Hace que pida la contraseña para usar sudo.
  - `-a` o `--args=` nos permite introducir a continuación los argumentos del módulo, que en el caso por defecto es ejecución.

El comando `ansible` tiene otros muchos modificadores que podemos consultar con `ansible --help`. Por ejemplo
  - `-k` o `--ask-pass` que nos preguntará por la password para conectarnos a las máquinas del grupo indicado.

Para definir un grupo, podemos editar el fichero hosts de ansible. Por defecto está ubicado en `/etc/ansible/hosts`. Los nombres de los grupos se encierran entre corchetes \[GRUPO\] y a continuación ponemos en una línea cada host que corresponda a ese grupo. Un host puede pertenecer a más de un grupo.

Para ver que máquinas corresponden a un grupo usamos el siguiente comando:

    ansible GROUP --list-hosts

El módulo ping
--------------

El modificador `-m NOMBRE_MÓDULO` o `--module-name=NOMBRE_MÓDULO` nos permite ejecutar módulos predefinidos de ansible. Por ejemplo, el módulo ping. Este módulo simplemente comprobará que todos los equipos del grupo indicado estén online.

La sintaxis del comando es:

    ansible GROUP -m ping

Este comando nos debería devolver una salida como la siguiente, en la que todos los nodos menos uno responden a ping:

```
server2.example.com | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
server1.example.com | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
station1.example.com | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
station3.example.com | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
server3.example.com | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
station2.example.com | UNREACHABLE! => {
    "changed": false, 
    "msg": "Failed to connect to the host via ssh: ssh: connect to host station2.example.com port 22: Connection timed out\r\n", 
    "unreachable": true
}
```

Otros módulos con _Ansible_
---------------------------

Como hemos visto, _Ansible_ nos permite extender su funcionalidad usando módulos. Si vamos la página de documentación de [módulos](http://docs.ansible.com/ansible/latest/modules.html) encontraremos más información.

### El módulo _copy_

El módulo `copy` que nos permite copiar archivos hacia los hosts deseados, por ejemplo:

    ansible GROUP -b -K -m copy -a "src=./hosts dest=/etc/hosts owner=root group=root mode=644"

Este comando copiará el archivo _hosts_ que está en el directorio local y sobrescribirá el archivo `/etc/hosts` de los equipos de destino.

### El módulo _yum_

El módulo yum nos permite gestionar el software instalado mediante yum en sistemas compatibles con el comando. Por ejemplo:

 - El siguiente comando actualiza todos los paquetes instalados a la última versión

    `ansible GROUP -b -K -m yum -a "name=* state=latest"`

 - El siguiente comando instala la última versión el paquete _elinks_ en el grupo indicado. Si el paquete ya está instalado, se asegura de que esté en la última versión y si no lo está lo actualiza.

    `ansible GROUP -b -K -m yum -a "name=elinks state=latest"`

 - Si por ejemplo quisiésemos desinstalar el paquete, el comando a usar sería el siguiente:

    `ansible GROUP -b -K -m yum -a "name=elinks state=removed"`
