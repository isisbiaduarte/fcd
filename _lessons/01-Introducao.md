---
layout: page
title: Introdução
nav_order: 1
---

# Aula 1 - Introdução

{: .no_toc .mb-2 }

Apresentando o curso.
{: .fs-6 .fw-300 }

{: .no_toc .text-delta }
Resultados Esperados

1. Entender um pouco de onde veio a ciência de dados
1. Receber as tarefas de configuração do ambiente
1. Ficar motivado!


```python
#In: 
# Imports
import babypandas as bpd
import numpy as np

import plotly.express as px
import matplotlib.pyplot as plt
plt.style.use('ggplot')
```

## Fundamentos de Ciência de Dados - 2024.1

### Bem-vindo ao curso de Ciência de Dados (UFMG)! 👋
- Hoje terermos uma visita guiada à ciência de dados.
- Nosso curso foi desenvolvido pela UC Berkeley em 2015 (data8.org).
- Em 2017 criamos a matéria de Introdução à Ciência de Dados (DCC212).
  - Vocês vão ver essa matéria
  - Porém o data8 era muito simples para o quarto período
- Voltamos com a ideia de ter o data8 no primeiro período no curso de Ciência de Dados
- Nos baseamos na abordagem de sucesso da UCSD (data10.com)
  - Também adaptada de Berkeley
- **Objetivo:** Aprenda programação e estatísticas suficientes para fazer ciência de dados.
  - Estatísticas sem muita matemática, principalmente simulação.
  - Estabelece as bases para todos os outros cursos do curso.

### Agenda

- Pessoal do curso.
- O que é ciência de dados?
- Como será esse curso?
- Demonstração com base na literatura.

## Equipe do curso

### Professor: Flavio Vinicius Diniz de Figueiredo (DCC)
- BSc em Ciência da Computação pela UFCG
- Mestrado e Doutorado em Ciência da Computação pela UFMG
  - Estudos parciais na Carnegie Mellon University e University of Brittish Columbia
- Já ensinou mais de 20 turmas de Introdução à Ciência de Dados (4o Período)
- Membro da comissão que fundou o curso
  - Em particula
### Professor: Uriel Moreira Silva (DEST)
- BS em Matemática e Ciência da Computação pela Loyola Maryland, PhD em Matemática (combinatória) pela UCSD 🔱.
- Ensino na UCSD: Matemática ➡️ CSE ➡️ DSC.
- 9ª vez ensinando DSC 10!
- Também ensine o DSC 40A com frequência.
- Interesses externos: artesanato, jogos de tabuleiro, caminhadas, panificação 🎂.

### TAs, Tutores e Mascote

Além disso, temos vários outros membros da equipe do curso que estão aqui para apoiá-lo nas discussões, no horário comercial e online.

- **2 Garaduate Tas**: Dayan Stockard e Dasha Varkus.
- **17 professores de graduação**: Gabriel Cha, Eric Chen, Charlie Gillet, Vanessa Hu, Dylan Lee, Anthony Li, Jasmine Lo, Linda Long, Aishani Mohapatra, Harshita Saha, Abel Seyoum, Selim Shaalan, Yutian (Skylar) Shi , Tony Ta, Zairan Xiang, Diego Zavalza e Luran (Lauren) Zhang,
- **1 mascote panda de pelúcia**: Bebê Panda. 🐼

Saiba mais sobre eles em [dsc10.com/staff](https://dsc10.com/staff).

## O que é “ciência de dados”? 🤔

<center><img src='01-Introducao/images/what-is-ds.png' width=1250>Todo mundo parece ter sua própria definição de ciência de dados.</center>

### O que é "ciência de dados"?

A ciência de dados trata de **tirar conclusões úteis a partir de dados usando computação**. Ao longo do trimestre, abordaremos vários aspectos da ciência de dados:

- Primeiras 4 semanas: use Python para **explorar** dados.
- Muita visualização 📈📊 e “manipulação de dados”, utilizando ferramentas padrão da indústria.

- Próximas 4 semanas: use dados para **inferir** sobre uma população, dada apenas uma amostra.
- Confie fortemente na simulação, em vez de fórmulas.

- Últimas duas semanas: use dados do passado para **prever** o que pode acontecer no futuro.
- Um gostinho de aprendizado de máquina 🤖.

### A ciência de dados é mais relevante do que nunca 🤧

Passamos alguns anos analisando gráficos como este:

<center><img src='01-Introducao/images/covid.jpg' width=75%></center>

### Também pode ser divertido!

<center><img src='01-Introducao/images/rapper_vocab.jpg' width=75%></center>

Do artigo de [The Pudding](https://pudding.cool/) em [The Pudding](https://pudding.cool/).

## Logística do curso

### Site do curso

O site do curso é o seu balcão único para todas as coisas relacionadas ao curso.

<center><h3><a href="https://flaviovdf.io/fcd">https://flaviovdf.io/fcd</a></h3></center>

É aqui que serão postadas palestras, trabalhos de casa, laboratórios, discussões e todos os outros conteúdos. **Verifique com frequência!!**

### Ambientes da UFMG

- **Moodle**: fórum de perguntas e respostas. Todos os anúncios serão feitos aqui.
- **Virtual Programming Lab**: onde você enviará todas as tarefas para avaliação automática. Dentro do moodle.

### Palestra

- As palestras serão presenciais!!
- A participação nunca será exigida, mas é incentivada. Caso você seja reprovado na matéria e tenha menos que 75% de presença, **reprovação por falta!!** (isso é ruim). Caso passe, não olhamos suas presenças.
- Os slides/código da aula serão vinculados ao site do curso, tanto em formato de código "executável" quanto em arquivo HTML (✏️), que você pode salvar como PDF e anotar em seu tablet.
- Tentaremos tornar as palestras envolventes. **Traga seu laptop ou tablet**, se tiver.

### Verificação de conceito ✅ – Resposta em [cc.dsc10.com](http://cc.dsc10.com)

**Quantos dos seguintes alimentos se qualificam como sanduíche?**

<center><img src='images/foods.png' width=30%></center>

UMA.0
B. 1
C. 2
D.3
E. 4             

_(Sempre usaremos o mesmo link para verificações de conceito, então você deve marcá-lo._)



### Discussão

- As seções de discussão foram elaboradas para que você pratique as **ideias conceituais** do curso.
- Todos os trabalhos desta aula serão feitos no computador através de código, mas os exames são em papel e presenciais.
- Para cada discussão, preparamos um conjunto de problemas, **composto de problemas de exames antigos** (ver [practice.dsc10.com](https://practice.dsc10.com)).
- Os conjuntos de problemas são publicados online, portanto traga um computador ou tablet para acessá-los. Mas, assim como nos exames, você responderá aos problemas **no papel**.
- Os conjuntos de problemas para discussão não são enviados a lugar nenhum.
- A participação não é obrigatória, porém **crédito extra** é fornecido pela participação.
- Se você comparecer, deverá comparecer à seção de discussão à qual foi designado.
- Assim como as palestras, as discussões serão podcastadas.

### Cronograma de Discussão

Cada aluno é designado para uma discussão específica da seguinte maneira. Você pode solicitar uma alteração de horário em [Beginning of Quarter Survey](https://forms.gle/EpXSJitSUFtBiNgN9). <span style="color:red"><b>🚨 Ignore a discussão e o tempo de laboratório listados na programação do seu curso.</b></span>

<center><img src='images/discussion.jpg' width=90%></center>

### Laboratórios

- Os laboratórios referem-se a **tarefas de laboratório**, que são uma parte obrigatória do curso e ajudam você a desenvolver fluência em Python e no trabalho com dados.
- Ao trabalhar nos laboratórios, você poderá executar **testes de autoavaliação** que informam se suas respostas estão corretas.
- Para laboratórios, se você passar em todos os testes do autonivelador, obterá 100\%!
- Você deve enviar laboratórios individualmente, mas pode discutir ideias com outras pessoas (sem compartilhamento de código).
- Os laboratórios geralmente acontecem aos **sábados às 23h59** no Gradescope. O primeiro laboratório (previsto para sábado, 14 de janeiro) terá instruções de envio.

### Trabalhos de casa e projetos

- As tarefas semanais de lição de casa baseiam-se nas habilidades que você desenvolve nos laboratórios.
- Uma diferença fundamental entre trabalhos de casa e laboratórios é que **passar nos testes de autoavaliação não garante uma pontuação perfeita!**
- Nos trabalhos de casa, temos “testes ocultos” que só são executados após o envio do trabalho.
- Os testes que lhe são disponibilizados no próprio trabalho apenas verificam se a sua resposta é razoável/no caminho certo.
- Novamente, você mesmo deve fazer as tarefas de casa, mas pode discutir ideias com outros alunos (sem compartilhar código).
- Os trabalhos de casa geralmente são entregues às **terças-feiras às 23h59** e enviados ao Gradescope.
- No **Projeto Intermediário** e no **Projeto Final**, você se aprofundará em um conjunto de dados! Os projetos são mais longos do que os trabalhos de casa, por isso damos-lhe mais tempo para trabalhar neles.
- Projetos deste trimestre: Restaurantes 🍔 e Great British Bake Off Great British Bake Off 👩‍🍳🍰.
- Você pode trabalhar em projetos com parceiros, seguindo estes [project partner guidelines](https://dsc10.com/project-partners). Vocês dois devem contribuir ativamente em **todas as partes** do projeto.

### Exames

Teremos dois exames neste trimestre.
- **Exame de meio de semestre**: sexta-feira, 10 de fevereiro, no horário agendado da aula.
- **Exame Final**: Sábado, 18 de março, das 15h às 18h.
- Ambos os exames serão realizados presencialmente e em papel. Deixe-nos saber se você tiver um conflito no [Beginning of Quarter Survey](https://forms.gle/EpXSJitSUFtBiNgN9).

### Leituras e recursos

- Faremos leituras de duas fontes. As leituras de cada palestra serão publicadas na página inicial do curso.
- [Computational and Inferential Thinking (CIT)](https://inferencialthinking.com), o livro criado para a versão deste curso em Berkeley.
- [`babypandas` notes](https://notes.dsc10.com), escrito especificamente para a primeira parte do DSC 10.
- A guia [Resources](https://dsc10.com/resources) do site do curso contém links para recursos úteis que você deseja usar ao longo do curso (por exemplo, folha de referência do DSC 10, tutoriais de programação, vídeos complementares).
- A guia [Debugging](https://dsc10.com/debugging) do site do curso contém respostas para muitos problemas técnicos comuns.

### Uma semana típica no DSC 10

| Domingo | Segunda-feira | Terça-feira | Quarta-feira | Quinta-feira | Sexta-feira | Sábado |
|: -- :|: -- :|: -- :|: -- :|: -- :|: -- :|: -- :|
| Nada! 😎 | Palestra | | Palestra | | Palestra | |
| | | | Discussão | | | |
| | | **HW devido** | | | | **Laboratório previsto** |

Consulte [Syllabus](https://dsc10.com/syllabus) para obter mais detalhes.

### Primeira tarefa
- O laboratório 0 será entregue **Sábado, 14 de janeiro às 23h59**.
- Será lançado ainda hoje
- <span style='color:red'><b>🚨 Importante: comece cedo e envie com frequência</b>.</span>

### Conseguindo ajuda
Este é um curso difícil e rápido, mas estamos aqui para ajudá-lo – veja como:

- **Horário de expediente (OH)**.
- Remoto e presencial durante toda a semana.
- Veja a programação e o link do Zoom no [Calendar 📆](https://dsc10.com/calendar).
- **EdStem**.
- Poste aqui qualquer dúvida logística ou conceitual (não envie por e-mail).
- Nenhum código ou soluções em postagens públicas. Essas postagens devem ser privadas para instrutores + funcionários.
- Caso contrário, poste publicamente (anonimamente, se desejar).
- <span style="color:red;"><b>🚨 Importante: use isso a seu favor!</b></span>

### Oportunidade: bootcamp Python

Diversity in Data Science, uma organização estudantil, está realizando um bootcamp Python de uma semana especificamente **para alunos do DSC 10 sem experiência anterior em programação**. O bootcamp é **esta semana**. Inscreva-se [here](https://docs.google.com/forms/d/e/1FAIpQLSdlRXdN6hzlC8IWGA8iuABDseRFkX22m2nATTQRJvkg_DTyPg/viewform).


<center><img src='images/bootcamp.png' width=40%></center>

### Conselhos de alunos anteriores

Ao final de cada trimestre, solicitamos aos alunos do DSC 10 que dêem conselhos aos futuros alunos do curso. Aqui estão algumas respostas:

> "Vá para o horário de expediente! Arranje um parceiro para o projeto mesmo que não queira. Se você não entende algum assunto tente o seguinte: vá para o horário de expediente, pergunte no [EdStem], confira as [leituras] , veja as notas da aula. Comece as tarefas mais cedo, tente terminar 2 dias antes para verificar seu trabalho.

> "Eu aconselharia que participasse das discussões e do horário de expediente sempre que possível, pois muitas vezes me descobri aprendendo coisas novas mesmo quando não vinha com uma pergunta pronta."

> "NÃO fique para trás nas palestras. Torna-se muito difícil acompanhar os conceitos. Vá para a seção de discussão! Ouvir um conceito explicado uma vez pode ser difícil de entender, então a seção de discussão foi extremamente útil."

> "Vá para o horário de expediente! É muito importante dar voz à voz. Converse com o professor após as aulas, compareça ao horário de expediente, poste suas dúvidas e faça uma pergunta."

### Colaboração

#### Fazer perguntas é altamente recomendável!
- Discuta todas as questões entre si (exceto exames).
- Envie tarefas de laboratório individualmente, mas você pode trabalhar com outras pessoas (sem compartilhamento de código).
- Envie trabalhos de casa individualmente, mas você pode discutir estratégias de resolução de problemas com outras pessoas (sem compartilhamento de código).
- Apresentar projetos individualmente ou em pares utilizando programação em pares.

#### Os limites da colaboração:
- Não compartilhem soluções entre si nem olhem o código de alguém.
- Os parceiros do projecto devem contribuir para todas as partes do projecto. Não divida o projeto.
- Violações de integridade acadêmica geralmente resultam em reprovação no curso.

### Estamos aqui para ajudá-lo!

Independentemente da sua formação, você pode ter sucesso neste curso. **Nenhuma experiência anterior em programação ou estatística será assumida!**

Assista no YouTube: [We’re All Data Scientists | Rebecca Nugent | TEDxCMU](https://www.youtube.com/watch?v=YMnqPTLoj7o).

### Recursos do campus

Serviços de Aconselhamento e Psicologia (CAPS) é uma unidade do campus que oferece “aconselhamento de curto prazo para questões acadêmicas, profissionais e pessoais e também oferece serviços de psiquiatria para circunstâncias em que a medicação pode ajudar no aconselhamento”.
Se você ou alguém que você conhece precisar de cuidados de saúde mental, entre em contato com o CAPS.

<center><h3><a href="https://caps.ucsd.edu/">caps.ucsd.edu</a></h3></center>

## Demonstração

### Próxima vez

- Na quarta-feira começaremos a programar 💻 em Python 🐍.
- Fique atento ao anúncio via EdStem sobre se a aula de quarta-feira será remota ou presencial. De qualquer forma, as seções de discussão presencial começarão na quarta-feira!
