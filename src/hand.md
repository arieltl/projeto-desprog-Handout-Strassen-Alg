Algoritmo de Strassen
======

Soma de matrizes quadradas
---------
Antes de falar de multiplicação de matrizes, precisamos falar um pouco sobre soma de matrizes.
Em particular, sobre a complexidade de somar duas matrizes.

Apesar de você ter aprendido no Ensino Médio, na matéria de Tópicos Essenciais de Matemática e Física e na matéria de Matemática da Variação, não podemos contar com a sua retenção de conhecimento. Sabemos que alguns alunos jogam Magic durante as aulas.

Caso particular de uma matriz 3x3: 
$$
		\begin{bmatrix}
		a & b & c \\
		d & e & f \\
		g & h & i \\
		\end{bmatrix}
		+
		\begin{bmatrix}
		j & k & l \\
		m & n & o \\
		p & q & r \\
		\end{bmatrix}
		=
		\begin{bmatrix}
		(a+j) & (b+k) & (c+l) \\
		(d+m) & (e+n) & (f+o) \\
		(g+p) & (h+q) & (i+r) \\
		\end{bmatrix}
$$
Pode-se generalizar em:
$$
	\begin{bmatrix}
	a_{11} & a_{12} & \cdots & a_{1n} \\
	a_{21} & a_{22} & \cdots & a_{2n} \\
	\vdots & \vdots & \ddots & \vdots \\
	a_{m1} & a_{m2} & \cdots & a_{mn} \\
	\end{bmatrix}
	+
	\begin{bmatrix}
	b_{11} & b_{12} & \cdots & b_{1n} \\
	b_{21} & b_{22} & \cdots & b_{2n} \\
	\vdots & \vdots & \ddots & \vdots \\
	b_{m1} & b_{m2} & \cdots & b_{mn} \\
	\end{bmatrix}
	=
	\begin{bmatrix}
	c_{11} & c_{12} & \cdots & c_{1n} \\
	c_{21} & c_{22} & \cdots & c_{2n} \\
	\vdots & \vdots & \ddots & \vdots \\
	c_{m1} & c_{m2} & \cdots & c_{mn} \\
	\end{bmatrix}
	$$
	$$
	c_{ij} = a_{ij} + b_{ij}
$$

??? Checkpoint
Qual a complexidade da soma de matrizes quadradas?
:::Gabarito
É necessário fazer uma soma para cada elemento da matriz. A matriz tem $n^2$ elementos, então a função é $O(n^2)$
:::

???

Multiplicação de matrizes quadradas
---------
Vamos também fazer uma breve revisão de como se multiplicam matrizes. Primeiro veremos um caso particular de uma matriz 3x3:
$$
		\begin{bmatrix}
		a & b & c \\
		d & e & f \\
		g & h & i \\
		\end{bmatrix}
		\times
		\begin{bmatrix}
		j & k & l \\
		m & n & o \\
		p & q & r \\
		\end{bmatrix}
		=
		\begin{bmatrix}
		r_{11} & r_{12} & r_{13} \\
		r_{21} & r_{22} & r_{23} \\
		r_{31} & r_{32} & r_{33} \\
		\end{bmatrix}
$$
Como temos uma matriz 3x3 o nosso resultado também será uma matriz 3x3! E para determinar cada um dos termos dessa matriz fazemos da seguinte maneira: 
* O elemento $A_{1}$ será definido pela soma da multiplicação de cada elemento da  linha **1** da primeira matriz por cada elemento da coluna **1** da segunda matriz,
* O elemento $A_{2}$ será definido pela soma da multiplicação de cada elemento da  linha **1** da primeira matriz por cada elemento da coluna **2** da segunda matriz,
Dê uma olhada no passo a passo para um melhor entendimento!

:matriz

Explicando o raciocinio em palavras: 
Para calcular o termo que estará na posição da linha 1 e da coluna 1, vamos utilizar a linha 1 da primeira matriz e a coluna 1 da segunda matriz. Com isso
fazemos a multiplicação do primeiro elemento da linha pelo o primeiro elemento da coluna somado com o segundo elemento da linha vezes o segundo elemento da coluna e assim por diante.


Então vamos fazer esse passo a passo para $A_{1}$ , $A_{2}$, ... , $A_{8}$ , $A_{9}$.

Fazendo as contas para todos os termos, vamos chegar em: 

$$
		\begin{bmatrix}
		a & b & c \\
		d & e & f \\
		g & h & i \\
		\end{bmatrix}
		\times
		\begin{bmatrix}
		j & k & l \\
		m & n & o \\
		p & q & r \\
		\end{bmatrix}
		=
		\begin{bmatrix}
		(aj + bm + cp) & (ak + bn + cq) & (al + bo + cr) \\
		(dj + em + fp) & (dk + en + fq) & (dl + eo + fr) \\
		(gj + hm + ip) & (gk + hn + iq) & (gl + ho + ir) \\
		\end{bmatrix}
$$


??? Checkpoint
Agora pegue papel e lápis e faça a multiplicação de uma matriz 2x2?
$$
		\begin{bmatrix}
		a_{11} & a_{12} \\
		a_{21} & a_{22} \\
		\end{bmatrix} \times 
		\begin{bmatrix}
		b_{11} & b_{12} \\
		b_{21} & b_{22} \\
		\end{bmatrix}
		= ?
$$
:::Gabarito
$$
		\begin{bmatrix}
		a_{11} \cdot b_{11} + a_{12} \cdot b_{21} & a_{11} \cdot b_{12} + a_{12} \cdot b_{22} \\
		a_{21} \cdot b_{11} + a_{22} \cdot b_{21} & a_{21} \cdot b_{12} + a_{22} \cdot b_{22} \\
		\end{bmatrix}
$$
:::
???

Pode-se generalizar em:
$$
     \begin{bmatrix}
         a_{11} & a_{12} & \cdots & a_{1n}\\
         a_{21} & a_{22} & \cdots & a_{2n}\\ 
         \vdots & \vdots & \ddots & \vdots\\ 
         a_{m1} & a_{m2} & \cdots & a_{mn} 
     \end{bmatrix}
     \times
     \begin{bmatrix}
         b_{11} & b_{12} & \cdots & b_{1p}\\
         b_{21} & b_{22} & \cdots & b_{2p}\\ 
         \vdots & \vdots & \ddots & \vdots\\ 
         b_{n1} & b_{n2} & \cdots & b_{np} 
     \end{bmatrix}
      =
     \begin{bmatrix}
         c_{11} & c_{12} & \cdots & c_{1p}\\
         c_{21} & c_{22} & \cdots & c_{2p}\\ 
         \vdots & \vdots & \ddots & \vdots\\ 
         c_{m1} & c_{m2} & \cdots & c_{mp} 
     \end{bmatrix}
  $$
  $$ c_{ij}= a_{i1} b_{1j} + a_{i2} b_{2j} +\cdots+ a_{in} + b_{nj} = \sum_{k=1}^n a_{ik}b_{kj} $$  

??? Checkpoint
Qual a complexidade da multiplicação de matrizes quadradas?
:::Gabarito
Há 3 loops de 0 a n aninhados, logo a complexidade função é $O(n^3)$.
:::

???

É evidente que a complexidade de tempo da multiplicação de matrizes é uma ordem de grandeza superior que a da soma. A multiplicação é uma operação bastante importante para diversas áreas da computação, como processamento de imagens e inteligência artificial. Como podemos aproximar a complexidade da multiplicação
à da soma?

!!! Atenção
Note que para realizar a multiplicação de duas matrizes 2x2 são necessárias 4 somas e 8 multiplicações. Mantenha isso em mente por enquanto. Logo será importante.
!!!

Algoritmo Strassen para caso particular matrizes 2x2 
---------

??? Checkpoint
Dada as matrizes:
$$
	A =
	\begin{bmatrix}
	a_{11} & a_{12} \\
	a_{21} & a_{22} \\
	\end{bmatrix}\\
$$
$$
	B=
	\begin{bmatrix}
	b_{11} & b_{12} \\
	b_{21} & b_{22} \\
	\end{bmatrix}
$$
Calcule $(a_{21} + a_{22}) \cdot b_{11} + a_{22} \cdot (b_{21} - b_{11})$ e compare com o resultado da multiplicação feita anteriormente no papel
:::Gabarito
Você deve ter chegado em $a_{21}b_{11}$+$a_{22}b_{21}$ que é $c_{21}$.
:::
???

Volker Strassen descobriu uma forma alternativa para realizar a multiplicação de duas matrizes 2x2. Primeiramente, é necessário calcular 7 produtos a partir dos elementos das matrizes que deseja-se multiplicar. Note que o cálculo do último checkpoint é $P_2+P_4$.
$$
	\begin{aligned}
	P_1 &= (a_{11} + a_{22}) \cdot (b_{11} + b_{22}) \\
	P_2 &= (a_{21} + a_{22}) \cdot b_{11} \\
	P_3 &= a_{11} \cdot (b_{12} - b_{22}) \\
	P_4 &= a_{22} \cdot (b_{21} - b_{11}) \\
	P_5 &= (a_{11} + a_{12}) \cdot b_{22} \\
	P_6 &= (a_{21} - a_{11}) \cdot (b_{11} + b_{12}) \\
	P_7 &= (a_{12} - a_{22}) \cdot (b_{21} + b_{22}) \\
	\end{aligned}
$$

Os outros elementos da matriz $C$ podem ser calculadas usando as expressões abaixo:
$$
	\begin{bmatrix}
	a_{11} & a_{12} \\
	a_{21} & a_{22} \\
	\end{bmatrix}
	\times
	\begin{bmatrix}
	b_{11} & b_{12} \\
	b_{21} & b_{22} \\
	\end{bmatrix}
	=
	\begin{bmatrix}
	P_1 + P_4 - P_5 + P_7 & P_3 + P_5 \\
	P_2 + P_4 & P_1 - P_2 + P_3 + P_6 \\
	\end{bmatrix}
$$

!!!
A demonstração de como ele chegou nesses produtos não entra no escopo da disciplina, caso tenha curiosidade veja este [video](https://www.youtube.com/watch?v=OSelhO6Qnlc.) 
!!!

Sabemos que no método tradicional de uma matriz quadrada de ordem n temos $n^3$ multiplicações. Portanto, para resultar uma matriz 2x2 precisamos de 8 multiplicações. Utilizando esse novo método, precisamos apenas de 7 multiplicações. Por outro lado, temos 18 adições para se realizar, enquanto no método tradicional seriam 4.

Será que a diminuição de apenas uma multiplicação vai compensar tantas adições extras? Nesse caso, em que os elementos da matriz são apenas números realmente
não parece fazer sentido. Porém estamos analisando apenas o caso de n=2, vamos agora tentar generalizar para matrizes com outros n.

Multiplicação com divisão e conquista
---------
O verdadeiro algoritmo de Strassen não funciona apenas para matrizes 2x2. Para entendermos o algoritmo completo, precisamos primeiro ver como podemos quebrar uma multiplicação de matrizes em multiplicações menores.
Por exemplo uma matriz X de ordem 4 pode ser representada como uma matriz 2x2 onde cada elemento é uma matriz 2x2.
$$
X = \begin{bmatrix}
x_{11} & x_{12} & x_{13} & x_{14} \\
x_{21} & x_{22} & x_{23} & x_{24} \\
x_{31} & x_{32} & x_{33} & x_{34} \\
x_{41} & x_{42} & x_{43} & x_{44}
\end{bmatrix}
$$

Se formos dividir a matriz X , que é uma matriz 4x4 em uma composição de matrizes 2x2, podemos ver da seguinte forma: 


$$	
A = \begin{bmatrix}
x_{11} & x_{12} \\
x_{21} & x_{22} \\
\end{bmatrix}\\
B = \begin{bmatrix}
x_{13} & x_{14} \\
x_{23} & x_{24} \\
\end{bmatrix}\\
C = \begin{bmatrix}
x_{31} & x_{32} \\
x_{41} & x_{42} \\
\end{bmatrix}\\
D = \begin{bmatrix}
x_{33} & x_{34} \\
x_{43} & x_{44} \\
\end{bmatrix}
$$

Com isso, podemos escrever que :
$$
X = \begin{bmatrix}
A & B \\
C & D \\
\end{bmatrix} = \begin{bmatrix}
x_{11} & x_{12} & x_{13} & x_{14} \\
x_{21} & x_{22} & x_{23} & x_{24} \\
x_{31} & x_{32} & x_{33} & x_{34} \\
x_{41} & x_{42} & x_{43} & x_{44}
\end{bmatrix}
$$

Considere tambem a matriz Y:
$$
Y = \begin{bmatrix} 
y_{11} & y_{12} & y_{13} & y_{14} \\
y_{21} & y_{22} & y_{23} & y_{24} \\
y_{31} & y_{32} & y_{33} & y_{34} \\
y_{41} & y_{42} & y_{43} & y_{44}
\end{bmatrix} = \begin{bmatrix} 
E & F \\
G & H \\
\end{bmatrix}
$$
Agora vamos fazer a multiplicação entre X e Y. Aplicando o método tradicional chegamos em :

$$Z = X \times Y $$
$$
Z = \begin{bmatrix}
x_{11}y_{11} + x_{12}y_{21} + x_{13}y_{31} + x_{14}y_{41} & x_{11}y_{12} + x_{12}y_{22} + x_{13}y_{32} + x_{14}y_{42} & z_{13} & z_{14} \\
x_{21}y_{11} + x_{22}y_{21} + x_{23}y_{31} + x_{24}y_{41} & x_{21}y_{12} + x_{22}y_{22} + x_{23}y_{32} + x_{24}y_{42} & z_{23} & z_{24} \\
z_{31} & z_{32} & z_{33} & z_{34} \\
z_{41} & z_{42} & z_{43} & z_{44}
\end{bmatrix} \\
$$
??? Checkpoint
Calcule no Papel $AE + BG$.
:::Gabarito
$$
A \times E + B \times G = 
$$
$$
 \begin{bmatrix}
x_{11}y_{11} + x_{12}y_{21} + x_{13}y_{31} + x_{14}y_{41} & x_{11}y_{12} + x_{12}y_{22} + x_{13}y_{32} + x_{14}y_{42}  \\
x_{21}y_{11} + x_{22}y_{21} + x_{23}y_{31} + x_{24}y_{41} & x_{21}y_{12} + x_{22}y_{22} + x_{23}y_{32} + x_{24}y_{42} \\
\end{bmatrix}
$$
Você pode perceber que isto é o segundo quadrante de $Z$.
:::

???
!!!
Note que AE aqui é uma multiplicação de matrizes. Além disso, AE+BG é uma soma de matrizes.
!!!

Podemos então calcular os outros quadrantes de maneira semelhante:
$$
X \times Y = 
	\begin{bmatrix}
		AE+BG & AF+BH \\
		CE+DG & CF+DH\\
	\end{bmatrix}
$$

Note que o resultado dessa multiplicação é muito parecido com a multiplicação 2x2 feita anteriormente, porém dessa vez os elementos não são números e sim matrizes de ordem 2.
Mas antes de prosseguir vamos fazer uma verificação dessa propriedade. Considere as seguintes matrizes:


O resultado aqui seria o mesmo que multiplicar as duas como matrizes 4x4.




??? Checkpoint
Aplicando a ideia de Strassen e a quebra em matrizes menores, faça o P2 e o P3 de Strassen considerando o produto de matrizes $X \times Y$
:::Gabarito.
A ideia aqui é fazer uma associação, no qual $a_{11} -> A , a_{12} -> B,b_{11} -> E$ e assim sucessivamente. Desse modo vamos ter:
$$
	\begin{aligned}
	P_1 &= (A + D) \cdot (E + H) \\
	P_2 &= (C + D) \cdot E \\
	P_3 &= A \cdot (F - H) \\
	P_4 &= D \cdot (G - E) \\
	P_5 &= (A + B) \cdot H \\
	P_6 &= (C - A) \cdot (E + F) \\
	P_7 &= (B - D) \cdot (G + H) \\
	\end{aligned}
$$
Agora todos os termos são matrizes!
:::

???


Então, se essa propriedade funciona, podemos quebrar matrizes maiores até chegar em matrizes 2 x 2 e utilizar o método de Strassen. Perceba agora que a diminuição do número de multiplicações tem muito mais peso, uma vez que agora estaremos economizando uma multiplicação de matrizes!!


Como vimos, somar uma matriz possui complexidade de tempo quadrada enquanto multiplicar tem complexidade cúbica. Se voltarmos a pergunta do fim da seção anterior, agora sim faz sentido ter um numero maior de adicoes pois cada multiplicacao seria $O(n^3)$.


Complexidade do Algoritmo de Strassen
------------------------
Se sabemos a solução de uma versão menor do problema, podemos usar essa solução para calcular facilmente a solução do original. Portanto, o algoritmo de Strassen pode ser solucionado com recursão.

??? Checkpoint

Qual a base e o passo da recursão da aplicação do Algoritmo de Strassen?

*DICA*: a cada chamada da função é passada a ordem da matriz, sendo essa ordem a metade da matriz onde ela se encontra.

::: Gabarito
Base:
``` c
Se a ordem da matriz igual a 1, retorna sem fazer nada
Se não, continua na função
```
Passo:
``` c
strassen(n/2), sendo n a ordem da matriz
```
:::

???

??? Checkpoint

Quantas vezes a função é chamada no escopo da função?

::: Gabarito
A função deve ser chamada 7 vezes.
:::

???

??? Checkpoint

Quantas operações são feitas a cada chamada da função?

*DICA*: volte na sessão "Algoritmo Strassen para caso particular matrizes 2x2"

::: Gabarito
18 somas de matrizes. Note que as 7 multiplicações de matrizes serão substituídas por chamadas recursivas.
:::

???

Portanto, podemos definir sua complexidade.
* Se n for menor ou igual a 1: apenas 1 iteração. (parte não-recursiva)
* Se n for maior que 1: $n^2$ iterações (parte não-recursiva) + 7 iterações de recursiva(n/2).
!!! Atenção
A cada iteração da função serão feitas 18 somas de matrizas. A complexidade da soma de uma matriz é $O(n^2)$.
!!!
??? Checkpoint

Qual a função matemática que define a recorrência da função?

*DICA*: A cada chamada da função será calculada a soma das matrizes recebidas com complexidade $O(n^2)$.

::: Gabarito
$$
f(n)=\left\{
\begin{array}{ll}
7\cdot f(n/2) + n^2, & \text{se } n>1 \\
1, & \text{se } n\leq 1 \\
\end{array}
\right.
$$
:::

???

Por fim, podemos concluir que a árvore da função é

![](strassen.drawio.png)

??? Checkpoint

Qual a soma das chamadas não recursivas, ou seja, a soma dos vermelhos?

::: Gabarito
$7^0\cdot {\frac{n}{2^0}}^2 + 7^1\cdot {\frac{n}{2^1}}^2 + 7^2\cdot {\frac{n}{2^2}}^2 + ... + 7^{(h-2)}\cdot {\frac{n}{2^{h-2}}}^2 + 7^{(h-1)}$
$7^0\cdot {\frac{n^2}{4^0}} + 7^1\cdot {\frac{n^2}{4^1}} + 7^2\cdot {\frac{n^2}{4^2}} + ... + 7^{(h-2)}\cdot {\frac{n^2}{4^{h-2}}} + 7^{(h-1)}$
$n^2\cdot (\frac{7}{4}^0 + \frac{7}{4}^1 + \frac{7}{4}^2 + ... + \frac{7}{4}^{(h-2)}) + 7^{(h-1)}$

Soma de PG!

$n^2\cdot (1 \cdot \frac{\frac{7}{4}^{(h-1)}-1}{\frac{7}{4}-1}) + 7^{(h-1)}$
:::

???

??? Checkpoint

Qual a altura da árvore?

::: Gabarito
$\log _{2} n$
:::

???
!!!
Para entender as contas a seguir, lembre-se das [propriedades de logaritmo](https://ensino.hashi.pro.br/desprog/resumo/analise/caixa.html).
!!!
Substituindo a altura da árvore na soma das chamadas não recursivas temos:

$n^{2}  \cdot \frac {((7/4)^{(\log_2 n)-1})-1}{7/4-1}+7^{(\log_2 n)-1}$

Aplicando a propriedade distributiva da potenciação, temos:

$n^{2}  \cdot \frac {(7^{(\log_2 n)-1})/ (4^{(\log_2 n)-1})-1}{7/4-1}+7^{(\log_2 n)-1}$

Aplicando a propriedade da mudança de base de logaritmos, temos:

$n^{2}  \cdot \frac {(7^{(\frac{\log_7 n}{\log_7 2}-1)}) / (4^{(\log_2 n)-1})-1}{7/4-1}+7^{(\log_2 n)-1}$

Aplicando a propriedade inversa da multiplicação de potências de mesma base, temos:

$n^{2}  \cdot \frac {(7^{(\frac{\log_7 n}{\log_7 2})} \cdot 7^{-1}) /((2^2)^{(\log_2 n)} \cdot 4^{-1})-1}{7/4-1}+7^{(\log_2 n)-1}$

Isolando o *n* no denominador da fração e aplicando a propriedade $x^{\log_x n} = n$, temos

$n^{2}  \cdot \frac {(\frac{n^{\log_7 2}}{n^2}) \cdot 4/7 -1}{7/4-1}+7^{(\log_2 n)-1}$

??? Checkpoint

Aplicando a propriedade distributiva e ignorando as partes irrelevantes para o cálculo da complexidade, qual a complexidade do algoritmo? 

::: Gabarito

$n^{2}  \cdot (\frac{n^{\log_7 2}}{n^2})$

$n^{\log_7 2}$

Portanto, a complexidade do algoritmo de strassen é, aproximadamente, $O(n^{2.8})$.

:::

???

!!! Curiosidade
O algoritmo de Alman, Williams (2020) possui complexidade $O(n^{2.372})$. Atualmente, é o algortimo mais eficiente para multiplicação de matrizes.
!!!






