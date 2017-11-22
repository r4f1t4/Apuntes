Environments
============

En Chef podemos definir entornos _-environments-_ que podemos usar para crear grupos de servidores de desarrollo, integración, produccion, etc. Esto nos permite asignar versiones concretas de cookbooks a cada entorno para automatizar y/o controlar el despliegue de nuevas versiones de aplicaciones etc.

Un entorno puede tener unas versiones especificadas de cookbooks, y aún así permitir que se ejecuten otras dependiendo de la configuración del entorno.

Por defecto, todos los nodos definidos en chef pertenecen al entorno `_default`, este entorno no puede ser modificado.


