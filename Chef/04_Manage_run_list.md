Uso de run\_lists contra un nodo y configuración del nodo
=========================================================

Añadiendo run\_lists con el comando _knife_
-------------------------------------------
En primer lugar comprobamos contra el servidor de Chef que nodos hay disponible. Para ello usamos el siguiente comando, que mostrará una lista de los servidores disponibles.

      knife node list
Una ves que sepamos a que nodo queremos añadir una run\_list usamos el siguiente comando:

      knife node run_list add node.name 'recipe[cookbook]'
Ese comando añade la recipe por defecto del cookbook _cookbook_ al nodo.

Para saber que run\_lists tiene añadidas un nodo tenemos varias opciones. La primera puede ser ejecutar el siguiente comando:

      knife node show node.name
Nos mostrará entre otra información las run lists que tenga asociadas actualmente. Si usamos además el modificador `-l` nos dará toda la información que tiene del nodo. Esa información es la misma que se puede consultar en el nodo con el comando `ohai` y se puede usar en las recipes.

Para añadir mas recipes a la run\_list podemos re-ejectuar el comando anterior. Al hacer eso, la recipe se añade al final de todas las recipes que puede tener ya asignadas ese nodo.

      knife node run_list add node.name 'recipe[cookbook::recipeName]'
Podemos afinar más en la inserción de nuestra recipe con los modificadores `-a` y `-b`. Con `-a` la recipe se insertará después de la recipe especificada **after** y con `-b` antes **before**. Los comandos de inserción quedarían del siguiente modo:

      knife node run_list add node.name 'recipe[cookbook::secondRecipeName]' -a 'recipe[cookbook::firstRecipeName]'
      knife node run_list add node.name 'recipe[cookbook::firstRecipeName]' -b 'recipe[cookbook::secondRecipeName]'

Para finalizar, si queremos borrar una o más recipes usamos `remove` en vez de `add` del siguiente modo:

      knife mode run_list remove server.name 'recipe[cookbook::recipe1],recipe[cookbook::recipe2], ... recipe[cookbook::recipeN]'

Al añadir multiples recetas en un comando, las recetas se añadirán a la run list en el orden indicado, por lo que el orden es relevante. Sin embargo, cuando borramos recetas el orden es indiferente.

Ejecutando la run-list asignada
-------------------------------
El siguiente paso lógico, sería ejecutar la run list que acabamos de asignar, sin embargo, corremos el riesgo de causar problemas si la run list no hace exactamente lo que nosotros pensamos. Para ello existe el modo _why run_. Si le pasamos al comando `chef-client` la opción `--why-run` Chef nos mostrará todos los puntos por los que pasaría la run list para darnos una idea de que sucederá una vez que ejecutemos la run list.

      chef-client --why-run 


Configuración del nodo cliente
==============================
La configuración de los nodos clientes se encuentra en la carpeta `/etc/chef` de cada nodo. En particular, dentro de dicha carpeta, en el fichero _client.rb_ podemos encontrar las siguientes opciones:
+ la ubicación de los logs
+ La url del servidor Chef
+ El nombre del nodo.
+ La ubicación de los _trusted certificates_ para conectarse con los servidores Chef.


