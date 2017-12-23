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
