---
title: 'FIXME'
---

## Glossary

## Resumen de Comandos Básicos

| Acción        | Archivos | Directorios o Carpetas | 
| ------------- | -------- | ---------------------- |
| Inspeccionar  | ls       | ls                     | 
| Ver contenido | cat      | ls                     | 
| Navega hacia  |          | cd                     | 
| Mover         | mv       | mv                     | 
| Copiar        | cp       | cp -r                  | 
| Crear         | nano     | mkdir                  | 
| Eliminar      | rm       | rmdir, rm -r           | 

## Jerarquía del sistema de archivos

La siguiente es una descripción general de un sistema de archivos Unix estándar.
La jerarquía exacta depende de la plataforma,
por lo que es posible que no veas exactamente los mismos archivos / directorios en tu computadora:

![](fig/standard-filesystem-hierarchy.svg){alt='Jerarquía del sistema de archivos de Linux'}

## Glosario

{: auto\_ids}

argumento
: Un valor dado a una función o programa cuando se ejecuta. El término a menudo se usa indistintamente (y de manera inconsistente) con [parámetro](#parmetro).

bandera o **flag**
: Una forma concisa de especificar una opción o configuración a un programa de línea de comandos. Por convención, las aplicaciones Unix usan un guión seguido de una sola letra, como `-v`, o dos guiones seguidos de una palabra, como
`--verbose`, mientras que las aplicaciones de DOS usan una barra inclinada, como `/ V`. Dependiendo de la aplicación, un indicador puede ir seguido de un único argumento, como en `-o / tmp / output.txt`.

comillas
: (en la terminal):
Se utilizan comillas de varios tipos para evitar que el intérprete interprete caracteres especiales. Por ejemplo, para pasar la secuencia de caracteres `*.txt` a un programa, generalmente es necesario escribirlo como `'* .txt'` (con comillas simples) para que la terminal no intente expandir el comodín `*`.

comentario
: Un comentario en un programa pretende ayudar a los lectores a entender lo que está sucediendo, pero es ignorado por la computadora. Los comentarios en Python, R y la terminal de Unix comienzan con un caracter `#` y se ejecutan hasta el final de la línea; los comentarios en SQL comienzan con `--`, y otros idiomas tienen otras convenciones.

comodín o caracter especial
: Un caracter utilizado para coincidir con patrones. En la terminal de Unix, el comodín `*` coincide con cero o más caracteres, para que `*.txt` coincida con todos los archivos cuyos nombres terminen en `.txt`.

cuerpo de bucle
: El conjunto de instrucciones o comandos que se repiten dentro de un [for bucle](#for-bucle) o [while bucle](#while-bucle).

directorio de inicio
: El directorio predeterminado asociado con una cuenta en un sistema informático. Por convención, todos los archivos de un usuario se almacenan en o debajo de su directorio de inicio.

directorio de padres
: El directorio que "contiene" el que está en cuestión. Cada directorio en un sistema de archivos, excepto el [directorio raíz](#directorio-raz), tiene un padre. Por lo general, se hace referencia al padre de un directorio usando la notación abreviada `..` (pronunciado "dot dot").

directorio de trabajo actual
: El directorio del que se calculan [paths relativos](#path-relativo); equivalentemente, el lugar donde se buscan los archivos a los que se hace referencia solo por nombre. Cada [proceso](#proceso) tiene un directorio de trabajo actual. El directorio de trabajo actual generalmente se refiere a la notación abreviada `.` (pronunciado "punto").

directorio raíz
: El directorio más alto en un [sistema de archivos](#sistema-de-archivos). Su nombre es "/" en Unix (incluidos Linux y Mac OS X) y "\\" en Microsoft Windows.

entrada estándar
: Flujo de entrada predeterminado de un proceso. En aplicaciones interactivas de línea de comandos, generalmente está conectado al teclado; en un [pipe](#pipe), recibe datos de [salida estándar](#salida-estndar) del proceso anterior.

expresión regular
: Un patrón que especifica un conjunto de secuencia de caracteres. Los RE se usan con mayor frecuencia para encontrar secuencias de caracteres.

extensión de archivo
: La parte del nombre de un archivo que aparece después del "." final. Por convención, esto identifica el tipo de archivo:
    `.txt` significa "archivo de texto", `.png` significa "archivo de red portátil de gráficos", y así. Estas convenciones no son aplicadas por la mayoría de los sistemas operativos:
    es perfectamente posible (¡pero confuso!) nombrar un archivo de sonido MP3 `homepage.html`. Dado que muchas aplicaciones usan extensiones de nombre de archivo para identificar el [tipo MIME](#tipo-mime) del archivo, los archivos de desincronización pueden hacer que esas aplicaciones fallen.

filtrar
: Un programa que transforma una secuencia de datos. Muchas herramientas de línea de comandos de Unix están escritas como filtros: leen datos de [entrada estándar](#entrada-estndar),
    procesarlo y escribir el resultado en [salida estándar](#salida-estndar).

for bucle
: Un bucle que se ejecuta una vez para cada valor en algún tipo de conjunto, lista o rango. Ver también: [while bucle](#while-bucle).

guión de terminal
: Un conjunto de comandos [terminal](#terminal) almacenados en un archivo para su reutilización. Un script de terminal es un programa ejecutado por la terminal; el nombre "script" se usa por razones históricas.

interfaz de línea de comando
: Una interfaz de usuario basada en comandos de tipeo, generalmente en un [REPL](#read-evaluate-print-loop). Ver también: [interfaz gráfica de usuario](#interfaz-grfica-del-usuario).

interfaz gráfica del usuario
: Una interfaz de usuario basada en la selección de elementos y acciones desde una pantalla gráfica, usualmente controlado usando un mouse. Ver también: [interfaz de línea de comandos](#interfaz-de-lnea-de-comando).

lazo
: Un conjunto de instrucciones que se ejecutarán varias veces. Consiste en un [cuerpo de bucle](#cuerpo-de-bucle) y (por lo general) un condición para salir del bucle. Ver también [for bucle](#for-bucle) y [while bucle](#while-bucle).

ortogonal
: Tener significados o comportamientos que son independientes el uno del otro. Si un conjunto de conceptos o herramientas son ortogonales, se pueden combinar de cualquier manera.

parámetro
: Una variable nombrada en la declaración de una función que se usa para mantener un valor pasado a la llamada. El término a menudo se usa indistintamente (y de manera inconsistente) con [argumento](#argumento).

path
: Una descripción que especifica la ubicación de un archivo o directorio dentro de un [sistema de archivos](#sistema-de-archivos). Ver también: [path absoluto](#path-absoluto), [path relativo](#path-relativo).

path absoluto
: Un [path](#path) que hace referencia a una ubicación particular en un sistema de archivos. Los paths absolutos generalmente se escriben con respecto al sistema de archivos [directorio raíz](#directorio-raz), y comienzan con "/" (en Unix) o "\\" (en Microsoft Windows). Ver también: [path relativo](#path-relativo).

path relativo
: Un [path](#path) que especifica la ubicación de un archivo o directorio con respecto al [directorio de trabajo actual](#directorio-de-trabajo-actual). Cualquier ruta que no comience con un caracter separador ("/" o "\\") es una ruta relativa. Ver también: [path absoluto](#path-absoluto).

pipe
: Una conexión desde la salida de un programa a la entrada de otro. Cuando dos o más programas están conectados de esta manera, se denominan "canalización" o **piping**.

proceso
: Una instancia en ejecución de un programa, que contiene código, valores de variables, archivos abiertos y conexiones de red, y así sucesivamente. Los procesos son los "actores" que maneja el [sistema operativo](#sistema-operativo); generalmente ejecuta cada proceso durante unos pocos milisegundos a la vez para dar la impresión de que están ejecutándose simultáneamente.

prompt
: Un caracter o caracteres se muestran con [REPL](#read-evaluate-print-loop) para mostrar que está esperando su próximo comando.

read-evaluate-print-loop
: (REPL): Una [interfaz de línea de comandos](#interfaz-de-lnea-de-comando) que lee un comando del usuario, lo ejecuta, imprime el resultado y espera otro comando.

redirigir
: Para enviar la salida de un comando a un archivo en lugar de a la pantalla u otro comando, o de manera equivalente, para leer la entrada de un comando desde un archivo.

sistema de archivos
: Un conjunto de archivos, directorios y dispositivos de entrada y salida (E/S) (como teclados y pantallas). Un sistema de archivos puede extenderse a través de muchos dispositivos físicos, o muchos sistemas de archivos pueden almacenarse en un solo dispositivo físico; el [sistema operativo](#sistema-operativo) administra el acceso.

salida estándar
: Flujo de salida predeterminado de un proceso. En aplicaciones interactivas de línea de comandos, los datos enviados a la salida estándar se muestran en la pantalla; con un [pipe](#pipe), se pasa a la [entrada estándar](#entrada-estndar) del siguiente proceso.

sistema operativo
: Software que gestiona las interacciones entre usuarios y los [procesos](#proceso) de hardware y software. Por ejemplo, Linux, OS X y Windows.

subdirectorio
: Un directorio contenido en otro directorio.

tabulación completa
: Una función proporcionada por muchos sistemas interactivos en los que presionar la tecla Tab activa la finalización automática de la palabra o comando actual.

terminal
: Una [interfaz de línea de comando] como Bash (Bourne-Again Shell) o la terminal de Microsoft Windows DOS que permite a un usuario interactuar con el [sistema operativo](#sistema-operativo).

terminal de comandos o terminal de shell
: Ver [terminal](#terminal)

tipo MIME
: Los tipos MIME (extensiones multipropósito de correo de Internet) describen diferentes tipos de archivos para el intercambio en Internet, por ejemplo, imágenes, audio y documentos.

variable
: Un nombre en un programa que está asociado con un valor o una colección de valores.

while bucle
: Un bucle que se ejecuta siempre que alguna condición sea verdadera. Ver también: [for bucle](#for-bucle).


