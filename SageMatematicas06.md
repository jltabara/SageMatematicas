# Álgebra Lineal Elemental

## Vectores y operaciones

Los objetos más básicos del Álgebra Lineal son los vectores. A nivel elemental un vector es simplemente una colección ordenada de números. Si tenemos dos números decimos que el vector es de dos dimensiones, si tenemos una terna hablamos de vectores tridimensionales, \dots

Para construir un vector se utiliza la orden


`vector(R, lista)`


Las listas en Sage siempre van entre corchetes. El tamaño de la lista coincide con la dimensión del vector.  Matemáticamente todo vector es un elemento de cierto $A^n$, donde $A$ es anillo.  Para indicarle a Sage el anillo o cuerpo que contiene a los coeficientes, lo introducimos como opción en el comando que crea los vectores.  Si no introducimos ninguno, Sage asocia a cada vector el mínimo anillo que contiene a los coeficientes que forman parte del vector.  Para saber cual es el conjunto que considera Sage que contiene a los coeficientes 


`.base_ring()`



```
sage: u = vector([2, 5, 6])
sage: type(u)
<type 'sage.modules.vector_integer_dense.Vector_integer_dense'>
sage: u.base_ring()
Integer Ring
sage: v = vector([4, 3/5, 8/3])
sage: type(v)
<type 'sage.modules.vector_rational_dense.Vector_rational_dense'>
sage: v.base_ring()
Rational Field
sage: w = vector([3, 6, 1.56])
sage: type(w)
<type 'sage.modules.free_module_element.FreeModuleElement_generic_dense'>
sage: w.base_ring()
Real Field with 53 bits of precision
sage: u + v
(6, 28/5, 26/3)
sage: 4*u + 6*v - 1.09*w
(28.7300000000000, 17.0600000000000, 38.2996000000000)
sage: # Podemos cambiar el anillo asociado a un vector
sage: a = vector(QQ, [2, 5, 6]);  a.base_ring()
Rational Field
```


Para realizar el producto escalar de dos vectores se pueden emplear indistintamente los siguientes métodos (aunque también se puede utilizar el asterisco)


`.dot_product(), .inner_product()`



La norma de un vector (que es la raíz cuadrada del producto escalar del vector consigo mismo)


`.norm()`



```
sage: u = vector([2, 5, 6]);  v = vector([4, 3/5, 8/3])
sage: u.dot_product(v)
27
sage: v.inner_product(u)
27
sage: u * v  # La multiplicacion de vectores es la escalar
27
sage: u.norm(), u * u
(sqrt(65), 65)
```


Para el producto vectorial (definido solamente para vectores tridimensionales)


`.cross_product()`


Aunque no es muy habitual, también a veces es necesario multiplicar dos vectores coordenada a coordenada, siendo el resultado un vector de la misma dimensión


`.pairwise_product()`


Para transponer un vector  y \guillemotleft colocarlo\guillemotright\ vertical


`.transpose()`


El resultado de transponer un vector, no es un vector sino una matriz.

Sage puede dibujar vectores de dos y tres dimensiones. El método para hacerlo es


`.plot()`



```
sage: u = vector([2, 5, 6])
sage: v = vector([4, 3/5, 8/3])
sage: # El producto vectorial no conmuta, sino que anticonmuta
sage: u.cross_product(v); v.cross_product(u)
(146/15, 56/3, -94/5)
(-146/15, -56/3, 94/5)
sage: u.pairwise_product(v)
(8, 3, 16)
sage: u.transpose()

[2]
[5]
[6]
sage: type(_)
<type 'sage.matrix.matrix_integer_dense.Matrix_integer_dense'>
sage: u = vector([2,4])
sage: u.plot()
```

\begin{figure}[ht]
 \centering
 \includegraphics[width=300pt]{vector.png}
 \caption{Vector bidimensional construido con  u.plot()}
\end{figure}



## Matrices y operaciones





A nivel operativo, puede decirse que la parte fundamental del Álgebra Lineal es la teoría de matrices.  A nivel matemático un vector es simplemente una matriz de una sola fila. Sin embargo para Sage son distintos y se definen con distintos comandos.

Para construir una matriz utilizamos el comando


`matrix(R, [f1, f2, ... , fn])`


Una matriz es una "lista de listas"t.  Por lo tanto dentro del paréntesis debe ir situado siempre un corchete.  Dentro de dicho corchete deben figurar las filas de la matriz en cuestión.  Dichas filas son ellas mismas listas, por lo que también debemos colocarlas entre corchetes. De la misma forma que en los vectores, podemos cambiar el anillo asociado, añadiéndolo como opción.

Las operaciones con matrices y vectores se realizan con los operadores habituales. El producto matricial se realiza con el asterisco y las potencias con el circunflejo.


```
sage: A = matrix([[1,4,6],[3,6,8],[-3,8,1]]); A

[ 1  4  6]
[ 3  6  8]
[-3  8  1]
sage: B = matrix([[-3,7,9],[8,-3,-7],[-2,0,9]]); B

[-3  7  9]
[ 8 -3 -7]
[-2  0  9]
sage: type(A)
<type 'sage.matrix.matrix_integer_dense.Matrix_integer_dense'>
sage: A.base_ring()
Integer Ring
sage: A + B 

[-2 11 15]
[11  3  1]
[-5  8 10]
sage: 3*A - 5*B

[ 18 -23 -27]
[-31  33  59]
[  1  24 -42]
sage: # La multiplicación no es conmutativa
sage: A * B

[ 17  -5  35]
[ 23   3  57]
[ 71 -45 -74]
sage: B * A

[ -9 102  47]
[ 20 -42  17]
[-29  64  -3]
sage: A^3

[  91  788  622]
[ 111 1252  952]
[   9  712  507]
```


Escribir matrices de ordenes altos suele resultar tedioso. Sage es capaz de "inventarse" matrices por nosotros.  Para ello debemos indicarle de que tipo van a ser los elementos de la matriz, el tamaño de la matriz y el  máximo de los números aleatorios que puede generar el programa. La orden es 


`random_matrix(ZZ, n, m, x = max)`


Aquí `n` y `m` indican el tamaño de la matriz.  Si se omite alguno de ellos, el programa supone que la matriz es cuadrada.

Para constuir la matriz identidad de orden $n$


`identity_matrix(n)`


Para construir una matriz diagonal


`diagonal_matrix(lista)`


donde `lista` es una lista formada por los elementos de la diagonal principal.


```
sage: A = random_matrix(ZZ, 3, 4, x = 10); A
[7 4 7 7]
[2 2 0 3]
[5 6 6 4]
sage: I = identity_matrix(3)
sage: I * A == A
True
sage: D = diagonal_matrix([3,6,-2,7]); D
[ 3  0  0  0]
[ 0  6  0  0]
[ 0  0 -2  0]
[ 0  0  0  7]
```


## Determinantes

Los determinantes son números asociados a las matrices cuadradas.  En ellos está resumido cierta información algebraica y geométrica relacionada con la matriz.  Es conocido que una matriz es invertible si y solamente si su determinante es distinto de cero.   Si la matriz no es cuadrada, \guillemotleft tachando\guillemotright\ filas y columnas podemos conseguir matrices cuadradas.  Los determinantes de dichas matrices  son los llamados menores.  Ligado a estos menores está el concepto de rango, que es el orden del mayor menor no nulo (aunque lo parezca no es un juego de palabras).  Los menores también sirven para construir la llamada matriz adjunta, muy relacionada con la inversa de la matriz.




Para calcular el determinante hay dos órdenes sinónimas


`.det(), .determinant() `


Para transponer una matriz


`.transpose()`



```
sage: A = random_matrix(ZZ, 4, x = 10); A

[4 2 3 7]
[6 9 3 2]
[4 9 2 5]
[1 6 0 4]
sage: B = A.transpose()
sage: A.det(); B.det()
62
62
sage: # Veamos la potencia de calculo de Sage
sage: A = random_matrix(ZZ, 500, x = 100)  # Una matriz de buen tamaño
sage: # Si escribimos time delante de un comando, nos dice lo que tarda
sage: # nuestro ordenador en realizar la operacion
sage: time A.det()
CPU times: user 9.21 s, sys: 0.07 s, total: 9.28 s
Wall time: 9.47 s
-51312118766062178215790287935910377804815521445276795844487992527289790
185754714277249296185253535819264346574960388348158150444222215717575271
582620208094273237654671407368449045494839625212568472820719182539369433
864144720619663533251447469207291330153663448885671161958743048804994401
033369198349588668211157092638507512238595926560582643712759086633633708
482592874208145460666299045405171350050208266234159017778314185118760333
122454222253557536031505255901630478649166521589863768860741764828469001
683955795874513743653697706392383461862894573146608266064281142813682113
343342262901515285739793161679021156636778269697375440293345428819927467
796075365373944915716559812112994273303001261081017214968312830561064174
742102238656132613385336089794969093158673579033166743398037770518036090
200513908945937873038712551913007436660951653274622106974218118890880317
868291047134215148396261992465767292143426822492602358671070588167633756
452096207163642405442038989272613126779682918059872287314653467587309168
493206248897710248054823898698472410039075480209369247191548256509067865
446925519063716764816895631173310097276903861710660521969905714397564184
207334741277542639830742489806961602424027410160496053353915810429096033
824699653357329390587994669534905897466587232310063644315336894711907545
5330
```

Si la matriz es invertible, su inversa se calcula elevando a -1 o con la orden



`.inverse()`





Los menores de una matriz 



`.minors(n)`


donde `n` es el tamaño de los menores.  Debemos tener cuidado pues la colocación de los menores puede resultar inesperado para muchos, puesto que no es el mismo que el que se utiliza en la matriz adjunta.

El rango de la matriz



`.rank()`


La matriz adjunta



`.adjoint()`


En España se suele denominar matriz adjunta a la formada por los menores. Si queremos construir la matriz inversa, esta matriz adjunta, la debemos transponer y después dividir por un número.  Sin embargo en los países anglosagones denominan matriz adjunta a lo que para nosotros sería la matriz adjunta y transpuesta.  Naturalmente Sage sigue la convención anglosagona.



```
sage: A = random_matrix(ZZ, 4, x = 10)
sage: A.det()
-906
sage: # Determinante no nulo, implica rango maximo
sage: A.rank()
4
sage: # Dos formas de calcular la inversa
sage: A^(-1)

[  38/453   61/453 -104/453   -8/453]
[ -22/151  -15/302   33/302   49/302]
[   7/151  -57/302   65/302    5/302]
[  -2/151   81/302    3/302  -23/302]
sage: A.inverse()

[  38/453   61/453 -104/453   -8/453]
[ -22/151  -15/302   33/302   49/302]
[   7/151  -57/302   65/302    5/302]
[  -2/151   81/302    3/302  -23/302]
sage: # Observese la colocacion de los menores y el signo
sage: A.adjoint()

[ -76 -122  208   16]
[ 132   45  -99 -147]
[ -42  171 -195  -15]
[  12 -243   -9   69]
sage: A.minors(3)
[69, 15, -147, -16, 9, -195, 99, 208, -243, -171, 45, 122, -12, -42, -132, -76]
```




## Operaciones elementales con matrices

Llamamos operación elemental a la suma de un múltiplo de una columna a otra columna (también con filas). Las operaciones elementales tienen la propiedad de que no varían el rango de la matriz (la explicación de este hecho en el capítulo siguiente).  Las operaciones con filas también útiles en el método de Gauss, que es uno de los métodos elementales para resolver sistemas de ecuaciones lineales.

Las operaciones elementales con Sage presentan el problema de que la numeración empieza en cero.  Para definir una operación elemental debemos dar 3 datos: la fila a la que vamos a multiplicar, el escalar por el que vamos a multiplicar y la fila a la que vamos a sumarle el resultado anterior. El método en el programa es


`.add_multiple_of_row(f1, f2, lambda)`


Esta operación sustituye la fila`f1` por`f1 + lambda * f2`.


Si queremos hacer operaciones elementales con columnas el comando es


`.add_multiple_of_row(f1, f2, lambda)`


Debemos tener en cuenta que estos comandos cambian a la matriz sobre la que se ejecutan.

Normalmente se utilizan las operaciones elementales para triangularizar la matriz. En Sage hay dos comandos para triangularizar matrices


`.echelon_form(), .echelonize()`


El primero nos muestra como quedaría la matriz triangulada, sin embargo el segundo cambia la matriz sobre la que actua y la triangulariza (es un detalle técnico, pero importante).

```
sage: A = identity_matrix(4)
sage: A.add_multiple_of_row(0, 3, 100); A  # Le estamos sumando a la fila 1 
[  1   0   0 100]
[  0   1   0   0]
[  0   0   1   0]
[  0   0   0   1]
sage: # Observamos que la matriz A ha cambiado
sage: A = random_matrix(ZZ, 4, x = 10)
sage: A.echelon_form()  # La matriz A no cambia
[   1    0    0  670]
[   0    1    0  222]
[   0    0    1   45]
[   0    0    0 1668]
sage: A
[2 8 5 5]
[3 5 5 9]
[7 0 7 1]
[2 9 0 2]
sage: A.echelonize(); A  # Ahora si que cambiamos la matriz A

[   1    0    0  670]
[   0    1    0  222]
[   0    0    1   45]
[   0    0    0 1668]
```



## Sistemas de ecuaciones



Muchas ecuaciones matriciales son del tipo $A \cdot X = B$, donde $A$ y $B$ son matrices dadas y $X$ es la matriz incógnita.  Sage es capaz de resolver sistemas de este tipo con el método


`A.solve_right(B)`


Del mismo modo las ecuaciones de la forma $X \cdot A = B$ se resuelve con el método


`A.solve_left(B)`


El primer caso incluye de modo natural los sistemas de ecuaciones lineales, que es el caso en que $B$ es una matriz columna. Aunque $B$ teóricamente debe ser una matriz columna, si en su lugar colocamos un vector, el programa lo interpreta como una columna.

Si el sistema es compatible determinado ($X$ existe y es única) entonces el programa calcula dicha solución. Si el sistema es incompatible ($X$ no existe) Sage produce un error.  Si el sistema es compatible indeterminado (existen varias $X$ que cumplen la ecuación) el programa únicamente calcula una de ellas.


```
sage: A = random_matrix(ZZ, 4); A

[-4 -5  1  1]
[ 1 -7  2 -1]
[-6  1  1  4]
[ 1 -3  1 -4]
sage: B = random_matrix(ZZ, 4, 2); B

[ 1  0]
[-4  0]
[ 1 -1]
[ 2 -1]
sage: X = A.solve_right(B); X

[-27/23   4/23]
[ 17/69 -17/69]
[-82/69 -56/69]
[-88/69  19/69]
sage: A * X

[ 1  0]
[-4  0]
[ 1 -1]
[ 2 -1]
sage: # Si B es un vector, Sage comprende lo que queremos hacer
sage: b = vector([1,4,7, 90])
sage: X = A.solve_right(b); X
(-265/23, 2039/207, 6116/207, -5254/207)
sage: A * X
(1, 4, 7, 90)
```
