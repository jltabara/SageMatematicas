# Álgebra Lineal

El concepto fundamental del Álgebra Lineal es el de espacio vectorial. Un espacio vectorial es un grupo abeliano donde se ha definido una multiplicación por escalares. Los escalares forman siempre un cuerpo (si solamente forman un anillo, la estructura se denomina {\sf módulo}). Fijado un cuerpo $k$, para cada número entero $n$ existe un único (salvo isomorfismos) $k$-espacio vectorial de dimensión $n$. También existen espacios vectoriales de dimensión infinita, aunque no trataremos ahora de ellos. El espacio coordenado $k^n$ con las estructuras canónicas es un modelo de espacio vectorial de dimensión $n$.

## Espacios vectoriales


Una cualquiera de las órdenes


`VectorSpace(k, n), k^n`


crea el espacio vectorial $k^n$.  Es lo que denominaremos espacio vectorial ambiente.   Para construir un vector de un espacio vectorial `V` empleamos la orden


`V(lista)`


Naturalmente la lista debe  tener el tamaño adecuado a la dimensión del espacio vectorial.

Para saber cual es la dimensión de un espacio vectorial `V` simplemente escribimos su nombre y el programa nos da todos los datos del espacio vectorial. También podemos utilizar el método


`.dimension()`


Todo espacio vectorial de dimensión $n$ posee una base, formada por $n$ vectores linealmente independientes.  Aunque existan muchas bases en un espacio vectorial, Sage realiza una elección y asocia a cada espacio vectorial una base.  En el caso de los espacios vectoriales canónicos $k^n$ existe una base también canónica y coincide con la que le asocia el programa.  Para obtener una base de un espacio vectorial se emplea uno de los comandos


`.basis(), basis_matrix()`


La diferencia es puramente técnica.  En el primer caso el resultado es una lista y en el segundo es una matriz, donde las filas forman los vectores de la base.

```
sage: V = VectorSpace(QQ, 5); V
Vector space of dimension 5 over Rational Field
sage: W = QQ^5; W
Vector space of dimension 5 over Rational Field
sage: V == W 
True
sage: a = V([3,7,9,2/7,1])
sage: a in V
True
sage: V.dimension()
5
sage: V.basis()

[
(1, 0, 0, 0, 0),
(0, 1, 0, 0, 0),
(0, 0, 1, 0, 0),
(0, 0, 0, 1, 0),
(0, 0, 0, 0, 1)
]
sage: V.basis_matrix()

[1 0 0 0 0]
[0 1 0 0 0]
[0 0 1 0 0]
[0 0 0 1 0]
[0 0 0 0 1]
```



Dentro del espacio vectorial ambiente existen subconjuntos que tienen estructura de espacio vectorial.  Son los denominados subespacios vectoriales.  Para especificar un subespacio vectorial debemos dar un conjunto de vectores que lo generen, utilizando la orden


`.subspace(lista)`


Los subespacios son ellos mismos espacios vectoriales, por ello todo lo dicho para espacios vectoriales vale para subespacios. El problema que tiene este comando es que nosotros damos los vectores que generan y Sage "elige" una base del subespacio. Posiblemente no sea la base que a nosotros nos interese.  Si queremos fijar una base del subespacio empleamos el comando


`.subspace_with_basis(lista)`


En este caso la lista deben ser vectores linealmente independientes y formarán la base asociada a dicho subespacio.

```
sage: V = QQ^5
sage: a = V([2,5,7,2,7]); b = V([2,0,8,2,5]); c = V([1,8,3,2,-90])
sage: # El subespacio generado por a y b es
sage: W = V.subspace([a,b]
sage: W.basis_matrix()

[   1    0    4    1  5/2]
[   0    1 -1/5    0  2/5]
sage: # Ahora construimos un subespacio vectorial donde a y b sean base
sage: Z = V.subspace_with_basis([a,b])
sage: Z.basis_matrix()

[2 5 7 2 7]
[2 0 8 2 5]
sage: Z == W
True
```

Si `W` y `Z` son subespacios de un mismo espacio vectorial (el llamado espacio vectorial ambiente) podemos realizar la intersección conjuntista de ambos subespacios. Resulta que el conjunto resultante es nuevamente un subespacio. La orden es


`W.intersection(Z)`


El mínimo subespacio  que contiene a la vez a `W` y a `Z` se denota habitualmente `W + Z` y se dice que es la suma de dos subespacios.  En Sage se utiliza la misma notación.




Todo espacio vectorial (en particular los subespacios) en Sage tienen vectores de $n$ coordenadas (aunque el espacio vectorial puede tener dimensión menor). Se denomina {\sf grado} de un espacio vectorial precisamente al tamaño de los vectores que lo contienen. Si un espacio vectorial tiene grado $n$, es de modo natural un subespacio de $k^n$.  Este último conjunto es lo que se denomina *espacio vectorial ambiente*.  Las órdenes para extraer la información son


`.degree(), .ambient_vector_space()`


```
sage: V = QQ^5
sage: a = V([2,5,7,2,7]); b = V([2,0,8,2,5]); c = V([1,8,3,2,-90])
sage: W = V.subspace([a,b])
sage: Z = V.subspace([b,c])
sage: W.degree()
5
sage: W.dimension()
2
sage: W.intersection(Z)

Vector space of degree 5 and dimension 1 over Rational Field
Basis matrix:
[  1   0   4   1 5/2]
sage: W + Z

Vector space of degree 5 and dimension 3 over Rational Field
Basis matrix:
[     1      0      0  -17/3 1281/2]
[     0      1      0    1/3  -63/2]
[     0      0      1    5/3 -319/2]
```


## Las matrices y los espacios vectoriales

Las matrices aparecen en prácticamente cualquier campo de las Matemáticas. Sin embargo su origen histórico (relacionado con la resolución de sistemas lineales) hacen que su relación con el Álgebra Lineal sea muy estrecha. Veamos algunos puntos donde las matrices se aplican al Álgebra Lineal.

Fijadas bases en dos espacios vectoriales, cada aplicación lineal entre los espacios vectoriales da lugar a una matriz.  El tamaño de la matriz es el producto de las dimensiones de  ambos espacios.  La correspondencia inversa también es correcta: a cada matriz le corresponde una aplicación lineal (una vez fijadas las bases).  Esta es la primera relación que guardan las matrices con los espacios vectoriales.

Si tomamos una matriz, cada una de sus filas puede considerarse como un vector. Por lo tanto una matriz "es"   un conjunto de vectores. Lo mismo puede decirse de las columnas de la matriz.

Cada matriz  simétrica induce un producto escalar (no necesariamente definido positivo) en un espacio vectorial (una vez fijada una base). Aunque Sage permite tratar este aspecto, no lo discutiremos en estas notas.

En todo lo que sigue supondremos que las matrices y vectores tienen los tamaños adecuados para poder realizar las multiplicaciones.  Asimismo a veces debemos entender un vector como una columna y no como una fila.

Dada una matriz $A$, se dice que un vector $v$  pertenece al núcleo de $A$ si $vA=0$.  Es fácil ver que el núcleo de cualquier matriz es siempre un subespacio vectorial.  Esta el la noción de Sage para el núcleo de una matriz.  Sin embargo en Álgebra Lineal es más habitual considerar el núcleo de una matriz al conjunto $v$ de vectores que satisfacen $Av=0$. Por ello Sage tiene varios comandos relacionados con matrices que comienza o bien por`left` o bien por `right`. En particular para calcular los núcleos de una matriz tenemos los comandos


`.left_kernel(), .right_kernel()`



```
sage: A = random_matrix(QQ, 3, 5); A
[   0    1    0    0    0]
[  -1   -1 -1/2    1  1/2]
[   1    2    1    0   -2]
sage: A.kernel()
Vector space of degree 3 and dimension 0 over Rational Field
Basis matrix:
[]
sage: A.left_kernel()
Vector space of degree 3 and dimension 0 over Rational Field
Basis matrix:
[]
sage: A.right_kernel()
Vector space of degree 5 and dimension 2 over Rational Field
Basis matrix:
[  1   0   0 3/4 1/2]
[  0   0   1 1/4 1/2]
sage: # Comprobemos que el primer vector de la base pertenece al núcleo
sage: A * vector(QQ, [1, 0, 0, 3/4, 1/2]).transpose()
[0]
[0]
[0]
sage: # Veamos que kernel es lo mismo que left_kernel en Sage
sage: A = random_matrix(QQ, 5, 3)
sage: A.kernel() == A.left_kernel()
True
```

Dada una matriz `A`, entendemos que sus columnas son vectores de la dimensión adecuada.  El subespacio que generan las columnas de la matriz se obtiene con el método


`.column_space()`


Un resultado de Álgebra Lineal dice que la dimensión de este subespacio vectorial coincide con el rango de la matriz (que es número de vectores columna linealmente independientes).  Si queremos hacer lo mismo, pero con los vectores fila, la orden es


`.row_space()`


El espacio vectorial tiene como cuerpo asociado el mismo que la matriz.

## Polinomio característico y autovectores



Consideremos un espacio vectorial $V$ y un endomorfismo $\phi$.  Fijada una base de dicho espacio vectorial, al endomorfismo $\phi$ le asociamos la matriz $A$. Todo lo que vamos a decir se refiere a la matriz, pero una comprensión más adecuada se tiene entendiendo la matriz como un endomorfismo.


Dada una matriz cuadrada $A$, decimos que un vector $v$ es un *autovector* si existe un escalar $\lambda$ tal que $Av=\lambda v$.  El escalar $\lambda$ se denomina *autovalor* asociado a dicho vector.  Se tienen los siguientes resultados de Álgebra Lineal.



- Fijado un autovalor $\lambda$, el conjunto de vectores $V_\lambda$ que son autovectores con dicho autovalor, es un subespacio vectorial.

- Dados dos autovalores distintos $\lambda$ y $\mu$, sus subespacios vectoriales asociados $V_\lambda$ y $V_\mu$ tienen intersección vacía.  También se dice que los subespacios están en posición de suma directa.

- Los autovalores de una matriz son las raíces del polinomios característico (también las soluciones del polinomio mínimo).  Debido a esto el estudio de los autovalores y autovectores depende fuertemente del cuerpo base. La situación ideal se produce cuando el cuerpo es algebraicamente cerrado.

- En general no es cierto que la suma de todos los subespacios asociados a todos los autovalores sea el espacio total (ni aun en el caso de cuerpos algebraicamente cerrados).  En el caso en que esto ocurra se dice que la matriz es diagonalizable.



El polinomio característico y el mínimo se obtienen con


`.charpoly(), .minpoly()`


En muchos casos estos dos polinomios coinciden. A nivel operativo suele ser más sencillo calcular el polinomios característico, puesto que basta desarrollar un determinante. Sin embargo a nivel teórico es más interesante el polinomio mínimo, que es el polinomio mónico y de menor grado que \guillemotleft anula\guillemotright\ a la matriz (un polinomio $p(x)$ anula a una matriz $A$ si $p(A)$ es la matriz nula). El *Teorema de Hamilton-Cayley* nos dice que el polinomio mínimo es siempre un divisor del característico.


```
sage: # Construiremos una matriz C que tenga autovalores y no sea trivial
sage: A = diagonal_matrix(QQ,[4, 4, -5, 1, 12])
sage: B = random_matrix(ZZ, 5, x = 5)
sage: C = B * A * B^(-1); C

[-1089/268    662/67  -415/268  -411/134   267/134]
[-1807/134    731/67   707/134   -129/67    477/67]
[-1597/268    620/67   237/268  -207/134   111/134]
[-1831/134    736/67   263/134     79/67    381/67]
[-1573/134    967/67  -391/134   -147/67    475/67]
sage: f = C.charpoly()
sage: g = C.minpoly()
sage: # El polinomio minimo siempre es divisor del caracteristico 
sage: g.divides(f)
True
sage: # Ademas tienen las mismas raices (aunque distinta multiplicidad)
sage: f.roots(); g.roots()
[(12, 1), (1, 1), (-5, 1), (4, 2)]
[(12, 1), (4, 1), (1, 1), (-5, 1)]
```

El conjunto de autovalores de una matriz se obtiene con el comando


`.eigenvalues()`



Para obtener los subespacios vectoriales $V_\lambda$, utilizamos el comando


`.eigenspaces_right()`


```
sage: # Seguimos con las definiciones anteriores
sage: C.eigenvalues()
[12, 1, -5, 4, 4]
sage: # El autovalor doble aparece dos veces
sage: factor(C.charpoly())
(x - 12) * (x - 1) * (x + 5) * (x - 4)^2
sage: C.eigenspaces_right()

[
(12, Vector space of degree 5 and dimension 1 over Rational Field
User basis matrix:
[1 2 1 2 2]),
(1, Vector space of degree 5 and dimension 1 over Rational Field
User basis matrix:
[  1 3/4 1/2   1 3/4]),
(-5, Vector space of degree 5 and dimension 1 over Rational Field
User basis matrix:
[  1   0   1 2/3 4/3]),
(4, Vector space of degree 5 and dimension 2 over Rational Field
User basis matrix:
[   1    0   -1 -1/2  5/2]
[   0    1    2  3/4 -9/4])
]
sage: # Comprobemos que el primero es efectivamente un autovector de valor 12
sage: C * vector([1,2,1,2,2])
(12, 24, 12, 24, 24)
```

