## Cuaterniones

El invento o descubrimiento de los cuaterniones se debe al matemático irlandés Hamilton. Una vez que fue asimilado por la comunidad matemática que los números complejos eran simplemente pares de números reales con unas reglas especiales de multiplicación, Hamilton intentó realizar lo mismo con ternas de números. Fracasó, naturalmente, puesto que hoy es conocido que no puede existir ningun cuerpo de grado 3 sobre los números reales.  Sin embargo, en un momento de inspiración, descubrió las reglas para multiplicar cuaternas de números.  Estas reglas cumplían todas las propiedades, salvo la propiedad conmutativa de la multiplicación.  A día de hoy sabemos construir, a semejanza de lo hecho por Hamilton, extensiones de cuerpos (no necesariamente del cuerpo real) de grado 4 y que cumplen propiedades similares.  Nosotros trabajaremos con el Álgebra cuaternionica más simple, que es aquella en la que los generadores $i,j,k$ cumplen $i^2= -1$, $j^2=-1$ e $ij=k$, construida sobre el cuerpo $\Q$.

La orden para crear el Álgebra cuaternionica es 

`A.<i,j,k> = QuaternionAlgebra(QQ, -1, -1)`

Una vez hecho esto, cualquier cuaternión se puede escribir como una combinación lineal de la unidad y de las tres letras. Las operaciones, como siempre, se realizan con los operadores habituales.  Debemos tener en cuenta que la multiplicación no es conmutativa.

```
sage: A.<i,j,k> = QuaternionAlgebra(QQ, -1, -1)
sage: a = 4 + 5*i + 8*j + 2*k
sage: type(a)
<class 'sage.algebras.quaternion_algebra_element.QuaternionAlgebraElement'>
sage: b = 8 + 2*i -9*j + 3*k
sage: a * b
88 + 90*i + 17*j - 33*k
sage: b * a
88 + 6*i + 39*j + 89*k
sage: a + 7*b
60 + 19*i - 55*j + 23*k
sage: b^4
-23164 - 1920*i + 8640*j - 2880*k
sage: c = 1 / b  # El inverso de b 
sage: b * c
1
```

El conjugado de un cuaternión $a+bi+cj+dk$ es $a-bi-cj-dk$

`.conjugate()`

La norma de un cuaternión es su módulo en el espacio de cuatro dimensiones. Coincide con la multiplicación de un cuaternión por su conjugado (como en el caso complejo)

`.reduced_norm()`

La parte real (también llamada escalar) del cuaternión anterior es $a$ y la parte vectorial $bi+cj+dk$. Un cuaternión se dice que es imaginario puro (o simplemente que es un cuaternión puro), si su parte real es nula, por analogía con el caso complejo.

`|.scalar_part(), .pure_part()`

```
sage: a = 4 + 5*i + 8*j + 2*k
sage: a.reduced_norm()
109
sage: a * a.conjugate()
109
sage: a.conjugate() * a  # Un cuaternion y su conjugado conmutan
109
sage: a.scalar_part()
4
sage: a.pure_part()
5*i + 8*j + 2*k
```


