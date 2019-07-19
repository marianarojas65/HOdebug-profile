En la carpeta `bugs/`
# bugs
Se compilo con make all y se generan los siguientes ejecutables los cuales una vez ejecutados me dan en pantalla:
1) add_array_dynamic.e
`The addition is 6`

2) add_array_nobugs.e 
`The addition is 6`

3) add_array_segfault.e
`Violación de segmento (core generado)`
Para corregir el problema de memoria en la sentencia:
i) se cambia en #add_array
  `for (i = 0; i <= n + 1; i++) {`
por
  `for (i = 0; i < n; i++) {`
ii) Para la alocación dinámica de memoria en `main`
  int *a, *b;
tengo que incluir la librería
#include <stdlib.h> 
y en main 
  a = malloc(sizeof(int) * 3);
  b = malloc(sizeof(int) * 3);
iii) compilo `make segfault` y corro el programa
`The addition is 6` - ahora anda bien

4) add_array_static.e
`The addition is 329903526` - corre pero el resultado es erroneo
i) cambio
`  for (i = 0; i <= n + 1; i++) {`
por 
`  for (i = 0; i < n; i++) {`
con esta corrección el programa da el resultado correcto - hay un warning
ii) corrijo la allocación de memoria
#include <stdlib.h>
En main, cambio la declaración de los vectores a almacenamiento dinámico
`  int *a, *b;`
`  a = malloc(sizeof(int) * 3);`
`  b = malloc(sizeof(int) * 3);`
ahora corre y da el resultado deseado.
# Un nuevo ejercicio
En dynamic.c
`  int sum = 1;`
`  int i = 0;`
`  for (i = 1; i <= n + 1; i++) {`
`    sum=sum*abs(a[i]);`
`    sum=sum*abs(b[i]);`
Con las correcciones me da
#The addition is 36

Cómo uso un debugger?
gdb ./my_program.e

# Floating point exception

En la carpeta `fpe/` hay tres códigos de C, independientes, para
compilar.  

2/3 programas puedo compilar pero el tercero, square_root.c, no se puede compilar porque da un error en la función sqrt 
`gcc -g square_root.c -o square_root.e`

i) primer intento no logra generar el ejecutable

square_root.e
square_root.c: In function ‘main’:
square_root.c:26:9: warning: implicit declaration of function ‘sqrt’ [-Wimplicit-function-declaration]
   tmp = sqrt(tmp);
         ^
square_root.c:26:9: warning: incompatible implicit declaration of built-in function ‘sqrt’
square_root.c:26:9: note: include ‘<math.h>’ or provide a declaration of ‘sqrt’
/tmp/ccBUQH8L.o: En la función `main':
/home/gab223pc01/Documentos/WTPC-Mariana/Jueves-18-julio/HOdebug-profile/debug/fpe/square_root.c:26: referencia a `sqrt' sin definir
collect2: error: ld returned 1 exit status

ii) Para corregir el error en la función `sqrt`
entro en el directorio `fpe_x87_sse` y compilo la librería
gcc -c fpe_x87_sse.c -o fpe_x87_sse.o
gcc -c square_root.c -o square_root.o
gcc square_root.o fpe_x87_sse.o -o square_root.e

Pero no podemos crear el ejecutable.

iii) Se realizó un linqueo dinámico con el flag -lm square_root.c
gcc fpe_x87_sse.o square_root.o -lm -o square_root.e
ahora si se genera el ejecutable que funciona bien.

- ¿Qué función requiere agregar `-DTRAPFPE`? ¿Cómo pueden hacer que el
programa *linkee* adecuadamente?

- Para cada uno de los ejecutables, ¿qué hace agregar la opción
`-DTRAPFPE` al compilar? ¿En qué se diferencian los mensajes de salida
de los programas con y sin esa opción cuando tratan de hacer una
operación matemática prohbida, como dividir por 0 o calcular la raíz
cuadrada de un número negativo?

















