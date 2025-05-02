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

O fecho convexo de um conjunto de pontos $S$ é o menor polígono convexo que os contém, formado por alguns dos próprios pontos de $S$ como vértices. Vamos começar com um exemplo simples:

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
estudaremos agora a **lógica** dele e o seu passo a passo, sem se preocupar com o código ou implementação.

Passo 1: precisamos escolher um ponto para começar, tal ponto de inicio chama-se 'pivô'.

???
Dada a seguinte imagem, qual o ponto apropriado para ser o pivô ?

![](arvores1.png)

::: Gabarito

**O ponto B** seria a escolha mais tradicional para o algoritmo, pois ele é aquele com a **menor coordenada em Y**, porém, qualquer ponto de uma extremidade serviria: o mais da direita (D), mais a esquerda (C ou H) ou o mais a cima (G) também são opções válidas. Vamos adotar sempre o ponto mais baixo, em caso de haver mais de um como com o ponto mais a esquerda desse exemplo, vamos adotar sempre aquele escolher aquele mais à esquerda (menor X).

:::

???

Passo 2: Agora que sabemos em que ponto começar, para onde iremos? Devemos calcular o ângulo que cada outro ponto faz em relação a uma linha horizontal passando pelo pivô e ordenamos todos os pontos com base nesse ângulo, de modo que os pontos fiquem ordenados no sentido **anti-horário**. Uma vez que temos essa lista ordenada, começamos traçando uma reta entre o pivô e o ponto que forma o menor ângulo.

Siga os exemplos abaixo para uma explicação mais visual:


Dado um conjunto de pontos: 
??? Exemplo 1

![](angulos1.png)

???

Descobre-se os ângulos formados entres os pontos e o pivô, em relação ao eixo X: 
??? Exemplo 2

![](angulos2.png)

???

Sabendo que o $\alpha$ é o menor ângulo, traça-se uma reta entre o pivô (A) e o ponto B: 
??? Exemplo 3

![](angulos3.png)

???

Excelente, temos o início do nosso fecho convexo, sabemos que temos os ângulos, entre o pivô e os outros pontos em relação ao eixo X, ordenados crescentemente. Então, deve-se traçar uma reta entre o ponto atual e o ponto que tem o segundo menor ângulo. 

!!!
Perceba ainda, que ocorreu um 'giro' no sentido **anti-horário** entre a continuação da reta AB (pontilhado) e a reta EB, esse fato é **crucial** para o funcionamento do algoritmo, pois caso encontremos um 'giro' no sentido **horário** teriamos que mudar o procedimento.
!!!

??? Exemplo 4

![](angulos4.png)

???

A mesma coisa se repete entre os pontos E e F.

??? Exemplo 5

![](angulos5.png)

???

Entretanto, quando conecta-se F e D observa-se um giro no sentido **horário**, quando isso ocorre devemos **descartar** o ponto F e as conexões com ele, pois ele com certeza não pertence ao fecho, então retornamos ao ponto anterior (E)

??? Exemplo 6

![](angulos6.png)

???

Observamos o mesmo problema quando conectamos DE, assim, descartamos E e voltamos para B

??? Exemplo 7

![](angulos7.png)

???

Conectando B com D não observamos esse problema, assim, é possível continuar o algoritmo até o fecho estar completo

??? Exemplo 8

![](angulos8.png)

???

Por fim, quando continuamos com esse procedimento, traçaremos os segmentos DC e CA, concluindo o fecho para este exemplo: 

??? Fecho Completo

![](angulos9.png)

???

???Exercício 

Dada a imagem abaixo escreva qual deverá ser a **sequência** de segmentos formados quando aplicado o algoritmo de Grahan Scan. Não esqueça de incluir os segmentos que foram feitos e depois descartados

![](angulos10.png)


::: Gabarito



:::


???

