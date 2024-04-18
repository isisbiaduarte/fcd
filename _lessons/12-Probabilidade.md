---
layout: page
title: Probabilidade
nav_order: 12
---
[<img src="https://raw.githubusercontent.com/flaviovdf/fcd/master/assets/colab_favicon_small.png" style="float: right;">](https://colab.research.google.com/github/flaviovdf/fcd/blob/master/_lessons/09-Funcoes.ipynb)

# Tópico 12 – Probabilidade
{: .no_toc .mb-2 }

Vamos aprender o básico do básico de probabilidade. Vocês terão mais de um curso com esse assunto!
{: .fs-6 .fw-300 }

{: .no_toc .text-delta }
Resultados Esperados

1. Entender o que são eventos, massa e densidade.
2. Entender algumas propriedades básicas de probabilidade.
3. Independência e Bayes

{: .no_toc .text-delta }
Material Adaptado do [DSC10 (UCSD)](https://dsc10.com/)

### Agenda

Abordaremos os fundamentos da teoria das probabilidades. Esta é uma lição de matemática; faça anotações escritas. ✍🏽

### Alguns locais para ler mais sobre o assunto

A probabilidade é um assunto complicado. Se não clicar durante a aula ou nas tarefas, dê uma olhada nos seguintes recursos:

-[Computational and Inferential Thinking, Chapter 9.5](https://inferentialthinking.com/chapters/09/5/Finding_Probabilities.html).

-[Theory Meets Data, Chapters 1 and 2](http://stat88.org/textbook/content/Chapter_01/00_The_Basics.html).

-[Khan Academy's unit on Probability](https://www.khanacademy.org/math/probability/xa88397b6:probability).

### Teoria da probabilidade

- Algumas coisas na vida *parecem* aleatórias.
- por exemplo. jogando uma moeda ou lançando um dado 🎲.
- A **probabilidade** de ver "cara" ao lançar uma moeda honesta é $\frac{1}{2}$.
- Uma interpretação da probabilidade diz que se lançássemos uma moeda infinitamente muitas vezes, então $\frac{1}{2}$ dos resultados seriam caras.

### Terminologia

- **Experimento**: Um processo ou ação cujo resultado é aleatório.
- por exemplo, lançar um dado.
- por exemplo, jogar uma moeda duas vezes.
- **Resultado**: O resultado de um experimento.
- por exemplo, os resultados possíveis do lançamento de um dado de seis lados são 1, 2, 3, 4, 5 e 6.
- por exemplo, os resultados possíveis de jogar uma moeda duas vezes são HH, HT, TH e TT.
- **Evento**: um conjunto de resultados.
- por exemplo, o evento em que o dado cai num número par é o conjunto de resultados {2, 4, 6}.
- por exemplo, o evento em que o dado cai em 5 é o conjunto de resultados {5}.
- por exemplo, o evento em que há pelo menos 1 cara em 2 lançamentos é o conjunto de resultados {HH, HT, TH}.

### Terminologia

- **Probabilidade**: um número entre 0 e 1 (equivalentemente, entre 0% e 100%) que descreve a probabilidade de um evento.
- 0: o evento nunca acontece.
- 1: o evento sempre acontece.
- Notação: se $A$ é um evento, $P(A)$ é a probabilidade desse evento.

### Resultados igualmente prováveis

- Se todos os resultados do evento $A$ forem igualmente prováveis, então a probabilidade de $A$ é

$$P(A) = \frac{\text{\# de resultados satisfatórios $A$}}{\text{número total de resultados}}$$

- **Exemplo 1:** Suponha que lançamos uma moeda **não enviesada** 3 vezes. Qual é a probabilidade de vermos exatamente 2 caras?

### Verificação de conceito ✅

Tenho três cartas: vermelha, azul e verde. Qual é a chance de eu escolher uma carta aleatoriamente e ela ser verde, então – **sem devolvê-la** – eu escolher outra carta aleatoriamente e ela ser vermelha?

- A) $\frac{1}{9}$
- B) $\frac{1}{6}$
- C) $\frac{1}{3}$
- D) $\frac{2}{3}$
- E) Nenhuma das opções acima.

### Probabilidades condicionais

- Dois eventos $A$ e $B$ podem acontecer. Suponha que sabemos que $A$ aconteceu, mas não sabemos se $B$ aconteceu.
- Se todos os resultados forem igualmente prováveis, então a probabilidade condicional de $B$ dado $A$ é:

$$
P(B \text{ dado } A) = P(B | A) =
= \frac{
\text{\# de resultados que satisfazem $A$ e $B$}
}{
\text{\# de resultados satisfatórios $A$}
}
$$
- Intuitivamente, isso é semelhante à definição da probabilidade regular de $B$, $P(B) = \frac{
\text{\# de resultados satisfatórios $B$}
}{
\text{número total de resultados}
}$, se você restringir o conjunto de resultados possíveis apenas aos do evento $A$.

### Verificação de conceito ✅

$$
P(B \text{ dado } A) = P(B | A)
= \frac{
\text{\# de resultados que satisfazem $A$ e $B$}
}{
\text{\# de resultados satisfatórios $A$}
}
$$

Eu jogo um dado de seis lados e não digo qual é o resultado, mas digo que é 3 ou menos. Qual é a probabilidade de o resultado ser par?

- A) $\frac{1}{2}$
- B) $\frac{1}{3}$
- C) $\frac{1}{4}$
- D) Nenhuma das opções acima.

### Probabilidade de que dois eventos aconteçam

- Suponha novamente que $A$ e $B$ são dois eventos e que todos os resultados são igualmente prováveis. Então, a probabilidade de que $A$ e $B$ ocorram é

$$
P(A \text{ e } B) = P(A, B) = \frac{
\text{\# de resultados que satisfazem $A$ e $B$}
}{
\text{número total de resultados}
}
$$

- **Exemplo 2:** Eu jogo um dado justo de seis lados. Qual é a probabilidade de o lançamento ser 3 ou menos e par?

### A regra da multiplicação

- A regra de multiplicação especifica como calcular a probabilidade de $A$ e $B$ acontecerem, mesmo que todos os resultados não sejam igualmente prováveis.

$$
P(A \text{ e } B)
=
P(A) \cdot P(B \text{ dado } A)
$$

- **Exemplo 2, novamente:** Eu jogo um dado justo de seis lados. Qual é a probabilidade de o lançamento ser 3 ou menos e par?

### E se $A$ não for afetado por $B$? 🤔

- A regra de multiplicação afirma que, para quaisquer dois eventos $A$ e $B$, $$P(A \text{ e } B) = P(A) \cdot P(B \text{ dado } A)$$
- E se saber que $A$ acontece não lhe disser nada sobre a probabilidade de $B$ acontecer?
- Suponha que joguemos uma moeda honesta três vezes.
- A probabilidade de o segundo lançamento dar cara não depende do resultado do primeiro lançamento.
- Então, o que é $P(A \text{ e } B)$?

### Eventos independentes

- Dois eventos $A$ e $B$ são independentes se $P(B \text{ dado } A) = P(B) \:$, ou equivalentemente se $$P(A \text{ e } B) = P (A) \cdot P(B)$$
- **Exemplo 3:** Suponha que temos uma moeda que é **viciada** e dá cara com probabilidade de 0,7. Cada lançamento é independente de todos os outros lançamentos. Nós viramos 5 vezes. Qual é a probabilidade de vermos 5 caras seguidas?

### Probabilidade de um evento *não* acontecer

- A probabilidade de $A$ **não** acontecer é $1 - P(A) \:$.
- Por exemplo, se a probabilidade de amanhã fazer sol é 0,85, então a probabilidade de não fazer sol amanhã é 0,15.

### Verificação de conceito ✅

Cada vez que ligo para minha avó 👵, a probabilidade de ela atender o telefone é $\frac{1}{3}$, independentemente para cada ligação. Se eu ligar três vezes para minha avó hoje, qual a chance de falar com ela pelo menos uma vez?

- A) $\frac{1}{3}$
- B) $\frac{2}{3}$
- C) $\frac{1}{2}$
- D)$1$
- E) Nenhuma das opções acima.

### Probabilidade de qualquer um dos dois eventos acontecer

- Suponha novamente que $A$ e $B$ são dois eventos e que todos os resultados são igualmente prováveis. Então, a probabilidade de que $A$ ou $B$ ocorram é

$$
P(A \text{ ou } B) = \frac{
\text{\# de resultados que satisfazem $A$ ou $B$}
}{
\text{número total de resultados}
}
$$

- **Exemplo 4:** Eu jogo um dado justo de seis lados. Qual é a probabilidade de o lançamento ser par ou pelo menos 5?

### A regra de adição

- Suponha que se $A$ acontecer, então $B$ não acontecerá, e se $B$ acontecer, então $A$ não acontecerá.
- Tais eventos são chamados de **mutuamente exclusivos** – eles **não têm sobreposição**.
- Se $A$ e $B$ são quaisquer dois eventos mutuamente exclusivos, então

$$P(A \text{ ou } B) = P(A) + P(B)$$

- **Exemplo 5:** Suponha que eu tenha duas moedas tendenciosas, a moeda $A$ e a moeda $B$. A moeda $A$ dá cara com probabilidade 0,6 e a moeda $B$ dá cara com probabilidade 0,3. Eu jogo as duas moedas uma vez. Qual é a probabilidade de eu ver duas faces diferentes?

### À parte: prova da regra de adição para eventos igualmente prováveis

Você não é obrigado a saber como “provar” nada neste curso; você pode achar isso interessante.

Se $A$ e $B$ são eventos que consistem em resultados igualmente prováveis ​​e, além disso, $A$ e $B$ são mutuamente exclusivos (o que significa que não têm sobreposição), então

$$
\begin{align*}
P(A \text{ ou } B)
&= \frac{
\text{\# de resultados que satisfazem $A$ ou $B$}
}{
\text{número total de resultados}
}
\\[1em]
&= \frac{
(\text{\# de resultados satisfatórios $A$})
+
(\text{\# de resultados satisfatórios $B$})
}{
\text{número total de resultados}
}
\\[1em]
&= \frac{
(\text{\# de resultados satisfatórios $A$})
}{
\text{número total de resultados}
}
+
\frac{
(\text{\# de resultados satisfatórios $B$})
}{
\text{número total de resultados}
}
\\[1em]
&= P(A) + P(B)
\end{align*}
$$

### Resumo, da próxima vez

- Probabilidade descreve a probabilidade de ocorrência de um evento.
- Existem várias regras para calcular probabilidades. Analisamos muitos casos especiais que envolviam eventos igualmente prováveis.
- Existem duas regras gerais a ter em conta:
- A **regra de multiplicação**, que afirma que para quaisquer dois eventos, $P(A \text{ e } B) = P(B \text{ dado } A) \cdot P(A) \:$.
- A **regra de adição**, que afirma que para quaisquer dois eventos **mutuamente exclusivos**, $P(A \text{ ou } B) = P(A) + P(B)$.
- **Próxima vez:** simulações.


```python
#In:

```
