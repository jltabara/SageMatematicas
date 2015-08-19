# Ecuaciones y sistemas

Sage puede resolver muchos tipos de ecuaciones. Algunas las resuelve de un modo exacto y otras de un modo aproximado.  Además de esto también resuelve ecuaciones en modo simbólico. En este caso los coeficientes de la ecuación son letras.

## Ecuaciones elementales

En esta sección llamamos ecuaciones elementales a las polinómicas de grados bajos, para las que existe una fórmula (utilizando operaciones racionales y extracción de raíces) para su resolución. Los matemáticos italianos del Renacimiento encontraron las fórmulas de resolución para las ecuaciones de grado menor que 5. A principios de siglo 19 Abel y Ruffini demostraron que no existía ninguna fórmula general para la resolución de las ecuaciones de grado igual o superior al quinto. Finalmente, también en el siglo 19, Galois dió las condiciones necesarias y suficientes para que una ecuación de grado superior al cuarto fuese resoluble (por radicales).

En Sage **las ecuaciones están separadas por dos signos igual**. Si las separamos solamente por un signo igual nos dará un error. **Si no escribimos el doble igual el programa supone que la ecuación está igualada a cero**.

El comando para resolver ecuaciones es 

`solve(ecuacion, x)`

donde estamos suponiendo que la ecuación tiene como incógnita la letra $x$. Este comando no tiene en cuenta la multiplicidad de las soluciones.


```
sage: solve(x^2 - 5*x +6 == 0, x)  # Una ecuacion segundo grado
[x == 3, x == 2]
sage: solve(x^2 - 5*x + 6, x)  # Ahora sin el signo igual
[x == 3, x == 2]
sage: solve(x^2 - 4*x + 4 == 0, x)  # Una con solucion doble
[x == 2]
sage: solve(x^3 - 3*x + 2 == 0, x) 
[x == -2, x == 1]
sage: solve((x + 1)/9 +(3*x - 4)/45 == x + (5*x - 54)/12, x)
[x == 814/223]
sage: solve(x^8 - 10*x^6 + 37*x^4 - 60*x^2 + 36, x)
[x == -sqrt(2), x == sqrt(2), x == -sqrt(3), x == sqrt(3)]
sage: # Esta ecuacion de octavo grado la ha resuelto
sage: # Sin embargo una arbitraria quinto grado no la resuelve
sage: solve(x^5 -4*x^4 - 13*x^2 + 2*x -4, x)
[0 == x^5 - 4*x^4 - 13*x^2 + 2*x - 4]
```

Hasta ahora en todas las ecuaciones que hemos resuelto la incógnita (o variable) es $x$. Si intentamos repetir esto con otra letra, el programa nos informará de un error. Naturalmente se pueden resolver ecuaciones con otro nombre de variable, pero para ello debemos "informar" a Sage de que dicha letra la queremos considerar como una incógnita. Para ello debemos escribir

`var('letra')`

La letra debe ir necesariamente entre comillas simples. Se pueden "inicializar" más letras escribiendo varias separadas por comas (o simplemente por espacios).


```
sage: var('a')
a
sage: solve(a^2 - 5*a + 6, a)
[a == 3, a == 2]
sage: var('b,c,d,e')  # Ya podemos utilizar todas estas letras
(b, c, d, e)
sage: solve(d^2 - 5*d + 6, d)
[d == 3, d == 2]
sage: var('a b c d')
(a, b, c, d)
sage: solve(a*x^2 + b*x + c , x) # Formula general
[x == -1/2*(b + sqrt(b^2 - 4*a*c))/a,
x == -1/2*(b - sqrt(b^2 - 4*a*c))/a]
```


Es muy común que al resolver ecuaciones aparezcan soluciones complejas. El símbolo que usa el programa para la unidad imaginaria es `I`.


```
sage: solve(x^2 +4,x)  # Solucion compleja
[x == -2*I, x == 2*I]
sage: solve(3*x^3 + 5*x^2 - 4*x +5,x) 
[x == 61*(sqrt(3)*I/2 - 1/2)/(81*(sqrt(1423)/(18*sqrt(3)) - 2005/1458)^(1/3)) +
(sqrt(1423)/(18*sqrt(3)) - 2005/1458)^(1/3)*(-sqrt(3)*I/2 - 1/2) - 5/9, x ==
(sqrt(1423)/(18*sqrt(3)) - 2005/1458)^(1/3)*(sqrt(3)*I/2 - 1/2) + 
61*(-sqrt(3)*I/2 - 1/2)/(81*(sqrt(1423)/(18*sqrt(3)) - 2005/1458)^(1/3)) - 5/9,
x == (sqrt(1423)/(18*sqrt(3)) - 2005/1458)^(1/3) + 
61/(81*(sqrt(1423)/(18*sqrt(3)) - 2005/1458)^(1/3)) - 5/9]
sage: # La solucion puede ser exacta pero poco util
```


## Resolución de sistemas

Un sistema es simplemente un conjunto de ecuaciones.  En Sage se escriben formando lo que técnicamente se conoce como una lista. **Debemos escribir entre corchetes cada una de las ecuaciones y separarlas por comas**. Además, en el comando `solve`, después de escribir la lista, debemos escribir los nombres de las variables que queramos resolver. 


```
sage: var('x,y')
(x, y)
sage: solve([x + y == 2, x - y == 0], x, y)  # Un sistema lineal
[[x == 1, y == 1]]
sage: solve(x^2 + y == 2, y)  # Despejamos una letra
[y == 2 - x^2]
sage: solve([x + y == 2, 2*x + 2*y == 4], x, y)  # Indeterminado
[[x == 2 - r1, y == r1]]
sage: # Sage denomina r1 al parametro
sage: solve([x^2 + y^2 == 2, 2*x + 2*y == 3], x, y)  # No lineal
[[x == (3 - sqrt(7))/4, y == (sqrt(7) + 3)/4],
[x == (sqrt(7) + 3)/4, y == (3 - sqrt(7))/4]]
```


## Manipulación de ecuaciones

Cuando trabajamos con las ecuaciones en secundaria, el profesor nos indica que sigamos una serie de pasos. El primero sueler ser desarrollar totalmente ambos miembros de la ecuación. Después podemos mover términos de un miembro a otro de la ecuación empleando las reglas "lo que esta sumando pasa restando",...  En realidad lo que hacemos con esa regla es sumar (o restar) la misma cantidad a ambos miembros de la ecuación.  Lo mismo sucede con las otras reglas.

Cuando aplicamos el método de reducción (o de Gauss) decimos que sumamos dos ecuaciones y lo que hacemos es sumar las parte izquierda y derecha por separado.  Aunque no es muy habitual, también podemos multiplicar dos ecuaciones, multiplicando por separado las partes izquierda y derecha. Todo es lo hace Sage de un modo muy visual.  También es habitual "elevar al cuadrado" ambos miembros de una ecuación.  Esto no lo hace el programa directamente, pero podemos hacerlo multiplicando la ecuación por si misma.



```
sage: # Aunque no es necesario damos nombre a las ecuaciones
sage: f = (x^2 + 2 == x^3 + x)
sage: g = (3*x^2 - 5*x == 8)
sage: # Restamos 2 a la ecuacion f
sage: f - 2
x^2 == x^3 + x - 2
sage: # Tambien podriamos haber restado x^3 + x
sage: f - (x^3 + x)
-x^3 + x^2 - x + 2 == 0
sage: # Multiplicamos la ecuacion g por 9
sage: 9 * g
9*(3*x^2 - 5*x) == 72
sage: g / 88   # Ahora la dividimos por 88
(3*x^2 - 5*x)/88 == 1/11
sage: f + g  # "Sumamos" las dos ecuaciones
4*x^2 - 5*x + 2 == x^3 + x + 8
sage: f * g  # "Multiplicamos" las dos ecuaciones
(x^2 + 2)*(3*x^2 - 5*x) == 8*(x^3 + x)
sage: f * f  # Elevamos al cuadrado ambos miembros
(x^2 + 2)^2 == (x^3 + x)^2
```




Para desarrollar (expandir) ambos miembros de una ecuación se emplea el método

`.expand()`

Si queremos quedarnos con el primer miembro de la ecuacion

`.left()`

y para la parte derecha

`.right()`

Para sustituir la variable por un valor particular 

`.subs(x = valor)`


```
sage: f = (x-3)^2 + (3*x-5)/89 == (2*x - 5)*(x-2)
sage: f
(3*x - 5)/89 + (x - 3)^2 == (x - 2)*(2*x - 5)
sage: f.expand()
x^2 - 531*x/89 + 796/89 == 2*x^2 - 9*x + 10
sage: f.left()
(3*x - 5)/89 + (x - 3)^2
sage: f.right()
(x - 2)*(2*x - 5)
sage: # Sustituimos x por el valor 7
sage: f.subs(x=7)
1440/89 == 45
```

## Resolución numérica

Cuando empezamos a estudiar las ecuaciones parece que todas tienen solución exacta y que basta seguir un método (más o menos complicado) para conseguir resolver de modo exacto la ecuación.  Ello es debido, o bien a que las ecuaciones son muy sencillas, o bien a que están "preparadas" para que funcionen los métodos. Pero la realidad nos dice que en general una ecuación no es resoluble de modo exacto. Si queremos conseguir soluciones debemos recurrir a métodos númericos que nos porporcionarán las soluciones con el grado de aproximación deseado.  Debido a los algoritmos empleados para los métodos númericos,  debemos especificar el intervalo donde el algoritmo debe "buscar" la solución.  Normalmente aunque haya varias soluciones en el intervalo dado, el algoritmo nos "encontrará" solamente una.  En el caso de ecuaciones polinómicas hay métodos que nos permiten encontrar todas las soluciones, pero eso lo veremos en una sección posterior.

El método para encontrar la solución númerica de una ecuación es

`.find_root(intervalo)`


```
sage: f = (x^2 -5*x + 6 == 0)
sage: # Encontrar solucion en el intervalo (-20,5)
sage: f.find_root(-20,5)
3.0
sage: f.find_root(0,2.5)
2.0
sage: g = (sin(x^2)+3 == exp(x)) # Una ecuacion mas complicada
sage: g.find_root(-10,10)
1.3737974476331929
sage: g.subs(x=_)
3.950323394682415 == 3.950323394682415
sage: # La variable _ guarda siempre el ultimo valor calculado
sage: # El problema de este metodo es fijar un intervalo adecuado
sage: h = x^6 - 5*x^4 - 3*x^2 + 5*x - 12   # Una de sexto grado
sage: h.find_root(-10,20)
2.3554150099757294
```

