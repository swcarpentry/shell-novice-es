---
title: Discussion
---

## Sopa de letras

Si el comando para averiguar quiénes somos es `whoami`, el comando para encontrar
donde deberíamos llamarnos `whereami`, ¿por qué es `pwd`
¿en lugar? La respuesta habitual es que a principios de la década de 1970, cuando Unix era
primero en desarrollo, cada tecla contada: los dispositivos del día
fueron lentos, y retroceder en un teletipo fue tan doloroso que cortar el
número de teclas para cortar el número de errores de tipeo fue
de hecho una victoria para la usabilidad. La realidad es que los comandos se agregaron a
Unix uno por uno, sin ningún plan maestro, por personas que estaban inmersas en
su jerga. El resultado es tan inconsistente como el roolz uv Inglish
Spelling, pero estamos atrapados con eso ahora.

## Códigos de control de trabajo

La terminal acepta algunos comandos especiales que permiten a los usuarios interactuar
con procesos o programas en ejecución. Puedes ingresar cada uno de estos
"códigos de control" presionando la tecla `Ctrl` y luego presionando uno
de los personajes de control. En otros tutoriales, puede ver el término
`Control` o `^` usado para representar la tecla `Ctrl` (por ejemplo,
Los siguientes son equivalentes "Ctrl-C", "Ctrl + C", "Control-C", "Control + C", "^ C".

- `Ctrl-C`:
      interrumpe y cancela un programa en ejecución.
      Esto es útil si desea cancelar un comando que tarda demasiado en ejecutarse.

- `Ctrl-D`:
      indica el final de un archivo o secuencia de caracteres que está ingresando en la línea de comando.
      Por ejemplo, vimos anteriormente que el comando `wc` cuenta líneas, palabras y caracteres en un archivo.
      Si simplemente escribimos `wc` y presionamos la tecla Enter sin proporcionar un nombre de archivo,
      entonces `wc` supondrá que queremos analizar todas las cosas que escribimos a continuación.
      Después de escribir nuestro magnum opus directamente en el intérprete de comandos del terminal,
      entonces podemos escribir Ctrl-D para decir `wc` que hemos terminado y nos gustaría ver los resultados del conteo de palabras.

- `Ctrl-Z`:
      Suspende un proceso pero no lo finaliza.
      A continuación, puede utilizar el comando `fg` para reiniciar el trabajo en primer plano.

Para los nuevos usuarios de terminal, todos estos códigos de control pueden parecer tener
el mismo efecto: hacen que las cosas "se vayan". Pero es útil
entiende las diferencias. En general, si algo salió mal y
solo quiere recuperar su mensaje de shell, es mejor usar
`Ctrl-C`.

## Otras terminals

Antes de que Bash se hiciera popular a fines de los años noventa, los científicos ampliamente
utilizado (y algunos todavía usan) otro terminal, C-shell o Csh. Bash y Csh
tienen conjuntos de características similares, pero sus reglas de sintaxis son diferentes y
esto los hace incompatibles entre sí. Algunas otras terminals tienen
apareció desde entonces, incluyendo ksh, zsh y un número de otros; son
mayormente compatible con Bash, y Bash es la terminal que se selecciona automáticamente en la mayoría
implementaciones modernas de Unix (incluida la mayoría de los paquetes que proporcionan
herramientas tipo Unix para Windows) pero si obtiene errores extraños en la terminal
guiones escritos por colegas, compruebe para ver qué terminal eran
escrito para.

## Bash Configuraciones

Desea personalizar rutas, variables de entorno, alias,
y otros comportamientos de tu termianl?
Esta excelente publicación de blog "[Bash Configurations Demystified] [bash-demystified]"
de Dalton Hubble
cubre consejos, trucos y cómo evitar peligros.

## Permisos

Unix controla quién puede leer, modificar y ejecutar archivos usando *permisos*.
Discutiremos cómo maneja Windows los permisos al final de la sección:
los conceptos son similares,
pero las reglas son diferentes.

Comencemos con Nelle.
Ella tiene un nombre de usuario único,
`nelle.nemo`,
Y un ID de usuario,
1404\.

:::::::::::::::::::::::::::::::::::::::::  callout

## ¿Por qué IDs con números enteros?

¿Por qué usar enteros para IDs?
Una vez más, la respuesta se remonta a principios de los años setenta.
Las cadenas de caracteres como `alan.turing` son de longitud variable,
y comparar una a otro toma muchas instrucciones.
Enteros,
por otra parte,
utilizan una cantidad bastante pequeña de almacenamiento (normalmente cuatro caracteres),
y puede ser comparados con una sola instrucción.
Para hacer operaciones rápidas y sencillas,
los programadores suelen realizar un seguimiento de las cosas internamente usando números enteros,
luego utilizan una tabla de búsqueda de algún tipo
para traducir esos números enteros en un texto fácil de usar para su presentación.
Por supuesto,
los programadores siendo programadores,
a menudo se saltarán la parte de usar el texto
y sólo utilizan los enteros,
de la misma manera que alguien que trabaja en un laboratorio podría hablar del Experimento 28
en lugar de "los ensayos cronotípicos de respuesta alfa en anacondas".


::::::::::::::::::::::::::::::::::::::::::::::::::

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

- el propietario del archivo puede leerlo y escribirlo, pero no ejecutarlo;
- otras personas del grupo del archivo pueden leerlo, pero no modificarlo o ejecutarlo; y
- otras personas (no el grupo ni el propietario) no pueden hacer nada con él en absoluto.

Veamos este modelo en acción.
Si usamos `cd` en el directorio `labs` y ejecutamos `ls -F`,
pone un `*` al final del nombre de `setup`.
Esta es su forma de decirnos que `setup` es ejecutable,
es decir,
que es (probablemente) algo que el ordenador puede ejecutar.

```bash
$ cd labs
$ ls -F
```

```output
safety.txt    setup*     waiver.txt
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Necesario pero no suficiente

El hecho de que algo este marcado como ejecutable
en realidad no significa que contiene un programa de algún tipo.
Podríamos marcar fácilmente este archivo HTML (de esta página) como ejecutable
utilizando los comandos que se presentan a continuación.
Dependiendo del sistema operativo que utilicemos,
tratar de "ejecutarlo" no funcionará
(porque no contiene instrucciones que la computadora reconozca)
o puede hacer que el sistema operativo abra el archivo
con cualquier aplicación que normalmente lo maneja
(como un navegador web).


::::::::::::::::::::::::::::::::::::::::::::::::::

Ahora vamos a ejecutar el comando `ls -l`:

```bash
$ ls -l
```

```output
-rw-rw-r-- 1 vlad bio  1158  2010-07-11 08:22 safety.txt
-rwxr-xr-x 1 vlad bio 31988  2010-07-23 20:04 setup
-rw-rw-r-- 1 vlad bio  2312  2010-07-11 08:23 waiver.txt
```

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

```bash
$ ls -l final.grd
```

```output
-rwxrwxrwx 1 vlad bio  4215  2010-08-29 22:30 final.grd
```

Ups: todo el mundo puede leerlo, y lo que es peor,
modificarlo
(también podrían tratar de ejecutar el archivo de calificaciones como un programa,
lo que casi con seguridad no funcionaría.)

El comando para cambiar los permisos del propietario a `rw-` es:

```bash
$ chmod u=rw final.grd
```

La 'u' señala que estamos cambiando los privilegios
del usuario (es decir, el propietario del archivo),
y `rw` es el nuevo conjunto de permisos.
Un rápido `ls -l` nos muestra que funcionó,
ya que los permisos del propietario ahora están configurados para leer y escribir (pero no para ejecutar):

```bash
$ ls -l final.grd
```

```output
-rw-rwxrwx 1 vlad bio  4215  2010-08-30 08:19 final.grd
```

Vamos a ejecutar `chmod` de nuevo para dar al grupo permisos de sólo lectura:

```bash
$ chmod g=r final.grd
$ ls -l final.grd
```

```output
-rw-r--rw- 1 vlad bio  4215  2010-08-30 08:19 final.grd
```

Y finalmente,
vamos a quitarles los permisos a "todos" (todos en el sistema que no son ni el propietario del archivo o están su grupo):

```bash
$ chmod a= final.grd
$ ls -l final.grd
```

```output
-rw-r----- 1 vlad bio  4215  2010-08-30 08:20 final.grd
```

La 'a' indica que estamos cambiando permisos para "todos",
y, puesto que no hay nada a la derecha del "=",
los nuevos permisos de "todos" están vacíos.

También podemos buscar por permisos.
Aquí, por ejemplo, podemos usar `-type f -perm -u=x` para encontrar archivos
que el usuario puede ejecutar:

```bash
$ find . -type f -perm -u=x
```

```output
./tools/format
./tools/stats
```

Antes de ir más lejos,
Vamos a ejecutar `ls -a -l`
Para obtener un listado de formulario largo que incluye entradas de directorio que normalmente están ocultas:

```bash
$ ls -a -l
```

```output
drwxr-xr-x 1 vlad bio     0  2010-08-14 09:55 .
drwxr-xr-x 1 vlad bio  8192  2010-08-27 23:11 ..
-rw-rw-r-- 1 vlad bio  1158  2010-07-11 08:22 safety.txt
-rwxr-xr-x 1 vlad bio 31988  2010-07-23 20:04 setup
-rw-rw-r-- 1 vlad bio  2312  2010-07-11 08:23 waiver.txt
```

Los permisos para `.` y `..` (este directorio y su padre) comienzan con un 'd'.
pero mira el resto de sus permisos:
La 'x' significa que "ejecutar" está activado.
¿Qué significa eso?
Un directorio no es un programa, ¿cómo podemos "ejecutarlo"?

De hecho, 'x' significa algo diferente para los directorios.
Da a alguien el derecho a *recorrer* el directorio, pero no a mirar su contenido.
La distinción es sutil, así que echemos un vistazo a un ejemplo.
El directorio de inicio de Vlad tiene tres subdirectorios llamados `venus`, `mars` y `pluto`:

![](fig/x-for-directories.svg){alt='Permisos de ejecución para directorios'}

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

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge

Si `ls -l myfile.php` devuelve los siguientes detalles:

```output
-rwxr-xr-- 1 caro zoo  2312  2014-10-25 18:30 myfile.php
```

¿Cuál de las siguientes afirmaciones es verdadera?

1. caro (el propietario) puede leer, escribir y ejecutar myfile.php
2. caro (el propietario) no puede escribir en myfile.php
3. Los miembros de caro (un grupo) pueden leer, escribir y ejecutar myfile.php
4. Los miembros de zoo (un grupo) no pueden ejecutar myfile.php
  

::::::::::::::::::::::::::::::::::::::::::::::::::

## Blast en la línea de comandos

[Blast](https://es.wikipedia.org/wiki/BLAST) es uno de los programas de alineamiento
de secuencias más populares. Se utiliza
para buscar similitud (homología) entre distintas secuencias
generando alineamientos locales de manera heurística. De hecho es tan popular
que las publicaciones que lo describen figuran entre los 20
artículos más citados del [mundo](https://www.nature.com/news/the-top-100-papers-1.16224).

### Configurando Blast

Instalamos Blast previamente en nuestro proceso de preparación para esta lección.
Verifiquemos que nuestra instalación de Blast funcionó correctamente escribiendo:

```bash
blastn
```

o en Windows:

```bash
blastn.exe
```

Si obtienes la siguiente salida, tu programa está bien configurado:

```output
BLAST query/options error: Either a BLAST database or subject sequence(s) must be specified
```

Primero creemos un directorio para almacenar nuestras bases de datos de blast:

```bash
mkdir $HOME/blastdb
```

Movamos el archivo de nuestra base de datos a ese directorio:

```bash
mv uniprot_sprot.fasta $HOME/blastdb/
cd $HOME/blastdb
```

Y cambiemos el formato de la base de datos que descargamos de [Uniprot](https://www.uniprot.org/)
para que Blast pueda usarla como datos de búsqueda.

```bash
makeblastdb -in uniprot_sprot.fasta -dbtype prot
```

Ahora, vamos a configurar la **variable de entorno** que especifica
en dónde se encuentra nuestra base de datos de Blast

```bash
export BLASTDB=$HOME/blastdb
```

¡Estamos listos!

### Usando Blast

Blast acepta como entrada un archivo tipo fasta similar a este:

```output
>MIMI_L4 complement(6238..7602)
MPQKTSKSKSSRTRYIEDSDDETRGRSRNRSIEKSRSRSLDKSQKKSRDK
SLTRSRSKSPEKSKSRSKSLTRSRSKSPKKCITGNRKNSKHTKKDNEYTT
EESDEESDDESDGETNEESDEELDNKSDGESDEEISEESDEEISEESDED
VPEEEYDDNDIRNIIIENINNEFARGKFGDFNVIIMKDNGFINATKLCKN
```

Blast cuenta con varios algoritmos que le permiten hacer búsquedas distintas:

1. **blastn** - consulta de nucleótidos vs base de datos de nucleótidos
2. **blastp** - consulta de proteínas vs base de datos de proteínas
3. **blastx** - consulta de nucleótidos traducidos a 6 marcos de lectura vs base de datos de proteínas
4. **tblastn** - consulta de proteínas vs base de datos de nucleótidos traducidos a 6 marcos de lectura
5. **tblastx** - consulta de nucleótidos traducidos a 6 marcos de lectura vs base de datos de nucleótidos traducidos a 6 marcos de lectura
6. **megaBlast** - búsqueda ultra rápida para ADN de alta similitud (95%)
7. **psiblast** - búsqueda iterativa de secuencias similares

Podemos explorar sus opciones usando:

```bash
blastn -h
```

```output
USAGE
  blastn [-h] [-help] [-import_search_strategy filename]
    [-export_search_strategy filename] [-task task_name] [-db database_name]
    [-dbsize num_letters] [-gilist filename] [-seqidlist filename]
    [-negative_gilist filename] [-entrez_query entrez_query]
    [-db_soft_mask filtering_algorithm] [-db_hard_mask filtering_algorithm]
    [-subject subject_input_file] [-subject_loc range] [-query input_file]
    [-out output_file] [-evalue evalue] [-word_size int_value]
    [-gapopen open_penalty] [-gapextend extend_penalty]
    [-perc_identity float_value] [-qcov_hsp_perc float_value]
    [-max_hsps int_value] [-xdrop_ungap float_value] [-xdrop_gap float_value]
    [-xdrop_gap_final float_value] [-searchsp int_value]
    [-sum_stats bool_value] [-penalty penalty] [-reward reward] [-no_greedy]
    [-min_raw_gapped_score int_value] [-template_type type]
    [-template_length int_value] [-dust DUST_options]
    [-filtering_db filtering_database]
    [-window_masker_taxid window_masker_taxid]
    [-window_masker_db window_masker_db] [-soft_masking soft_masking]
    [-ungapped] [-culling_limit int_value] [-best_hit_overhang float_value]
    [-best_hit_score_edge float_value] [-window_size int_value]
    [-off_diagonal_range int_value] [-use_index boolean] [-index_name string]
    [-lcase_masking] [-query_loc range] [-strand strand] [-parse_deflines]
    [-outfmt format] [-show_gis] [-num_descriptions int_value]
    [-num_alignments int_value] [-line_length line_length] [-html]
    [-max_target_seqs num_sequences] [-num_threads int_value] [-remote]
    [-version]

DESCRIPTION
   Nucleotide-Nucleotide BLAST 2.5.0+

Use '-help' to print detailed descriptions of command line arguments
```

Esto nos permitirá usar más opciones de Blast.

Entremos a nuestro directorio de trabajo:

```bash
cd ~/data-shell
mkdir blast_analysis
cd blast_analysis
```

## Un ejemplo de Blast

Examinemos un ejemplo de una búsqueda usando Blast. Este es el tipo de salida que se genera
por defecto:

```output
>ref|NM_009586.1| UniGene infoGeoGene info Homo sapiens single-minded homolog 2 (Drosophila) (SIM2), transcript
variant SIM2s, mRNA
Length=2823

 Score =  329 bits (178),  Expect = 6e-87
 Identities = 253/288 (87%), Gaps = 9/288 (3%)
 Strand=Plus/Plus

Query  6378  GTGGCTCACGCCTGTAATCCCAGCACTTTGGGAGGCCAAGGTGGGCAGATCAC-TGGAGG  6436
             ||||||||| |||||||||||||||||||||||||||||||||||| |||||| || |||
Sbjct  2541  GTGGCTCACACCTGTAATCCCAGCACTTTGGGAGGCCAAGGTGGGCGGATCACCTG-AGG  2599

Query  6437  TCAGGAGTTCGAAACCAGCCTGGCCAACATGGTGAAACCCCATCTCTACTAAAAATACAG  6496
             ||||||||| |  || |||||| |||||| | |||||||||||||| ||||||||||||
Sbjct  2600  TCAGGAGTTTGCGACAAGCCTG-CCAACAAGCTGAAACCCCATCTCCACTAAAAATACAA  2658

Query  6497  AAATTAGCCGGTCATGGTGGTG-GACACCTGTAATCCCAGCTACTCAGGTGGCTAAGGCA  6555
             |||||||  || |||||||||| | ||||||||||||||||||||| || |||| ||  |
Sbjct  2659  AAATTAGTTGGGCATGGTGGTGAG-CACCTGTAATCCCAGCTACTCTGGAGGCTGAGATA  2717

Query  6556  GGAGAATCACTTCAGCCCGGGAGGTGGAGGTTGCAGTGAGCCAAGATCATACCACGGCAC  6615
             |||| ||||||| | |||||||||||||||||||||||||| ||||||| | ||| ||||
Sbjct  2718  GGAGGATCACTTGAACCCGGGAGGTGGAGGTTGCAGTGAGCTAAGATCACATCACTGCAC  2777

Query  6616  TCCAGCCTGGGTGACAG--TGAGACTGTGGCTCAAAAAAAAAAAAAAA  6661
             |||||||||||| ||||  |||||||||  ||||||||||||||||||
Sbjct  2778  TCCAGCCTGGGTAACAGAGTGAGACTGT--CTCAAAAAAAAAAAAAAA  2823
```

Podemos ver que hay nucleótidos idénticos entre estas dos secuencias
(denotados por el símbolo '|'), así como nucleótidos diferentes (donde no hay
similitud), así como inserciones y deleciones (denotadas por guiones '-'). Al principio
y al final de cada línea del alineamiento se marcan las posiciones de inicio y término
en la secuencia de interés (**query**) así como en la secuencia encontrada (**subject**).

A pesar de que es muy informativa si tenemos una sola secuencia, no lo es tanto cuando hay
muchas secuencias que queremos analizar, por ello en el siguiente ejercicio usaremos un
formato diferente.

## Usando Blast

Realicemos una búsqueda usando Blast. Vamos a descargar el siguiente archivo [fasta](https://www.uniprot.org/uniprot/P38398.fasta).

```bash
wget http://www.uniprot.org/uniprot/P38398.fasta
```

```output
Resolving www.uniprot.org... 128.175.240.211, 193.62.193.81
Connecting to www.uniprot.org|128.175.240.211|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1997 (2.0K) [text/plain]
Saving to: 'P38398.fasta'

P38398.fasta                       100%[===============================================================>]   1.95K  --.-KB/s    in 0s

2017-01-13 00:07:01 (54.4 MB/s) - 'P38398.fasta' saved [1997/1997]
```

Y veamos las primeras líneas:

```bash
head P38398.fasta
```

```output
>sp|P38398|BRCA1_HUMAN Breast cancer type 1 susceptibility protein OS=Homo sapiens GN=BRCA1 PE=1 SV=2
MDLSALRVEEVQNVINAMQKILECPICLELIKEPVSTKCDHIFCKFCMLKLLNQKKGPSQ
CPLCKNDITKRSLQESTRFSQLVEELLKIICAFQLDTGLEYANSYNFAKKENNSPEHLKD
EVSIIQSMGYRNRAKRLLQSEPENPSLQETSLSVQLSNLGTVRTLRTKQRIQPQKTSVYI
ELGSDSSEDTVNKATYCSVGDQELLQITPQGTRDEISLDSAKKAACEFSETDVTNTEHHQ
PSNNDLNTTEKRAAERHPEKYQGSSVSNLHVEPCGTNTHASSLQHENSSLLLTKDRMNVE
KAEFCNKSKQPGLARSQHNRWAGSKETCNDRRTPSTEKKVDLNADPLCERKEWNKQKLPC
SENPRDTEDVPWITLNSSIQKVNEWFSRSDELLGSDDSHDGESESNAKVADVLDVLNEVD
EYSGSSEKIDLLASDPHEALICKSERVHSKSVESNIEDKIFGKTYRKKASLPNLSHVTEN
LIIGAFVTEPQIIQERPLTNKLKRKRRPTSGLHPEDFIKKADLAVQKTPEMINQGTNQTE
```

Y le vamos a cambiar el nombre:

```bash
mv P38398.fasta BRCA1_HUMAN.fasta
```

Ejecutemos nuestra primera búsqueda de Blast usando el siguiente comando:

```bash
blastp -query BRCA1_HUMAN.fasta -db uniprot_sprot.fasta -outfmt 7 -max_target_seqs 5
```

¿Qué significa cada una de las opciones que utilizamos?

- `-query` se refiere al archivo con la secuencia a la cual le queremos encontrar secuencias homólogas.
  Debe estar en formato fasta.
- `-db` Se refiere a la base de datos que queremos usar.
- `-outfmt` se refiere al formato de salida deseamos, en este caso el tipo 7, que es en columnas con
  comentarios.
- `-max_target_seqs` Nos indica que se reportarán máximo 5 'hits' o secuencias similares a nuestra
  secuencia de interés. Sólo se muestran las 5 secuencias con mayores niveles de identidad.

¿Por qué utilizamos blastp?

```output
# BLASTP 2.5.0+
# Query: sp|P38398|BRCA1_HUMAN Breast cancer type 1 susceptibility protein OS=Homo sapiens GN=BRCA1 PE=1 SV=2
# Database: uniprot_sprot.fasta
# Fields: query acc., subject acc., % identity, alignment length, mismatches, gap opens, q. start, q. end, s. start, s. end, evalue, bit score
# 5 hits found
sp|P38398|BRCA1_HUMAN	sp|P38398|BRCA1_HUMAN	100.000	1863	0	0	1	1863	1	1863	0.0	3844
sp|P38398|BRCA1_HUMAN	sp|Q9GKK8|BRCA1_PANTR	98.443	1863	29	0	1	1863	1	1863	0.0	3773
sp|P38398|BRCA1_HUMAN	sp|Q6J6I8|BRCA1_GORGO	98.014	1863	37	0	1	1863	1	1863	0.0	3757
sp|P38398|BRCA1_HUMAN	sp|Q6J6J0|BRCA1_PONPY	96.833	1863	59	0	1	1863	1	1863	0.0	3699
sp|P38398|BRCA1_HUMAN	sp|Q6J6I9|BRCA1_MACMU	93.026	1864	128	2	1	1863	1	1863	0.0	3504
# BLAST processed 1 queries
```

La salida nos muestra las 5 secuencias con mayor identidad a nuestra secuencia de interés, así como
información acerca de cada una de estas secuencias:

1. `query acc.` - El nombre de nuestra secuencia de interés.
2. `subject acc.` - El nombre de la secuencia con identidad a nuestra secuencia de interés.
3. `% identity` - El porcentaje de identidad.
4. `alignment length` - La longitud del alineamiento.
5. `mismatches` - El número de posiciones diferentes (**mismatches**).
6. `gap opens` - El número de gaps.
7. `q. start` - La posición de inicio en nuestra secuencia de interés.
8. `q. end` - La posición de término en nuestra secuencia de interés.
9. `s. start` - La posición de inicio en la secuencia en la base de datos.
10. `s. end` - La posición de término en la secuencia en la base de datos.
11. `evalue` - El **e-value**.
12. `bit score` - El **bit score**.

Ahondemos un poco más en el significado de los últimos dos campos.

# El e-value

El valor esperado o **e-value** es el número de secuencias que obtendrían el mismo nivel de homología sólo por azar
dado el tamaño de la base de datos que estamos usando. Por lo tanto, mientras más cercano a
cero sea este valor, más significativo es, es decir, hay pocas posibilidades de que encontremos
un nivel de similitud de secuencias como el observado por azar. Por lo tanto, mientras más grande sea la base
de datos de secuencias que estamos usando, más significativos serán los valores esperados.

Este valor se usa comúnmente para filtrar resultados de Blast y sólo obtener aquellos que
son probablemente debidos a que los organismos que las contienen comparten un ancestro común y
no al hecho de que tenemos una base de datos enorme.

# El bit score

El **bit score** sirve como indicador de qué tan bueno es un alineamiento, mientras más alto
sea esta métrica, mejor será el alineamiento. Este se calcula por medio de una fórmula que
toma en cuenta cuántos residuos similares existen en el alineamiento así como el número de gaps
en el mismo. Es comparable entre bases de datos de diferente tamaño.

Hay muchas más opciones de Blast, para ello debes revisar el manual que ese encuentra en
NCBI - [https://www.ncbi.nlm.nih.gov/books/NBK279690/](https://www.ncbi.nlm.nih.gov/books/NBK279690/).

## Proyecto Final Opcional

Usando los comandos disponibles en esta lección deberán escribir un **script** que,
a partir únicamente de tres archivos fasta, genere la siguiente información.

1. El nombre del archivo más largo (en número de caracteres).
2. Un archivo concatenado de todas las secuencias.
3. Un análisis de este archivo con:
  1. El numero de líneas en el archivo,
  2. El numero de caracteres en el archivo,
  3. El número total de secuencias,
  4. El nombre de la penúltima secuencia,
  5. El número de secuencias únicas (sin importar sus nombres).

Por ejemplo:

Dados los siguientes archivos fasta [Proteins\_1.fasta](files/final_project/example/Proteins_1.fasta),
[Proteins\_2.fasta](files/final_project/example/Proteins_2.fasta), [Proteins\_3.fasta](files/final_project/example/Proteins_3.fasta),

```bash
bash fasta_analysis_script_ID.sh *.fasta
```

Generará el siguiente archivo [Complete\_ID.fasta](files/final_project/example/Complete_ID.fasta) e imprimirá en la terminal:

```output
Longest file: Proteins_1.fasta
Concatenated file name: Complete_ID.fasta

Analysis of Complete_ID.fasta
  Number of lines: 600
  Number of characters: 207076
  Number of sequences: 300
  Name of second to last sequence: XP_012933189.1_PREDICTED:_calcium-binding_and_coiled-coil_domain-containing_protein_2_isoform_X2
  Number of unique sequences: 298
```

- La descripciones en la salida deberán ser idénticas a las mostradas arriba.
- El **script** deberá imprimir a salida estándar.
- Deberá funcionar con cualquier otro archivo o archivos fasta, diferentes a los proporcionados.
- Deberá estar comentado, explicando brevemente qué hace el **script** y qué realiza cada línea.

Todas las ocurrencias de 'ID' deberán ser substituidas por un identificador, pueden usar un número entre 1 y 16. No tienen que hacer este ID variable, es decir, no es necesario que el ID se pueda cambiar en la línea de comandos cuando se ejecuta el script.

Los paquetes de archivos los pueden descargar de:

`https://software-carpentry.github.io/shell-novice-es/final_project/data/Fastas_<identificador_de_estudiante>.tar.gz`

Y descomprimirlos con tar de la siguiente manera, por ejemplo, para el estudiante con ID=1:

```bash
tar -xvf Fastas_1.tar.gz
```

Éste generará un directorio llamado `Fastas_1` dentro del cual realizarán sus análisis.

Se entregarán dos archivos:

1. fasta\_analysis\_script\_`<identificador_de_estudiante>`.sh (e.g. fasta\_analysis\_script\_5.sh)
2. Complete\_`<identificador_de_estudiante>`.fasta (e.g. Complete\_5.fasta)

#### Reto extra

Intenta imprimir un menú de
ayuda si se ejecuta el **script** sin opciones.

#### Tips

Para borrar uno o más espacios al inicio de la línea se puede utilizar:

```bash
sed 's/ *//'
```

[bash-demystified]: https://blog.dghubble.io/post/.bashprofile-.profile-and-.bashrc-conventions/



