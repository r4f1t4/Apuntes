Rutas para los laboratorios en KVM con el portátil y el sobremesa
=================================================================
Estas rutas son con las que tengo configurado el laboratorio de máquinas virtuales entre el equipo sobremesa y el portátil. Con estas rutas los equipos se pueden comunicar entre sí y con Internet. Además cada red tiene su segmento predefinido.

Las IPs de los equipos físicos son las siguientes:

  - Router:		192.168.1.1
  - Sobremesa: 		192.168.1.50
  - Portátil:		192.168.1.54

A cada máquina virtual le asignamos dos tarjetas de red, conectadas a dos redes según corresponda.

  - **default** rango 192.168.122.0/24 y tipo NAT.
  - **laptop** rango 192.168.101.0/24 de tipo enrutado. Sólo en las máquinas que están en el portátil.
  - **sobremesa** rango 192.168.100.0/24 de tipo enrutado. Sólo en las máquinas virtuales definidas en el equipo sobremesa.

Sobremesa:
----------

El equipo sobremesa ha de quedar con las siguientes redes para permitir la intercomunicación.

```
default via 192.168.1.1 dev enp0s7 proto dhcp metric 100 
192.168.1.0/24 dev enp0s7 proto kernel scope link src 192.168.1.50 metric 100 
192.168.100.0/24 dev virbr1 proto kernel scope link src 192.168.100.1 
192.168.101.0/24 via 192.168.1.54 dev enp0s7 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 
```

Portátil:
---------

El equipo portátil ha de quedar con las siguientes redes para permitir la intercomunicación.

```
default via 192.168.1.1 dev wlp3s0 proto static metric 600 
192.168.1.0/24 dev wlp3s0 proto kernel scope link src 192.168.1.54 metric 600 
192.168.100.0/24 via 192.168.1.50 dev wlp3s0 
192.168.101.0/24 dev virbr1 proto kernel scope link src 192.168.101.1 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 
```

Virtual Sobremesa:
------------------

Los equipos virtuales han de tener configuradas dos redes. Una conectada a la red nat, que podemos dejar configurada con dhcp y por donde irá nuestro gateway por defecto. La otra con IP estática y configurada para que no sea nunca la puerta de enlace por defecto. 

Por ejemplo, para un host _Centos_ o _RedHat_ configuraremos las siguientes rutas en un fichero llamado `route-ifname` donde ifname el el nombre de la configuración que le hemos dado a nuestra interfáz. Por simplicidad es recomendable usar el nombre del dispositivo, por ejemplo `route-eth1` y darle el nombre _eth1_ a la configuración de la red.

El archivo de configuración se guardará en `/etc/sysconfig/network-scripts/route-eth1` y tendrá el siguiente contenido:

```
192.168.1.54/32 via 192.168.100.1
192.168.101.0/24 via 192.168.100.1
```


Las máquinas virtuales de este entorno han de quedar con las siguientes redes para permitir la intercomunicación.

```
default via 192.168.122.1 dev eth0 proto static metric 100 
192.168.1.54 via 192.168.100.1 dev eth1 
192.168.100.0/24 dev eth1 proto kernel scope link src 192.168.100.11 metric 100 
192.168.101.0/24 via 192.168.100.1 dev eth1 
192.168.122.0/24 dev eth0 proto kernel scope link src 192.168.122.103 metric 100 

```

Virtual Portátil:
-----------------

La configuración de las máquinas virtuales que corren en el equipo portátil es similar a la del sobremesa, pero teniendo en cuenta que la red por la que se comunicarán con el host es la 192.168.101.0.

El fichero `/etc/sysconfig/network-scripts/route-eth1` tendrá este contenido:

```
192.168.1.50/32 via 192.168.101.1
192.168.100.0/24 via 192.168.101.1
```

Y la configuración de las rutas quedará así:

```
default via 192.168.122.1 dev eth0 proto static metric 100 
192.168.1.50 via 192.168.101.1 dev eth1 
192.168.100.0/24 via 192.168.101.1 dev eth1 
192.168.101.0/24 dev eth1 proto kernel scope link src 192.168.101.11 metric 100 
192.168.122.0/24 dev eth0 proto kernel scope link src 192.168.122.158 metric 100 
```

