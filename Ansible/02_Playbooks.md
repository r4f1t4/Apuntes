Ansible Playbooks
=================

Los playbooks nos permiten agrupar sets de _"plays"_ o tareas en archivos para luego ejecutarlos. El objetivo es definir unas políticas en los playbooks que hagan que el entorno cumpla unas determinadas características. Los playbooks se han de escribir en formato Yaml.

Ejemplo de playbook desde la [documentación](http://docs.ansible.com/ansible/latest/playbooks_intro.html) de _Ansible_:

```yaml
---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum: name=httpd state=latest
  - name: write the apache config file
    template: src=/srv/httpd.j2 dest=/etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running (and enable it at boot)
    service: name=httpd state=started enabled=yes
  handlers:
    - name: restart apache
      service: name=httpd state=restarted
```

Cuando ejecutamos un playbook y falla, se crea un fichero _.retry_. La propia salida de error nos indica cómo usar ese fichero para ejecutar el playbook de nuevo saltándonos la parte que ya ha ido bien.

El ejemplo anterior, se conecta con el usuario root, para evaluar 3 tareas especificas:

  - Instalar Apache con el módulo _yum_
  - Escribir la configuración de apache con el módulo _template_
  - Asegurarse de que Apache está arrancado y está habilitado para que se arranque automáticamente con el módulo _service_

Además, se pueden llegar a ejecutar dos tareas más.

  - La tarea que escribe el fichero de configuración de apache, tiene un apartado "_notify_" este apartado lo que hace es notificar a otra tarea, que está bajo "_handlers_" que ha de ejecutarse. El resultado es que la tarea "_restart apache_" sólo se ejecuta si el archivo de configuración de Apache ha sido modificado.
  - La tarea _Gathering Facts_ se ejecutará por defecto si no especificamos lo contrario. Esta tarea obtiene información _(facts)_ acerca del servidor al que se conecta.

La ejecución del anterior playbook sería la siguiente:

```
$ ansible-playbook apache-playbook.yml 

PLAY [webservers] ***************************************************************************************

TASK [Gathering Facts] **********************************************************************************
ok: [server1.example.com]

TASK [ensure apache is at the latest version] ***********************************************************
changed: [server1.example.com]

TASK [write the apache config file] *********************************************************************
changed: [server1.example.com]

TASK [ensure apache is running (and enable it at boot)] *************************************************
changed: [server1.example.com]

RUNNING HANDLER [restart apache] ************************************************************************
changed: [server1.example.com]

PLAY RECAP **********************************************************************************************
server1.example.com    : ok=5    changed=4    unreachable=0    failed=0   
```


