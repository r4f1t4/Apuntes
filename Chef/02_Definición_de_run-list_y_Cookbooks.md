Introducción a las _run-lists_ y los _cookbooks_
================================================

Run lists
---------

Una _run-list_ es una lista de cookbooks/recipes que se ejecutarán en un nodo determinado.

      run_list "recipe[base]","recipe[apache]""recpie[selinux_policy]"

El comando anterior lo que está entre corchetes, son cookbooks. Dentro de los cookboks, si no especificamos ninguna recipe (como en el comando anterior) solo se ejecutará la recipe que esté definida por defecto y las recipes a las que llame esta. Para especificar que se ejecute una recipe dentro de un cookbook desde un `run_list` usamos la siguiente nomenclatura:

      run_list "recipe[base::recipe]","recipe[apache::recipe]""recpie[selinux_policy::recipe]"

Si una recipe se asigna a un nodo más de una vez, Chef sólo la ejecutará una vez.

Include_recipe
--------------
Una recipe puede llamar a otra recipe para que se ejecute desde un cookbook diferente. La nomenclatura sería la siguiente:
      include_recipe 'cookbook_name::recipe_name'

Cookbooks
---------
Los cookbooks pueden contener la siguiente información:
 - recipes
 - atribute files
 - file descriptions
 - templates
 - Extensiones de Chef, como librerías y custom resources.

Los cookbooks de Chef se usan para definir un escenario. Por ejemplo, un cookbook puede definir todo lo necesario para instalar y configurar un servidor web apache.

Cada cookbook tiene que contener los siguientes ficheros:
 - **README.md** Dentro del path `cookbooks/cookbookname/README.md` tiene que existir un fichero con la descripción del cookbook escrito en Markdown.
 - **metadata.rb** Dentro del path `cookbooks/cookbookname/metadata.rb` contiene al menos:
    - La versión de Chef para la que está escrito el cookbook.
    - Las dependencias sobre otros cookbooks o sobre otras versiones del mismo cookbook. (necesario para el `include_recipe`)
    - La versión del cookbook. Los cookbooks se suben al servidor de Chef, y este es capáz de almacenar varias versiones de un mismo cookbook, por lo que es necesario que cada cookbook indique su versión.

El contenido típico del _metadata.rb_ sería el siguiente:

```
name 'mycookbook'
maintainer 'The Authors'
maintainer_email 'you@example.com'
license 'all_rights'
description 'Installs/configures mycookbook'
long_description 'Installs/configures mycookbook'
version '0.1.0'
depends 'mysql_cookbook','>=1.0'
```
 - **default.rb** Esta es la recipe por defecto del cookbook que se ejecuta que se ejecuta al incluir el cookbook en un run/_list. Todas las demás recipes son ignoradas, salvo que sean llamadas directemente desde run/_list, o desde la default.rb usando include/_recipe. Generalmente usamos este fichero para hacer la parte principal del trabajo, como instalar paquetes, o arrancar servicios. 
    - los siguientes dos ejemplos de run_list son equivalentes:

```
run_list "recipe[apache]
run_list "recipe[apache::default]
```

Generar un Cookbook
-------------------
El primer paso para generar un cookbook es crear un árbol de configuración vacío que luego rellenaremos con los detalles particulares del cookbook. Chef nos ayuda en esto creándonos un árbol vacío con el comando:

      chef generate cookbook /path/to/cookbook/name
Dentro de la carpeta "name" se creará una plantilla de cookbook con nombre "name" vacía.
La estructura del cookbook creado es la siguiente:

```
.
├── Berksfile
├── chefignore
├── LICENSE
├── metadata.rb
├── nodes
│   └── r4f1t41.mylabserver.com.json
├── README.md
├── recipes
│   ├── default.rb
│   └── learn.rb
├── spec
│   ├── spec_helper.rb
│   └── unit
│       └── recipes
│           └── default_spec.rb
└── test
    └── smoke
        └── default
            └── default_test.rb
```
