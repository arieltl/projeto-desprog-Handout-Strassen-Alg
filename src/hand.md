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

Implemente em C uma função que receba um inteiro n e três matrizes quadradas de ordem n. Ao rodar a função, a terceira matriz deve a soma da primeira e da segunda.
::: Gabarito

```c
void matrix_sum(int matrix1[n][n], int matrix2[n][n], int result_matrix[n][n], int n) {
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			result_matrix[i][j] = matrix1[i][j] + matrix2[i][j];
		}
	}
}
```

:::

???
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
		(aj + bm + cp) & (ak + bn + cq) & (al + bo + cr) \\
		(dj + em + fp) & (dk + en + fq) & (dl + eo + fr) \\
		(gj + hm + ip) & (gk + hn + iq) & (gl + ho + ir) \\
		\end{bmatrix}
$$
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
??? Exercício

Implemente em C uma função que receba um inteiro n e três matrizes quadradas de ordem n e altera a terceira matriz para ser a multiplicação da primeira e da segunda.
::: Gabarito
```c
void matrix_multiply(int matrix1[n][n], int matrix2[n][n], int result_matrix[n][n], int n) {
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			result_matrix[i][j] = 0;
			for (int k = 0; k < n; k++) {
				result_matrix[i][j] += matrix1[i][k] * matrix2[k][j];
			}
		}
	}
}
```

:::

???
??? Checkpoint
Qual a complexidade da multiplicação de matrizes quadradas?
:::Gabarito
Há 3 loops de 0 a n aninhados, logo a complexidade função é $O(n^3)$.
:::

???

É evidente que a complexidade de tempo da multiplicação de matrizes é uma ordem de grandeza superior que a da soma. A multiplicação é uma operação bastante importante para diversas áreas da computação, como processamento de imagens e inteligência artificial. Como podemos aproximar a complexidade da multiplicação
à da soma?

Algoritmo Strassen para caso particular matrizes 2x2 
---------
Volker Strassen descobriu uma forma alternativa para realizar a multiplicação de duas matrizes 2x2. Primeiramente, é necessário calcular 7 produtos
a partir dos elementos das matrizes que deseja-se multiplicar.
$$
	\begin{aligned}
	P_1 &= (a_{11} + a_{22})(b_{11} + b_{22}) \\
	P_2 &= (a_{21} + a_{22})b_{11} \\
	P_3 &= a_{11}(b_{12} - b_{22}) \\
	P_4 &= a_{22}(b_{21} - b_{11}) \\
	P_5 &= (a_{11} + a_{12})b_{22} \\
	P_6 &= (a_{21} - a_{11})(b_{11} + b_{12}) \\
	P_7 &= (a_{12} - a_{22})(b_{21} + b_{22}) \\
	\end{aligned}
$$
Depois, podemos calcular a matriz resultante usando apenas somas. O resultado será igual ao do método aprendido no Ensino Médio.
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
A demonstração do porque isso funciona não entra no escopo da matéria. Apenas confie, funciona.
!!!

Sabemos que no método tradicional de uma matriz quadrada de ordem n temos $n^3$ multiplicações. Portanto, para resultar uma matriz 2x2 precisamos de 8 multiplicações. Utilizando esse novo método, precisamos apenas de 7 multiplicações. Por outro lado, temos 18 adições para se realizar, enquanto no método tradicional seriam 4.

Será que a diminuição de apenas uma multiplicação vai compensar tantas adições extras? Nesse caso, em que os elementos da matriz são apenas números realmente
não parece fazer sentido. Porém estamos analisando apenas o caso de n=2,vamos agora tentar generalizar para matrizes com outros n.

 


Multiplicação com divisão e conquista
---------
O verdadeiro algoritmo de Strassen não funciona apenas para matrizes 2x2. Para entendermos o algoritmo completo, precisamos primeiro ver como podemos quebrar uma multiplicação de matrizes em multiplicações menores.
Por exemplo uma matriz X de ordem 4 pode ser representada como uma matriz 2x2 onde cada elemento é uma matriz 2x2.
$$
	X
	=
	\begin{bmatrix}
	a & b & c & d \\
	e & f & g & h \\
	i & j & k & l \\
	m & n & o & p \\
	\end{bmatrix}
	=
	\begin{bmatrix}
		A & B \\
		C & D \\
	\end{bmatrix}
$$
$$	
	A =
\begin{bmatrix}
a & b \\
e & f \\
\end{bmatrix}
;
 B = 
\begin{bmatrix}
c & d \\
g & h \\
\end{bmatrix} ;
C =
\begin{bmatrix}
i & j \\
m & n \\
\end{bmatrix};
D =
\begin{bmatrix}
k & l \\
o & p \\
\end{bmatrix};
$$
Poderíamos ter também uma matriz Y 2x2 onde cada elemento é uma matriz 2x2.
$$ 
Y =
	\begin{bmatrix}
		E & F \\
		G & H\\
	\end{bmatrix}
$$
E podemos multiplicar X e Y como matrizes 2 por 2 usando:
$$
X \times Y = 
	\begin{bmatrix}
		AE+BG & AF+BH \\
		CE+DG & CF+DH\\
	\end{bmatrix}
$$
O resultado aqui seria o mesmo que multiplicar as duas como matrizes 4x4.

!!!
Note que AE aqui é uma multiplicação de matrizes. Além disso, AE+BG é uma soma de matrizes.
!!!

Como vimos, somar uma matriz possui complexidade de tempo quadrada enquanto multiplicar tem complexidade cúbica. Se voltarmos a pergunta do fim da seção anterior, agora sim faz sentido ter um numero maior de adicoes pois cada multiplicacao seria $O(n^3)$.

Strassen generalizado recursivo
------------------------
Considerando as mesmas matrizes x e y da etapa anterior, poderiamos utilizar a estratégia criada por strassen para calcular AE, BG, AF, BH...
Mas podemos ir alem disso, já que X e Y são matrizes 2x2 podemos usar a mesma estratégia para calcular X e Y, porém agoras os 7 produtos serão matrizes:

$$
	\begin{aligned}
	M_1 &= (a_{11} + a_{22})(b_{11} + b_{22}) \\
	M_2 &= (a_{21} + a_{22})b_{11} \\
	M_3 &= a_{11}(b_{12} - b_{22}) \\
	M_4 &= a_{22}(b_{21} - b_{11}) \\
	M_5 &= (a_{11} + a_{12})b_{22} \\
	M_6 &= (a_{21} - a_{11})(b_{11} + b_{12}) \\
	M_7 &= (a_{12} - a_{22})(b_{21} + b_{22}) \\
	\end{aligned}
$$

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
	M_1 + M_4 - M_5 + M_7 & M_3 + M_5 \\
	M_2 + M_4 & M_1 - M_2 + M_3 + M_6 \\
	\end{bmatrix}
$$
Aqui a11,a12... e b11,b12... são matrizes.

Portanto podemos partir de qualquer matriz quadrada de ordem n onde n é uma potencia de 2 aplicar essa estrategia de quebrar a matriz em 4 pedaços recursivamente e estaremos sempre trabalhando com matrizes com n=2.

**A implementar na proxima sprint**
* Detalhar mais a seção Strassen generalizado
* Implementar strassen
* strassen em matrizes em que n nao é potência de 2
* Conclusão de complexidade de tempo de Strassen