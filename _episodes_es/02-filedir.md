---
title: "Navegación de archivos y directorios"
teaching: 15
exercises: 0
questions:
- "¿Cómo puedo moverme en mi computadora?"
- "¿Cómo puedo ver qué archivos y directorios tengo?"
- "¿Cómo puedo especificar la ubicación de un archivo o directorio en mi computadora?"
objectives:
- "Explicar las similitudes y diferencias entre un archivo y un directorio."
- "Traducir una ruta absoluta una ruta relativa y viceversa."
- "Construir rutas absolutas y relativas que identifican archivos y directorios específicos."
- "Explicar los pasos del ciclo de lectura-ejecución-impresión del shell."
- "Identificar el comando real, opciones y nombres de archivo en una llamada de línea de comandos."
- "Demostrar el uso de la autocompletado con el tabulador, y explicar sus ventajas."
keypoints:
- "El sistema de archivos es responsable de administrar la información en el disco."
- "La información se almacena en archivos, que se almacenan en directorios (carpetas)."
- "Los directorios también pueden almacenar otros directorios, formando un árbol de directorios."
- "`cd path` cambia el directorio de trabajo actual."
- "`ls path` imprime un listado de un archivo o directorio específico; `ls` por si solo lista el directorio de trabajo actual."
- "`pwd` imprime el directorio de trabajo actual del usuario."
- "`whoami` muestra la identidad actual del usuario."
- "`/` es el directorio raíz de todo el sistema de archivos."
- "Una ruta relativa especifica una ubicación desde la ubicación actual."
- "Una ruta absoluta especifica una ubicación desde la raíz del sistema de archivos."
- "Los nombres de directorio en una ruta están separados por '/' en Unix, pero por '\\\' en Windows."
- "'..' significa 'el directorio por encima del actual'; '.' por si solo significa 'el directorio actual'."
- "La mayoría de los nombres de los archivos son `algo.extension`. La extensión no es necesaria y no garantiza nada, pero normalmente se utiliza para indicar el tipo de datos en el archivo."
- "La mayoría de los comandos toman opciones (flags) que comienzan con un '-'."
---

La parte del sistema operativo responsable de administrar archivos y directorios
Se denomina **sistema de archivos** (**file system**).
Organiza nuestros datos en archivos,
que contienen información,
y directorios (también llamados "carpetas"),
que contienen archivos u otros directorios.

Varios comandos se utilizan con frecuencia para crear, inspeccionar, cambiar el nombre y eliminar archivos y directorios.
Para comenzar a explorarlos,
vamos a abrir una ventana de shell:

> ## La magia de preparación
>
> Si escribes el comando:
> `PS1 = '$'`
> En tu shell, seguido de presionar la tecla 'enter',
> tu ventana debe verse como nuestro ejemplo en esta lección.
> Esto no es necesario para continuar así que lo dejamos a tu criterio.
{: .callout}

~~~
$
~~~
{: .bash}

El signo de dólar es un **indicador** (**prompt**), que nos muestra que el shell está esperando 
la entrada;
tu shell puede usar un carácter diferente como indicador y puede agregar información antes
del indicador. Al escribir comandos, ya sea a partir de estas lecciones o de otras fuentes,
no escriba el indicador (*$*), sólo los comandos después del mismo.

Escribe el comando `whoami`,
luego presiona la tecla Enter para enviar el comando al shell.
La salida del comando es la ID del usuario actual,
es decir,
nos muestra quién somos de acuerdo al shell:

~~~
$ whoami
~~~
{: .bash}

~~~
Alicia
~~~
{: .output}

Más específicamente, cuando escribimos `whoami` el shell:

1. encuentra un programa llamado `whoami`,
2. ejecuta ese programa,
3. muestra la salida de ese programa, luego
4. muestra un nuevo mensaje para decirnos que está listo para más comandos.

> ## Variaciones en el nombre de usuario
>
> En esta lección, hemos utilizado el nombre de usuario `Alicia` (asociado
> con nuestro científico hipotético Alicia) en el ejemplo de entrada y salida en todo.
> Sin embargo, cuando
> escribas los comandos de esta lección en tu computadora,
> deberías ver y usar algo diferente,
> específicamente, el nombre de usuario asociado con la cuenta de usuario en su computadora. Este
> username será la salida de `whoami`. En
> los ejemplos siguientes, `Alicia` siempre será reemplazado por ese nombre de usuario.
{: .callout}

Averiguemos dónde estamos ejecutando el comando `pwd`
(que significa "imprime directorio de trabajo" - "print working directory").
En cualquier momento,
nuestro **directorio actual**
es nuestro directorio predeterminado actual,
es decir,
el directorio en el que la computadora asume que queremos ejecutar comandos
a menos que especifiquemos explícitamente otra cosa.
En este caso
la respuesta de la computadora es `/home/Alicia`,
el cuál es el directorio de inicio de Alicia, también conocido como su **home**:

~~~
$ pwd
~~~
{: .bash}

~~~
/home/Alicia
~~~
{: .output}

Para entender lo que es un "directorio de inicio"
echemos un vistazo a cómo se organiza el sistema de archivos como un todo. 
Como ejemplo, discutiremos 
el sistema de archivos en la computadora de nuestro científica Alicia. Después de este
ejemplo, aprenderás comandos para explorar tu propio sistema de archivos,
que se parecerá a este, pero no será exactamente idéntico.

En el ordenador de Alicia, el sistema de archivos se ve así:

![The File System](../fig/filesystem.svg)

En la parte superior está el **directorio raíz** o **root**
que tiene todo lo demás.
Nos referimos a este directorio usando en caracter de barra `/` por si solo;
ésta es la barra principal en `/home/Alicia`.

Dentro de ese directorio hay varios otros directorios:
`bin` (que es donde se almacenan algunos programas integrados),
`data` (para archivos de datos diversos),
`home` (donde se encuentran los directorios personales de los usuarios),
`tmp` (para archivos temporales que no necesitan ser almacenados a largo plazo),
etcétera.

Sabemos que nuestro actual directorio de trabajo `/home/Alicia` se almacena dentro de`/home`
porque `/home` es la primera parte de su nombre.
Igualmente,
sabemos que `/home` se almacena dentro del directorio raíz` / `
porque su nombre comienza con `/`.

> ## Diagonales
>
> Observe que hay dos significados para el carácter `/`.
> Cuando aparezca antes del nombre de archivo o directorio,
> se refiere al directorio raíz. Cuando aparezca *dentro de* un nombre,
> es sólo un separador.
{: .callout}

Dentro de `/home`,
encontramos un directorio para cada usuario con una cuenta en la máquina de Alicia,
sus colegas la momia y el hombre lobo.

![Home Directories](../fig/home-directories.svg)

Los archivos de la momia se almacenan en `/home/imhotep`,
los del hombre lobo están en `/home/larry`,
y los de Alicia's en `/home/Alicia`. Dado que Alicia es el usuario en nuestros
ejemplos, es por eso que recibimos `/home/Alicia` como nuestro directorio personal.
Normalmente, cada vea que se abre una nuevo terminal, estará en
en directorio de inicio por default.

Ahora vamos a aprender el comando que nos permitirá ver el contenido de nuestro
sistema de archivos. Podemos ver lo que hay en nuestro directorio personal ejecutando `ls`,
que significa "listado":

~~~
$ ls
~~~
{: .bash}

~~~
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
~~~
{: .output}

(Una vez más, sus resultados pueden ser ligeramente diferentes dependiendo de su funcionamiento
y cómo ha personalizado su sistema de archivos.)

`ls` imprime los nombres de los archivos y directorios en el directorio actual en
orden alfabético,
dispuestos cuidadosamente en columnas.
Podemos hacer su salida más comprensible usando la opción o **flag** `-F`,
que le indica a `ls` que agregue un `/`a los nombres de los directorios:

~~~
$ ls -F
~~~
{: .bash}

~~~
Applications/ Documents/    Library/      Music/        Public/
Desktop/      Downloads/    Movies/       Pictures/
~~~
{: .output}

`ls` tiene muchas otras opciones. Para averiguar cuales son, podemos escribir:

~~~
$ ls --help
~~~
{: .bash}

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

Muchos comandos bash, y programas que la gente ha escrito que se pueden 
ejecutar desde dentro de bash, aceptan la opción `--help` para mostrar más
información sobre cómo usar los comandos o programas.

Para más información sobre cómo usar `ls` podemos escribir `man ls`.
`man` es el comando "manual" de Unix:
Imprime una descripción de un comando y sus opciones,
y (si tienes suerte) proporciona algunos ejemplos de cómo usarlo.

> ## `man` y Git para Windows
>
> La shell de bash proporcionada por Git para Windows no
> incluye soporte para el comando `man`. Una búsqueda en la web de
> `unix man page COMMAND` (por ejemplo, `unix man page grep`)
> proporciona enlaces a numerosas copias del manual de Unix
> páginas en línea.
> Por ejemplo, GNU proporciona enlaces a su
> [Manuales](http://www.gnu.org/manual/manual.html):
> Estos incluyen [grep](http://www.gnu.org/software/grep/manual/),
> y el de
> [utilidades básicas de GNU](http://www.gnu.org/software/coreutils/manual/coreutils.html),
> que cubre muchos comandos incluidos en esta lección.
{: .callout}

Para navegar por las páginas de `man`,
puede usar las teclas de flecha arriba y abajo para moverse línea por línea,
o pruebe las teclas "b" y la barra espaciadora para saltar hacia arriba y hacia abajo por página completa.
Salga de las páginas `man` escribiendo "q".

Aquí podemos ver que nuestro directorio home contiene principalmente **sub-directorios**.
Cualquier nombre en la salida que no tenga barras,
son **archivos** comunes y corrientes.
Observe que hay un espacio entre `ls` y `-F`:
sin este,
el shell cree que estamos tratando de ejecutar un comando llamado `ls-F`,
el cual no existe.

> ## Parámetros vs. Argumentos
>
> De acuerdo con [Wikipedia](https://en.wikipedia.org/wiki/Parameter_(computer_programming)#Parameters_and_arguments),
> Los términos **argumento** y **parámetro**
> significa cosas ligeramente diferentes.
> En la práctica,
> sin embargo,
> la mayoría de la gente los usa indistintamente,
> así que nosotros haremos lo mismo.
{: .callout}

También podemos usar `ls` para ver el contenido de un directorio diferente. Veamos
nuestro directorio `Desktop` ejecutando` ls -F Desktop`,
es decir,
el comando `ls` con los **argumentos**` -F` y `Desktop`.
El segundo argumento --- el que *no* tiene un guión al inicio --- dice a `ls` que
queremos un listado de algo que no es nuestro directorio de trabajo actual:

~~~
$ ls -F Desktop
~~~
{: .bash}

~~~
data-shell/
~~~
{: .output}

Su salida debe ser una lista de todos los archivos y subdirectorios de su
Desktop, incluido el directorio `data-shell` que descargó
al comienzo de la lección. Eche un vistazo a su escritorio para confirmar que
su salida es correcta.

Como puede ver ahora, el uso de un shell bash depende fuertemente de la idea de que
sus archivos se organizan en un sistema de archivos jerárquico.
Organizar las cosas jerárquicamente de esta manera nos ayuda a realizar un seguimiento de nuestro trabajo:
es posible poner centenares de archivos en nuestro directorio casero,
así como es posible acumular cientos de papeles impresos en nuestro escritorio,
pero es una estrategia autodestructiva.

Ahora que sabemos que el directorio `data-shell` se encuentra en nuestro escritorio,
podemos hacer dos cosas.

Primero, podemos ver su contenido, usando la misma estrategia que antes, pasando
un nombre de directorio a `ls`:

~~~
$ ls -F Desktop/data-shell
~~~
{: .bash}

~~~
creatures/          molecules/          notes.txt           solar.pdf
data/               north-pacific-gyre/ pizza.cfg           writing/
~~~
{: .output}

En segundo lugar, podemos cambiar nuestra ubicación a un directorio diferente, por lo que
ya no estaremos ubicados en
nuestro directorio personal.

El comando para cambiar las ubicaciones es `cd` seguido de un
nombre de directorio para cambiar nuestro directorio de trabajo.
`cd` significa "cambio de directorio" (change directory),
lo cual es un poco engañoso:
el comando no cambia el directorio,
cambia la idea del shell de en qué directorio estamos.

Digamos que queremos pasar al directorio `data` que vimos anteriormente. Podemos
utilizar la siguiente serie de comandos para llegar allí:

~~~
$ cd Desktop
$ cd data-shell
$ cd data
~~~
{: .bash}

Estos comandos nos moverán de nuestro directorio de inicio a nuestro Desktop, luego
al directorio `data-shell`, y luego en el directorio `data`. `cd` no imprime nada,
pero si ejecutamos `pwd` después de esto, podemos ver que ahora estamos
en `/home/Alicia/Desktop/data-shell/data`.
Si ejecutamos `ls` sin argumentos ahora,
lista los contenidos de `/home/Alicia/Desktop/data-shell/data`,
Porque ahí es donde estamos ahora:

~~~
$ pwd
~~~
{: .bash}

~~~
/home/Alicia/Desktop/data-shell/data
~~~
{: .output}

~~~
$ ls -F
~~~
{: .bash}

~~~
amino-acids.txt   elements/     pdb/	        salmon.txt
animals.txt       morse.txt     planets.txt     sunspot.txt
~~~
{: .output}

Ahora sabemos cómo bajar por el árbol de directorios, pero
¿cómo subimos (regresamos)? Podemos probar lo siguiente:

~~~
cd data-shell
~~~
{: .bash}

~~~
-bash: cd: data-shell: No such file or directory
~~~
{: .error}

¡Pero tenemos un error! ¿Por qué es esto?

Con nuestros métodos hasta ahora,
`cd` sólo puede ver subdirectorios dentro de su directorio actual. Existen
diferentes maneras de ver los directorios por encima de su ubicación actual; empezaremos
con el más simple.

Hay un acceso directo en el shell para subir un nivel de directorio
que se parece a esto:

~~~
$ cd ..
~~~
{: .bash}

`..` es el nombre de directorio especial que significa
"el directorio que contiene este",
O más brevemente,
el **padre** del directorio actual.
Por supuesto,
si ejecutamos `pwd` después de ejecutar` cd ..`, volvemos a `/home/Alicia/Desktop/data-shell`:

~~~
$ pwd
~~~
{: .bash}

~~~
/home/Alicia/Desktop/data-shell
~~~
{: .output}

Normalmente no aparece el directorio especial `..` cuando ejecutamos` ls`. Si queremos
mostrarlo, podemos dar a `ls` la opción `-a`:

~~~
$ ls -F -a
~~~
{: .bash}

~~~
./                  creatures/          notes.txt
../                 data/               pizza.cfg
.bash_profile       molecules/          solar.pdf
Desktop/            north-pacific-gyre/ writing/
~~~
{: .output}

`-a` significa "mostrar todo";
Obliga a `ls` a mostrarnos nombres de archivos y directorios que comienzan con `.`,
como `..` (que, si estamos en` /home/Alicia`, hace referencia al directorio `/home`)
Como puedes ver,
también muestra otro directorio especial que se llama simplemente `.`,
que significa "el directorio de trabajo actual".
Puede parecer redundante tener un nombre para él,
pero veremos algunos usos para ello en el transcurso de este curso.

> ## Otros archivos ocultos
>
> Además de los directorios ocultos `..` y `.`, también puede ver un archivo
> llamado `.bash_profile`. Este archivo normalmente contiene configuraciones de ajuste del shell.
> También puede ver otros archivos y directorios que comienzan
> con `.`. Estos son generalmente archivos y directorios que se utilizan para configurar
> programas diferentes en su computadora. El prefijo `.` se utiliza para evitar que
> archivos de configuración de llenar la terminal cuando un comando `ls` estándar
> se utiliza.
{: .callout}

> ## Ortogonalidad
>
> Los nombres especiales `.` y `..` no son exclusivos de `cd`;
> son interpretados de la misma manera por cada programa.
> Por ejemplo,
> si estamos en `/home/Alicia/data`,
> el comando `ls ..` nos dará una lista de`/home/Alicia`.
> Cuando los significados de las partes son los mismos no importa cómo se combinan,
> los programadores dicen que son **ortogonales**:
> los sistemas ortogonales tienden a ser más fáciles de aprender
> porque hay menos casos especiales y excepciones a seguir.
{: .callout}

Estos son entonces los comandos básicos para navegar por el sistema de archivos en su computadora:
`pwd`,` ls` y `cd`. Vamos a explorar algunas variaciones en esos comandos. ¿Qué pasa
si escribe `cd` por sí solo, sin dar
un directorio?

~~~
$ cd
~~~
{: .bash}

¿Cómo puede comprobar lo que sucedió? ¡`pwd` nos da la respuesta!

~~~
$ pwd
~~~
{: .bash}

~~~
/home/Alicia
~~~
{: .output}

Resulta que `cd` sin un argumento te devolverá a tu directorio de inicio,
lo cual es genial si te has perdido en tu propio sistema de archivos.

Vamos a intentar volver al directorio `data` desde antes. La última vez, usamos
tres comandos, pero en realidad podemos enlazar la lista de directorios
para llegar a `data` en un solo paso:

~~~
$ cd Desktop/data-shell/data
~~~
{: .bash}

Compruebe que nos hemos movido al lugar correcto ejecutando `pwd` y `ls -F`.

Si queremos subir un nivel desde el directorio de datos, podríamos usar `cd ..`. Pero
hay otra manera de moverse a cualquier directorio, independientemente de su
ubicación actual.

Hasta ahora, al especificar nombres de directorio, o incluso una ruta de directorio (como anteriormente),
hemos estado usando **caminos relativos**. Cuando utiliza una ruta relativa con un comando
como `ls` o` cd`, intenta encontrar esa ubicación desde donde estamos,
en lugar de la raíz del sistema de archivos.

Sin embargo, es posible especificar la **ruta absoluta** a un directorio 
incluyendo su ruta completa desde el directorio raíz, que está indicado por una
diagonal principal. Esta `/` principal le dice a la computadora que siga el camino desde
la raíz del sistema de archivos, por lo que siempre se refiere a exactamente un directorio,
sin importar donde estamos cuando ejecutamos el comando.

Esto nos permite pasar a nuestro directorio `data-shell` desde cualquier lugar
del sistema de directorios (incluyendo desde `data`). Para encontrar el camino absoluto
que estamos buscando, podemos usar `pwd` y luego extraer la pieza que necesitamos
para moverse a `data-shell`.

~~~
$ pwd
~~~
{: .bash}

~~~
/home/Alicia/Desktop/data-shell/data
~~~
{: .output}

~~~
$ cd /home/Alicia/Desktop/data-shell
~~~
{: .bash}

Ejecute `pwd` y `ls -F` para asegurarse de que estamos en el directorio que deseamos.

> ## Dos más accesos directos
>
> La shell interpreta el carácter `~` (tilde) al inicio de una ruta a
> como "el directorio inicial del usuario actual". Por ejemplo, si el hogar de Alicia
> es `/home/Alicia`, entonces` ~/data` es equivalente a
> `/home/Alicia/data`. Esto sólo funciona si es el primer carácter en la
> ruta: `aqui/alla/~/otrolado` *no* es `aquí/alla/home/Alicia/otrolado`.
>
> Otro atajo es el carácter `-` (guión). `cd` que se interpreta como
> *el directorio anterior que estaba en*, lo cual es más rápido que tener que recordar,
> y luego escribir, la ruta completa. Esta es una manera *muy* eficiente de ir y venir
> entre directorios. La diferencia entre `cd ..` y `cd -` es
> que el primero te mueva hacia *adelante*, mientras que el último te *regresa*. Usted puede
> pensar en ello como el botón *último canal* en un control remoto de su televisión.
{: .callout}

### Pipeline de Alicia: Organización de archivos

Sabiendo todo esto de archivos y directorios,
Alicia está lista para organizar los archivos que creará la máquina de análisis de proteínas.
Primero,
crea un directorio llamado `north-pacific-gyre`
(Para recordar de dónde provienen los datos).
Dentro de este,
crea un directorio llamado `2012-07-03`,
que es la fecha en que comenzó a procesar las muestras.
Solía utilizar nombres como «conference-paper» y «revised-results»,
pero los encontró difíciles de entender después de un par de años.
(La gota que derramo el vaso fue cuando se encontró creando
un directorio denominado `revised-revised-results-3`.)

> ## Ordenando la salida
>
> Alicia nombra sus directorios "año-mes-día",
> con ceros a la cabeza para meses y días,
> porque el shell muestra los nombres de archivos y directorios en orden alfabético.
> Si usara nombres de mes, diciembre vendría antes de julio;
> si no utiliza ceros a la izquierda,
> Noviembre ('11') vendría antes de julio ('7'). Del mismo modo, poner el año primero
> significa que junio de 2012 aparecerá antes de junio de 2013.
{: .callout}

Cada una de sus muestras físicas está etiquetada según la convención de su laboratorio
con un identificador único de diez caracteres,
tal como "NENE01729A".
Esto es lo que utilizó en su registro de la colección
para registrar la ubicación, el tiempo, la profundidad y otras características de la muestra,
por lo que decide utilizarlo como parte del nombre de cada archivo de datos.
Dado que la salida de la máquina de ensayo es texto sin formato,
ella llamará a sus archivos `NENE01729A.txt`, `NENE01812A.txt`, y así sucesivamente.
Todos los 1,520 archivos estarán en el mismo directorio.

Ahora en su directorio actual `data-shell`,
Alicia puede ver qué archivos tiene usando el comando:

~~~
$ ls north-pacific-gyre/2012-07-03/
~~~
{: .bash}

Esto es mucho para teclear,
pero puede permitir que el shell haga la mayor parte del trabajo a través de lo 
que se llama **autocompletado con el tabulador**.
Si escribe:

~~~
$ ls nor
~~~
{: .bash}

Y después presiona el tabulador (la tecla de tabulador en su teclado),
el shell completa automáticamente el nombre del directorio para ella:

~~~
$ ls north-pacific-gyre/
~~~
{: .bash}

Si presiona el tabulador otra vez,
Bash añadirá `2012-07-03/` al comando,
Ya que es el único autocompletamiento posible.
Presionar el tabulador de nuevo no hace nada,
ya que hay 19 posibilidades;
presionando el tabulador dos veces muestra una lista de todos los archivos,
y así.
Esto se denomina **autocompletado con el tabulador**,
y lo veremos en muchas otras herramientas a medida que avancemos.

> ## Rutas Absolutas vs Relativas
>
> A partir de `/home/amanda/data/`,
> ¿Cuál de los siguientes comandos podría Amanda usar para navegar a su directorio de inicio,
> que es `/home/amanda`?
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
> > 3. No: El directorio personal de Amanda es `/home/amanda`.
> > 4. No: sube dos niveles, es decir termina en `/home`.
> > 5. Sí:  `~` significa el directorio personal del usuario, en este caso` /home/amanda`.
> > 6. No: esto navegaria en un directorio `home` en el directorio actual si existe.
> > 7. Sí: innecesariamente complicado, pero correcto.
> > 8. Sí: un atajo para volver al directorio principal del usuario.
> > 9. Sí: sube un nivel.
> {: .solution}
{: .challenge}

> ## Resolución de ruta relativa
>
> Si se utiliza el diagrama de sistema de directorios de abajo, si `pwd` muestra `/home/thing`,
> ¿Qué mostrará `ls ../ backup`?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original pnas_final pnas_sub`
>
> ![Sistema de archivos para las preguntas del desafío](../fig/filesystem-challenge.svg)
>
> > ## Solución
> > 1. No: hay *is* un directorio `backup` en `/home`.
> > 2. No: este es el contenido de `home/thing/backup`,
> >    pero con `..` pedimos un nivel más arriba.
> > 3. No: vea la explicación anterior.
> >    Además, no especificamos `-F` para mostrar `/`al final de los nombres de directorio.
> > 4. Sí: `../backup` se refiere a `/home/backup`.
> {: .solution}
{: .challenge}

> ## `ls` comprensión de lectura
>
> Suponiendo una estructura de directorio como en la figura anterior,
> si `pwd` muestra`/home/backup`,
> Y `-r` le dice` ls` para mostrar las cosas en orden inverso,
> Qué comando mostrará:
>
> ~~~
> pnas_sub/ pnas_final/ original/
> ~~~
> {: .output}
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /home/backup`
> 4. O bien #2 o #3, pero no #1.
>
> > ## Solution
> >  1. No: `pwd` no es el nombre de un directorio.
> >  2. Sí: `ls` sin argumento de directorio lista archivos y directorios en el directorio actual.
> >  3. Sí: utiliza explícitamente el camino absoluto.
> >  4. Correcto: vea las explicaciones arriba.
> {: .solution}
{: .challenge}

> ## Explorando más argumentos de `ls`
>
> ¿Qué hace el comando `ls` cuando se utilizan con los argumentos` -l` y `-h`?
>
> Algunos de sus resultados son sobre propiedades que no cubrimos en esta lección (tales como
> como permisos de archivo y propiedad), pero el resto debe ser útil
> sin embargo.
>
> > ## Solución
> > Los argumentos `-l` hacen que `ls` utilice un formato de lista **l**argo, mostrando no sólo
> > los nombres de archivo/directorio, sino también información adicional como el tamaño del archivo
> > y la hora de su última modificación. El argumento `-h` hace que el tamaño del archivo sea
> > "**h**uman readable" (legible por humanos), es decir, muestra algo como `5.3K` en lugar de` 5369`.
> {: .solution}
{: .challenge}

> ## Listando recursivamente y por tiempo
>
> El comando `ls -R` enumera el contenido de los directorios recursivamente, es decir, lista
> sus subdirectorios, subsubdirectorios, etc. en orden alfabético
> en cada nivel. El comando `ls -t` enumera las cosas por el tiempo del último cambio, con
> los archivos o directorios más recientemente cambiados primero.
> ¿En qué orden se muestra `ls -R -t`? Sugerencia: `ls -l` usa una lista larga
> de formato para ver los tiempos.
>
> > ## Solución
> > Los directorios se listan alfabéticamente en cada nivel, los archivos/directorios
> > en cada directorio se ordenan por la hora del último cambio.
> {: .solution}
{: .challenge}
