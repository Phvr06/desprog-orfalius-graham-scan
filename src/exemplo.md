Graham Scan
======

Como construir uma cerca?
---------

Imagine que você é responsável por cercar uma série de árvores plantadas aleatoriamente em um terreno. Sua missão é colocar uma cerca **ao redor de todas elas**, gastando o **mínimo possível de material**.

Você não pode cortar nenhuma árvore, então a cerca deve dar a volta **por fora de todas**. Naturalmente, você começa a caminhar ao redor do terreno e pensa: “por onde devo passar a cerca para que tudo fique dentro, mas sem enrolar demais o caminho?”

---

Geometria
---------

Vamos abstrair um pouco da situação das árvores e utilizar a geometria para tentar nos ajudar a resolver esse problema.

??? Checkpoint
Qual seria o modo de traduzir o cenário desse problema para a geometria? Quais elementos geométricos poderiam representar bem:
* o terreno?
* as árvores?
* as cercas individualmente?
* todas as certas depois de conectadas?

::: Gabarito
* O terreno poderia ser representado por um plano cartesiano.
* As árvores poderiam ser representadas por pontos no plano.
* Cada cerca seria um segmento de reta entre 2 pontos.
* Quando todos os segmentos de reta estiverem conectados é formado um polígono em volta dos pontos.
:::

???

Muito bem, agora que já pensamos sobre como o nosso cenário poderia ser abstraído para a geometria, vamos à um exemplo prático.

??? Checkpoint

Dada a seguinte distribuição de árvores, imagine como seria o desenho de uma cerca que contorne todas elas com o menor comprimento possível, sem deixar nenhuma árvore de fora, por quais árvores (A - H) essa cerca passa?

![](arvores1.png)

::: Gabarito

Você deve ter imaginado algo como:

![](arvores2.png)

Essa cerca passa pelas árvores B, D, G, C e H,formando um polígono, que na área de geometria computacional é chamado de **fecho convexo**. 

:::

???

Fecho Convexo
---------

Um fecho convexo é o **menor conunto convexo** que contém um conjunto de pontos denominado de S. Geometricamente, forma um polígono convexo, cujos vértices são um subconjunto de pontos de S. Vamos começar analisando um exemplo simples:

??? Exemplo 1

![](Pontos1.png)

???

No exemplo acima, temos o conjunto de pontos *S* = (A,B,C,D). O fecho convexo desse conjunto seria o equivalente ao triângulo ABC

??? Exemplo 1

![](Pontos2.png)

???

Note que, o ponto D pertence a *S*, mas não pertence ao fecho convexo de *S*. Suponha agora que um novo ponto E seja adicionado ao conjunto de pontos, conforme a figura abaixo

??? Exemplo 2

![](PontoE1.png)

???

O novo fecho convexo do conjunto *S* passa a ser

??? Exemplo 2

![](PontoE2.png)

???

Introdução ao algoritmo de Grahan Scan
---

Eu sei que intuitivamente é muito fácil delimitar o fecho convexo, a sua mente faz isso sozinha. Mas, precisamos pensar em método que realiza essa tarefa, diversas pessoas deram soluções distintas para essa questão e uma delas foi o **algoritmo de Grahan Scan**. Então, 
estudaremos agora a **lógica** dele e o seu passo a passo, sem se preocupar com o cógigo ou implementação.

Passo 1: precisamos escolher um ponto para começar, tal ponto de inicio chama-se 'pivô'.

???
Dada a seguinte imagem, qual o ponto apropriado para ser o pivô ?

![](arvores1.png)

::: Gabarito

**O ponto B**, pois ele é aquele com a **menor coordenada em Y**, ainda, se existissem 2 pontos com a mesma coordenada em Y, deve-se escolher aquele com o menor X.

:::

???

Passo 2: Agora que sabemos em que ponto começar, para onde iremos ? **Começando do ponto com maior X**, devemos calcular o ângulo que cada outro ponto faz em relação a uma linha horizontal passando pelo pivô e o ordenamos de modo crescente, de modo que essa iteção ocorra no sentido **anti-horário**. Uma vez que temos essa lista ordenada, começamos traçando uma reta entre o pivô e o ponto que forma o menor ângulo.

Siga os exemplos abaixo para uma explicação mais visual:


Dado um conjunto de pontos: 
??? Exemplo 1

![](angulos1.png)

???

Descobre-se os ângulos formados entres os pontos e o pivõ, em relação ao eixo X: 
??? Exemplo 2

![](angulos2.png)

???

Sabendo que o alpha é o menor ângulo, traça-se uma reta entre o pivô (A) e o ponto B: 
??? Exemplo 3

![](angulos3.png)

???

Excelente, temos o início do nosso fecho convexo, sabemos que temos os ângulos, entre o pivô e os outros pontos em relação ao eixo X, ordenados crescentemente. Então, deve-se traçar uma reta entre o ponto atual e o ponto que tem o segundo menor ângulo. 

!!!
CUIDADO!! Perceba ainda, que ocorreu um 'giro' no sentido **anti-horário** entre a continuação da reta AB (pontilhado) e a reta EB, esse fato é **crucial** para o funcionamento do algoritmo, pois caso encontremos um 'giro' no sentido **horário** teremos que mudar o procedimento.
!!!

??? Exemplo 4

![](angulos4.png)

???

A mesma coisa se repete entre o ponto E e F.

??? Exemplo 5

![](angulos5.png)

???

Entretanto, quando conecta-se F e D observa-se um giro no sentido **horário**, quando isso ocorre devemos **descartar** o ponto F, pois ele com certeza não pertence ao fecho e retornamos ao ponto anterior (E)

??? Exemplo 6

![](angulos6.png)

???

Observamos o mesmo problema quando conectamos DE, assim, descartamos D e voltamos para B

??? Exemplo 7

![](angulos7.png)

???

Conectando B com D não observamos esse problema, assim, é possível continuar o algoritmo até o fecho estar completo

??? Exemplo 8

![](angulos8.png)

???

Por fim: 

??? Fecho Completo

![](angulos9.png)

???

???Exercício 

Dada a imagem abaixo escreva qual deverá ser a **sequência** de segmentos formados quando aplicado o algoritmo de Grahan Scan

![](angulos10.png)


::: Gabarito


:::


???

