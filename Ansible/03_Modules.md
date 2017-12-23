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

Módulo htpasswd
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

Módulo shell
------------

El módulo [shell](http://docs.ansible.com/ansible/latest/shell_module.html) es muy parecido al módulo command, pero ejecuta los comandos a través de una una shell (/bin/sh), por lo que determinadas opciones, como redirecciones y variables de entorno están disponibles.

Ejemplo:

```yaml
- name: Execute the command in remote shell; stdout goes to the specified file on the remote.
  shell: somescript.sh >> somelog.txt
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

Módulo file
-----------

El módulo [file](http://docs.ansible.com/ansible/latest/file_module.html) nos permite manipular archivos. Entre sus posibilidades están la creación y el borrado de archivos y directorios, definir sus atributos, sus permisos, etc.

Ejemplo:

```yaml
- file:
    path: /etc/foo.conf
    owner: foo
    group: foo
    mode: 0644
```

Módulo unarchive
----------------

El módulo [unarchive](http://docs.ansible.com/ansible/latest/unarchive_module.html) nos permite descomprimir un arvhivo en la máquina de destino. El archivo puede estar ya en 4
```

Módulo unarchive
----------------

El módulo [unarchive](http://docs.ansible.com/ansible/latest/unarchive_module.html) nos permite descomprimir un arvhivo en la máquina de destino. El archivo puede estar ya en la máquina, en ese caso lo indicaremos con la opción `remote_src: yes`. Por defecto, el módulo _unarchive_ asume que el fichero está en la workstation y lo transfiere a los servidores de destino.

Ejemplo:

```yaml
- name: Extract foo.tgz into /var/lib/foo
  unarchive:
    src: foo.tgz
    dest: /var/lib/foo
```



Módulo get\_url
--------------

El módulo [get\_url](http://docs.ansible.com/ansible/latest/get_url_module.html) permite descargar archivos desde servicdores http, https o ftp en los servidores remotos.


Ejemplo:


```yaml
- name: Download foo.conf
  get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    mode: 0440
```


Módulo lineinfile
-----------------

El módulo [lineinfile](http://docs.ansible.com/ansible/latest/lineinfile_module.html) nos permite asegurarnos de que una línea dentro de un fichero dado existe o no. Además, también nos permite cambiar una sola línea dentro de un fichero.

Para editar varias líneas similares, podemos usar el módulo [replace](http://docs.ansible.com/ansible/latest/replace_module.html) y para insertar o cambiar un bloque de líneas el módulo [blockinfile](http://docs.ansible.com/ansible/latest/lineinfile_module.html).

Normalmente al usar este módulo especificaremos una expresión regular mediante la opción _regexp:_. Ansible buscará todas las líneas que coincidan, y sólo la última coincidencia será cambiada.

Ejemplos:

Este ejemplo busca la línea que empiece por "SELINUX=" y la cambia por "SELINUX=enforcing"

```yaml
-la máquina, en ese caso lo indicaremos con la opción `remote_src: yes`. Por defecto, el módulo _unarchive_ asume que el fichero está en la workstation y lo transfiere a los servidores de destino.

Ejemplo:

```yaml
- name: Extract foo.tgz into /var/lib/foo
  unarchive:
    src: foo.tgz
    dest: /var/lib/foo
```



Módulo get\_url
--------------

El módulo [get\_url](http://docs.ansible.com/ansible/latest/get_url_module.html) permite descargar archivos desde servicdores http, https o ftp en los servidores remotos.


Ejemplo:


```yaml
- name: Download foo.conf
  get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    mode: 0440
```


Módulo lineinfile
-----------------

El módulo [lineinfile](http://docs.ansible.com/ansible/latest/lineinfile_module.html) nos permite asegurarnos de que una línea dentro de un fichero dado existe o no. Además, también nos permite cambiar una sola línea dentro de un fichero.

Para editar varias líneas similares, podemos usar el módulo [replace](http://docs.ansible.com/ansible/latest/replace_module.html) y para insertar o cambiar un bloque de líneas el módulo [blockinfile](http://docs.ansible.com/ansible/latest/lineinfile_module.html).

Normalmente al usar este módulo especificaremos una expresión regular mediante la opción _regexp:_. Ansible buscará todas las líneas que coincidan, y sólo la última coincidencia será cambiada.

Ejemplos:

Este ejemplo busca la línea que empiece por "SELINUX=" y la cambia por "SELINUX=enforcing"

```yaml
- lineinfile:
    path: /etc/selinux/config
    regexp: '^SELINUX='
    line: 'SELINUX=enforcing'
```

Este ejemplo borra la última línea del fichero /etc/sudoers que empiece por "%wheel"

```yaml
- lineinfile:
    path: /etc/sudoers
    state: absent
    regexp: '^%wheel'
```

Módulo ping
-----------

El módulo [ping](http://docs.ansible.com/ansible/latest/ping_module.html) nos permite comprobar la conectividad de los equipos.

Ejemplo:

```yaml
- ping
```

Módulo script
-------------

El módulo [script](http://docs.ansible.com/ansible/latest/script_module.html) transfiere el script dado desde la workstation hasta los servidores y lo ejecuta.

```yaml
# Run a script that creates a file, but only if the file is not yet created
- script: /some/local/create_file.sh --some-arguments 1234
  args:
    creates: /the/created/file.txt
```

Módulo service
--------------

El módulo [service](http://docs.ansible.com/ansible/latest/service_module.html) nos permite controlar servicios corriendo en máquinas remotas.

Ejemplo:

```yaml
- service:
    name: httpd
    state: started
```

Módulo yum
----------

El módulo [yum](http://docs.ansible.com/ansible/latest/yum_module.html) nos permite gestionar software en equipos que usen yum como gestor de paquetes, como por ejemplo RedHat linux o Centos.

Ejemplo:

```
- name: install the latest version of Apache
  yum:
    name: httpd
    state: latest
```

Módulo apt
----------

Similar al módulo _yum_, el módulo [apt](http://docs.ansible.com/ansible/latest/yum_module.html) gestiona software en equipos que usen apt como gestor de paquetes. (Debian, Ubuntu...)

Ejemplo:

```yaml
- name: Update repositories cache and install "foo" package
  apt:
    name: foo
    update_cache: yes
```

Módulo debug
------------

El módulo [debug](http://docs.ansible.com/ansible/latest/debug_module.html) nos permite mostrar textos durante la ejecución del playbook.

Ejemplo:

```yaml
- debug:
    msg: "System {{ inventory_hostname }} has uuid {{ ansible_product_uuid }}"
```

Si lo que queremos es mostrar la salida de un comando, lo de debemos hacer es almacenarla en una variable registro para poder mostrarla. Por ejemplo:

```yaml
  - command: echo "this is a test"
    register: test
  - debug:
      msg: "El mensaje es {{ test.stdout }} y no debería haber salida de error: {{ test.stderr }}"
```

