Test Kitchen
============

Chef provee la herramienta kitchen para testear nuestras recipes en un entorno virtual antes de implementarlo en un entorno real.

El comando _kitchen_ admite las siguientes variables:
 - `kitchen init` Crea una plantilla .kitchen.yml
 - `kitchen create` Nos permite crea una o más instancias
 - `kitchen list` Lista las instancias
 - `kitchen converge` 
 - `kitchen verify` Verifica las instancias especificadas
 - `kitchen destroy` _Destruye_ las instancias especificadas
 - `kitchen test` Equivale a ejecutar los siguientes comandos uno tras otro en este orden:
    - `kitchen destroy`
    - `kitchen create`
    - `kitchen converge`
    - `kitchen verify`
    - `kitchen destroy`
 - `kitchen login` Se loguea dentro de una instancia
 - `kitchen help`

Ejemplo de .kitchen.yml sacado de [https://docs.chef.io/config_yml_kitchen.html]

```yaml
 driver:
   name: vagrant

 provisioner:
   name: chef_zero

 platforms:
   - name: ubuntu-12.04
   - name: centos-6.4
   - name: debian-7.1.0

suites:
  - name: default
    run_list:
      - recipe[apache::httpd]
    excludes:
      - debian-7.1.0
```

