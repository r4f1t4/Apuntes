Ansible Ad-Hoc Commands
=======================

_Ansible_ nos permite lanzar comandos contra grupos de máquinas sin necesidad de más configuraciones. El comando a usar sería el siguiente:

    ansible group -b -K -a "/path/to/command/ arguments"

Donde:

  - `-b` o `--become` le dice a _ansible_ que ha de usar _sudo_ para ejecutar el comando.
  - `-K` o `--ask-become-pass` Hace que pida la contraseña para usar sudo.
  - `-a` o `--args=` nos permite introducir a continuación los argumentos del módulo, que en el caso por defecto es ejecución.

El comando `ansible` tiene otros muchos modificadores que podemos consultar con `ansible --help`. Por ejemplo
  - `-k` o `--ask-pass` que nos preguntará por la password para conectarnos a las máquinas del grupo indicado.
