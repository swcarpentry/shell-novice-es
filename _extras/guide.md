---
layout: page
title: "Instructor Notes"
permalink: /guide/
---


* ¿Por qué aprendemos a usar el terminal?
  * Permite a los usuarios automatizar tareas repetitivas
  * Y captura pequeños pasos de manipulación de datos que normalmente no se graban hacer la investigación reproducible
  
* El problema
  * Ejecutar el mismo flujo de trabajo en varias muestras puede ser innecesariamente laborioso
  * Manipulación manual de archivos de datos:
	* a menudo no se captura en la documentación es difícil de reproducir
	* es difícil solucionar problemas, revisar o mejorar
* La terminal
  * Los flujos de trabajo pueden automatizarse mediante el uso de scripts de terminal
  * Los comandos incorporados permiten una fácil manipulación de los datos (por ejemplo, sort, grep, etc.)
  * Cada paso puede capturarse en el script de terminal y permitir la reproducibilidad y la resolución de problemas fáciles

## En general

Muchas personas han cuestionado si todavía debemos enseñar la terminal.
Después de todo,
cualquiera que quiera renombrar varios miles de archivos de datos
puede hacerlo de manera interactiva en el intérprete de Python,
y cualquiera que esté haciendo un análisis de datos serio
probablemente haga la mayor parte de su trabajo dentro del IPython Notebook o RStudio.
Entonces, ¿por qué enseñar la terminal?

La primera respuesta es
"Porque mucho más depende de eso".
Instalación de software,
configurando su editor predeterminado,
y el control de máquinas remotas frecuentemente asume una familiaridad básica con la terminal,
y con ideas relacionadas como entrada y salida estándar.
Muchas herramientas también usan su terminología
(por ejemplo, los comandos mágicos `% ls` y`% cd` en IPython).

La segunda respuesta es
"Porque es una manera fácil de presentar algunas ideas fundamentales sobre cómo usar computadoras".
A medida que enseñamos a la gente a usar la terminal de Unix,
les enseñamos que deben hacer que la computadora repita las cosas
(a través de la terminación de la pestaña,
`!` seguido de un número de comando,
y `for` loops)
en lugar de repetir las cosas por sí mismos.
También les enseñamos a tomar cosas que descubrieron que hacen con frecuencia
y guardarlos para su posterior reutilización
(a través de scripts de terminal),
dar nombres sensatos a las cosas,
y escribir un poco de documentación
(como un comentario en la parte superior de los scripts de terminal)
para mejorar la vida de sus futuros seres.

La tercera respuesta es
"Porque permite el uso de muchas herramientas específicas de dominio y recursos informáticos a los que los investigadores no pueden acceder de otra manera".
La familiaridad con la terminal es muy útil para las máquinas de acceso remoto,
utilizando la infraestructura informática de alto rendimiento,
y ejecutar nuevas herramientas especializadas en muchas disciplinas.
No enseñamos HPC o habilidades específicas de dominio aquí
pero sienta las bases para un mayor desarrollo de estas habilidades.
En particular,
comprender la sintaxis de comandos, indicadores y sistemas de ayuda es útil para herramientas específicas de dominio
y comprender el sistema de archivos (y cómo navegarlo) es útil para el acceso remoto.

Finalmente,
y quizás lo más importante,
enseñando a las personas la terminal nos permite enseñarles
pensar en la programación en términos de composición de funciones.
En el caso del terminal,
esto toma la forma de pipelines en lugar de llamadas a funciones anidadas,
pero la idea central de "piezas pequeñas, unidas libremente" es la misma.

Todo este material puede ser cubierto en tres horas
siempre que los alumnos que usen Windows no se encuentren con obstáculos como:

* no ser capaz de averiguar dónde está su directorio de inicio
    (particularmente si están usando Cygwin);
* no ser capaz de ejecutar un editor de texto plano;
    y
* la terminal que se niega a ejecutar scripts que incluyen terminaciones de línea DOS.

## Preparando para enseñar

* Use el directorio `data` para ejercicios en el taller y ejemplos de codificación en vivo.
     Puede clonar el directorio shell-novice o usar `Download ZIP`
     botón a la derecha para obtener el [repositorio] completo (https://github.com/swcarpentry/shell-novice). También proporcionamos ahora
     un archivo zip del directorio `data` que se puede descargar por sí mismo
     del repositorio haciendo clic derecho + Guardar o ver la página ["setup"] ({{page.root}} / setup /) en el sitio web de la lección para más detalles.

* Sitio web: se han usado varias prácticas.
    * Opción 1: puede proporcionar enlaces a los alumnos antes de la lección para que puedan seguirlos,
        alcanzar,
y vea ejercicios (especialmente si está siguiendo el contenido de la lección sin muchos cambios).
    * Opción 2: No mostrar el sitio web a los estudiantes durante la lección, ya que puede ser una distracción:
        los estudiantes pueden leer en lugar de escuchar, y tener otra ventana abierta es una carga cognitiva adicional.
* En cualquier caso, asegúrese de señalar el sitio web como una referencia posterior al taller.

* Contenido:
    A menos que tenga una cantidad de tiempo realmente generosa (más de 4 horas),
    es probable que no cubra TODO el material de esta lección en una sola sesión de medio día.
    Planee con anticipación lo que puede omitir, lo que realmente desea enfatizar, etc.

* Ejercicios:
    Piense de antemano acerca de cómo podría manejar los ejercicios durante la lección.
    ¿Cómo los está asignando (sitio web, diapositiva, folleto)?
    ¿Quieres que todos lo intenten y luego muestras la solución?

Haga que un alumno muestre la solución?
    Haga que cada grupo haga un ejercicio diferente y presente sus soluciones?

* `reference.md` se puede imprimir y entregar a los estudiantes como referencia, a su elección.

* Otra preparación:
    Siéntase libre de agregar sus propios ejemplos o comentarios secundarios,
    pero debes saber que no debería ser necesario:
    los temas y los comandos se pueden enseñar como se indica en las páginas de la lección.
    Si crees que hay un lugar donde falta la lección,
    no dude en presentar un problema o enviar una solicitud de extracción.

## Notas de enseñanza

* ¡Excelente recurso en línea!
    <http://explainshell.com/> diseccionará cualquier comando de terminal que escriba
    y mostrar texto de ayuda para cada pieza. Otra buena herramienta manual podría ser <http://tldr-pages.github.io/> con breves manuales muy descriptivos para comandos de terminal, útiles especialmente en Windows mientras se usa Git BASH donde `man` no podría funcionar.

* Otro recurso en línea genial es <http://www.shellcheck.net>,
    que verificará los scripts de terminal (tanto cargados como ingresados) para errores comunes.

* Recursos para "dividir" tu terminal para que los comandos recientes
    permanecer a la vista: <https://github.com/rgaiacs/swc-shell-split-window>.

* Ejecutar un editor de texto desde la línea de comando puede ser
    el mayor escollo durante toda la lección:
    muchos intentarán ejecutar el mismo editor que el instructor
    (lo que puede dejarlos atrapados en el horrible infierno inferior que es Vim),
    o no sabrá cómo navegar al directorio correcto
    para guardar su archivo,
    o ejecutará un procesador de texto en lugar de un editor de texto sin formato.
    La forma más rápida de superar estos problemas es tener aprendices más conocedores
    ayudar a quienes lo necesitan.

* Introducción y navegación del sistema de archivos en la terminal (cubierto en
    [Navegación de archivos y directorios] ({{page.root}} / 02-filedir /) sección) puede ser confuso. Puede tener el explorador de archivos de la GUI y la terminal abierto uno al lado del otro para que los alumnos puedan ver el contenido y la estructura del archivo mientras usan la terminal para navegar por el sistema.

* La finalización de pestañas suena como algo pequeño: no lo es.
    Volviendo a ejecutar comandos viejos usando `! 123` o`! Wc`
    no es una cosa pequeña tampoco,
    y tampoco lo son la expansión de comodines y los bucles `for`.
    Cada uno es una oportunidad para repetir una de las grandes ideas de Software Carpentry:
    si la computadora * puede * repetirlo,
    algún programador en algún lugar seguramente habrá construido
    de alguna manera para que la computadora * lo * repita.

* Construyendo una tubería con cuatro o cinco etapas,
    luego ponerlo en un script de terminal para su reutilización
    y llamando a ese script dentro de un bucle `for`,
    es una gran oportunidad para mostrar cómo
    "siete más o menos dos"
    se conecta a la programación.
    Una vez que hemos descubierto cómo hacer algo moderadamente complicado,
    lo hacemos reutilizable y le damos un nombre
    de modo que solo ocupe un espacio en la memoria de trabajo
    en lugar de varios.
    También es una buena oportunidad para hablar sobre programación exploratoria:
    en lugar de diseñar un programa por adelantado,
    podemos hacer algunas cosas útiles
    y luego decidir retroactivamente cuáles valen la pena encapsular
    para su futura reutilización.

* Si todo va bien, puedes llevar a casa el punto de ese archivo
    las extensiones están esencialmente ahí para ayudar a las computadoras (y
    lectores) entienden el contenido del archivo y no son un requisito de los archivos
    (cubierto brevemente en [Navegación de archivos y directorios] ({{page.root}} / 02-filedir /)).
    Esto se puede hacer en la sección [Tuberías y filtros] ({{page.root}} / 04-pipefilter /) mostrando que
    puede redirigir salida estándar a un archivo sin la extensión .txt
    (por ejemplo, longitudes) y que el archivo resultante sigue siendo un archivo de texto perfectamente utilizable.
    Haga notar que si hace doble clic en la GUI, la computadora
    probablemente te pregunte qué quieres hacer.

* Tenemos que omitir muchas cosas importantes debido a las limitaciones de tiempo,
    incluyendo permisos de archivos, control de trabajos y SSH.
    Si los alumnos ya entienden el material básico,
    esto se puede cubrir utilizando las lecciones en línea como pautas.
    Estas limitaciones también tienen consecuencias de seguimiento:

* Es difícil discutir `#!` (Shebang) sin antes discutir
    permisos, que no hacemos `#!` es también [bonita
    complicado] [shebang], así que incluso si discutimos los permisos,
    probablemente todavía no quiera discutir `#!`.

* Instalar Bash y un conjunto razonable de comandos de Unix en Windows
    siempre implica algo de manipulación y frustración.
    Consulte el último conjunto de pautas de instalación para obtener asesoramiento,
    y pruébalo tú mismo * antes * enseñando una clase.

* En máquinas con Windows
    si `nano` no se ha instalado correctamente con el
    [Software Carpentry Windows Installer] [Windows-installer]
    es posible usar `notepad` como alternativa. Habrá una GUI
    interfaz y terminaciones de línea se tratan de manera diferente, pero de lo contrario, para
    los propósitos de esta lección, `notepad` y` nano` se pueden usar casi como intercambiablemente.

* En Windows, parece que:

    ~~~
    $ cd
    $ cd Desktop
    ~~~
    {: .language-bash}

    siempre pondrá a alguien en su escritorio.
    Pídales que creen el directorio de ejemplos para los ejercicios de terminal allí
    para que puedan encontrarlo fácilmente
    y mire cómo evoluciona.

* Manténgase dentro de los comandos que cumplen con POSIX, como lo hacen todos los materiales de enseñanza.
   Su terminal particular puede tener extensiones más allá de POSIX que no están disponibles
   en otras máquinas, especialmente la predeterminada OSX bash y Windows bash emulators.
   Por ejemplo, POSIX `ls` no tiene una opción` --ignore = `o` -I`, y POSIX
   `head` toma` -n 10` o `-10`, pero no la forma larga de` --lines = 10`.

## Windows

Instalación de Bash y un conjunto razonable de comandos de Unix en Windows
siempre implica algo de manipulación y frustración.
Consulte el último conjunto de pautas de instalación para obtener asesoramiento,
y pruébalo tú mismo * antes * enseñando una clase.
Las opciones que hemos explorado incluyen:

1. [msysGit] (http://msysgit.github.io/) (también llamado "Git Bash"),
2. [Cygwin] (http://www.cygwin.com/),
3. usando una máquina virtual de escritorio, y
4. Hacer que los alumnos se conecten a una máquina Unix remota (generalmente una VM en la nube).

Cygwin fue la opción preferida hasta mediados de 2013,
pero una vez que comenzamos a enseñar a Git,
msysGit demostró funcionar mejor.
Las máquinas virtuales de escritorio y las máquinas virtuales basadas en la nube funcionan bien para los estudiantes técnicamente sofisticados,
y puede reducir la instalación y configuración al inicio del taller,
pero:

1. no funcionan bien en máquinas poco potentes,
2. son confusos para los principiantes (porque las cosas simples como copiar y pegar funcionan de manera diferente),
3. los alumnos abandonan el taller sin un entorno de trabajo en su sistema operativo de elección, y
4. los estudiantes pueden aparecer sin haber descargado la VM o la conexión inalámbrica se reducirá (o se congestionará) durante la lección.

Lo que sea que uses,
por favor * pruébelo usted mismo * en una máquina Windows * antes * de su taller:
las cosas siempre pueden haber cambiado a tu espalda desde tu último taller.
Y por favor también haz uso de nuestro
[Instalador de Windows de Software Carpentry][windows-installer].

[shebang]: http://www.in-ulm.de/~mascheck/various/shebang/
[windows-installer]: {{site.swc_github}} / windows-installer
