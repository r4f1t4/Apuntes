Environments
============

En Chef podemos definir entornos _-environments-_ que podemos usar para crear grupos de servidores de desarrollo, integración, produccion, etc. Esto nos permite asignar versiones concretas de cookbooks a cada entorno para automatizar y/o controlar el despliegue de nuevas versiones de aplicaciones etc.

Un entorno puede tener unas versiones especificadas de cookbooks, y aún así permitir que se ejecuten otras dependiendo de la configuración del entorno.

Por defecto, todos los nodos definidos en chef pertenecen al entorno `_default`, este entorno no puede ser modificado.

Para crear un entorno lo podemos hacer desde la web de Chef manage. **En Policy --> Manage --> _Create_**. Desde este apartado podemos crear el entorno, definir sus _constraints_ que nos permiten filtrar que versiones de cookbooks admiten, sus atributos por defecto e incluso sobrescribir ciertos atributos.

Para asignar nodos a un entorno u otro, lo podemos hacer también desde la página de administración en **Nodes**. Dentro de los detalles de cada nodo podemos entre otras cosas seleccionar el entorno al que pertenecen.
