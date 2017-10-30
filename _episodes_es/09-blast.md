---
title: "Blast en la línea de comandos"
teaching: 15
exercises: 0
questions:
- "¿Qué es un análisis de Blast?"
- "¿Cómo puedo realizar una búsqueda de Blast?"
objectives:
- "Entender qué tipo de búsquedas realiza Blast."
- "Configurar bases de datos para usar Blast en la línea de comandos."
- "Realizar descargas de secuencias desde la línea de comandos."
- "Realizar búsquedas de Blast locales desde la línea de comandos."
- "Interpretar los resultados de una búsqueda de Blast."
keypoints:
- "Blast busca similitud entre secuencias realizando un alineamiento local heurístico"
- "Se pueden realizar búsquedas de Blast en la línea de comandos"
---

[Blast](https://es.wikipedia.org/wiki/BLAST) es uno de los programas de alineamiento
de secuencias más populares. Se utiliza
para buscar similitud (homología) entre distintas secuencias
generando alineamientos locales de manera heurística. De hecho es tan popular
que las publicaciones que lo describen figuran entre los 20
artículos más citados del [mundo](http://www.nature.com/news/the-top-100-papers-1.16224).

### Configurando Blast

Instalamos Blast+ previamente en nuestro proceso de preparación para esta lección.
Verifiquemos que nuestra instalación de Blast funcionó correctamente escribiendo:

~~~
blastn
~~~
{: .bash}

o en Windows:

~~~
blastn.exe
~~~
{: .bash}

Si obtienes la siguiente salida, tu programa está bien configurado:

~~~
BLAST query/options error: Either a BLAST database or subject sequence(s) must be specified
~~~
{: .output}

Primero creemos un directorio para almacenar nuestras bases de datos de blast:

~~~
mkdir $HOME/blastdb
~~~
{: .bash}

Movamos el archivo de nuestra base de datos a ese directorio:

~~~
mv uniprot_sprot.fasta $HOME/blastdb/
cd $HOME/blastdb
~~~
{: .bash}

Y cambiemos el formato de la base de datos que descargamos de [Uniprot](http://www.uniprot.org/)
para que Blast pueda usarla como datos de búsqueda.

~~~
makeblastdb -in uniprot_sprot.fasta -dbtype prot
~~~
{: .blast}

Ahora, vamos a configurar la **variable de entorno** que especifica
en dónde se encuentra nuestra base de datos de Blast

~~~
export BLASTDB=$HOME/blastdb
~~~
{: .bash}

¡Estamos listos!

### Usando Blast

Blast acepta como entrada un archivo tipo fasta similar a este:

~~~
>MIMI_L4 complement(6238..7602)
MPQKTSKSKSSRTRYIEDSDDETRGRSRNRSIEKSRSRSLDKSQKKSRDK
SLTRSRSKSPEKSKSRSKSLTRSRSKSPKKCITGNRKNSKHTKKDNEYTT
EESDEESDDESDGETNEESDEELDNKSDGESDEEISEESDEEISEESDED
VPEEEYDDNDIRNIIIENINNEFARGKFGDFNVIIMKDNGFINATKLCKN
~~~
{: .output}

Blast cuenta con varios algoritmos que le permiten hacer búsquedas distintas:

1. **blastn** - consulta de nucleótidos vs base de datos de nucleótidos
2. **blastp** - consulta de proteínas vs base de datos de proteínas
3. **blastx** - consulta de nucleótidos traducidos a 6 marcos de lectura vs base de datos de proteínas
4. **tblastn** - consulta de proteínas vs base de datos de nucleótidos traducidos a 6 marcos de lectura
5. **tblastx** - consulta de nucleótidos traducidos a 6 marcos de lectura vs base de datos de nucleótidos traducidos a 6 marcos de lectura
6. **megaBlast** - búsqueda ultra rápida para ADN de alta similitud (95%)
7. **psiblast** - búsqueda iterativa de secuencias similares

Podemos explorar sus opciones usando:

~~~
blastn -h
~~~
{: .bash}

~~~
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
~~~
{: .output}

Esto nos permitirá usar más opciones de Blast.

Entremos a nuestro directorio de trabajo:

~~~
cd ~/data-shell
mkdir blast_analysis
cd blast_analysis
~~~
{: .bash}

## Un ejemplo de Blast

Examinemos un ejemplo de una búsqueda usando Blast. Este es el tipo de salida que se genera
por defecto:

~~~
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
~~~
{: .output}

Podemos ver que hay nucleótidos idénticos entre estas dos secuencias
(denotados por el símbolo '|'), así como nucleótidos diferentes (donde no hay
similitud), así como inserciones y deleciones (denotadas por guiones '-'). Al principio
y al final de cada línea del alineamiento se marcan las posiciones de inicio y término
en la secuencia de interés (**query**) así como en la secuencia encontrada (**subject**).

A pesar de que es muy informativa si tenemos una sola secuencia, no lo es tanto cuando hay
muchas secuencias que queremos analizar, por ello en el siguiente ejercicio usaremos un
formato diferente.

## Usando Blast

Realicemos una búsqueda usando Blast. Vamos a descargar el siguiente archivo [fasta](http://www.uniprot.org/uniprot/P38398.fasta).

~~~
wget http://www.uniprot.org/uniprot/P38398.fasta
~~~
{: .bash}

~~~
Resolving www.uniprot.org... 128.175.240.211, 193.62.193.81
Connecting to www.uniprot.org|128.175.240.211|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1997 (2.0K) [text/plain]
Saving to: 'P38398.fasta'

P38398.fasta                       100%[===============================================================>]   1.95K  --.-KB/s    in 0s

2017-01-13 00:07:01 (54.4 MB/s) - 'P38398.fasta' saved [1997/1997]
~~~
{: .output}

Y veamos las primeras líneas:

~~~
head P38398.fasta
~~~
{: .bash}

~~~
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
~~~
{: .output}

Y le vamos a cambiar el nombre:

~~~
mv P38398.fasta BRCA1_HUMAN.fasta
~~~
{: .bash}

Ejecutemos nuestra primera búsqueda de Blast usando el siguiente comando:

~~~
blastp -query BRCA1_HUMAN.fasta -db uniprot_sprot.fasta -outfmt 7 -max_target_seqs 5
~~~
{: .bash}

¿Qué significa cada una de las opciones que utilizamos?

* `-query` se refiere al archivo con la secuencia a la cual le queremos encontrar secuencias homólogas.
Debe estar en formato fasta.
* `-db` Se refiere a la base de datos que queremos usar.
* `-outfmt` se refiere al formato de salida deseamos, en este caso el tipo 7, que es en columnas con
comentarios.
* `-max_target_seqs` Nos indica que se reportarán máximo 5 'hits' o secuencias similares a nuestra
secuencia de interés. Sólo se muestran las 5 secuencias con mayores niveles de identidad.

¿Por qué utilizamos blastp?

~~~
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
~~~
{: .output}

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
NCBI - https://www.ncbi.nlm.nih.gov/books/NBK279690/.
