Comandos varios relacionados con virtualización KVM
===================================================

virt-builder
------------
El comando `virt-builder`

    virt-builder distribution --root-password password:mypassword

virt-install
------------
El comando `virt-install` nos permite crear nuevas máquinas virtuales.

El siguiente comando crea una nueva máquina a partir de una imagen de disco ya existente, como por ejemplo una ya creada desde `virt-builder`. La máquina resultante tendrá como nombre _machinename_ 1024Mb de ram y 1 cpu virtual asignada.

    virt-install --name machinename --ram 1024 --vcpus=1 --disk path=/path/to/image.img --import
