---
title: "Scripts de la terminal"
teaching: 15
exercises: 0
questions:
- "¿Cómo puedo guardar y reutilizar comandos?"
objectives:
- "Escriba un **script** de la terminal que ejecute un comando o una serie de comandos para un conjunto fijo de archivos."
- "Ejecutar un **script** de la terminal desde la línea de comandos."
- "Escribir un **script** de la terminal que opere sobre un conjunto de archivos definidos por el usuario en línea de comandos."
- "Crear **pipelines** que incluyan **script** de la terminal que tú y otros hayan escrito."
keypoints:
- "Guardar comandos en archivos (normalmente llamados **scripts** de la terminal) para su reutilización."
- "`bash filename` ejecuta los comandos guardados en un archivo."
- "`$@`se refiere a todos los parámetros de la línea de comandos de un **script** de la terminal."
- "`$1`, `$2`, etc., se refieren al primer parámetro de la línea de comandos, al segundo parámetro de la línea de comandos, etc."
- "Coloque las variables entre comillas si los valores tienen espacios en ellas."
- "Dejar que los usuarios decidan qué archivos procesar es más flexible y más consistente con los comandos de Unix."
---

Finalmente estamos listos para ver lo que hace que la terminal sea un entorno de programación tan potente.
Vamos a tomar los comandos que repetimos con frecuencia y los vamos a guardar en archivos
de modo que podemos volver a ejecutar todas esas operaciones más tarde escribiendo un sólo comando.
Por razones históricas, a un conjunto de comandos guardados en un archivo se le llama un **script de la terminal**, pero no se equivoquen: estos son en realidad pequeños programas.

Comencemos por volver a `molecules/` creando un nuevo archivo, `middle.sh`, que se convertirá en nuestro **script** de la terminal:

~~~
$ cd molecules
$ nano middle.sh
~~~
{: .bash}

El comando `nano middle.sh` abre el archivo `middle.sh` dentro del editor de texto "nano" (que se ejecuta dentro de la terminal).
Si el archivo no existe, se creará. Podemos usar el editor de texto para editar directamente el archivo - simplemente insertaremos la siguiente línea:

~~~
head -n 15 octane.pdb | tail -n 5
~~~
{: .source}

Esta es una variación del **pipe** que construimos anteriormente: selecciona las líneas 11-15 del archivo `octane.pdb`. Recuerda, *no* lo estamos ejecutando como un comando todavía: sólo estamos poniendo los comandos en un archivo.

A continuación, guardamos el archivo (`Ctrl-O` en nano), y salimos del editor de texto (`Ctrl-X` en nano). Comprueba que el directorio `molecules` ahora contiene un archivo llamado `middle.sh`.

Una vez que hayamos guardado el archivo, podemos pedirle a la terminal que ejecute los comandos que contiene. Nuestra terminal se llama `bash`, por lo que ejecutamos el siguiente comando:

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

Efectivamente, la salida de nuestro **script** es exactamente lo que obtendríamos si ejecutamos ese **pipeline** directamente
en la terminal.

> ## Texto vs. Lo que sea
>
> Por lo general llamamos a programas como Microsoft Word o LibreOffice Writer "editores 
> de texto", pero tenemos que ser un poco más cuidadosos cuando se trata de
> programación. De forma predeterminada, Microsoft Word utiliza archivos .docx para almacenar no
> sólo texto, sino también formatear información sobre fuentes, encabezados,
> etcétera. Esta información adicional no se almacena como caracteres y no es comprendida por
> herramientas como `head`: los comandos esperan que los archivos de entrada contengan
> unicamente letras, dígitos y puntuación en un teclado de computadora estándar.
> Por lo tanto, al editar programas, debemos utilizar un editor de texto sin formatos o tener cuidado de guardar 
> nuestros archivos como texto sin formato.
>
{: .callout}

¿Qué pasa si queremos seleccionar líneas de un archivo arbitrario?
Podríamos editar `middle.sh` cada vez para cambiar el nombre de archivo,
pero eso probablemente llevaría más tiempo que simplemente volver a escribir el comando.
En cambio, editemos middle.sh y hagamos que sea más versátil:

~~~
$ nano middle.sh
~~~
{: .bash}

Ahora, dentro de "nano", reemplaza el texto `octane.pdb` con la variable especial denominada `$1`:

~~~
head -n 15 "$1" | tail -n 5
~~~
{: .output}

Dentro de un **script** de la terminal,`$1` significa "el primer nombre de archivo (u otro parámetro) en la línea de comandos".
Ahora podemos ejecutar nuestro **script** de esta manera:

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

o con un archivo diferente:

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

> ## Comillas dobles alrededor de parámetros
>
> Por la misma razón que ponemos la variable del bucle dentro de comillas dobles,
> cuando el nombre de archivo contenga espacios, rodeamos `$1` con comillas dobles.
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

Cambiando los parámetros de nuestra línea de comando, podemos cambiar el comportamiento del **script**:

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

Esto funciona,pero a la siguiente persona que use `middle.sh` se le puede dificultar ver lo que hace.
Podemos mejorar nuestro **script** agregando algunos **comentarios** en la parte superior:

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

Un comentario comienza con un caracter `#` y se ejecuta hasta el final de la línea. La computadora ignora los comentarios,
pero son invaluables para ayudar a la gente (incluyendote a ti en un futuro) a entender y usar **scripts**. La única advertencia es que cada vez que modifiques un **script**, debes comprobar que el comentario siga siendo preciso:
Una explicación que envía al lector en la dirección equivocada es peor que ninguna.

¿Qué sucede si queremos procesar muchos archivos en un solo pipeline?
Por ejemplo, si queremos ordenar nuestros archivos `.pdb` por longitud, deberíamos escribir:

~~~
$ wc -l *.pdb | sort -n
~~~
{: .bash}

dado que `wc -l` lista el número de líneas en los archivos (recuerda que `wc` significa 'word count', y si agregas el `-l` significa 'contar líneas') y `sort -n` ordena numéricamente.
Podríamos poner esto en un archivo, pero entonces sólo podría ordenar una lista de archivos `.pdb` en el directorio actual.
Si queremos ser capaces de obtener una lista ordenada de otros tipos de archivos, necesitamos una manera de conseguir estos nombres en el **script**. No podemos usar `$1`, `$2`, y así sucesivamente porque no sabemos cuántos archivos hay.
En su lugar, utilizamos la variable especial `$@`, lo que significa, "Todos los parámetros de la línea de comandos para el **script**". También debemos poner `$@` dentro de comillas dobles para el caso de parámetros que contienen espacios (`"$@"` es equivalente a `"$1"` `"$2"` ...). He aquí un ejemplo:

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
> ¿Qué pasa si se supone que un **script** procesa un grupo de archivos, pero nosotros
> no le damos ningún nombre de archivo? Por ejemplo, ¿qué pasa si escribimos?:
>
> ~~~
> $ bash sorted.sh
> ~~~
> {: .bash}
>
> pero sin agregar `*.dat` (o cualquier otra cosa)? En este caso, `$@` se expande a
> nada en absoluto, por lo que el **pipeline** dentro del **script** es efectivamente:
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

Supongamos que acabamos de ejecutar una serie de comandos que hicieron algo útil - por ejemplo, crearon un gráfico que nos gustaría utilizar en un documento. Nos gustaría poder volver a hacer el gráfico más tarde si es necesario, por lo que queremos guardar los comandos en un archivo. En lugar de escribirlos de nuevo (y potencialmente equivocarse) podemos hacer esto:

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

Después de un poco de trabajo en un editor para eliminar los números de serie en los comandos,
y eliminar la línea final donde llamamos el comando `history`, tenemos un registro completamente preciso de cómo creamos dicha figura.

En la práctica, la mayoría de las personas desarrollan **scripts** de la terminal ejecutando comandos en el prompt de dicha terminal varias veces para asegurarse de que están haciendo las cosas bien, y a continuación los guardan en un archivo para su reutilización. Este estilo de trabajo permite a la gente replicar lo que descubre sobre sus datos y su flujo de trabajo con una llamada a `history` y editando para limpiar la salida pueden guardarlo como un **script** de la terminal.

## El pipeline de Nelle: Creando un **script**

El supervisor de Nelle insistió en que todos sus análisis deben ser reproducibles. Nelle se da cuenta que debería haber proporcionado un par de parámetros adicionales a `goostats` cuando procesó sus archivos. Esto podría haber sido un desastre si hubiera hecho todo el análisis a mano, pero gracias a los bucles `for`, sólo le tomará un par de horas para volver a realizar el análisis. La forma más fácil de capturar todos los pasos es en un **script**. Ella ejecuta el editor y escribe lo siguiente:

~~~
# Calculate reduced stats for data files.
for datafile in "$@"
do
    echo $datafile
    bash goostats $datafile stats-$datafile
done
~~~
{: .bash}

Guarda esto en un archivo llamado `do-stats.sh` para que ahora pueda volver a hacer la primera etapa de su análisis escribiendo:

~~~
$ bash do-stats.sh NENE*[AB].txt
~~~
{: .bash}

Ella también puede hacer esto:

~~~
$ bash do-stats.sh NENE*[AB].txt | wc -l
~~~
{: .bash}

de modo que la salida es sólo el número de archivos procesados en lugar de los nombres de los archivos que se procesaron.

Una cosa a tener en cuenta sobre el **script** de Nelle es que permite que la persona que lo ejecuta decida que archivos procesar.
Podría haberlo escrito así:

~~~
# Calculate stats for Site A and Site B data files.
for datafile in NENE*[AB].txt
do
    echo $datafile
    bash goostats $datafile stats-$datafile
done
~~~
{: .bash}

La ventaja es que esto siempre selecciona los archivos correctos: Ella no tiene que recordar excluir los archivos 'Z'.
La desventaja es que *siempre* selecciona esos archivos - ella no puede ejecutarlo en todos los archivos (incluidos los archivos 'Z'), o en los archivos 'G' o 'H' que sus colegas de la Antártida están produciendo, sin editar el **script** manualmente.
Si quisiera ser más aventurera, Nelle podría modificar su **script** para verificar los parámetros en línea de comandos, y utilizar `NENE*[AB].txt` si no se ha proporcionado ninguno. Por supuesto, esto introduce otro equilibrio entre flexibilidad y complejidad.

> ## Variables en los **scripts** de la terminal
>
> En el directorio `molecules`, imagina que tienes un **script** de la terminal llamado `script.sh` que contiene los siguientes
> comandos:
>
> ~~~
> head -n $2 $1
> tail -n $3 $1
> ~~~
> {: .bash}
>
> Mientras estés en el directorio `molecules`, escribe el siguiente comando:
>
> ~~~
> bash script.sh '*.pdb' 1 1
> ~~~
> {: .bash}
>
> ¿Cuál de los siguientes resultados esperarías ver?
>
> 1. Todas las líneas entre la primera y la última línea de cada archivo que terminan en `.pdb`
> en el directorio `molecules`.
> 2. La primera y la última línea de cada archivo que termina en `.pdb` en el directorio `molecules`.
> 3. La primera y la última línea de cada archivo en el directorio `molecules`.
> 4. Un error debido a las comillas alrededor de `*.pdb`.
>
> ## Solución
>> La respuesta correcta es la 2.
>> Las variables $1, $2 y $3 representan los argumentos para el **script**, tal que los comandos ejecutados son:
>>
>> ~~~
>> $ head -n 1 cubane.pdb ethan ee.pdb octane.pdb pentane.pdb propane.pdb
>> $ tail -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
>> ~~~
>> La terminal no expande '* .pdb' porque está rodeado por comillas. El primer parámetro del **script** es '*.pdb', que se expande 
>> dentro del **script** desde el principio al final.
> {: .solution}
{: .challenge}

> ## Listando especies únicas
>
> Leah tiene varios cientos de archivos de datos, cada uno de los cuales tiene el siguiente formato :
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
> Un ejemplo de este tipo de archivo se puede ver en data-shell/data/animal-counts/animals.txt.
>
> Escribe un **script** de la terminal llamado `species.sh` que tome cualquier número de
> nombres de archivos como parámetros de línea de comandos, y utilice `cut`, `sort` y
> `uniq` para mostrar una lista de las especies únicas que aparecen en cada uno de
> esos archivos por separado.
> ## Solución
>> ~~~
>> # Script to find unique species in csv files where species is the second data field
>> # This script accepts any number of file names as command line arguments
>> 
>> # Loop over all files
>> for file in $@ 
>> do
>> 	echo "Unique species in $file:"
>> 	# Extract species names
>> 	cut -d , -f 2 $file | sort | uniq
>> done
>> ~~~
>{: .solution}
{: .challenge}
> ## Encuentre el archivo más largo con una extensión determinada
>
> Escribe un **script** de la terminal llamado `longest.sh` que tome el nombre de un
> directorio y una extensión de nombre de archivo como sus parámetros, e imprima
> el nombre del archivo con más líneas en ese directorio con esa extensión. Por ejemplo:
>
> ~~~
> $ bash longest.sh /tmp/data pdb
> ~~~
> {: .bash}
>
> Mostraría el nombre del archivo `.pdb` en`/tmp/data` que tenga más líneas.
>
> ## Solución
>> 
>> ~~~
>>  # Script que toma 2 parámetros: 
>>  #    1. un nombre de directorio
>>  #    2. una extensión de archivo
>>  # y muestra el nombre del archivo de ese directorio 
>>  # con las líneas que coincide con la extensión del archivo.
>>  
>> wc -l $1/*.$2 | sort -n | tail -n 2 | head -n 1
>> ~~~
>{: .solution}
{: .challenge}

> ## ¿Por qué grabar los comandos en el historial antes de ejecutarlos?
>
> Si ejecutas el comando:
>
> ~~~
> $ history | tail -n 5 > recent.sh
> ~~~
> {: .bash}
>
> El último comando del archivo es el mismo comando `history`, es decir,
> la terminal ha añadido `history` al registro de comandos antes de
> ejecutarlo. De hecho, la terminal *siempre* agrega comandos al registro
> antes de ejecutarlos. ¿Por qué crees que hace esto?
>
> ## Solución
>> 
>> Si un comando hace que algo se cuelgue, podría ser útil saber cuál era ese comando para saber cual fue el problema. 
>> Si el comando sólo se registra después de ejecutarlo, no tendríamos un registro del último comando ejecutado en caso
>> de un bloqueo.
>>
>{: .solution}
{: .challenge}

> ## Script de lectura y comprensión
>
> Considera el directorio data-shell/molecules una vez más. Este contiene una cantidad de archivos .pdb 
> además de otro archivo que hayas creado. Explica qué haría un **script** llamado example.sh cuando se ejecutara como bash
> example.sh * .pdb si contuviera las siguientes líneas:
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
>
> ## Solución
>> 
>> El **script** 1 mostrará una lista de todos los archivos que contienen un punto en su nombre.
>> 
>> El **script** 2 mostrará el contenido de los primeros 3 archivos que coinciden con la extensión del archivo. 
>> La terminal expande el caracter especial antes de pasar los argumentos al script example.sh.
>> 
>> El **script** 3 mostrará todos los parámetros del script (es decir, todos los archivos .pdb), seguido de .pdb. 
>> cubane.pdb etano.pdb metano.pdb octano.pdb pentano.pdb propano.pdb.pdb
>{: .solution}
{: .challenge}

> ## Depuración (debugging) de scripts
>
> Supongamos que ha guardado el siguiente **script** en un archivo denominado `do-errors.sh`
> en el directorio `north-pacific-gyre/2012-07-03` de Nelle:
>
> ~~~
> # Calcular las estadísticas de los archivos de datos.
> for datafile in "$@"
> do
>     echo $datfile
>     bash goostats $datafile stats-$datafile
> done
> ~~~
> {: .bash}
>
> Cuando lo ejecutas:
>
> ~~~
> $ bash do-errors.sh NENE*[AB].txt
> ~~~
> {: .bash}
>
> La salida está en blanco.
> Para averiguar por qué, vuelva a ejecutar el **script** utilizando la opción `-x`:
>
> ~~~
> bash -x do-errors.sh NENE*[AB].txt
> ~~~
> {: .bash}
>
> ¿Cuál es el resultado que muestra?
> ¿Qué línea es responsable del error?
>
> ## Solución
>> 
>> El indicador `-x` hace que `bash` se ejecute en modo de depuración. Esto muestra cada comando mientras se ejecuta, 
>> lo que te ayudará a localizar errores. En este ejemplo, podemos ver que `echo` no está mostrando nada. 
>> Hemos creado un error tipográfico en el nombre de la variable del bucle, la variable `datfile` no existe, 
>> por lo que devuelve una cadena vacía.
>> 
>{: .solution}
{: .challenge}
