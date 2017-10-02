---
title: "Bucles (loops)"
teaching: 15
exercises: 0
questions:
- "¿Cómo puedo realizar las mismas acciones en muchos archivos diferentes?"
objectives:
- "Escribe un bucle que aplique uno o más comandos por separado a cada archivo de un conjunto de archivos."
- "Trazar los valores tomados por una variable de bucle durante la ejecución del bucle."
- "Explicar la diferencia entre el nombre de una variable y su valor."
- "Explicar por qué los espacios y algunos caracteres de puntuación no deben usarse en los nombres de archivo."
- "Demostrar cómo ver qué comandos han sido ejecutados recientemente."
- "Volver a ejecutar comandos ejecutados recientemente sin volver a escribirlos."
keypoints:
- "Un bucle `for` repite comandos una vez para cada elemento de una lista."
- "Cada bucle `for` necesita una variable para referirse al elemento en el que está trabajando actualmente."
- "Uso de `$name` para expandir una variable (es decir, obtener su valor). También se puede usar `${name}`."
- "No utilice espacios, comillas o caracteres comodín como '*' o '?' en nombres de directorios, ya que complica la expansión variable."
- "Proporcionar a los archivos nombres coherentes que sean fáciles de combinar con los patrones comodín para facilitar la selección de los bucles."
- "Utilice la tecla de flecha hacia arriba para desplazarse hacia arriba a través de comandos anteriores para editarlos y repetirlos."
- "Usar` Ctrl-R` para buscar a través de los comandos previamente introducidos. "
- "Use `history` para mostrar comandos recientes, y `!number` para repetir un comando por número."
---

Los **Loops** son fundamentales para mejorar la productividad a través de la automatización, ya que nos permiten ejecutar
comandos repetitivos. Al igual que los comodines y la terminación de fichas, el uso de bucles también
reduce la cantidad de tecleo (y los errores de escritura/dedo).
Supongamos que tenemos varios cientos de archivos de datos del genoma denominados `basilisk.fasta`,` unicorn.fasta` y así sucesivamente.
En este ejemplo,
usaremos el directorio `creatures` que sólo tiene dos archivos de ejemplo,
pero los principios se pueden aplicar a muchos muchos más archivos a la vez.
Nos gustaría modificar estos archivos, pero también guardar una versión de los archivos originales, nombrar las copias
`original-basilisk.fasta` y` original-unicorn.fasta`.
No podemos usar:

~~~
$ cp *.fasta original-*.fasta
~~~
{: .bash}

Porque se expandiría a:

~~~
$ cp basilisk.fasta unicorn.fasta original-*.fasta
~~~
{: .bash}

Esto no respalda nuestros archivos, en su lugar obtenemos un error:

~~~
cp: target `original-*.fasta' is not a directory
~~~
{: .error}

Este problema surge cuando `cp` recibe más de dos entradas. Cuando esto sucede,
espera que la última entrada sea un directorio donde pueda copiar todos los archivos que se le pasaron.
Dado que no existe ningún directorio denominado `original-*.fasta` en el directorio `creatures` genera un
error.

En cambio, podemos usar un **bucle**
para hacer alguna operación una vez para cada cosa en una lista.
Aquí hay un ejemplo sencillo que muestra las tres primeras líneas de cada archivo de una sola vez:

~~~
$ for filename in basilisk.fasta unicorn.fasta
> do
>    head -n 3 $filename
> done
~~~
{: .bash}

~~~
>Basiliskus_viborinious_genome_v1
GAAATGGATTTAAAATAATAAAAAAACTTTTAGGTTAACAAGTAGAGGGTGACTCAAAAGGTCGTTAAGA
GACGATTTTTTTGAGGCACCTCTTTTTTATTATTATTTTAGAATTTTCACATCGGTTCATTTTGATTAGG
>Unicornius_imaginario_genome_v1
TTTGGGCCCTTTGTTCGCCTGCTCGCCGTCTAGCGCGAGCCGCCCATCTCGTTGTCGTCCTCACTCGCAG
GCCTCCCCCTTCCCGACGAACTCGTCGCCATGATCCTCTCTCATCTCGATGAACACGACGCCGTTCCTGC
~~~
{: .output}

Cuando el shell ve la palabra clave `for`,
sabe que debe repetir un comando (o grupo de comandos) una vez para cada cosa `in` (en) una lista.
Para cada iteración,
el nombre de cada cosa se asigna secuencialmente a
la **variable** y los comandos dentro del bucle se ejecutan antes de pasar a
la siguiente cosa en la lista.
Dentro del bucle,
pedimos el valor de la variable poniendo `$` delante de ella.
El `$` le dice al intérprete de shell que trate
la **variable** como un nombre de variable y sustituir su valor en su lugar,
en lugar de tratarlo como texto o como un comando externo.

En este ejemplo, la lista tiene dos nombres de archivo: `basilisk.fasta` y` unicorn.fasta`.
Cada vez que el bucle se itera, asignará un nombre de archivo a la variable `filename`
y ejecutará el comando `head`.
La primera vez en el bucle,
`$filename` es` basilisk.fasta`.
El intérprete ejecuta el comando `head` en `basilisk.fasta`,
e imprime las
primeras tres líneas de `basilisk.fasta`.
Para la segunda iteración, `$filename` se convierte en
`unicorn.fasta`. Esta vez, el shell ejecuta `head` en` unicorn.fasta`
e imprime las tres primeras líneas de `unicorn.fasta`.
Dado que la lista era de sólo dos elementos, la shell sale del bucle `for`.

Cuando se utilizan variables, también es
posible poner los nombres en llaves para delimitar claramente la variable
nombre: `$filename` es equivalente a` ${filename}`, pero es diferente de
`${file}name`. Puede encontrar esta notación en los programas de otras personas.

> ## Siga las instrucciones
>
> El indicador de comandos cambia de `$` a `>` y de nuevo a `$`
> conforme escribimos nuestro bucle. El segundo indicador, `>`, es diferente para recordarnos
> que todavía no hemos terminado de escribir un comando completo. Un punto y coma, `;`,
> se puede utilizar para separar dos comandos escritos en una sola línea.
{: .callout}

> ## Los mismos símbolos, diferentes significados
>
> Aquí vemos `>` siendo utilizado como un prompt de shell, mientras que `>` también se
> utiliza para redirigir la salida.
> De forma similar, `$` se utiliza como indicador de shell, pero, como vimos antes,
> también se utiliza para pedir que el shell obtenga el valor de una variable.
>
> Si el *shell* imprime `>` o `$` entonces espera que se le escriba algo,
> y el símbolo es un prompt.
>
> Si *usted* escribe `>` o `$`, es una instrucción de usted para
> el shell indicandole redirigir la salida o obtener el valor de una variable.
{: .callout}

Hemos llamado a la variable en este bucle `filename`
con el fin de hacer su propósito más claro para los lectores humanos.
El shell en sí no le importa lo que la variable se llama;
si escribimos este bucle como:

~~~
for x in basilisk.fasta unicorn.fasta
do
    head -n 3 $x
done
~~~
{: .bash}

or:

~~~
for temperature in basilisk.fasta unicorn.fasta
do
    head -n 3 $temperature
done
~~~
{: .bash}

Funcionaría exactamente de la misma manera.
*No lo haga así.*
Los programas sólo son útiles si la gente puede entenderlos,
nombres tan crípticos (como `x`) o nombres engañosos (como` temperature`)
aumentan las probabilidades de que el programa no haga lo que sus lectores piensan que hace.

He aquí un bucle un poco más complicado:

~~~
for filename in *.fasta
do
    echo $filename
    head -n 100 $filename | tail -n 20
done
~~~
{: .bash}

El shell comienza expandiendo `*.fasta` para crear la lista de archivos que procesará.
Después de esto, el **cuerpo de bucle**
ejecuta dos comandos para cada uno de esos archivos.
El primero, `echo`, simplemente imprime sus parámetros de línea de comandos a la salida estándar.
Por ejemplo:

~~~
$ echo hello there
~~~
{: .bash}

prints:

~~~
hello there
~~~
{: .output}

En este caso,
Ya que el shell expande `$filename` para que sea el nombre de un archivo,
`echo $ filename` sólo imprime el nombre del archivo.
Tenga en cuenta que no podemos escribir esto como:

~~~
for filename in *.fasta
do
    $filename
    head -n 100 $filename | tail -n 20
done
~~~
{: .bash}

porque entonces la primera vez a través del bucle,
cuando `$ filename` se expandió a `basilisk.fasta`, el shell intentaría ejecutar `basilisk.fasta` como un programa.
Finalmente,
la combinación `head` y `tail` selecciona las líneas 81-100
de cualquier archivo que se esté procesando
(suponiendo que el archivo tiene al menos 100 líneas).

> ## Espacios en los nombres
>
> El espacio en blanco se utiliza para separar los elementos de la lista
> que vamos a usar en el bucle. Si en la lista tenemos elementos
> con espacios en blanco necesitamos citar esos elementos
> y nuestra variable al usarlo.
> Supongamos que nuestros archivos de datos tienen el nombre:
>
> ~~~
> red dragon.fasta
> purple unicorn.fasta
> ~~~
> {: .source}
>
> Necesitamos usar
>
> ~~~
> for filename in "red dragon.fasta" "purple unicorn.fasta"
> do
>     head -n 100 "$filename" | tail -n 20
> done
> ~~~
> {: .bash}
>
> Es más sencillo simplemente evitar el uso de espacios en blanco (u otros caracteres especiales) en los nombres de archivo.
{: .callout}

Volviendo a nuestro problema original de copia de archivos,
Podemos resolverlo usando este bucle:

~~~
for filename in *.fasta
do
    cp $filename original-$filename
done
~~~
{: .bash}

![Bucle For en acción](../fig/shell_script_for_loop_flow_chart.svg)

Este bucle ejecuta el comando `cp` una vez para cada nombre de archivo.
La primera vez,
cuando `$filename` se expande a `basilisk.fasta`,
El shell ejecuta:

~~~
cp basilisk.fasta original-basilisk.fasta
~~~
{: .bash}

La segunda vez, el comando es:

~~~
cp unicorn.fasta original-unicorn.fasta
~~~
{: .bash}

## Pipeline de Alicia: Procesando Archivos

Alicia está ahora lista para procesar sus archivos de datos.
Dado que todavía está aprendiendo cómo utilizar la shell,
decide construir los comandos requeridos en etapas.
Su primer paso es asegurarse de que puede seleccionar los archivos correctos --- recuerde,
estos son aquellos cuyos nombres terminan en 'A' o 'B', en lugar de 'Z'. A partir de su directorio personal, Alicia teclea:

~~~
$ cd north-pacific-gyre/2012-07-03
$ for datafile in *[AB].txt
> do
>     echo $datafile
> done
~~~
{: .bash}

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
~~~
{: .output}

Su siguiente paso es decidir
cómo llamar a los archivos que creará el programa de análisis `goostats`.
Prefijar el nombre de cada archivo de entrada con "stats" parece simple,
así que modifica su bucle para hacer eso:

~~~
$ for datafile in *[AB].txt
> do
>     echo $datafile stats-$datafile
> done
~~~
{: .bash}

~~~
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
~~~
{: .output}

Ella todavía no ha ejecutado `goostats`,
pero ahora está segura de que puede seleccionar los archivos correctos y generar los nombres de archivo de salida correctos.

Escribir comandos una y otra vez es cada vez más tedioso,
y a Alicia le preocupa no cometer errores,
Así que en lugar de volver a entrar en su bucle,
presiona la flecha hacia arriba.
En respuesta,
El shell vuelve a mostrar el bucle completo en una línea
(usando puntos y comas para separar las piezas):

~~~
$ for datafile in *[AB].txt; do echo $datafile stats-$datafile; done
~~~
{: .bash}

Utilizando la tecla de flecha izquierda,
Alicia realiza una copia de seguridad y cambia el comando `echo` a `bash goostats`:

~~~
$ for datafile in *[AB].txt; do bash goostats $datafile stats-$datafile; done
~~~
{: .bash}

Cuando presiona Enter,
el shell ejecuta el comando modificado.
Sin embargo, nada parece suceder --- no hay salida.
Después de un momento, Alicia se da cuenta de que ya que su guión ya no imprime nada a la pantalla,
ella no tiene ni idea de si está funcionando, y mucho menos con qué rapidez.
Mata el comando de ejecución escribiendo `Ctrl-C`,
e utiliza la flecha hacia arriba para repetir el comando,
y lo edita para que lea:

~~~
$ for datafile in *[AB].txt; do echo $datafile; bash goostats $datafile stats-$datafile; done
~~~
{: .bash}

> ## Principio y Fin
>
> Podemos pasar al principio de una línea en el shell escribiendo `Ctrl-A`
> y al final usando `Ctrl-E`.
{: .callout}

Ahora, cuando ejecuta su programa,
produce una línea de salida cada cinco segundos aproximadamente:

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
~~~
{: .output}

1518 veces 5 segundos,
dividido por 60,
le dice que su script tomará alrededor de dos horas en terminar de analizar todos los archivos.
Como un chequeo final,
abre otra ventana terminal,
entra en `north-pacific-gyre/2012-07-03`,
e utiliza `cat stats-NENE01729B.txt`
para examinar uno de los archivos de salida.
Se ve bien,
así que decide tomarse un café y ponerse al día con su lectura.

> ## Aquellos que saben que la historia puede elegir repetirla
>
> Otra forma de repetir el trabajo anterior es usar el comando `history` para
> obtener una lista de los últimos cientos de comandos que se han ejecutado, y
> entonces para usar `!123` (donde" 123 "es reemplazado por el número de comando) para
> repetir uno de esos comandos. Por ejemplo, si Alicia escribe esto:
>
> ~~~
> $ history | tail -n 5
> ~~~
> {: .bash}
> ~~~
>   456  ls -l NENE0*.txt
>   457  rm stats-NENE01729B.txt.txt
>   458  bash goostats NENE01729B.txt stats-NENE01729B.txt
>   459  ls -l NENE0*.txt
>   460  history
> ~~~
> {: .output}
>
> Entonces puede volver a ejecutar `goostats` en` NENE01729B.txt` simplemente escribiendo
> `!458`.
{: .callout}

> ## Otros comandos de historial
>
> Existen varios otros comandos de acceso directo para acceder al historial.
> Dos de los más útiles son `!!`, que recupera el
> comando anterior (puede o no encontrar esto más conveniente que
> usar simplemente la flecha hacia arriba), y `!$`, que recupera la última palabra del último
> comando. Es útil más a menudo de lo que cabría esperar: después de
> `bash goostats NENE01729B.txt stats-NENE01729B.txt`, puede escribir
> `less !$` Para ver el archivo `stats-NENE01729B.txt`, que es
> más rápido que hacer la flecha hacia arriba y editar la línea de comandos.
{: .callout}

> ## Variables en bucles
>
> Supongamos que `ls` muestra inicialmente:
>
> ~~~
> fructose.fasta    glucose.fasta   sucrose.fasta
> ~~~
> {: .output}
>
> ¿Cuál es la salida de?:
>
> ~~~
> for datafile in *.fasta
> do
>     ls *.fasta
> done
> ~~~
> {: .bash}
>
> Ahora, ¿cuál es la salida de?:
>
> ~~~
> for datafile in *.fasta
> do
>	ls $datafile
> done
> ~~~
> {: .bash}
>
> ¿Por qué estos dos bucles dan resultados diferentes?
{: .challenge}


> ## Guardar en un archivo en un bucle - Primera parte
>
> En el mismo directorio, ¿cuál es el efecto de este bucle?
>
> ~~~
> for sugar in *.fasta
> do
>     echo $sugar
>     cat $sugar > xylose.fasta
> done
> ~~~
> {: .bash}
>
> 1. Imprime `fructose.fasta`,` glucose.fasta` y `sucrose.fasta`, y el texto de `sucrose.fasta` se guardará en un archivo llamado `xylose.fasta`.
> 2. Imprime `fructose.fasta`, `glucose.fasta` y `sucrose.fasta`, y el texto de los tres archivos sería
> concatenado y guardado en un archivo llamado `xylose.fasta`.
> 3. Imprime `fructose.fasta`, `glucose.fasta`, `sucrose.fasta`, y
> `xylose.fasta`, y el texto de `sucrose.fasta` se guardará en un archivo llamado `xylose.fasta`.
> 4. Ninguna de las anteriores.
{: .challenge}

> ## Guardar en un archivo en un bucle - Segunda parte
>
> En otro directorio, donde `ls` devuelve:
>
> ~~~
> fructose.fasta    glucose.fasta   sucrose.fasta   maltose.txt
> ~~~
> {: .output}
>
> ¿Cuál sería la salida del siguiente bucle?
>
> ~~~
> for datafile in *.fasta
> do
>     cat $datafile >> sugar.fasta
> done
> ~~~
> {: .bash}
>
> 1. Todo el texto de `fructose.fasta`, `glucose.fasta` y `sucrose.fasta` sería
> concatenado y guardado en un archivo llamado `sugar.fasta`.
> 2. El texto de `sucrose.fasta` se guardará en un archivo llamado `sugar.fasta`.
> 3. Todo el texto de `fructose.fasta`, `glucose.fasta`, `sucrose.fasta` y `maltose.txt`
> sería concatenado y guardado en un archivo llamado `sugar.fasta`.
> 4. Se imprimirá todo el texto de `fructose.fasta`,` glucose.fasta` y `sucrose.fasta`
> a la pantalla y guardado en un archivo llamado `sugar.fasta`.
{: .challenge}

> ## Limitación de conjuntos de archivos
>
> En el mismo directorio, donde `ls` muestra (sin el archivo `sugar.fasta`):
>
> ~~~
> fructose.fasta    glucose.fasta   sucrose.fasta   maltose.txt
> ~~~
> {: .output}
>
> ¿Cuál sería la salida del siguiente bucle?
>
> ~~~
> for filename in s*
> do
>     ls $filename 
> done
> ~~~
> {: .bash}
>
> 1. No se muestran archivos.
> 2. Todos los archivos están listados.
> 3. Sólo se enumeran `fructose.fasta`, `glucose.fasta` y `maltose.txt`.
> 4. Solamente se encuentra `sucrose.fasta`.
>
> ¿Cómo diferiría el resultado usando el siguiente comando?
>
> ~~~
> for filename in *s*
> do
>     ls $filename 
> done
> ~~~
> {: .bash}
>
> 1. Se listarían los mismos archivos.
> 2. Todos los archivos se enumeran esta vez.
> 3. Esta vez no se listan archivos.
> 4. El archivo `sucrose.fasta` aparecerá en la lista dos veces, con los otros archivos listados una vez cada uno.
{: .challenge}

> ## Haciendo una ejecución "en seco" (dry-run)
>
> Un bucle es una forma de hacer muchas cosas a la vez --- o de cometer muchos errores a
> la vez si se de forma incorrecta. Una manera de comprobar lo que un bucle *hará* 
> es hacer eco de los comandos que ejecutará en lugar de ejecutarlos la primera vez.
>
> Supongamos que queremos obtener una vista previa de los comandos que ejecutará el siguiente bucle
> sin ejecutar realmente esos comandos:
>
> ~~~
> for file in *.fasta
> do
>   analyze $file > analyzed-$file
> done
> ~~~
> {: .bash}
>
> ¿Cuál es la diferencia entre los dos bucles abajo, y cuál 
> deberíamos usar?
>
> ~~~
> # Version 1
> for file in *.fasta
> do
>   echo analyze $file > analyzed-$file
> done
> ~~~
> {: .bash}
>
> ~~~
> # Version 2
> for file in *.fasta
> do
>   echo "analyze $file > analyzed-$file"
> done
> ~~~
> {: .bash}
{: .challenge}

> ## Bucle anidado
>
> Supongamos que queremos configurar una estructura de directorios para organizar
> algunos experimentos que miden la tasa de crecimiento en medio con diferentes tipos de azúcar
> *y* temperaturas diferentes. ¿Cuál sería el
> resultado del siguiente código?:
>
> ~~~
> for sugar in fructose glucose sucrose
> do
>     for temperature in 25 30 37 40
>     do
>         mkdir $sugar-$temperature
>     done
> done
> ~~~
> {: .bash}
{: .challenge}
