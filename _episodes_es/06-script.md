---
title: "Scripts de shell"
teaching: 15
exercises: 0
questions:
- "¿Cómo puedo guardar y reutilizar comandos?"
objectives:
- "Escriba un script de shell que ejecute un comando o una serie de comandos para un conjunto fijo de archivos."
- "Ejecutar un script de shell desde la línea de comandos."
- "Escribir un script de shell que funciona en un conjunto de archivos definidos por el usuario en la línea de comandos."
- "Crear pipelines que incluyan scripts de shell que usted y otros hayan escrito."
keypoints:
- "Guardar comandos en archivos (normalmente llamados scripts de shell) para su reutilización."
- "`bash filename` ejecuta los comandos guardados en un archivo."
- "`$@`se refiere a todos los parámetros de la línea de comandos de un shell script."
- "`$1`, `$2`, etc., se refieren al primer parámetro de la línea de comandos, al segundo parámetro de la línea de comandos, etc."
- "Coloque las variables entre comillas si los valores tienen espacios en ellas."
- "Dejar que los usuarios decidan qué archivos procesar es más flexible y más consistente con los comandos de Unix."
---

Finalmente estamos listos para ver lo que hace que la shell sea un entorno de programación tan potente.
Vamos a tomar los comandos que repetimos con frecuencia y guardarlos en archivos
de modo que podemos volver a ejecutar todas esas operaciones de nuevo más tarde escribiendo un solo comando.
Por razones históricas,
a un conjunto de comandos guardados en un archivo se le llama un **shell script**,
pero no se equivoquen:
estos son en realidad pequeños programas.

Comencemos por volver a `molecules/` y pongamos la siguiente línea en un nuevo archivo, `middle.sh`:

~~~
$ cd molecules
$ nano middle.sh
~~~
{: .bash}

El comando `nano middle.sh` abre el archivo `middle.sh` dentro del editor de texto "nano"
(que se ejecuta dentro del shell).
Si el archivo no existe, se creará.
Podemos usar el editor de texto para editar directamente el archivo --- simplemente insertaremos la siguiente línea:

~~~
head -n 15 octane.pdb | tail -n 5
~~~
{: .source}

Esta es una variación de la tubería que construimos anteriormente:
selecciona las líneas 11-15 del archivo `octane.pdb`.
Recuerde, *no* lo estamos ejecutando como un comando todavía:
solo estamos poniendo los comandos en un archivo.

A continuación, guardamos el archivo (`Ctrl-O` en nano),
y salimos del editor de texto (`Ctrl-X` en nano).
Compruebe que el directorio `molecules` ahora contiene un archivo llamado `middle.sh`.

Una vez que hayamos guardado el archivo,
Podemos pedirle al shell que ejecute los comandos que contiene.
Nuestro shell se llama `bash`, por lo que ejecutamos el siguiente comando:

~~~
$ bash middle.sh
~~~
{: .bash}

~~~
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~
{: .output}

Efectivamente,
la salida de nuestro script es exactamente lo que obtendríamos si ejecutamos esa tubería directamente
en la terminal.

> ## Texto vs. Lo que sea
>
> Por lo general llamamos a programas como Microsoft Word o LibreOffice Writer "editores
> de texto", pero tenemos que ser un poco más cuidadosos cuando se trata de
> programación. De forma predeterminada, Microsoft Word utiliza archivos .docx para almacenar no
> sólo texto, sino también formatear información sobre fuentes, encabezados,
> etcétera. Esta información adicional no se almacena como caracteres y no es comprendida por
> herramientas como `head`: los comandos esperan que los archivos de entrada contengan
> unicamente las letras, los dígitos y la puntuación en un teclado de computador estándar.
> Por lo tanto, al editar programas, debemos utilizar un
> editor de texto, además de tener cuidado de guardar nuestros archivos como texto sin formato.
{: .callout}

¿Qué pasa si queremos seleccionar líneas de un archivo arbitrario?
Podríamos editar `middle.sh` cada vez para cambiar el nombre de archivo,
pero que probablemente llevaría más tiempo que simplemente volver a escribir el comando.
En su lugar, vamos a editar `middle.sh` para hacerlo más versátil:

~~~
$ nano middle.sh
~~~
{: .bash}

Ahora, dentro de "nano", reemplace el texto `octane.pdb` con la variable especial llamada `$1`:

~~~
head -n 15 "$1" | tail -n 5
~~~
{: .output}

Dentro de un script de shell,
`$1` significa "el primer nombre de archivo (u otro parámetro) en la línea de comandos".
Ahora podemos ejecutar nuestro script de esta manera:

~~~
$ bash middle.sh octane.pdb
~~~
{: .bash}

~~~
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~
{: .output}

o en un archivo diferente:

~~~
$ bash middle.sh pentane.pdb
~~~
{: .bash}

~~~
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~
{: .output}

> ## Comillas dobles alrededor de argumentos
>
> Por la misma razón que ponemos la variable del bucle dentro de comillas dobles,
> cuando el nombre de archivo contenga espacios,
> rodeamos `$1` con comillas dobles.
{: .callout}

Aún así necesitamos editar `middle.sh` cada vez que queramos ajustar el rango de líneas.
Vamos a arreglar esto usando las variables especiales `$2` y` $3` para el
número de líneas que se pasarán respectivamente a `head` y `tail`:

~~~
$ nano middle.sh
~~~
{: .bash}

~~~
head -n "$2" "$1" | tail -n "$3"
~~~
{: .output}

Ahora podemos ejecutar:

~~~
$ bash middle.sh pentane.pdb 15 5
~~~
{: .bash}

~~~
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~
{: .output}

Cambiando los argumentos a nuestro comando, podemos cambiar el comportamiento
del script:

~~~
$ bash middle.sh pentane.pdb 20 5
~~~
{: .bash}

~~~
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
ATOM     15  H           1      -2.608  -0.407   1.130  1.00  0.00
ATOM     16  H           1      -2.540  -1.303  -0.404  1.00  0.00
ATOM     17  H           1      -3.393   0.254  -0.321  1.00  0.00
TER      18              1
~~~
{: .output}

Esto funciona,
Pero puede a la siguiente persona que use `middle.sh` se le puede dificultar averiguar lo que hace.
Podemos mejorar nuestro script agregando algunos **comentarios** en la parte superior:

~~~
$ nano middle.sh
~~~
{: .bash}

~~~
# Select lines from the middle of a file.
# Usage: bash middle.sh filename end_line num_lines
head -n "$2" "$1" | tail -n "$3"
~~~
{: .output}

Un comentario comienza con un caracter `#` y se ejecuta hasta el final de la línea.
La computadora ignora los comentarios,
pero son invaluables para ayudar a la gente (incluyendo a tu futuro yo) a entender y usar scripts.
La única advertencia es que cada vez que modifique el script,
debe comprobar que el comentario sigue siendo exacto:
Una explicación que envía al lector en la dirección equivocada es peor que ninguna en absoluto.

Generalmente escribimos los comentarios en inglés para ampliar su uso en la comunidad científica, 
además de que nos permite agregar nuestros scripts a publicaciones.

¿Qué sucede si queremos procesar muchos archivos en una sola tubería?
Por ejemplo, si queremos ordenar nuestros archivos `.pdb` por longitud, deberíamos escribir:

~~~
$ wc -l *.pdb | sort -n
~~~
{: .bash}

dado que `wc -l` lista el número de líneas en los archivos
(recuerde que `wc` significa 'word count', y si agrega el `-l` significa 'contar líneas')
y `sort -n` ordena las cosas numéricamente.
Podríamos poner esto en un archivo,
pero entonces sólo podría ordenar una lista de archivos `.pdb` en el directorio actual.
Si queremos ser capaces de obtener una lista ordenada de otros tipos de archivos,
necesitamos una manera de conseguir todos esos nombres en el guión.
No podemos usar `$1`, `$2`, y así sucesivamente
porque no sabemos cuántos archivos hay.
En su lugar, utilizamos la variable especial `$@`,
lo que significa,
"Todos los parámetros de la línea de comandos a la secuencia de comandos de shell."
También debemos poner `$@` dentro de comillas dobles
para manejar el caso de los parámetros que contienen espacios
(`"$@"` es equivalente a `"$1"` `"$2"` ...)
He aquí un ejemplo:

~~~
$ nano sorted.sh
~~~
{: .bash}

~~~
# Sort filenames by their length.
# Usage: bash sorted.sh one_or_more_filenames
wc -l "$@" | sort -n
~~~
{: .output}

~~~
$ bash sorted.sh *.pdb ../creatures/*.dat
~~~
{: .bash}

~~~
9 methane.pdb
12 ethane.pdb
15 propane.pdb
20 cubane.pdb
21 pentane.pdb
30 octane.pdb
163 ../creatures/basilisk.dat
163 ../creatures/unicorn.dat
~~~
{: .output}

> ## ¿Por qué no está haciendo nada?
>
> ¿Qué pasa si se supone que un script procesa un montón de archivos, pero nosotros
> no le damos nombres de archivo? Por ejemplo, ¿qué pasa si escribimos?:
>
> ~~~
> $ bash sorted.sh
> ~~~
> {: .bash}
>
> pero sin agregar `*.dat` (o cualquier otra cosa)? En este caso, `$@` se expande a
> nada en absoluto, por lo que la tubería dentro del script es efectivamente:
>
> ~~~
> $ wc -l | sort -n
> ~~~
> {: .bash}
>
> Dado que no tiene ningún nombre de archivo, `wc` supone que debe
> procesar entrada estándar, por lo que sólo se sienta allí y espera a que le demos
> algunos datos de forma interactiva. Desde el exterior, sin embargo, todo lo que vemos es
> que el programa está estático: el guión no parece hacer nada.
{: .callout}

Supongamos que acabamos de ejecutar una serie de comandos que hicieron algo útil --- por ejemplo,
que crearon un gráfico que nos gustaría utilizar en un documento.
Nos gustaría poder volver a crear el gráfico más tarde si es necesario,
por lo que queremos guardar los comandos en un archivo.
En lugar de escribirlos de nuevo
(y potencialmente equivocarse)
podemos hacer esto:

~~~
$ history | tail -n 5 > redo-figure-3.sh
~~~
{: .bash}

El archivo `redo-figure-3.sh` ahora contiene:

~~~
297 bash goostats -r NENE01729B.txt stats-NENE01729B.txt
298 bash goodiff stats-NENE01729B.txt /data/validated/01729.txt > 01729-differences.txt
299 cut -d ',' -f 2-3 01729-differences.txt > 01729-time-series.txt
300 ygraph --format scatter --color bw --borders none 01729-time-series.txt figure-3.png
301 history | tail -n 5 > redo-figure-3.sh
~~~
{: .source}

After a moment's work in an editor to remove the serial numbers on the commands,
and to remove the final line where we called the `history` command,
we have a completely accurate record of how we created that figure.

In practice, most people develop shell scripts by running commands at the shell prompt a few times
to make sure they're doing the right thing,
then saving them in a file for re-use.
This style of work allows people to recycle
what they discover about their data and their workflow with one call to `history`
and a bit of editing to clean up the output
and save it as a shell script.

Después de un poco de trabajo en un editor para eliminar los números de serie en los comandos,
y para eliminar la línea final donde llamamos el comando `history`,
tenemos un registro completamente exacto de cómo creamos esa figura.

En la práctica, la mayoría de las personas desarrollan scripts de shell ejecutando comandos en el indicador de shell varias veces
para asegurarse de que están haciendo lo correcto,
y a continuación los guardan en un archivo para su reutilización.
Este estilo de trabajo permite a la gente reciclar
lo que descubren sobre sus datos y su flujo de trabajo con una llamada a `history`
y con un poco de edición para limpiar la salida
pueden guardarlo como un script de shell.

## La tubería de Alicia: Creando un script

Un comentario extraño de su supervisor ha hecho que Alicia se dé cuenta de que
debería haber proporcionado un par de parámetros adicionales a `goostats` cuando procesó sus archivos.
Esto podría haber sido un desastre si hubiera hecho todo el análisis a mano,
pero gracias a los bucles `for`,
sólo le tomará un par de horas para volver a realizar el análisis.

Pero la experiencia le ha enseñado que si algo tiene que hacerse dos veces,
probablemente tendrá que hacerse una tercera o cuarta vez también.
Ella dirige el editor y escribe lo siguiente:

~~~
# Calculate reduced stats for data files at J = 100 c/bp.
for datafile in "$@"
do
    echo $datafile
    bash goostats -J 100 -r $datafile stats-$datafile
done
~~~
{: .bash}

(Los parámetros `-J 100` y `-r` son los que su supervisor dijo que debería haber usado.)
Ella guarda esto en un archivo llamado `do-stats.sh`
y ahora puede repetir la primera etapa de su análisis escribiendo:

~~~
$ bash do-stats.sh *[AB].txt
~~~
{: .bash}

Ella también puede hacer esto:

~~~
$ bash do-stats.sh *[AB].txt | wc -l
~~~
{: .bash}

de modo que la salida es sólo el número de archivos procesados
en lugar de los nombres de los archivos que se procesaron.

Una cosa a tener en cuenta sobre el script de Alicia es que
permite que la persona que lo ejecuta decida que archivos procesar.
Podría haberlo escrito así:

~~~
# Calculate reduced stats for  A and Site B data files at J = 100 c/bp.
for datafile in *[AB].txt
do
    echo $datafile
    bash goostats -J 100 -r $datafile stats-$datafile
done
~~~
{: .bash}

La ventaja es que esto siempre selecciona los archivos correctos:
Ella no tiene que recordar excluir los archivos 'Z'.
La desventaja es que *siempre* selecciona sólo esos archivos --- ella no puede ejecutarlo en todos los archivos
(incluidos los directorios 'Z'),
o en los archivos 'G' o 'H' que sus colegas de la Antártida están produciendo,
sin editar el script manualmente.
Si quisiera ser más aventurera,
Alicia podría modificar su script para comprobar los parámetros de la línea de comandos,
Y utilizar `*[AB].txt` si no se ha proporcionado ninguno.
Por supuesto, esto introduce otro equilibrio entre flexibilidad y complejidad.

> ## Variables en los scripts de shell
>
> En el directorio `molecules`, imagine que tiene un script de shell llamado `script.sh` que contiene los
> comandos siguientes:
>
> ~~~
> head -n $2 $1
> tail -n $3 $1
> ~~~
> {: .bash}
>
> Mientras esté en el directorio `molecules`, escriba el siguiente comando:
>
> ~~~
> bash script.sh '*.pdb' 1 1
> ~~~
> {: .bash}
>
> ¿Cuál de los siguientes resultados esperaría ver?
>
> 1. Todas las líneas entre la primera y la última línea de cada archivo que terminan en `.pdb`
> en el directorio `molecules`.
> 2. La primera y la última línea de cada archivo que termina en `.pdb` en el directorio `molecules`.
> 3. La primera y la última línea de cada archivo en el directorio `molecules`.
> 4. Un error debido a las comillas alrededor de `*.pdb`.
{: .challenge}

> ## Listando especies únicas
>
> Leah tiene varios cientos de archivos de datos, cada uno de los cuales tiene el formato siguiente:
>
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> 2013-11-06,fox,1
> 2013-11-07,rabbit,18
> 2013-11-07,bear,1
> ~~~
> {: .source}
>
> Escribe un script de shell llamado `species.sh` que tome cualquier número de
> nombres de archivos como parámetros de línea de comandos, y utilice `cut`, `sort` y
> `uniq` para imprimir una lista de las especies únicas que aparecen en cada uno de
> esos archivos por separado.
{: .challenge}

> ## Encuentre el archivo más largo con una extensión determinada
>
> Escribe un script de shell llamado `longest.sh` que tome el nombre de un
> directorio y una extensión de nombre de archivo como sus parámetros, e imprima
> el nombre del archivo con más líneas en ese directorio
> con esa extensión. Por ejemplo:
>
> ~~~
> $ bash longest.sh /tmp/data pdb
> ~~~
> {: .bash}
>
> Imprimirá el nombre del archivo `.pdb` en`/tmp/data` que tenga
> más líneas.
{: .challenge}

> ## ¿Por qué registrar comandos en la historia antes de ejecutarlos?
>
> Si ejecuta el comando:
>
> ~~~
> $ history | tail -n 5 > recent.sh
> ~~~
> {: .bash}
>
> El último comando del archivo es el comando `history` mismo, es decir,
> el shell ha añadido `history` al registro de comandos antes de
> ejecutarlo. De hecho, el shell *siempre* agrega comandos al registro
> antes de ejecutarlos. ¿Por qué cree que hace esto?
{: .challenge}



> ## Script Reading Comprehension
>
> El directorio `data` de Joel contiene tres archivos:` fructose.dat`,
> `glucose.dat` y `sucrose.dat`. Explica qué haría un script llamado
> `example.sh` cuando se ejecute como `bash example.sh *.dat` si
> contiene las líneas siguientes:
>
> ~~~
> # Script 1
> echo *.*
> ~~~
> {: .bash}
>
> ~~~
> # Script 2
> for filename in $1 $2 $3
> do
>     cat $filename
> done
> ~~~
> {: .bash}
>
> ~~~
> # Script 3
> echo $@.dat
> ~~~
> {: .bash}
{: .challenge}

> ## Depuración (debugging) de Scripts
>
> Supongamos que ha guardado el siguiente script en un archivo denominado `do-errors.sh`
> en el directorio `north-pacific-gyre/2012-07-03` de Alicia:
>
> ~~~
> # Calculate reduced stats for data files at J = 100 c/bp.
> for datafile in "$@"
> do
>     echo $datfile
>     bash goostats -J 100 -r $datafile stats-$datafile
> done
> ~~~
> {: .bash}
>
> Cuando lo ejecute:
>
> ~~~
> $ bash do-errors.sh *[AB].txt
> ~~~
> {: .bash}
>
> La salida está en blanco.
> Para averiguar por qué, vuelva a ejecutar el script utilizando la opción `-x`:
>
> ~~~
> bash -x do-errors.sh *[AB].txt
> ~~~
> {: .bash}
>
> ¿Cuál es el resultado que muestra?
> ¿Qué línea es responsable del error?
{: .challenge}
