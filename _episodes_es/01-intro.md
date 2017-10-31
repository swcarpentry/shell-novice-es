---
Title: "Introducción a la Terminal"
Lesson: 5
Exercises: 0
Questions:
- "¿Qué es una terminal y por qué utilizarla?"
Objetives:
- "Explicar cómo se relaciona la terminal con el teclado, la pantalla, el sistema operativo y los programas de los usuarios."
- "Explicar cuándo y por qué se deben utilizar interfaces de línea de comandos en lugar de interfaces gráficas."
Keypoints:
- "Una terminal es un programa cuyo objetivo principal es leer comandos y ejecutar otros programas."
- "Las principales ventajas de la terminal son su alta relación acción-tecla, su soporte para la automatización de tareas repetitivas, y que puede utilizarse para acceder a otras máquinas en una red."
- "Las desventajas principales de la terminal son su naturaleza primordialmente textual y que sus comandos y operación pueden llegar a ser muy crípticos."
---

### Introducción

En su nivel más sencillo, las computadoras hacen cuatro cosas:

-   ejecutar programas
-   guardar datos
-   comunicarse entre ellas
-   interactuar con nosotros

Pueden hacer estas cosas de muchas maneras distintas, 
incluyendo conexiones directas entre cerebro-computadora o 
interfaces de voz. 
Aunque estas interfaces son cada vez más comunes, la mayoría de las interacciones aún se llevan a cabo a través de pantallas, ratones, pantallas táctiles y teclados.
A pesar de que la mayoría de los sistemas operativos modernos se comunican con sus 
usuarios a través de ventanas, íconos y apuntadores, estas tecnologías no eran
comunes sino hasta los años 80s. Las raíces de estas *interfaces gráficas de usuario*
se remontan al trabajo de Doug Engelbart's en los 60s, el cual podemos ver en lo que
se ha denominado "[La Madre de todos los Demos](http://www.youtube.com/watch?v=a11JDLBXtPQ)".

### Interfaz de Línea de Comandos

Remontándonos aún más allá, 
la única manera de interactuar con las computadoras tempranas era reorganizando 
sus cables. 
Después, entre los 50s y los 80s, la mayoría de la gente utilizaba impresoras de línea.
Estos aparatos solo permitían generar entradas y salidas de letras, números y signos 
de puntuación que se encontraban en un teclado estándar, por lo que los lenguajes 
de programación y las interfaces con el *software* tuvieron que ser diseñados con esa 
limitante en mente. 

A este tipo de interfaz se le denomina **interfaz de línea de comandos** 
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

### La Terminal

Esta descripción hace pensar que el usuario envía comandos directamente a la computadora
y que la computadora envía el resultado o salida directamente al usuario.
De hecho,
por lo general hay un programa intermediario conocido como un
**terminal** o **línea de comandos**.
Lo que el usuario escribe se pasa a la terminal,
la cual calcula qué comandos ejecutar y ordena al equipo su ejecución.
(En inglés, a la terminal se le llama "shell", que quiere decir concha, porque encierra al sistema operativo
con el fin de ocultar algo de su complejidad y hacer más fácil la interacción con él.)

Una terminal es un programa como cualquier otro.
Lo que la hace especial es que su trabajo es ejecutar otros programas, 
en lugar de realizar los cálculos en sí.
La terminal más popular de Unix se llama **Bash**, que proviene de **Bourne Again Shell**
(así llamado porque deriva de una versión previa escrita por Stephen Bourne).
**Bash** es la terminal por defecto en la mayoría de las implementaciones modernas de Unix,
y en la mayoría de los paquetes que proporcionan herramientas similares a las de Unix 
para Windows.

### ¿Por qué usarlo?

Utilizar **bash** o cualquier otra terminal
a veces se siente más como programación que como usar un ratón.
Los comandos son cortos (a menudo con sólo un par de caracteres de largo),
sus nombres son frecuentemente crípticos,
y su salida son líneas de texto en lugar de algo visual, como un gráfico.
Por otra parte,
con unas cuantas teclas la terminal nos permite combinar las herramientas existentes en
potentes **pipelines** y manejar grandes volúmenes de datos automáticamente. Esta automatización
no sólo nos hace más productivos, sino que también mejora la reproducibilidad de nuestros 
trabajo dado que permite repetir procesos de forma idéntica con unos simples comandos.
Además, la línea de comandos es a menudo la forma más fácil de interactuar con máquinas remotas y superordenadores.
La familiaridad con la terminal es casi esencial para utilizar una variedad de herramientas y recursos especializados,
incluyendo sistemas de computación de alto rendimiento.
A medida que los **clusters** y los sistemas de computación en la nube se vuelven más 
populares para el análisis de datos científicos,
ser capaz de interactuar con ellos se convierte en una habilidad necesaria.
Podemos aprovechar las habilidades que adquiriremos en línea de comandos
para abordar una amplia gama de preguntas científicas y desafíos computacionales.

## Pipeline de Nelle: Punto de partida

Nelle Nemo, una bióloga marina,
acaba de regresar de un estudio de seis meses del 
[Gyre del Pacífico Norte](http://en.wikipedia.org/wiki/North_Pacific_Gyre),
en donde ha estado muestreando la vida marina gelatinosa en la
[Gran Mancha de Basura del Pacífico](http://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch).
Tiene 1,520 muestras en total, ahora necesita:

1. Procesar cada muestra en una máquina de ensayo
 para medir la abundancia relativa de 300 proteínas diferentes.
 La salida de la máquina para una sola muestra es
 un archivo con una línea para cada proteína, y un archivo con la secuencia de cada proteína. 
2. Calcular las estadísticas de cada una de las proteínas por separado
 usando un programa que su supervisor escribió llamado `goostat`.
3. Comparar las estadísticas de cada proteína con las estadísticas correspondientes de las otras proteínas
 utilizando un programa que escribió uno de los estudiantes de doctorado llamado `goodiff`.
4. Resumir los resultados.
 A su supervisor le gustaría mucho que su análisis estuviera listo para fin de mes,
  para que su artículo pueda aparecer en un próximo número especial de *Aquatic Goo Letters*.

La máquina de ensayo tarda aproximadamente media hora en procesar cada muestra.
La buena noticia es que
sólo se necesitan dos minutos para configurar cada ensayo.
Dado que su laboratorio tiene ocho máquinas de ensayo que puede utilizar en paralelo,
este paso "sólo" durará unas dos semanas.

La mala noticia es que si quiere ejecutar `goostat` y` goodiff` a mano,
Nelle tendrá que ingresar los nombres de los archivos y hacer clic en "Aceptar" 46,370 veces
(1520 carreras de `goostat`, más 300 * 299/2 (la mitad de 300 veces 299) ejecuciones de` goodiff`).
Dado que cada ejecución toma 30 segundos,
le llevará más de dos semanas (sin dormir ni comer).
Nelle no sólo no cumpliría su plazo de entrega de resultados,
sino que las posibilidades de que escriba todos los comandos correctamente son prácticamente cero.

Las siguientes lecciones explorarán una mejor alternativa para que Nelle realice su análisis.
Más específicamente,
explicaremos cómo puede usar la línea de comandos
para automatizar los pasos repetitivos en su **pipeline**. Así, su computadora podrá trabajar las 24 horas del día mientras ella escribe su artículo.
Además,
una vez que Nelle haya generado un **pipeline**
podrá usarlo de nuevo cada vez que colecte nuevos datos.

