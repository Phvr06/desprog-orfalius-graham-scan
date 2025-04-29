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


