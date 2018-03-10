---
title: "Encontrando archivos"
teaching: 15
exercises: 0
questions:
- "¿Cómo puedo encontrar archivos?"
- "¿Cómo puedo encontrar algunas cosas dentro de mis archivos?"
objectives:
- "Usar `grep` para seleccionar las líneas de archivos de texto que coincidan con patrones simples."
- "Usar `find` para encontrar archivos cuyos nombres coincidan con patrones simples."
- "Usar la respuesta de un comando como parámetro de entrada para otro comando."
- "Explicar lo que se entiende por archivos de 'texto' y 'binarios', y por qué muchas herramientas comunes no manejan bien estos últimos."
keypoints:
- "`find` encuentra archivos con propiedades específicas que coinciden con los patrones especificados."
- "`grep` selecciona líneas en archivos que coinciden con los patrones especificados."
- "`--help` es un indicador usado por muchos comandos bash y programas que se pueden ejecutar desde dentro de Bash, se usa para mostrar más información sobre cómo usar estos comandos o programas."
- "`man command` muestra la página del manual de un comando."
- "`$(comando)` contiene la salida de un comando."
---

De la misma manera que muchos de nosotros ahora usamos "googlear" como un
verbo que significa "encontrar", los programadores de Unix usan cotidianamente
la palabra "grep".
"grep" es una contracción de "global/regular expression/print" (global/expresión regular/imprimir),
una secuencia común de operaciones en los primeros editores de texto de Unix.
También es el nombre de un programa de línea de comandos muy útil.

El comando `grep` encuentra e imprime líneas en archivos que coinciden con un patrón.
Para nuestros ejemplos,
usaremos un archivo que contenga tres haikus publicados en
1998 en la revista *Salon*. Para este conjunto de ejemplos,
Vamos a estar trabajando en el subdirectorio "writing":

~~~
$ cd
$ cd writing
$ cat haiku.txt
~~~
{: .bash}

~~~
The Tao that is seen
Is not the true Tao, until
You bring fresh toner.

With searching comes loss
and the presence of absence:
"My Thesis" not found.

Yesterday it worked
Today it is not working
Software is like that.
~~~
{: .output}

> ## Del dicho "Para siempre, o cinco años" 
>
> Nota: no hemos vinculado a los haikus originales porque parecen ya no estar disponibles en el sitio de *Salon*.
> Como [Jeff Rothenberg dijo](http://www.clir.org/pubs/archives/ensuring.pdf),
> "La información digital dura para siempre --- o cinco años, lo que ocurra primero."
{: .callout}

Busquemos líneas que contengan la palabra "not":

~~~
$ grep not haiku.txt
~~~
{: .bash}

~~~
Is not the true Tao, until
"My Thesis" not found
Today it is not working
~~~
{: .output}

En este ejemplo, `not` es el patrón que estamos buscando.
Es bastante simple:
cada carácter alfanumérico coincide con sí mismo.
Después del patrón viene el nombre o los nombres de los archivos que estamos buscando.
La salida son las tres líneas del archivo que contienen las letras "not".

Vamos a probar un patrón distinto: "The". 

~~~
$ grep The haiku.txt
~~~
{: .bash}

~~~
The Tao that is seen
"My Thesis" not found.
~~~
{: .output}

Esta vez, las dos líneas que incluyen las letras "The" forman parte del producto de la búsqueda. 
Sin embargo, una instancia de esas letras está contenida en una palabra más grande, "Thesis".

Para restringir los aciertos a las líneas que contienen la palabra "The" por sí sola,
podemos usar `grep` con el indicador` -w`.
Esto limitará los coincidencias a palabras.

~~~
$ grep -w The haiku.txt
~~~
{: .bash}

~~~
The Tao that is seen
~~~
{: .output}


Nota que la palabra o patrón de búsqueda incluye el comienzo y el final de una línea, no solamente aquellas letras rodeadas de espacios. 
A veces, queremos buscar una frase, en vez de una sola palabra. Esto puede hacerse fácilmente con `grep` usando la frase entre comillas.

~~~
$ grep -w "is not" haiku.txt
~~~
{: .bash}

~~~
Today it is not working
~~~
{: .output}

Hemos visto que no requerimos comillas alrededor de palabras simples,
pero es útil utilizar comillas cuando se buscan varias palabras consecutivas.
También ayuda a facilitar la distinción entre el término o frase de búsqueda
y el archivo en el que se está buscando.
Utilizaremos comillas en los próximos ejemplos.

Otra opción útil es `-n`, que numera las líneas que coinciden:

~~~
$ grep -n "it" haiku.txt
~~~
{: .bash}

~~~
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
~~~
{: .output}

Aquí, podemos ver que las líneas 5, 9 y 10 contienen las letras "it".

Podemos combinar opciones (es decir, **flags**) como lo hacemos con otros comandos de Unix.
Por ejemplo, vamos a encontrar las líneas que contienen la palabra "the". Podemos combinar
la opción `-w` para encontrar las líneas que contienen la palabra "the" con `-n` para numerar las líneas que coinciden:

~~~
$ grep -n -w "the" haiku.txt
~~~
{: .bash}

~~~
2:Is not the true Tao, until
6:and the presence of absence:
~~~
{: .output}

Ahora queremos usar la opción `-i` para hacer que nuestra búsqueda sea insensible a mayúsculas y minúsculas:

~~~
$ grep -n -w -i "the" haiku.txt
~~~
{: .bash}

~~~
1:The Tao that is seen
2:Is not the true Tao, until
6:and the presence of absence:
~~~
{: .output}

Ahora, queremos usar la opción `-v` para invertir nuestra búsqueda, es decir, queremos que
obtener las líneas que NO contienen la palabra "the".

~~~
$ grep -n -w -v "the" haiku.txt
~~~
{: .bash}

~~~
1:The Tao that is seen
3:You bring fresh toner.
4:
5:With searching comes loss
7:"My Thesis" not found.
8:
9:Yesterday it worked
10:Today it is not working
11:Software is like that.
~~~
{: .output}

`grep` tiene muchas otras opciones. Para averiguar cuáles son, podemos escribir:

~~~
$ grep --help
~~~
{: .bash}

~~~
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE or standard input.
PATTERN is, by default, a basic regular expression (BRE).
Example: grep -i 'hello world' menu.h main.c

Regexp selection and interpretation:
  -E, --extended-regexp     PATTERN is an extended regular expression (ERE)
  -F, --fixed-strings       PATTERN is a set of newline-separated fixed strings
  -G, --basic-regexp        PATTERN is a basic regular expression (BRE)
  -P, --perl-regexp         PATTERN is a Perl regular expression
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         ignore case distinctions
  -w, --word-regexp         force PATTERN to match only whole words
  -x, --line-regexp         force PATTERN to match only whole lines
  -z, --null-data           a data line ends in 0 byte, not newline

Miscellaneous:
...        ...        ...
~~~
{: .output}

> ## Utilizando `grep`
>
> Haciendo referencia a `haiku.txt`
> presentado al inicio de este tema,
> qué comando resultaría en la salida siguiente:
>
> ~~~
> and the presence of absence:
> ~~~
> {: .output}
>
> 1. `grep "of" haiku.txt`
> 2. `grep -E "of" haiku.txt`
> 3. `grep -w "of" haiku.txt`
> 4. `grep -i "of" haiku.txt`
{: .challenge}

> ## Caracteres especiales o comodines
>
> La verdadera ventaja de `grep` no proviene de sus opciones; viene del
> hecho de que los patrones pueden incluir comodines. (El nombre técnico para
> estos son **expresiones regulares**, que
> es lo que significa el "re" en "grep".) Las expresiones regulares son complejas
> y poderosas. Nota: Si quieres realizar búsquedas complejas, y quieres saber más detalles mira la lección
> de [expresiones regulares](http://v4.software-carpentry.org/regexp/index.html). 
> Como prueba, podemos
> encontrar líneas que tienen una 'o' en la segunda posición, por ejemplo:
>
> ~~~
> $ grep -E '^.o' haiku.txt
> ~~~
> {: .bash}
>
> ~~~
> You bring fresh toner.
> Today it is not working
> Software is like that.
> ~~~
> {: .output}
>
> Utilizamos el indicador `-E` y ponemos el patrón entre comillas para evitar que el shell
> intente interpretarlo. (Si el patrón contiene un `*`, por
> ejemplo, el shell intentaría expandirlo antes de ejecutar grep.)
> El `^` en el patrón de búsqueda indica que la coincidencia debe ser al inicio de la línea. El `.`
> coincide con un solo carácter (como `?` en el shell), mientras que el `o`
> coincide con una 'o' real que en este caso viene a ser el segundo caracter.
{: .callout}

> ## Seguimiento de una especie
> 
> Leah tiene varios cientos de
> archivos de datos guardados en un directorio, cada uno de los cuales tiene el formato siguiente:
> 
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> ~~~
> {: .source}
>
> Quiere escribir un **script** de shell que tome un directorio y una especie
> como parámetros de línea de comandos y que le devuelva un archivo llamado `species.txt`
> que contenga una lista de fechas y el número de individuos de esa especie observados en esa fecha,
> como este archivo de números de conejos:
> 
> ~~~
> 2013-11-05,22
> 2013-11-06,19
> 2013-11-07,18
> ~~~
> {: .source}
>
> Escribe estos comandos y **pipes** en el orden correcto para lograrlo:
> 
> ~~~
> cut -d : -f 2  
> >  
> |  
> grep -w $1 -r $2  
> |  
> $1.txt  
> cut -d , -f 1,3  
> ~~~
> {: .bash}
>
> Sugerencia: usa `man grep` para buscar cómo seleccionar texto recursivamente en un directorio
> usando grep
> y `man cut` para seleccionar más de un campo en una línea.
> Encuentra un ejemplo en el siguiente archivo `data-shell/data/animal-counts/animals.txt`
>
> > ## Solución
> >
> > ```
> > grep -w $1 -r $2 | cut -d : -f 2 | cut -d , -f 1,3  > $1.txt
> > ```
> > {: .source}
> >
> > Puedes llamar al **script** de la siguiente forma:
> >
> > ```
> > $ bash count-species.sh bear .
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Mujercitas
>
> Tú y tu amigo tienen una desacuerdo, después de terminar de leer *Little Women* de
> Louisa May Alcott. De las cuatro hermanas en el
> libro, Jo, Meg, Beth, y Amy, tu amigo piensa que Jo era la
> más mencionada. Tú, sin embargo, estás segura de que era Amy. Afortunadamente tu
> tienes un archivo llamado `LittleWomen.txt` en data/, que contiene el texto completo de la novela.
> Utilizando un bucle `for`, ¿cómo tabularías el número de veces que cada una
> de las cuatro hermanas es mencionada?
>
> Sugerencia: una solución sería emplear
> los comandos `grep` y `wc` y un `|`, mientras que otra solución podría utilizar
> opciones de `grep`.
> Normalmente hay más de una forma para solucionar una tarea de programación. Cada solución es seleccionada de acuerdo a la combinación entre cuál es la que produce el resultado correcto, con mayor elegancia, de la manera más clara y con mayor velocidad.
> 
> > ## Solución
> > ```
> > for sis in Jo Meg Beth Amy
> > do
> > 	echo $sis:
> >	  grep -ow $sis LittleWomen.txt | wc -l
> > done
> > ```
> > {: .source}
> >
> > Alternativa, menos eficiente:
> > ```
> > for sis in Jo Meg Beth Amy
> > do
> > 	echo $sis:
> >	grep -ocw $sis LittleWomen.txt
> > done
> > ```
> > {: .source}
> >
> > Esta solución es menos eficiente porque `grep -c` solamente reporta el número de las líneas.
> > El número total de coincidencias que es reportado por este método va a ser más bajo si hay más de una coincidencia por línea. 
> {: .solution}
{: .challenge}

Mientras `grep` encuentra líneas en los archivos,
El comando `find` busca los archivos.
Una vez más,
tienes muchas opciones.
Para mostrar cómo funcionan las más simples, utilizaremos el árbol de directorios que se muestra a continuación.

![Árbol de archivos para el ejemplo de búsqueda](../fig/find-file-tree.svg)

El directorio `writing` de Nelle contiene un archivo llamado `haiku.txt` y tres subdirectorios:
`thesis` (que contiene un archivo tristemente vacío, `empty-draft.md`),
`data` (que contiene dos archivos `LittleWomen.txt` , `one.txt` y `two.txt`),
un directorio `tools` que contiene los programas` format` y `stats`,
y un subdirectorio llamado `old`, con un archivo `oldtool`.

Para nuestro primer comando,
ejecutemos `find .`.
~~~
$ find .
~~~
{: .bash}

~~~
.
./data
./data/one.txt
./data/LittleWomen.txt
./data/two.txt
./tools
./tools/format
./tools/old
./tools/old/oldtool
./tools/stats
./haiku.txt
./thesis
./thesis/empty-draft.md
~~~
{: .output}

Como siempre,
el `.` por sí mismo significa el directorio de trabajo actual,
que es donde queremos que empiece nuestra búsqueda.
La salida de `find` es el nombre de cada archivo **y** directorio
dentro del directorio de trabajo actual.
Esto puede parecer inútil al principio, pero `find` tiene muchas opciones
para filtrar la salida y en esta lección vamos a descubrir algunas
de ellas.

La primera opción en nuestra lista es
`-type d` que significa "encontrar directorios".
Por supuesto que
la salida de `find` serán los nombres de los cinco directorios en nuestro pequeño árbol
(incluyendo `.`):

~~~
$ find . -type d
~~~
{: .bash}

~~~
./
./data
./thesis
./tools
./tools/old
~~~
{: .output}

Si cambiamos `-type d` por `-type f`,
recibimos una lista de todos los archivos:

~~~
$ find . -type f
~~~
{: .bash}

~~~
./haiku.txt
./tools/stats
./tools/old/oldtool
./tools/format
./thesis/empty-draft.md
./data/one.txt
./data/LittleWomen.txt
./data/two.txt
~~~
{: .output}

Ahora tratemos de buscar por nombre:

~~~
$ find . -name *.txt
~~~
{: .bash}

~~~
./haiku.txt
~~~
{: .output}

Esperábamos que encontrara todos los archivos de texto,
Pero sólo imprime `./haiku.txt`.
El problema es que el shell amplía los caracteres comodín como `*` *antes* de ejecutar los comandos.
Dado que `*.txt` en el directorio actual se expande a `haiku.txt`,
El comando que ejecutamos era:

~~~
$ find . -name haiku.txt
~~~
{: .bash}

`find` hizo lo que pedimos, pero pedimos la cosa equivocada.

Para conseguir lo que queremos,
vamos a hacer lo que hicimos con `grep`:
escribe `* txt` entre comillas simples para evitar que el shell expanda el comodín `*`.
De esta manera,
`find` obtiene realmente el patrón `*.txt`, no el nombre de archivo expandido `haiku.txt`:

~~~
$ find . -name '*.txt'
~~~
{: .bash}

~~~
./data/one.txt
./data/LittleWomen.txt
./data/two.txt
./haiku.txt
~~~
{: .output}

> ## Listar vs. Buscar
>
> `ls` y` find` se pueden usar para hacer cosas similares dadas las opciones correctas,
> pero en circunstancias normales,
> `ls` enumera todo lo que puede,
> mientras que `find` busca cosas con ciertas propiedades y las muestra.
{: .callout}

Como mencionamos anteriormente,
el poder de la línea de comandos reside en la combinación de herramientas.
Hemos visto cómo hacerlo con **pipes**,
veamos otra técnica.
Como acabamos de ver,
`find . -name '*.txt'` nos da una lista de todos los archivos de texto dentro o más abajo del directorio actual.
¿Cómo podemos combinar eso con `wc -l` para contar las líneas en todos esos archivos?

La forma más sencilla es poner el comando `find` dentro de `$()`:

~~~
$ wc -l $(find . -name '*.txt')
~~~
{: .bash}

~~~
11 ./haiku.txt
300 ./data/two.txt
21022 ./data/LittleWomen.txt
70 ./data/one.txt
21403 total
~~~
{: .output}

Cuando el shell ejecuta este comando,
lo primero que hace es ejecutar lo que esté dentro del `$()`.
Luego reemplaza la expresión `$()` con la salida de ese comando.
Dado que la salida de `find` son los cuatro nombres de directorio `./data/one.txt`, `./data/LittleWomen.txt`, `./data/two.txt`, y `./haiku.txt`,
el shell construye el comando:

~~~
$ wc -l ./data/one.txt ./data/LittleWomen.txt ./data/two.txt ./haiku.txt
~~~
{: .bash}

que es lo que queríamos.
Esta expansión es exactamente lo que hace el shell cuando se expanden comodines como `*` y `?`,
pero nos permite usar cualquier comando que deseemos como nuestro propio "comodín".

Es muy común usar `find` y `grep` juntos.
El primero encuentra archivos que coinciden con un patrón;
el segundo busca líneas dentro de los archivos que coinciden con otro patrón.
Aquí, por ejemplo, podemos encontrar archivos PDB que contienen átomos de hierro
buscando la cadena "FE" en todos los archivos `.pdb` por encima del directorio actual:

~~~
$ grep "FE" $(find .. -name '*.pdb')
~~~
{: .bash}

~~~
../data/pdb/heme.pdb:ATOM     25 FE           1      -0.924   0.535  -0.518
~~~
{: .output}

> ## Buscando coincidencias y restando
>
> `-v` es una opción de `grep` que invierte la coincidencia de patrones, de modo que sólo las líneas
> que *no* coinciden con el patrón se imprimen. Dado esto ¿cuál de
> los siguientes comandos encontrarán todos los archivos en `/data` cuyos nombres
> terminan en `s.txt` (por ejemplo, `animals.txt` or `planets.txt`), pero
> *no* contienen la palabra `net`?
> Después de pensar en tu respuesta, puedes probar los comandos en el directorio `data-shell`.
>
> 1.  `find data -name '*s.txt' | grep -v net`
> 2.  `find data -name *s.txt | grep -v net`
> 3.  `grep -v "temp" $(find data -name '*s.txt')`
> 4.  Ninguna de las anteriores.
>
>> ## Solución
>> La respuesta correcta es 1. Poniendo la expresión entre comillas, evita que el shell
>> la expanda, y luego lo pasa al comando `find`.
>>
>> La opción 2 es incorrecta porque el shell expande `*s.txt` en lugar de mandar el nombre correcto al
>> `find`.
>>
>> La opción 3 es incorrecta porque busca en los contenidos de los archivos las líneas que
>> no coinciden con "temp", en lugar de buscar los nombres de los archivos.
> {: .solution}
{: .challenge}


> ## Archivos binarios
>
> Nos hemos centrado exclusivamente en encontrar cosas en archivos de texto. ¿Y si
> sus datos se almacenan como imágenes, en bases de datos, o en algún otro formato?
> Una opción sería extender herramientas como `grep` para manejar esos formatos.
> Esto no ha sucedido, y probablemente no se hará, porque hay demasiados
> formatos disponibles. 
>
> La segunda opción es convertir los datos en texto, o extraer los
> bits de texto de los datos. Este es probablemente el enfoque más común,
> ya que sólo requiere generar una herramienta por formato de datos (para
> extraer información). Por un lado, facilita realizar las cosas simples.
> Por el contrario, las cosas complejas son generalmente imposibles. Por
> ejemplo, es bastante fácil escribir un programa que extraiga las dimensiones X, Y
> de los archivos de imagen para que podamos después usar `grep`, pero ¿cómo
> escribir algo para encontrar valores en una hoja de cálculo cuyas celdas contienen
> fórmulas?
>
> La tercera opción es reconocer que laterminal y el procesamiento de texto
> tiene sus límites, y en caso necesario debes utilizar otro lenguaje de programación.
> Sin embargo, el shell te seguirá siendo útil para realizar tareas de 
> procesamiento diarias.
{: .callout}

La terminal de Unix es más antiguo que la mayoría de las personas que lo usan. 
Ha sobrevivido tanto tiempo porque es uno de los ambientes de programación más productivos
jamás creados --- quizás incluso *el* más productivo. Su sintaxis
puede ser críptica, pero las personas que lo dominan pueden experimentar con
diferentes comandos interactivamente, y luego utilizar lo que han aprendido para
automatizar su trabajo. Las interfaces gráficas de usuario pueden ser más sencillas
al principio, pero el shell sigue invicto en segunda instancia. Y como Alfred
North Whitehead escribió en 1911, "La civilización avanza extendiendo el
número de operaciones importantes que podemos realizar sin pensar
en ellas."

> ## `find` Comprensión de lectura de pipeline
>
> Escribe un breve comentario explicativo para el siguiente **script** de shell:
>
> ~~~
> wc -l $(find . -name '*.dat') | sort -n
> ~~~
> {: .bash}
>
>> ## Solución
>> 1. Encuentra todos los archivos con una extensión `.dat` en el directorio actual
>> 2. Cuenta el número de líneas que contiene cada uno de estos archivos
>> 3. Ordena la salida del paso 2. numéricamente
> {: .solution}
{: .challenge}

> ## Búsqueda de archivos con propiedades diferentes
>
> El comando `find` puede recibir varios otros criterios conocidos como "tests"
> para localizar archivos con atributos específicos, como tiempo de creación, tamaño,
> permisos o dueño. Explora el manual con `man find` y luego
> escribe un solo comando para encontrar todos los archivos en o debajo del directorio actual
> que han sido modificados por el usuario `ahmed` en las últimas 24 horas.
>
> Sugerencia 1: necesitarás utilizar tres tests: `-type`,` -mtime` y `-user`.
>
> Sugerencia 2: El valor de `-mtime` tendrá que ser negativo --- ¿por qué?
>
> > ## Solución: 
> > Asumiendo que el directorio de inicio de Nelle es nuestro directorio en el que trabajamos, podemos usar:
> >
> > ~~~
> > $ find ./ -type f -mtime -1 -user ahmed
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}
