Capítulo 1
============================




```{epigraph}
**"Ser ignorante no es tan penoso como no estar dispuesto a aprender."**<br />

 -- Benjamin Franklin

```



## Introducción

  

Las redes tensoriales son un concepto relativamente joven que tiene su origen en la notación gráfica de Penrose, la cual comenzó a aplicarse en problemas prácticos en 1971 {cite}`penrose_1971`. Sin embargo, permaneció fuera de los temas populares en ciencia hasta que Deutsch uso la notación diagramática proveniente de ella, para formular lo que llamó redes computacionales cuánticas que se conocen hoy en día como circuitos cuánticos {cite}`D1989`. ¡Así es! Los circuitos que generas en [Qiskit](qiskit.org) son un caso especial de redes tensoriales. Un dato curioso es que mientras que el modelo que se popularizó es el de Deustsch, Feynman había formulado otro modelo diagramático relacionado con este un poco antes {cite}`Feynman1986`. Sin embargo, incluso después de ello no se habló mucho de redes tensoriales.

El interés resurge gracias al uso de la teoría de redes tensoriales para cálculos numéricos con el nacimiento de algoritmos como MPS, TT, TTN, MERA y PEPS. Varios de ellos tienen su origen en materia condensada y se pensaron para resolver problemas en mecánica cuántica, un poco inspirados en el grupo de renormalización de la matriz densidad. Sin embargo, estas técnicas numéricas también han encontrado aplicaciones en machine learning, debido a que, al igual que en materia condensada,  los problemas numéricos son de alta dimensión, incluso llevando a empresas como Google a invertir en la investigación de estos temas y el desarrollo de librerías para el cálculo numérico de estas técnicas como [TensorNetwork](https://github.com/google/TensorNetwork).

Una red tensorial es un arreglo contable de tensores conectados por contracciones (suma de índices) entre ellos, y no solo han ayudado en la comprensión teórica de las funciones de onda cuántica, sino que también forman la base de muchos potentes enfoques de simulación numérica. Entonces... ¿de qué trata este libro? El escrito a continuación trata de lo que se suelen llamar “métodos de redes tensoriales” que es el conjunto de herramientas asociadas al cálculo y simplificación de redes tensoriales. Estos métodos además de ser usados en las areas ya mencionadas de física de la materia condensada {cite}`1308.3318` y machine learning {cite}`NIPS2015_6855456e`, también están presentes en información cuántica {cite}`PhysRevLett.125.190402`, open quantum systems {cite}`PhysRevLett.121.227401`, gravedad cuántica {cite}`Chirco_2018` y muchas otras áreas de la ciencia.

En este capítulo, introduciremos algunos de los conceptos básicos que son necesarios para entender lo que es una red tensorial, y como las técnicas de esta teoría pueden ser usadas en cálculos numéricos. Comenzaremos por lo primero que debemos entender, que es un tensor. 





## ¿Que es un Tensor?



Un tensor es un arreglo multidimensional de números complejos, este concepto matemático generaliza la idea de mapas multilineales, es decir funciones de muchos parámetros que son lineales con respecto a cada uno de ellos. Ahora bien, si después de esta definición un tanto formal no te ha quedado claro, intentemos con una versión un poco más intuitiva.



Un tensor es una serie de números denotados con $N$ índices, donde $N$ es lo que llamamos el orden del tensor. Todos hemos tratado con tensores en nuestra vida, por ejemplo:



* $5$ es un tensor de rango $0$, es una serie de numeros para la cual no necesitamos indices para referirnos a un elemento, pasa lo mismo para cualquier escalar



* El vector $\vec{T}=\begin{pmatrix} 0  \\ 1 \end{pmatrix}$, es un tensor de rango $1$, necesitamos un indice para referirnos a alguno de sus elementos (i.e $i=1,2$)



```{image} images/vector.png
:alt: vector
:width: 200px
:align: center
```



* Una matriz $\vec{T}=\begin{pmatrix} 0 & 0 \\1 & 1 \end{pmatrix}$ es un tensor de rango $2$, necesitamos 2 índices para identificar inequivocamente cada uno de sus elementos



```{image} images/matriz.png
:alt: matriz
:width: 200px
:align: center
```



* Un tensor de rango 3 es un objeto matemático con el que la mayoría de las personas están menos familiarizadas, pero imaginemos unos números ordenados en un cubo de rubik, para referirnos a ellos se necesitarían 3 índices.



```{image} images/tensor.png
:alt: Tensor de rango 3
:width: 200px
:align: center
```


|      Nombre     |             Ejemplo             | Rango |              Notacion             |
|:---------------:|:-------------------------------:|:-----:|:---------------------------------:|
|     Escalar     |            Constante            |   0   |             $\lambda$             |
|      Vector     |         Funcion de onda         |   1   |             $\psi_{i}$            |
|      Matriz     |             Operador            |   2   |             $A_{i.j}$             |
|       : :       |               : :               |  : :  |                : :                |
| Tensor Rango-N  | Funcion de onda para N-cuerpos  |   N   | $\psi_{\alpha_{1},...\alpha_{N}}$ |










La manera en la que se suele tratar con los tensores en física y matemática, es mediante lo que se conoce como notación indicial y el convenio de suma de Einstein, las cuales introduciremos en la próxima sección 

<<<<<<< HEAD
### Representacion gráfica de redes tensoriales  
=======
## Representación gráfica de redes tensoriales  
>>>>>>> ab3240fc3d5a0e570b63d074e8d67108b12d434e

Utilizamos una notación de diagramas para los tensores, donde cada tensor es dibujado como una figura sólida con un número de conexiones correspondientes a su orden. A continuación mostramos unos ejemplos:

**El Vector** $A_{i}$ se representa como:

```{image} images/vector_tensor.png
:alt: Un vector en su representación gráfica
:width: 140px
:align: center
```

**La matriz** $B_{ij}$ se representa como:

```{image} images/matrix_tensor.png
:alt: Una matríz en su representación gráfica
:width: 140px
:align: center
```

**Un tensor de orden 3** $C_{ijk}$ se representa como:

```{image} images/tensor_orden_3.png
:alt: Un tensor de orden 3 en su representación gráfica
:width: 140px
:align: center
```

Podemos formar redes de múltiples tensores donde un índice compartido por dos tensores denota una suma o contracción sobre el índice.

```{image} images/contraccion.png
:alt: Representación gráfica de una contracción entre dos tensores 
:width: 270px
:align: center
```

La multiplicación de dos matrices $A$ y $B$ se puede detonar en forma de suma como:
$C_{ik} = \sum_{j}A_{ij}B_{jk} $. Su equivalente en diagramas de tensores es:


```{image} images/ejemplo1.png
:alt: Representación gráfica de una contracción entre dos tensores, denotando una multiplicación de matrices 
:width: 270px
:align: center
```
De similar manera, podemos pensar en cómo representar otras operaciones comunes con matrices, como el producto interno. Si ahora consideramos dos vectores $A$ y $B$, su producto interno $\sum_{i}A_{i}\cdot B_{i}$ va a tener un equivalente en diagramas de tensores de forma:

```{image} images/prod_interno1.png
:alt: Representación gráfica de un producto interno con vectores
:width: 270px
:align: center
```

Con dos tensores $D$ y $E$ de rango 4 por ejemplo su producto interno $\sum_{ijkl}D_{ijkl}\cdot E_{ijkl}$ es equivalente al diagrama:

```{image} images/prod_interno2.png
:alt: Representación gráfica de un producto interno con tensores rango-4
:width: 270px
:align: center
```

Finalmente, también podemos representar la traza de un arreglo. Considerando una matriz $A$, su traza $\sum_{i}A_{ii}$ tiene un diagrama equivalente con la forma:

```{image} images/traza.png
:alt: Representación gráfica de la traza de una matriz
:width: 270px
:align: center
```

Podemos ver el poder de la notación con diagramas de tensores en la contracción de 3 tensores, donde en notación de sumatoria se expresa como: 
$D_{ijk} = \sum_{lmn}A_{ljm}B_{iln}C_{nmk} $. En cambio la notacion con diagramas es mucho mas sencilla de interpretar:

```{image} images/ejemplo2.png
:alt: Representación gráfica de una contracción entre tres tensores
:width: 270px
:align: center
```

En muchas aplicaciones, la meta es aproximar un tensor de alto orden, como el siguiente:

```{image} images/tensor_N.png
:alt: Tensor de orden N
:width: 270px
:align: center
```
A un red tensorial compuesta de muchos tensores de bajo orden:

```{image} images/tensor_N_2.png
:alt: Red tensorial de N tensores de bajo orden
:width: 270px
:align: center
```

La representación de redes tensoriales es muy útil por varias razones:

1. Pueden ofrecer una representación más comprimida de estructuras de datos grandes
2. Permite una caracterización de la estructura de los datos. Además de que la notación de diagramas permite una intuición visual más clara. 
3. Ya que la descomposición de los datos es robusta, se pueden utilizar para trabajar con ruido o con datos faltantes.
4. Permite tener un Framework unificado para manipular grandes datos. Como evaluar información estadística utilizando una colección pequeña de redes tensoriales sin la necesitada de tener conocimiento específico sobre la estructura de todo el sistema o de lo que representa.


La **teoría de redes tensoriales** se enfoca en entender como esta representación funciona y en que situaciones es más óptima utilizarla. En cambio, los **algoritmos de redes tensoriales** se enfocan en métodos para obtener , manipular y extraer información de estas representaciones.

## Redes tensoriales para sistemas cuánticos de muchos cuerpos

### Redes Tensoriales MPS

También conocidas como Tensor Train (TT), las redes tensoriales **Matrix Product State** son una manera de representar un tensor de rango-$N$ como una cadena de tensores de rango-$3$. Esta representación es el objetivo de muchos algoritmos debido a que provee una buena aproximación de las redes tensoriales y es más eficiente computacionalmente.

```{image} images/MPS.png
:alt: Ejecución de MPS como un recorte del Tensor objetivo en tensores más pequeños
:width: 470px
:align: center
```

Viendo está imagen puede ser difícil imaginarnos de donde viene dicha eficiencia, pero vayamos más a fondo. Recordemos que el estado cuántico $|\psi\rangle=\sum_{ij\dots k}\psi_{ij\dots k}|u_{ij\dots k}\rangle$ requiere de $2^n$ coeficientes independientes para ser descrito completamente, es decir, crece exponencialmente con el tamaño de n. Con MPS queremos poder describir al estado cómo:

$$|\psi\rangle = \sum_{ij\dots k}Tr(A_{i}^{[1]}A_{j}^{[2]}\dots A_{k}^{[n]})|u_{ijkl}\rangle$$

De esta manera los recursos necesarios para describirlo solo crecen linealmente con un factor de escalamiento de $nd\chi^{2}$ donde $d$ es la dimensión del subsistema y $\chi$ por $\chi$ es el limite superior de las matrices. Podemos observar en esta ecuación que conocer los componentes de $\psi$ solo es cosa de calcular el producto entre las matrices. Es de aquí de donde viene el nombre del método.

La capacidad de MPS de reducir los recursos viene de una herramienta llamada **Singular Value Decomposition** que se aplica de manera recursiva al tensor inicial $T$ para obtener las matrices que cumplen $T=U\Sigma V$, donde $U$ y $V$ son unitarios y $\Sigma$ es real, diagonal y no-negativa. 

Si volvemos al ejemplo de la imagen, podemos notar que los tensores de los extremos solo tienen dos conexiones, por lo que el producto va a dar como resultado un escalar entonces la traza no es necesaria. Y su ecuación tendria la forma:
$$|\psi\rangle = \sum_{ijkl}A_{i}^{[1]}A_{j}^{[2]}A_{k}^{[3]}A_{l}^{[4]}|u_{ij\dots k}\rangle$$



```{bibliography}
```
<!--- 



## Notacion indicial y convenio de la suma de Einstein 


Esta introduccion a la notacion indicial y la suma de einstein sera un poco distinta de la usual ya que estaremos revisando tres formas de ver estas manipulaciones, una que llamaremos algebraica (usada en relatividad), la diagramatica que aun no tiene estandar pero que llamaremos de Penrose siguiendo [articulo de biamonte] y la de dirac (usualmente usada en cuantica). Esto nos dara una perspectiva mas amplia al poder apreciar las ventajas y desventajas de cada una de ellas, y resaltar lo intuitiva que es la notacion grafica.









