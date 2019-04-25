---
title: "Navegación de archivos y directorios"
teaching: 15
exercises: 0
questions:
- "¿Cómo puedo moverme dentro de mi computadora?"
- "¿Cómo puedo ver qué archivos y directorios tengo?"
- "¿Cómo puedo especificar la ubicación de un archivo o directorio en mi computadora?"
objectives:
   - "Explicar las similitudes y diferencias entre un archivo y un directorio."
   - "Convertir una ruta absoluta en una ruta relativa y viceversa."
   - "Construir rutas absolutas y relativas que identifican archivos y directorios específicos."
   - "Explicar los pasos del ciclo de lectura-ejecución-impresión de la terminal."
   - "Identificar el comando, opciones y nombres de archivo en una llamada de línea de comandos."
   - "Demostrar el uso del autocompletado con el tabulador y explicar sus ventajas."
keypoints:
- "El sistema de archivos es responsable de administrar la información en el disco."
- "La información se almacena en archivos, que a su vez se almacenan en directorios (carpetas)."
- "Los directorios también pueden almacenar otros directorios, formando un árbol de directorios."
- "`cd path` cambia el directorio de trabajo actual."
- "`ls path` imprime un listado de un archivo o directorio específico; `ls` por si solo lista el contenido del directorio de trabajo actual."
- "`pwd` imprime el directorio de trabajo actual del usuario."
- "`whoami` muestra la identidad actual del usuario."
- "`/` es el directorio raíz de todo el sistema de archivos."
- "Una ruta relativa especifica una ubicación desde la ubicación actual."
- "Una ruta absoluta especifica una ubicación desde la raíz del sistema de archivos."
- "Los nombres de directorio en una ruta están separados por '/' en Unix, pero por \\\ en Windows."
- "'..' significa 'el directorio por encima del actual'; '.' significa 'el directorio actual'."
- "La mayoría de los nombres de los archivos son `algo.extension`. La extensión no es necesaria y no garantiza nada, pero normalmente se utiliza para indicar el tipo de datos en el archivo."
- "La mayoría de los comandos toman opciones (**flags**) que comienzan con un '-'."
---

La parte del sistema operativo responsable de administrar archivos y directorios
se le denomina **sistema de archivos** (**file system**). Organiza nuestros
datos en archivos que contienen información, y directorios (también llamados
"carpetas"), que contienen archivos u otros directorios.

Varios comandos se utilizan con frecuencia para crear, inspeccionar, cambiar el
nombre y eliminar archivos y directorios. Para comenzar a explorarlos, abramos
una terminal:

> ## La magia de preparación
>
> Si escribes el comando: `PS1='$ '` en tu terminal, seguido de presionar la
> tecla 'Enter', tu ventana se verá como nuestro ejemplo en esta lección.
> Esto no es necesario para continuar así que lo dejamos a tu criterio.
{: .callout}

~~~
$
~~~
{: .language-bash}

El signo `$` es un **prompt**, que nos muestra que la terminal está esperando
una entrada; tu terminal puede usar un carácter diferente como prompt y puede
agregar información antes de él. Al teclear comandos, ya sea a partir de estas
lecciones o de otras fuentes, no escribas el prompt (*$*), sólo los comandos
que le siguen.

Escribe el comando `whoami`, luego presiona la tecla Enter para enviar el
comando a la terminal. La salida de este comando es el ID del usuario actual,
es decir, nos muestra como quién nos identifica la terminal:

~~~
$ whoami
~~~
{: .language-bash}

~~~
nelle
~~~
{: .output}

Más específicamente, cuando escribimos `whoami` la terminal:

1. encuentra un programa llamado `whoami`,
2. ejecuta ese programa,
3. muestra la salida de ese programa, luego
4. muestra un nuevo **prompt** para decirnos que está listo para más comandos.

> ## Variaciones en el username
>
> En esta lección, hemos utilizado el nombre de usuario `nelle` (asociado a
> nuestra científica hipotética Nelle) en todos los ejemplos de entrada y
> salida. Sin embargo, cuando escribas los comandos de esta lección en tu
> computadora, deberías ver y usar algo diferente, específicamente, el
> **username** asociado con tu cuenta de usuario en la computadora que estás
> utilizando. Este **username** será la salida de `whoami`. En los ejemplos
> siguientes, `nelle` siempre será reemplazado por ese **username**.
{: .callout}

> ## Comandos desconocidos
> Recuerda, la terminal es un programa que llama a otros programas en lugar
> de realizar los cálculos ella misma. Los comandos que escribes en la terminal
> deben ser los nombres de programas existentes. Si tecleas el nombre de un
> programa que no existe y oprimes Enter, verás un mensaje de error similar a
> este:
>
> ~~~
> $ mycommand
> ~~~
> {: .language-bash}
>
> ~~~
> -bash: mycommand: command not found
> ~~~
> {: .error}
>
> La terminal te dice que no puede encontrar el programa `mycommand` porque este
> programa no existe en tu computadora. En este curso aprenderás varios
> comandos, pero existen muchos más de los que mencionaremos.
{: .callout}

Averiguemos dónde estamos, ejecutando el comando `pwd` (que significa "imprime
directorio de trabajo" - "print working directory"). En cualquier momento,
nuestro **directorio actual** es nuestro directorio predeterminado, es decir,
el directorio que la computadora supone queremos ejecutar comandos a menos que
especifiquemos explícitamente otra cosa. En este caso la respuesta de la
computadora es `/Users/nelle`, el cual es el directorio de inicio de Nelle,
también conocido como su directorio **home**:

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle
~~~
{: .output}

> ## Variaciones en el Directorio de Inicio
> El directorio **home** puede lucir diferente en distintos sistemas operativos.
> En Linux puede verse como `/home/nelle`, en Windows puede ser similar a
> `C:\Documents and Settings\nelle` o `C:\Users\nelle` (pueden variar según la
> versión de Windows que estés utilizando). En los ejemplos que siguen
> utilizaremos la salida de Mac como estándar. Linux y Windows pueden variar
> ligeramente, pero lucen similares en general.
{: .callout}

Para entender lo que es un "directorio **home**" echemos un vistazo a cómo se
organiza el sistema de archivos. Como ejemplo, discutiremos el sistema de
archivos en la computadora de nuestra científica Nelle. Después de este ejemplo,
aprenderás comandos para explorar tu propio sistema de archivos. Se parecerá a
este, pero no será exactamente idéntico.

En la computadora de Nelle, el sistema de archivos se ve así:

![The File System](../fig/filesystem.svg)

En la parte superior está el **directorio raíz** o **root** que contiene todo
lo demás. Nos referimos a este directorio usando un caracter de barra `/` por
si solo; esta es la barra al inicio de `/Users/nelle`.

Dentro de ese directorio hay otros directorios:
`bin` (que es donde se almacenan algunos programas preinstalados),
`data` (para archivos de datos diversos),
`Users` (donde se encuentran los directorios personales de los usuarios),
`tmp` (para archivos temporales que no necesitan ser almacenados a largo plazo),
etcétera.

Sabemos que nuestro directorio actual de trabajo `/Users/nelle` se almacena
dentro de`/Users` porque `/Users` es la primera parte de su nombre. Igualmente,
sabemos que `/Users` se almacena dentro del directorio raíz` / ` porque su
nombre comienza con `/`.

> ## Diagonales
>
> Observa que hay dos significados para el carácter `/`. Cuando aparezca antes 
> del nombre de archivo o directorio, se refiere al directorio **root**. Cuando 
> aparezca *dentro de* un nombre, es solo un separador.
{: .callout}

Dentro de `/Users`, encontramos un directorio para cada usuario con una cuenta 
en la máquina de Nelle, sus colegas Mummy y Wolfman.

![Home Directories](../fig/home-directories.svg)

Los archivos de Mummy se almacenan en `/Users/imhotep`, los de Wolfman están en
`/Users/larry`, y los de Nelle en `/Users/nelle`. Dado que Nelle es el usuario 
en nuestros ejemplos, es por eso que recibimos `/Users/nelle` como nuestro 
directorio personal. Normalmente, cada vez que abres una nueva terminal te 
encontrarás en tu directorio **home**.

Ahora vamos a aprender el comando que nos permitirá ver el contenido de nuestro
sistema de archivos. Podemos ver lo que hay en nuestro directorio personal 
ejecutando `ls`, que significa "listar":

~~~
$ ls
~~~
{: .language-bash}

~~~
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
~~~
{: .output}

(Una vez más, tus resultados pueden ser ligeramente diferentes dependiendo de 
tu sistema operativo y cómo has personalizado tu sistema de archivos.)

`ls` imprime los nombres de los archivos y directorios en el directorio actual 
en orden alfabético, dispuestos ordenadamente en columnas. Podemos hacer su 
salida más comprensible usando la opción o **flag** `-F`, que le indica a `ls` 
que agregue un `/`a los nombres de los directorios:

~~~
$ ls -F
~~~
{: .language-bash}

~~~
Applications/ Documents/    Library/      Music/        Public/
Desktop/      Downloads/    Movies/       Pictures/
~~~
{: .output}

`ls` permite muchas otras **flags**. Para averiguar cuales son, podemos escribir:

~~~
$ ls --help
~~~
{: .language-bash}

~~~
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                         like -l, but do not list owner
      --group-directories-first
                             group directories before files;
                               can be augmented with a --sort option, but any
                               use of --sort=none (-U) disables grouping
  -G, --no-group             in a long listing, don't print group names
  -h, --human-readable       with -l and/or -s, print human readable sizes
                               (e.g., 1K 234M 2G)
      --si                   likewise, but use powers of 1000 not 1024
  -H, --dereference-command-line
                             follow symbolic links listed on the command line
      --dereference-command-line-symlink-to-dir
                             follow each command line symbolic link
                               that points to a directory
      --hide=PATTERN         do not list implied entries matching shell PATTERN
                               (overridden by -a or -A)
      --indicator-style=WORD  append indicator with style WORD to entry names:
                               none (default), slash (-p),
                               file-type (--file-type), classify (-F)
  -i, --inode                print the index number of each file
  -I, --ignore=PATTERN       do not list implied entries matching shell PATTERN
  -k, --kibibytes            default to 1024-byte blocks for disk usage
  -l                         use a long listing format
  -L, --dereference          when showing file information for a symbolic
                               link, show information for the file the link
                               references rather than for the link itself
  -m                         fill width with a comma separated list of entries
  -n, --numeric-uid-gid      like -l, but list numeric user and group IDs
  -N, --literal              print raw entry names (don't treat e.g. control
                               characters specially)
  -o                         like -l, but do not list group information
  -p, --indicator-style=slash
                             append / indicator to directories
  -q, --hide-control-chars   print ? instead of nongraphic characters
      --show-control-chars   show nongraphic characters as-is (the default,
                               unless program is 'ls' and output is a terminal)
  -Q, --quote-name           enclose entry names in double quotes
      --quoting-style=WORD   use quoting style WORD for entry names:
                               literal, locale, shell, shell-always,
                               shell-escape, shell-escape-always, c, escape
  -r, --reverse              reverse order while sorting
  -R, --recursive            list subdirectories recursively
  -s, --size                 print the allocated size of each file, in blocks
  -S                         sort by file size, largest first
      --sort=WORD            sort by WORD instead of name: none (-U), size (-S),
                               time (-t), version (-v), extension (-X)
      --time=WORD            with -l, show time as WORD instead of default
                               modification time: atime or access or use (-u);
                               ctime or status (-c); also use specified time
                               as sort key if --sort=time (newest first)
      --time-style=STYLE     with -l, show times using style STYLE:
                               full-iso, long-iso, iso, locale, or +FORMAT;
                               FORMAT is interpreted like in 'date'; if FORMAT
                               is FORMAT1<newline>FORMAT2, then FORMAT1 applies
                               to non-recent files and FORMAT2 to recent files;
                               if STYLE is prefixed with 'posix-', STYLE
                               takes effect only outside the POSIX locale
  -t                         sort by modification time, newest first
  -T, --tabsize=COLS         assume tab stops at each COLS instead of 8
  -u                         with -lt: sort by, and show, access time;
                               with -l: show access time and sort by name;
                               otherwise: sort by access time, newest first
  -U                         do not sort; list entries in directory order
  -v                         natural sort of (version) numbers within text
  -w, --width=COLS           set output width to COLS.  0 means no limit
  -x                         list entries by lines instead of by columns
  -X                         sort alphabetically by entry extension
  -Z, --context              print any security context of each file
  -1                         list one file per line.  Avoid '\n' with -q or -b
      --help     display this help and exit
      --version  output version information and exit

The SIZE argument is an integer and optional unit (example: 10K is 10*1024).
Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).

Using color to distinguish file types is disabled both by default and
with --color=never.  With --color=auto, ls emits color codes only when
standard output is connected to a terminal.  The LS_COLORS environment
variable can change the settings.  Use the dircolors command to set it.

Exit status:
 0  if OK,
 1  if minor problems (e.g., cannot access subdirectory),
 2  if serious trouble (e.g., cannot access command-line argument).

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
Full documentation at: <http://www.gnu.org/software/coreutils/ls>
or available locally via: info '(coreutils) ls invocation'
~~~
{: .output}

Muchos comandos bash y programas que la gente ha escrito que se pueden
ejecutar desde el bash, aceptan la opción `--help` para mostrar más
información sobre cómo usar los comandos o programas.

> ## Opciones de línea de comandos inexistentes
> Si intentas utilizar una **flag** que no existe, `ls` y otros programas 
> imprimirán un mensaje de error similar a este:
>
> ~~~
> $ ls -j
> ~~~
> {: .language-bash}
>
> ~~~
> ls: invalid option -- 'j'
> Try 'ls --help' for more information.
> ~~~
> {: .error}
{: .callout}

Para más información sobre cómo usar `ls` podemos escribir `man ls`. `man` es 
el comando "manual" de Unix: imprime la descripción de un comando y sus 
opciones, y (si tienes suerte) proporciona algunos ejemplos de cómo usarlo.

> ## `man` y Git para Windows
>
> La terminal proporcionada por Git para Windows no incluye soporte para el 
> comando `man`. Una búsqueda en la web de `unix man page COMMAND` (por 
> ejemplo, `unix man page grep`) proporciona enlaces a numerosas copias en 
> línea del manual de Unix. Por ejemplo, GNU proporciona enlaces a sus
> [Manuales](http://www.gnu.org/manual/manual.html), que incluyen 
> [grep](http://www.gnu.org/software/grep/manual/), y
> [utilidades básicas de GNU](http://www.gnu.org/software/coreutils/manual/coreutils.html),
> que cubre muchos comandos incluidos en esta lección.
{: .callout}

Para navegar por las páginas de `man`, las teclas de flecha arriba y abajo te 
permiten moverte línea por línea, las teclas "b" y la barra espaciadora permiten 
saltar hacia arriba y hacia abajo una página a la vez. Puedes salir de las 
páginas `man` escribiendo "q".

Aquí podemos ver que nuestro directorio **home** contiene principalmente 
**subdirectorios**. Cualquier nombre en la salida que no tenga barras se refiere 
a **archivos** comunes y corrientes. Observa que hay un espacio entre `ls` y 
`-F`: sin él, la terminal cree que estamos tratando de ejecutar un comando 
llamado `ls-F`, el cual no existe.

> ## Parámetros vs. Argumentos
>
> De acuerdo con [Wikipedia](https://en.wikipedia.org/wiki/Parameter_(computer_programming)#Parameters_and_arguments),
> los términos **argumento** y **parámetro** significan cosas ligeramente 
> diferentes. En la práctica, sin embargo, la mayoría de la gente los usa 
> indistintamente para referirse a los términos que acompañan a un comando. 
> Considera el siguiente ejemplo:
> ~~~
> $ ls -lh Documents
> ~~~
> {: .language-bash} 
>
> `ls` es el comando, `-lh` son **flags** (u opciones), y `Documents` 
> es el argumento.
{: .callout}

También podemos usar `ls` para ver el contenido de un directorio diferente. 
Veamos nuestro directorio `Desktop` ejecutando` ls -F Desktop`, es decir,
el comando `ls` con **flag** `-F` y el **argumento** `Desktop`. El argumento 
`Desktop` le dice a `ls` que queremos una lista de algo distinto a nuestro 
directorio actual.

~~~
$ ls -F Desktop
~~~
{: .language-bash}

~~~
data-shell/
~~~
{: .output}

La salida debe ser una lista de todos los archivos y subdirectorios en tu
**Desktop**, incluido el directorio `data-shell` que descargaste al comienzo de
la lección. Echa un vistazo a tu escritorio para confirmar que la salida es 
correcta.

Como puedes ver ahora, el uso de una terminal está basado de la idea de que tus
archivos se organizan en un sistema de archivos jerárquico. Organizar las cosas 
jerárquicamente nos ayuda a realizar un seguimiento de nuestro trabajo: es 
posible poner centenares de archivos en nuestro directorio **home**, así como es
posible acumular cientos de papeles impresos en nuestro escritorio, pero es una 
estrategia muy poco eficiente.

Ahora que sabemos que el directorio `data-shell` se encuentra en `Desktop`, 
podemos hacer dos cosas.

Primero, podemos ver su contenido, usando la misma estrategia que antes, 
pasando un nombre de directorio a `ls`:

~~~
$ ls -F Desktop/data-shell
~~~
{: .language-bash}

~~~
creatures/          molecules/          notes.txt           solar.pdf
data/               north-pacific-gyre/ pizza.cfg           writing/
~~~
{: .output}

En segundo lugar, podemos cambiar nuestra ubicación a un directorio diferente, 
por lo que ya no estaremos ubicados en nuestro directorio **home**.

El comando para cambiar de ubicación es `cd`, seguido del nombre de un 
directorio para cambiar nuestro directorio de trabajo. `cd` significa "cambio 
de directorio" (change directory), lo cual es un poco engañoso: el comando no 
cambia el directorio, cambia la idea de la terminal de en qué directorio estamos.

Digamos que queremos pasar al directorio `data` que vimos anteriormente. 
Podemos utilizar la siguiente serie de comandos para llegar allí:

~~~
$ cd Desktop
$ cd data-shell
$ cd data
~~~
{: .language-bash}

Estos comandos nos moverán del directorio **home** al directorio Desktop, luego
al directorio `data-shell`, y finalmente al directorio `data`. `cd` no imprime 
nada, pero si ejecutamos `pwd` después de esto, podemos ver que ahora estamos en
`/Users/nelle/Desktop/data-shell/data`. Si ejecutamos `ls` ahora sin argumentos,
se listan los contenidos de `/Users/nelle/Desktop/data-shell/data`, porque ahí 
es donde estamos ahora:

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/data-shell/data
~~~
{: .output}

~~~
$ ls -F
~~~
{: .language-bash}

~~~
amino-acids.txt   elements/     pdb/	          salmon.txt
animals.txt       morse.txt     planets.txt     sunspot.txt
~~~
{: .output}

Ahora sabemos cómo bajar por el árbol de directorios, pero ¿cómo subimos 
(regresamos)? Podemos probar lo siguiente:

~~~
cd data-shell
~~~
{: .language-bash}

~~~
-bash: cd: data-shell: No such file or directory
~~~
{: .error}

¡Pero tenemos un error! ¿Por qué pasa esto?

Con los métodos aprendidos hasta ahora, `cd` sólo puede ver subdirectorios 
dentro del directorio actual. Existen distintas maneras de ver los directorios 
que se encuentran encima de tu ubicación actual; empezaremos con el más simple.

Existe un acceso directo en la terminal para subir un nivel de directorio que 
se ve así:

~~~
$ cd ..
~~~
{: .language-bash}

`..` es un nombre de directorio especial que significa "el directorio que 
contiene a este", o más brevemente, el **padre** del directorio actual. Por 
supuesto, si ejecutamos `pwd` después de ejecutar` cd ..`, volvemos a 
`/Users/nelle/Desktop/data-shell`:

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/data-shell
~~~
{: .output}

Normalmente no aparece el directorio especial `..` cuando ejecutamos` ls`. Si 
queremos mostrarlo, podemos dar a `ls` la opción `-a`:

~~~
$ ls -F -a
~~~
{: .language-bash}

~~~
./                  creatures/          notes.txt
../                 data/               pizza.cfg
.bash_profile       molecules/          solar.pdf
Desktop/            north-pacific-gyre/ writing/
~~~
{: .output}

`-a` significa "mostrar todo"; obliga a `ls` a mostrarnos nombres de archivos y
directorios que comienzan con `.`, como `..` (que, si estamos en` /Users/nelle`,
hace referencia al directorio `/Users`) Como puedes ver, también muestra otro 
directorio especial que se llama simplemente `.`, que significa "el directorio 
de trabajo actual". Puede parecer redundante tener un nombre para él, pero 
veremos algunos usos para ello en el transcurso de este curso.

Nota que varias **flags** del mismo comando pueden combinarse en el mismo `-`, 
sin espacios entre los argumentos: `ls -F -a` es equivalente a `ls -Fa`.

> ## Otros archivos ocultos
>
> Además de los directorios ocultos `..` y `.`, también puedes ver un archivo
> llamado `.bash_profile`. Este archivo normalmente contiene configuraciones 
> de ajuste de la terminal. También puedes ver otros archivos y directorios que
> comienzan con `.`. Estos son generalmente archivos y directorios que se 
> utilizan para configurar diferentes programas en tu computadora. El prefijo 
> `.` se utiliza para evitar que archivos de configuración llenen la terminal 
> cuando un comando `ls` estándar se utiliza.
{: .callout}

> ## Ortogonalidad
>
> Los nombres especiales `.` y `..` no son exclusivos de `cd`; son interpretados
> de la misma manera por todos los programas. Por ejemplo, si estamos en 
> `/Users/nelle/data`, el comando `ls ..` nos dará una lista de`/Users/nelle`.
> Cuando los significados de las partes son los mismos, no importa cómo se 
> combinan, los programadores dicen que son **ortogonales**: los sistemas 
> ortogonales tienden a ser más fáciles de aprender porque hay menos casos 
> especiales y excepciones que recordar.
{: .callout}

Estos son entonces los comandos básicos para navegar por el sistema de archivos 
de tu computadora: `pwd`,` ls` y `cd`. Exploremos algunas variaciones de estos 
comandos. ¿Qué pasa si escribes `cd` por sí solo, sin dar un directorio?

~~~
$ cd
~~~
{: .language-bash}

¿Cómo puedes comprobar lo que sucedió? ¡`pwd` nos da la respuesta!

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle
~~~
{: .output}

Resulta que `cd` sin un argumento te devolverá a tu directorio **home**, lo 
cual es genial si te has perdido en tu propio sistema de archivos.

Vamos a intentar volver al directorio `data` que utilizamos antes. La última 
vez, usamos tres comandos, pero en realidad podemos enlazar la lista de 
directorios para llegar a `data` en un solo paso:

~~~
$ cd Desktop/data-shell/data
~~~
{: .language-bash}

Comprueba que nos hemos movido al lugar correcto ejecutando `pwd` y `ls -F`.

Si queremos subir un nivel desde el directorio de datos, podríamos usar 
`cd ..`. Pero hay otra manera de moverse a cualquier directorio, 
independientemente de tu ubicación actual.

Hasta ahora, al especificar nombres de directorio, o incluso una ruta de 
directorio (como anteriormente), hemos estado usando **caminos relativos**. 
Cuando utilizas una ruta relativa con un comando como `ls` o` cd`, la terminal 
intenta encontrar esa ubicación desde donde estamos, en lugar de la raíz del 
sistema de archivos.

Sin embargo, es posible especificar la **ruta absoluta** a un directorio 
incluyendo su ruta completa desde el directorio raíz, que está indicado por una
diagonal principal. Esta `/` principal le dice a la computadora que siga el 
camino desde la raíz del sistema de archivos, por lo que siempre se refiere a un
sólo directorio, sin importar donde estemos cuando ejecutamos el comando.

Esto nos permite pasar a nuestro directorio `data-shell` desde cualquier lugar
del sistema de directorios (incluyendo desde `data`). Para encontrar el camino 
absoluto que estamos buscando, podemos usar `pwd` y luego extraer la pieza que 
necesitamos para movernos a `data-shell`.

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/data-shell/data
~~~
{: .output}

~~~
$ cd /Users/nelle/Desktop/data-shell
~~~
{: .language-bash}

Ejecuta `pwd` y `ls -F` para asegurarte de que estás en el directorio que 
esperas.

> ## Dos atajos más
>
> La terminal interpreta el carácter `~` (tilde) al inicio de una ruta como 
> "el directorio inicial del usuario actual". Por ejemplo, si el **home** de 
> Nelle es `/Users/nelle`, entonces` ~/data` es equivalente a
> `/Users/nelle/data`. Esto sólo funciona si es el primer carácter en la ruta:
> `aqui/alla/~/otrolado` *no* es `aquí/alla/Users/nelle/otrolado`.
>
> Otro atajo es el carácter `-` (guión). `cd` lo interpreta como *el directorio 
> anterior en el que estaba*, lo cual es más rápido que tener que recordar,
> y luego escribir, la ruta completa. Esta es una manera *muy* eficiente de ir 
> y venir entre directorios. La diferencia entre `cd ..` y `cd -` es que el 
> primero te mueva hacia *adelante*, mientras que el último te *regresa*. Puedes
> pensar en ello como el botón *último canal* en el control remoto de tu 
> televisión.
{: .callout}

### Pipeline de Nelle: Organizando archivos

Sabiendo todo esto de archivos y directorios, Nelle está lista para organizar 
los archivos que creará la máquina de análisis de proteínas. Primero, Nelle crea 
un directorio llamado `north-pacific-gyre` (para recordar de dónde provienen los 
datos). Dentro de éste, crea un directorio llamado `2012-07-03`, que es la fecha 
en que comenzó a procesar las muestras. Solía utilizar nombres como 
`conference-paper` y `revised-results`, pero se tornaban difíciles de entender 
después de un par de años. (La gota que derramo el vaso fue cuando se encontró 
creando un directorio denominado `revised-revised-results-3`.)

> ## Ordenando la salida
>
> Nelle nombra sus directorios "año-mes-día", con ceros a la cabeza para meses 
> y días, porque la terminal muestra los nombres de archivos y directorios en 
> orden alfabético. Si usara nombres de mes, diciembre vendría antes de julio;
> si no utiliza ceros a la izquierda, Noviembre ('11') vendría antes de julio 
> ('7'). Del mismo modo, poner el año primero significa que junio de 2012 
> aparecerá antes de junio de 2013.
{: .callout}

Cada una de sus muestras físicas está etiquetada según la convención de su 
laboratorio con un identificador único de diez caracteres, tal como "NENE01729A".
Esto es lo que utilizó en su registro de la colección para registrar la 
ubicación, el tiempo, la profundidad y otras características de la muestra, por 
lo que decidió utilizarlo como parte del nombre de cada archivo de datos. Dado 
que la salida de la máquina de ensayo es texto sin formato, ella llamará a sus 
archivos `NENE01729A.txt`, `NENE01812A.txt`, y así sucesivamente. Todos los 
1,520 archivos estarán en el mismo directorio.

Ahora en su directorio actual `data-shell`, Nelle puede ver qué archivos tiene 
usando el comando:

~~~
$ ls north-pacific-gyre/2012-07-03/
~~~
{: .language-bash}

Esto es mucho que teclear, pero puede permitir que la terminal haga la mayor 
parte del trabajo a través de lo que se llama **autocompletado con el 
tabulador**. Si escribe:

~~~
$ ls nor
~~~
{: .language-bash}

y después presiona el tabulador (la tecla de tabulador en su teclado), la 
terminal completa automáticamente el nombre del directorio por ella:

~~~
$ ls north-pacific-gyre/
~~~
{: .language-bash}

Si presiona el tabulador otra vez, Bash añadirá `2012-07-03/` al comando, ya 
que es el único autocompletamiento posible. Presionar el tabulador de nuevo no 
hace nada, ya que hay 19 posibilidades; presionar el tabulador dos veces muestra 
una lista de todos los archivos, y así sucesivamente. Esto se denomina 
**autocompletado con el tabulador**, y lo veremos en muchas otras herramientas a
medida que avancemos.

> ## Rutas Absolutas vs Relativas
>
> A partir de `/Users/amanda/data/`, ¿Cuál de los siguientes comandos podría 
> Amanda usar para navegar a su directorio de inicio, que es `/Users/amanda`?
>
> 1. `cd .`
> 2. `cd /`
> 3. `cd /home/amanda`
> 4. `cd ../..`
> 5. `cd ~`
> 6. `cd home`
> 7. `cd ~/data/..`
> 8. `cd`
> 9. `cd ..`
>
> > ## Solución
> > 1. No: `.` significa el directorio actual.
> > 2. No: `/` significa el directorio raíz.
> > 3. No: El directorio **home** de Amanda es `/Users/amanda`.
> > 4. No: sube dos niveles, es decir termina en `/Users`.
> > 5. Sí:  `~` significa el directorio **home** del usuario, en este caso 
> >    `/Users/amanda`.
> > 6. No: esto navegaría a un directorio `home` en el directorio actual, si 
> >    existe.
> > 7. Sí: innecesariamente complicado, pero correcto.
> > 8. Sí: un atajo para volver al directorio **home** del usuario.
> > 9. Sí: sube un nivel.
> {: .solution}
{: .challenge}

> ## Resolución de ruta relativa
>
> Si se utiliza el diagrama de sistema de directorios de abajo, si `pwd` muestra
> `/Users/thing`, ¿Qué mostrará `ls -F ../backup`?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original pnas_final pnas_sub`
>
> ![Sistema de archivos para las preguntas del desafío](../fig/filesystem-challenge.svg)
>
> > ## Solución
> > 1. No: sí existe un directorio `backup` en `/Users`.
> > 2. No: este es el contenido de `Users/thing/backup`,
> >    pero con `..` pedimos un nivel más arriba.
> > 3. No: lee la explicación anterior.
> > 4. Sí: `../backup` se refiere a `/Users/backup`.
> {: .solution}
{: .challenge}

> ## `ls` comprensión de lectura
>
> Suponiendo una estructura de directorio como en la figura anterior, si `pwd` 
> muestra `/Users/backup`, y `-r` le dice a `ls` que muestre el resultado en 
> orden inverso, ¿qué comando mostrará:
>
> ~~~
> pnas_sub/ pnas_final/ original/
> ~~~
> {: .output}
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /Users/backup`
> 4.  #2 y #3, pero no #1.
>
> > ## Solution
> >  1. No: `pwd` no es el nombre de un directorio.
> >  2. Sí: `ls` sin argumento de directorio lista archivos y directorios en el
> >     directorio actual.
> >  3. Sí: utiliza explícitamente el camino absoluto.
> >  4. Correcto: vea las explicaciones arriba.
> {: .solution}
{: .challenge}

> ## Explorando más argumentos de `ls`
>
> ¿Qué hace el comando `ls` cuando se utiliza con los argumentos` -l` y `-h`?
>
> Algunos de sus resultados son sobre propiedades que no cubrimos en esta 
> lección (tales como permisos de archivo y propiedad), sin embargo, el resto 
> debe ser útil.
>
> > ## Solución
> > El argumento `-l` hace que `ls` utilice un formato de lista **l**argo, 
> > mostrando no sólo los nombres de archivo/directorio, sino también información 
> > adicional como el tamaño del archivo y la hora de su última modificación. El 
> > argumento `-h` hace que el tamaño del archivo sea "**h**uman readable" 
> > (legible por humanos), es decir, muestra algo como `5.3K` en lugar de` 5369`.
> {: .solution}
{: .challenge}

> ## Listando recursivamente y por tiempo
>
> El comando `ls -R` enumera el contenido de los directorios recursivamente, es 
> decir, lista sus subdirectorios, subsubdirectorios, etc. en orden alfabético 
> en cada nivel. El comando `ls -t` ordena el resultado según la fecha del 
> último cambio, los archivos o directorios más recientemente modificados 
> aparecen primero. ¿En qué orden muestra los resultados el usar `ls -R -t`? 
> Pista: `ls -l` usa un formato de lista larga para ver los **timestamps**.
>
> > ## Solución
> > Los directorios se enlistan alfabéticamente en cada nivel, los 
> > archivos/directorios en cada directorio se ordenan por la hora del último 
> > cambio.
> {: .solution}
{: .challenge}


{% include links.md %}
