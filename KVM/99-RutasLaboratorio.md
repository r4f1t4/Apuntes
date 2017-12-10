Rutas para los laboratorios en KVM con el portátil y el sobremesa
=================================================================
Estas rutas son con las que tengo configurado el laboratorio de máquinas virtuales entre el equipo sobremesa y el portátil. Con estas rutas los equipos se pueden comunicar entre sí y con Internet. Además cada red tiene su segmento predefinido.


Sobremesa:
----------

```
default via 192.168.1.1 dev enp0s7 proto dhcp metric 100 
192.168.1.0/24 dev enp0s7 proto kernel scope link src 192.168.1.50 metric 100 
192.168.100.0/24 dev virbr1 proto kernel scope link src 192.168.100.1 
192.168.101.0/24 via 192.168.1.54 dev enp0s7 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 
```

Portátil:
---------

```
default via 192.168.1.1 dev wlp3s0 proto static metric 600 
192.168.1.0/24 dev wlp3s0 proto kernel scope link src 192.168.1.54 metric 600 
192.168.100.0/24 via 192.168.1.50 dev wlp3s0 
192.168.101.0/24 dev virbr1 proto kernel scope link src 192.168.101.1 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 
```

Virtual Sobremesa:
------------------

```
default via 192.168.122.1 dev eth0 proto static metric 100 
192.168.1.54 via 192.168.100.1 dev eth1 
192.168.100.0/24 dev eth1 proto kernel scope link src 192.168.100.11 metric 100 
192.168.101.0/24 via 192.168.100.1 dev eth1 
192.168.122.0/24 dev eth0 proto kernel scope link src 192.168.122.103 metric 100 

```

Virtual Portátil:
-----------------

```
default via 192.168.122.1 dev eth0 proto static metric 100 
192.168.1.50 via 192.168.101.1 dev eth1 
192.168.100.0/24 via 192.168.101.1 dev eth1 
192.168.101.0/24 dev eth1 proto kernel scope link src 192.168.101.11 metric 100 
192.168.122.0/24 dev eth0 proto kernel scope link src 192.168.122.158 metric 100 
```

