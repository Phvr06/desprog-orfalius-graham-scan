Graham Scan
======

Como construir uma cerca?
---------

Observe essa série de árvores plantadas aleatoriamente em um terreno. Sua missão é colocar uma cerca **ao redor de todas elas**, gastando o **mínimo possível de material**.

![](arvores.png)

??? Exercício
Você não pode cortar nenhuma árvore, então a cerca deve dar a volta **por fora de todas**. Naturalmente, você começa a caminhar ao redor do terreno e pensa: “por onde devo passar a cerca para que tudo fique dentro, mas sem enrolar demais o caminho?”. Desenhe como você acha que deveria ser o cercado

::: Gabarito
![](gabarito_arvores.png)
:::

???
---

Geometria
---------

Vamos abstrair um pouco da situação das árvores e utilizar a geometria para tentar nos ajudar a resolver esse problema.

??? Exercício
Qual seria o modo de traduzir o cenário desse problema para a geometria? Quais elementos geométricos poderiam representar bem:
* o terreno?
* as árvores?
* as cercas individualmente?
* todas as cercas depois de conectadas?

::: Gabarito
* O terreno poderia ser representado por um plano cartesiano.
* As árvores poderiam ser representadas por pontos no plano.
* Cada cerca seria um segmento de reta entre 2 pontos.
* Quando todos os segmentos de reta estiverem conectados é formado um polígono em volta dos pontos.
:::

???

Muito bem, agora que já pensamos sobre como o nosso cenário poderia ser abstraído para a geometria, vamos à um exemplo prático.

??? Exercício

Dada a seguinte distribuição de árvores, imagine como seria o desenho de uma cerca que contorne todas elas com o menor comprimento possível, sem deixar nenhuma árvore de fora, por quais árvores (A - H) essa cerca passa?

![](arvores1.png)

::: Gabarito

Você deve ter imaginado algo como:

![](arvores2.png)

Essa cerca passa pelas árvores B, D, G, C e H,formando um polígono, que na área de geometria computacional é chamado de **fecho convexo**. 

:::

???

---

Fecho Convexo
---------

Para definir o fecho convexo primeiro vamos voltar para o básico da geometria e relembrar o que é um polígono convexo.

??? Exercício
Qual das seguintes figuras é um polígono convexo?

![](pconv1.png)

![](pconv2.png)

::: Gabarito
A primeira figura é um polígono convexo.
:::

???

??? Exercício
Agora para formalizar uma definição. O que diferencia o polígono que foi classificado como convexo e o outro (côncavo) no exercício anterior?

::: Gabarito
Para um polígono ser considerado convexo, ele não pode possuir nenhum ângulo interno maior do que 180°.



Uma outra regra simples para definir é que qualquer segmento de reta que tenha 2 extremidades dentro do polígono não pode passar por fora do polígono.

![](pconv3.png)
![](pconv4.png)

:::

???


Agora que já relembramos o que é um polígono convexo podemos avançar. O fecho convexo de um conjunto de pontos $S$ é o subconjunto de $S$ que forma o menor polígono convexo que envolva todos os pontos de $S$. Vamos começar com um exemplo simples:

??? Exercício
Para esse conjunto de pontos, qual seria o fecho convexo?

![](Pontos1.png)

::: Gabarito
Para o conjunto de pontos $S = (A,B,C,D)$. O fecho convexo seria $(A,B,C)$, o que forma o equivalente ao triângulo $\overline{\rm ABC}$

![](Pontos2.png)

Note que, o ponto D pertence a $S$, mas não pertence ao fecho convexo de $S$.

:::

???


??? Exemplo 2
Suponha agora que um novo ponto E seja adicionado ao conjunto de pontos, conforme a figura abaixo. Qual seria o feixo convexo?

![](PontoE1.png)

::: Gabarito
O novo fecho convexo do conjunto $S$ passa a ser $(E,C,B,A)$

![](PontoE2.png)

:::

???


Ou seja, um fecho convexo de um conjunto de pontos é um polígono convexo que engloba **todos** os pontos de dado conjunto.

---

Introdução ao algoritmo de Grahan Scan
---------

Intuitivamente é muito fácil delimitar o fecho convexo, a sua mente faz isso sozinha. Mas, precisamos pensar em um método que realiza essa tarefa, diversas soluções para essa questão foram criadas, e uma delas foi o **algoritmo de Grahan Scan**. Estudaremos agora a **lógica** dele e seu passo a passo, sem se preocupar inicialmente com seu código ou implementação.

No entanto, uma coisa é importante de se ter em mente, o algoritmo deve receber um conjunto de pontos que compõe $S$ e deve devolver os pontos **em ordem** que formam o fecho convexo de $S$, adotaremos como padrão que a ordem desses pontos deve ser no sentido **anti-horário**. No caso do exercício aterior, por exemplo, a saída deveria ser $(E,C,B,A)$, nessa ordem.

Passo 1: precisamos escolher um ponto para começar, tal ponto de inicio chama-se "pivô".

???
Nesse exemplo inicial, qual o ponto apropriado para ser o pivô? Pense em qual dos pontos a seguir é fácil de ser identificado por um algoritmo e com certeza faz parte do fecho convexo, essa é uma boa escolha para pivô, há mais de uma resposta correta.

![](arvores1.png)

::: Gabarito

**O ponto B** seria a escolha mais tradicional para o algoritmo, pois ele é aquele com a **menor coordenada em Y**, porém, qualquer ponto de uma extremidade serviria: o mais da direita (D), mais a esquerda (C ou H) ou o mais a cima (G) também são opções válidas. Vamos adotar sempre o ponto mais baixo, e, em caso de haver outro ponto na mesma coordenada Y, será adotado o com a menor coordenada X, ou seja, o mais a esquerda.

:::

???Exemplo

Passo 2: Agora que sabemos em que ponto começar, para onde iremos?

Primeiro, vamos pensar aonde queremos chegar, desejamos que os pontos que o algoritmo devolve sejam **ordenados** no sentido **anti-horário**.

??? Exercício
Pensando nesse objetivo, qual **propriedade geométrica** entre o pivô e cada um dos pontos que poderíamos adotar como critério para que os pontos ficassem ordenados no sentido anti-horário em relação ao pivô?


::: Gabarito
Para cada ponto, calculamos o ângulo entre a linha horizontal que passa pelo pivô e o segmento que o une ao pivô. Em seguida, como esperamos uma saída dos pontos **ordenados** no sentido **anti-horário**, ordenaremos todos os pontos em ordem crescente desse ângulo, o que os dispõe no sentido anti-horário ao redor do pivô. 
:::

???

Agora com essa ideia em mente devemos praticar a ordenação, utilize o conjunto de pontos abaixo para fazer o que se pede

??? Conjunto de pontos

![](angulos1.png)

???

??? Exercício

Desenhe o segmento de reta entre o pivô e os pontos e represente com $\alpha$, $\beta$ e $\gamma$, respectivamente, os 3 menores ângulos

::: Gabarito
![](angulos2.png)

??? Extra
Consegue pensar em uma implementação de vetor que seja aplicável a essa ocasião ? Sabendo que será necessário adicionar/remover o segmento mais 'recente'

::: Gabarito
Vetor em pilha! Mas não precisa se preocupar com isso agora.
:::
???

:::


???

Excelente, temos a nossa ordenação, agora para começar a formar o fecho convexo devemos traçar segmentos de reta que formem o polígono que desejamos.

???
Considerando a ordenação que foi feita, com qual ponto o pivô deveria formar o primeiro segmento de reta? 

::: Gabarito
Como temos os pontos ordenados pelo ângulo e desejamos que os pontos sejam dados no sentido anti-horário, o ponto escolhido deve ser aquele com o menor ângulo, no caso, o ponto $B$. Esse critério de escolha para formação do próximo segmento de reta vai se repetir durante todo o algoritmo.

![](angulos3.png)

:::

???

OK, temos o início do nosso fecho convexo, então agora vamos repetir o mesmo processo, mantemos o segmento $\overline{\rm AB}$ e formamos o próximo segmento com o último ponto que foi utilizado ($B$) e o próximo ponto na ordem ($E$).


??? Exemplo 4

![](angulos4.png)

???

!!! Cuidado
Percebeu um 'giro' ocorrendo? Lembre-se disso e mantenha o produto vetorial em mente!
!!!

??? Execício
Para praticar, desenhe o próximo segmento de reta e indique se ocorreu um giro no sentido horário ou anti-horário.

::: Gabarito do primeiro segmento
Ocorreu um giro no sentido anti-horário.

![](angulos5.png)
:::
???

??? Exercício
Agora faça o mesmo para o próximo segmento.

::: Gabarito do segundo segmento
Ocorreu um giro no sentido horário.

![](angulos6.png)

Caso continuássemos o polígono com os próximos segmentos, $\overline{\rm DC}$ e $\overline{\rm CA}$, o polígono seria convexo? Caso não, qual poderia ser o procedimento para continuar?

Dica: Lembre da regra da mão direita

::: Gabarito
Quando conecta-se $F$ e $D$ observa-se um giro no sentido **horário** quando isso ocorre, o ângulo interno não será mais inferior a 180°, impossibilitando que o polígono formado seja convexo. Portanto, devemos **descartar** o ponto $F$ e as conexões com ele, pois ele com certeza não pertence ao fecho, então retornamos ao ponto anterior ($E$) e voltamos a seguir a lista ordenada, sem o $F$ dessa vez. Isso deverá ser repetido caso haja mais giros no sentido horário.

Ainda, uma maneira mais prática de pensar é usando a regra da mão direita para calcular o sentido da resultante do produto vetorial. Caso essa resultante esteja "entrando" na tela descarta-se o ponto.
:::

???

??? Exercício
Se você seguiu corretamente os exercícios você chegou a conclusão que precisa ocorrer o descarte de um ponto.
Como deveria ficar o fecho convexo após esse procedimento ?

::: Gabarito real do segundo segmento
![](angulos7.png)

Deve ter percebido que o giro para o segmento $\overline{\rm ED}$ também foi no sentido horário, logo, $E$ também deve ser descartado. Como ficaria de fato o segundo segmento?

::: Gabarito real real do segundo segmento
![](angulos8.png)
:::
???

Dando sequência ao procedimento, descobriremos que os segmentos $\overline{\rm DC}$ e $\overline{\rm CA}$ atendem a todos os requisitos, e portanto, é obtido o fecho convexo desse conjunto de pontos: 

??? Fecho Completo

![](angulos9.png)

???

Para esse exemplo a sequência de segmentos que foram formados durante o algoritmo foi::

Ordem dos pontos em relação ao ângulo: B-E-F-D-C

* $+ \overline{\rm AB}$
* $+ \overline{\rm EB}$
* $+ \overline{\rm EF}$
* $+ \overline{\rm DF}$
* $- \overline{\rm DF}$
* $- \overline{\rm EF}$
* $+ \overline{\rm DE}$
* $- \overline{\rm DE}$
* $- \overline{\rm EB}$
* $+ \overline{\rm DB}$
* $+ \overline{\rm DC}$
* $+ \overline{\rm CA}$

Ou seja, para cada vez que um segmento é adicionado, usa-se a notação "$+ \overline{\rm AB}$". e para cada vez que voltar um passo, ou seja, para quando um segmento não será mais utilizado, é usada a notação "$- \overline{\rm AB}$".

???Exercício 

Dada a imagem abaixo escreva qual deverá ser a **sequência** de segmentos formados quando aplicado o algoritmo de Grahan Scan. Pode ser útil escrever primeiro qual a ordem dos pontos em relação ao A. Não esqueça de incluir os segmentos que foram feitos e depois descartados!

![](angulos10.png)


::: Gabarito

Adotando o ponto A como o pivô, teremos a seguinte ordem:

D-B-C-E-G-F-H-I

Para chegar a essa ordem, os seguintes segmentos foram utilizados:

* $+ \overline{\rm AD}$
* $+ \overline{\rm DB}$
* $+ \overline{\rm BC}$
* $- \overline{\rm BC}$
* $- \overline{\rm DB}$
* $+ \overline{\rm DC}$
* $+ \overline{\rm CE}$
* $- \overline{\rm CE}$
* $- \overline{\rm DC}$
* $+ \overline{\rm DE}$
* $+ \overline{\rm EG}$
* $+ \overline{\rm GF}$
* $- \overline{\rm GF}$
* $- \overline{\rm EG}$
* $+ \overline{\rm EF}$
* $+ \overline{\rm FH}$
* $+ \overline{\rm HI}$
* $+ \overline{\rm IA}$

:::


???

---

Desafio
---------
Parabéns, você chegou ao fim do handout, caso sobre tempo, sinta-se livre para aprender como faz a implementação.

---
Implementação do algoritmo
---------

Agora que já há um entendimento maior sobre o algoritmo de Graham Scan vamos partir para a implementação do algoritmo.

??? Exercício 1

A primeira e segunda partes da implementação são encontrar o ponto com o menor Y e ordenar os pontos com base no ângulo que formam com o pivô, considere que 2 funções já foram implementadas para cuidar disso, caso haja interesse, o código para elas está em "Curiosidade".

Prosseguindo com a implementação, queremos que a função de Graham Scan receba todos os pontos e devolva aqueles que compõe o fecho convexo. O algoritmo vai ir passando por diversos pontos e adicionando/removendo eles com base nas condições explicadas anteriormente, de modo que sempre um ponto removido é o mais recente a ter sido adicionado, qual estrutura de dados aprendida em aula tem características que podem ser úteis nesse caso?

::: Curiosidade

Considere que a seguinte implementação de pontos já foi feita:
``` c
typedef struct {
  int x;
  int y;
} Ponto;

typedef struct {
  Ponto* pontos;
  int tamanho;
} Pontos;
```

A implementação a seguir cuida de encontrar o ponto pivô e de ordenar todos os pontos com base no ângulo formado com ele.

``` c
#include <math.h>

Ponto encontra_pivo(Pontos p) {
  Ponto menor = p.pontos[0];
  for (int i = 1; i < p.tamanho; i++) {
    if (p.pontos[i].y < menor.y) {
      menor = p.pontos[i];
    } else if (p.pontos[i].y == menor.y) {
      if (p.pontos[i].x < menor.x) {
        menor = p.pontos[i];
      }
    }
  }

  return menor;
}

double calcula_angulo(Ponto p0, Ponto p) {
    return atan2(p.y - p0.y, p.x - p0.x);
}

double dist(Ponto a, Ponto b) {
    int dx = a.x - b.x;
    int dy = a.y - b.y;
    return dx * dx + dy * dy;
}

void troca(Ponto* v, int i, int j) {
    Ponto temp = v[i];
    v[i] = v[j];
    v[j] = temp;
}

int particiona(Ponto* v, int l, int r, Ponto pivo) {
    int p = l;
    double angulo_p = calcula_angulo(pivo, v[r]);

    for (int i = l; i < r; i++) {
        double angulo_i = calcula_angulo(pivo, v[i]);

        if (angulo_i < angulo_p || (angulo_i == angulo_p && dist(pivo, v[i]) < dist(pivo, v[r]))) {
            troca(v, p, i);
            p++;
        }
    }
    troca(v, p, r);
    return p;
}

void quick_sort_r(Ponto* v, int l, int r, Ponto pivo) {
    if (l >= r) return;
    int p = particiona(v, l, r, pivo);
    quick_sort_r(v, l, p - 1, pivo);
    quick_sort_r(v, p + 1, r, pivo);
}

void quick_sort(Ponto* v, int n, Ponto pivo) {
    quick_sort_r(v, 0, n - 1, pivo);
}
```

:::

::: Gabarito

A estrutura de pilha funciona muito bem para o que desejamos fazer para o Graham Scan. Considere durante esse handout a implementação vista em aula com vetores estáticos, ela servirá para o propósito.

:::

???

??? Exercício 2
Ok, agora que já estabelecemos algumas coisas importantes, vamos prosseguir com a implementação.
Considerando as funções já implementadas, teriamos um esqueleto de código parecido com isso:
``` c
Pontos graham_scan(Pontos p) {
  Ponto pivo = encontra_pivo(p);
  quick_sort(p.pontos, p.tamanho, pivo);

  stack_int *pilha = stack_int_new(p.tamanho);
}
```

Para continuar, um passo fundamental é como podemos determinar se um giro foi feito no sentido horário ou antihorário? Isso pode ser um pouco difícil de pensar sem ajuda, então há uma dica disponível caso pense que é necessário. Não é preciso pensar no código por enquanto, pense na geometria do problema. 

::: Dica
Pense nos segmentos de retas entre pontos como vetores, **lembre-se da regra da mão direita** (https://youtube.com/shorts/CXyjnxNH7tk?si=DZR6hHgZ5w8c-5xf), o que acontece com o produto vetorial entre esses dois vetores no caso de uma rotação em cada sentido?

Rotação antihorária:

![](prodvet1.png)

Rotação horária:

![](prodvet2.png)

:::

::: Gabarito
Partindo da dica, basta calcular o produto vetorial entre 2 vetores, um para o segmento $\overline{\rm AB}$ outro para $\overline{\rm BC}$. Vamos chamar $\overline{\rm AB}$ de $\vec{u}$ e $\overline{\rm BC}$ de $\vec{v}$. 
O produto vetorial é o resultado do determinante da matriz:

$\begin{bmatrix}
\vec{i} & \vec{j} & \vec{k}\\
\vec{u_{x}} & \vec{u_{y}} & 0\\
\vec{v_{x}} & \vec{v_{y}} & 0\\
\end{bmatrix}$

que resulta na expressão:

$(\vec{u_{x}} \cdot \vec{v_{y}} - \vec{v_{x}} \cdot \vec{u_{y}})\cdot \vec{k}$

Caso o resultado dessa conta seja positivo, significa que a rotação é no sentido antihorário, se for negativo, a rotação é no sentido horário, se for 0, significa que os pontos estão na mesma linha.


:::

???

??? Exercício 3

Agora sim, sabendo sobre o exercício anterior, tente escrever o código para uma função que devolve 1 caso a rotação seja no sentido antihorário, 0 para colinear e -1 para rotação no sentido horário. A assinatura da função é:
``` c
int orientacao(Ponto a, Ponto b, Ponto c);
```

::: Gabarito
Há muitas forma de escrever esse código, aqui está uma delas:
``` c
int orientacao(Ponto a, Ponto b, Ponto c) {
  Ponto u;
  u.x = b.x - a.x;
  u.y = b.y - a.y;
  Ponto v;
  v.x = c.x - b.x;
  v.y = c.y - b.y;

  int val = (u.x * v.y) - (u.y * v.x);

  if (val == 0) return 0;
  return (val > 0) ? 1 : -1;
}
```

:::

???

??? Exercício 4

Agora é a hora de realmente implementar o algoritmo, todas as funções auxiliares necessárias estão prontas. Complete o início da função graham_scan e implemente o loop principal do algoritmo, que vai adicionar à pilha os pontos que fazem parte do fecho, lembre-se que a pilha armazena apenas inteiros, e não pontos:

``` c

Pontos graham_scan(Pontos p) {
  Ponto pivo = encontra_pivo(p);

  quick_sort_angulo(p.pontos, p.tamanho, pivo);

  stack_int *pilha = stack_int_new(p.tamanho);
  stack_int_push(pilha, 0);
  stack_int_push(pilha, 1);

  // IMPLEMENTE O FOR AQUI
}

```

::: Gabarito

``` c
Pontos graham_scan(Pontos p) {
  Ponto pivo = encontra_pivo(p);

  quick_sort_angulo(p.pontos, p.tamanho, pivo);

  stack_int *pilha = stack_int_new(p.tamanho);
  stack_int_push(pilha, 0);
  stack_int_push(pilha, 1);

  for (int i = 2; i < p.tamanho; i++) {
    while (pilha->size >= 2) {
      int topo = pilha->data[pilha->size - 1];
      int anterior = pilha->data[pilha->size - 2];

      if (orientacao(p.pontos[anterior], p.pontos[topo], p.pontos[i]) != 1) {
        stack_int_pop(pilha);
      } else {
        break;
      }
    }
    stack_int_push(pilha, i);
  }
}

```

:::

???

??? Exercício 5

Agora é a hora de implementar o último loop do código de graham_scan. A terceira parte do código (que vem depois do for implementado no exercício anterior) está dada abaixo:

``` c
  Pontos resultado;
  resultado.tamanho = pilha->size;
  resultado.pontos = malloc(resultado.tamanho * sizeof(Ponto));

  //IMPLEMENTE O FOR AQUI

  stack_int_delete(&pilha);
  return resultado;
```
::: Gabarito

``` c
Pontos resultado;
  resultado.tamanho = pilha->size;
  resultado.pontos = malloc(resultado.tamanho * sizeof(Ponto));

  for (int j = 0; j < pilha->size; j++) {
    resultado.pontos[j] = p.pontos[pilha->data[j]];
  }

  stack_int_delete(&pilha);
  return resultado;

```

:::

???

??? Resposta Final
O código completo portanto ficou da seguinte maneira:

::: Gabarito
``` c

Pontos graham_scan(Pontos p) {
  Ponto pivo = encontra_pivo(p);

  quick_sort_angulo(p.pontos, p.tamanho, pivo);

  stack_int *pilha = stack_int_new(p.tamanho);

  stack_int_push(pilha, 0);
  stack_int_push(pilha, 1);

  for (int i = 2; i < p.tamanho; i++) {
    while (pilha->size >= 2) {
      int topo = pilha->data[pilha->size - 1];
      int anterior = pilha->data[pilha->size - 2];

      if (orientacao(p.pontos[anterior], p.pontos[topo], p.pontos[i]) != 1) {
        stack_int_pop(pilha);
      } else {
        break;
      }
    }
    stack_int_push(pilha, i);
  }

  Pontos resultado;
  resultado.tamanho = pilha->size;
  resultado.pontos = malloc(resultado.tamanho * sizeof(Ponto));

  for (int i = 0; i < pilha->size; i++) {
    resultado.pontos[i] = p.pontos[pilha->data[i]];
  }

  stack_int_delete(&pilha);
  return resultado;
}

```
:::
???

---

Complexidade do algoritmo
---------

??? Exercício 1
Começando com a primeira parte do código, qual será a complexidade dessa seção do código:


``` c
  Ponto pivo = encontra_pivo(p);
```

::: Gabarito

Esse trecho do código percorre a lista de n pontos uma única vez, e portanto sua complexidade será **$O(n)**

:::

???

??? Exercicio 2
Agora, para algo mais simples e visto em aula, qual deve ser a complexidade dessa próxima parte do código:

``` c
  quick_sort_angulo(p.pontos, p.tamanho, pivo);
``` 
::: Gabarito

Obviamente, como visto em sala, a complexidade desse algoritmo sera **O(n log n)**

:::

???

??? Exercicio 3
Agora, para a complexidade da próxima estapa do código, em que os pontos são adicionados à pilha

``` c
  stack_int *pilha = stack_int_new(p.tamanho);

  stack_int_push(pilha, 0);
  stack_int_push(pilha, 1);

  for (int i = 2; i < p.tamanho; i++) {
    while (pilha->size >= 2) {
      int topo = pilha->data[pilha->size - 1];
      int anterior = pilha->data[pilha->size - 2];

      if (orientacao(p.pontos[anterior], p.pontos[topo], p.pontos[i]) != 1) {
        stack_int_pop(pilha);
      } else {
        break;
      }
    }
    stack_int_push(pilha, i);
  }
``` 
::: Gabarito

Cada ponto entra na pilha uma vez e sai no máximo uma vez (push e pop). O total de operações de pop em toda a execução é **≤n**, e potanto, sua complexidade será **O(n)**

:::

??? 

??? Exercicio 4
Agora, para a complexidade da última estapa do código, a extração do resultado

``` c
  Pontos resultado;
  resultado.tamanho = pilha->size;
  resultado.pontos = malloc(resultado.tamanho * sizeof(Ponto));

  for (int i = 0; i < pilha->size; i++) {
    resultado.pontos[i] = p.pontos[pilha->data[i]];
  }

  stack_int_delete(&pilha);
  return resultado;
``` 
::: Gabarito

Copia até n pontos da estrutura da pilha para a saída, e portanto sua complexidade será **O(n)**

:::

??? 

A complexidade total do algoritmo portanto será:

``` c
O(n) + O(nl og n) + O(n) + O(n) = O(n log n)
``` 

!!! Observação

O passo dominante é a ordenação angular por quicksort, garantindo que o Graham Scan, no caso médio, seja de complexidade *O(n log n)*

!!!