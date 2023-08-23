# Minitaller QEMU
## Setup Inicial

Primero debemos verificar que nuestra computadora es capaz de virtualizar. Para ello corremos el siguiente comando.
```
egrep -c '(vmx|svm)' /proc/cpuinfo
```
Despues de correrlo deberiamos ver la cantidad de nucleos disponibles para la virtualizacion (debe ser mayor a 0).

## Instalación

A continuacion vamos a instalar QEMU asi como una interfaz grafica y todas sus dependencias.
```
sudo apt install qemu-kvm qemu-system qemu-utils python3 python3-pip libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager -y
```

Ahora verificaremos que el servicio libvirtd este funcionando correctamente (necesitamos que libvirtd este activo siempre que queramos usar QEMU).
```
sudo systemctl status libvirtd.service
```

## Creacion de red para las maquinas virtuales

```
sudo virsh net-start default
```
