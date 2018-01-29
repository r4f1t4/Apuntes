Ansible Templates
=================

**Ansible** nos permite definir una plantilla en formato Jinja2 con variables que serán substituidas por sus respectivos valores en tiempo de ejecución. Esto nos permite, por ejemplo, definir un archivo de configuración genérico en el que los valores particulares para cada servidor se rellenarán para cada servidor de forma individual en el momento que se ejecute el playbook.

En la documentación de Ansible tenemos ejemplos de cómo funciona el lenguaje de plantillas [Jinja2](http://docs.ansible.com/ansible/latest/playbooks_templating.html) y de cómo ejecutar esas plantillas desde el módulo [template](http://docs.ansible.com/ansible/latest/template_module.html). Información más detallada del funcionamiento de las plantillas en [la plagina oficial de jinja](http://jinja.pocoo.org/docs/2.10/templates/).

Ejemplos de playbook y plantilla usando Jinja2:

**Playbook:**

```yaml
---
- hosts: master
  become: yes
  vars:
    hostname: "{{ ansible_hostname }}"
  tasks:
  - name: Ensure that apache is installed
    yum:
      name: httpd
      state: latest
  - name: write the index file
    template:
      src: 01_firstTemplate.j2
      dest: /var/www/html/index.html
    notify:
    - restart httpd
  - name: Make sure that apache is running
    service:
      name: httpd
      state: started
  - name: Ensure that http port 80 is open
    firewalld:
      service: http
      permanent: true
      state: enabled
      immediate: yes
  handlers:
  - name: restart httpd
    service:
      name: httpd
      state: restarted
```

**Template**

```jinja2
<h1>This is the {{hostname}} web server page</h1>
```
