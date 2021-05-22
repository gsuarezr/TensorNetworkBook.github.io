Capítulo 1
============================



**"Ser ignorante no es tan penoso como no estar dispuesto a aprender."**<br />

― Benjamin Franklin





## Introducción



Una red tensorial es un arreglo contable de tensores conectados por contracciones (suma de índices) entre ellos, y no solo han ayudado en la comprensión teórica de las funciones de onda cuántica, sino que también forman la base de muchos potentes enfoques de simulación numérica. Entonces... ¿de qué trata este libro? El escrito a continuación trata de lo que se suele llamar “métodos de redes tensoriales” que es el conjunto de herramientas asociadas al cálculo y simplificación de redes tensoriales. Estos métodos son usados en información cuántica [ref], física de la materia condensada [ref], machine learning [ref], open quantum systems [ref], gravedad cuántica [ref] y muchas otras áreas de la ciencia.



Las redes tensoriales son un concepto relativamente joven que tiene su origen en la notación gráfica de Penrose, la cual comenzó a aplicarse en problemas prácticos en 1971 [Roger Penrose. Applications of negative dimensional tensors]. Sin embargo, permaneció fuera de los temas populares en ciencia hasta que Deutsch uso la notación diagramática proveniente de ella, para formular lo que llamó redes computacionales cuánticas que se conocen hoy en día como circuitos cuánticos[D. Deutsch. Quantum computational networks]. ¡Así es! Los circuitos que generas en Qiskit son un caso especial de redes tensoriales. Un dato curioso es que mientras que el modelo que se popularizó es el de Alemán, Feynman había formulado otro modelo diagramático relacionado con este un poco antes [Foundations of Phys.,16:507, 1986.]. Sin embargo, incluso después de ello no se habló mucho de redes tensoriales.





El interés resurge gracias a el uso de la teoría de redes tensoriales para cálculos numéricos con el nacimiento de algoritmos como MPS, TT , TTN, MERA y PEPS. Varios de ellos tienen su origen en materia condensada y se pensaron para resolver problemas en mecánica cuántica, un poco inspirados en el grupo de renormalización de la matriz densidad. Sin embargo estas técnicas numéricas también han encontrado aplicaciones en machine learning [] . Esto  debido a que al igual que en materia condensada,  los problemas numéricos son de alta dimensión, incluso llevando a empresas como Google a invertir en la investigación de estos temas y el desarrollo de librerías para el cálculo numérico de estas técnicas como [TensorNetwork](https://github.com/google/TensorNetwork).




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
| Tennsor Rango-N | Funcion de onda para N-cuerpos  |   N   | $\psi_{\alpha_{1},...\alpha_{N}}$ |




La manera en la que se suele tratar con los tensores en física y matemática, es mediante lo que se conoce como notación indicial y el convenio de suma de Einstein, las cuales introduciremos en la próxima sección 

<!--- 



## Notacion indicial y convenio de la suma de Einstein 

Esta introduccion a la notacion indicial y la suma de einstein sera un poco distinta de la usual ya que estaremos revisando tres formas de ver estas manipulaciones, una que llamaremos algebraica (usada en relatividad), la diagramatica que aun no tiene estandar pero que llamaremos de Penrose siguiendo [articulo de biamonte] y la de dirac (usualmente usada en cuantica). Esto nos dara una perspectiva mas amplia al poder apreciar las ventajas y desventajas de cada una de ellas, y resaltar lo intuitiva que es la notacion grafica.








