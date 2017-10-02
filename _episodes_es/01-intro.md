---
title: "Introducción al Shell"
teaching: 5
exercises: 0
questions:
- "¿Qué es un shell de comandos y por qué utilizaría uno?"
objectives:
- "Explicar cómo se relaciona el shell con el teclado, la pantalla, el sistema operativo y los programas de los usuarios."
- "Explique cuándo y por qué se deben utilizar interfaces de línea de comandos en lugar de interfaces gráficas."
keypoints:
- "Explicar las similitudes y diferencias entre un archivo y un directorio."
- "Traducir una dirección (path) absoluta en una relativa y viceversa."
- "Construir rutas absolutas y relativas que identifican archivos y directorios específicos."
- "Explicar los pasos del ciclo de lectura-ejecución-impresión del shell."
- "Identificar el comando real, banderas y nombres de archivo en una llamada de línea de comandos."
- "Demostrar el uso de la autocompleción por tabs, y explicar sus ventajas."
keypoints:
- "Un shell es un programa cuyo objetivo principal es leer comandos y ejecutar otros programas."
- "Las principales ventajas de la shell son su alta relación acción-tecla, su soporte para la automatización de tareas repetitivas, y que se puede utilizar para acceder a las máquinas en red."
- "Las desventajas principales del shell son su naturaleza primordialmente textual y cuan críptico sus comandos y operación pueden ser."
---

En su nivel más sencillo, las computadoras hacen cuatro cosas:

-   ejecutar programas
-   guardar datos
-   comunicarse una con otra
-   e interactuar con nosotros

Pueden hace estas cosas de muchas maneras distintas, 
incluyendo a través de conexiones directas entre cerebro-computadora así como 
interfaces de voz. 
Debido a que estas interfaces de interacción están aún 
poco desarrolladas, aún tenemos que utilizar pantallas, ratones, 
pantallas táctiles y teclados.
A pesar de la mayoría de los sistemas operativos modernos se comunican con sus 
usuarios a través de ventanas, íconos y apuntadores, estas tecnologías no eran
comunes sino hasta los años 80s. La raíces de estas *interfaces gráficas de usuario*
se remontan al trabajo de Doug Engelbart's los 60s, el cual podemos ver en lo que
se ha denominado "[La Madre de todos los Demos](http://www.youtube.com/watch?v=a11JDLBXtPQ)".

Remontándonos aún más allá, 
la única manera de interactuar con las computadoras tempranas era reorganizando 
sus cables. 
Después, entre los 50s y los 80s la mayoría de la gente utilizaba impresoras de línea.
Estos aparatos solo permitían generar entradas y salidas de letras, número y signos 
de puntuación que se encontraban en un teclado estándar, por lo que los lenguajes 
de programación y las interfaces con el *software* tuvieron que ser diseñados con esa 
limitante en mente. 

A este tipo de interface se le denomina **interfaz de línea de comandos** 
(**command-line interface**, o CLI por sus siglas en inglés) para distinguirla de la 
**interfaz gráfica de usuario** (**graphical user interface**, o GUI) que es la 
que utilizan la mayoría de los usuarios actuales.
El corazón del CLI es un ciclo conocido como **read-evaluate-print loop**, o REPL 
(**ciclo lectura-ejecución-impresión**):
cuando el usuario teclea un comando y después presiona la tecla Enter, 
la computadora lo lee, 
ejecuta
e imprime el resultado (también conocido como output).
Después el usuario escribe otro comando y el ciclo continúa hasta que el 
usuario se desconecta del equipo. 

Esta descripción hace pensar que el usuario envía comandos directamente a la computadora,
y que la computadora envía el resultado o salida directamente al usuario.
De hecho,
por lo general hay un programa intermediario conocido como un
**shell de comandos**.
Lo que el usuario escribe se pasa al shell,
el cual calcula qué comandos ejecutar y ordena al equipo su ejecución.
(Tengan en cuenta que el shell se llama "el shell" o concha porque encierra al sistema operativo
con el fin de ocultar algo de su complejidad y hacer más fácil interactuar con él.)

Un shell es un programa como cualquier otro.
Lo que lo hace especial es que su trabajo es ejecutar otros programas 
en lugar de realizar los cálculos en sí.
El shell más popular de Unix se llama Bash,
el Bourne Again SHell
(Así llamado porque se deriva de una versión previa escrita por Stephen Bourne).
Bash es el shell por defecto en la mayoría de las implementaciones modernas de Unix
y en la mayoría de los paquetes que proporcionan herramientas similares a las de Unix 
para Windows.

Utilizar Bash o cualquier otro shell
a veces se siente más como programación que usar un ratón.
Los comandos son cortos (a menudo sólo un par de caracteres de largo),
sus nombres frecuentemente crípticos,
y su salida es líneas de texto en lugar de algo visual como un gráfico.
Por otra parte,
con sólo unas pocas teclas, el shell nos permite combinar las herramientas existentes en
potentes pipelines y manejar grandes volúmenes de datos automáticamente. Esta automatización
no sólo nos hace más productivos, sino que también mejora la reproducibilidad de nuestros 
trabajo dado que nos permite repetir un proceso de manera idéntica con unos pocos comandos simples.
Además, la línea de comandos es a menudo la forma más fácil de interactuar con máquinas remotas y superordenadores.
La familiaridad con el shell es casi esencial para ejecutar una variedad de herramientas y recursos especializados
incluyendo sistemas de computación de alto rendimiento.
A medida que los clusters y los sistemas de computación en la nube se vuelven más 
populares para el análisis de datos científicos,
ser capaz de interactuar con ellos se está convirtiendo en una habilidad necesaria.
Podemos aprovechar las habilidades de línea de comandos que aprenderemos
para abordar una amplia gama de cuestiones científicas y desafíos computacionales.

## Pipeline de Alicia: Punto de partida

Alicia Nemo, una bióloga marina,
acaba de regresar de una encuesta de seis meses del 
[Gyre del Pacífico Norte](http://en.wikipedia.org/wiki/North_Pacific_Gyre),
en donde ha estado muestreando la vida marina gelatinosa en la
[Gran Mancha de Basura del Pacífico](http://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch).
Tiene 1,520 muestras en total, y ahora necesita:

1. Procesar cada muestra a través de una máquina de ensayo
 para medir la abundancia relativa de 300 proteínas diferentes.
 La salida de la máquina para una sola muestra es
 un archivo con una línea para cada proteína así como un archivo con la secuencia de cada proteína. 
2. Calcular las estadísticas de cada una de las proteínas por separado
 usando un programa que su supervisor escribió llamado `goostat`.
3. Comparar las estadísticas de cada proteína con las estadísticas correspondientes para cada otra proteína
 utilizando un programa que escribió uno de los estudiantes de doctorado llamado `goodiff`.
4. Resumir los resultados.
 A su supervisor realmente le gustaría que termine su análisis para el final del mes
  para que su artículo pueda aparecer en un próximo número especial de *Aquatic Goo Letters*.

La máquina de ensayo tarda aproximadamente media hora en procesar cada muestra.
La buena noticia es que
sólo se necesitan dos minutos para configurar cada ensayo.
Dado que su laboratorio tiene ocho máquinas de ensayo que puede utilizar en paralelo,
este paso "sólo" durará unas dos semanas.

La mala noticia es que si tiene que ejecutar `goostat` y` goodiff` a mano,
Alicia tendrá que ingresar los nombres de los archivos y haga clic en "Aceptar" 46,370 veces
(1520 carreras de `goostat`, más 300 * 299/2 (la mitad de 300 veces 299) ejecuciones de` goodiff`).
Dado que cada ejecución toma 30 segundos ,
le llevará más de dos semanas (sin dormir ni comer).
Alicia no sólo se perdería su plazo de entrega de resultados,
sino que las posibilidades de que escriba todos los comandos correctamente son prácticamente cero.

Las siguientes lecciones explorarán una mejor alternativa para que Alicia realice su análisis.
Más específicamente,
explicaremos cómo puede usar un shell de comandos
para automatizar los pasos repetitivos en su tubería de procesamiento
para que su computadora pueda trabajar las 24 horas del día mientras escribe su trabajo.
Además,
una vez que Alicia haya generado una tubería de procesamiento,
podrá usarla de nuevo cada vez que colecte más datos.

