Ansible Modules
===============

Ansible tiene módulos predefinidos para gran variedad de tareas. En la [documentación](http://docs.ansible.com/ansible/latest/modules_by_category.html) de _Ansible_ podemos encontrarlos todos junto a sus opciones y ejemplos variados. A continuación los módulos más usados.

User module
-----------

El módulo **_user_** nos permite gestionar usuarios; tanto su creación como su borrado así como multitud de atributos relacionados; como su pertenencia a grupos, contraseña, claves ssh, etc.

Ejemplo:

```yaml
- user:
    name: gollum
    comment: "Hobitt Smeagol"
    shell: /bin/csh
    groups: hobitts,creatures,ringbearers,oldones
    generate_ssh_key: yes
    ssh_key_bits: 2048
    ssh_key_file: ssh/id_rsa
```


