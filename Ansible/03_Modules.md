Ansible Modules
===============

Ansible tiene módulos predefinidos para gran variedad de tareas. En la [documentación](http://docs.ansible.com/ansible/latest/modules_by_category.html) de _Ansible_ podemos encontrarlos todos junto a sus opciones y ejemplos variados. 

Dichos módulos pueden ser usados en playbooks o en comandos ad-hoc.

A continuación los módulos más usados.

Módulo user
-----------

El módulo [user](http://docs.ansible.com/ansible/latest/user_module.html) nos permite gestionar usuarios; tanto su creación como su borrado así como multitud de atributos relacionados; como su pertenencia a grupos, contraseña, claves ssh, etc.

Ejemplo:

```yaml
- user:
    name: gollum
    comment: "Hobit Smeagol"
    shell: /bin/csh
    groups: hobits,creatures,ringbearers,oldones
    generate_ssh_key: yes
    ssh_key_bits: 2048
    ssh_key_file: ssh/id_rsa
```

Módulo group
------------

Similar a el módulo _user_, el módulo [group](http://docs.ansible.com/ansible/latest/group_module.html) nos permite crear grupos y configurarlos. Las opciones son más limitadas, ya que sólo podemos crear o borrar el grupo, darle un nombre, establecer el GID y especificar si es o no un grupo de sistema. La pertenencia a grupos la gestionaremos desde los usuarios.

Ejemplo:

```yaml
- group:
    name: hobits
    gid: 7000
``` 

Módulo command 
--------------

El módulo [command](http://docs.ansible.com/ansible/latest/command_module.html) nos permite ejecutar comandos en nodos remotos. El comando no se ejecutará a través de una shell, por lo que ciertas variables no estarán disponibles. Además no podremos usar redirecciones, pipes, etc.

Las opciones más interesantes son:

- _creates_ Permite especificar un fichero, o patrón de ficheros, que si existe hará que el comando no se ejecute. 
- _removes_ Similar a creates, pero el fichero no ha de estar presente.
- _chdir_ Cambia al directorio especificado antes de ejecutar el comando.

Ejemplo:

```yaml
- name: This command will change the working directory to somedir/ and will only run when /path/to/database doesn't exist.
  command: /usr/bin/make_database.sh arg1 arg2
  args:
    chdir: somedir/
    creates: /path/to/database
```

El apartado _args:_ es una forma de incluir los argumentos de un módulo en un playbook. Esta tarea también podría haberse definido del siguiente modo:

```yaml
- name: Run the command if the specified file does not exist.
  command: /usr/bin/make_database.sh arg1 arg2 creates=/path/to/database
```

Módulo copy
-----------

El módulo [copy](http://docs.ansible.com/ansible/latest/copy_module.html) nos permite copiar ficheros desde el equipo local a los equipos remotos definiendo al mismo tiempo los atributos del fichero o directorio.

Según la documentación de Ansible no se recomienda el uso de este módulo para copiar grandes cantidades de ficheros. En su lugar se puede usar el módulo [synchronize](http://docs.ansible.com/ansible/latest/synchronize_module.html) que usa rsync para copiar los ficheros.

Ejemplo:

```yaml
- copy:
    src: /srv/myfiles/foo.conf
    dest: /etc/foo.conf
    owner: foo
    group: foo
    mode: 0644
```

Para descargar ficheros de máquinas remotas podemos usar el módulo [fetch](http://docs.ansible.com/ansible/latest/fetch_module.html). Este módulo nos deja los ficheros descargados organizados por nombre de máquina. 

Modulo get_url
--------------

El módulo [get_url](http://docs.ansible.com/ansible/latest/get_url_module.html) permite descargar archivos desde servicdores http, https o ftp en los servidores remotos.


Ejemplo:


```yaml
- name: Download foo.conf
  get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    mode: 0440
```

Modulo htpasswd
---------------

El módulo [htpasswd](http://docs.ansible.com/ansible/latest/htpasswd_module.html) permite configurar usuarios para páginas web corriendo bajo Apache o Ngnix. 

Ejemplo:

```yaml
- htpasswd:
    path: /etc/nginx/passwdfile
    name: janedoe
    password: '9s36?;fyNp'
    owner: root
    group: www-data
    mode: 0640
```


