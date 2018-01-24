Ansible Conditionals
====================

Ansible nos permite usar [condiciones](http://docs.ansible.com/ansible/latest/playbooks_conditionals.html) _(conditionals)_ para decidir si una parte de un playbook se ejecuta o no o para realizar bucles. 

La directiva _when_
-------------------

La forma mas similar a un _if_ en Ansible es la directiva [when](http://docs.ansible.com/ansible/latest/playbooks_conditionals.html#the-when-statement) Esta directiva nos permite filtrar cuando se ejecutan determinadas partes de un playbook.

Como nota hay que tener en cuenta que las variables que usemos dentro del apartado _when_ no necesitan estar envueltas entre corchetes `{{}}`. Esto también incluye a los _Ansible facts_

Ejemplo:

```yaml
tasks:
  - name: "Instalar Apache en RedHat"
    yum:
      name: httpd
      state: latest
    when: ansible_os_family == "RedHat"
  - name: "Instalar Apache en Debian"
    apt:
      name: apache2
      state: latest
    when: ansible_os_family == "Debian"
```

Podemos poner más de una condición, si las ponemos en formato lista, _cada una en una línea empezando por guión_, las condiciones se evalúan como si estuviesen unidas por `and`. También podemos definir las condiciones de forma explícita y agrupar mediante paréntesis.

Ejemplo:

```yaml
tasks:
  - name: "shut down CentOS 6 and Debian 7 systems"
    command: /sbin/shutdown -t now
    when: (ansible_distribution == "CentOS" and ansible_distribution_major_version == "6") or
          (ansible_distribution == "Debian" and ansible_distribution_major_version == "7")
```

También podemos definir variables dentro del propio playbook y usarlas posteriormente en los módulos. Por ejemplo:

```yaml
---
- hosts: all
  vars:
    secret_file: /etc/hosts
    secure_environment : true
  tasks:
  - name: Copy secret file to ansible user directory in secure environments
    copy:
      src: "{{ secret_file }}"
      dest: "{{ansible_user_dir}}/secretfile.txt"
      mode: 0600
    when:  secure_environment
```

La directiva *with\_items*
--------------------------

Esta directiva es similar a un _Foreach_ en lenguajes de programación. Ejecuta el código o módulo correspondiente una vez por cada uno de los ítem definidos.

Ejemplo:

```yaml
tasks:
- name: Instalar las utilidades de administracion preferidas
  yum:
    name: "{{ item }}"
    state: latest
  with_items: 
    - wget
    - telnet
    - elinks
```

Si los ítems por los que queremos iterar los tenemos definidos en el apartado de vars, o en un fichero de variables, lo ejecutaríamos de la siguiente forma:

```yaml
vars:
  favorites:
    - wget
    - telnet
    - elinks
tasks:
- name: "Instalar las utilidades de administracion preferidas"
  yum:
    name: "{{ item }}"
    state: latest
  with_items: "{{ favorites }}"
```

Directiva *with\_file*
----------------------

Si lo que queremos iterar son ficheros, podemos usar la directiva `with_file`. Esta es similar a la directiva `with_items`, pero el contenido de la variable será el contenido del fichero.

Ejemplo:

```yaml
tasks:
- name: Muestra el contenido de varios ficheros
  debug:
    msg: "{{ item }}"
  with_file:
    - /etc/redhat-release
    - /etc/resolv.conf
```

Gestión de errores
------------------

Otro aspecto de los playbooks es la gestión de Errores. Por defecto, cuando una tarea falla en un playbook, la ejecución se detiene y nos devuelve un error. Este comportamiento lo podemos cambiar incluyendo la opción `ignore_errors: true` en la tarea que prevemos que pueda fallar. 

Ejemplo desde la documentación de ansible:

```yaml
tasks:
  - command: /bin/false
    register: result
    ignore_errors: True

  - command: /bin/something
    when: result|failed

  # In older versions of ansible use |success, now both are valid but succeeded uses the correct tense.
  - command: /bin/something_else
    when: result|succeeded

  - command: /bin/still/something_else
    when: result|skipped
```

El ejemplo anterior nos muestra un esqueleto de cómo continuar en caso de error en una tarea que se prevé arriesgada, y a continuación realizar una acción u otra en base al resultado de la ejecución que queda almacenado en la variable _result_ mediante la opción ´register: result´

### Bloques

Dentro de la gestión de errores, Ansible soporta desde la versión 2.0 el concepto [blocks](http://docs.ansible.com/ansible/latest/playbooks_blocks.html). Una de las funciones de los bloques agrupar tareas, de modo que podemos aplicarles opciones o incluso condiciones al grupo.

Ejemplo:

```yaml
   tasks:
     - name: Install Apache
       block:
         - yum: name={{ item }} state=installed
           with_items:
             - httpd
             - memcached
         - template: src=templates/src.j2 dest=/etc/foo.conf
         - service: name=bar state=started enabled=True
       when: ansible_distribution == 'CentOS'
       become: true
       become_user: root
```

El anterior ejemplo habilita las opciones `become` y `become_user` para el bloque. Además, añade una condición a todo el bloque para que sólo se ejecute cuando la variable de ansible `ansible_distribution` tenga el valor _CentOS_.

Los bloques también nos dan una funcionalidad similar a las sentencias _try - catch - continue_ habituales en los lenguajes de programación de la siguiente manera:

Ejemplo:

```yaml
  tasks:
   - name: Attempt and gracefull roll back demo
     block:
       - debug: msg='I execute normally'
       - command: /bin/false
       - debug: msg='I never execute, due to the above task failing'
     rescue:
       - debug: msg='I caught an error'
       - command: /bin/false
       - debug: msg='I also never execute :-('
     always:
       - debug: msg="this always executes"
```

El anterior ejemplo sacado de la web de Ansible es autoexplicativo. El bloque estándar `block:` se ejecuta siempre, pero sólo hasta el punto en donde sucede el error. El bloque `rescue:` se ejecuta sólo en caso de error. Y el bloque `always:` se ejecuta siempre.

Tags
----

Aunque técnicamente nos son condicionales, otra forma de agrupar y controlar la ejecución de partes de un playbook es mediante [tags](http://docs.ansible.com/ansible/latest/playbooks_tags.html). Los tags nos permiten especificar que partes de un playbook se van a ejecutar con el siguiente comando

    ansible-playbook --tags "tagName" playbookName.yml

También podemos usar los _tags_ para excluir partes de un playbook de la siguiente forma:

    ansible-playbook --tags "tagName" playbookName.yml

Por último, el tag _allways_ se ejecutará siempre que no se excluya explicitamente; aunque no lo incluyamos al llamar al playbook.

Ejemplo:

```yaml
- name: be sure ntp is installed
  yum: name=ntp state=installed
  tags: ntp

- name: be sure ntp is configured
  template: src=ntp.conf.j2 dest=/etc/ntp.conf
  notify:
    - restart ntpd
  tags: ntp

- name: be sure ntpd is running and enabled
  service: name=ntpd state=started enabled=yes
  tags: ntp
```
