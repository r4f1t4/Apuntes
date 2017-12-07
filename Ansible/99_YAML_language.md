YAML
====

YAML es el nombre del formato que usa Ansible para sus playbooks. Es usado por Ansible y por otros muchos proyectos para almacenar datos. El proposito del formato es almacenar los datos en un formato que sea fácil de leer y entender para una persona.

Formato
-------

Al principio de un fichero en YAML veremos que en la primera línea aparecen tre guiones `---` Esto marca el inicio de un fichero de configuración. En el formato standard se usa también para separar distintos documentos dentro del mismo flujo de datos.

Las listas empiezan con `-` por ejemplo:

```yaml
---

- Coches:
    - Subaru
    - Toyota
    - Honda

- Motos:
    - Honda
    - Suzuki
    - Kawasaki
```

En el ejemplo anterior tenemos 3 listas, la primera, aunque no aparezca nombrada sería la lista de coches y motos, que a su vez contienen sendas listas. 

Para agrupar los elementos de las listas, es necesario sangrar correctamente el texto. Es importante que este sangrado lo hagamos usando espacios; fallará si usamos tabuladores.


