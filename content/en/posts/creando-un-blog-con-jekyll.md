+++
title = 'Creando Un Blog Con Jekyll'
date = "2019-10-10 00:04:40 -0300"
categories = ["jekyll", "update","archive"]
author = "Cristian Guevara"
draft = false
+++

## Resumen

Leyendo este post aprenderás: como crear un sitio web utilizando la herramienta jekyll. Para llegar a este objetivo, también aprenderás un poquito de: Linea de Comandos de windows, que es el lenguaje de programación ruby, que es el gestor de paquetes gem (porque te voy a enseñar a bajartelos y usarlos), como se desarrolla una investigación en internet para apoyar un diseño, y los criterios que utilicé para tomar decisiones con el objetivo de crear este blog.   

## Introduccion

En este post abordaré de manera lo más completa, pero a la vez pedagógica posible la creación de un blog utilizando jekyll.

Eso incluye la etapa de "diseño", scopear el proyecto, estudiar pros y contras de alternativas, discutir brevemente las distintas razones por las que hice el proyecto en jekyll, y luego la parte de implementacion en la cual se ejecuta la planificacion y el diseño derivados arriba.

## Diseñando el Blog

Primero, busqué blogs sobre tecnología que me gustaban y que tenia como referente. El recurso que me ayudó
fue el proyecto de github engineering blogs "a curated list of engineering blogs"; dentro de los que más me llaman 
la atencion y me motivaron a nivel de empresas fueron: el de UBER y el de Instagram. Es increible para la
motivación ver el 1er post del blog técnico de instagram, cuyo título habla por sí mismo:

"Keeping Instagram up with over a million new users in twelve hours"

## Delimitando Alternativas

De la fase previa, vi que habían varias alternativas: django, wordpress, jekyll, pelican, vuepress, y una
pila de otras. Acá se revisan varias fuentes, tales como otros blogs, foros, comunidades de distinto tipo
(Si, stackoverflow, quora, listas de mails, todas son comunidades). También tengo que revisar material y
hacer una estimación gruesa de los costos de cada curso de acción. Por ejemplo, cuanto me voy a demorar,
¿es muy dificil de aprender?¿hay recursos para preguntar?

Como explicacion de alto nivel, creo que django es mucho para lo que quiero hacer. Sirve para otras cosas, más
potentes, además tiene otra curva de aprendizaje y empresas como Cornershop solicitan en sus perfiles Django 
por ejemplo*

## ¿Por qué jekyll?

Mi primera razón y más importante, es que los sitios que más me gustaron en mi búsqueda estaban
desarrollados con esta herramienta. Como hago esto porque quiero, y quiero entretenerme haciendo este blog
es una razón suficientemente válida. Sin embargo, como es un blog que busca ser pedagógico y técnico a la vez,
tienen que haber razones que se linkeen a lo pedagógico y a lo técnico (sipo, quiero que sea un blog serio, no chamullo)

En segundo lugar, es una forma de trabajar ruby sin tener necesariamente que dejar estancados mis otros proyectos.
Ruby me llama la atención porque tiene buena literatura, rails tiene súper buena fama, y al parecer es 
fácil de trabajar (o sea, es eficiente en términos de time-to-market: gasto poco tiempo en aprenderlo
y logro el objetivo que es sacar este blog lo antes posible). No por nada Richard de Sylicon Valley le dice a su doctor
que aprendió Ruby on Rails en un fin de semana cuando tenía 17 años. 

En síntesis, lo aprendo como habilitador de Jekyll y ya está, excusa perfecta para trabajar con ruby. Una tarea hecha a la medida de ruby.
O sea, en verdad, django es re interesante pero creo que me demoraría más y no lo pasaría tan bien en el proceso, al final
es sólo para hacer un blog. Wordpress, no me gusta porque: vulnerabilidades conocidas, y

En tercer lugar, Jekyll es un generador de sitios estáticos que no tiene base de datos. En la comparativa con Wordpress por ejemplo
hay muchos más plugins, pero eso también hace que se debilite la postura de seguridad del blog debido a que los plugins de wordpress
son de una calidad muy variable. (como los paquetes estadisticos rebuscados en r que nadie les da mantenimiento*). El que la alternativa
en wordpress tenga plugins de calidad variable hace que tenga que invertir tiempo en hardening que no quiero invertir sólo para tener un blog,
o sea en la parte de mantenimiento y actualizaciones es un potencial cacho que prefiero no tener.  

En cuarto lugar, hay muchas opciones de implementarlo: en windows windows, en debian dentro de windows con wsl, en docker dentro de debian dentro de
windows, además de debian sólo. Como el blog quiere comunicar, ser pedagógico y técnico, es bueno que el público objetivo más ligado al mundo de 
la FEN por ejemplo pueda implementarlo asi como tambien alguien con un trasfondo más técnico se pueda dar el gustito de implementar el blog como a él
le guste o simplemente encuentre un contenido valorable al leer esta entrada. Es bueno que tenga esa versatilidad. 

En quinto lugar, tiene suficiente tiempo siendo usado para tener un montón de referencias. Esto se respalda en la calidad de los blogs en la revisión inicial
del proyecto. 

Si bien pelican u otras alternativas estan teniendo auge en este mismo instante*, tiene un contra muy importante que es 
la curva de aprendizaje y el ecosistema porque luego de hacer el blog, voy a querer mantenerlo o agregarle 
cosas. Y en verdad,quiero que haya un acervo cultural comunitario de fácil acceso que me permita que sea
sustentable la mantencion y el agregarle cosas al blog. Por ejemplo, lo primero que quiero hacer es que pueda estar en varios idiomas. Entonces,
claramente es una ventaja relativa de jekyll que sea de fácil acceso e implementación una alternativa para hacer que el blog este en varios idiomas. 

Por lo tanto, lo voy a hacer en jekyll.

## IMPLEMENTACION

La primera parte es tomar la decision de como lo voy a instalar. Como esta expuesto más arriba, el objetivo del
blog es técnico, pedagógico, esta orientado a esas dos audiencias y jekyll me da la versatilidad de que puede explicarse
en fácil pero si quiero algo más sofisticado lo puedo hacer. Y por sobre todas las cosas funciona.

 Entonces, me queda mejor linkeado con los objetivos del proyecto para este primer post hacer la implementacion desde
windows-windows de forma que si alguien de la FEN esta leyendo esto desde la biblioteca o cualquier sala, lo pueda implementar
ahi mismo. Sin necesidad de instalar cosas raras, facilito, bien explicado. Pero con contenido que aporta a alguien que busca
algo más. 

### Paso 1.- Instalar dependencias.

 Primero, tengo que tener claro todo lo que tengo que instalar. Pero al mismo tiempo, tengo que tener claras las partes. Segundo,
no asustarse si algo sale mal sino ver como se soluciona. Las dependencias son: ruby, gem, jekyll y bundler.

Entonces, primero voy a la página de ruby y me descargo el ruby installer. Me abre el wizard de instalación. 

Luego, se abre una aplicacion de linea de comandos. Tengo que meter las opciones 1, 2 y 3. Se demora, y es normal.

Ahora, apreto win+R escribo CMD y apreto enter. Se va a abrir el terminal de windows 10.

Entonces, ahora compruebo que se haya instalado todo correctamente corriendo los siguientes comandos:

{{< highlight ruby "linenos=inline, hl_lines=1, style=dracula" >}}
ruby -v
gem -v
{{< /highlight >}}

ruby es el lenguaje de programacion con que esta escrito jekyll, sin ruby el computador no entiende el lenguaje en que esta
escrito. gem es un gestor de paquetes de ruby. Con esto nos vamos a bajar jekyll y bundler.


{{< highlight ruby "linenos=inline, hl_lines=1, style=dracula" >}}
gem install jekyll bundler
{{< /highlight >}}

comprobamos que se haya bajado bien:

{{< highlight ruby "linenos=inline, hl_lines=1, style=dracula" >}}
jekyll
{{< /highlight >}}

ahora, aprovechamos que estamos en la terminal y creamos desde ahi una carpeta. Una forma de llamar a las carpetas es directorio, y
es la forma en que el computador organiza los archivos, como un arbolito que tiene muchas ramas (directorios) y de esas ramas salen 
hojas (archivos). Ahora, lo que vamos a hacer es crear un directorio con un comando desde el terminal, vamos a movernos a ese 
directorio desde el terminal, es decir vamos a decirle al terminal que trabaje con ese directorio (working directory).

{{< highlight powershell "style=dracula" >}}
mkdir proyecto
{{< /highlight >}}

creamos el directorio proyecto

{{< highlight powershell "style=dracula" >}}
cd proyecto
pwd
{{< /highlight >}}

nos movemos a proyecto. CD quiere decir "change directory". pwd es un comando muy util que quiere decir "print working directory", y
nos dice cual es el directorio con el que el terminal esta trabajando. Es muy útil porque jekyll es una CLI, una aplicacion que corre
desde la linea de comandos. Entonces es buena práctica tener claro en que directorio estoy antes de ejecutar un comando desde el 
terminal, más aun cuando me va a crear carpetas y archivos.

Ahora que estamos trabajando en proyecto, corro el comando :

{{< highlight powershell "style=dracula" >}}
jekyll new nombre_blog
{{< /highlight >}}

eso le dice a jekyll que me haga un nuevo blog. eso consiste en que me haga un directorio que a su vez contiene otros directorios y archivos.
Esa estructura de directorios le dice al programa donde esta cada cosa que quiero modificar desde mi página web. Imagina que lo que acabas de 
hacer es obtener una caja llena de herramientas y piezas para armar un motorcito. La herramienta es jekyll, y las piezas son los directorios
y archivos contenidos en el directorio nombre_proyecto.

Ahora, corremos el siguiente comando:

{{< highlight powershell "style=dracula" >}}
bundle exec jekyll serve
{{< /highlight >}}

entonces, le digo a la herramienta bundle que ejecute el comando exec, y a jekyll que ejecute serve. Eso nos va a levantar un servidor
en localhost. Entonces, ahora voy a mi browser favorito: chrome, opera, firefox, chromium, el que sea y en la barra escribo esta direccion:

http://127.0.0.1:4000

apreto enter y listo, ya cree el blog. 

## REFERENCIAS

https://github.com/kilimchoi/engineering-blogs

https://instagram-engineering.com/keeping-instagram-up-with-over-a-million-new-users-in-twelve-hours-b75298b782c5

https://eng.uber.com/measuring-kotlin-build-performance/

### Referencia a Cornershop y Django:

https://www.getonbrd.cl/empleos/programacion/desarrollador-python-cornershop-inc-218e

### RUBY

Sandy Metz Practical Object Oriented Design in Ruby. An Agile primer.

Michael Hartl The Ruby on Rails Tutorial. 

https://silicon-valley.fandom.com/wiki/Third_Party_Insourcing

### COMPARANDO ALTERNATIVAS

https://stackshare.io/stackups/hugo-vs-jekyll-vs-pelican

https://stackshare.io/stackups/jekyll-vs-wordpress

es chistoso! -> https://forestry.io/blog/is-it-time-to-move-on-from-wordpress/

Ejemplo de paquetes R a los que nadie da mantenimiento!!! *Preguntar a Gabriel Cabrera como dev de paquetes de r
que opina al respecto: 

https://github.com/rforge/introcompfinr A la fecha de este escrito, su latest commit fue el 2015

https://github.com/ibartomeus/BeeIT?fbclid=IwAR1bDKVDf75TvOOEvVagNfHnchp4AhRUo5cmtl_NFZT5n0Y4iyGSQUmJlWc

Ejemplo de la ventaja del punto #5

https://forestry.io/blog/creating-a-multilingual-blog-with-jekyll/

### Jekyll

https://github.com/jekyll/jekyll

https://jekyllrb.com/docs/

https://talk.jekyllrb.com/

Referencia extra a sylicon valley:

https://spectrum.ieee.org/view-from-the-valley/computing/software/a-madefortv-compression-algorithm
