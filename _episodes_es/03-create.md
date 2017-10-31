---
title: "Trabajando con archivos y directorios"
teaching: 15
exercises: 0
questions:
- "¿Cómo puedo crear, copiar y eliminar archivos y directorios?"
- "¿Cómo puedo editar archivos?"
objectives:
- "Crear una jerarquía de directorios que coincida con un diagrama dado."
- "Crear archivos en esa jerarquía usando un editor o copiando y renombrando archivos existentes."
- "Mostrar el contenido de un directorio utilizando la línea de comandos."
- "Eliminar archivos y/o directorios específicos."
keypoints:
- "`cp old new` copia un archivo."
- "`mkdir path` crea un nuevo directorio."
- "`mv old new` mueve (renombra) un archivo o directorio."
- "`rm path` elimina un archivo."
- "El uso de la tecla Control puede ser descrito de muchas maneras, incluyendo` Ctrl-X`, `Control-X` y` ^ X`. "
- "El shell no tiene una papelera de reciclaje o bote de basura: una vez que algo se elimina, se borra completamente."
- "Dependiendo del tipo de trabajo que se requiera, puede ser necesario utilizar un editor de textos más poderoso que Nano."
---

Ahora sabemos cómo explorar archivos y directorios,
pero ¿cómo los creamos en primer lugar?
Volvamos a nuestro directorio `data-shell` en Desktop
y utilicemos el comando `ls -F` para ver lo que contiene:

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell
~~~
{: .output}

~~~
$ ls -F
~~~
{: .bash}

~~~
creatures/  molecules/           pizza.cfg
data/       north-pacific-gyre/  solar.pdf
Desktop/    notes.txt            writing/
~~~
{: .output}
Creemos un nuevo directorio llamado `thesis` usando el comando` mkdir thesis`
(que no genera una salida):

~~~
$ mkdir thesis
~~~
{: .bash}

Como su nombre sugiere,
`mkdir` significa "make directory", 
que significa "crear directorio" en inglés. 
Dado que `thesis` es una ruta relativa
(es decir, no inicia con una diagonal),
el nuevo directorio se crea en el directorio de trabajo actual: 

~~~
$ ls -F
~~~
{: .bash}

~~~
creatures/  north-pacific-gyre/  thesis/
data/       notes.txt            writing/
Desktop/    pizza.cfg
molecules/  solar.pdf
~~~
{: .output}

> ## Dos maneras de hacer lo mismo
> Usar la terminal para crear un directorio no es diferente de usar un navegador de archivos gráfico.
> Si abres el directorio actual utilizando el explorador de archivos gráfico de tu sistema operativo,
> el directorio `thesis` aparecerá allí también.
> Si bien son dos formas diferentes de interactuar con los archivos,
> los archivos y los directorios con los que trabajamos son los mismos.
{: .callout}

> ## Buena nomenclatura para archivos y directorios
>
> Usar nombres complicados para archivos y directorios pueden hacer tu vida muy complicada cuando se trabaja en la línea de comandos. Te ofrecemos algunos
> consejos útiles para nombrar tus archivos de ahora en adelante.
>
> 1. No uses espacios en blanco.
>
> Los espacios en blanco pueden hacer un nombre más significativo, pero, dado que se utilizan para separar argumentos en la línea de comandos, es mejor evitarlos en nombres de archivos y directorios.
> Puedes utilizar `-` o` _` en lugar de espacios en blanco.
>
> 2. No comiences el nombre con un `-` (guión).
>
> Los comandos tratan a los nombres que comienzan con `-` como opciones.
>
> 3. Utiliza únicamente letras, números, `.` (punto),` -` (guión) y `_` (guión bajo).
>
> Muchos otros caracteres tienen un significado especial en la línea de comandos
> y los aprenderemos durante esta lección. Algunos sólo harán que tu comando no funcione, otros pueden incluso hacer que pierdas datos.
>
> Si necesitas referirte a nombres de archivos o directorios que tengan espacios en blanco
> u otro carácter no alfanumérico, se debe poner el nombre entre comillas (`""`).
{: .callout}

Dado que acabamos de crear el directorio `thesis`, aún se encuentra vacío:

~~~
$ ls -F thesis
~~~
{: .bash}

Cambiemos nuestro directorio de trabajo a `thesis` usando` cd`,
y a continuación, ejecutemos un editor de texto llamado Nano para crear un archivo denominado `draft.txt`:

~~~
$ cd thesis
$ nano draft.txt
~~~
{: .bash}

> ## ¿Qué editor usar?
>
> Cuando decimos, "`nano` es un editor de texto", realmente queremos decir "texto": 
> sólo funciona con datos de caracteres simples, no con tablas, imágenes o cualquier otro
> formato amigable con el usuario. Lo utilizamos en ejemplos porque es un editor muy sencillo que permite funciones muy básicas. Sin embargo, por estas mismas cualidades, podría ser insuficiente para necesidades de la vida real. En los sistemas Unix (como Linux y Mac OS X)
> muchos programadores utilizan [Emacs](http://www.gnu.org/software/emacs/) o
> [Vim](http://www.vim.org/) (ambos requieren más tiempo para familiarizarse), o un editor gráfico como
> [Gedit](http://projects.gnome.org/gedit/). En Windows puedes
> utilizar [Notepad ++](http://notepad-plus-plus.org/). Windows también tiene un editor
> interno llamado `notepad` que se puede ejecutar desde la línea de comandos de la misma
> manera que `nano` para los propósitos de esta lección.
>
> Sea cual sea el editor que uses, necesitarás saber dónde busca
> y guarda archivos. Si lo inicias desde la terminal, usará (probablemente)
> el directorio de trabajo actual como ubicación predeterminada. Si utilizas
> el menú de inicio de tu computadora puede ser que los archivos se guarden en tu **Desktop** o el
> directorio de Documentos. Puedes cambiar de directorio destino navegando a
> otro directorio la primera vez guardes el archivo usando "Guardar como ...".
{: .callout}

Escribamos algunas líneas de texto.
Una vez que estemos contentos con nuestro texto, podemos presionar `Ctrl-O` (presiona la tecla Ctrl o Control y, mientras
la mantienes presionada, oprime la tecla O) para escribir nuestros datos en el disco
(se nos preguntará en qué archivo queremos guardar esto:
presiona Enter para aceptar el valor predeterminado sugerido `draft.txt`).

![Nano in Action](../fig/nano-screenshot.png)

Una vez que nuestro archivo está guardado, podemos usar `Ctrl-X` para salir del editor y volver a la terminal.

> ## Tecla Control, Ctrl o ^ 
>
> La tecla Control también se denomina tecla "Ctrl". Hay varias maneras
> de indicar el uso de la tecla Control. Por ejemplo,
> una instrucción para presionar la tecla Control y, mientras la mantienes pulsada,
> presionar la tecla X, puede ser descrita de cualquiera de las siguientes maneras:
>
> * `Control-X`
> * `Control+X`
> * `Ctrl-X`
> * `Ctrl+X`
> * `^X`
> * `C-x`
>
> En nano, a lo largo de la parte inferior de la pantalla se lee `^G Get Help ^O WriteOut`.
> Esto significa que puedes usar `Control-G` para obtener ayuda y` Control-O` para guardar tu
> archivo.
{: .callout}

`nano` no deja ninguna salida en la pantalla después de que salir del programa,
pero `ls` ahora muestra que hemos creado un archivo llamado` draft.txt`:

~~~
$ ls
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

Limpiemos un poco ejecutando `rm draft.txt`:

~~~
$ rm draft.txt
~~~
{: .bash}

Este comando elimina archivos (`rm` es la abreviatura de "remove", "remover" en inglés).
Si ejecutamos `ls` de nuevo,
la salida estará vacía una vez más,
indicándonos que nuestro archivo ha desaparecido:

~~~
$ ls
~~~
{: .bash}

> ## Eliminar es para siempre
>
> La terminal de Unix no tiene un contenedor de basura donde podamos restaurar archivos
> eliminados (aunque la mayoría de las interfaces gráficas de Unix sí lo tienen). En su lugar,
> cuando eliminamos archivos, se desenganchan del sistema de archivos para que
> su espacio de almacenamiento en disco pueda ser reciclado. Existen herramientas para encontrar y
> recuperar archivos eliminados, pero no hay garantía de que
> funcionen en todas las situaciones, ya que la computadora puede reciclar
> el espacio en disco del archivo en cuestión inmediatamente, perdiéndose de manera permanente.
{: .callout}

Creemos de nuevo el archivo
y después subamos un directorio a `/Users/nelle/Desktop/data-shell` usando `cd ..`:

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell/thesis
~~~
{: .output}

~~~
$ nano draft.txt
$ ls
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

~~~
$ cd ..
~~~
{: .bash}

Si tratamos de eliminar todo el directorio `thesis` usando `rm thesis`,
obtenemos un mensaje de error:

~~~
$ rm thesis
~~~
{: .bash}

~~~
rm: cannot remove `thesis': Is a directory
~~~
{: .error}

Esto ocurre porque `rm` por defecto sólo funciona en archivos, no en directorios.

Para realmente deshacernos de `thesis` también debemos eliminar el archivo `draft.txt`.
Podemos hacer esto con la opción [recursiva](https://en.wikipedia.org/wiki/Recursion) para `rm`:

~~~
$ rm -r thesis
~~~
{: .bash}

> ## Un gran poder conlleva una gran responsabilidad
>
> Eliminar los archivos en un directorio recursivamente puede ser una operación
> muy peligrosa. Si nos preocupa lo que podríamos eliminar, podemos
> añadir la opción "interactiva" `-i` a `rm`, que nos pedirá confirmar cada paso.
>
> ~~~
> $ rm -r -i thesis
> rm: descend into directory ‘thesis’? y
> rm: remove regular file ‘thesis/draft.txt’? y
> rm: remove directory ‘thesis’? y
> ~~~
> {: .bash}
>
> Esto elimina todo en el directorio y después el directorio, preguntando
> en cada paso para que se confirme la eliminación.
{: .callout}

Vamos a crear el directorio y el archivo una vez más.
(Ten en cuenta que esta vez estamos ejecutando `nano` con la ruta de acceso `thesis/draft.txt`,
en lugar de ir al directorio `thesis` y ejecutar `nano` en `draft.txt`.)

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell
~~~
{: .output}

~~~
$ mkdir thesis
$ nano thesis/draft.txt
$ ls thesis
~~~
{: .bash}

~~~
draft.txt
~~~
{: .output}

`draft.txt` no es un nombre particularmente informativo,
así que cambiemos el nombre del archivo usando el comando `mv`,
que es la abreviatura de "move" (mover):

~~~
$ mv thesis/draft.txt thesis/quotes.txt
~~~
{: .bash}

El primer parámetro dice a `mv` lo que estamos "moviendo"",
mientras que el segundo indica a dónde hay que moverlo.
En este caso
estamos moviendo `thesis/draft.txt` a `thesis/quotes.txt`,
que tiene el mismo efecto que cambiar el nombre del archivo.
Como esperamos,
`ls` nos muestra que `thesis` ahora contiene un archivo llamado `quotes.txt`:

~~~
$ ls thesis
~~~
{: .bash}

~~~
quotes.txt
~~~
{: .output}

Hay que tener cuidado al especificar el nombre del archivo destino, ya que `mv`
remplaza silenciosamente cualquier archivo existente con el mismo nombre,
provocando pérdida de datos. Un indicador adicional, `mv -i` (o `mv --interactive`),
se puede utilizar para hacer que `mv` te pida confirmación antes de sobrescribir.

Sólo por el gusto de la consistencia,
`mv` también funciona en directorios, es decir, no existe un comando separado `mvdir`. 
Vamos a mover `quotes.txt` al directorio de trabajo actual.
Utilizamos `mv` una vez más,
pero esta vez sólo usaremos el nombre de un directorio como el segundo parámetro
para indicar a `mv` que queremos mantener el nombre de archivo,
pero poner el archivo en algún lugar nuevo.
(es por eso que el comando se llama "mover".)
En este caso,
el nombre de directorio que usamos es el nombre de directorio especial `.` que mencionamos anteriormente.

~~~
$ mv thesis/quotes.txt .
~~~
{: .bash}

El resultado es mover el archivo desde el directorio en el que estaba en el directorio de trabajo actual.
`ls` ahora nos muestra que `thesis` está vacío:

~~~
$ ls thesis
~~~
{: .bash}

Además,
`ls` con un nombre de archivo o un nombre de directorio como parámetro sólo lista ese archivo o directorio.
Podemos usar esto para ver que `quotes.txt` todavía está en nuestro directorio actual:

~~~
$ ls quotes.txt
~~~
{: .bash}

~~~
quotes.txt
~~~
{: .output}

El comando `cp` funciona muy bien como` mv`,
excepto que copia un archivo en lugar de moverlo.
Podemos comprobar que hizo lo correcto usando `ls`
con dos rutas como parámetros --- como la mayoría de los comandos Unix,
`ls` puede recibir múltiples rutas a la vez:

~~~
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .bash}

~~~
quotes.txt   thesis/quotations.txt
~~~
{: .output}

Para probar que hicimos una copia, eliminemos el archivo `quotes.txt` del directorio actual
y después ejecutemos el mismo `ls` de nuevo.

~~~
$ rm quotes.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .bash}

~~~
ls: cannot access quotes.txt: No such file or directory
thesis/quotations.txt
~~~
{: .error}

Esta vez el error nos dice que no se puede encontrar `quotes.txt` en el directorio actual,
pero encuentra la copia en `thesis` que no hemos borrado.

> ## ¿Qué hay en un nombre?
>
> Tal vez notaste que todos los nombres de los archivos de Nelle son "algo punto
> algo", y en esta parte de la lección, usamos siempre la extensión
> `.txt`. Esto es sólo una convención: podemos llamar a un archivo `mythesis` o
> casi cualquier cosa que queramos. Sin embargo, la mayoría de la gente usa nombres de dos partes
> para que sea más fácil (para ellos y sus programas) diferenciar entre tipos
> de archivos. La segunda parte de este nombre se llama la
> **extensión de nombre de archivo** e indica
> qué tipo de datos contiene el archivo: `.txt` señala un archivo de texto sin formato, `.pdf`
> indica un documento PDF, `.cfg` es un archivo de configuración lleno de parámetros
> para algún programa, `.png` es una imagen PNG, y así sucesivamente.
>
> Esto es sólo una convención, aunque una importante. Los archivos contienen solo
> bytes: depende de nosotros y de nuestros programas interpretar esos bytes
> de acuerdo a las reglas para archivos de texto, documentos PDF,
> archivos de configuración, imágenes, etc.
>
> Nombrar una imagen PNG de una ballena como `whale.mp3` no lo convierte
> mágicamente en una grabación del canto de las ballenas, aunque *podría*
> hacer que el sistema operativo intente abrirlo con un reproductor de música
> cuando alguien hace doble clic en él.
{: .callout}

> ## Cambiando el nombre de archivos
>
> Supón que has creado un archivo `.txt` en tu directorio actual para incluír una lista de las
> pruebas estadísticas que necesitas hacer para analizar tus datos, y lo llamarás: `statstics.txt`
>
> Después de crear y guardar este archivo, te das cuenta de que has escrito mal el nombre del archivo. Si deseas
> corregir el error, ¿cuál de los siguientes comandos podrías utilizar para hacerlo?
>
> 1. `cp statstics.txt statistics.txt`
> 2. `mv statstics.txt statistics.txt`
> 3. `mv statstics.txt .`
> 4. `cp statstics.txt .`
>
Solución
> > 1. No. Mientras esto crearía un archivo con el nombre correcto, el archivo con nombre incorrecto todavía existiría en el directorio y necesitaría ser borrado.
> > 2. Sí, esto funcionaría para renombrar el archivo.
> > 3. No, el punto (.) indica dónde mover el archivo, pero no proporciona un nuevo nombre de archivo; no pueden crearse nombres de archivo idénticos.
> > 4. No, el punto (.) indica dónde copiar el archivo, pero no proporciona un nuevo nombre de archivo; no pueden crearse nombres de archivo idénticos.
> {: .solution}
{: .challenge}


> ## Moviendo y copiando
>
> ¿Cuál es la salida del último comando `ls` en la secuencia que se muestra a continuación?
>
> ~~~
> $ pwd
> ~~~
> {: .bash}
> ~~~
> /Users/jamie/data
> ~~~
> {: .output}
> ~~~
> $ ls
> ~~~
> {: .bash}
> ~~~
> proteins.dat
> ~~~
> {: .output}
> ~~~
> $ mkdir recombine
> $ mv proteins.dat recombine
> $ cp recombine/proteins.dat ../proteins-saved.dat
> $ ls
> ~~~
> {: .bash}
>
> 1.   `proteins-saved.dat recombine`
> 2.   `recombine`
> 3.   `proteins.dat recombine`
> 4.   `proteins-saved.dat`
>
> > ## Solución
> > Comenzamos en el directorio `/Users/jamie/data` y creamos una nueva carpeta llamada `recombine`.
> > La segunda línea mueve (`mv`) el archivo `proteins.dat` a la nueva carpeta (`recombine`).
> > La tercera línea hace una copia del archivo que acabamos de mover. La parte difícil aquí es en dónde se copió el archivo. Recuerda que `..` significa "subir un nivel", por lo que el archivo copiado ahora está en `/Users/jamie`.
> > Observa que `..` se interpreta con respecto al directorio actual de trabajo,
> > **no** con respecto a la ubicación del archivo que se está copiando.
> > Por lo tanto, lo único que se mostrará usando ls (en `/Users/jamie/data`) es la carpeta recombine.
> >
> > 1. No, consulta la explicación anterior. `proteins-saved.dat` se encuentra en `/Users/jamie`
> > 2. Sí
> > 3. No, consulta la explicación anterior. `proteins.dat` se encuentra en`/Users/jamie/data/recombine`
> > 4. No, consulta la explicación anterior. `Proteins-saved.dat` se encuentra en`/Users/jamie`
> {: .solution}
{: .challenge}

> ## Organización de directorios y archivos
>
> Jamie está trabajando en un proyecto y nota que sus archivos no están muy bien
> organizados:
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> analyzed/  fructose.dat    raw/   sucrose.dat
> ~~~
> {: .output}
>
> Los archivos `fructose.dat` y` sucrose.dat` contienen la salida de sus 
> análisis. ¿Qué comando(s) cubierto(s) en esta lección necesita ejecutar para que los comandos a continuación
> produzcan la salida mostrada?
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> analyzed/   raw/
> ~~~
> {: .output}
> ~~~
> $ ls analyzed
> ~~~
> {: .bash}
> ~~~
> fructose.dat    sucrose.dat
> ~~~
> {: .output}
>
>> ## Solución
>>
>> ~~~
>> mv *.dat analyzed
>> ~~~
>>
>> {: .bash} Jamie necesita mover sus archivos `fructose.dat` y `sucrose.dat` al directorio `analyzed`. La terminal expandirá *.dat para abarcar todos los archivos .dat en el directorio. Después, el comando `mv` moverá la lista de archivos .dat al directorio "analyzed". {: .solution} {: .challenge}

> ## Copiar con varios archivos
>
> Para este ejercicio puedes probar los comandos del directorio `data-shell/data`
> En el ejemplo que sigue, ¿qué hace `cp` cuando se le dan varios nombres de archivo y un nombre de directorio?:
>
> ~~~
> $ mkdir backup
> $ cp amino-acids.txt animals.txt backup/
> ~~~
> {: .bash}
>
> En el siguiente ejemplo, ¿qué hace `cp` cuando se le dan tres o más nombres de archivo?
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> amino-acids.txt  animals.txt  backup/  elements/  morse.txt  pdb/  planets.txt  salmon.txt  sunspot.txt
> ~~~
> {: .output}
> ~~~
> $ cp amino-acids.txt animals.txt morse.txt
> ~~~
> {: .bash}
>
>> ## Solución
>> 
>> Si se pasa como argumento más de un archivo seguido del nombre de un directorio (siempre que el nombre del directorio sea el último argumento), `cp` copia los archivos en el directorio especificado.
>>
>> Si se proveen tres nombres de archivo, `cp` arroja un error porque espera un nombre de directorio como último argumento.
>> ~~~
>> cp: target ‘morse.txt’ is not a directory
>> ~~~
>> {: .output} {: .solution} {: .challenge}


> ## Listado recursivo y por tiempo
>
> El comando `ls -R` enumera el contenido de los directorios recursivamente,
> es decir, enumera sus subdirectorios, subdirectorios secundarios, etc.
> en orden alfabético en cada nivel.
> El comando `ls -t` enumera los contenidos de acuerdo a la fecha y hora del último cambio,
> empezando por los archivos o directorios modificados más recientemente.
> ¿En qué orden muestra los archivos el comando `ls -R -t`?
>
>> ## Solución
>> El comando `ls -R` enumera los directorios recursivamente en orden cronológico de cada nivel, y los archivos en cada direcotrio también son desplegados en orden cronológico. {: .solution} {: .challenge}

> ## Creación de archivos de una manera diferente
>
> Hemos visto cómo crear archivos de texto usando el editor `nano`.
> Ahora, intenta el siguiente comando en tu directorio personal:
>
> ~~~
> $ cd                  # ir al directorio **home**
> $ touch my_file.txt
> ~~~
> {: .bash}
>
> 1. ¿Qué hizo el comando touch?
> Cuando abre su directorio de inicio con el explorador de archivos GUI,
> ¿aparece el archivo?
>
> 2. Utilice `ls -l` para inspeccionar los archivos. ¿Qué tan grande es `my_file.txt`?
>
> 3. ¿En que circunstancias desearía crear un archivo de esta manera?
>
>> ## Solución
>>
>> 1. El comando touch genera un nuevo archivo llamado "my_file.txt" en tu directorio **home**. Si te encuentras en tu directorio **home**, puedes observar el archivo recién creado utilizando `ls` en la terminal. También puedes visualizar "my_file.txt" en tu explorados de archivos GUI.
>> 2. Cuando inspeccionas el archivo con "ls -l", nota que el tamaño de "my_file.txt" es 0 kb. En otras palabras, no contiene dato alguno. Si abres "my_file.txt" en un editor de texto, aparecerá en blanco.
>> 3. Algunos programas no generan nuevos archivos de salida, pero requieren archivos en blanco que ya se hayan generado. Cuando uno de estos programas es ejecutado, automáticamente busca un archivo existente para llenarlo con su salida. El comando touch te permite generar eficientemente archivos en blanco para que este tipo de programas puedan usarlos. {: .solution} {: .challenge}

> ## Pasar a la carpeta actual
>
> Después de ejecutar los siguientes comandos,
> Jamie se da cuenta de que puso los archivos `sucrose.dat` y` maltose.dat` en la carpeta incorrecta:
>
> ~~~
> $ ls -F
> raw/ analyzed/
> $ ls -F analyzed
> fructose.dat glucose.dat maltose.dat sucrose.dat
> $ cd raw/
> ~~~
> {: .bash}
>
> Rellena los espacios en blanco para mover estos archivos a la carpeta actual
> (es decir, en la que ella está actualmente):
>
> ~~~
> $ mv ___/sucrose.dat  ___/maltose.dat ___
> ~~~
> {: .bash}
>
>> ## Solución
>>
>> ~~~
>> $ mv ../analyzed/sucrose.dat ../analyzed/maltose.dat .
>> ~~~
>>
>> {: .bash} Recuerda que `..` se refiere al directorio padre (es decir, el directorio en el nivel superior al actual) y `.` se refiere al directorio actual. {: .solution} {: .challenge}
>>

> ## Utilizando `rm` con seguridad
>
> ¿Qué ocurre cuando escribimos `rm -i thesis /quotations.txt`?
> ¿Por qué querríamos esta protección cuando usamos `rm`?
>
> > ## Solución
> >
>> ~~~
>> $ rm: remove regular file 'thesis/quotations.txt'?
>> ~~~
>>
> > {: .bash} La opción -i provocará que se pregunte antes de eliminar un elemento. La terminal de Unix no cuenta con una papelera de reciclaje, así que todos los archivos que sean eliminados desaparecerán para siempre. Por medio de la opción -i tienes la oportunidad de revisar que sólo estés eliminando los archivos que realmente deseas borrar. {: .solution} {: .challenge}

> ## Copiar una estructura de carpetas sin archivos
>
> Estás iniciando un nuevo experimento y te gustaría duplicar la estructura de archivos
> que utilizaste para tu experimento anterior, sin los archivos de datos para que puedas
> añadir los nuevos datos.
>
> Suponte que la estructura de archivos está en una carpeta llamada '2016-05-18-data',
> que contiene un directorio `data`. `data` a su vez contiene dos carpetas denominadas `raw` y `processed`, que contienen archivos de datos.
> El objetivo es copiar la estructura de archivos de la carpeta `2016-05-18-data`
> en una carpeta llamada `2016-05-20-data` y eliminar los archivos de datos de
> el directorio que acabas de crear.
>
> ¿Cuál de los siguientes conjuntos de comandos lograrían este objetivo?
> ¿Qué harían los otros comandos?
>
> ~~~
> $ cp -r 2016-05-18-data/ 2016-05-20-data/
> $ rm 2016-05-20-data/data/raw/*
> $ rm 2016-05-20-data/data/processed/*
> ~~~
> {: .bash}
> ~~~
> $ rm 2016-05-20-data/raw/*
> $ rm 2016-05-20-data/processed/*
> $ cp -r 2016-05-18-data/ 2016-5-20-data/
> ~~~
> {: .bash}
> ~~~
> $ cp -r 2016-05-18-data/ 2016-05-20-data/
> $ rm -r -i 2016-05-20-data/
> ~~~
> {: .bash}
>
>> ## Solución
>>
>> El primer grupo de comandos logra este objetivo. Primero se cre una copia recursiva de la carpeta `data`. Después, dos comandos `rm` eliminan todos los archivos en los directorios especificados. La terminal interpreta el caracter especial `*` para incluír todos los archivos y subdirectorios.
>>
>> El segundo grupo de comandos está en el orden incorrecto: intenta borrar archivos que aún no han sido copiados, seguido del comando recursivo que los copiaría.
>>
>> El tercer grupo de comandos podría lograr el objetivo deseado, pero de una forma muy poco eficiente: el primer comando copia el directorio de forma recursiva, pero el segundo comando borra de forma interactiva, requiriendo confirmación antes de borrar cada archivo y directorio. {: .solution} {: .challenge}
