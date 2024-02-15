# Flavio Josefo (Secuencia de percepción) en sentido del reloj

40 Solados en circulo, matan al de la derecha hasta que quede uno ¿Dónde se debe de colocar Josefo para sobrevivir?


La sintesis del problema es: Si hay un circulo n personas, y cada uno mata al del sentido horario ¿Donde se debe colocar el superviviente?


### Muestreo de 10

#### Nivel 1
 >>>
                                            1
                                         10   2   
                                        9       3
                                        8       4
                                          7   5        
                                            6   
 >>>

 1 -> 2    
 3 -> 4     
 5 -> 6     
 7 -> 8     
 9 -> 10     
#### Nivel 2
 >>>
                                            1
                                        9       3
                                          7   5                
 >>>
 1 -> 3    
 5 -> 7     
 9 -> 1      
#### Nivel 3
 >>>                                       
                                        9   5                                                             
 >>>
 5 -> 9    
     
### Explicación

En este caso podemos localizar que el superviviente en la situación par de 10 elementos es el que se encuentra en la posición:

$$
LugarSuperviviente = n/2 = 10/2 = 5     
$$

### Muestreo de 11

#### Nivel 1
 >>>
                                           11 1
                                         10     2   
                                        9         3
                                        8         4
                                          7     5        
                                             6   
 >>>

 1 -> 2    
 3 -> 4     
 5 -> 6     
 7 -> 8     
 9 -> 10     
 11 -> 1     
#### Nivel 2
 >>>
                                            11
                                                 
                                        9         3
                                                 
                                          7     5        
                                                
 >>>
 3 -> 5    
 7 -> 9     
 11 -> 3      
#### Nivel 3
 >>>                                                                                    
                                        11       7                                            
 >>>
 7 -> 11    
     
### Explicación

En este caso podemos localizar que el superviviente en la situación impar de 11 elementos es el que se encuentra en la posición: 7.
Pero a diferencia del caso anterior la formula que habiamos no cuadra. 

$$
LugarSuperviviente = n/2 = 11/2 = 5.5     
$$



## Tabla de muestreos

| Cantidad de soldados | Lugar del superviviente |
|--------------|--------------|
| 1     | 1     | 
| 2     | 1  | 
| 3     | 3  | 
| 4     | 1   | 
| 5     | 3   | 
| 6     | 5   | 
| 7     | 7  | 
| 8     | 1   | 
| 9     | 3  | 
| 10    | 5    | 
| 11    | 7    | 
| 12    | 9    | 
| 13    | 11    | 
| 14    | 13    | 
| 15    | 15   | 
| 16    | 1    |
| 17    | 3    |

En la tabla se describen los muestreos con n cantidad de soldados y en que lugar se salvaran. Con esta tabla podemos comenzar a encontrar relaciones;




1. Todos los lugares seguros son **impares**
2. Los números 1, 3, 5 y 7 se repiten constanteemente
3. Todos los elementos que su continua división entre 2 den un producto de 1; Toda potencia de 2 tiene un lugar seguro.

Lo que podemos localizar y trabajar con ellos es con las potencias de 2; localizaremos la potencia de 2 más cercana y crearemos un escenario para alcanzar el número de soldados. En este caso haremos un caliz con 26 soldados.

$$
(2^4)+10 = 26 
$$

En este caso lo que nos importa es el 10, que llamaremos **y**

Si aplicamos esta regla a los casos que ya conocemos tenemos lo siguiente:


| Cantidad de soldados | Lugar del superviviente |$$ y = CantidadSoldados-(2^x) $$|
|--------------|--------------|-|
| 1     | 1     | 0 |
| 2     | 1  | 0 |
| 3     | 3  | 1|
| 4     | 1   | 0|
| 5     | 3   | 1|
| 6     | 5   | 2|
| 7     | 7  | 3|
| 8     | 1   | 0|
| 9     | 3  |1 |
| 10    | 5    | 2|
| 11    | 7    | 3|
| 12    | 9    | 4|
| 13    | 11    | 5|
| 14    | 13    | 6|
| 15    | 15   | 7|
| 16    | 1    |0|
| 17    | 3    |1|


Podemos notar una correlación no muy evidente, la variable **y** nos puede servir para llegar al lugar seguro.

$$
LugarSuperviviente = (2*y) + 1
$$

### Pruebas

Volveremos a revisar las dos pruebas que realizamos al comienzo con 10 y 11 soldados, pero ahora aplicaremos la fórmula.
## Muestreo de 10

#### Formula
Aplicamos la fórmula inicial y desarrollamos:

$$
y = ((2^3) - 10) 
$$

$$
y = 4 
$$

$$
LugarSuperviviente = 2(y)+1
$$

Podemos ver que coincide perfectamente con el lugar seguro que habíamos obtenido manualmente.

$$
LugarSuperviviente = 5
$$

#### Nivel 1
 >>>
                                            1
                                         10   2   
                                        9       3
                                        8       4
                                          7   5        
                                            6   
 >>>

 1 -> 2    
 3 -> 4     
 5 -> 6     
 7 -> 8     
 9 -> 10     
#### Nivel 2
 >>>
                                            1
                                        9       3
                                          7   5                
 >>>
 1 -> 3    
 5 -> 7     
 9 -> 1      
#### Nivel 3
 >>>                                       
                                        9   5                                                             
 >>>
 5 -> 9    
     

### Muestreo de 11

#### Formula
Aplicamos la fórmula y desarrollamos:

$$
y = (11 - (2^3))
$$

$$
y = 3
$$

$$
LugarSuperviviente = 2(y) + 1
$$

Como podemos ver, ha coincidido con lo que hicimos manualmente.     

$$
LugarSuperviviente = 6+1 = 7
$$

#### Nivel 1
 >>>
                                           11 1
                                         10     2   
                                        9         3
                                        8         4
                                          7     5        
                                             6   
 >>>

 1 -> 2    
 3 -> 4     
 5 -> 6     
 7 -> 8     
 9 -> 10     
 11 -> 1     
#### Nivel 2
 >>>
                                            11
                                                 
                                        9         3
                                                 
                                          7     5        
                                                
 >>>
 3 -> 5    
 7 -> 9     
 11 -> 3      
#### Nivel 3
 >>>                                                                                    
                                        11       7                                            
 >>>
 7 -> 11    
     
## Solución a 40

$$
y = (40 - (2^5))
$$
$$
y = 8
$$

$$
LugarSuperviviente = 2(8) + 1
$$

$$
LugarSuperviviente = 17
$$



#### Nivel 1
 >>>
           1  2  3  4  5  6  7  8
          40                        9
                                     
         39                          10
                                     
        38                            11
                                     
       37                              12
                                     
      36                                13
                                     
     35                                  14
                                     
    34                                    15
                                     
    33                                    16
                                     
     32                                  17
                                     
      31                                18
                                     
       30                              19
                                     
        29                            20
                                     
         28                          21
                                     
          27                        22
                                     
                26  25  24  23

 >>>

   1 ->  2  
   3 ->  4  
   5 ->  6  
   7 ->  8  
   9 -> 10  
  11 -> 12  
  13 -> 14  
  15 -> 16  
  17 -> 18  
  19 -> 20  
  21 -> 22  
  23 -> 24  
  25 -> 26  
  27 -> 28  
  29 -> 30  
  31 -> 32  
  33 -> 34  
  35 -> 36  
  37 -> 38  
  39 -> 40  

#### Nivel 2
 >>>
             1    3    5    7  
                                  9                                
         39                                                       
                                    11                                   
       37                                                                 
                                      13                                 
     35                                                                     
                                        15                                  
    33                                                                      
                                       17                                    
      31                                                                  
                                     19                                  
        29                                                                
                                  21                                    
          27                                                            
                  25    23

 >>>

1 ->  3  
5 ->  7  
9 ->  11  
13 ->  15  
17 -> 19  
21 -> 23  
25 -> 27  
29 -> 31  
33 -> 35  
37 -> 39  
#### Nivel 3
 >>>
             1        5      
                                  9                                                                       
       37                                                                  
                                      13                                   
    33                                                                       
                                       17                              
        29                                                            
                                   21                                 
                  25    

 >>>

1 ->  5  
9 ->  13     
17 ->  21    
25 ->  29  
33 -> 37  
#### Nivel 4
 >>>
             1             
       33           9                                                                                                                                        
          25     17                                                                                               
     

 >>>

1 ->  9  
17 ->  25   
33 ->  1  
#### Nivel 5
 >>>
                          
       33                                                                                                                                                  
               17                                                                                               
     

 >>>

17 ->  33 

  
#### solución
Como se ha visto, ambos resultados; manual y con calculo han coincidido,

## Solución a 41

$$
y = (41 - (2^5))
$$

$$
y = 9
$$

$$
LugarSuperviviente = 2(9) + 1
$$

$$
LugarSuperviviente = 19
$$



#### Nivel 1
 >>>
           41 1  2  3  4  5  6  7  8
          40                        9
                                     
         39                          10
                                     
        38                            11
                                     
       37                              12
                                     
      36                                13
                                     
     35                                  14
                                     
    34                                    15
                                     
    33                                    16
                                     
     32                                  17
                                     
      31                                18
                                     
       30                              19
                                     
        29                            20
                                     
         28                          21
                                     
          27                        22
                                     
                26  25  24  23

 >>>

   1 ->  2  
   3 ->  4  
   5 ->  6  
   7 ->  8  
   9 -> 10  
  11 -> 12  
  13 -> 14  
  15 -> 16  
  17 -> 18  
  19 -> 20  
  21 -> 22  
  23 -> 24  
  25 -> 26  
  27 -> 28  
  29 -> 30  
  31 -> 32  
  33 -> 34  
  35 -> 36  
  37 -> 38  
  39 -> 40  
  41 -> 1  

#### Nivel 2
 >>>
             41    3    5    7  
                                  9                                
         39                                                       
                                    11                                   
       37                                                                 
                                      13                                 
     35                                                                     
                                        15                                  
    33                                                                      
                                       17                                    
      31                                                                  
                                     19                                  
        29                                                                
                                  21                                    
          27                                                            
                  25    23

 >>>

3 ->  5  
7 ->  9  
11 -> 13  
15 -> 17  
19 -> 21  
23 -> 25  
27 -> 29  
31 -> 33  
35 -> 37  
39 -> 41  
#### Nivel 3
 >>>
          39  3   7                                                                                                                         
                     11                                                                                                
     35                                                                     
                      15                                                                                                          
      31                                                                  
                    19                                                                                              
          27    23

 >>>

3 ->  7  
11 ->  15  
19 ->  23  
27 -> 31  
35 -> 39  

#### Nivel 4
 >>>
          3                                                                                                                                                                         
      35        11                                                                                                          
        27    19

 >>>

3 ->  11    
19 ->  27  
35 ->  3 
#### Nivel 5
 >>>
            19                                                                                                          
      35                                                                                                                                                                   

 >>>

19 -> 35 


#### solución
Como se ha visto, ambos resultados; manual y con calculo han coincidido,


# Flavio Josefo (Secuencia de percepción) en contra del sentido del reloj

Ahora que ya entendemos como funciona, intentaremos ir de forma más reducida para hacerlo en contra del sentido del reloj 

## Muestreo de 10

#### Nivel 1
 >>>
                                            1
                                         10   2   
                                        9       3
                                        8       4
                                          7   5        
                                            6   
 >>>

 10 -> 9    
 8 -> 7     
 6 -> 5     
 4 -> 3     
 2 -> 1     
#### Nivel 2
 >>>
                                            10
                                        8       2
                                          6   4                
 >>>
 10 -> 8    
 6 -> 4     
 2 -> 10      
#### Nivel 3
 >>>                                       
                                        6   2                                                             
 >>>
 6 -> 2    
     
#### interpretacion
Como podemos ver, ahora el resultado es 6

### Muestreo de 11

#### Nivel 1
 >>>
                                           11 1
                                         10     2   
                                        9         3
                                        8         4
                                          7     5        
                                             6   
 >>>

 11 -> 10    
 9 -> 8     
 7 -> 6     
 5 -> 4     
 3 -> 2     
 1 -> 11     
#### Nivel 2
 >>>
                                             1
                                  
                                        9         3
                                                 
                                          7     5       
                                                
 >>>
 9 -> 7    
 8 -> 3    
 1 -> 9     
#### Nivel 3
 >>>                                                                                    
                                        8       1                                           
 >>>
 8 -> 1  

#### interpretacion
Como podemos ver, ahora el resultado es 8


### Posible fórmula

$$
y = (11 - (2^3))
$$

$$
y = 3
$$

$$
LugarSuperviviente = (2*y) + 2
$$

Siguiendo esta suposición; ambas pruebas anteriorres cobran sentido y coherencia:

#### 10
$$
y = (10 - (2^3))
$$

$$
y = 2
$$

$$
LugarSuperviviente = CantidadSoldados - ((2*y) + 2)
$$

$$
LugarSuperviviente = 6
$$

#### 11
$$
y = (11 - (2^3))
$$

$$
y = 3
$$

$$
LugarSuperviviente = (2*y) + 2
$$

$$
LugarSuperviviente = 8
$$


Ahora haremos las pruebas de los dos ejemplos deseados, 40 y 41 en contra del sentido del reloj.


#### Nivel 1
 >>>
           1  2  3  4  5  6  7  8
          40                        9
                                     
         39                          10
                                     
        38                            11
                                     
       37                              12
                                     
      36                                13
                                     
     35                                  14
                                     
    34                                    15
                                     
    33                                    16
                                     
     32                                  17
                                     
      31                                18
                                     
       30                              19
                                     
        29                            20
                                     
         28                          21
                                     
          27                        22
                                     
                26  25  24  23

 >>>

40 -> 39  
38 -> 37  
36 -> 35  
34 -> 33  
32 -> 31  
30 -> 29  
28 -> 27  
26 -> 25  
24 -> 23  
22 -> 21  
20 -> 19  
18 -> 17  
16 -> 15  
14 -> 13  
12 -> 11  
10 -> 9  
 8 -> 7  
 6 -> 5  
 4 -> 3  
 2 -> 1  

#### Nivel 2
 >>>
             2    4    6    8
          40                                                
                                   10                               
        38                                                               
                                     12                                    
      36                                                                  
                                       14                                     
    34                                                                        
                                        16                                    
     32                                                                      
                                      18                                    
       30                                                                  
                                    20                                   
         28                                                              
                                  22                                   
                26       24  

 >>>

40 -> 38  
36 -> 34  
32 -> 30  
28 -> 26  
24 -> 22  
20 -> 18  
16 -> 14  
12 -> 10  
8 -> 6  
4 -> 2

#### Nivel 3
 >>>
                 4    8
          40                                                                                                   
                          12                                    
      36                                                                                                                                                                                                         
                            16                                    
     32                                                                                                                                                                                                              
         28               20                                   
                 24  

 >>>

40 -> 36  
32 -> 28  
24 -> 20  
16 -> 12  
8  -> 4  

#### Nivel 4
 >>>
                8
          40        16                                    
            32   24  

 >>>

40 -> 32  
24 -> 16  
8 -> 40  

#### Nivel 4
 >>>
                8
                                     
                 24  

 >>>

24 -> 8 