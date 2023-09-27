# Full Emulation
## Setup Inicial

Como es costumbre, iniciamos actualizando nuestros paquetes y repositorios.

```
sudo apt update && sudo apt upgrade -y
```

## Instalación

Instalamos los paquetes de qemu necesarios asi como el bios y el setup de UEFI para poder bootear nuestra maquina RISC-V
```
sudo apt-get install opensbi qemu-system-misc u-boot-qemu
```
Descargamos la imagen preinstalada para un servidor RISC-V dando click al siguiente link:
https://cdimage.ubuntu.com/releases/22.04.3/release/ubuntu-22.04.3-preinstalled-server-riscv64+unmatched.img.xz?_ga=2.59623140.814164571.1695846449-657263962.1692753967

Si el link de descarga directa no funciona buscar la imagen para QEMU en su versión Ubuntu 22.04.3 en el siguiente link:
https://ubuntu.com/download/risc-v

Ahora copiamos la imagen a nuestro directorio de trabajo y procedemos a descomprimirla.
```
xz -dk ubuntu-22.04.3-preinstalled-server-riscv64+unmatched.img.xz
```
Para poder instalar los programas que requerimos sin problemas es necesario aumentar el tamaño de la imagen, con el siguiente comando la aumentamos en 5GB. El sistema de archivos automaticamente se expande para tomar el nuevo tamaño.
```
qemu-img resize -f raw ubuntu-22.04.2-preinstalled-server-riscv64+unmatched.img +5G
```
Tenemos todo listo para correr nuestra maquina RISC-V, la iniciamos con el siguiente comando.
```
qemu-system-riscv64 \
-machine virt -nographic -m 2048 -smp 4 \
-bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin \
-kernel /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf \
-device virtio-net-device,netdev=eth0 -netdev user,id=eth0 \
-device virtio-rng-pci \
-drive file=ubuntu-22.04.2-preinstalled-server-riscv64+unmatched.img,format=raw,if=virtio
```
