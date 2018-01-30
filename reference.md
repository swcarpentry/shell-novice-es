---
layout: reference
permalink: /reference/
---

## Resumen de Comandos Básicos

| Acción | Archivos | Carpetas |
| ------------- | ------- | -------------- |
| Inspeccionar | ls | ls |
| Ver contenido | cat | ls |
| Navega hacia | | cd |
| Mover | mv | mv |
| Copiar | cp | cp -r |
| Crear | nano | mkdir |
| Eliminar | rm | rmdir, rm -r |

## Jerarquía del sistema de archivos

La siguiente es una descripción general de un sistema de archivos Unix estándar.
La jerarquía exacta depende de la plataforma,
por lo que es posible que no vea exactamente los mismos archivos / directorios en su computadora:

! [Jerarquía del sistema de archivos de Linux](../fig/standard-filesystem-hierarchy.svg)

## Glosario

{: auto_ids}
path absoluto
: A [path](#path) que hace referencia a una ubicación particular en un sistema de archivos.
    Los paths absolutos generalmente se escriben con respecto al sistema de archivos
    [directorio root](#directorio-root),
    y comience con "/" (en Unix) o "\\" (en Microsoft Windows).
    Ver también: [path relativa](#path-relativa).

argumento
: Un valor dado a una función o programa cuando se ejecuta.
    El término a menudo se usa indistintamente (y de manera inconsistente) con [parámetro](#parámetro).

terminal de comando
: Ver [terminal](#terminal)

interfaz de línea de comando
: Una interfaz de usuario basada en comandos de tipeo,
    generalmente en un [REPL](#read-evaluate-print-loop).
    Ver también: [interfaz gráfica de usuario](#graphical-user-interface).

comentario
: Un comentario en un programa que pretende ayudar a los lectores humanos a entender lo que está sucediendo,
    pero es ignorado por la computadora.
    Los comentarios en Python, R y el terminal de Unix comienzan con un caracter `#` y se ejecutan hasta el final de la linea;
    los comentarios en SQL comienzan con `--`,
    y otros idiomas tienen otras convenciones.

directorio de trabajo actual
: El directorio del que se calculan [paths relativos](#path-relativa);
    equivalentemente,
    el lugar donde se buscan los archivos a los que se hace referencia solo por nombre.
    Cada [proceso](#proceso) tiene un directorio de trabajo actual.
    El directorio de trabajo actual generalmente se refiere a la notación abreviada `.` (pronunciado "punto").

sistema de archivos
: Un conjunto de archivos, directorios y dispositivos de E/S (como teclados y pantallas).
    Un sistema de archivos puede extenderse a través de muchos dispositivos físicos,
    o muchos sistemas de archivos pueden almacenarse en un solo dispositivo físico;
    el [sistema operativo](#sistema operativo) administra el acceso.

extensión de archivo
: La parte del nombre de un archivo que aparece después del "." final.
    Por convención, esto identifica el tipo de archivo:
    `.txt` significa "archivo de texto", `.png` significa "archivo de red portátil de gráficos",
    y así. Estas convenciones no son aplicadas por la mayoría de los sistemas operativos:
    es perfectamente posible (¡pero confuso!) nombrar un archivo de sonido MP3 `homepage.html`.
    Dado que muchas aplicaciones usan extensiones de nombre de archivo para identificar el [tipo MIME](# mime-type) del archivo,
    los archivos de desincronización pueden hacer que esas aplicaciones fallen.

filtrar
: Un programa que transforma una secuencia de datos.
    Muchas herramientas de línea de comandos de Unix están escritas como filtros:
    leen datos de [entrada estándar](#entrada-estándar),
    procesarlo y escribir el resultado en [salida estándar](# salida estándar).

bandera
: Una forma concisa de especificar una opción o configuración a un programa de línea de comandos.
    Por convención, las aplicaciones Unix usan un guion seguido de una sola letra,
    como `-v`, o dos guiones seguidos de una palabra, como` --verbose`,
    mientras que las aplicaciones de DOS usan una barra inclinada, como `/ V`.
    Dependiendo de la aplicación, un indicador puede ir seguido de un único argumento, como en `-o / tmp / output.txt`.

for bucle
: Un bucle que se ejecuta una vez para cada valor en algún tipo de conjunto, lista o rango.
    Ver también: [while bucle](#while-bucle).

interfaz gráfica del usuario
: Una interfaz de usuario basada en la selección de elementos y acciones desde una pantalla gráfica,
    usualmente controlado usando un mouse.
    Ver también: [interfaz de línea de comandos](#command-line-interface).

directorio de inicio
: El directorio predeterminado asociado con una cuenta en un sistema informático.
    Por convención, todos los archivos de un usuario se almacenan en o debajo de su directorio de inicio.

lazo
: Un conjunto de instrucciones que se ejecutarán varias veces. Consiste en un [cuerpo de bucle](#bucle-cuerpo) y (por lo general) un
    condición para salir del bucle. Ver también [for bucle](# for-bucle) y [while bucle](#while-bucle).

cuerpo de bucle
: El conjunto de instrucciones o comandos que se repiten dentro de un [for bucle](# for-bucle)
    o [while bucle](# while-bucle).

Tipo MIME
: Los tipos MIME (extensiones multipropósito de correo de Internet) describen diferentes tipos de archivos para el intercambio en Internet,
    por ejemplo, imágenes, audio y documentos.

sistema operativo
: Software que gestiona las interacciones entre usuarios, hardware y software [procesos](#proceso). Común
    ejemplos son Linux, OS X y Windows.

ortogonal
: Tener significados o comportamientos que son independientes el uno del otro.
    Si un conjunto de conceptos o herramientas son ortogonales,
    se pueden combinar de cualquier manera.

parámetro
: Una variable nombrada en la declaración de una función que se usa para mantener un valor pasado a la llamada.
    El término a menudo se usa indistintamente (y de manera inconsistente) con [argumento](#argumento).

directorio de padres
: El directorio que "contiene" el que está en cuestión.
    Cada directorio en un sistema de archivos, excepto el [directorio raíz](#directorio-raíz), tiene un padre.
    Por lo general, se hace referencia al padre de un directorio usando la notación abreviada `..` (pronunciado "dot dot").

path
: Una descripción que especifica la ubicación de un archivo o directorio dentro de un [sistema de archivos](#file-system).
    Ver también: [path absoluta](#path-absoluta), [path relativa](# path-relativa).

tubo
: Una conexión desde la salida de un programa a la entrada de otro.
    Cuando dos o más programas están conectados de esta manera, se denominan "canalización".

proceso
: Una instancia en ejecución de un programa, que contiene código, valores de variables,
    abrir archivos y conexiones de red, y así sucesivamente.
    Los procesos son los "actores" que maneja el [sistema operativo](#sistema-operativo);
    generalmente ejecuta cada proceso durante unos pocos milisegundos a la vez
    para dar la impresión de que están ejecutándose simultáneamente.

prompt
: Un caractere o caracteres se muestran con [REPL](#read-evaluate-print-loop) para mostrar que
    está esperando su próximo comando.

citando
: (en el terminal):
    Utilizar comillas de varios tipos para evitar que el intérprete interprete caracteres especiales.
    Por ejemplo, para pasar la secuencia de caracteres `*.txt` a un programa,
    generalmente es necesario escribirlo como `'* .txt'` (con comillas simples)
    para que el terminal no intente expandir el comodín `*`.

read-evaluate-print loop
: (REPL): A [interfaz de línea de comandos](#command-line-interface) que lee un comando del usuario,
    lo ejecuta, imprime el resultado y espera otro comando.

redirigir
: Para enviar la salida de un comando a un archivo en lugar de a la pantalla u otro comando,
    o de manera equivalente, para leer la entrada de un comando desde un archivo.

expresión regular
: Un patrón que especifica un conjunto de secuencia de caracteres.
    Los RE se usan con mayor frecuencia para encontrar secuencias de caracteres.

path relativo
: A [path](#ruta) que especifica la ubicación de un archivo o directorio
    con respecto al [directorio de trabajo actual](#current-working-directory).
    Cualquier ruta que no comience con un carácter separador ("/" o "\\") es una ruta relativa.
    Ver también: [path absoluta](#path-absoluta).

directorio raíz
: El directorio más alto en un [sistema de archivos](# file-system).
    Su nombre es "/" en Unix (incluidos Linux y Mac OS X) y "\\" en Microsoft Windows.

terminal
: A [interfaz de línea de comando](#cli) como Bash (Bourne-Again Shell)
    o el terminal de Microsoft Windows DOS
    que permite a un usuario interactuar con el [sistema operativo](#sistema-operativo).

guión de terminal
: Un conjunto de comandos [terminal](#terminal) almacenados en un archivo para su reutilización.
    Un script de terminal es un programa ejecutado por el terminal;
    el nombre "script" se usa por razones históricas.


entrada estándar
: Flujo de entrada predeterminado de un proceso.
    En aplicaciones interactivas de línea de comandos,
    generalmente está conectado al teclado;
    en un [tubo](#tubería),
    recibe datos de [salida estándar](#estándar-salida) del proceso anterior.

salida estándar
: Flujo de salida predeterminado de un proceso.
    En aplicaciones interactivas de línea de comandos,
    los datos enviados a la salida estándar se muestran en la pantalla;
    en un [tubo](#tubería),
    se pasa a la [entrada estándar](#entrada-estándar) del siguiente proceso.

subdirectorio
: Un directorio contenido en otro directorio.

tabulación completa
: Una función proporcionada por muchos sistemas interactivos en los que
    presionar la tecla Tab activa la finalización automática de la palabra o comando actual.

variable
: Un nombre en un programa que está asociado con un valor o una colección de valores.

while bucle
: Un bucle que se ejecuta siempre que alguna condición sea verdadera.
    Ver también: [for bucle](#for-bucle).

comodín
: Un caracteres utilizado en la coincidencia de patrones.
    En el terminal de Unix,
    el comodín `*` coincide con cero o más caracteres,
    para que `* .txt` coincida con todos los archivos cuyos nombres terminen en` .txt`.