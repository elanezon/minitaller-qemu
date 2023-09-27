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
qemu-img resize -f raw ubuntu-22.04.3-preinstalled-server-riscv64+unmatched.img +5G
```
Tenemos todo listo para correr nuestra maquina RISC-V, la iniciamos con el siguiente comando.
```
qemu-system-riscv64 \
-machine virt -nographic -m 2048 -smp 4 \
-bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin \
-kernel /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf \
-device virtio-net-device,netdev=eth0 -netdev user,id=eth0 \
-device virtio-rng-pci \
-drive file=ubuntu-22.04.3-preinstalled-server-riscv64+unmatched.img,format=raw,if=virtio
```
Cuando vea el siguiente mensaje en la consola presione Enter:

[  OK  ] Reached target Cloud-init target.

El usuario y la contraseña es ubuntu, debere crear una nueva contraseña para poder ingresar.

Para poder compilar es necesario instalar el paquete build-essentials

Correr el siguiente comando puede tardar un tiempo ya que hay muchos paquetes que actualizar
```
sudo apt update && sudo apt upgrade -y
```
Instalamos los paquetes necesarios para la compilación
```
sudo apt install build-essential
```

Corremos el siguiente comando para asegurarnos que gcc se instalo de manera correcta
```
gcc --version
```
Creamos un archivo piramid.c
```
nano piramid.c
```
Copiamos el siguiente codigo en piramid.c
```
#include <stdio.h>
int main() {
   int rows, coef = 1, space, i, j;
   printf("Enter the number of rows: ");
   scanf("%d", &rows);
   for (i = 0; i < rows; i++) {
      for (space = 1; space <= rows - i; space++)
         printf("  ");
      for (j = 0; j <= i; j++) {
         if (j == 0 || i == 0)
            coef = 1;
         else
            coef = coef * (i - j + 1) / j;
         printf("%4d", coef);
      }
      printf("\n");
   }
   return 0;
}

```
Compilamos el codigo
```
gcc -o pascalstriangle piramid.c
```
Y finalmente lo corremos.
```
./pascalstriangle
```
