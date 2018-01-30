---
layout: page
title: "Discussion"
permalink: /discuss/
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

El terminal acepta algunos comandos especiales que permiten a los usuarios interactuar
con procesos o programas en ejecución. Puedes ingresar cada uno de estos
"códigos de control" presionando la tecla `Ctrl` y luego presionando uno
de los personajes de control. En otros tutoriales, puede ver el término
`Control` o `^` usado para representar la tecla `Ctrl` (por ejemplo,
Los siguientes son equivalentes "Ctrl-C", "Ctrl + C", "Control-C", "Control + C", "^ C".

* `Ctrl-C`:
    interrumpe y cancela un programa en ejecución.
    Esto es útil si desea cancelar un comando que tarda demasiado en ejecutarse.

* `Ctrl-D`:
    indica el final de un archivo o secuencia de caracteres que está ingresando en la línea de comando.
    Por ejemplo, vimos anteriormente que el comando `wc` cuenta líneas, palabras y caracteres en un archivo.
    Si simplemente escribimos `wc` y presionamos la tecla Enter sin proporcionar un nombre de archivo,
    entonces `wc` supondrá que queremos analizar todas las cosas que escribimos a continuación.
    Después de escribir nuestro magnum opus directamente en el intérprete de comandos del terminal,
    entonces podemos escribir Ctrl-D para decir `wc` que hemos terminado y nos gustaría ver los resultados del conteo de palabras.

* `Ctrl-Z`:
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
mayormente compatible con Bash, y Bash es el terminal que se selecciona automáticamente en la mayoría
implementaciones modernas de Unix (incluida la mayoría de los paquetes que proporcionan
herramientas tipo Unix para Windows) pero si obtiene errores extraños en el terminal
guiones escritos por colegas, compruebe para ver qué terminal eran
escrito para.

## Bash Configuraciones

Desea personalizar rutas, variables de entorno, alias,
y otros comportamientos de tu termianl?
Esta excelente publicación de blog "[Bash Configurations Demystified] [bash-demystified]"
de Dalton Hubble
cubre consejos, trucos y cómo evitar peligros.

[bash-demystified]: http://dghubble.com/blog/posts/.bashprofile-.profile-and-.bashrc-conventions/