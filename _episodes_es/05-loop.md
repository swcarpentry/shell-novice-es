---
Título: "Bucles"
Lección: 15
Ejercicios: 0
Preguntas:
- "¿Cómo puedo realizar las mismas acciones en varios archivos diferentes?"
Objetivos:
- "Escribir un bucle que aplique uno o más comandos por separado a cada archivo de un conjunto de archivos."
- "Rastrear los valores tomados por una variable de bucle durante la ejecución del bucle."
- "Explicar la diferencia entre el nombre de una variable y su valor."
- "Explicar por qué los espacios y algunos caracteres de puntuación no deben usarse en nombres de archivo."
- "Demostrar cómo ver qué comandos han sido ejecutados recientemente."
- "Volver a ejecutar comandos ejecutados recientemente sin volver a escribirlos."
Conceptos clave:
- "Un bucle `for` repite comandos una vez para cada elemento de una lista."
- "Cada bucle `for` necesita una variable para referirse al elemento en el que está trabajando actualmente."
- "Uso de `$name` para expandir una variable (es decir, obtener su valor). También se puede usar `${name}`."
- "No utilizar espacios, comillas o caracteres especiales como '*' o '?' en nombres de directorios, ya que complica la expansión de variables."
- "Proporcionar a los archivos nombres coherentes que sean fáciles de combinar con los caracteres especiales para facilitar la selección de los bucles."
- "Utilizar la tecla de flecha hacia arriba para desplazarse por los comandos anteriores para editarlos y repetirlos."
- "Usar` Ctrl-R` para buscar a través de los comandos previamente introducidos. "
- "Usar `history` para mostrar comandos recientes, y `!number` para repetir un comando por número."
---

Los **bucles** (loops en inglés) son fundamentales para mejorar la productividad a través de la automatización, ya que nos permiten ejecutar
comandos de forma repetitiva. Al igual que los caracteres especiales y el autocompletado, el uso de bucles también
reduce la cantidad de tecleo (y los errores de escritura/dedo).
Suponte que tenemos varios cientos de archivos con datos genómicos denominados `basilisk.dat`,` unicorn.dat` y así sucesivamente.
En este ejemplo,
usaremos el directorio `creatures` que sólo tiene dos archivos de ejemplo,
pero los principios se pueden aplicar a muchos muchos más archivos a la vez.
Nos gustaría modificar estos archivos, pero también guardar una versión de los archivos originales, nombrando las copias
`original-basilisk.dat` y` original-unicorn.dat`.
No podemos usar:

~~~
$ cp *.dat original-*.dat
~~~
{: .bash}

Porque se expandiría a:

~~~
$ cp basilisk.dat unicorn.dat original-*.dat
~~~
{: .bash}

Esto no respalda nuestros archivos, en su lugar obtenemos un error:

~~~
cp: target `original-*.dat' is not a directory
~~~
{: .error}

Este problema surge cuando `cp` recibe más de dos argumentos de entrada. Cuando esto sucede,
espera que la última entrada sea un directorio donde pueda copiar todos los archivos que se le pasaron.
Dado que no existe ningún directorio denominado `original-*.fasta` en el directorio `creatures`, se genera un
error.

En cambio, podemos usar un **bucle**
para ejecutar una operación a la vez sobre cada cosa en una lista.
Aquí un ejemplo sencillo que muestra las tres primeras líneas de cada archivo de una sola vez:

~~~
$ for filename in basilisk.dat unicorn.dat
> do
>    head -n 3 $filename
> done
~~~
{: .bash}

~~~
COMMON NAME: basilisk
CLASSIFICATION: basiliscus vulgaris
UPDATED: 1745-05-02
COMMON NAME: unicorn
CLASSIFICATION: equus monoceros
UPDATED: 1738-11-24
~~~
{: .output}

Cuando la terminal reconoce la palabra clave `for`,
sabe que debe repetir un comando (o grupo de comandos) una vez para cada elemento de una lista.
Cada vez que el bucle se ejecuta (llamada iteración), un elemento de la lista es asignado secuencialmente a una **variable**, los comandos dentro del bucle se ejecutan antes de pasar al siguiente elemento de la lista.
Dentro del bucle,
pedimos el valor de la variable poniendo `$` delante de ella.
El `$` le dice al intérprete de la terminal que trate
la **variable** como un nombre de variable y substituya su valor,
en lugar de tratarlo como texto o como un comando externo.

En este ejemplo, la lista tiene dos nombres de archivo: `basilisk.dat` y` unicorn.dat`.
Cada vez que el bucle se itera, asignará un nombre de archivo a la variable `filename`
y ejecutará el comando `head`.
La primera vez en el bucle,
`$filename` es` basilisk.dat`.
El intérprete ejecuta el comando `head` en `basilisk.dat`,
e imprime las
primeras tres líneas de `basilisk.dat`.
Para la segunda iteración, `$filename` se convierte en
`unicorn.dat`. Esta vez, la terminal ejecuta `head` en` unicorn.dat`
e imprime las tres primeras líneas de `unicorn.dat`.
Dado que la lista era sólo de dos elementos, la terminal sale del bucle `for`.

Cuando se utilizan variables, también es
posible poner sus nombres en llaves para delimitar claramente el nombre de la variable: `$filename` es equivalente a` ${filename}`, pero es diferente de
`${file}name`. Puede ser que encuentres esta notación en programas escritos por otras personas.

> ## Siga las instrucciones
>
> El **prompt** de la terminal cambia de `$` a `>` y de nuevo a `$`
> conforme escribimos nuestro bucle. El segundo **prompt**, `>`, es diferente para recordarnos
> que todavía no hemos terminado de escribir un comando completo. Un punto y coma, `;`,
> se puede utilizar para separar dos comandos escritos en una sola línea.
{: .callout}

> ## Los mismos símbolos, diferentes significados
>
> Aquí vemos `>` siendo utilizado como un prompt de la terminal, mientras que `>` también se
> utiliza para redirigir la salida de un comando.
> De forma similar, `$` se utiliza como prompt de la terminal, pero, como vimos antes,
> también se utiliza para pedir que la terminal obtenga el valor de una variable.
>
> Si la *terminal* imprime `>` o `$` entonces espera que escribas algo,
> y el símbolo es un prompt.
>
> Si *tú* escribes `>` o `$`, es una instrucción de ti para
> la terminal indicándole redirigir la salida o obtener el valor de una variable.
{: .callout}

Hemos llamado a la variable en este bucle `filename`
con el fin de hacer su propósito más claro para los lectores humanos.
A la terminal no le importa el nombre de la variable;
si escribimos este bucle como:

~~~
for x in basilisk.dat unicorn.dat
do
    head -n 3 $x
done
~~~
{: .bash}

or:

~~~
for temperature in basilisk.dat unicorn.dat
do
    head -n 3 $temperature
done
~~~
{: .bash}

Funcionaría exactamente de la misma manera.
*No lo hagas así.*
Los programas sólo son útiles si la gente puede entenderlos,
nombres crípticos (como `x`) o nombres engañosos (como` temperature`)
aumentan las probabilidades de que el programa no haga lo que sus lectores piensan que hace.

He aquí un bucle un poco más complicado:

~~~
for filename in *.dat
do
    echo $filename
    head -n 100 $filename | tail -n 20
done
~~~
{: .bash}

La terminal comienza expandiendo `*.dat` para crear la lista de archivos que procesará.
Después de esto, el **cuerpo del bucle**
ejecuta dos comandos para cada uno de esos archivos.
El primero, `echo`, simplemente imprime sus parámetros de línea de comandos a la salida estándar.
Por ejemplo:

~~~
$ echo hello there
~~~
{: .bash}

regresa:

~~~
hello there
~~~
{: .output}

En este caso, ya que la terminal expande `$filename` para que sea el nombre de un archivo,
`echo $filename` sólo imprime el nombre del archivo.
Ten en cuenta que no podemos escribir esto como:

~~~
for filename in *.dat
do
    $filename
    head -n 100 $filename | tail -n 20
done
~~~
{: .bash}

porque entonces la primera vez a través del bucle,
cuando `$filename` se expande a `basilisk.dat`, la terminal intentará ejecutar `basilisk.dat` como un programa. Finalmente, la combinación `head` y `tail` selecciona las líneas 81-100
de cualquier archivo que se esté procesando
(suponiendo que el archivo tiene al menos 100 líneas).

> ## Espacios en los nombres
>
> El espacio en blanco se utiliza para separar los elementos de la lista
> que vamos a usar en el bucle. Si en la lista tenemos elementos
> con espacios en blanco necesitamos entrecomillar esos elementos
> y nuestra variable al usarlo.
> Supongamos que nuestros archivos de datos se llaman:
>
> ~~~
> red dragon.dat
> purple unicorn.dat
> ~~~
> {: .source}
>
> Necesitamos usar
>
> ~~~
> for filename in "red dragon.dat" "purple unicorn.dat"
> do
>     head -n 100 "$filename" | tail -n 20
> done
> ~~~
> {: .bash}
>
> Es más sencillo simplemente evitar el uso de espacios en blanco (u otros caracteres especiales) en los nombres de archivo.
>
> Los archivos mencionados no existen, si corremos el código del último ejemplo, el comando `head` será incapaz de encontrarlos, sin embargo el mensaje de error regresará el nombre de los archivos que espera:
>
> ~~~
> head: cannot open ‘red dragon.dat’ for reading: No such file or directory
> head: cannot open ‘purple unicorn.dat’ for reading: No such file or directory
> ~~~
> {: .output} Intenta remover las comillas en `$filename` en el bucle anterior para observar el efecto de las comillas en los espacios en blanco:
> ~~~
> head: cannot open ‘red’ for reading: No such file or directory
> head: cannot open ‘dragon.dat’ for reading: No such file or directory
> head: cannot open ‘purple’ for reading: No such file or directory
> head: cannot open ‘unicorn.dat’ for reading: No such file or directory
> ~~~
> {: .output} {: .callout}

Volviendo a nuestro problema original de copia de archivos,
Podemos resolverlo usando este bucle:

~~~
for filename in *.dat
do
    cp $filename original-$filename
done
~~~
{: .bash}

Este bucle ejecuta el comando `cp` una vez para cada nombre de archivo.
La primera vez, cuando `$filename` se convierte en `basilisk.dat`,
la terminal ejecuta:

~~~
cp basilisk.dat original-basilisk.dat
~~~
{: .bash}

La segunda vez, el comando es:

~~~
cp unicorn.dat original-unicorn.dat
~~~
{: .bash}

Como el comando `cp` no produce una salida normalmente, es difícil verificar que el bucle está funcionando de forma correcta. Al antecederle `echo` es posibe ver cada comando como *sería* ejecutado. El siguiente diagrama muestra lo que sucede cuando el código modificado es ejecutado, y demuestra cómo el uso de `echo` es una práctica útil.

![Bucle For en acción](../fig/shell_script_for_loop_flow_chart.svg)


## Pipeline de Nelle: Procesando Archivos

Nelle ahora está lista para procesar sus archivos de datos. Dado que todavía está aprendiendo cómo utilizar la terminal, decide construir los comandos requeridos en etapas.
Su primer paso es asegurarse de que puede seleccionar los archivos correctos (recuerda,
aquellos cuyos nombres terminan en 'A' o 'B', en lugar de 'Z'). Posicionada en su directorio home, Nelle teclea:

~~~
$ cd north-pacific-gyre/2012-07-03
$ for datafile in NENE*[AB].txt
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

Su siguiente paso es decidir cómo llamar a los archivos que creará el programa de análisis `goostats`.
Prefijar el nombre de cada archivo de entrada con "stats" parece simple,
así que modifica su bucle para hacer eso:

~~~
$ for datafile in NENE*[AB].txt
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
y a Nelle le preocupa cometer errores, así que en lugar de volver a crear su bucle,
presiona la flecha hacia arriba. En respuesta, la terminal vuelve a mostrar el bucle completo en una línea (usando puntos y comas para separar las piezas):

~~~
$ for datafile in NENE*[AB].txt; do echo $datafile stats-$datafile; done
~~~
{: .bash}

Utilizando la tecla de flecha izquierda, Nelle realiza una copia de seguridad y cambia el comando `echo` a `bash goostats`:

~~~
$ for datafile in NENE*[AB].txt; do bash goostats $datafile stats-$datafile; done
~~~
{: .bash}

Cuando presiona Enter, la terminal ejecuta el comando modificado.
Sin embargo, nada parece suceder porque no hay salida.
Después de un momento, Nelle se da cuenta de que, ya que su script no imprime nada a la pantalla,
no tiene ni idea de si está funcionando, y mucho menos con qué rapidez.
Mata el comando de ejecución escribiendo `Ctrl-C`,
e utiliza la flecha hacia arriba para repetir el comando,
y lo edita para que se vea así:

~~~
$ for datafile in NENE*[AB].txt; do echo $datafile; bash goostats $datafile stats-$datafile; done
~~~
{: .bash}

> ## Principio y Fin
>
> Podemos pasar al principio de una línea en la terminal escribiendo `Ctrl-A`
> y al final usando `Ctrl-E`. {: .callout}

Ahora, cuando ejecuta su programa, produce una línea de salida cada cinco segundos aproximadamente:

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
~~~
{: .output}

1518 veces 5 segundos, dividido entre 60, le dice que su **script** tomará alrededor de dos horas en terminar de analizar todos los archivos.
Como un chequeo final, abre otra ventana de terminal,
entra en `north-pacific-gyre/2012-07-03`,
e utiliza `cat stats-NENE01729B.txt`
para examinar uno de los archivos de salida.
Se ve bien, así que decide tomarse un café y ponerse al día con su lectura.

> ## Aquellos que conocen la historia pueden elegir repetirla
>
> Otra forma de repetir el trabajo anterior es usar el comando `history` para
> obtener una lista de los últimos cientos de comandos que se han ejecutado, y
> usar `!123` (donde" 123 "es reemplazado por el número de comando) para
> repetir uno de esos comandos. Por ejemplo, si Nelle escribe esto:
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
> `!458`. {: .callout}

> ## Otros comandos del historial
>
> Existen otros comandos de acceso directo para acceder al historial:
> - `Ctrl-R` permite buscar en el historial en un modo denominado "búsqueda reversa" ("reverse-i-search" en inglés), que permite buscar el comando más reciente en tu historial que coincide con el texto que introduzcas a continuación. Presionar `Ctrl-R` una o más veces adicionales permite buscar coincidencias más antiguas.
> - `!!` recupera el comando anterior inmediato (puedes ser que esto te parezca más conveniente que usar la flecha hacia arriba).
> - `!$` recupera la última palabra del último comando. Esto es más útil de lo que pensarías: después de `bash goostats NENE01729B.txt stats-NENE01729B.txt`, puedes teclear `less !$` para ver el archivo `NENE01729B.txt`, que es más rápid que utilizar la flecha hacia arriba y editar el comando. {: .callout}

> ## Variables en bucles
>
> En el directorio `data-shell/molecules`, `ls` regresa la siguiente salida:
>
> ~~~
> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> ~~~
> {: .output}
>
> ¿Cuál es la salida del siguiente código?:
>
> ~~~
> for datafile in *.pdb
> do
>     ls *.pdb
> done
> ~~~
> {: .bash}
>
> Ahora, ¿cuál es la salida de este código?:
>
> ~~~
> for datafile in *.pdb
> do
>	  ls $datafile
> done
> ~~~
> {: .bash}
>
> ¿Por qué estos dos bucles dan resultados diferentes? {: .challenge}
>
>> ## Solución
>>
>> El primer bloque de código regresa la misma salida en cada iteración del bucle. La terminal expande el caracter especial `*.pdb` en el cuerpo del bucle (y al principio del mismo) para coincidir con todos los archivos que terminan en `.pdb`, y los enumera utilizando `ls`. El bucel expandido se vería así:
>>
>> ~~~
>> for datafile in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
>> do
>> ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
>> done
>> ~~~
>> {: .bash}
>>
>> ~~~
>> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
>> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
>> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
>> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
>> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
>> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
>> ~~~
>> {: .output}
>>
>> El segundo bloque de código enumera a un archivo distinto en cada iteración. El valor de la variable `datafile` es evaluado usando `$datafile` y después enumerado usando `ls`.
>>
>> ~~~
>> cubane.pdb
>> ethane.pdb
>> methane.pdb
>> octane.pdb
>> pentane.pdb
>> propane.pdb
>> ~~~
>> {: .output} {: .solution} {: .challenge}

> ## Guardar en un archivo dentro de un bucle - Primera parte
>
> En el mismo directorio, ¿cuál es el efecto de este bucle?
>
> ~~~
> for alkanes in *.pdb
> do
>     echo $alkanes
>     cat $alkanes > alkanes.pdb
> done
> ~~~
> {: .bash}
>
> 1. Imprime `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` y `propane.pdb`, y el texto de `propane.pdb` se guarda en un archivo llamado `alkanes.pdb`.
> 2. Imprime `cubane.pdb`, `ethane.pdb`, `methane.pdb`, y concatena el texto de los tres archivos en un archivo llamado `alkanes.pdb`.
> 3. Imprime `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` y `pentane.pdb`, y guarda el texto de `propane.pdb` en un archivo llamado `alkanes.pdb`.
> 4. Ninguna de las anteriores.
>
>> ## Solución
>>
>> 1. El texto de cada archivo se escribe (uno a la vez) en `alkanes.pdb`. Sin embargo, el archivo se sobrescribe en cada iteración del bucle, por lo que el contenido final de `alkanes.pdb` es sólo el texto proveniente de `propane.pdb`. {: .solution} {: .challenge}

> ## Guardar en un archivo en un bucle - Segunda parte
>
> En el mismo directorio, ¿cuál sería la salida del siguiente bucle?
>
> ~~~
> for datafile in *.pdb
> do
>    cat $datafile >> all.pdb
> done
> ~~~
> {: .bash}
>
> 1. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` y `pentane.pdb` sería
> concatenado y guardado en un archivo llamado `all.pdb`.
> 2. El texto de `ethane.pdb` se guardará en un archivo llamado `all.pdb`.
> 3. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` y `propane.pdb` sería concatenado y guardado en un archivo llamado `all.pdb`.
> 4. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` y `propane.pdb` se imprimirá en pantalla y será salvado en un archivo llamado `all.pdb`.
>
>> ## Solución
>>
>> La opción correcta es 3. `>>` concatena en un archivo, en lugar de sobrescribirlo con la salida del comando. Dado que la salida del comando `cat` ha sido redirigida, nada se imprime en pantalla. {: .solution} {: .challenge}

> ## Limitación de conjuntos de archivos
>
> En el mismo directorio, ¿cuál sería la salida del siguiente bucle?:
>
> ~~~
> for filename in c*
> do
>     ls $filename 
> done
> ~~~
> {: .bash}
>
> 1. No se muestra ningún archivo.
> 2. Todos los archivos son enumerados.
> 3. Sólo se enumeran `cubane.pdb`, `octane.pdb` y `pentane.pdb`.
> 4. Sólamente se enumera `cubane.pdb`.
>
>> ## Solución
>> 
>> La respuesta correcta es 4. `*` coincide con cero o más caracteres, así que cualquier nombre que comience con la letra c, seguida de cero o más caracteres, coincidirá con el comando. {: .solution}
>
> ¿Cómo diferiría el resultado si usáramos el siguiente comando en lugar del anterior?
>
> ~~~
> for filename in *c*
> do
>     ls $filename 
> done
> ~~~
> {: .bash}
>
> 1. Se listarían los mismos archivos.
> 2. Esta vez se enumerarían todos los archivos.
> 3. Esta vez no enumeraría ningún archivo.
> 4. Sólo se enumeraría el archivo `octane.pdb`.
>
>> ## Solución
>>
>> La respuesta correcta es 4. `*` coincide con cero o más caracteres, por lo que la expresión `*c*` coincidirá con un nombre de archivo con cero o más caracteres antes de la letra c, y cero o más caracteres después de la letra c. {: .solution} {: .challenge}

> ## Haciendo una ejecución "en seco" (dry-run)
>
> Un bucle es una forma de hacer muchas cosas a la vez, o de cometer muchos errores a
> la vez si se diseña de forma incorrecta. Una manera de comprobar lo que un bucle *hará* 
> es usar el comando `echo`. En lugar de ejecutar los comandos, los mostrará en pantalla.
>
> Supongamos que queremos obtener una vista previa de los comandos que ejecutará el siguiente bucle,
> sin ejecutarlos realmente:
>
> ~~~
> for file in *.pdb
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
> for file in *.pdb
> do
>   echo analyze $file > analyzed-$file
> done
> ~~~
> {: .bash}
>
> ~~~
> # Version 2
> for file in *.pdb
> do
>   echo "analyze $file > analyzed-$file"
> done
> ~~~
> {: .bash}
>
>> ## Solución
>>
>> La versión que querríamos utilizar es la 2. Esta versión imprime en pantalla todo lo entrecomillado, expandiendo la variable del bucle porque incluye el signo `$` como prefijo.
>>
>> La versión 1 redirige la salida del comando `echo analyze $file` a un archivo, `analyzed-$file`. Es decir, se genera una serie de archivos: `analyzed-cubane.pdb`, `analyzed-ethane.pdb`, etc.
>>
>> Prueba ambas versiones en tu terminal para ver la salida. Asegúrate de abrir los archivos `analyzed-*.pdb` para ver su contenido. {: .solution} {: .challenge}

> ## Bucles anidado
>
> Suponte que queremos configurar una estructura de directorios para organizar
> algunos experimentos que miden constantes de velocidad de reacción que involucran distintos compuestos *y* distintas temperaturas. ¿Cuál sería el
> resultado del siguiente código?:
>
> ~~~
> for species in cubane ethane methane
> do
>     for temperature in 25 30 37 40
>     do
>         mkdir $species-$temperature
>     done
> done
> ~~~
> {: .bash}
>
>> ## Solución
>>
>> Tenemos un bucle anidado, es decir, un bucle dentro de otro bucle: para cada `specie` del bucle exterior, el bucle interior (el bucle anidado) itera sobre la lista de temperaturas y crea un nuevo directorio para cada combinación.
>>
>> ¡Intenta ejecutar el código para descubrir qué directorio se crean! {: .solution} {: .challenge}

