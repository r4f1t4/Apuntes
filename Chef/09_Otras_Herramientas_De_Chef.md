Otras herramientas de Chef
==========================

Chef tiene otras herramientas. A continuación una descripción de ellas.

Habitat
-------

**Habitat** es la herramienta de **Chef** para automatizar el empaquetado y distribución de aplicaciones. Su filosofía es mirar al despliegue de aplicaciones desde el punto de vista del software en vez de desde el de la infraestructura.
En la práctica esto significa que **Habitat** nos va a permitir definir como se va a compilar, desplegar y administrar la aplicación y sus servicios. **Habitat** puede ser desplegado sobre distintas infraestructuras, como hardware físico, máquinas virtuales, containers y en PAAS.

Las ordenes de **Habitat** en la consola comienzan mediante el comando `hab` y para definir como se va a construir e instalar una aplicación hay que crear un fichero `plan.sh`


Compliance
----------
**Compliance** es un servicio autonomo de **Chef**. Puede ser instalado en un servidor y monitorizar una infraestructura para asegurar que esta cumple unos requisitos definidos. Para ello es necesario crear un perfil con los requisitos, **Compliance** usará ese perfil, o perfiles, para conectarse a los servidores de la infraestructura y comprobar que los requisitos se cumplen.
**Compliance** es autónomo poruqe no necesita que el cliente de Chef esté instalado en las máquinas que forman la infraestructura. Usa _ssh_ para conectarse a máquinas UNIX y _WinRM_ para conectarse a servidores Windows.
Los perfles que usa **Compliance** están escritos en lenguage **InSpec** que está diseñado para que sea fácil de leer.

InSpec
------

**InSpec** es el lenguaje que usamos para definir los tests que ejecutaremos sobre la infraestructura, por ejemplo desde **Compliance**, aunque también pueden definirse tests en Cookbooks y ejecutarse mediante el comando `kitchen`.

Un ejemplo de InSpec desde [https://github.com/chef/inspec]

```ruby
# Disallow insecure protocols by testing

describe package('telnetd') do
  it { should_not be_installed }
end

describe inetd_conf do
  its("telnet") { should eq nil }
end
```


