
# Teoría  elemental de números

## Los anillos $\mathbb{Z}_n$

A comienzos del siglo 19 Gauss se dió cuenta de que en  la Teoría de Números  era fundamental el estudio de las congruencias.  Desde un punto de vista moderno, esto equivale a comprender la estructura de los anillos $\mathbb{Z}_n$. El caso en que $n$ es primo es especial, ya que en este caso el anillo es un cuerpo.

Una vez construido el anillo en cuestión, todas las operaciones aritméticas se realizan con los operadores habituales.  Es importante no confundir los elementos de los anillos cociente $\mathbb{Z}_n$ con los elementos del anillo de los enteros. Cuando tengamos dudas le podemos preguntar al programa.

La construcción del anillo $\mathbb{Z}_n$ se realiza con la instrucción


 `IntegerModRing(n)`



```
sage: R = IntegerModRing(23)  # Creamos el anillo Z_23 y lo llamamos R
sage: R
Ring of integers modulo 23
sage: a = R(12)  # Asi se crean los elementos del anillo
sage: type(a)
<type 'sage.rings.integer_mod.IntegerMod_int'>
sage: parent(a)
Ring of integers modulo 23
sage: a.base_ring()
Ring of integers modulo 23
sage: b = R(14)
sage: a + b, a - b, a * b, a / b
(3, 21, 7, 14)
sage: a^(-1)  # El inverso de a, que en este caso existe
2
sage: a * R(2)
1
sage: a^2345
6
```


Si el anillo no tiene módulo primo, existen elementos invertibles (los primos con el módulo) y también elementos que no tienen inverso.  Para averiguarlo tenemos el método


 `.is_unit()`


Una vez que sabemos que es invertible podremos calcular el inverso (que es único).

De la misma forma, no todo elemento del anillo cociente es un cuadrado


 `.is_square()`

 
Si un elemento es un cuadrado se puede extraer su raíz cuadrada con el método `.sqrt()`. Puede ocurrir que la raíz cuadrada no sea única. Si queremos obtenerlas todas debemos añadir la opción  `all = True`


```
sage: R = IntegerModRing(88)
sage: a = R(4)
sage: a.is_unit()
False
sage: a.is_square()
True
sage: a.sqrt()   # Una raíz cuadrada
2
sage: a.sqrt(all = True)  # Todas las raices cuadradas
[2, 42, 46, 86]
sage: R(2)^2, R(42)^2, R(46)^2, R(86)^2
(4, 4, 4, 4)
```




## Funciones multiplicativas}


Las funciones multiplicativas se utilizan habitualmente en Teoría de Números y están caracte\-rizadas por satisfacer la siguiente propiedad

$$
f(nm)= f(n)f(m) \quad \text{ si } n \text{ y } m \text{ son primos entre si}
$$

Por ello, si conocemos $f(p)$ y $f(q)$, siendo $p$ y $q$ números primos, podemos conocer $f(pq)$.  Por el teorema fundamental de la aritmética, si se conoce $f(p^k)$  para cualquier primo, podemos conocer $f(n)$ para cualquier $n$.




Dado un número $n$, consideramos todos sus divisores positivos, incluyendo la unidad y él mismo. Posteriormente sumamos todos los divisores. El número así obtenido es costumbre denotarlo $\sigma(n)$. Hemos construido así la función $\sigma: \N \rightarrow \N$. Esta función es multiplicativa y es sencillo calcular su valor para las potencias de primos.


 `sigma(n)`



```
sage: sigma(8)
15
sage: divisors(8)
[1, 2, 4, 8]
sage: 1 + 2 + 4 + 8
15
sage: # Calculemos sigma sobre un numero primo
sage: sigma(7)
8
sage: # Calculemos sigma sobre potencias de primos
sage: sigma(7), sigma(7^2), sigma(7^3), sigma(7^4)
(8, 57, 400, 2801)
sage: # Para obtener estos numeros debemos sumar progresiones geometricas
sage: sigma(7^4), 1 + 7 + 7^2 + 7^3 + 7^4
(2801, 2801)
sage: # Comprobemos que es multiplicativa
sage: n = 2^5 * 3^6
sage: m = 5^4 * 7^3
sage: n,m
(23328, 214375)
sage: gcd(n,m)
1
sage: sigma(n*m), sigma(n) * sigma(m)
(21511551600, 21511551600)
sage: # Los numeros perfectos son aquellos que sigma(n) = 2n
sage: sigma(6), 2 * 6
(12, 12)
sage: sigma(28), 2 * 28
(56, 56)
```


La función $\tau$ cuenta el número de divisores positivos, incluyendo la unidad y el mismo número.  Es también una  función multiplicativa.


 `number_of_divisors(n)`



```
sage: divisors(90)
[1, 2, 3, 5, 6, 9, 10, 15, 18, 30, 45, 90]
sage: number_of_divisors(90)
12
sage: # Sobre los numeros primos la funcion tau es sencilla
sage: number_of_divisors(23)
2
sage: # Tambien es sencilla sobre potencias de primos
sage: number_of_divisors(7^6), number_of_divisors(7^201)
(7, 202)
sage: # Veamos que es multiplicativa
sage: n = 2^4 * 3^7
sage: m = 5^3 * 11^5
sage: n, m, gcd(n,m)
(34992, 20131375, 1)
sage: number_of_divisors(n * m)
960
sage: number_of_divisors(n) * number_of_divisors(m)
960
```


La función $\mu$ de Möbius es un poco más complicada de definir. Tomamos un número natural $n$ y lo descomponemos en factores primos.  Si algún factor primo aparece con exponente $2$ o superior, entonces $\mu(n)=0$.  Si todos los factores que aparecen en $n$ son de grado $1$ entonces los contamos.  Si hay un número par de factores primos, entonces $\mu(n)=1$.  Si hay un número impar de factores entonces $\mu(n)=-1$. Es sencillo comprobar que si $\mu(n)=0$ entonces existe un cuadrado $r^2$ que divide a $n$. El inverso también es correcto: Si algún cuadrado divide a $n$ entonces $\mu(n)=0$. También es una función multiplicativa y en cierto sentido es la más importante de todas, debido a la fórmula de inversión de Möbius (ver cualquier libro de Teoría de Números).


 `moebius()`


Dado un número natural $n$ consideramos todos los números menores que él. De estos algunos son primos con $n$ y otros no. Consideramos únicamente los que son primos con $n$ y los contamos. Dicho número se denota $\phi(n)$. El primero en utilizar esta función de un modo consciente fue Euler, y hoy en dia se llama la función {\sf phi de Euler}.  Como hemos visto en la primera sección de este capítulo, el número $\phi(n)$ coincide con el conjunto de unidades del anillo $\mathbb{Z}_n$.


 `euler_phi()`




```
sage: moebius(2^2*3*5)  # Un factor repetido
0
sage: moebius(3*5*7)  # Un numero impar de factores, sin repeticion
-1
sage: moebius(3*5*11*23)  # Un numero par de factores sin repeticion
1
sage: euler_phi(7)  # Sobre los numeros primos es sencilla
6
sage: euler_phi(23)
22
sage: # Ahora sobre potencias de primos
sage: euler_phi(7^2)
42
sage: euler_phi(7^3)
294
sage: euler_phi(77655697987878788899999876)
38445632524277534820378240
``

