---
title: "Permisos"
teaching: 15
exercises: 0
questions:
- "¿Para qué sirven los permisos?"
- "¿Cómo puedo otorgar permisos a archivos y directorios?"
objectives:
- "Usar `chmod` para cambiar permisos de archivos."
keypoints:
- "`chmod` cambia permisos."
---

Unix controla quién puede leer, modificar y ejecutar archivos usando *permisos*.
Discutiremos cómo maneja Windows los permisos al final de la sección:
los conceptos son similares,
pero las reglas son diferentes.

Comencemos con Nelle.
Ella tiene un nombre de usuario único,
`nelle.nemo`,
Y un ID de usuario,
1404.

> ## ¿Por qué IDs con números enteros?
>
> ¿Por qué usar enteros para IDs?
> Una vez más, la respuesta se remonta a principios de los años setenta.
> Las cadenas de caracteres como `alan.turing` son de longitud variable,
> y comparar una a otro toma muchas instrucciones.
> Enteros,
> por otra parte,
> utilizan una cantidad bastante pequeña de almacenamiento (normalmente cuatro caracteres),
> y puede ser comparados con una sola instrucción.
> Para hacer operaciones rápidas y sencillas,
> los programadores suelen realizar un seguimiento de las cosas internamente usando números enteros,
> luego utilizan una tabla de búsqueda de algún tipo
> para traducir esos números enteros en un texto fácil de usar para su presentación.
> Por supuesto,
> los programadores siendo programadores,
> a menudo se saltarán la parte de usar el texto
> y sólo utilizan los enteros,
> de la misma manera que alguien que trabaja en un laboratorio podría hablar del Experimento 28
> en lugar de "los ensayos cronotípicos de respuesta alfa en anacondas".
{: .callout}

Los usuarios pueden pertenecer a cualquier número de grupos,
cada uno de los cuales tiene un nombre de grupo único 
y un identificador numérico o ID de grupo.
La lista de quién está en qué grupo se almacena normalmente en el archivo `/etc/group`.
(si está delante de una máquina Unix en este momento,
intente ejecutar `cat /etc/group` para ver ese archivo.)

Ahora veamos archivos y directorios.
Cada archivo y directorio en un equipo Unix pertenece a un propietario y un grupo.
Junto con el contenido de cada archivo,
el sistema operativo almacena los ID numéricos del usuario y del grupo que lo posee.

El modelo de usuario y grupo significa que
para cada archivo
cada usuario en el sistema cae en una de tres categorías:

1. el propietario del archivo,
2. alguien en el grupo del archivo, y
3. todos los demás.

Para cada una de estas tres categorías,
la computadora realiza un seguimiento de
si las personas de esa categoría pueden leer el archivo,
escribir en el archivo,
o ejecutar el archivo
(es decir, ejecutarlo si es un programa).

Por ejemplo, si un archivo tuviera el siguiente conjunto de permisos:

<table class="table table-striped">
<tr><td></td><th>user</th><th>group</th><th>all</th></tr>
<tr><th>read</th><td>yes</td><td>yes</td><td>no</td></tr>
<tr><th>write</th><td>yes</td><td>no</td><td>no</td></tr>
<tr><th>execute</th><td>no</td><td>no</td><td>no</td></tr>
</table>

esto significaría que:

* el propietario del archivo puede leerlo y escribirlo, pero no ejecutarlo;
* otras personas del grupo del archivo pueden leerlo, pero no modificarlo o ejecutarlo; y
* otras personas (no el grupo ni el propietario) no pueden hacer nada con él en absoluto.

Veamos este modelo en acción.
Si usamos `cd` en el directorio `labs` y ejecutamos `ls -F`,
pone un `*` al final del nombre de `setup`.
Esta es su forma de decirnos que `setup` es ejecutable,
es decir,
que es (probablemente) algo que el ordenador puede ejecutar.

~~~
$ cd labs
$ ls -F
~~~
{: .bash}

~~~
safety.txt    setup*     waiver.txt
~~~
{: .output}

> ## Necesario pero no suficiente
>
> El hecho de que algo este marcado como ejecutable
> en realidad no significa que contiene un programa de algún tipo.
> Podríamos marcar fácilmente este archivo HTML (de esta página) como ejecutable
> utilizando los comandos que se presentan a continuación.
> Dependiendo del sistema operativo que utilicemos,
> tratar de "ejecutarlo" no funcionará
> (porque no contiene instrucciones que la computadora reconozca)
> o puede hacer que el sistema operativo abra el archivo
> con cualquier aplicación que normalmente lo maneja
> (como un navegador web).
{: .callout}

Ahora vamos a ejecutar el comando `ls -l`:

~~~
$ ls -l
~~~
{: .bash}

~~~
-rw-rw-r-- 1 vlad bio  1158  2010-07-11 08:22 safety.txt
-rwxr-xr-x 1 vlad bio 31988  2010-07-23 20:04 setup
-rw-rw-r-- 1 vlad bio  2312  2010-07-11 08:23 waiver.txt
~~~
{: .output}

La opción `-l` le dice a `ls` que nos de una lista larga.
Es una gran cantidad de información, así que vamos a explicar las columnas una a la vez.

En el lado derecho, tenemos los nombres de los archivos.
junto a ellos,
moviéndose a la izquierda,
están los tiempos y fechas en que fueron modificados por última vez.
Los sistemas de respaldo y otras herramientas utilizan esta información de varias maneras,
pero tú puedes utilizarlo para saber cuando tú (o cualquier otra persona con permiso)
realizó la última modificación de un archivo.

Junto al tiempo de modificación se encuentra el tamaño del archivo en bytes
y los nombres del usuario y del grupo que lo posee
(en este caso, «vlad» y «bio», respectivamente).
Pasaremos por alto la segunda columna por ahora
(que muestra `1` para cada archivo)
porque es la primera columna la que nos concierne.
Esta muestra los permisos del archivo, es decir, quién lo puede leer, escribir o ejecutar.

Echemos un vistazo a una de esas cadenas de permisos:
`-rwxr-xr-x`.
El primer carácter nos dice qué tipo de cosa es esta:
'-' significa que es un archivo regular,
mientras que 'd' significa que es un directorio,
y otros caracteres significan cosas más esotéricas.

Los siguientes tres caracteres nos dicen qué permisos tiene el propietario del archivo.
En este caso, el propietario puede leer, escribir y ejecutar el archivo: `rwx`.
El triplete medio nos muestra los permisos del grupo.
Si el permiso está desactivado, vemos un guión, por lo que `r-x` significa "leer y ejecutar, pero no escribir".
El triplete final nos muestra lo que pueden hacer todos los que no son propietarios del archivo o no están en el grupo del archivo.
En este caso, es 'r-x' de nuevo, por lo que todo el mundo en el sistema puede ver el contenido del archivo y ejecutarlo.

Para cambiar los permisos, usamos el comando `chmod`
(cuyo nombre significa "modo de cambio" (change mode)).
Aquí está un listado de formato largo que muestra los permisos de las calificaciones finales del curso que Vlad está enseñando:

~~~
$ ls -l final.grd
~~~
{: .bash}

~~~
-rwxrwxrwx 1 vlad bio  4215  2010-08-29 22:30 final.grd
~~~
{: .output}

Ups: todo el mundo puede leerlo, y lo que es peor,
modificarlo
(también podrían tratar de ejecutar el archivo de calificaciones como un programa,
lo que casi con seguridad no funcionaría.)

El comando para cambiar los permisos del propietario a `rw-` es:

~~~ {.input}
$ chmod u=rw final.grd
~~~

La 'u' señala que estamos cambiando los privilegios
del usuario (es decir, el propietario del archivo),
y `rw` es el nuevo conjunto de permisos.
Un rápido `ls -l` nos muestra que funcionó,
ya que los permisos del propietario ahora están configurados para leer y escribir (pero no para ejecutar):

~~~
$ ls -l final.grd
~~~
{: .bash}

~~~
-rw-rwxrwx 1 vlad bio  4215  2010-08-30 08:19 final.grd
~~~
{: .output}

Vamos a ejecutar `chmod` de nuevo para dar al grupo permisos de sólo lectura:

~~~
$ chmod g=r final.grd
$ ls -l final.grd
~~~
{: .bash}


~~~
-rw-r--rw- 1 vlad bio  4215  2010-08-30 08:19 final.grd
~~~
{: .output}

Y finalmente,
vamos a quitarles los permisos a "todos" (todos en el sistema que no son ni el propietario del archivo o están su grupo):

~~~
$ chmod a= final.grd
$ ls -l final.grd
~~~
{: .bash}

~~~
-rw-r----- 1 vlad bio  4215  2010-08-30 08:20 final.grd
~~~
{: .output}

La 'a' indica que estamos cambiando permisos para "todos",
y, puesto que no hay nada a la derecha del "=",
los nuevos permisos de "todos" están vacíos.

También podemos buscar por permisos.
Aquí, por ejemplo, podemos usar `-type f -perm -u=x` para encontrar archivos
que el usuario puede ejecutar:

~~~
$ find . -type f -perm -u=x
~~~
{: .bash}

~~~
./tools/format
./tools/stats
~~~
{: .output}

Antes de ir más lejos,
Vamos a ejecutar `ls -a -l`
Para obtener un listado de formulario largo que incluye entradas de directorio que normalmente están ocultas:

~~~
$ ls -a -l
~~~
{: .bash}

~~~
drwxr-xr-x 1 vlad bio     0  2010-08-14 09:55 .
drwxr-xr-x 1 vlad bio  8192  2010-08-27 23:11 ..
-rw-rw-r-- 1 vlad bio  1158  2010-07-11 08:22 safety.txt
-rwxr-xr-x 1 vlad bio 31988  2010-07-23 20:04 setup
-rw-rw-r-- 1 vlad bio  2312  2010-07-11 08:23 waiver.txt
~~~
{: .output}

Los permisos para `.` y `..` (este directorio y su padre) comienzan con un 'd'.
pero mira el resto de sus permisos:
La 'x' significa que "ejecutar" está activado.
¿Qué significa eso?
Un directorio no es un programa, ¿cómo podemos "ejecutarlo"?

De hecho, 'x' significa algo diferente para los directorios.
Da a alguien el derecho a *recorrer* el directorio, pero no a mirar su contenido.
La distinción es sutil, así que echemos un vistazo a un ejemplo.
El directorio de inicio de Vlad tiene tres subdirectorios llamados `venus`, `mars` y `pluto`:

![Permisos de ejecución para directorios](../fig/x-for-directories.svg)




Cada uno de ellos tiene un subdirectorio llamado `notes`,
y esos subdirectorios contienen varios archivos.
Si los permisos de un usuario en `venus` son 'r-x',
entonces si ella trata de ver el contenido de `venus` y `venus/notes `usando` ls`,
la computadora le permite ver ambos.
Si sus permisos en `mars` son sólo 'r--',
entonces se le permite leer el contenido de `mars` y `mars/notes`.
Pero si sus permisos en `pluto` son sólo '-x',
No puede ver lo que hay en el directorio `pluto`:
`ls pluto` le dirá que no tiene permiso para ver su contenido.
Si ella trata de mirar en `pluto/notas`, sin embargo, la computadora se lo permitirá.
Se le permite pasar por `pluto`, pero no para mirar lo que hay.
Este truco da a la gente una forma de hacer que algunos de sus directorios sean visibles para todos
sin abrir todo lo demás.

#### ¿Qué hay de Windows?

Estos son los conceptos básicos de los permisos en Unix.
Como dijimos al principio, sin embargo, las cosas funcionan de manera diferente en Windows.
Allí, los permisos están definidos por listas de control de acceso,
o ACL.
Una ACL es una lista de pares, cada uno de los cuales combina un "quién" con un "qué".
Por ejemplo,
tú podrías dar a la Antonio permiso para agregar datos a un archivo sin darle permiso para leerlo o borrarlo,
y dar permiso a Tania para borrar un archivo sin poder ver lo que contiene.

Esto es más flexible que el modelo Unix,
pero también es más complejo de administrar y entender en sistemas pequeños
(si tú tienes un sistema de computadora grande,
*nada* es fácil de administrar o entender).
Algunas variantes modernas de Unix aceptan ACL, así como los permisos antiguos de lectura-escritura-ejecución,
pero casi nadie los utiliza.

> ## Challenge 
> Si `ls -l myfile.php` devuelve los siguientes detalles:
>
> ~~~
> -rwxr-xr-- 1 caro zoo  2312  2014-10-25 18:30 myfile.php
> ~~~
> {: .output}
> ¿Cuál de las siguientes afirmaciones es verdadera?
>
> 1. caro (el propietario) puede leer, escribir y ejecutar myfile.php
> 2. caro (el propietario) no puede escribir en myfile.php
> 3. Los miembros de caro (un grupo) pueden leer, escribir y ejecutar myfile.php
> 4. Los miembros de zoo (un grupo) no pueden ejecutar myfile.php
{: .challenge}
