Ansible Include and Import
==========================

La forma original de insertar tareas, o incluso playbooks completos, dentro de un playbook era mediante la sentencia `import`; pero desde la versión 2.4 de **Ansible** se considera obsoleta; esta sentencia ha sido substituida por las sentencias `import_playbook`, `import_task`, `include_playbook` e `include_task`. La página de [documentación](https://docs.ansible.com/ansible/2.4/playbooks_reuse_includes.html) explica las diferencias entre estas cuatro diferentes formas de incluir código de otros ficheros en un playbook, y la página de [Creating Reusable Playbooks](https://docs.ansible.com/ansible/2.4/playbooks_reuse_includes.html) profundiza más en las diferencias.

Como resumen, diremos que las directivas `import*` son estáticas, y su contenido se procesa una vez en el momento en el que el playbook original es parseado; y las directivas `include*` son dinámicas, y se procesan a medida que llega su momento de ejecución. La principal consecuencia es que las directivas _include_ pueden ser usadas en bucles y las _import_ no.

Un ejemplo de uso de import es el siguiente:

En primer lugar el playbook desde el que importaremos una task:

```yaml
---
- hosts: master
  tasks:
    - import_tasks: tasks/main.yml
```

Y en segundo lugar la task:

```yaml
---
- shell: cat /etc/motd
  register: motd_contents
- debug:
    msg: "stdout={{motd_contents}}"
- debug:
    msg: "MOTD is EMPTY"
  when: motd_contents.stdout == ""
```

Ansible Roles
=============

Otra opción para reutilizar código, o al menos dividir playbooks en secciones más pequeñas para que el mantenimiento posterior sea más sencillo, es mediante el uso de [roles](https://docs.ansible.com/ansible/2.4/playbooks_reuse_roles.html). La diferencia entre un rol y una tarea, es que en vez de definir una tareas e importarla directamente llamando al nombre de archivo que la contiene, con un rol, creamos una carpeta con el nombre del rol que contiene una jerarquía de carpetas con nombres predefinidos para cada tipo de archivo que es posible incluir en el rol. 

Esto es más fácil de visualizar con la siguiente jerarquía de ejemplo de rol:

```
site.yml
webservers.yml
fooservers.yml
roles/
   common/
     tasks/
     handlers/
     files/
     templates/
     vars/
     defaults/
     meta/
   webservers/
     tasks/
     defaults/
     meta/
```

Cada una de esas carpetas contendrá un archivo **main.yml** con la configuración relevante para cada carpeta. Los tipos de carpeta y sus contenidos son los siguientes:

- **tasks** - Contiene la lista principal de tareas que se ejecutarán en el rol.
- **handlers** - Contiene los _handlers_ del rol. Pueden ser usados desde el rol o desde fuera del rol.
- **defaults** - Variables por defecto para el rol. Cualquier otra definición de la misma variable en Ansible tiene preferencia sobre las variables definidas en esta carpeta. Más información en la [documentación](https://docs.ansible.com/ansible/2.4/playbooks_variables.html)
- **vars** - Otras variables para el rol, las variables definidas en esta carpeta tienen preferencia sobre las definidas en cualquier otra parte.
- **files** - Contiene los archivos que serán desplegados mediante el rol.
- **templates** - Contiene las plantillas que se usarán en el rol.
- **meta** - Metadatos del rol, dependencias de otros roles, si el rol se podrá ejecutar más de una vez con idénticos parámetros, etc.



