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
- "`rm path` elimina (elimina) un archivo."
- "`rmdir path` elimina (elimina) un directorio vacío."
- "El uso de la tecla Control puede ser descrito de muchas maneras, incluyendo` Ctrl-X`, `Control-X` y` ^ X`. "
- "El shell no tiene un compartimiento de basura: una vez que algo se borra, se borra completamente."
- "Nano es un editor de texto muy simple: por favor, sugerimos utilizar otro editor para el trabajo diario."
---

Ahora sabemos cómo explorar archivos y directorios,
Pero ¿cómo los creamos en primer lugar?
Volvamos a nuestro directorio `data-shell` en el Escritorio
y utilicemos el comando `ls -F` para ver lo que contiene:

~~~
$ pwd
~~~
{: .bash}

~~~
/home/Alicia/Desktop/data-shell
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

Vamos a crear un nuevo directorio llamado `thesis` usando el comando` mkdir thesis`
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
> Usar el shell para crear un directorio no es diferente a usar un navegador de archivos gráfico.
> Si abres el directorio actual utilizando el explorador de archivos gráfico de tu sistema operativo,
> el directorio `thesis` aparecerá allí también.
> Si bien son dos formas diferentes de interactuar con los archivos,
> los archivos y los directorios con los que trabajamos son los mismos.
{: .callout}

> ## Buena nomenclatura para archivos y directorios
>
> Dar nombres de archivos y directorios pueden hacer tu vida muy dolorosa
> cuando se trabaja en la línea de comandos. Te ofrecemos algunos útiles
> consejos para nombrar tus archivos de ahora en adelante.
>
> 1. No usar espacios en blanco.
>
> Los espacios en blanco pueden hacer un nombre más significativo
> Pero desde whitespace se utiliza para romper los argumentos en la línea de comandos
> Es mejor evitarlos en nombre de archivos y directorios.
> Puede utilizar `-` o` _` en lugar de espacios en blanco.
>
> 2. No comenzar el nombre con un `-` (guión).
>
> Los comandos tratan a los nombres que comienzan con `-` como opciones.
>
> 3. Utilizar unicamente letras, números, `.` (punto),` -` (guión) y `_` (guión bajo).
>
> Muchos otros caracteres tienen un significado especial en la línea de comandos
> y los aprenderemos durante esta lección. Algunos sólo harán que tu comando no funcione,
> ¡pero algunos de ellos pueden incluso hacer que pierdas algunos datos!
>
> Si necesitas referirte a nombres de archivos o directorios que tengan espacios en blanco
> u otro carácter no alfanumérico, se debe poner el nombre entre comillas (`""`).
{: .callout}

Dado que acabamos de crear el directorio `thesis`, aún está vacío:

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
> formato amigables para el usuario. Lo utilizamos en ejemplos porque la mayoría de la gente puede
> utilizarlo en cualquier lugar sin entrenamiento, pero por favor use algo más
> poderoso para el trabajo real. En los sistemas Unix (como Linux y Mac OS X)
> muchos programadores utilizan [Emacs](http://www.gnu.org/software/emacs/) o
> [Vim](http://www.vim.org/) (ambos de los cuales son completamente no intuitivos,
> incluso para los estándares Unix), o un editor gráfico como
> [Gedit](http://projects.gnome.org/gedit/). En Windows, puede
> utilizar [Notepad ++](http://notepad-plus-plus.org/). Windows también tiene un editor
> interno llamado `notepad` que se puede ejecutar desde la línea de comandos en la misma
> manera como `nano` para los propósitos de esta lección.
>
> Sea cual sea el editor que uses, necesitarás saber dónde busca
> y guarda archivos. Si lo inicias desde el shell, buscará (probablemente)
> en el directorio de trabajo actual como su ubicación predeterminada. Si utiliza
> el menú de inicio de su computadora, puede que guarde archivos en su escritorio o
> directorio de documentos. Puede cambiar esto navegando a
> otro directorio la primera vez que use "Guardar como ..."
{: .callout}

Escriba algunas líneas de texto.
Una vez que estemos contentos con nuestro texto, podemos presionar `Ctrl-O` (presionar la tecla Ctrl o Control y, mientras
mantenga pulsada la tecla O) para escribir nuestros datos en el disco
(se nos preguntará en qué archivo queremos guardar esto:
presione Enter para aceptar el valor predeterminado sugerido de `draft.txt`).

![Nano in Action](../fig/nano-screenshot.png)

Una vez que se guarda nuestro archivo, podemos usar `Ctrl-X` para salir del editor y
Volver a el shell.

> ## Tecla Control, Ctrl o ^ 
>
> La tecla Control también se denomina tecla "Ctrl". Hay varias maneras
> de indicar el uso de la tecla Control. Por ejemplo,
> una instrucción para presionar la tecla Control y, mientras la mantiene pulsada,
> presionar la tecla X, puede ser descrita de cualquiera de las siguientes maneras:
>
> * `Control-X`
> * `Control+X`
> * `Ctrl-X`
> * `Ctrl+X`
> * `^X`
>
> En nano, a lo largo de la parte inferior de la pantalla verá `^G Get Help ^O WriteOut`.
> Esto significa que puede usar `Control-G` para obtener ayuda y` Control-O` para guardar su
> archivo.
{: .callout}

`nano` no deja ninguna salida en la pantalla después de que salimos,
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

Este comando elimina los archivos (`rm` es la abreviatura de "remove").
Si ejecutamos `ls` de nuevo,
su salida estará vacía una vez más,
indicándonos que nuestro archivo ha desaparecido:

~~~
$ ls
~~~
{: .bash}

> ## Eliminar es para siempre
>
> La shell de Unix no tiene un contenedor de basura donde podamos restaurar archivos
> eliminados (aunque la mayoría de las interfaces gráficas de Unix si lo tienen). En su lugar,
> cuando eliminamos archivos, se desenganchan del sistema de archivos para que
> su espacio de almacenamiento en disco puede ser reciclado. Existen herramientas para encontrar y
> recuperar archivos eliminados existe, pero no hay garantía de que
> funcionan en todas las situaciones, ya que la computadora puede reciclar la
> el espacio en disco del archivo en cuestión inmediatamente, perdiéndose de manera permanente.
{: .callout}

Vamos a volver a crear ese archivo
y luego subir un directorio a `/home/Alicia/Desktop/data-shell` usando `cd ..`:

~~~
$ pwd
~~~
{: .bash}

~~~
/home/Alicia/Desktop/data-shell/thesis
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
recibimos un mensaje de error:

~~~
$ rm thesis
~~~
{: .bash}

~~~
rm: cannot remove `thesis': Is a directory
~~~
{: .error}

Esto ocurre porque `rm` por defecto sólo funciona en archivos, no en directorios.

Para realmente deshacernos de `thesis` también debemos eliminar el archivo` draft.txt`.
Podemos hacer esto con la opción [recursiva](https://en.wikipedia.org/wiki/Recursion) para `rm`:

~~~
$ rm -r thesis
~~~
{: .bash}

> ## Un gran poder conlleva una gran responsabilidad
>
> Eliminar los archivos en un directorio recursivamente puede ser una operación
> muy peligrosa. Si nos preocupa lo que podríamos eliminar, podemos
> añadir la bandera "interactiva" `-i` a `rm`, lo que nos pedirá confirmación
> antes de cada paso
>
> ~~~
> $ rm -r -i thesis
> rm: descend into directory ‘thesis’? y
> rm: remove regular file ‘thesis/draft.txt’? y
> rm: remove directory ‘thesis’? y
> ~~~
> {: .bash}
>
> Esto elimina todo en el directorio y luego el directorio mismo, preguntando
> en cada paso para que se confirme la eliminación.
{: .callout}

Vamos a crear ese directorio y archivo una vez más.
(Tenga en cuenta que esta vez estamos ejecutando `nano` con la ruta de acceso `thesis/draft.txt`,
en lugar de ir al directorio `thesis` y ejecutar `nano` en `draft.txt` allí.)

~~~
$ pwd
~~~
{: .bash}

~~~
/home/Alicia/Desktop/data-shell
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

El primer parámetro dice a `mv` lo que estamos moviendo,
mientras que el segundo indica a donde hay que moverlo.
En este caso,
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

Hay que tener cuidado al especificar el nombre del archivo de destino, ya que `mv`
remplaza silenciosamente cualquier archivo existente con el mismo nombre,
conduciendo a la pérdida de datos. Un indicador adicional, `mv -i` (o `mv --interactive`),
se puede utilizar para hacer que `mv` le pida confirmación antes de sobrescribir.

Sólo por el gusto de la inconsistencia,
`mv` también funciona en directorios --- no existe un comando separado `mvdir`. 

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

Para probar que hicimos una copia,
vamos a eliminar el archivo `quotes.txt` en el directorio actual
y luego ejecutar el mismo `ls` de nuevo.

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

Esta vez nos dice que no puede encontrar `quotes.txt` en el directorio actual,
pero encuentra la copia en `thesis` que no hemos borrado.

> ## ¿Qué hay en un nombre?
>
> Habrá notado que todos los nombres de los archivos de Alicia son "algo punto
> algo", y en esta parte de la lección, usamos siempre la extensión
> `.txt`. Esto es sólo una convención: podemos llamar a un archivo `mythesis` o
> casi cualquier cosa que queramos. Sin embargo, la mayoría de la gente usa nombres de dos partes
> la mayor parte del tiempo para ayudarles (y a sus programas) a contar diferentes tipos
> de archivos separados. La segunda parte de este nombre se llama la
> **extensión de nombre de archivo** e indica
> qué tipo de datos contiene el archivo: `.txt` señala un archivo de texto sin formato, `.pdf`
> indica un documento PDF, `.cfg` es un archivo de configuración lleno de parámetros
> para algún programa u otro, `.png` es una imagen PNG, y así sucesivamente.
>
> Esto es sólo una convención, aunque importante. Los archivos contienen solo
> bits: depende de nosotros y de nuestros programas interpretar esos bits
> de acuerdo con las reglas para archivos de texto sin formato, documentos PDF,
> archivos de configuración, imágenes, etc.
>
> Nombrar una imagen PNG de una ballena como `whale.mp3` no lo convierte
> mágicamente en una grabación del canto de las ballenas, aunque *podría*
> hacer que el sistema operativo intente abrirlo con un reproductor de música
> cuando alguien hace doble clic en él.
{: .callout}

> ## Cambiando de nombre a archivos
>
> Suponga que ha creado un archivo `.txt` en su directorio actual para contener una lista de las
> pruebas estadísticas que necesitará hacer para analizar sus datos, y lo llamará: `statstics.txt`
>
> Después de crear y guardar este archivo, te das cuenta de que has escrito mal el nombre del archivo. Si desea
> corregir el error, ¿cuál de los siguientes comandos podría utilizar para hacerlo?
>
> 1. `cp statstics.txt statistics.txt`
> 2. `mv statstics.txt statistics.txt`
> 3. `mv statstics.txt .`
> 4. `cp statstics.txt .`
>
Solución
> > 1. No. Mientras esto crearía un archivo con el nombre correcto, el archivo con nombre incorrecto todavía existe en el directorio
> > Y necesitaría ser borrado.
> > 2. Sí, esto funcionaría para renombrar el archivo.
> > 3. No, el punto (.) indica dónde mover el archivo, pero no proporciona un nuevo nombre de archivo; nombres de archivo idénticos
> > no se puede crear.
> > 4. No, el punto (.) indica dónde copiar el archivo, pero no proporciona un nuevo nombre de archivo; nombres de archivo idénticos
> > no se puede crear.
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
> /home/jamie/data
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
> > Comenzamos en el directorio `/home/jamie/data` y creamos una nueva carpeta llamada `recombine`.
> > La segunda línea mueve (`mv`) el archivo `proteins.dat` a la nueva carpeta (`recombine`).
> > La tercera línea hace una copia del archivo que acabamos de mover. La parte difícil aquí es a donde el archivo fue
> > copiado. Recuerde que `..` significa "subir un nivel", por lo que el archivo copiado ahora está en `/home/jamie`.
> > Observe que `..` se interpreta con respecto al directorio actual de trabajo,
> > **no** con respecto a la ubicación del archivo que se está copiando.
> > Por lo tanto, lo único que se mostrará usando ls (en `/home/jamie/data`) es la carpeta recombine.
> >
> > 1. No, vea la explicación arriba. `proteins-saved.dat` se encuentra en `/home/jamie`
> > 2. Sí
> > 3. No, vea la explicación anterior. `proteins.dat` se encuentra en`/home/jamie/data/recombine`
> > 4. No, vea la explicación anterior. `Proteins-saved.dat` se encuentra en`/home/jamie`
> {: .solution}
{: .challenge}

> ## Organización de directorios y archivos
>
> Jamie está trabajando en un proyecto y ve que sus archivos no están muy bien
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
{: .challenge}

> ## Copiar con varios archivos
>
> ¿Qué hace `cp` cuando se le dan varios nombres de archivo y un nombre de directorio?, como en:
>
> ~~~
> $ mkdir backup
> $ cp thesis/citations.txt thesis/quotations.txt backup
> ~~~
> {: .bash}
>
> ¿Qué hace `cp` cuando se le dan tres o más nombres de archivo?, como en:
>
> ~~~
> $ ls -F
> ~~~
> {: .bash}
> ~~~
> intro.txt    methods.txt    survey.txt
> ~~~
> {: .output}
> ~~~
> $ cp intro.txt methods.txt survey.txt
> ~~~
> {: .bash}
{: .challenge}


> ## Listado recursivo y por tiempo
>
> El comando `ls -R` enumera el contenido de los directorios recursivamente,
> es decir, enumera sus subdirectorios, subdirectorios secundarios, etc.
> en orden alfabético en cada nivel.
> El comando `ls -t` enumera las cosas por el tiempo del último cambio,
> empezando por los archivos o directorios modificados más recientemente.
> ¿En qué orden muestra los archivos el comando `ls -R -t`?
{: .challenge}

> ## Creación de archivos de una manera diferente
>
> Hemos visto cómo crear archivos de texto usando el editor `nano`.
> ahora, intente el siguiente comando en su directorio personal:
>
> ~~~
> $ cd                  # ir al directorio de inicio
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
{: .challenge}

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
> Rellene los espacios en blanco para mover estos archivos a la carpeta actual
> (es decir, en el que ella está actualmente):
>
> ~~~
> $ mv ___/sucrose.dat  ___/maltose.dat ___
> ~~~
> {: .bash}
{: .challenge}

> ## Utilizando `rm` con seguridad
>
> ¿Qué ocurre cuando escribimos `rm -i thesis /quotations.txt`?
> ¿Por qué querríamos esta protección cuando usamos `rm`?
>
> > ## Solution
> >
> > Solicita confirmación.
> {: .solution}
{: .challenge}

> ## Copiar una estructura de carpetas sin archivos
>
> Está iniciando un nuevo experimento y le gustaría duplicar el archivo
> de estructuras de su experimento anterior sin los archivos de datos para que pueda
> añadir nuevos datos.
>
> Suponga que la estructura de archivos está en una carpeta llamada '2016-05-18-data',
> que contiene carpetas denominadas 'raw' y 'processed' que contienen archivos de datos.
> El objetivo es copiar la estructura de archivos de la carpeta `2016-05-18-data`
> en una carpeta llamada `2016-05-20-data` y eliminar los archivos de datos de
> el directorio que acaba de crear.
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
> $ rm 2016-05-20-data/data/raw/*
> $ rm 2016-05-20-data/data/processed/*
> $ cp -r 2016-05-18-data/ 2016-5-20-data/
> ~~~
> {: .bash}
> ~~~
> $ cp -r 2016-05-18-data/ 2016-05-20-data/
> $ rm -r -i 2016-05-20-data/
> ~~~
> {: .bash}
{: .challenge}

