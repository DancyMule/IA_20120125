# 100 prisioneros (Prueba de rendimiento):

### 100 cajas con número, cada prisionero tiene un número. Si todos encuentran su número ganan, Sí uno falla todos mueren, cada uno puede abrir 50 cajas.


Tenemos 100 presos que pueden abrir la mitad de 100 cajas para encontrar su número; por lo que cada uno tiene 1/2 de posibilidad de encontrar la caja con su número si busca al azar pero si cada prisionero tiene una probabilidad de 50%, tenemos este resultado de tener éxito:

$$
(1/2)^{100} = 7.888609×10^{−31}
$$
       
Es un probabilidad exageradamente baja, por lo que abrir al azar no es buena estrategia.
En cambio si establecemos una estrategia de busqueda por variable obtenida (número que está en cada caja) es mucho más probable que lo encotremos


Con la siguiente estrategia, la probabilidad de que todos logren salir es del 31%


## Aleatoriedad

Al aleatorizar el número contenido en cada caja, estamos hablando de de una colocación específica la cual podemos calcular como permutaciones con algo bastante simple:



$$
100! = 9.332622e+157
$$


Por lo que podemos asumir que a fines prácticos es imposible que se haya creado un ciclo de 100 cajas.  

Por ciclo entendamos algo así; partiendo de que cada caja tiene su posición, esto se aleatoriza:

>
[1] [2] [3] [4] [5] [6] [7] [8] 
>

Creando así un escenario donde unas se relacionan con otras a manera de fila o camino.

>
[8] [4] [5] [2] [3] [1] [6] [7] 
>

Si prestamos atención, podemos ver que 2 y 4 solo han intercambiado posiciones (Ciclo de 2 elementos) al igual que 3 y 5 (Ciclo de 2 elementos). De otra forma un poco más compleja; 8, 7, 6 y 1 están relacionandos,  La caja 1 tiene el número 8, la caja 8 tiene el número 7, la caja número 7 tiene el 6 y la caja número 6 tiene el número 1.  dando como resultado en este ejemplo 2 ciclos de 2 y uno de 4. Con esto podemos concluir que si vamos a la caja en la posición del número que nos ha salido, eventualmente encontraremos nuestro número.    

Ahora recordemos que tenemos 50 intentos para abrir la caja correcta, por lo que debemos calcular la probabilidad de que exista un ciclo de 51 o más cajas.


Primero debemos entender que el número total de ciclos únicos de 100 es son:
$$
ciclos = 100!/100
$$

$$
P(100) = (ciclos)/(permutaciones)
$$
$$
P(100) = (100!/100)/(100!)
$$
$$
P(100) = (1/100)
$$

Esto representa que hay un 1% de que una disposición aleatoria de números y cajas resulte en un ciclo 100, es un resultado general replicable:

$$
P(99) = 1/99%
$$
$$
P(98) = 1/98%
$$
$$
P(97) = 1/97%
$$
$$
P(96) = 1/96%
$$
$$
...
$$

Así que la probabilidad de que haya un ciclo de más de 50 cajas es la suma de la probabilidad de todos los resultados replicables:

$$
P(+50) = {1/50}+{1/51}+{1/52}+{1/53}+...+{1/100}
$$
$$
P(+50) = .69
$$
Lo cual nos da como resultado que el 31.18% de las veces habrá un bucle menor de 50.

A primera vista quizás es un poco raro pero hay que recordar que la probabilidad del largo de los ciclos es una probabilidad independiente que estamos comparando contra la probabilidad de que todos aleatoriamente encuntren su número:

$$
(1/2)^{100} < .31
$$

### ¿Cómo inciamos esta estrategia?

Fácil, debes comenzar por tu número de prisionero y eventualmente llegarás a encontrarlo, sigues el número que aparezca en tu caja y evetualmente llegarás al principio del bucle. Es 100% seguro que si inicias por tu número, el bucle eventualmente terminará donde comenzó.

Para obtener la probabilidad global de éxito, primero debemos suponer un muestreo más amplio:
Sabemos que la probabilida de éxito con 100 prisioneros es: 

$$
P(exito) = 1-({1/50}+{1/51}+...+{1/100}+
$$

Por lo que podemos replicarla aumentando el número dr prisioneros, obteniendo un resultado más fiable:


$$
P(exito) = 1-({1/50}+{1/51}+...+{1/1000}) = 30.74 
$$
<br>
$$
P(exito) = 1-({1/50}+{1/51}+...+{1/1000000}) = 30.68533
$$
<br>
$$
P(exito) = 1-({1/50}+{1/51}+...+{1/1000000000}) = 30.68528
$$

Como se puede apreciar, mietras mas prisioneros agregamos, más nos acercamos a la tasa de éxito real. Su aproximación es:

$$
P(exito) = 1 - ln(2) = .30685
$$

Esto demuestra que si importar cuantos prisioneros sean, simepre hay al menos un 30% de probabilidad de todos se salven.

