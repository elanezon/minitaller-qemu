# Virtualization (VirtManager)
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
Primero es necesario habilitar el servicio de redes virtuales.
```
sudo virsh net-start default
```
Ahora con este comando haremos que inicie automaticamente al reiniciar la computadora.
```
sudo virsh net-autostart default
```
Tambien es necesario agregar los permisos necesarios para que el servicio se pueda conectar con las maquinas virtuales.
```
sudo usermod -aG libvirt $USER
sudo usermod -aG libvirt-qemu $USER
sudo usermod -aG kvm $USER
sudo usermod -aG input $USER
sudo usermod -aG disk $USER
```
Listo! Terminamos el setup inicial, solo falta reiniciar el equipo.

