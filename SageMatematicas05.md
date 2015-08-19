
# Análisis

Puede definirse el Análisis, de una manera un tanto tosca, como el estudio de las funciones. Las funciones más elementales son sin lugar a duda las polinómicas, construidas con una variable y las operaciones de suma, resta y multiplicación. A un nivel un poco más elevado están las funciones algebraicas, formadas por cocientes de polinomios.  En estas encontramos ya los primeros problemas con el dominio de definición.  Sin embargo pronto se descubre que con estas funciones no es suficiente. A pesar de que el abanico de posibilidades es ilimitado, la siguiente tabla muestra algunas de las funciones predefinidas en Sage.  Su uso es similar al que se realiza en Matemáticas.


| | |
| :------- | :------ |
 `sin(x)` | Seno de $x$ medido en radianes 
`cos(x)` | Coseno de $x$ medido en radianes 
`tan(x)` | Tangente de $x$ medido en radianes 
`asin(x)` | ArcoSeno de $x$ 
`acos(x)` | ArcoCoseno de $x$ 
`atan(x)` | ArcoTangente de $x$ 
`abs(x)` | Valor absoluto de $x$ 
`sinh(x)` | Seno hiperbólico de $x$ 
`cosh(x)` | Coseno hiperbólico de $x$ 
`tanh(x)` | Tangente hiperbólica de $x$ 
`exp(x)` | Exponencial de base el número $e$ 
`log(x, base)` | Logaritmo. Por defecto la base es $e$ 
`ln(x)` | Logaritmo natural (en base $e$) 


## Límites

El primer concepto necesario para desarrollar el Análisis es el de límite.  Para calcular un limite necesitamos al menos dos datos: una función y un punto en el que calcular el límite.  Para la existencia de límite los dos límites laterales deben existir y coincidir. Si queremos especificar un límite lateral necesitamos informar al programa de otro dato, lo que haremos con la opción `dir`, que puede adoptar los valores (siempre entre comillas simples) `plus` (por la derecha) o `minus` (por la izquierda).  También es muy habitual calcular límites en el infinito. La notación de Sage para el infinito es `oo`. El comando para calcular límites es


 `limit(f, x = a)`


Este comando tiene una opción, llamada `Taylor`, que nos permite resolver  más límites. Un comentario sobre el polinomio de Taylor se da en la última sección de este capítulo.

```
sage: f = x^2 + 3 
sage: limit(f, x = 7)  # Un limite sencillo
52
sage: f.subs(x = 7)   # Se podria haber hecho por sustitucion
52
sage: limit(sin(x)/x, x = 0)  # Este da lugar a una indeterminacion
1
sage: limit(ln(x), x = 0, dir = 'plus')   # Por la izquierda no existe
-Infinity
sage: limit(exp(x), x = oo), limit(exp(x), x = -oo)
(+Infinity, 0)
sage: var('t')  # Se puede utilizar cualquier variable
t
sage: limit(sin(t)/t, t = 0)
1
```



## Derivadas

Las derivadas de funciones se calculan con los comandos (ambos comando son "alias", o sea, son exactamente iguales)


 `diff(), derivative()`


Si la función a derivar es de una sola variable se pueden usar con un único argumento.El segundo argumento, que por defecto es 1, indica el orden de derivación (derivada segunda, tercera, etc).


```
sage: f(x) = x^4 + 3*x^3 + 2*x + sin(x); f
x |--> sin(x) + x^4 + 3*x^3 + 2*x
sage: f.diff()
x |--> cos(x) + 4*x^3 + 9*x^2 + 2
sage: f.diff(3)  # Derivada tercera
x |--> -cos(x) + 24*x + 18
sage: f.derivative()  # El otro comando
x |--> cos(x) + 4*x^3 + 9*x^2 + 2
```


Si la función es de varias variables, tenemos la derivada parcial. Para ello es necesario indicar, como segundo argumento, la variable sobre la que se deriva. Por lo demás el funcionamiento es similar al caso de una variable.



```
sage: var('x,y')
(x, y)
sage: f(x,y) = x^4*y^5
sage: f.diff(x)  # Derivada parcial respecto a x
(x, y) |--> 4*x^3*y^5
sage: f.diff(x,3)  # Derivada parcial tercera respecto a x
(x, y) |--> 24*x*y^5
sage: f.diff(x,y)  # Derivada parcial respecto a x y a y
(x, y) |--> 20*x^3*y^4
sage: f.diff(x,2,y,3)  # Segunda respecto a x y tercera respecto a y
(x, y) |--> 720*x^2*y^2
```



## Integrales

Para realizar integrales empleamos los comandos (alias)


 `integral(), integrate()`



Dicha función tiene cuatro argumentos, siendo los tres ultimos opcionales.  El primero es la funcion a integrar.  Si la funcion tiene una sola variable con esto llega para realizar integrales indefinidas.  El segundo argumento es la letra sobre la que se realiza la integración. Si la función tiene más de una variable es imprescindible informar de ello al programa. La integración en cualquiera de los dos casos es indefinida.  Los dos últimos argumentos son los límites inferior y superior, por lo que si los añadimos estaremos realizando una integración definida.



```
sage: f(x) = x^2
sage: f.integral()
x |--> x^3/3
sage: f.integral(x,0,10)  # Esta ya es una integral definida
1000/3
sage: var('a')
a
sage: f(a,x) = a * sin(x)  # Una funcion con dos variables
sage: f.integral(x)
(a, x) |--> -a*cos(x)
sage: f.integral(a)
(a, x) |--> a^2*sin(x)/2
```


## Polinomio de Taylor

Consideremos una función $f(x)$ y un punto $x = a$ en el dominio de $f$.  Sea $n$ un número natural.  Consideramos ahora el conjunto de todos los polinomios reales de grado menor que $n$.  Entre todos ellos, existe uno que, en cierto sentido, es el "más" próximo a $f(x)$ en un entorno del punto $x = a$.  Para calcular dicho polinomio utilizamos la orden


 `taylor(f, x, a, n)`



```
sage: f = exp(x) * sin(x) 
sage: t3 = taylor(f, x, 1, 3); t3
sin(1)*e + (sin(1) + cos(1))*e*(x - 1) + cos(1)*e*(x - 1)^2 -
(sin(1) - cos(1))*e*(x - 1)^3/3
sage: expand(t3) 
-1*e*sin(1)*x^3/3 + e*cos(1)*x^3/3 + e*sin(1)*x^2 + e*sin(1)/3 - e*cos(1)/3
sage: # Observese que es un polinomio de grado 3. Los senos, cosenos
sage: # y exponenciales que aparecen son numeros
sage: t7 = taylor(f, x, 1, 7)
sage: plot(f, -2, 4) + plot(t3, -2, 4, color = 'red')

sage: plot(f, -2, 4) + plot(t7, -2, 4, color = 'green')
```

![](/imagenes/taylor1.png)

![](/imagenes/taylor2.png)
