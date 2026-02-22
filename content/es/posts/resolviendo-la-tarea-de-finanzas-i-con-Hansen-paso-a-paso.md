---
title: "Probando el Rendimiento de Portafolios de Varianza Mínima y Tangencia en R"
date: 2019-03-15
translationKey: "min-var-tangency-r"
categories: ["jekyll", "actualización", "archivo"]
draft: true
author: "Cristian Guevara"
---

## Resumen

Leyendo este post, aprenderas como resolver paso a paso la tarea #1 de Finanzas I del semestre primavera 2019. Eso incluye entender lo que te preguntan, cargar los datos, visualizar los datos o presentar valores o matrices según la naturaleza de la pregunta. 

Testing Performance of Minimum Variance and Tangency Portfolios Doing out-of-sample-valuation with R

## Cargar y preprocesar los datos

### Dependencias

Para hacer lo que nos piden, primero es necesario cargar los datos. El dataset que nos dieron esta en archivo excel, entonces lo que yo necesito es pasar de excel a un dataframe en R.

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
library(readxl) #para leer excel
library(psych) #para usar describe
library(timeSeries) #Desde acá, las cargué empezando la p2. Queda la documentacion por si hay problemas.
library(fPortfolio)
library(quantmod)
library(caTools)
library(dplyr)
library(PerformanceAnalytics)
library(ggplot2)

{{< /highlight >}}

### Preparación de los datos

Primero Obtenemos los datos del archivo datos limpios

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
datos_limpios_10_18 <- read_excel("~/R Stuff/Tarea1_Hansen/datos_limpios_10_18.xlsx")
{{< /highlight >}}

También necesitamos el archivo de los retornos acumulados para gráficar.

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
datos_acumulado <- read_excel("~/R Stuff/Tarea1_Hansen/limpio_acumulado.xlsx")
{{< /highlight >}}

Obtenemos la estadistica descriptiva que se nos solicita

## Pregunta 1 A
Acá lo que hago es armarme las columnas por índice.
Entonces almaceno la informacion de cada indice en un vector, para cada indice

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
cum_consump <- datos_acumulado[3]
cum_manuf <- datos_acumulado[5]
cum_tech <- datos_acumulado[7]
cum_health <- datos_acumulado[9]
cum_fecha <- datos_acumulado[1]
cum_others <- datos_acumulado[11]
{{< /highlight >}}

Ahora, armo la matriz que se almacena en la variable M_acum.
Luego, lo exporto a formato csv. Este archivo lo encontramos en el directorio en el que estamos trabajando (recordar working directory, ver post #1)

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
M_acum <- cbind(cum_fecha,cum_consump,cum_manuf,cum_tech,cum_health,cum_others)
write.csv(M_acum)
{{< /highlight >}}

Ahora, para ejecutar la estadistica descriptiva que nos piden, usaremos la funcion ´describe´ que esta en la libreria ´psych´ , la elegí porque lo encontré fácil de usar. Además, es una función que también existe en la libreria de python ´pandas´ .

En variable p1_b almaceno la respuesta a lo que me piden
En la p1_b_auxiliar lo que hago es generar una variable sólo para sacarla media de la risk free rate en el período 2010-2018: 

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
p1_b <- describe(datos_limpios_10_18[2:6])
p1_b_auxiliar <- describe(datos_limpios_10_18)
{{< /highlight >}}

Obtenemos el ratio de sharpe, manualmente, con los datos de p1_b:

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
vector_medias <- c(1.0716667,0.7275000,1.1608333,1.1375926,0.9993519)
vector_rf <- c(0.0277777,0.0277777,0.0277777,0.0277777,0.0277777)
vector_sd <- c(3.391754,4.017619,4.130456,3.883420,4.423253)
vector_sharpe_indices <- ((vector_medias-vector_rf)/vector_sd)
{{< /highlight >}}

Donde corresponden correlativamente al heading de datos_limpios_10_18[2:6]
removemos los objetos auxiliares vector_rf y p1_b_auxiliar. Con eso tenemos la P1.

# P2 a : construir frontera eficiente periodo 10-18 para el objeto datos_limpios_10_18[2:6]

Hacemos una variable que almacene los retornos que nos interesan con type TimeSeries usando la funcion as.timeSeries()

Luego, hacemos una variable FronteraEficiente, que utiliza la funcion porfolioFrontier() de la libreria . Como argumento, le pongo la variable que acabo de crear, y como parametro constraint especifico LongOnly o sea, no venta corta.

Finalmente, hago el gráfico con plot(). Dentro de la función plot pongo la variable FronteraEficiente:

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
retornos_10_18_ts <- as.timeSeries(datos_limpios_10_18[2:6])

FronteraEficiente <- portfolioFrontier(retornos_10_18_ts, constraints = "LongOnly")

plot(FronteraEficiente)

tickers <- c("Consumption", "Manufacturing", "Tech", "Health", "Others")
PesosFrontera <- getWeights(FronteraEficiente)
colnames(PesosFrontera) <- tickers
risk_return <- frontierPoints(FronteraEficiente)
write.csv(risk_return, "risk_return.csv")

{{< /highlight >}}

Aca, me da un menú de opciones para el gráfico. Pongo las que satisfacen la condicion de la pregunta y lo exporto en pdf.

# P3 y P4

Ahora, para hacer portfolios de Minima Varianza utilizo la funcion minvariancePortfolio(), que tiene como argumento la variable de type timeSeries retornos_10_18_ts. Como parametro specs le pongo spec=porfolioSpec y en contraints, le pongo LongOnly

De forma analoga, utilizo la función tangencyPortfolio, que utiliza el mismo argumento y parametros.

Ahora yo quiero almacenar los pesos de cada activo de los portfolios. Para eso usaré la función getWeights(), que usará como argumento un portfolio ( en este caso, mvp o tangencia). Entonces, con getWeights() obtengo el atributo que representa los pesos o ponderaciones de ese portfolio.
 
{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
mvp <- minvariancePortfolio(retornos_10_18_ts, spec=portfolioSpec(), constraints="LongOnly")

tangencia <- tangencyPortfolio(retornos_10_18_ts, spec=portfolioSpec(), constraints="LongOnly")

mvp_pesos <- getWeights(mvp)
tangencia_pesos <- getWeights(tangencia)
{{< /highlight >}}

Ahora que tengo los pesos del portfolio, puedo graficar. 
Entonces, yo lo que quiero hacer es un grafico de barras que represente el % de inversión en cada activo. 

Para eso, utilizaremos las funciones ggplot(), geom_bar(), geom_text() y otras. La idea, es que ggplot() te va armando el gráfico por capas. Entonces, cada capa es una parte del gráfico que tu puedes controlar: 

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
mvp_df <- data.frame(mvp_pesos)
indices <- colnames(PesosFrontera)

  ggplot(data=mvp_df, aes(x=indices, y=mvp_pesos, fill=indices)) +
  geom_bar(stat="identity", position=position_dodge(),colour="black") +
  geom_text(aes(label=sprintf("%.02f %%",mvp_pesos*100)),
            position=position_dodge(width=0.9), vjust=-0.25, check_overlap = TRUE) +
  ggtitle("Ponderaciones Portfolio Minima Varianza (Sin Venta Corta)")+ theme(plot.title = element_text(hjust = 0.5)) +
  labs(x= "Indices", y = "Ponderaciones (%)")

  tangencia_df <- data.frame(tangencia_pesos)

  ggplot(data=tangencia_df, aes(x=indices, y=tangencia_pesos, fill=indices)) +
  geom_bar(stat="identity", position=position_dodge(),colour="black") +
  geom_text(aes(label=sprintf("%.02f %%",tangencia_pesos*100)),
            position=position_dodge(width=0.9), vjust=-0.25, check_overlap = TRUE) +
  ggtitle("Ponderaciones Portfolio de Tangencia")+ theme(plot.title = element_text(hjust = 0.5)) +
  labs(x= "Indices", y = "Ponderaciones (%)")

{{< /highlight >}}

# P5

Finalmente, me piden hacer una evaluacion fuera de muestra de los portfolios que encontré en las preguntas 3 y 4.

En el excel que dieron para la tarea, nos dieron datos que iban desde el 2010 hasta el 2019. En las preguntas anteriores, tan sólo habiamos utilizado los datos correspondientes al período 2010 - 2018, entonces lo que yo tengo que hacer es calcular como le va a portfolios construidos con los pesos que encontré, metiendole los datos correspondientes al periodo 2019.

Con eso, tengo que comparar. 

Resultado: El portfolio mvp tiene un poquitito más de retorno esperado

El código de abajo, además enseña a sacar la matriz varianza covarianza, en base a datos_limpios19
para hacer más leible el ejercicio, c(a,b,c,d,e) quiere decir "hacer columna con a,b,c,d,e".

M <- cbind quiere decir juntame todas esas columnas en el objeto M

Entonces, así se hace: 

{{< highlight r "linenos=inline, hl_lines=1, style=dracula" >}}
p5_aux <- describe(datos_limpios_19[2:6])
auxiliar <- describe(datos_limpios_19)

w <- c(0.7307,0.0514,0,0.2178,0)

w2 <- c(0.5924,0,0.0916,0.3160,0)

retorno_mvp19 <- t(w)%*%medias_19
retorno_tan19 <- t(w2)%*%medias_19

consumption <- c(8.11,1.23,2.84,4.12,-6.33,6.88)
manufacturing <- c(9.04,4.18,0.7,2.44,-8.33,7.34)
tech <- c(8.97,5.23,3.3,6.07,-7.82,7.44)
health <- c(5.04, 3.04,0.6,-3.06,-3.45,7.26)
others <- c(9.15,2.8,-1.27,6.61,-6.22,6.7)
M <- cbind(consumption,manufacturing,tech,health,others)

medias_19 <- c(2.81,2.56,3.87,1.57,2.96)
medias_rf_19 <- c(0.20,0.20,0.20,0.20,0.20)
sd_19 <- c(5.15,6.16,6.04,4.34,5.79)
varianza_19 <- c(sqrt(5.15),sqrt(6.16),sqrt(6.04),sqrt(4.34),sqrt(5.79))

sharpe_indices19 <- ((medias_19-medias_rf_19)/sd_19)

#Comparamos
#########################################################################################
 tickers [1] "Consumption"   "Manufacturing" "Tech"          "Health"        "Others" 
#
 10-18   [1] 0.3077726       0.1741634       0.2743173       0.2857829       0.2196515
#
    19   [1] 0.5067961       0.3831169       0.6076159       0.3156682       0.4766839
#########################################################################################
{{< /highlight >}}

Con eso finalizamos la pregunta #5
