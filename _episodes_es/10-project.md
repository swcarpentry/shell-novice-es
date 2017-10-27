---
título: "Proyecto Final"
preguntas:
- "¿Puedes aplicar lo aprendido?"
puntos clave:
- "Si hay dos proyectos iguales (o extremadamente parecidos) su resultado será descartado"
---

Usando los comandos disponibles en esta lección deberán escribir un script que, 
a partir únicamente de tres archivos fasta, genere la siguiente información: 

1. El nombre del archivo más largo (en número de caracteres) 
2. Un archivo concatenado de todas las secuencias. 
3. Un análisis de este archivo con:
	1. El numero de líneas en el archivo,
	2. El numero de caracteres en el archivo,
	3. El número total de secuencias,
	4. El nombre de la penúltima secuencia,
	5. El número de secuencias únicas (sin importar sus nombres) 

Por ejemplo:

Dados los siguientes archivos fasta [Proteins_1.fasta](https://liz-fernandez.github.io/PBI_linux_shell/final_project/example/Proteins_1.fasta), 
[Proteins_2.fasta](https://liz-fernandez.github.io/PBI_linux_shell/final_project/example/Proteins_2.fasta), [Proteins_3.fasta](https://liz-fernandez.github.io/PBI_linux_shell/final_project/example/Proteins_3.fasta), 

~~~
bash fasta_analysis_script_ID.sh *.fasta
~~~
{: .bash}

Generará el siguiente archivo [Complete_ID.fasta](https://liz-fernandez.github.io/PBI_linux_shell/final_project/example/Complete_ID.fasta) e imprimirá a la terminal:

~~~ 
Longest file: Proteins_1.fasta
Concatenated file name: Complete_ID.fasta

Analysis of Complete_ID.fasta
  Number of lines: 600
  Number of characters: 207076
  Number of sequences: 300
  Name of second to last sequence: XP_012933189.1_PREDICTED:_calcium-binding_and_coiled-coil_domain-containing_protein_2_isoform_X2
  Number of unique sequences: 298
~~~
{: .output}

* La descripciones en la salida deberán ser idénticas a las mostradas arriba.
* El script deberá imprimir la salida estándar. 
* Deberá funcionar con cualquier otro archivo o archivos fasta, diferentes a los proporcionados. 
* Deberá estar comentado, explicando brevemente qué hace el script y qué realiza cada línea. 

Todas las ocurrencias de 'ID' deberán ser substituidas por su identificador de estudiante, proporcionado por 
correo electrónico. No tienen que hacer este ID variable, es decir, si el ID no se puede cambiar en la 
línea de comandos cuando se ejecuta el script está bien.  

Los paquetes de archivos los pueden descargar de:

`https://liz-fernandez.github.io/PBI_linux_shell/final_project/data/Fastas_<identificador_de_estudiante>.tar.gz`

Y descomprimirlos con tar de la siguiente manera, por ejemplo, para el estudiante con ID=1:
~~~
tar -xvf Fastas_1.tar.gz
~~~
{: .bash}

Este generará un directorio llamado `Fastas_1` dentro del cual realizarán sus análisis.

Se entregarán dos archivos:

1. fasta_analysis_script_`<identificador_de_estudiante>`.sh (e.g. fasta_analysis_script_5.sh)
2. Complete_`<identificador_de_estudiante>`.fasta (e.g. Complete_5.fasta)

Se recibirán vía [correo electrónico](mailto:selene.fernandez@cinvestav.mx) a más tardar
el día sábado 29 de abril a la media noche. El correo debe incluir ambos archivos. 
**No hay prórrogas**. 

El día viernes 28 de abril estará abierto el aula de biocómputo para que prueben su 
script antes de enviarlo. 

#### Puntaje extra

Se dará medio punto extra (sobre 10 en la sección de linux) a aquel que imprima un menú de 
ayuda si se ejecuta el script sin opciones. 

#### Pista

Para borrar uno o más espacios al inicio de la línea se puede utilizar:

~~~ 
sed 's/ *//'
~~~
{: .bash}






