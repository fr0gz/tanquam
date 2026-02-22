---
title: "Usando Eigen Para Operar Matrices Con CPP Con Simulación De Datos"
date: 2020-06-06
translationKey: "using-eigen-matrix-cpp"
draft: false
math: true
---


## Resumen

Eigen es una librería de algebra lineal de C++ que hace el "heavy lifting" de las operaciones matriciales que ejecuta la popular librería TensorFlow. Mi motivación para escribir este post es ir conociendo en mayor profundidad la tecnología subyacente a dicha libreria de redes neuronales, con el fin de aprender como hacer las mías propias. Esto se torna más relevante porque con el advenimiento de TinyML y TensorFlow Lite (esto es: correr algoritmos de redes neuronales en dispositivos pequeños sin necesidad de conexion a internet para procesamiento en la nube), me nace la curiosidad de saber como funciona por la aplicacion que puede tener en un futuro cercano.

## Dependencias

En primer lugar, hay que cargar las dependencias. En este caso en particular, lo que yo quiero hacer es traerme herramientas de la libreria Eigen para construir los vectores asi como la operacion matricial que quiero hacer, que es una regresión lineal. También quiero meter dentro de mis vectores un montón de datos aleatorios. Entonces, lo que necesito es traerme un generador de números aleatorios así como las distribuciones de probabilidad de las que yo quiero "draftear" esos números (C++ soporta una infinidad de generadores de números aleatorios y distribuciones). Para esto, voy a necesitar herramientas contenidas en random.

Además, quiero hacer una prueba de performance de las operaciones para posteriormente compararla con Numpy: para eso voy a necesitar construirme un objeto reloj  

{{< highlight cpp "linenos=inline, hl_lines=1, style=dracula" >}}
/******************************************************************************
  Prácticas de matrices con Eigen: Haciendo una Regresion Lineal


  Copyright 2020 - Cristián Andrés Guevara Kalil <cguevarak@pm.me>
  University of Chile
  Redistribution and use in source and binary forms, with or without
  modification, are permitted provided that the following conditions
  are met:
  1. Redistributions of source code must retain the above copyright
  notice, this list of conditions and the following disclaimer.
  2. Redistributions in binary form must reproduce the above copyright
  notice, this list of conditions and the following disclaimer in the
  documentation and/or other materials provided with the distribution.
  3. Neither the name of the copyright holder nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.
  Complete BSD-3-clause License: https://opensource.org/licenses/BSD-3-Clause
******************************************************************************/

#include <iostream>
#include <time.h>
#include <random>
#include <Eigen/Dense>

{{< /highlight >}}

## Haciendo la simulación: Genero mis datos.

El primer paso es generar datos aleatorios. Para generar datos, voy a necesitar un generador de numeros pseudo aleatorios, que en este caso será un Mersenne Twister, y fijaré la semilla 24 porque es 42 al revés (los lectores de Douglas Adams saben que significa eso: so long and thanks for all the fish). Esto hace que sea un experimento replicable. 

Luego, construyo las distribuciones normales que voy a usar: una para x y otra para y. La distribucion standard a generar tiene que especificar su media y su desviacion standard.

{{< highlight cpp "linenos=inline, hl_lines=1, style=dracula" >}}
 	std::mt19937_64 mt_engine{ 24 };
 	std::normal_distribution<double> disx(10, 1);
	std::normal_distribution<double> disy(100, 15);
{{< /highlight >}}

## Construyendo las matrices

Ahora, vamos a utilizar la libreria Eigen. Lo que yo quiero hacer conceptualmente es crear una estructura que me almacene los datos que van a representar mi vector "x" y mi vector "y". Para hacer esta operación, el código contiene sintaxis de c++ moderno, por lo tanto hay que hacer la configuración correspondiente para que pueda compilar bien.

La sintáxis de Eigen es bien semántica: haz una matriz que almacene el tipo de dato double; haz una matriz que almacene el tipo de dato float, haz una matriz de ceros de 5000x2 que me vaya guardando los datos que va generando mi motor y que están distribuidos segun cierta distribución con parametros que ya generé.  Sin embargo una parte candidata a hacer una posterior reflexión más profunda es respecto de la aplicación de funciones a mi matriz, que es la forma especifica en que implemento el concepto de tomar los numeros de mi distribución y almacenarlos.

{{< highlight cpp "linenos=inline, hl_lines=1, style=dracula" >}}
    Eigen::MatrixXd x = Eigen::MatrixXf::Zero(5000,2).unaryExpr([&](double dummy){return disx(mt_engine);});
    Eigen::MatrixXd y = Eigen::MatrixXf::Zero(5000,1).unaryExpr([&](double dummy){return disy(mt_engine);});
    Eigen::MatrixXd xtx = x.transpose() * x;
    Eigen::MatrixXd xty = x.transpose() * y;
{{< /highlight >}}

Después de la generación de datos, la idea que intento plasmar es "asignale a la variable xtx el valor de x traspuesto por x".
De forma similar, la variable xty lo que hace es almacenar el valor de "x traspuesto por y". 

Para la gente de la FEN, esto no conlleva ningún misterio: son los componentes de la fórmula matricial para obtener los betas de una regresión. [X'X]^-1 * X'Y

\begin{aligned}
\hat{\beta} = (X' X)^{-1} (X' Y)
\end{aligned}

## Sacando la inversa y los Betas

Ahora, para sacar la inversa Eigen nos da una función bastante semantica

{{< highlight cpp "linenos=inline, hl_lines=1, style=dracula" >}}
   std::cout << "Inversa de X'*X" << std::endl;
   std::cout << xtx.inverse() << std::endl;
   std::cout << "Betas: " << std::endl;
   std::cout << xtx.inverse() * xty << std::endl;
{{< /highlight >}}

## Haciendo el Benchmarking

Para hacer el benchmarking, lo que hice fue envolver mi programita desde el principio con un reloj de la siguiente forma:

{{< highlight cpp "linenos=inline, hl_lines=1, style=dracula" >}}
  clock_t begin = clock();

  double elapsed_secs = double(end - begin) / CLOCKS_PER_SEC;
  std::cout << "Time taken : " << elapsed_secs << std::endl;
{{< /highlight >}}

 ## Finalmente: 
 
 Las aplicaciones de la libreria Eigen son bastante interesantes, y debo adelantar que según mi comparación con operaciones similares en numpy, en mi caso al menos fue bastante rápido (dio aprox 2.43 vs 12 segundos). Como agradecimiento quiero darle las gracias a la comunidad del software libre, la cual hizo posible una implementación exitosa de esta aplicación. 
 
