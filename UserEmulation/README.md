# User Emulation QEMU
## Setup Inicial

Primero actualizamos los repositorios y los paquetes con el siguiente comando.
```
sudo apt update -y && sudo apt upgrade -y
```

## Instalación

A continuacion vamos a instalar QEMU-User asi como un compilador para ARM y las librerias necesarias para poder ejecutar programas para esta arquitectura.
```
sudo apt install qemu-user qemu-user-static gcc-aarch64-linux-gnu binutils-aarch64-linux-gnu binutils-aarch64-linux-gnu-dbg build-essential -y
```

Ahora creamos un archivo fibo.c para compilarlo.

```
nano fibo.c
```
Copiamos el siguiente codigo en fibo.c

```
#include <stdio.h>
int main() {

  int i, n;

  // initialize first and second terms
  int t1 = 0, t2 = 1;

  // initialize the next term (3rd term)
  int nextTerm = t1 + t2;

  // get no. of terms from user
  printf("Enter the number of terms: ");
  scanf("%d", &n);

  // print the first two terms t1 and t2
  printf("Fibonacci Series: %d, %d, ", t1, t2);

  // print 3rd to nth terms
  for (i = 3; i <= n; ++i) {
    printf("%d, ", nextTerm);
    t1 = t2;
    t2 = nextTerm;
    nextTerm = t1 + t2;
  }

  return 0;
}
```
Guardamos los cambios y compilamos el archivo de manera estatica.

```
aarch64-linux-gnu-gcc -static -o fibostatic fibo.c
```
Corremos el siguiente comando para asegurarnos que haya sido compilado para la arquitectura deseada.

```
file fibostatic
```
Y ahora corremos el binario.
```
./fibostatic
```
NOTA: Normalmente esta ejecucion habria retornado un error ya que las instrucciones no son compatibles con la arquitectura x86 del sistema pero al haber instalado el paquete qemu-user-static podemos correr programas estaticos de las arquitecturas soportadas sin ningún problema.

## Programa Dinamico
Ahora vamos a compilar de manera dinamica el mismo programa.

```
aarch64-linux-gnu-gcc -o fibodyn fibo.c
```
Y lo ejecutamos de la siguiente manera.
```
qemu-aarch64 -L /usr/aarch64-linux-gnu ./fibodyn
```
El flag -L le indica a qemu-aarch64 donde puede encontrar todas las librerias necesarias para poder ejecutar el codigo de manera dinamica.
