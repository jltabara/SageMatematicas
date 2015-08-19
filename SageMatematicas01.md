# Aritmética Básica


Suele denominarse Aritmética  al estudio de las operaciones con números enteros. Nosotros entenderemos por Aritmética también el estudio de las operaciones con otros tipos de números, como pueden ser los racionales, los reales y los complejos.


## Operaciones con enteros


Las operaciones con enteros se realizan en Sage con los operadores habituales y son siempre exactas, no empleando nunca aproximaciones. El uso de paréntesis es similar al que se realiza en matemáticas, así como la jerarquía de operaciones.
Los espacios en blanco entre los operadores y los números son innecesarios. Solamente se usan para facilitar la lectura. 

```
sage: # Todo lo que aparece tras el signo # es un comentario
sage: 45 + 89
134
sage: 48 - 963
-915
sage: 125 * 56
7000
sage: 4 + 7 * 12 # Jerarquia habitual de operaciones
88
sage: (4 + 7) * 12 # Parentesis y cambio de la jerarquia
132
sage: 125/5 # Una division exacta. Resultado entero
25
sage: 45/23  # Si la division no es exacta tenemos un numero racional
45/23
sage: 190/24  # Si la fraccion es reducible, Sage la reduce
95/12
```

Para calcular potencias se emplea el circunflejo (o el doble asterisco) y Sage utiliza precisión infinita. Los exponentes negativos dan lugar a fracciones.

```
sage: # Podemos escribir dos o mas operaciones separadas por punto y coma
sage: 2^34; 2**34
17179869184
17179869184
sage: # Tambien se pueden separar por comas, y el resultado aparece 
sage: # en forma de lista, esto es, entre parentesis
sage: 2^34, 2**34
(17179869184, 17179869184)
sage: 2**400 # Precision infinita
2582249878086908589655919172003011874329705792829223512830
6593565406476220168411946296453532801378314359031719727474
93376
sage: 2^(-4)  # Las potencias de exponente negativo producen fracciones
1/16
sage: a = 3  # Las variables se inicializan con el signo =
sage: 2*a, a^2, 3*a - 2
(6, 9, 7)
```

##Operaciones con números racionales y reales


Las fracciones se escriben en la forma habitual, con la barra de división. Si en una operación aparecen fracciones y números enteros, el resultado será siempre una fracción o un número entero.

```
sage: 4/7 + 8/3 
68/21
sage: 3/5 - 6/11
3/55
sage: (3/4 - 4/5)^3 + (2/3) / (34/89)
711949/408000
```

Los números reales se escriben con el punto decimal. Si queremos escribirlos en notación científica, el exponente de la potencia de 10 se escribe tras la letra `e`. Si en una operación aparece un número decimal, el resultado proporcionado por Sage es también un número decimal. **Al trabajar con decimales Sage emplea aritmética no exacta y puede que algunos cálculos no sean correctos**.

```
sage: 1.563 + 3.89
5.45300000000000
sage: 1.456e25 / 2.456e-12
5.92833876221498e36
sage: (1.123782)^100
117005.737177117
sage: 4/5 + 0.87
1.67000000000000
sage: 91./23 # Basta un punto y da el resultado en decimales
3.95652173913043
```

La raíz cuadrada se calcula con la función `sqrt()`. Para calcular la raíz $n$-ésima elevamos a  $1/n$. Sage simplifica radicales, extrayendo factores siempre que es posible, cuando los números son enteros o racionales. Si el número al que se le calcula el radical es un número con decimales, nos da la respuesta con decimales.

```
sage: sqrt(4), sqrt(8), sqrt(32) # Extraccion de factores
(2, 2*sqrt(2), 4*sqrt(2))
sage: sqrt(32/27) # Extraccion de factores en fracciones
4/3*sqrt(2/3)
sage: 1024^(1/4) # La raiz cuarta extrayendo factores
4*4^(1/4)
sage: 1024^(1/7) # La raiz septima
2*8^(1/7)
sage: 1024.0^(1/7) # Ahora el numero es decimal
2.69180038526471
sage: sqrt(32.)
5.65685424949238
```

En Matemáticas existen distintos conjuntos numéricos. Tenemos el conjunto de los números enteros, denotado por $\mathbb{Z}$, el conjunto de los números racionales $\mathbb{Q}$, etc. En Sage hay algo similar, aunque no totalmente igual: en $\mathbb{Z}$ y en $\mathbb{Q}$ el programa utiliza aritmética exacta, sin embargo en $\mathbb{R}$ y en $\mathbb{C}$  utiliza aritmética aproximada, por lo que pueden producirse errores. El conjunto de los enteros se denota en el programa por `ZZ`. Los racionales, los reales y los complejos se denotan `QQ`, `RR` y `CC`. Para saber en que conjunto considera Sage que está cada número utilizamos el método

`.base_ring()`

aunque también se puede utilizar la función

`type()`

La diferencia entre método y función es su forma de utilización.  Para usar una función escribimos el nombre de la función y entre paréntesis el objeto al que se le aplica la función. Sin embargo para utilizar un método, primero escribimos el objeto sobre el que va a actuar, después escribimos un punto y finalmente el método. 

Para saber si un número pertenece a un conjunto numérico, empleamos el operador `in`. Si la respuesta es `True` el número pertenece al conjunto. Si retorna `False` no pertenece al conjunto.

```
sage: a = 2
sage: a.base_ring()
Integer Ring
sage: type(a)
<type 'sage.rings.integer.Integer'>
sage: # El comando ``in'' se utiliza como el simbolo pertenece
sage: a in ZZ, a in QQ, a in RR, a in CC
(True, True, True, True)
sage: a = 3/7
sage: a.base_ring()
Rational Field
sage: type(a)
<type 'sage.rings.rational.Rational'>
sage: a in ZZ, a in QQ, a in RR, a in CC
(False, True, True, True)
sage: a = 1.53876
sage: a.base_ring()
Real Field with 53 bits of precision
sage: type(a)
<type 'sage.rings.real_mpfr.RealLiteral'>
sage: a in ZZ, a in QQ, a in RR, a in CC
(False, True, True, True)
sage: # El numero decimal tambien es un numero racional
sage: a = sqrt(2) # Un numero irracional
sage: a in ZZ, a in QQ, a in RR, a in CC
(False, False, True, True)
```

Para obtener aproximaciones decimales utilizamos la función (y a la vez método)

`n()`

Esta función admite como opción `digits` para obtener las aproximaciones con distinto número de cifras.

```
sage: n(sqrt(2))   # Precision por defecto
1.41421356237310
sage: n(sqrt(2), digits=50)  # Mayor precision
1.4142135623730950488016887242096980785696718753769
sage: n(sqrt(2), digits=5)
1.4142
sage: sqrt(2).n()  # Actuando como metodo
1.41421356237310
sage: sqrt(2).n(digits=30)  # El modificador digits como metodo
1.41421356237309504880168872421
```


Para extraer el denominador y el numerador de un número racional (simplificados)

`.denominator()`, `.numerator()`

```
sage: a = 770/230
sage: denominator(a)
23
sage: a.denominator(). 
23
sage: a.numerator()
77
sage: a = -74/21
sage: a.numerator(), a.denominator()
(-74, 21)
```

Los números reales se pueden aproximar a los enteros de distintos modos. La forma más habitual es el redondeo, que devuelve el número entero más próximo al número decimal en cuestion. Pero también podemos redondear por defecto y por exceso. Los comandos para realizar todos estos tipos de redondeo son

`.round()`, `.floor()`, `.ceil()`


Estas 5 instrucciones se pueden utilizar como funciones o como métodos. La función `.round()` admite una opción, que nos sirve para especificar  con cuantos decimales queremos el redondeo. Incluso admite números negativos, si queremos redondear a las decenas, centenas, ... Sin embargo esto solamente funciona si utilizamos la función y no sirve con el método.

```
sage: a = 3.67
sage: a.round(), a.floor(), a.ceil()
(4, 3, 4)
sage: a = 3.234
sage: a.round(), a.floor(), a.ceil()
(3, 3, 4)
sage: a = -3.898
sage: a.round(), a.floor(), a.ceil()
(-4, -4, -3)
sage: a = 3456.876543
sage: round(a), round(a,2), round(a,5)
(3457, 3456.88, 3456.87654)
sage: round(a,-1), round(a,-2), round(a,-4)
(3460.0, 3500.0, 0.0)
```


## La ayuda en Sage

La notación de función es más intuitiva y más próxima al hacer matemático. Sin embargo los métodos tienen una ventaja adicional:  como primero debemos escribir el objeto, Sage ya "sabe" qué métodos se pueden aplicar a dicho objeto. Si escribimos un objeto y el punto y pulsamos el tabulador, el programa nos muestra un menú con los métodos que se pueden aplicar a dicho objeto. Si hemos empezado a escribir el método y pulsamos el tabulador, el programa completa la escritura, o en caso de que haya varios métodos que comiencen por dichas letras nos da las alternativas. Por esta razón nosotros potenciaremos la utilización de métodos.

Para obtener ayuda sobre un método o función, en vez de escribir los paréntesis, escribimos el signo de interrogación `?` y pulsamos Enter. Si la ayuda ocupa más de una pantalla la barra  espaciadora nos permite avanzar página.  Para salir de la ayuda debemos pulsar la tecla `q` (del inglés *quit*).  Puede ocurrir que gran parte de la ayuda sea incomprensible para nosotros, pero hay una parte de texto (eso si, en inglés) y ejemplos de uso que nos pueden ser de utilidad.
Si en vez de escribir el signo de interrogación escribimos dos signos de interrogación, el programa nos informa del código fuente de la función. En principio esto sólo tiene utilidad para una utilización más avanzada de Sage que la que proponemos en estas notas.

En el siguiente código hemos borrado la salida por ser demasiado larga. Si escribimos `[Tab]` significa que debemos pulsar el tabulador y si escribimos `[Enter]` debemos pulsar dicha tecla.

```
sage: # Veamos que metodos se pueden aplicar a un numero racional
sage: a = 7/3
sage: a.[Tab]
	  # salida eliminada       
sage: # Ahora los metodos que empiezan por d
sage: a.d[Tab]
	  # salida eliminada   
sage: # Pedimos ayuda sobre metodo denominator
sage: a.denominator?[Enter]
	  # salida eliminada 
sage: # Ahora las funciones que empiezan por j
sage: j[Tab]
	  # salida eliminada 
```



## Comparación de números

Los operadores de orden son

`<`,  `>`,  `<=`,  `>=`

Cuando comparamos dos números con uno de estos operadores, la respuesta es `True`, si la desigualdad es verdadera y `False` en caso contrario.

Además de los operadores de orden, tenemos los operadores

`==`, `!=`


El primero devuelve `True` cuando los dos números que comparamos son iguales. En cambio el segundo operador devuelve `True` cuando ambos son distintos.

```
sage: 5 < 6
True
sage: -5 > 6
False
sage: 78 < 78, 78 <= 78
(False, True)
sage: # El signo de comparacion es == y no =
sage: 45 == 9 * 5
True
sage: 78 != 78
False
sage: # En vez del simbolo != tambien se puede emplear <>
sage: 78 <> 78, 78 <> 123
(False, True)
```

## División euclídea

En el conjunto de los números enteros existe el concepto de *división euclídea*.  Dados dos números enteros arbitrarios $a$, $b$ podemos encontrar dos números, únicos, $c$ y $r$ que verifican:

- $a=b\cdot c+r$.

- $r$ es un entero positivo o nulo, menor que $|b|$.


Para obtener el cociente de la división de $n$ entre $m$ se emplean el comando

`//`

Para obtener el resto se pueden emplear dos formas alternativas

`.mod()`, `%`

```
sage: 23 // 5 # Cociente
4
sage: 23 % 5 # Resto
3
sage: # Comprobemos
sage: 4 * 5 + 3
23
sage: # Tambien funciona con numeros negativos
sage: -26 // 5
-6
sage: -26 % 5
4
sage: -6 * 5 + 4
-26
sage: # Lo mismo lo podemos realizar con el metodo mod
sage: (-26).mod(5)
4
sage: mod(-26, 5) # Como funcion de dos argumentos
4
sage: # El quinto numero de Fermat es
sage: 2^(2^5) + 1
4294967297
sage: # Fermat pensaba que era primo. Sin embargo Euler comprobo que
sage: 4294967297 % 641
0
```


## Primos y factorización

Un número natural es *primo* si sus únicos divisores (positivos) son la unidad y él mismo.  Los números que no son primos se denominan *compuestos*. Para ellos rige el *Teorema Fundamental de la Aritmética*, que afirma que todo número compuesto admite una factorización (única salvo el orden) en producto de primos.

La comprobación de que un número es primo es un problema computacionalmente costoso.  Por ello muchas veces (sobre todo para enteros "muy"  grandes) se hace un análisis probabilístico para verificar si un número es primo.  Aún peor, desde el punto de vista computacional, es el problema de la factorización. Por ello solo debemos esperar de  Sage resultados para enteros "razonables".

Para saber si un número es primo debemos emplear el método

`.is_prime()`

Sage también puede informarnos si un número es una potencia de un número primo

`.is_prime_power()`

Dado un número entero $n$, el primer número mayor que $n$ y primo

`.next_prime()`

Análogamente el primo inmediatamente anterior a $n$

`.previous_prime()`

Los siguientes métodos se expresan por si mismos

`.next_prime_power()`, `previous_prime_power()`

```
sage: a = 89; a.is_prime()
True
sage: 81.is_prime_power()  # Sabemos que 81 = 3^4
True
sage: 100.is_prime_power()  # Sin embargo 100 = 2^2*5^2
False
sage: 9.next_prime()
11
sage: previous_prime(16)
13
sage: next_prime_power(14)
16
sage: previous_prime_power(90)
89
sage: # Efectivamente, puesto que 89 = 89^1
```

Cuando el número es compuesto podemos estar interesados en obtener su descomposición en factores primos

`.factor()`

Muy relacionado con el problema de la factorización, se encuentra el problema de hallar todos los divisores de un número dado. Multiplicando cualesquiera factores de la descomposición de un número obtenemos un divisor de dicho número. Es más, cualquier divisor se puede obtener de ese modo. Para obtener una lista con todos los divisores de un número

`.divisors()`

```
sage: 360.factor()
2^3 * 3^2 * 5
sage: # Comprobemos que es correcto
sage: 2^3 * 3^2 * 5
360
sage: 180.divisors() 
[1, 2, 3, 4, 5, 6, 9, 10, 12, 15, 18, 20, 30, 36, 45, 60, 90, 180]
```


## Máximo común divisor y Mínimo común múltiplo

Dado un entero $n$ consideramos el conjunto de sus divisores positivos. Ciertamente este es un conjunto finito contenido en $\mathbb{N}$. Dado otro número $m$ consideramos nuevamente el conjunto de sus divisores. Hacemos la intersección de ambos conjuntos, que es necesariamente no vacía puesto que la unidad divide a todo número.  El mayor de los números de dicha intersección es, por definición, el máximo común divisor de ambos números.  Esta es la definición clásica de este concepto. 

Ahora consideramos los múltiplos positivos de $n$ y los de $m$. Hacemos la intersección de dichos conjuntos. Dicha intersección es no vacía, puesto que $nm$ está en ambos conjuntos. De todos los elementos de dicha intersección debe existir uno que sea el menor de todos (necesariamente es menor o igual que $nm$). Dicho número es el mínimo común múltiplo.


Para calcular el máximo común divisor de dos enteros (*greatest common divisor* en inglés)

`.gcd()`

El mínimo común múltiplo (*least common multiple* en inglés)

`.lcm()`


```
sage: (78).gcd(24)
6
sage: (78).lcm(24)
312
sage: # El producto del minimo por el maximo da el producto de los numeros
sage: 6 * 312, 78 * 24
(1872, 1872)
```

##Números complejos

Los números complejos son expresiones de la forma $a+bi$ donde $a$ y $b$ son números reales e $i$ es una raíz cuadrada de $-1$ (o lo que es lo mismo, $i^2 = -1$).  Por defecto Sage reconoce a la letra  `i` como un objeto simbólico con la propiedad $i^2 = -1$. Muchos cálculos los puede realizar directamente, pero en algunos casos es necesario comenzar el cálculo con números complejos con la instrucción `i = CC(i)` (no es necesario conocer ahora el significado de esta orden)


```
sage: type(i)
<type 'sage.symbolic.expression.Expression'>
sage: type(3 + 4*i)
<type 'sage.symbolic.expression.Expression'>
sage: i.base_ring()
Symbolic Ring
sage: i^2
-1
```

Las operaciones aritméticas se realizan nuevamente con los operadores habituales.  En las respuestas de Sage la unidad imaginaria se escribe en mayúsculas. Las operaciones con números complejos se realizan en aritmética aproximada, por lo que son susceptibles a errores. Además el número de decimales que da el programa puede hacer difícil la lectura del resultado.

```
sage: a = 5 + 8*i
sage: b = 3.4 + 9*i
sage: a + b
8.40000000000000 + 17.0000000000000*I
sage: 3*a + 8.9*b
45.2600000000000 + 104.100000000000*I
sage: a - b
1.60000000000000 - 1.00000000000000*I
sage: a * b
-55.0000000000000 + 72.2000000000000*I
sage: a / b
0.961538461538461 - 0.192307692307692*I
sage: a^5
25525.0000000000 - 70232.0000000000*I
```

Dado el número complejo $z=a+bi$ su parte real es $a$ y su parte imaginaria es $b$. Para obtenerlas utilizamos los métodos

`.real()`, `.imag()`

El módulo o valor absoluto es $\sqrt{a^2 + b^2}$

`.abs()`

El conjugado es $a-bi$

`.conjugate()`

La norma de un número complejo se obtiene multiplicando dicho número por su complejo conjugado.  Es siempre un número real, que resulta coincidir con $a^2+b^2$

`.norm()`

Si imaginamos el número complejo $z= a+bi$ como el vector $(a,b)$, dicho vector forma con la parte positiva del eje de las $x$ un cierto ángulo.  Es el denominado argumento del número complejo (Sage lo mide en radianes)

`.arg()`, `.argument()`

```
sage: a = 5 + 8*i
sage: a.real()
5
sage: a.imag()
8
sage: a.abs()
sqrt(89)
sage: a.norm()
89
sage: a.conjugate()
-8*I + 5
sage: # El metodo  arg() no funciona sin hacer i = CC(i)
sage: i = CC(i)
sage: a = 5 + 8*i
sage: a.arg()
1.01219701145133
sage: a.abs() # Ahora da las soluciones con decimales
9.43398113205660
sage: sqrt(89).n()
9.43398113205660
```

Como consecuencia del *Teorema Fundamental del Álgebra* todo número complejo tiene exactamente $n$ raíces $n$-ésimas (pues el polinomio $x^n - a$ debe tener $n$ soluciones). Para conocerlas utilizamos

`.nth_root(n)`

donde debemos indicar como argumento el orden de la raíz deseada. Por defecto Sage solamente nos devuelve una de las raíces.  Si utilizamos la opción `all = True` nos devuelve una lista con todas las raíces.  Para el caso de la raíz cuadrada se puede utilizar `sqrt()`.

```
sage: i = CC(i)
sage: a = 4.6 + 9.68*i
sage: b = a.nth_root(5); b
1.56634459784419 + 0.359216283265026*I
sage: b^5
4.59999999999999 + 9.68000000000000*I
sage: a.nth_root(5, all = True)
[1.56634459784419 + 0.359216283265026*I,
 0.142392112822719 + 1.60068617272853*I,
 -1.47834143238984 + 0.630062176803190*I,
 -1.05605736501685 - 1.21128633243841*I,
 0.825662086739775 - 1.37867830035833*I]
sage: sqrt(a)
2.76743452871272 + 1.74891219639849*I
sage: sqrt(a, all = True)
[2.76743452871272 + 1.74891219639849*I,
 -2.76743452871272 - 1.74891219639849*I]
```

## Miscelánea


Normalmente los números se escriben en base 10, pero se pueden expresar en otras bases. Sage está especialmente preparado para esta circunstancia. Si escribimos un número entero que comience por `0`, Sage entenderá que es un número escrito en el sistema octal. Si el número empieza por `0x` entiende que es un número escrito en hexadecimal. Para transformar un número a otra base $b$ (necesariamente menor que 36, que es la suma de las diez cifras y las 26 letras) se emplea el método

`.str(b)`

```
sage: 10, 11, 12 # decimal
(10, 11, 12)
sage: 010, 011, 012 # octal
(8, 9, 10)
sage: 0x10, 0x11, 0x12 # hexadecimal
(16, 17, 18)
sage: 03456 + 0x23a # Se pueden mezclar. Resultado en decimal
2408
sage: 348.str(2)
'101011100'
sage: 348.str(23)
'f3'
```


La factorial de un número entero positivo es el producto de todos los números enteros positivos menores o iguales que el dado

`.factorial()`

La sucesión de Fibonacci es $1,1,2,3,5,8, \dots$. Los dos primeros términos son la unidad. Los siguientes se obtienen sumando los dos anteriores. Para conocer el término $n$-ésimo de la sucesión

`fibonacci()`

En una sección anterior hemos dado la definición más elemental del concepto de máximo común divisor. Sin embargo muchas veces es más útil utilizar la siguiente. Dados dos enteros $p$ y $q$, consideremos todos los números naturales que se pueden expresar en la forma $np+mq$, siendo $n$ y $m$ números enteros arbitrarios. El elemento mínimo de dicho conjunto coincide con el máximo común divisor. Además del máximo común divisor $d$, muchas veces es necesario conocer una pareja de números enteros $n,m$ que cumplan $np+mq=d$. En esencia lo comentado anteriormente es el conocido *Lema de Bezout*. Para obtener todo empleamos el comando

`.xgcd()`

```
sage: 4.factorial(), factorial(4), factorial(5), 5*factorial(4)
(24, 24, 120, 120)
sage: fibonacci(6), fibonacci(7), fibonacci(8)
(8, 13, 21)
sage: xgcd(78,24)
(6, 9, -29)
sage: 9*78 + (-24)*29
6
```