---
layout: page
title: GroupBy
nav_order: 6
---
[<img src="./colab_favicon_small.png" style="float: right;">](https://colab.research.google.com/github/flaviovdf/fcd/blob/master/_lessons/06-GroupBy.ipynb)

# Tópico 6 – Mais consultas e GroupBy

Os dados de hoje estão [aqui (Afonso Pena)](https://raw.githubusercontent.com/flaviovdf/fcd/main/assets/06-GroupBy/data/afonso_pena.csv) e [aqui (IMDB)](https://raw.githubusercontent.com/flaviovdf/fcd/main/assets/06-GroupBy/data/imdb.csv)


```python
#In: 
# Descomente e execute as linhas a seguir para usar o BabyPandas e o PandasTutor
# (faça isso apenas se não tiver instalado ainda)
# ! pip install babypandas
# ! pip install pandas_tutor
```


```python
#In: 
import babypandas as bpd
import numpy as np


# Estamos importando o matplotlib pela primeira vez.
# Serve para fazer gráficos
import matplotlib.pyplot as plt
plt.style.use('ggplot')
```


```python
#In: 
# Esssas linhas servem para que o Pandas Tutor execute
%reload_ext pandas_tutor
%set_pandas_tutor_options {'projectorMode': True}
```



<script type="text/javascript" src="https://pandastutor.com/build/wsembed.bundle.2022-07-07-release.js"></script>
<script>
console.log("initializing pandas_tutor js")

function drawWsv(viz_id, spec, options) {
  if (typeof createWsvFromPandasTrace === 'undefined') {
    setTimeout(() => drawWsv(viz_id, spec, options), 2000) // retry in 2 seconds
    return
  }
  createWsvFromPandasTrace(viz_id, spec, options)
}
</script>



### Hoje

- Recapitulação: consultas.
- Consultas com múltiplas condições.
- Grupo por.
- Prática extra, incluindo problemas de desafio.

### Sobre os dados: Ainda estamos na feira da Afonso Pena

O DataFrame `df` contém dados dos feirantes da Afonso Pena


```python
#In: 
# para rodar no colab use 'https://raw.githubusercontent.com/flaviovdf/fcd/main/assets/06-GroupBy/data/afonso_pena.csv'
# i.e., df = bpd.read_csv('https://raw.githubusercontent.com/flaviovdf/fcd/main/assets/06-GroupBy/data/afonso_pena.csv')
df = bpd.read_csv('afonso_pena.csv')

produtos = df.get('NUMERO_PRODUTOS_CADASTRADOS')
area = df.get('AREA')

df = df.assign(
    DENSIDADE= produtos / area
)
df = df.set_index('ID_FEIRA_AFONSO_PENA_BARRACA')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>BOLSA DE BEBÊ, MALA DE MATERNIDADE, NECESSÁIRE...</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>BRINCO, ANEL, PULSEIRA, COLAR, ARCO</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, CONJUNTO, MACACÃO</td>
      <td>3.0</td>
      <td>11.838911</td>
      <td>0.253402</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>MACACÃO, BLUSA, SAPATINHO</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>87</th>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>BLUSA, BERMUDA, ROUPA DE GINÁSTICA, SAIA, VEST...</td>
      <td>7.0</td>
      <td>11.827592</td>
      <td>0.591836</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1350</th>
      <td>G.F4.V014</td>
      <td>BARRACA HELCIO LICIO SILVA</td>
      <td>HELCIO LICIO SILVA</td>
      <td>GRAZIELA CRISTINA RAMALHO SILVA</td>
      <td>Bijouterias</td>
      <td>ANEL, COLAR, BROCHE, BRINCO, PULSEIRA, ALIANÇA...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>1351</th>
      <td>J.F1.V004</td>
      <td>BARRACA DIEGO DOS SANTOS DIAS</td>
      <td>DIEGO DOS SANTOS DIAS</td>
      <td>CELSO DE SOUZA LINHARES</td>
      <td>Calçados</td>
      <td>RASTEIRINHA, SAPATO, SANDÁLIA</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>1352</th>
      <td>D.F4.V050</td>
      <td>BARRACA JAIR CORREA</td>
      <td>JAIR CORREA</td>
      <td>Keli Aparecida Batista Correa</td>
      <td>Vestuário</td>
      <td>VESTIDO DE MALHA, BATA DE TECIDO, CONJUNTO, SA...</td>
      <td>5.0</td>
      <td>11.827592</td>
      <td>0.422740</td>
    </tr>
    <tr>
      <th>1353</th>
      <td>G.F2.V010</td>
      <td>BARRACA SIDNEY FERNANDO KNEIPP SOARES</td>
      <td>SIDNEY FERNANDO KNEIPP SOARES</td>
      <td>ANA PAULA FAUSTINA DE SOUZA</td>
      <td>Bijouterias</td>
      <td>COLAR, GARGANTILHA, ARCO, ANEL, PASSADOR DE CA...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>1354</th>
      <td>G.F1.V061</td>
      <td>BARRACA REGINA GARCIA FERREIRA</td>
      <td>REGINA GARCIA FERREIRA</td>
      <td>Rejane Garcia Ferreira Clemente</td>
      <td>Bijouterias</td>
      <td>COLAR DE METAL, PRESILHA, PASSADOR DE CABELO, ...</td>
      <td>4.0</td>
      <td>11.827592</td>
      <td>0.338192</td>
    </tr>
  </tbody>
</table>
<p>1250 rows × 9 columns</p>
</div>



## Recapitulação: consultas

### O que é uma consulta? 🤔

- Uma "consulta" é um código que extrai linhas de um DataFrame para as quais determinadas condições são verdadeiras.
- Freqüentemente usamos consultas para _filtrar_ DataFrames para que contenham apenas as linhas que satisfaçam as condições declaradas em nossas perguntas.

### Como consultamos um DataFrame?

Para selecionar apenas determinadas linhas do DataFrame `df`:

1. Faça uma sequência (lista/array/Série) de `True`s (manter) e `False`s (jogar fora), geralmente fazendo uma comparação.
2. Em seguida, passe-o para `df[consulta]`.

### Comparações entre elementos

Existem vários tipos de comparações que podemos fazer.

|símbolo|significado|
|--------|--------|
|`==` |igual a |
|`!=` |diferente de |
|`<`|menos que|
|`<=`|menor ou igual a|
|`>`|maior que|
|`>=`|maior ou igual a|

## Exemplo 5: Qual o número de barracas de comida?

**Conceito principal**: consulta

### Estratégia

1. Consulta para extrair um DataFrame apenas das solicitações `'Alimentação'`.
2. Classifique por `'NUMERO_PRODUTOS_CADASTRADOS'` em ordem decrescente.
3. Extraia o primeiro elemento da coluna `'NOME_FEIRANTE'`.


```python
#In: 
comida = df[df.get('NOME_SETOR') == 'Alimentação']
comida.shape
```




    (100, 9)



### Qual o (**A PESSOA**) feirante que vende mais produtos de comida?


```python
#In: 
comida = comida.sort_values(by='NUMERO_PRODUTOS_CADASTRADOS', ascending=False)
comida
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>109</th>
      <td>Y.F1.V012</td>
      <td>BARRACA DAYSE PINTO NORBERTO</td>
      <td>DAYSE PINTO NORBERTO</td>
      <td>DJALMA ANTÔNIO DE FREITAS</td>
      <td>Alimentação</td>
      <td>CERVEJA, TORRESMO, CHIPS, AZEITONA, REFRIGERAN...</td>
      <td>21.0</td>
      <td>33.611057</td>
      <td>0.624794</td>
    </tr>
    <tr>
      <th>1406</th>
      <td>Z.F1.V007</td>
      <td>BARRACA FRANCINERE AMARAL CARDOSO RIBEIRO DE S...</td>
      <td>FRANCINERE AMARAL CARDOSO RIBEIRO DE SOUZA</td>
      <td>RAYKARD AGUIAR DE JESUS</td>
      <td>Alimentação</td>
      <td>CERVEJA, REFRIGERANTE, SUCO INDUSTRIALIZADO, E...</td>
      <td>20.0</td>
      <td>33.611058</td>
      <td>0.595042</td>
    </tr>
    <tr>
      <th>1015</th>
      <td>X.F1.V020</td>
      <td>BARRACA VERA LUIZA DE CARVALHO MACEDO</td>
      <td>VERA LUIZA DE CARVALHO MACEDO</td>
      <td>ORLANDINEIA ALVES</td>
      <td>Alimentação</td>
      <td>CERVEJA, SANDUÍCHE NATURAL, BISCOITO, BOLO, RO...</td>
      <td>17.0</td>
      <td>33.595966</td>
      <td>0.506013</td>
    </tr>
    <tr>
      <th>380</th>
      <td>Y.F1.V020</td>
      <td>BARRACA MARGARET LÚCIA DA COSTA SILVA</td>
      <td>MARGARET LÚCIA DA COSTA SILVA</td>
      <td>MARIA TRINDADE DECIOLA DE JESUS</td>
      <td>Alimentação</td>
      <td>ACARAJÉ, BOLINHO DE CARNE DE SOL, SUCO DE AÇAÍ...</td>
      <td>16.0</td>
      <td>33.539371</td>
      <td>0.477051</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Y.F2.V007</td>
      <td>BARRACA ANA CALDEIRA GOMES</td>
      <td>ANA CALDEIRA GOMES</td>
      <td>PAULO HENRIQUE DE JESUS CALDEIRA</td>
      <td>Alimentação</td>
      <td>CHURRASCO, PÃO DE QUEIJO, LEITE, REFRIGERANTE,...</td>
      <td>14.0</td>
      <td>33.611058</td>
      <td>0.416530</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1269</th>
      <td>X.F2.V013</td>
      <td>BARRACA FELIPE RODRIGUES ALVES DE DEUS</td>
      <td>FELIPE RODRIGUES ALVES DE DEUS</td>
      <td>SIDNEY ALVES DE DEUS</td>
      <td>Alimentação</td>
      <td>CHURRASCO</td>
      <td>1.0</td>
      <td>33.558728</td>
      <td>0.029799</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>X.F2.V011</td>
      <td>BARRACA WELLINGTON ALVES DE DEUS</td>
      <td>WELLINGTON ALVES DE DEUS</td>
      <td>SANDRO ALVES DE DEUS</td>
      <td>Alimentação</td>
      <td>CHURRASCO</td>
      <td>1.0</td>
      <td>33.595966</td>
      <td>0.029765</td>
    </tr>
    <tr>
      <th>1423</th>
      <td>X.F1.V003</td>
      <td>BARRACA CELIA APARECIDA MONTEIRO CIPRIANO</td>
      <td>CELIA APARECIDA MONTEIRO CIPRIANO</td>
      <td>APARECIDA SANTOS MONTEIRO</td>
      <td>Alimentação</td>
      <td>ACARAJÉ</td>
      <td>1.0</td>
      <td>33.697508</td>
      <td>0.029676</td>
    </tr>
    <tr>
      <th>261</th>
      <td>X.F2.V003</td>
      <td>BARRACA JOÃO GERALDO MARTINS DE SOUZA</td>
      <td>JOÃO GERALDO MARTINS DE SOUZA</td>
      <td>CHARLES DAVIDSON ROSA MARTINS</td>
      <td>Alimentação</td>
      <td>CHURRASCO</td>
      <td>1.0</td>
      <td>33.569063</td>
      <td>0.029789</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Y.F1.V003</td>
      <td>BARRACA CHAQUIBE HASSAN SOUKI HUNIOR</td>
      <td>CHAQUIBE HASSAN SOUKI HUNIOR</td>
      <td>GISMAR JOSÉ GOMES</td>
      <td>Alimentação</td>
      <td>DOCE DIVERSO</td>
      <td>1.0</td>
      <td>33.605972</td>
      <td>0.029757</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 9 columns</p>
</div>




```python
#In: 
# iloc pega a n-ésimo linha. não busca por índice, sim por linha
comida.get('NOME_FEIRANTE').iloc[0]
```




    'DAYSE PINTO NORBERTO'



### E se a condição não for satisfeita?


```python
#In: 
df[df.get('NOME_FEIRANTE') == 'FLAVIO FIGUEIREDO']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



### Verificação de conceito ✅

Qual expressão abaixo é avaliada como **o número total de produtos infantis**?

A. `df[df.get('NOME_SETOR') == 'Criança'].shape`

B. `df[df.get('NOME_SETOR') == 'Criança'].get('NUMERO_PRODUTOS_CADASTRADOS').sum()`

C.`df[df.get('NOME_SETOR') != 'Criança'].get('NUMERO_PRODUTOS_CADASTRADOS').sum()`

D. Mais de um dos itens acima.

### Atividade 🚘

**Pergunta**: Quais são os produtos vendidos pelo feirante que mais vende produtos infantis?

Escreva uma linha de código que avalie a resposta.


```python
#In: 
...
```




    Ellipsis



## Exemplo 6: Quantos feirantes vendem Vestuário Infantil e Produtos Infantis?

**Conceito principal**: Consultas com múltiplas condições.

### Múltiplas condições

- Para escrever uma consulta com múltiplas condições, use `&` para "e" e `|` para "ou".
- **Você deve usar `(`parênteses`)` em torno de cada condição!**
- ⚠️ Não use as palavras-chave Python `and` e `or` aqui! Eles não se comportam como você gostaria.
- Consulte [BPD 10.3](https://notes.dsc10.com/02-data_sets/querying.html#multiple-conditions) para obter uma explicação.


```python
#In: 
df[(df.get('NOME_SETOR') == 'Vestuário Infantil') | (df.get('NOME_SETOR') == 'Criança')]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>BOLSA DE BEBÊ, MALA DE MATERNIDADE, NECESSÁIRE...</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, CONJUNTO, MACACÃO</td>
      <td>3.0</td>
      <td>11.838911</td>
      <td>0.253402</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>MACACÃO, BLUSA, SAPATINHO</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>88</th>
      <td>E.F3.V016</td>
      <td>BARRACA CELINA MARIA  SILVA MARCELINO</td>
      <td>CELINA MARIA  SILVA MARCELINO</td>
      <td>ALBERTINO SERGIO MARCELINO</td>
      <td>Vestuário Infantil</td>
      <td>FANTASIA, BERMUDA, CALÇA, SAIA, VESTIDO, CAMISA</td>
      <td>6.0</td>
      <td>11.827592</td>
      <td>0.507288</td>
    </tr>
    <tr>
      <th>94</th>
      <td>E.F4.V029</td>
      <td>BARRACA CLARICE CONCEIÇÃO DAS GRAÇAS DE OLIVEI...</td>
      <td>CLARICE CONCEIÇÃO DAS GRAÇAS DE OLIVEIRA LIMA</td>
      <td>ANGELICA MARIA DE OLIVEIRA LIMA</td>
      <td>Vestuário Infantil</td>
      <td>CONJUNTO, SHORT, CUECA, CAMISA, CAMISETA, BLUS...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1426</th>
      <td>F.F3.V010</td>
      <td>BARRACA ALESSANDRA DE ABREU REIS</td>
      <td>ALESSANDRA DE ABREU REIS</td>
      <td>GLAUCIA HELENA DE ABREU TAVARES</td>
      <td>Criança</td>
      <td>ACESSÓRIOS PARA CACHORRO, ALMOFADA, CAMA DE TE...</td>
      <td>14.0</td>
      <td>11.838911</td>
      <td>1.182541</td>
    </tr>
    <tr>
      <th>1435</th>
      <td>E.F4.V006</td>
      <td>BARRACA CILDA LUZIA GUALBERTO</td>
      <td>CILDA LUZIA GUALBERTO</td>
      <td>CILMA MARIA GUALBERTO DE OLIVEIRA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, BOLERO, CUECA, BERMUDA, SAIA, BLUSA</td>
      <td>6.0</td>
      <td>11.827592</td>
      <td>0.507288</td>
    </tr>
    <tr>
      <th>1337</th>
      <td>F.F2.V004</td>
      <td>BARRACA MATHEUS PESSALI TIAGO BARBOSA</td>
      <td>MATHEUS PESSALI TIAGO BARBOSA</td>
      <td>MIRNA COSTA GONÇALVES</td>
      <td>Criança</td>
      <td>QUADRO, TOALHA FRALDA, TOALHA, BRINQUEDO PEDAG...</td>
      <td>6.0</td>
      <td>11.838911</td>
      <td>0.506803</td>
    </tr>
    <tr>
      <th>1342</th>
      <td>E.F1.V005</td>
      <td>BARRACA MAIRA FERNANDES DE MOURA</td>
      <td>MAIRA FERNANDES DE MOURA</td>
      <td>MAURÍCIO MARTINS TERRINHA</td>
      <td>Vestuário Infantil</td>
      <td>JARDINEIRA, VESTIDO, SHORT, SAIA, BLUSA</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>1343</th>
      <td>E.F1.V033</td>
      <td>BARRACA ELIANE CORREA JANSEN</td>
      <td>ELIANE CORREA JANSEN</td>
      <td>ANDREA CORREA JANSEN</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, TIARA, BLUSA, BODY, TOUCA, CALCINHA</td>
      <td>6.0</td>
      <td>11.838911</td>
      <td>0.506803</td>
    </tr>
  </tbody>
</table>
<p>224 rows × 9 columns</p>
</div>




```python
#In: 
# Dentro de de parênteses () ou de colchetes [] podemos quebrar a linha. ajuda na leitura
df[(df.get('NOME_SETOR') == 'Vestuário Infantil') | 
   (df.get('NOME_SETOR') == 'Criança')]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>BOLSA DE BEBÊ, MALA DE MATERNIDADE, NECESSÁIRE...</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, CONJUNTO, MACACÃO</td>
      <td>3.0</td>
      <td>11.838911</td>
      <td>0.253402</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>MACACÃO, BLUSA, SAPATINHO</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>88</th>
      <td>E.F3.V016</td>
      <td>BARRACA CELINA MARIA  SILVA MARCELINO</td>
      <td>CELINA MARIA  SILVA MARCELINO</td>
      <td>ALBERTINO SERGIO MARCELINO</td>
      <td>Vestuário Infantil</td>
      <td>FANTASIA, BERMUDA, CALÇA, SAIA, VESTIDO, CAMISA</td>
      <td>6.0</td>
      <td>11.827592</td>
      <td>0.507288</td>
    </tr>
    <tr>
      <th>94</th>
      <td>E.F4.V029</td>
      <td>BARRACA CLARICE CONCEIÇÃO DAS GRAÇAS DE OLIVEI...</td>
      <td>CLARICE CONCEIÇÃO DAS GRAÇAS DE OLIVEIRA LIMA</td>
      <td>ANGELICA MARIA DE OLIVEIRA LIMA</td>
      <td>Vestuário Infantil</td>
      <td>CONJUNTO, SHORT, CUECA, CAMISA, CAMISETA, BLUS...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1426</th>
      <td>F.F3.V010</td>
      <td>BARRACA ALESSANDRA DE ABREU REIS</td>
      <td>ALESSANDRA DE ABREU REIS</td>
      <td>GLAUCIA HELENA DE ABREU TAVARES</td>
      <td>Criança</td>
      <td>ACESSÓRIOS PARA CACHORRO, ALMOFADA, CAMA DE TE...</td>
      <td>14.0</td>
      <td>11.838911</td>
      <td>1.182541</td>
    </tr>
    <tr>
      <th>1435</th>
      <td>E.F4.V006</td>
      <td>BARRACA CILDA LUZIA GUALBERTO</td>
      <td>CILDA LUZIA GUALBERTO</td>
      <td>CILMA MARIA GUALBERTO DE OLIVEIRA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, BOLERO, CUECA, BERMUDA, SAIA, BLUSA</td>
      <td>6.0</td>
      <td>11.827592</td>
      <td>0.507288</td>
    </tr>
    <tr>
      <th>1337</th>
      <td>F.F2.V004</td>
      <td>BARRACA MATHEUS PESSALI TIAGO BARBOSA</td>
      <td>MATHEUS PESSALI TIAGO BARBOSA</td>
      <td>MIRNA COSTA GONÇALVES</td>
      <td>Criança</td>
      <td>QUADRO, TOALHA FRALDA, TOALHA, BRINQUEDO PEDAG...</td>
      <td>6.0</td>
      <td>11.838911</td>
      <td>0.506803</td>
    </tr>
    <tr>
      <th>1342</th>
      <td>E.F1.V005</td>
      <td>BARRACA MAIRA FERNANDES DE MOURA</td>
      <td>MAIRA FERNANDES DE MOURA</td>
      <td>MAURÍCIO MARTINS TERRINHA</td>
      <td>Vestuário Infantil</td>
      <td>JARDINEIRA, VESTIDO, SHORT, SAIA, BLUSA</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>1343</th>
      <td>E.F1.V033</td>
      <td>BARRACA ELIANE CORREA JANSEN</td>
      <td>ELIANE CORREA JANSEN</td>
      <td>ANDREA CORREA JANSEN</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, TIARA, BLUSA, BODY, TOUCA, CALCINHA</td>
      <td>6.0</td>
      <td>11.838911</td>
      <td>0.506803</td>
    </tr>
  </tbody>
</table>
<p>224 rows × 9 columns</p>
</div>



### Pergunta Original: Quantos feirante nos dois setores?


```python
#In: 
df[(df.get('NOME_SETOR') == 'Vestuário Infantil') | 
   (df.get('NOME_SETOR') == 'Criança')].shape[0]
```




    224




```python
#In: 
df.get('NOME_SETOR').unique()
```




    array(['Criança', 'Bijouterias', 'Vestuário Infantil', 'Vestuário',
           'Calçados', 'Alimentação', 'Cintos, Bolsas e Acessórios',
           'Artes e Pintura', 'Arranjos e Complementos',
           'Decoração e Utilidades', 'Cama, Mesa, Banho e Tapeçaria',
           'Mobilário, Flores, Arranjos, Cestaria', 'Esculturas'],
          dtype=object)



### Verificação de conceito ✅

Cada uma das perguntas a seguir pode ser respondida consultando o DataFrame `requests`.

1. Qual categoria tem mais produtos, 'Artes e Pintura' ou 'Esculturas'?
2. Quantos feirantes vendem 'Vestuário'?
3. Qual o nome do feirante que vende mais 'Vestuário'?
4. Quantas das perguntas acima **exigem** que a consulta tenha **múltiplas condições**?


Tente escrever o código para responder a cada pergunta.


```python
#In: 
...
```




    Ellipsis



### Selecionando linhas por posição com `.take`

- A consulta nos permite selecionar linhas que satisfaçam uma determinada _condição_.
- Também podemos selecionar linhas em _posições_ específicas com `.take([list_of_integer_positions])`. Isso mantém apenas as linhas cujas posições estão na lista especificada.
- Isso é análogo a usar `.iloc[]` em uma série.
- É raro precisar selecionar linhas por posição inteira. Consultar é **muito** mais útil.


```python
#In: 
df.take([1, 3, 5])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>BRINCO, ANEL, PULSEIRA, COLAR, ARCO</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>MACACÃO, BLUSA, SAPATINHO</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>88</th>
      <td>E.F3.V016</td>
      <td>BARRACA CELINA MARIA  SILVA MARCELINO</td>
      <td>CELINA MARIA  SILVA MARCELINO</td>
      <td>ALBERTINO SERGIO MARCELINO</td>
      <td>Vestuário Infantil</td>
      <td>FANTASIA, BERMUDA, CALÇA, SAIA, VESTIDO, CAMISA</td>
      <td>6.0</td>
      <td>11.827592</td>
      <td>0.507288</td>
    </tr>
  </tbody>
</table>
</div>




```python
#In: 
df.take(np.arange(5))
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>BOLSA DE BEBÊ, MALA DE MATERNIDADE, NECESSÁIRE...</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>BRINCO, ANEL, PULSEIRA, COLAR, ARCO</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, CONJUNTO, MACACÃO</td>
      <td>3.0</td>
      <td>11.838911</td>
      <td>0.253402</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>MACACÃO, BLUSA, SAPATINHO</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>87</th>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>BLUSA, BERMUDA, ROUPA DE GINÁSTICA, SAIA, VEST...</td>
      <td>7.0</td>
      <td>11.827592</td>
      <td>0.591836</td>
    </tr>
  </tbody>
</table>
</div>



## Exemplo 7: Qual é área todal de cada setor?

**Conceito chave**: Agrupamento por uma coluna.

### Organizando solicitações por bairro

- Podemos encontrar o número total da área por setor;
- Por exemplo, `df[df.get('NOME_SETOR') == 'Criança'].get('AREA').sum()`.
- Mas como podemos encontrar o total para **todos** os setores ao mesmo tempo?


```python
#In: 
df[df.get('NOME_SETOR') == 'Criança'].get('AREA').sum()
```




    1274.9974161428677




```python
#In: 
df[df.get('NOME_SETOR') == 'Vestuário Infantil'].get('AREA').sum()
```




    1431.87438966735



Parece que tem que haver uma maneira melhor. E aqui está!

### GroupBy: dividir, agregar e combinar

Observe o que acontece quando usamos o método `.groupby` em `requests` com o argumento `'neighborhood'`.


```python
#In: 
df.groupby('NOME_SETOR').sum()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>NOME_SETOR</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alimentação</th>
      <td>733.0</td>
      <td>3359.343283</td>
      <td>21.820242</td>
    </tr>
    <tr>
      <th>Arranjos e Complementos</th>
      <td>379.0</td>
      <td>615.329088</td>
      <td>32.028438</td>
    </tr>
    <tr>
      <th>Artes e Pintura</th>
      <td>62.0</td>
      <td>713.705731</td>
      <td>2.606116</td>
    </tr>
    <tr>
      <th>Bijouterias</th>
      <td>1047.0</td>
      <td>2165.445443</td>
      <td>88.479383</td>
    </tr>
    <tr>
      <th>Calçados</th>
      <td>774.0</td>
      <td>1881.447403</td>
      <td>65.409968</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Decoração e Utilidades</th>
      <td>423.0</td>
      <td>1033.677527</td>
      <td>29.927868</td>
    </tr>
    <tr>
      <th>Esculturas</th>
      <td>6.0</td>
      <td>47.580382</td>
      <td>0.252205</td>
    </tr>
    <tr>
      <th>Mobilário, Flores, Arranjos, Cestaria</th>
      <td>247.0</td>
      <td>771.488214</td>
      <td>14.012735</td>
    </tr>
    <tr>
      <th>Vestuário</th>
      <td>1454.0</td>
      <td>2591.442504</td>
      <td>122.878561</td>
    </tr>
    <tr>
      <th>Vestuário Infantil</th>
      <td>788.0</td>
      <td>1431.874390</td>
      <td>66.590892</td>
    </tr>
  </tbody>
</table>
<p>13 rows × 3 columns</p>
</div>



Observe que as contagens “totais” antes. O que acabou de acontecer? 🤯

### Um exemplo ilustrativo: Animais de estimação 🐱 🐶🐹

Considere os `animais de estimação` do DataFrame mostrados abaixo.



```python
#In: 
pets = bpd.DataFrame().assign(
    Species=['dog', 'cat', 'cat', 'dog', 'dog', 'hamster'],
    Color=['black', 'golden', 'black', 'white', 'golden', 'golden'],
    Weight=[40, 15, 20, 80, 25, 1],
    Age=[5, 8, 9, 2, 0.5, 3]
)
pets
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Species</th>
      <th>Color</th>
      <th>Weight</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>dog</td>
      <td>black</td>
      <td>40</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>cat</td>
      <td>golden</td>
      <td>15</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>cat</td>
      <td>black</td>
      <td>20</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>dog</td>
      <td>white</td>
      <td>80</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>dog</td>
      <td>golden</td>
      <td>25</td>
      <td>0.5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>hamster</td>
      <td>golden</td>
      <td>1</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>



### Visualizando `pets.groupby('Species').mean()`

1. **Divida** as linhas de `animais de estimação` em "grupos" de acordo com seus valores na coluna `'Espécies'`.
2. **Agregue** as linhas com o mesmo valor de `'Species'` tomando a `média` de todas as colunas numéricas.
3. **Combine** estes meios em um novo DataFrame que é indexado por `'Species'` e classificado por `'Species'` em ordem crescente.

Observe que o resultado contém apenas uma linha para gatos, uma linha para cães e uma linha para hamsters!


```python
#In: 
%%pt

pets.groupby('Species').mean()
```



<div class="pt-viz" id="pt-viz-0"></div>
<script>
drawWsv('#pt-viz-0', "{\n  \"code\": \"\\npets.groupby('Species').mean()\\n\",\n  \"explanation\": [\n    {\n      \"type\": \"GroupByCall\",\n      \"code_step\": \"pets.groupby('Species').mean()\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 4\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 23\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"using\",\n          \"pos\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"column\",\n            \"label\": \"Species\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Species\",\n              \"Color\",\n              \"Weight\",\n              \"Age\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              0,\n              1,\n              2,\n              3,\n              4,\n              5\n            ]\n          },\n          \"data\": [\n            [\n              \"dog\",\n              \"black\",\n              40,\n              5.0\n            ],\n            [\n              \"cat\",\n              \"golden\",\n              15,\n              8.0\n            ],\n            [\n              \"cat\",\n              \"black\",\n              20,\n              9.0\n            ],\n            [\n              \"dog\",\n              \"white\",\n              80,\n              2.0\n            ],\n            [\n              \"dog\",\n              \"golden\",\n              25,\n              0.5\n            ],\n            [\n              \"hamster\",\n              \"golden\",\n              1,\n              3.0\n            ]\n          ]\n        },\n        \"rhs\": {\n          \"type\": \"DataFrameGroupBy\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Species\",\n              \"Color\",\n              \"Weight\",\n              \"Age\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              0,\n              1,\n              2,\n              3,\n              4,\n              5\n            ]\n          },\n          \"data\": [\n            [\n              \"dog\",\n              \"black\",\n              40,\n              5.0\n            ],\n            [\n              \"cat\",\n              \"golden\",\n              15,\n              8.0\n            ],\n            [\n              \"cat\",\n              \"black\",\n              20,\n              9.0\n            ],\n            [\n              \"dog\",\n              \"white\",\n              80,\n              2.0\n            ],\n            [\n              \"dog\",\n              \"golden\",\n              25,\n              0.5\n            ],\n            [\n              \"hamster\",\n              \"golden\",\n              1,\n              3.0\n            ]\n          ],\n          \"group_data\": {\n            \"columns\": [\n              \"Species\"\n            ],\n            \"groups\": [\n              {\n                \"name\": [\n                  \"cat\"\n                ],\n                \"labels\": [\n                  1,\n                  2\n                ]\n              },\n              {\n                \"name\": [\n                  \"dog\"\n                ],\n                \"labels\": [\n                  0,\n                  3,\n                  4\n                ]\n              },\n              {\n                \"name\": [\n                  \"hamster\"\n                ],\n                \"labels\": [\n                  5\n                ]\n              }\n            ]\n          }\n        }\n      }\n    },\n    {\n      \"type\": \"AggCall\",\n      \"code_step\": \"pets.groupby('Species').mean()\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 23\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 30\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 1\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"cat\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 2\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"cat\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 0\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"dog\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 3\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"dog\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 4\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"dog\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 5\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"hamster\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": \"prev_rhs\",\n        \"rhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Weight\",\n              \"Age\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              \"Species\"\n            ],\n            \"labels\": [\n              \"cat\",\n              \"dog\",\n              \"hamster\"\n            ]\n          },\n          \"data\": [\n            [\n              17.5,\n              8.5\n            ],\n            [\n              48.333333333333336,\n              2.5\n            ],\n            [\n              1.0,\n              3.0\n            ]\n          ]\n        }\n      }\n    }\n  ]\n}", {"nohover": true, "maxDisplayRows": 7, "maxDisplayCols": 5, "colorPalette": ["#e41a1c", "#377eb8", "#4daf4a", "#984ea3", "#ff7f00", "#a65628", "#f781bf", "#999999"]});
</script>



### Tutor de Pandas

- Na última célula, vimos não apenas a saída do código, mas uma visualização do funcionamento interno do código.
- Isso graças ao Pandas Tutor, uma nova ferramenta desenvolvida por [Sam Lau](https://www.samlau.me), que ministrou este curso durante o verão.
- Pandas Tutor desenha diagramas para explicar o código `pandas` (e `babypandas`).
- Adicione `%%pt` ao topo de uma célula de código para explicar a última linha do código `babypandas`.
- Isso requer a importação do Pandas Tutor, o que já fizemos neste notebook.
- Você também pode usar o Pandas Tutor através de seu site, [pandastutor.com](https://pandastutor.com/).


```python
#In: 
# Without Pandas Tutor
pets.groupby('Species').mean()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Weight</th>
      <th>Age</th>
    </tr>
    <tr>
      <th>Species</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>cat</th>
      <td>17.500000</td>
      <td>8.5</td>
    </tr>
    <tr>
      <th>dog</th>
      <td>48.333333</td>
      <td>2.5</td>
    </tr>
    <tr>
      <th>hamster</th>
      <td>1.000000</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#In: 
%%pt

# With Pandas Tutor
pets.groupby('Species').mean()
```



<div class="pt-viz" id="pt-viz-1"></div>
<script>
drawWsv('#pt-viz-1', "{\n  \"code\": \"\\n# With Pandas Tutor\\npets.groupby('Species').mean()\\n\",\n  \"explanation\": [\n    {\n      \"type\": \"GroupByCall\",\n      \"code_step\": \"pets.groupby('Species').mean()\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 4\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 23\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"using\",\n          \"pos\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"column\",\n            \"label\": \"Species\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Species\",\n              \"Color\",\n              \"Weight\",\n              \"Age\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              0,\n              1,\n              2,\n              3,\n              4,\n              5\n            ]\n          },\n          \"data\": [\n            [\n              \"dog\",\n              \"black\",\n              40,\n              5.0\n            ],\n            [\n              \"cat\",\n              \"golden\",\n              15,\n              8.0\n            ],\n            [\n              \"cat\",\n              \"black\",\n              20,\n              9.0\n            ],\n            [\n              \"dog\",\n              \"white\",\n              80,\n              2.0\n            ],\n            [\n              \"dog\",\n              \"golden\",\n              25,\n              0.5\n            ],\n            [\n              \"hamster\",\n              \"golden\",\n              1,\n              3.0\n            ]\n          ]\n        },\n        \"rhs\": {\n          \"type\": \"DataFrameGroupBy\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Species\",\n              \"Color\",\n              \"Weight\",\n              \"Age\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              0,\n              1,\n              2,\n              3,\n              4,\n              5\n            ]\n          },\n          \"data\": [\n            [\n              \"dog\",\n              \"black\",\n              40,\n              5.0\n            ],\n            [\n              \"cat\",\n              \"golden\",\n              15,\n              8.0\n            ],\n            [\n              \"cat\",\n              \"black\",\n              20,\n              9.0\n            ],\n            [\n              \"dog\",\n              \"white\",\n              80,\n              2.0\n            ],\n            [\n              \"dog\",\n              \"golden\",\n              25,\n              0.5\n            ],\n            [\n              \"hamster\",\n              \"golden\",\n              1,\n              3.0\n            ]\n          ],\n          \"group_data\": {\n            \"columns\": [\n              \"Species\"\n            ],\n            \"groups\": [\n              {\n                \"name\": [\n                  \"cat\"\n                ],\n                \"labels\": [\n                  1,\n                  2\n                ]\n              },\n              {\n                \"name\": [\n                  \"dog\"\n                ],\n                \"labels\": [\n                  0,\n                  3,\n                  4\n                ]\n              },\n              {\n                \"name\": [\n                  \"hamster\"\n                ],\n                \"labels\": [\n                  5\n                ]\n              }\n            ]\n          }\n        }\n      }\n    },\n    {\n      \"type\": \"AggCall\",\n      \"code_step\": \"pets.groupby('Species').mean()\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 23\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 30\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 1\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"cat\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 2\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"cat\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 0\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"dog\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 3\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"dog\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 4\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"dog\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 5\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"hamster\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": \"prev_rhs\",\n        \"rhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Weight\",\n              \"Age\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              \"Species\"\n            ],\n            \"labels\": [\n              \"cat\",\n              \"dog\",\n              \"hamster\"\n            ]\n          },\n          \"data\": [\n            [\n              17.5,\n              8.5\n            ],\n            [\n              48.333333333333336,\n              2.5\n            ],\n            [\n              1.0,\n              3.0\n            ]\n          ]\n        }\n      }\n    }\n  ]\n}", {"nohover": true, "maxDisplayRows": 7, "maxDisplayCols": 5, "colorPalette": ["#e41a1c", "#377eb8", "#4daf4a", "#984ea3", "#ff7f00", "#a65628", "#f781bf", "#999999"]});
</script>



### Voltar para solicitações de serviço Get It Done 👷


```python
#In: 
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>BOLSA DE BEBÊ, MALA DE MATERNIDADE, NECESSÁIRE...</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>BRINCO, ANEL, PULSEIRA, COLAR, ARCO</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, CONJUNTO, MACACÃO</td>
      <td>3.0</td>
      <td>11.838911</td>
      <td>0.253402</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>MACACÃO, BLUSA, SAPATINHO</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>87</th>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>BLUSA, BERMUDA, ROUPA DE GINÁSTICA, SAIA, VEST...</td>
      <td>7.0</td>
      <td>11.827592</td>
      <td>0.591836</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1350</th>
      <td>G.F4.V014</td>
      <td>BARRACA HELCIO LICIO SILVA</td>
      <td>HELCIO LICIO SILVA</td>
      <td>GRAZIELA CRISTINA RAMALHO SILVA</td>
      <td>Bijouterias</td>
      <td>ANEL, COLAR, BROCHE, BRINCO, PULSEIRA, ALIANÇA...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>1351</th>
      <td>J.F1.V004</td>
      <td>BARRACA DIEGO DOS SANTOS DIAS</td>
      <td>DIEGO DOS SANTOS DIAS</td>
      <td>CELSO DE SOUZA LINHARES</td>
      <td>Calçados</td>
      <td>RASTEIRINHA, SAPATO, SANDÁLIA</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>1352</th>
      <td>D.F4.V050</td>
      <td>BARRACA JAIR CORREA</td>
      <td>JAIR CORREA</td>
      <td>Keli Aparecida Batista Correa</td>
      <td>Vestuário</td>
      <td>VESTIDO DE MALHA, BATA DE TECIDO, CONJUNTO, SA...</td>
      <td>5.0</td>
      <td>11.827592</td>
      <td>0.422740</td>
    </tr>
    <tr>
      <th>1353</th>
      <td>G.F2.V010</td>
      <td>BARRACA SIDNEY FERNANDO KNEIPP SOARES</td>
      <td>SIDNEY FERNANDO KNEIPP SOARES</td>
      <td>ANA PAULA FAUSTINA DE SOUZA</td>
      <td>Bijouterias</td>
      <td>COLAR, GARGANTILHA, ARCO, ANEL, PASSADOR DE CA...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>1354</th>
      <td>G.F1.V061</td>
      <td>BARRACA REGINA GARCIA FERREIRA</td>
      <td>REGINA GARCIA FERREIRA</td>
      <td>Rejane Garcia Ferreira Clemente</td>
      <td>Bijouterias</td>
      <td>COLAR DE METAL, PRESILHA, PASSADOR DE CABELO, ...</td>
      <td>4.0</td>
      <td>11.827592</td>
      <td>0.338192</td>
    </tr>
  </tbody>
</table>
<p>1250 rows × 9 columns</p>
</div>




```python
#In: 
df.groupby('NOME_SETOR').sum()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>NOME_SETOR</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alimentação</th>
      <td>733.0</td>
      <td>3359.343283</td>
      <td>21.820242</td>
    </tr>
    <tr>
      <th>Arranjos e Complementos</th>
      <td>379.0</td>
      <td>615.329088</td>
      <td>32.028438</td>
    </tr>
    <tr>
      <th>Artes e Pintura</th>
      <td>62.0</td>
      <td>713.705731</td>
      <td>2.606116</td>
    </tr>
    <tr>
      <th>Bijouterias</th>
      <td>1047.0</td>
      <td>2165.445443</td>
      <td>88.479383</td>
    </tr>
    <tr>
      <th>Calçados</th>
      <td>774.0</td>
      <td>1881.447403</td>
      <td>65.409968</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Decoração e Utilidades</th>
      <td>423.0</td>
      <td>1033.677527</td>
      <td>29.927868</td>
    </tr>
    <tr>
      <th>Esculturas</th>
      <td>6.0</td>
      <td>47.580382</td>
      <td>0.252205</td>
    </tr>
    <tr>
      <th>Mobilário, Flores, Arranjos, Cestaria</th>
      <td>247.0</td>
      <td>771.488214</td>
      <td>14.012735</td>
    </tr>
    <tr>
      <th>Vestuário</th>
      <td>1454.0</td>
      <td>2591.442504</td>
      <td>122.878561</td>
    </tr>
    <tr>
      <th>Vestuário Infantil</th>
      <td>788.0</td>
      <td>1431.874390</td>
      <td>66.590892</td>
    </tr>
  </tbody>
</table>
<p>13 rows × 3 columns</p>
</div>



### Usando `.groupby` em geral

Resumindo, `.groupby` agrega todas as linhas com o mesmo valor em uma coluna especificada (por exemplo, `'NOME_SETOR'`) em uma única linha no DataFrame resultante, usando um método de agregação (por exemplo, `.sum()`) para combinar valores.

1. **Escolha uma coluna para agrupar**.
- `.groupby(column_name)` reunirá linhas que possuem o mesmo valor na coluna especificada (`column_name`).
- No slide anterior agrupamos por `'NOME_SETOR'`.
2. **Escolha um método de agregação**.
- O método de agregação será aplicado **dentro** de cada grupo.
- No exemplo anterior, aplicamos o método `.sum()` a cada `'NOME_SETOR'`.
- O método de agregação é aplicado individualmente a cada coluna
- Se não fizer sentido usar o método de agregação em uma coluna, a coluna será eliminada da saída – veremos isso com mais detalhes em breve (Tipo o nome do feirante).
- Métodos de agregação comuns incluem `.count()`, `.sum()`, `.mean()`, `.median()`, `.max()` e `.min()`.

### Observação #1

- O índice mudou para nomes de SETORES.
- Em geral, os novos rótulos de linha são os *rótulos de grupo* (ou seja, os valores exclusivos na coluna em que agrupamos).


```python
#In: 
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>BOLSA DE BEBÊ, MALA DE MATERNIDADE, NECESSÁIRE...</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>BRINCO, ANEL, PULSEIRA, COLAR, ARCO</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, CONJUNTO, MACACÃO</td>
      <td>3.0</td>
      <td>11.838911</td>
      <td>0.253402</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>MACACÃO, BLUSA, SAPATINHO</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>87</th>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>BLUSA, BERMUDA, ROUPA DE GINÁSTICA, SAIA, VEST...</td>
      <td>7.0</td>
      <td>11.827592</td>
      <td>0.591836</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1350</th>
      <td>G.F4.V014</td>
      <td>BARRACA HELCIO LICIO SILVA</td>
      <td>HELCIO LICIO SILVA</td>
      <td>GRAZIELA CRISTINA RAMALHO SILVA</td>
      <td>Bijouterias</td>
      <td>ANEL, COLAR, BROCHE, BRINCO, PULSEIRA, ALIANÇA...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>1351</th>
      <td>J.F1.V004</td>
      <td>BARRACA DIEGO DOS SANTOS DIAS</td>
      <td>DIEGO DOS SANTOS DIAS</td>
      <td>CELSO DE SOUZA LINHARES</td>
      <td>Calçados</td>
      <td>RASTEIRINHA, SAPATO, SANDÁLIA</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>1352</th>
      <td>D.F4.V050</td>
      <td>BARRACA JAIR CORREA</td>
      <td>JAIR CORREA</td>
      <td>Keli Aparecida Batista Correa</td>
      <td>Vestuário</td>
      <td>VESTIDO DE MALHA, BATA DE TECIDO, CONJUNTO, SA...</td>
      <td>5.0</td>
      <td>11.827592</td>
      <td>0.422740</td>
    </tr>
    <tr>
      <th>1353</th>
      <td>G.F2.V010</td>
      <td>BARRACA SIDNEY FERNANDO KNEIPP SOARES</td>
      <td>SIDNEY FERNANDO KNEIPP SOARES</td>
      <td>ANA PAULA FAUSTINA DE SOUZA</td>
      <td>Bijouterias</td>
      <td>COLAR, GARGANTILHA, ARCO, ANEL, PASSADOR DE CA...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>1354</th>
      <td>G.F1.V061</td>
      <td>BARRACA REGINA GARCIA FERREIRA</td>
      <td>REGINA GARCIA FERREIRA</td>
      <td>Rejane Garcia Ferreira Clemente</td>
      <td>Bijouterias</td>
      <td>COLAR DE METAL, PRESILHA, PASSADOR DE CABELO, ...</td>
      <td>4.0</td>
      <td>11.827592</td>
      <td>0.338192</td>
    </tr>
  </tbody>
</table>
<p>1250 rows × 9 columns</p>
</div>




```python
#In: 
df.groupby('NOME_SETOR').sum()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>NOME_SETOR</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alimentação</th>
      <td>733.0</td>
      <td>3359.343283</td>
      <td>21.820242</td>
    </tr>
    <tr>
      <th>Arranjos e Complementos</th>
      <td>379.0</td>
      <td>615.329088</td>
      <td>32.028438</td>
    </tr>
    <tr>
      <th>Artes e Pintura</th>
      <td>62.0</td>
      <td>713.705731</td>
      <td>2.606116</td>
    </tr>
    <tr>
      <th>Bijouterias</th>
      <td>1047.0</td>
      <td>2165.445443</td>
      <td>88.479383</td>
    </tr>
    <tr>
      <th>Calçados</th>
      <td>774.0</td>
      <td>1881.447403</td>
      <td>65.409968</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Decoração e Utilidades</th>
      <td>423.0</td>
      <td>1033.677527</td>
      <td>29.927868</td>
    </tr>
    <tr>
      <th>Esculturas</th>
      <td>6.0</td>
      <td>47.580382</td>
      <td>0.252205</td>
    </tr>
    <tr>
      <th>Mobilário, Flores, Arranjos, Cestaria</th>
      <td>247.0</td>
      <td>771.488214</td>
      <td>14.012735</td>
    </tr>
    <tr>
      <th>Vestuário</th>
      <td>1454.0</td>
      <td>2591.442504</td>
      <td>122.878561</td>
    </tr>
    <tr>
      <th>Vestuário Infantil</th>
      <td>788.0</td>
      <td>1431.874390</td>
      <td>66.590892</td>
    </tr>
  </tbody>
</table>
<p>13 rows × 3 columns</p>
</div>



### Observação #2

A coluna `'NOME_FANTASIA'` e vároas outras desapareceram. Por que?


```python
#In: 
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>PRODUTOS</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83</th>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>BOLSA DE BEBÊ, MALA DE MATERNIDADE, NECESSÁIRE...</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>BRINCO, ANEL, PULSEIRA, COLAR, ARCO</td>
      <td>5.0</td>
      <td>11.838911</td>
      <td>0.422336</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>VESTIDO, CONJUNTO, MACACÃO</td>
      <td>3.0</td>
      <td>11.838911</td>
      <td>0.253402</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>MACACÃO, BLUSA, SAPATINHO</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>87</th>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>BLUSA, BERMUDA, ROUPA DE GINÁSTICA, SAIA, VEST...</td>
      <td>7.0</td>
      <td>11.827592</td>
      <td>0.591836</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1350</th>
      <td>G.F4.V014</td>
      <td>BARRACA HELCIO LICIO SILVA</td>
      <td>HELCIO LICIO SILVA</td>
      <td>GRAZIELA CRISTINA RAMALHO SILVA</td>
      <td>Bijouterias</td>
      <td>ANEL, COLAR, BROCHE, BRINCO, PULSEIRA, ALIANÇA...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>1351</th>
      <td>J.F1.V004</td>
      <td>BARRACA DIEGO DOS SANTOS DIAS</td>
      <td>DIEGO DOS SANTOS DIAS</td>
      <td>CELSO DE SOUZA LINHARES</td>
      <td>Calçados</td>
      <td>RASTEIRINHA, SAPATO, SANDÁLIA</td>
      <td>3.0</td>
      <td>11.827592</td>
      <td>0.253644</td>
    </tr>
    <tr>
      <th>1352</th>
      <td>D.F4.V050</td>
      <td>BARRACA JAIR CORREA</td>
      <td>JAIR CORREA</td>
      <td>Keli Aparecida Batista Correa</td>
      <td>Vestuário</td>
      <td>VESTIDO DE MALHA, BATA DE TECIDO, CONJUNTO, SA...</td>
      <td>5.0</td>
      <td>11.827592</td>
      <td>0.422740</td>
    </tr>
    <tr>
      <th>1353</th>
      <td>G.F2.V010</td>
      <td>BARRACA SIDNEY FERNANDO KNEIPP SOARES</td>
      <td>SIDNEY FERNANDO KNEIPP SOARES</td>
      <td>ANA PAULA FAUSTINA DE SOUZA</td>
      <td>Bijouterias</td>
      <td>COLAR, GARGANTILHA, ARCO, ANEL, PASSADOR DE CA...</td>
      <td>7.0</td>
      <td>11.838911</td>
      <td>0.591271</td>
    </tr>
    <tr>
      <th>1354</th>
      <td>G.F1.V061</td>
      <td>BARRACA REGINA GARCIA FERREIRA</td>
      <td>REGINA GARCIA FERREIRA</td>
      <td>Rejane Garcia Ferreira Clemente</td>
      <td>Bijouterias</td>
      <td>COLAR DE METAL, PRESILHA, PASSADOR DE CABELO, ...</td>
      <td>4.0</td>
      <td>11.827592</td>
      <td>0.338192</td>
    </tr>
  </tbody>
</table>
<p>1250 rows × 9 columns</p>
</div>




```python
#In: 
df.groupby('NOME_SETOR').sum()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>NOME_SETOR</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alimentação</th>
      <td>733.0</td>
      <td>3359.343283</td>
      <td>21.820242</td>
    </tr>
    <tr>
      <th>Arranjos e Complementos</th>
      <td>379.0</td>
      <td>615.329088</td>
      <td>32.028438</td>
    </tr>
    <tr>
      <th>Artes e Pintura</th>
      <td>62.0</td>
      <td>713.705731</td>
      <td>2.606116</td>
    </tr>
    <tr>
      <th>Bijouterias</th>
      <td>1047.0</td>
      <td>2165.445443</td>
      <td>88.479383</td>
    </tr>
    <tr>
      <th>Calçados</th>
      <td>774.0</td>
      <td>1881.447403</td>
      <td>65.409968</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Decoração e Utilidades</th>
      <td>423.0</td>
      <td>1033.677527</td>
      <td>29.927868</td>
    </tr>
    <tr>
      <th>Esculturas</th>
      <td>6.0</td>
      <td>47.580382</td>
      <td>0.252205</td>
    </tr>
    <tr>
      <th>Mobilário, Flores, Arranjos, Cestaria</th>
      <td>247.0</td>
      <td>771.488214</td>
      <td>14.012735</td>
    </tr>
    <tr>
      <th>Vestuário</th>
      <td>1454.0</td>
      <td>2591.442504</td>
      <td>122.878561</td>
    </tr>
    <tr>
      <th>Vestuário Infantil</th>
      <td>788.0</td>
      <td>1431.874390</td>
      <td>66.590892</td>
    </tr>
  </tbody>
</table>
<p>13 rows × 3 columns</p>
</div>



### Colunas desaparecendo ✨🐇🎩

- O método de agregação – `.sum()`, neste caso – é aplicado a cada coluna.
- Se não fizer sentido aplicá-lo a uma determinada coluna, essa coluna desaparecerá.
- Por exemplo, não podemos somar strings, como na coluna `'NOME_FANTASIA'`.

### Observação #3

- O método de agregação é aplicado a cada coluna **separadamente**.
- As linhas do DataFrame resultante precisam ser interpretadas com cuidado.


```python
#In: 
df.groupby('NOME_SETOR').mean()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
      <th>AREA</th>
      <th>DENSIDADE</th>
    </tr>
    <tr>
      <th>NOME_SETOR</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alimentação</th>
      <td>7.330000</td>
      <td>33.593433</td>
      <td>0.218202</td>
    </tr>
    <tr>
      <th>Arranjos e Complementos</th>
      <td>7.288462</td>
      <td>11.833252</td>
      <td>0.615931</td>
    </tr>
    <tr>
      <th>Artes e Pintura</th>
      <td>2.066667</td>
      <td>23.790191</td>
      <td>0.086871</td>
    </tr>
    <tr>
      <th>Bijouterias</th>
      <td>5.721311</td>
      <td>11.833035</td>
      <td>0.483494</td>
    </tr>
    <tr>
      <th>Calçados</th>
      <td>4.867925</td>
      <td>11.833003</td>
      <td>0.411383</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Decoração e Utilidades</th>
      <td>6.130435</td>
      <td>14.980834</td>
      <td>0.433737</td>
    </tr>
    <tr>
      <th>Esculturas</th>
      <td>3.000000</td>
      <td>23.790191</td>
      <td>0.126102</td>
    </tr>
    <tr>
      <th>Mobilário, Flores, Arranjos, Cestaria</th>
      <td>5.880952</td>
      <td>18.368767</td>
      <td>0.333637</td>
    </tr>
    <tr>
      <th>Vestuário</th>
      <td>6.639269</td>
      <td>11.833071</td>
      <td>0.561089</td>
    </tr>
    <tr>
      <th>Vestuário Infantil</th>
      <td>6.512397</td>
      <td>11.833673</td>
      <td>0.550338</td>
    </tr>
  </tbody>
</table>
<p>13 rows × 3 columns</p>
</div>



### Duas escolhas a serem feitas ao usar `.groupby`

Como encontramos **o número de diferentes serviços solicitados em cada bairro**?

**Duas opções**:
1. Por qual coluna devemos agrupar?
2. Que método de agregação devemos usar?
- Alguns mais comuns são `.count()`, `.sum()`, `.mean()`, `.median()`, `.max()` e `.min()`.


```python
#In: 
...
```




    Ellipsis



### Observação #4

- Para contar a quantidade de elementos por grupo use o `.size()`


```python
#In: 
num_barracas = df.groupby('NOME_SETOR').size()
num_barracas
```




    NOME_SETOR
    Alimentação                              100
    Arranjos e Complementos                   52
    Artes e Pintura                           30
    Bijouterias                              183
    Calçados                                 159
                                            ... 
    Decoração e Utilidades                    69
    Esculturas                                 2
    Mobilário, Flores, Arranjos, Cestaria     42
    Vestuário                                219
    Vestuário Infantil                       121
    Length: 13, dtype: int64



## Mais prática: conjunto de dados IMDb 🎞️

- Os dados estão [aqui](https://raw.githubusercontent.com/flaviovdf/fcd/main/assets/06-GroupBy/data/imdb.csv)

![](https://raw.githubusercontent.com/flaviovdf/fcd/master/assets/06-GroupBy/images/imdb.png)


```python
#In: 
# No colab, use este caminho 'https://raw.githubusercontent.com/flaviovdf/fcd/main/assets/06-GroupBy/data/imdb.csv'
imdb = bpd.read_csv('imdb.csv').set_index('Title').sort_values(by='Rating')
imdb
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Votes</th>
      <th>Rating</th>
      <th>Year</th>
      <th>Decade</th>
    </tr>
    <tr>
      <th>Title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Akira</th>
      <td>91652</td>
      <td>8.0</td>
      <td>1988</td>
      <td>1980</td>
    </tr>
    <tr>
      <th>Per un pugno di dollari</th>
      <td>124671</td>
      <td>8.0</td>
      <td>1964</td>
      <td>1960</td>
    </tr>
    <tr>
      <th>Guardians of the Galaxy</th>
      <td>527349</td>
      <td>8.0</td>
      <td>2014</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>The Man Who Shot Liberty Valance</th>
      <td>49135</td>
      <td>8.0</td>
      <td>1962</td>
      <td>1960</td>
    </tr>
    <tr>
      <th>Underground</th>
      <td>39447</td>
      <td>8.0</td>
      <td>1995</td>
      <td>1990</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Schindler's List</th>
      <td>761224</td>
      <td>8.9</td>
      <td>1993</td>
      <td>1990</td>
    </tr>
    <tr>
      <th>12 Angry Men</th>
      <td>384187</td>
      <td>8.9</td>
      <td>1957</td>
      <td>1950</td>
    </tr>
    <tr>
      <th>The Godfather: Part II</th>
      <td>692753</td>
      <td>9.0</td>
      <td>1974</td>
      <td>1970</td>
    </tr>
    <tr>
      <th>The Shawshank Redemption</th>
      <td>1498733</td>
      <td>9.2</td>
      <td>1994</td>
      <td>1990</td>
    </tr>
    <tr>
      <th>The Godfather</th>
      <td>1027398</td>
      <td>9.2</td>
      <td>1972</td>
      <td>1970</td>
    </tr>
  </tbody>
</table>
<p>250 rows × 4 columns</p>
</div>



### Pergunta: Quantos filmes aparecem em cada década?


```python
#In: 
imdb.groupby('Decade').count()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Votes</th>
      <th>Rating</th>
      <th>Year</th>
    </tr>
    <tr>
      <th>Decade</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1920</th>
      <td>4</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1930</th>
      <td>7</td>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1940</th>
      <td>14</td>
      <td>14</td>
      <td>14</td>
    </tr>
    <tr>
      <th>1950</th>
      <td>30</td>
      <td>30</td>
      <td>30</td>
    </tr>
    <tr>
      <th>1960</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1970</th>
      <td>21</td>
      <td>21</td>
      <td>21</td>
    </tr>
    <tr>
      <th>1980</th>
      <td>31</td>
      <td>31</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1990</th>
      <td>42</td>
      <td>42</td>
      <td>42</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>50</td>
      <td>50</td>
      <td>50</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>29</td>
      <td>29</td>
      <td>29</td>
    </tr>
  </tbody>
</table>
</div>




```python
#In: 
# We'll learn how to make plots like this in the next lecture!
imdb.groupby('Decade').count().plot(y='Year');
```


    
![png](06-GroupBy_files/06-GroupBy_71_0.png)
    


### Pergunta: Qual foi o filme de maior audiência da década de 1990?

Vamos tentar fazer isso de duas maneiras diferentes.

#### Sem agrupamento


```python
#In: 
%%pt
imdb[imdb.get('Decade') == 1990].sort_values('Rating', ascending=False).index[0]
```



<div class="pt-viz" id="pt-viz-2"></div>
<script>
drawWsv('#pt-viz-2', "{\n  \"code\": \"imdb[imdb.get('Decade') == 1990].sort_values('Rating', ascending=False).index[0]\\n\",\n  \"explanation\": [\n    {\n      \"type\": \"Subscript\",\n      \"code_step\": \"imdb[imdb.get('Decade') == 1990].sort_values('Rating', ascending=False).index[0]\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 4\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 32\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"using\",\n          \"pos\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"column\",\n            \"label\": \"Decade\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Underground\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Underground\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Beauty and the Beast\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Beauty and the Beast\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Truman Show\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Truman Show\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Jurassic Park\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Jurassic Park\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Trois couleurs: Rouge\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Trois couleurs: Rouge\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Twelve Monkeys\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Twelve Monkeys\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Groundhog Day\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Groundhog Day\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"La haine\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"La haine\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Before Sunrise\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Before Sunrise\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"In the Name of the Father\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"In the Name of the Father\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Sixth Sense\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Sixth Sense\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Fargo\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Fargo\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Trainspotting\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Trainspotting\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Good Will Hunting\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Good Will Hunting\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Casino\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Casino\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Lock, Stock and Two Smoking Barrels (1998)\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Lock, Stock and Two Smoking Barrels (1998)\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Big Lebowski\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Big Lebowski\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Heat\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Heat\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Braveheart\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Braveheart\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"L.A. Confidential\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"L.A. Confidential\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Toy Story\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Toy Story\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Reservoir Dogs\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Reservoir Dogs\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Unforgiven\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Unforgiven\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Lion King\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Lion King\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Mononoke-hime\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Mononoke-hime\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"American Beauty\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"American Beauty\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"American History X\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"American History X\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Terminator 2: Judgment Day\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Terminator 2: Judgment Day\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Saving Private Ryan\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Saving Private Ryan\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Green Mile\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Green Mile\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Se7en\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Se7en\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"La vita \\u00e8 bella\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"La vita \\u00e8 bella\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"L\\u00e9on\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"L\\u00e9on\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Usual Suspects\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Usual Suspects\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Silence of the Lambs\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Silence of the Lambs\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Forrest Gump\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Forrest Gump\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Matrix\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Matrix\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Goodfellas\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Goodfellas\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Fight Club\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Fight Club\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Pulp Fiction\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Pulp Fiction\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Schindler's List\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Schindler's List\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Shawshank Redemption\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Shawshank Redemption\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Votes\",\n              \"Rating\",\n              \"Year\",\n              \"Decade\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              \"Title\"\n            ],\n            \"labels\": [\n              \"Akira\",\n              \"Per un pugno di dollari\",\n              \"Guardians of the Galaxy\",\n              \"The Man Who Shot Liberty Valance\",\n              \"Underground\",\n              \"Le samoura\\u00ef\",\n              \"Beauty and the Beast\",\n              \"La strada\",\n              \"The Avengers\",\n              \"The King's Speech\",\n              \"Barry Lyndon\",\n              \"Dog Day Afternoon\",\n              \"The Imitation Game\",\n              \"The Truman Show\",\n              \"Roman Holiday\",\n              \"The Hustler\",\n              \"X-Men: Days of Future Past\",\n              \"High Noon\",\n              \"Before Sunset\",\n              \"Monsters, Inc. (2001)\",\n              \"Harry Potter and the Deathly Hallows: Part 2\",\n              \"Lagaan: Once Upon a Time in India\",\n              \"Jurassic Park\",\n              \"Trois couleurs: Rouge\",\n              \"Fanny och Alexander\",\n              \"The Help\",\n              \"Who's Afraid of Virginia Woolf?\",\n              \"The Terminator\",\n              \"Twelve Monkeys\",\n              \"Kumonosu-j\\u00f4\",\n              \"Groundhog Day\",\n              \"Yip Man\",\n              \"Notorious\",\n              \"Pirates of the Caribbean: The Curse of the Black Pearl\",\n              \"La haine\",\n              \"Mou gaan dou\",\n              \"Salinui chueok\",\n              \"The Big Sleep\",\n              \"Paris, Texas (1984)\",\n              \"Rocky\",\n              \"Prisoners\",\n              \"The Killing\",\n              \"The Night of the Hunter\",\n              \"La battaglia di Algeri\",\n              \"Bom yeoreum gaeul gyeoul geurigo bom\",\n              \"Papillon\",\n              \"Before Sunrise\",\n              \"Tenk\\u00fb no shiro Rapyuta\",\n              \"Jaws\",\n              \"Relatos salvajes\",\n              \"Incendies\",\n              \"Hachi: A Dog's Tale\",\n              \"The Best Years of Our Lives\",\n              \"The Wizard of Oz\",\n              \"Cool Hand Luke\",\n              \"In the Name of the Father\",\n              \"The Grapes of Wrath\",\n              \"A Beautiful Mind\",\n              \"Donnie Darko\",\n              \"Mary and Max\",\n              \"Stand by Me\",\n              \"The Grand Budapest Hotel\",\n              \"Butch Cassidy and the Sundance Kid\",\n              \"The Sixth Sense\",\n              \"The Thing\",\n              \"Le salaire de la peur\",\n              \"Persona\",\n              \"Annie Hall\",\n              \"Ben-Hur\",\n              \"8\\u00bd\",\n              \"Fargo\",\n              \"Strangers on a Train\",\n              \"Gone Girl\",\n              \"Million Dollar Baby\",\n              \"Network\",\n              \"It Happened One Night\",\n              \"Trainspotting\",\n              \"Les diaboliques\",\n              \"There Will Be Blood\",\n              \"The Princess Bride\",\n              \"Dial M for Murder\",\n              \"Hotel Rwanda\",\n              \"Sin City\",\n              \"The Deer Hunter\",\n              \"How to Train Your Dragon\",\n              \"Into the Wild\",\n              \"Rush (2013/I)\",\n              \"Gandhi\",\n              \"The Maltese Falcon\",\n              \"Amores perros\",\n              \"12 Years a Slave\",\n              \"The Bourne Ultimatum\",\n              \"Kaze no tani no Naushika\",\n              \"Platoon\",\n              \"Finding Nemo\",\n              \"Stalker\",\n              \"Life of Brian\",\n              \"Gone with the Wind\",\n              \"Touch of Evil\",\n              \"No Country for Old Men\",\n              \"Les quatre cents coups\",\n              \"Shutter Island\",\n              \"Kill Bill: Vol. 1\",\n              \"El secreto de sus ojos\",\n              \"Gran Torino\",\n              \"Smultronst\\u00e4llet\",\n              \"Good Will Hunting\",\n              \"Jagten\",\n              \"The Bridge on the River Kwai\",\n              \"The Elephant Man\",\n              \"Casino\",\n              \"Lock, Stock and Two Smoking Barrels (1998)\",\n              \"The Wolf of Wall Street\",\n              \"Tonari no Totoro\",\n              \"3 Idiots\",\n              \"The General\",\n              \"Judgment at Nuremberg\",\n              \"The Gold Rush\",\n              \"The Big Lebowski\",\n              \"The Great Escape\",\n              \"Up\",\n              \"Warrior\",\n              \"Ran\",\n              \"Der Untergang\",\n              \"Hauru no ugoku shiro\",\n              \"V for Vendetta\",\n              \"Chinatown\",\n              \"El laberinto del fauno\",\n              \"Blade Runner\",\n              \"On the Waterfront\",\n              \"Raging Bull\",\n              \"Det sjunde inseglet\",\n              \"Ikiru\",\n              \"Mr. Smith Goes to Washington\",\n              \"Rebecca\",\n              \"Heat\",\n              \"Die Hard\",\n              \"Per qualche dollaro in pi\\u00f9\",\n              \"Mad Max: Fury Road\",\n              \"Monty Python and the Holy Grail\",\n              \"Witness for the Prosecution\",\n              \"A Clockwork Orange\",\n              \"The Third Man\",\n              \"Braveheart\",\n              \"Amadeus\",\n              \"Indiana Jones and the Last Crusade\",\n              \"The Kid\",\n              \"Y\\u00f4jinb\\u00f4\",\n              \"Jodaeiye Nader az Simin\",\n              \"Eternal Sunshine of the Spotless Mind\",\n              \"The Apartment\",\n              \"Requiem for a Dream\",\n              \"Rash\\u00f4mon\",\n              \"Toy Story 3\",\n              \"Double Indemnity\",\n              \"Metropolis\",\n              \"L.A. Confidential\",\n              \"Snatch.\",\n              \"Lawrence of Arabia\",\n              \"2001: A Space Odyssey\",\n              \"The Sting\",\n              \"The Treasure of the Sierra Madre\",\n              \"Ladri di biciclette\",\n              \"Toy Story\",\n              \"Inglourious Basterds\",\n              \"Singin' in the Rain\",\n              \"All About Eve\",\n              \"To Kill a Mockingbird\",\n              \"Scarface\",\n              \"Full Metal Jacket\",\n              \"Taxi Driver\",\n              \"Reservoir Dogs\",\n              \"Unforgiven\",\n              \"Some Like It Hot\",\n              \"Batman Begins\",\n              \"The Great Dictator\",\n              \"The Shining\",\n              \"Nuovo Cinema Paradiso\",\n              \"The Dark Knight Rises\",\n              \"WALL\\u00b7E\",\n              \"Citizen Kane\",\n              \"Once Upon a Time in America\",\n              \"The Lion King\",\n              \"Das Leben der Anderen\",\n              \"Oldeuboi\",\n              \"Mononoke-hime\",\n              \"Vertigo\",\n              \"Das Boot\",\n              \"M\",\n              \"Star Wars: Episode VI - Return of the Jedi\",\n              \"Le fabuleux destin d'Am\\u00e9lie Poulain\",\n              \"American Beauty\",\n              \"Django Unchained\",\n              \"North by Northwest\",\n              \"Aliens\",\n              \"Paths of Glory\",\n              \"Hotaru no haka\",\n              \"American History X\",\n              \"City Lights\",\n              \"Rear Window\",\n              \"Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb\",\n              \"Terminator 2: Judgment Day\",\n              \"Inside Out (2015/I)\",\n              \"Sen to Chihiro no Kamikakushi\",\n              \"Casablanca\",\n              \"Back to the Future\",\n              \"Modern Times\",\n              \"Saving Private Ryan\",\n              \"Gladiator\",\n              \"Raiders of the Lost Ark\",\n              \"Intouchables\",\n              \"Psycho\",\n              \"Whiplash\",\n              \"The Green Mile\",\n              \"Memento\",\n              \"The Prestige\",\n              \"The Departed\",\n              \"The Pianist\",\n              \"Alien\",\n              \"Apocalypse Now\",\n              \"Sunset Blvd.\",\n              \"Se7en\",\n              \"La vita \\u00e8 bella\",\n              \"Interstellar\",\n              \"It's a Wonderful Life\",\n              \"Cidade de Deus\",\n              \"L\\u00e9on\",\n              \"C'era una volta il West\",\n              \"The Usual Suspects\",\n              \"The Silence of the Lambs\",\n              \"Forrest Gump\",\n              \"The Lord of the Rings: The Two Towers\",\n              \"Star Wars: Episode V - The Empire Strikes Back\",\n              \"The Matrix\",\n              \"Star Wars\",\n              \"Goodfellas\",\n              \"One Flew Over the Cuckoo's Nest\",\n              \"Shichinin no samurai\",\n              \"Inception\",\n              \"Fight Club\",\n              \"The Lord of the Rings: The Fellowship of the Ring\",\n              \"Pulp Fiction\",\n              \"The Dark Knight\",\n              \"Il buono, il brutto, il cattivo (1966)\",\n              \"The Lord of the Rings: The Return of the King\",\n              \"Schindler's List\",\n              \"12 Angry Men\",\n              \"The Godfather: Part II\",\n              \"The Shawshank Redemption\",\n              \"The Godfather\"\n            ]\n          },\n          \"data\": [\n            [\n              91652.0,\n              8.0,\n              1988.0,\n              1980.0\n            ],\n            [\n              124671.0,\n              8.0,\n              1964.0,\n              1960.0\n            ],\n            [\n              527349.0,\n              8.0,\n              2014.0,\n              2010.0\n            ],\n            [\n              49135.0,\n              8.0,\n              1962.0,\n              1960.0\n            ],\n            [\n              39447.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              28012.0,\n              8.0,\n              1967.0,\n              1960.0\n            ],\n            [\n              268480.0,\n              8.0,\n              1991.0,\n              1990.0\n            ],\n            [\n              42446.0,\n              8.0,\n              1954.0,\n              1950.0\n            ],\n            [\n              862016.0,\n              8.0,\n              2012.0,\n              2010.0\n            ],\n            [\n              434906.0,\n              8.0,\n              2010.0,\n              2010.0\n            ],\n            [\n              89920.0,\n              8.0,\n              1975.0,\n              1970.0\n            ],\n            [\n              164419.0,\n              8.0,\n              1975.0,\n              1970.0\n            ],\n            [\n              321834.0,\n              8.0,\n              2014.0,\n              2010.0\n            ],\n            [\n              583004.0,\n              8.0,\n              1998.0,\n              1990.0\n            ],\n            [\n              87437.0,\n              8.0,\n              1953.0,\n              1950.0\n            ],\n            [\n              57842.0,\n              8.0,\n              1961.0,\n              1960.0\n            ],\n            [\n              427099.0,\n              8.0,\n              2014.0,\n              2010.0\n            ],\n            [\n              72007.0,\n              8.0,\n              1952.0,\n              1950.0\n            ],\n            [\n              147277.0,\n              8.0,\n              2004.0,\n              2000.0\n            ],\n            [\n              500576.0,\n              8.0,\n              2001.0,\n              2000.0\n            ],\n            [\n              466111.0,\n              8.0,\n              2011.0,\n              2010.0\n            ],\n            [\n              59377.0,\n              8.0,\n              2001.0,\n              2000.0\n            ],\n            [\n              520391.0,\n              8.0,\n              1993.0,\n              1990.0\n            ],\n            [\n              57644.0,\n              8.0,\n              1994.0,\n              1990.0\n            ],\n            [\n              37972.0,\n              8.0,\n              1982.0,\n              1980.0\n            ],\n            [\n              282774.0,\n              8.0,\n              2011.0,\n              2010.0\n            ],\n            [\n              50208.0,\n              8.0,\n              1966.0,\n              1960.0\n            ],\n            [\n              527274.0,\n              8.0,\n              1984.0,\n              1980.0\n            ],\n            [\n              415809.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              26012.0,\n              8.0,\n              1957.0,\n              1950.0\n            ],\n            [\n              384272.0,\n              8.0,\n              1993.0,\n              1990.0\n            ],\n            [\n              138240.0,\n              8.0,\n              2008.0,\n              2000.0\n            ],\n            [\n              66622.0,\n              8.0,\n              1946.0,\n              1940.0\n            ],\n            [\n              718645.0,\n              8.0,\n              2003.0,\n              2000.0\n            ],\n            [\n              90937.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              82440.0,\n              8.0,\n              2002.0,\n              2000.0\n            ],\n            [\n              54651.0,\n              8.0,\n              2003.0,\n              2000.0\n            ],\n            [\n              59578.0,\n              8.0,\n              1946.0,\n              1940.0\n            ],\n            [\n              43090.0,\n              8.0,\n              1984.0,\n              1980.0\n            ],\n            [\n              318041.0,\n              8.0,\n              1976.0,\n              1970.0\n            ],\n            [\n              312516.0,\n              8.0,\n              2013.0,\n              2010.0\n            ],\n            [\n              56671.0,\n              8.0,\n              1956.0,\n              1950.0\n            ],\n            [\n              57974.0,\n              8.0,\n              1955.0,\n              1950.0\n            ],\n            [\n              32385.0,\n              8.0,\n              1966.0,\n              1960.0\n            ],\n            [\n              55382.0,\n              8.0,\n              2003.0,\n              2000.0\n            ],\n            [\n              87591.0,\n              8.0,\n              1973.0,\n              1970.0\n            ],\n            [\n              158867.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              83941.0,\n              8.0,\n              1986.0,\n              1980.0\n            ],\n            [\n              364225.0,\n              8.0,\n              1975.0,\n              1970.0\n            ],\n            [\n              46987.0,\n              8.0,\n              2014.0,\n              2010.0\n            ],\n            [\n              65122.0,\n              8.1,\n              2010.0,\n              2010.0\n            ],\n            [\n              121599.0,\n              8.1,\n              2009.0,\n              2000.0\n            ],\n            [\n              35983.0,\n              8.1,\n              1946.0,\n              1940.0\n            ],\n            [\n              259235.0,\n              8.1,\n              1939.0,\n              1930.0\n            ],\n            [\n              108128.0,\n              8.1,\n              1967.0,\n              1960.0\n            ],\n            [\n              95212.0,\n              8.1,\n              1993.0,\n              1990.0\n            ],\n            [\n              55793.0,\n              8.1,\n              1940.0,\n              1940.0\n            ],\n            [\n              525515.0,\n              8.1,\n              2001.0,\n              2000.0\n            ],\n            [\n              522119.0,\n              8.1,\n              2001.0,\n              2000.0\n            ],\n            [\n              102735.0,\n              8.1,\n              2009.0,\n              2000.0\n            ],\n            [\n              242381.0,\n              8.1,\n              1986.0,\n              1980.0\n            ],\n            [\n              369141.0,\n              8.1,\n              2014.0,\n              2010.0\n            ],\n            [\n              134552.0,\n              8.1,\n              1969.0,\n              1960.0\n            ],\n            [\n              630994.0,\n              8.1,\n              1999.0,\n              1990.0\n            ],\n            [\n              228230.0,\n              8.1,\n              1982.0,\n              1980.0\n            ],\n            [\n              31003.0,\n              8.1,\n              1953.0,\n              1950.0\n            ],\n            [\n              52573.0,\n              8.1,\n              1966.0,\n              1960.0\n            ],\n            [\n              169982.0,\n              8.1,\n              1977.0,\n              1970.0\n            ],\n            [\n              141768.0,\n              8.1,\n              1959.0,\n              1950.0\n            ],\n            [\n              69988.0,\n              8.1,\n              1963.0,\n              1960.0\n            ],\n            [\n              395997.0,\n              8.1,\n              1996.0,\n              1990.0\n            ],\n            [\n              85012.0,\n              8.1,\n              1951.0,\n              1950.0\n            ],\n            [\n              441174.0,\n              8.1,\n              2014.0,\n              2010.0\n            ],\n            [\n              424957.0,\n              8.1,\n              2004.0,\n              2000.0\n            ],\n            [\n              91689.0,\n              8.1,\n              1976.0,\n              1970.0\n            ],\n            [\n              56842.0,\n              8.1,\n              1934.0,\n              1930.0\n            ],\n            [\n              419372.0,\n              8.1,\n              1996.0,\n              1990.0\n            ],\n            [\n              36725.0,\n              8.1,\n              1955.0,\n              1950.0\n            ],\n            [\n              331348.0,\n              8.1,\n              2007.0,\n              2000.0\n            ],\n            [\n              264285.0,\n              8.1,\n              1987.0,\n              1980.0\n            ],\n            [\n              92244.0,\n              8.1,\n              1954.0,\n              1950.0\n            ],\n            [\n              237292.0,\n              8.1,\n              2004.0,\n              2000.0\n            ],\n            [\n              596880.0,\n              8.1,\n              2005.0,\n              2000.0\n            ],\n            [\n              209677.0,\n              8.1,\n              1978.0,\n              1970.0\n            ],\n            [\n              415845.0,\n              8.1,\n              2010.0,\n              2010.0\n            ],\n            [\n              372803.0,\n              8.1,\n              2007.0,\n              2000.0\n            ],\n            [\n              258882.0,\n              8.1,\n              2013.0,\n              2010.0\n            ],\n            [\n              151697.0,\n              8.1,\n              1982.0,\n              1980.0\n            ],\n            [\n              101754.0,\n              8.1,\n              1941.0,\n              1940.0\n            ],\n            [\n              156030.0,\n              8.1,\n              2000.0,\n              2000.0\n            ],\n            [\n              359121.0,\n              8.1,\n              2013.0,\n              2010.0\n            ],\n            [\n              433487.0,\n              8.1,\n              2007.0,\n              2000.0\n            ],\n            [\n              81809.0,\n              8.1,\n              1984.0,\n              1980.0\n            ],\n            [\n              262404.0,\n              8.1,\n              1986.0,\n              1980.0\n            ],\n            [\n              591219.0,\n              8.1,\n              2003.0,\n              2000.0\n            ],\n            [\n              59272.0,\n              8.1,\n              1979.0,\n              1970.0\n            ],\n            [\n              242886.0,\n              8.1,\n              1979.0,\n              1970.0\n            ],\n            [\n              192791.0,\n              8.1,\n              1939.0,\n              1930.0\n            ],\n            [\n              65408.0,\n              8.1,\n              1958.0,\n              1950.0\n            ],\n            [\n              540744.0,\n              8.1,\n              2007.0,\n              2000.0\n            ],\n            [\n              61776.0,\n              8.1,\n              1959.0,\n              1950.0\n            ],\n            [\n              670328.0,\n              8.1,\n              2010.0,\n              2010.0\n            ],\n            [\n              647120.0,\n              8.1,\n              2003.0,\n              2000.0\n            ],\n            [\n              109254.0,\n              8.2,\n              2009.0,\n              2000.0\n            ],\n            [\n              502773.0,\n              8.2,\n              2008.0,\n              2000.0\n            ],\n            [\n              55861.0,\n              8.2,\n              1957.0,\n              1950.0\n            ],\n            [\n              529800.0,\n              8.2,\n              1997.0,\n              1990.0\n            ],\n            [\n              137310.0,\n              8.2,\n              2012.0,\n              2010.0\n            ],\n            [\n              132677.0,\n              8.2,\n              1957.0,\n              1950.0\n            ],\n            [\n              145514.0,\n              8.2,\n              1980.0,\n              1980.0\n            ],\n            [\n              294394.0,\n              8.2,\n              1995.0,\n              1990.0\n            ],\n            [\n              372863.0,\n              8.2,\n              1998.0,\n              1990.0\n            ],\n            [\n              624943.0,\n              8.2,\n              2013.0,\n              2010.0\n            ],\n            [\n              142605.0,\n              8.2,\n              1988.0,\n              1980.0\n            ],\n            [\n              167076.0,\n              8.2,\n              2009.0,\n              2000.0\n            ],\n            [\n              46332.0,\n              8.2,\n              1926.0,\n              1920.0\n            ],\n            [\n              37664.0,\n              8.2,\n              1961.0,\n              1960.0\n            ],\n            [\n              58506.0,\n              8.2,\n              1925.0,\n              1920.0\n            ],\n            [\n              473988.0,\n              8.2,\n              1998.0,\n              1990.0\n            ],\n            [\n              147372.0,\n              8.2,\n              1963.0,\n              1960.0\n            ],\n            [\n              566091.0,\n              8.2,\n              2009.0,\n              2000.0\n            ],\n            [\n              287727.0,\n              8.2,\n              2011.0,\n              2010.0\n            ],\n            [\n              69287.0,\n              8.2,\n              1985.0,\n              1980.0\n            ],\n            [\n              223834.0,\n              8.2,\n              2004.0,\n              2000.0\n            ],\n            [\n              182525.0,\n              8.2,\n              2004.0,\n              2000.0\n            ],\n            [\n              700999.0,\n              8.2,\n              2005.0,\n              2000.0\n            ],\n            [\n              192744.0,\n              8.2,\n              1974.0,\n              1970.0\n            ],\n            [\n              416467.0,\n              8.2,\n              2006.0,\n              2000.0\n            ],\n            [\n              415615.0,\n              8.2,\n              1982.0,\n              1980.0\n            ],\n            [\n              89233.0,\n              8.2,\n              1954.0,\n              1950.0\n            ],\n            [\n              210647.0,\n              8.2,\n              1980.0,\n              1980.0\n            ],\n            [\n              98949.0,\n              8.2,\n              1957.0,\n              1950.0\n            ],\n            [\n              36638.0,\n              8.2,\n              1952.0,\n              1950.0\n            ],\n            [\n              69510.0,\n              8.2,\n              1939.0,\n              1930.0\n            ],\n            [\n              78216.0,\n              8.2,\n              1940.0,\n              1940.0\n            ],\n            [\n              388239.0,\n              8.2,\n              1995.0,\n              1990.0\n            ],\n            [\n              529287.0,\n              8.2,\n              1988.0,\n              1980.0\n            ],\n            [\n              135556.0,\n              8.3,\n              1965.0,\n              1960.0\n            ],\n            [\n              262425.0,\n              8.3,\n              2015.0,\n              2010.0\n            ],\n            [\n              343300.0,\n              8.3,\n              1975.0,\n              1970.0\n            ],\n            [\n              53186.0,\n              8.3,\n              1957.0,\n              1950.0\n            ],\n            [\n              489807.0,\n              8.3,\n              1971.0,\n              1970.0\n            ],\n            [\n              101785.0,\n              8.3,\n              1949.0,\n              1940.0\n            ],\n            [\n              653769.0,\n              8.3,\n              1995.0,\n              1990.0\n            ],\n            [\n              240308.0,\n              8.3,\n              1984.0,\n              1980.0\n            ],\n            [\n              453748.0,\n              8.3,\n              1989.0,\n              1980.0\n            ],\n            [\n              55784.0,\n              8.3,\n              1921.0,\n              1920.0\n            ],\n            [\n              65370.0,\n              8.3,\n              1961.0,\n              1960.0\n            ],\n            [\n              133760.0,\n              8.3,\n              2011.0,\n              2010.0\n            ],\n            [\n              589477.0,\n              8.3,\n              2004.0,\n              2000.0\n            ],\n            [\n              96190.0,\n              8.3,\n              1960.0,\n              1960.0\n            ],\n            [\n              511177.0,\n              8.3,\n              2000.0,\n              2000.0\n            ],\n            [\n              90434.0,\n              8.3,\n              1950.0,\n              1950.0\n            ],\n            [\n              470158.0,\n              8.3,\n              2010.0,\n              2010.0\n            ],\n            [\n              86715.0,\n              8.3,\n              1944.0,\n              1940.0\n            ],\n            [\n              98794.0,\n              8.3,\n              1927.0,\n              1920.0\n            ],\n            [\n              376590.0,\n              8.3,\n              1997.0,\n              1990.0\n            ],\n            [\n              536053.0,\n              8.3,\n              2000.0,\n              2000.0\n            ],\n            [\n              170302.0,\n              8.3,\n              1962.0,\n              1960.0\n            ],\n            [\n              373482.0,\n              8.3,\n              1968.0,\n              1960.0\n            ],\n            [\n              155691.0,\n              8.3,\n              1973.0,\n              1970.0\n            ],\n            [\n              71003.0,\n              8.3,\n              1948.0,\n              1940.0\n            ],\n            [\n              81887.0,\n              8.3,\n              1948.0,\n              1940.0\n            ],\n            [\n              535249.0,\n              8.3,\n              1995.0,\n              1990.0\n            ],\n            [\n              755013.0,\n              8.3,\n              2009.0,\n              2000.0\n            ],\n            [\n              132823.0,\n              8.3,\n              1952.0,\n              1950.0\n            ],\n            [\n              74178.0,\n              8.3,\n              1950.0,\n              1950.0\n            ],\n            [\n              188160.0,\n              8.3,\n              1962.0,\n              1960.0\n            ],\n            [\n              475594.0,\n              8.3,\n              1983.0,\n              1980.0\n            ],\n            [\n              425461.0,\n              8.3,\n              1987.0,\n              1980.0\n            ],\n            [\n              444534.0,\n              8.3,\n              1976.0,\n              1970.0\n            ],\n            [\n              578684.0,\n              8.3,\n              1992.0,\n              1990.0\n            ],\n            [\n              248514.0,\n              8.3,\n              1992.0,\n              1990.0\n            ],\n            [\n              156432.0,\n              8.3,\n              1959.0,\n              1950.0\n            ],\n            [\n              850601.0,\n              8.3,\n              2005.0,\n              2000.0\n            ],\n            [\n              117590.0,\n              8.4,\n              1940.0,\n              1940.0\n            ],\n            [\n              532559.0,\n              8.4,\n              1980.0,\n              1980.0\n            ],\n            [\n              132121.0,\n              8.4,\n              1988.0,\n              1980.0\n            ],\n            [\n              997350.0,\n              8.4,\n              2012.0,\n              2010.0\n            ],\n            [\n              618914.0,\n              8.4,\n              2008.0,\n              2000.0\n            ],\n            [\n              268905.0,\n              8.4,\n              1941.0,\n              1940.0\n            ],\n            [\n              195266.0,\n              8.4,\n              1984.0,\n              1980.0\n            ],\n            [\n              548750.0,\n              8.4,\n              1994.0,\n              1990.0\n            ],\n            [\n              229533.0,\n              8.4,\n              2006.0,\n              2000.0\n            ],\n            [\n              311070.0,\n              8.4,\n              2003.0,\n              2000.0\n            ],\n            [\n              192165.0,\n              8.4,\n              1997.0,\n              1990.0\n            ],\n            [\n              218430.0,\n              8.4,\n              1958.0,\n              1950.0\n            ],\n            [\n              151256.0,\n              8.4,\n              1981.0,\n              1980.0\n            ],\n            [\n              88355.0,\n              8.4,\n              1931.0,\n              1930.0\n            ],\n            [\n              556811.0,\n              8.4,\n              1983.0,\n              1980.0\n            ],\n            [\n              474068.0,\n              8.4,\n              2001.0,\n              2000.0\n            ],\n            [\n              735056.0,\n              8.4,\n              1999.0,\n              1990.0\n            ],\n            [\n              804729.0,\n              8.4,\n              2012.0,\n              2010.0\n            ],\n            [\n              198795.0,\n              8.4,\n              1959.0,\n              1950.0\n            ],\n            [\n              436218.0,\n              8.4,\n              1986.0,\n              1980.0\n            ],\n            [\n              106038.0,\n              8.4,\n              1957.0,\n              1950.0\n            ],\n            [\n              115740.0,\n              8.4,\n              1988.0,\n              1980.0\n            ],\n            [\n              694602.0,\n              8.5,\n              1998.0,\n              1990.0\n            ],\n            [\n              92375.0,\n              8.5,\n              1931.0,\n              1930.0\n            ],\n            [\n              280432.0,\n              8.5,\n              1954.0,\n              1950.0\n            ],\n            [\n              309141.0,\n              8.5,\n              1964.0,\n              1960.0\n            ],\n            [\n              658564.0,\n              8.5,\n              1991.0,\n              1990.0\n            ],\n            [\n              79615.0,\n              8.5,\n              2015.0,\n              2010.0\n            ],\n            [\n              357355.0,\n              8.5,\n              2001.0,\n              2000.0\n            ],\n            [\n              350551.0,\n              8.5,\n              1942.0,\n              1940.0\n            ],\n            [\n              625463.0,\n              8.5,\n              1985.0,\n              1980.0\n            ],\n            [\n              121668.0,\n              8.5,\n              1936.0,\n              1930.0\n            ],\n            [\n              769893.0,\n              8.5,\n              1998.0,\n              1990.0\n            ],\n            [\n              862216.0,\n              8.5,\n              2000.0,\n              2000.0\n            ],\n            [\n              585474.0,\n              8.5,\n              1981.0,\n              1980.0\n            ],\n            [\n              417122.0,\n              8.5,\n              2011.0,\n              2010.0\n            ],\n            [\n              371791.0,\n              8.5,\n              1960.0,\n              1960.0\n            ],\n            [\n              264333.0,\n              8.5,\n              2014.0,\n              2010.0\n            ],\n            [\n              672878.0,\n              8.5,\n              1999.0,\n              1990.0\n            ],\n            [\n              752122.0,\n              8.5,\n              2000.0,\n              2000.0\n            ],\n            [\n              733490.0,\n              8.5,\n              2006.0,\n              2000.0\n            ],\n            [\n              763871.0,\n              8.5,\n              2006.0,\n              2000.0\n            ],\n            [\n              431856.0,\n              8.5,\n              2002.0,\n              2000.0\n            ],\n            [\n              496833.0,\n              8.5,\n              1979.0,\n              1970.0\n            ],\n            [\n              403754.0,\n              8.5,\n              1979.0,\n              1970.0\n            ],\n            [\n              123879.0,\n              8.5,\n              1950.0,\n              1950.0\n            ],\n            [\n              895411.0,\n              8.6,\n              1995.0,\n              1990.0\n            ],\n            [\n              358305.0,\n              8.6,\n              1997.0,\n              1990.0\n            ],\n            [\n              689541.0,\n              8.6,\n              2014.0,\n              2010.0\n            ],\n            [\n              242353.0,\n              8.6,\n              1946.0,\n              1940.0\n            ],\n            [\n              476501.0,\n              8.6,\n              2002.0,\n              2000.0\n            ],\n            [\n              635139.0,\n              8.6,\n              1994.0,\n              1990.0\n            ],\n            [\n              192206.0,\n              8.6,\n              1968.0,\n              1960.0\n            ],\n            [\n              656756.0,\n              8.6,\n              1995.0,\n              1990.0\n            ],\n            [\n              767224.0,\n              8.6,\n              1991.0,\n              1990.0\n            ],\n            [\n              1078416.0,\n              8.7,\n              1994.0,\n              1990.0\n            ],\n            [\n              967389.0,\n              8.7,\n              2002.0,\n              2000.0\n            ],\n            [\n              700283.0,\n              8.7,\n              1980.0,\n              1980.0\n            ],\n            [\n              1073043.0,\n              8.7,\n              1999.0,\n              1990.0\n            ],\n            [\n              770011.0,\n              8.7,\n              1977.0,\n              1970.0\n            ],\n            [\n              644556.0,\n              8.7,\n              1990.0,\n              1990.0\n            ],\n            [\n              606395.0,\n              8.7,\n              1975.0,\n              1970.0\n            ],\n            [\n              206216.0,\n              8.7,\n              1954.0,\n              1950.0\n            ],\n            [\n              1271949.0,\n              8.7,\n              2010.0,\n              2010.0\n            ],\n            [\n              1177098.0,\n              8.8,\n              1999.0,\n              1990.0\n            ],\n            [\n              1099087.0,\n              8.8,\n              2001.0,\n              2000.0\n            ],\n            [\n              1166532.0,\n              8.9,\n              1994.0,\n              1990.0\n            ],\n            [\n              1473049.0,\n              8.9,\n              2008.0,\n              2000.0\n            ],\n            [\n              447875.0,\n              8.9,\n              1966.0,\n              1960.0\n            ],\n            [\n              1074146.0,\n              8.9,\n              2003.0,\n              2000.0\n            ],\n            [\n              761224.0,\n              8.9,\n              1993.0,\n              1990.0\n            ],\n            [\n              384187.0,\n              8.9,\n              1957.0,\n              1950.0\n            ],\n            [\n              692753.0,\n              9.0,\n              1974.0,\n              1970.0\n            ],\n            [\n              1498733.0,\n              9.2,\n              1994.0,\n              1990.0\n            ],\n            [\n              1027398.0,\n              9.2,\n              1972.0,\n              1970.0\n            ]\n          ]\n        },\n        \"rhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Votes\",\n              \"Rating\",\n              \"Year\",\n              \"Decade\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              \"Title\"\n            ],\n            \"labels\": [\n              \"Underground\",\n              \"Beauty and the Beast\",\n              \"The Truman Show\",\n              \"Jurassic Park\",\n              \"Trois couleurs: Rouge\",\n              \"Twelve Monkeys\",\n              \"Groundhog Day\",\n              \"La haine\",\n              \"Before Sunrise\",\n              \"In the Name of the Father\",\n              \"The Sixth Sense\",\n              \"Fargo\",\n              \"Trainspotting\",\n              \"Good Will Hunting\",\n              \"Casino\",\n              \"Lock, Stock and Two Smoking Barrels (1998)\",\n              \"The Big Lebowski\",\n              \"Heat\",\n              \"Braveheart\",\n              \"L.A. Confidential\",\n              \"Toy Story\",\n              \"Reservoir Dogs\",\n              \"Unforgiven\",\n              \"The Lion King\",\n              \"Mononoke-hime\",\n              \"American Beauty\",\n              \"American History X\",\n              \"Terminator 2: Judgment Day\",\n              \"Saving Private Ryan\",\n              \"The Green Mile\",\n              \"Se7en\",\n              \"La vita \\u00e8 bella\",\n              \"L\\u00e9on\",\n              \"The Usual Suspects\",\n              \"The Silence of the Lambs\",\n              \"Forrest Gump\",\n              \"The Matrix\",\n              \"Goodfellas\",\n              \"Fight Club\",\n              \"Pulp Fiction\",\n              \"Schindler's List\",\n              \"The Shawshank Redemption\"\n            ]\n          },\n          \"data\": [\n            [\n              39447.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              268480.0,\n              8.0,\n              1991.0,\n              1990.0\n            ],\n            [\n              583004.0,\n              8.0,\n              1998.0,\n              1990.0\n            ],\n            [\n              520391.0,\n              8.0,\n              1993.0,\n              1990.0\n            ],\n            [\n              57644.0,\n              8.0,\n              1994.0,\n              1990.0\n            ],\n            [\n              415809.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              384272.0,\n              8.0,\n              1993.0,\n              1990.0\n            ],\n            [\n              90937.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              158867.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              95212.0,\n              8.1,\n              1993.0,\n              1990.0\n            ],\n            [\n              630994.0,\n              8.1,\n              1999.0,\n              1990.0\n            ],\n            [\n              395997.0,\n              8.1,\n              1996.0,\n              1990.0\n            ],\n            [\n              419372.0,\n              8.1,\n              1996.0,\n              1990.0\n            ],\n            [\n              529800.0,\n              8.2,\n              1997.0,\n              1990.0\n            ],\n            [\n              294394.0,\n              8.2,\n              1995.0,\n              1990.0\n            ],\n            [\n              372863.0,\n              8.2,\n              1998.0,\n              1990.0\n            ],\n            [\n              473988.0,\n              8.2,\n              1998.0,\n              1990.0\n            ],\n            [\n              388239.0,\n              8.2,\n              1995.0,\n              1990.0\n            ],\n            [\n              653769.0,\n              8.3,\n              1995.0,\n              1990.0\n            ],\n            [\n              376590.0,\n              8.3,\n              1997.0,\n              1990.0\n            ],\n            [\n              535249.0,\n              8.3,\n              1995.0,\n              1990.0\n            ],\n            [\n              578684.0,\n              8.3,\n              1992.0,\n              1990.0\n            ],\n            [\n              248514.0,\n              8.3,\n              1992.0,\n              1990.0\n            ],\n            [\n              548750.0,\n              8.4,\n              1994.0,\n              1990.0\n            ],\n            [\n              192165.0,\n              8.4,\n              1997.0,\n              1990.0\n            ],\n            [\n              735056.0,\n              8.4,\n              1999.0,\n              1990.0\n            ],\n            [\n              694602.0,\n              8.5,\n              1998.0,\n              1990.0\n            ],\n            [\n              658564.0,\n              8.5,\n              1991.0,\n              1990.0\n            ],\n            [\n              769893.0,\n              8.5,\n              1998.0,\n              1990.0\n            ],\n            [\n              672878.0,\n              8.5,\n              1999.0,\n              1990.0\n            ],\n            [\n              895411.0,\n              8.6,\n              1995.0,\n              1990.0\n            ],\n            [\n              358305.0,\n              8.6,\n              1997.0,\n              1990.0\n            ],\n            [\n              635139.0,\n              8.6,\n              1994.0,\n              1990.0\n            ],\n            [\n              656756.0,\n              8.6,\n              1995.0,\n              1990.0\n            ],\n            [\n              767224.0,\n              8.6,\n              1991.0,\n              1990.0\n            ],\n            [\n              1078416.0,\n              8.7,\n              1994.0,\n              1990.0\n            ],\n            [\n              1073043.0,\n              8.7,\n              1999.0,\n              1990.0\n            ],\n            [\n              644556.0,\n              8.7,\n              1990.0,\n              1990.0\n            ],\n            [\n              1177098.0,\n              8.8,\n              1999.0,\n              1990.0\n            ],\n            [\n              1166532.0,\n              8.9,\n              1994.0,\n              1990.0\n            ],\n            [\n              761224.0,\n              8.9,\n              1993.0,\n              1990.0\n            ],\n            [\n              1498733.0,\n              9.2,\n              1994.0,\n              1990.0\n            ]\n          ]\n        }\n      }\n    },\n    {\n      \"type\": \"SortValuesCall\",\n      \"code_step\": \"imdb[imdb.get('Decade') == 1990].sort_values('Rating', ascending=False).index[0]\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 32\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 71\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"using\",\n          \"pos\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"column\",\n            \"label\": \"Rating\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Underground\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Underground\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Beauty and the Beast\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Beauty and the Beast\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Truman Show\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Truman Show\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Jurassic Park\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Jurassic Park\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Trois couleurs: Rouge\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Trois couleurs: Rouge\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Twelve Monkeys\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Twelve Monkeys\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Groundhog Day\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Groundhog Day\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"La haine\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"La haine\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Before Sunrise\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Before Sunrise\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"In the Name of the Father\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"In the Name of the Father\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Sixth Sense\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Sixth Sense\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Fargo\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Fargo\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Trainspotting\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Trainspotting\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Good Will Hunting\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Good Will Hunting\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Casino\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Casino\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Lock, Stock and Two Smoking Barrels (1998)\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Lock, Stock and Two Smoking Barrels (1998)\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Big Lebowski\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Big Lebowski\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Heat\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Heat\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Braveheart\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Braveheart\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"L.A. Confidential\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"L.A. Confidential\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Toy Story\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Toy Story\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Reservoir Dogs\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Reservoir Dogs\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Unforgiven\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Unforgiven\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Lion King\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Lion King\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Mononoke-hime\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Mononoke-hime\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"American Beauty\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"American Beauty\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"American History X\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"American History X\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Terminator 2: Judgment Day\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Terminator 2: Judgment Day\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Saving Private Ryan\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Saving Private Ryan\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Green Mile\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Green Mile\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Se7en\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Se7en\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"La vita \\u00e8 bella\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"La vita \\u00e8 bella\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"L\\u00e9on\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"L\\u00e9on\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Usual Suspects\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Usual Suspects\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Silence of the Lambs\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Silence of the Lambs\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Forrest Gump\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Forrest Gump\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Matrix\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Matrix\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Goodfellas\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Goodfellas\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Fight Club\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Fight Club\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Pulp Fiction\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Pulp Fiction\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"Schindler's List\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"Schindler's List\"\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": \"The Shawshank Redemption\"\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": \"The Shawshank Redemption\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": \"prev_rhs\",\n        \"rhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Votes\",\n              \"Rating\",\n              \"Year\",\n              \"Decade\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              \"Title\"\n            ],\n            \"labels\": [\n              \"The Shawshank Redemption\",\n              \"Schindler's List\",\n              \"Pulp Fiction\",\n              \"Fight Club\",\n              \"Goodfellas\",\n              \"The Matrix\",\n              \"Forrest Gump\",\n              \"La vita \\u00e8 bella\",\n              \"Se7en\",\n              \"L\\u00e9on\",\n              \"The Usual Suspects\",\n              \"The Silence of the Lambs\",\n              \"The Green Mile\",\n              \"Saving Private Ryan\",\n              \"Terminator 2: Judgment Day\",\n              \"American History X\",\n              \"American Beauty\",\n              \"Mononoke-hime\",\n              \"The Lion King\",\n              \"Unforgiven\",\n              \"Reservoir Dogs\",\n              \"L.A. Confidential\",\n              \"Braveheart\",\n              \"Toy Story\",\n              \"Heat\",\n              \"The Big Lebowski\",\n              \"Lock, Stock and Two Smoking Barrels (1998)\",\n              \"Casino\",\n              \"Good Will Hunting\",\n              \"Fargo\",\n              \"Trainspotting\",\n              \"The Sixth Sense\",\n              \"In the Name of the Father\",\n              \"Twelve Monkeys\",\n              \"The Truman Show\",\n              \"Jurassic Park\",\n              \"Trois couleurs: Rouge\",\n              \"Beauty and the Beast\",\n              \"Groundhog Day\",\n              \"La haine\",\n              \"Before Sunrise\",\n              \"Underground\"\n            ]\n          },\n          \"data\": [\n            [\n              1498733.0,\n              9.2,\n              1994.0,\n              1990.0\n            ],\n            [\n              761224.0,\n              8.9,\n              1993.0,\n              1990.0\n            ],\n            [\n              1166532.0,\n              8.9,\n              1994.0,\n              1990.0\n            ],\n            [\n              1177098.0,\n              8.8,\n              1999.0,\n              1990.0\n            ],\n            [\n              644556.0,\n              8.7,\n              1990.0,\n              1990.0\n            ],\n            [\n              1073043.0,\n              8.7,\n              1999.0,\n              1990.0\n            ],\n            [\n              1078416.0,\n              8.7,\n              1994.0,\n              1990.0\n            ],\n            [\n              358305.0,\n              8.6,\n              1997.0,\n              1990.0\n            ],\n            [\n              895411.0,\n              8.6,\n              1995.0,\n              1990.0\n            ],\n            [\n              635139.0,\n              8.6,\n              1994.0,\n              1990.0\n            ],\n            [\n              656756.0,\n              8.6,\n              1995.0,\n              1990.0\n            ],\n            [\n              767224.0,\n              8.6,\n              1991.0,\n              1990.0\n            ],\n            [\n              672878.0,\n              8.5,\n              1999.0,\n              1990.0\n            ],\n            [\n              769893.0,\n              8.5,\n              1998.0,\n              1990.0\n            ],\n            [\n              658564.0,\n              8.5,\n              1991.0,\n              1990.0\n            ],\n            [\n              694602.0,\n              8.5,\n              1998.0,\n              1990.0\n            ],\n            [\n              735056.0,\n              8.4,\n              1999.0,\n              1990.0\n            ],\n            [\n              192165.0,\n              8.4,\n              1997.0,\n              1990.0\n            ],\n            [\n              548750.0,\n              8.4,\n              1994.0,\n              1990.0\n            ],\n            [\n              248514.0,\n              8.3,\n              1992.0,\n              1990.0\n            ],\n            [\n              578684.0,\n              8.3,\n              1992.0,\n              1990.0\n            ],\n            [\n              376590.0,\n              8.3,\n              1997.0,\n              1990.0\n            ],\n            [\n              653769.0,\n              8.3,\n              1995.0,\n              1990.0\n            ],\n            [\n              535249.0,\n              8.3,\n              1995.0,\n              1990.0\n            ],\n            [\n              388239.0,\n              8.2,\n              1995.0,\n              1990.0\n            ],\n            [\n              473988.0,\n              8.2,\n              1998.0,\n              1990.0\n            ],\n            [\n              372863.0,\n              8.2,\n              1998.0,\n              1990.0\n            ],\n            [\n              294394.0,\n              8.2,\n              1995.0,\n              1990.0\n            ],\n            [\n              529800.0,\n              8.2,\n              1997.0,\n              1990.0\n            ],\n            [\n              395997.0,\n              8.1,\n              1996.0,\n              1990.0\n            ],\n            [\n              419372.0,\n              8.1,\n              1996.0,\n              1990.0\n            ],\n            [\n              630994.0,\n              8.1,\n              1999.0,\n              1990.0\n            ],\n            [\n              95212.0,\n              8.1,\n              1993.0,\n              1990.0\n            ],\n            [\n              415809.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              583004.0,\n              8.0,\n              1998.0,\n              1990.0\n            ],\n            [\n              520391.0,\n              8.0,\n              1993.0,\n              1990.0\n            ],\n            [\n              57644.0,\n              8.0,\n              1994.0,\n              1990.0\n            ],\n            [\n              268480.0,\n              8.0,\n              1991.0,\n              1990.0\n            ],\n            [\n              384272.0,\n              8.0,\n              1993.0,\n              1990.0\n            ],\n            [\n              90937.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              158867.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              39447.0,\n              8.0,\n              1995.0,\n              1990.0\n            ]\n          ]\n        }\n      }\n    },\n    {\n      \"type\": \"Subscript\",\n      \"code_step\": \"imdb[imdb.get('Decade') == 1990].sort_values('Rating', ascending=False).index[0]\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 77\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 80\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"cell\",\n            \"anchor\": \"lhs\",\n            \"row\": 0,\n            \"column\": null\n          },\n          \"to\": {\n            \"type\": \"scalar\",\n            \"anchor\": \"rhs\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": \"prev_rhs\",\n        \"rhs\": {\n          \"type\": \"Scalar\",\n          \"py_type\": \"<class 'str'>\",\n          \"data\": \"The Shawshank Redemption\"\n        }\n      }\n    }\n  ]\n}", {"nohover": true, "maxDisplayRows": 7, "maxDisplayCols": 5, "colorPalette": ["#e41a1c", "#377eb8", "#4daf4a", "#984ea3", "#ff7f00", "#a65628", "#f781bf", "#999999"]});
</script>



_Nota:_ O comando para extrair o índice de um DataFrame é `.index` - sem parênteses! Isso é diferente da maneira como extraímos colunas, com `.get()`, porque o índice não é uma coluna.

#### Com agrupamento


```python
#In: 
%%pt
imdb.reset_index().groupby('Decade').max()
```



<div class="pt-viz" id="pt-viz-3"></div>
<script>
drawWsv('#pt-viz-3', "{\n  \"code\": \"imdb.reset_index().groupby('Decade').max()\\n\",\n  \"explanation\": [\n    {\n      \"type\": \"ResetIndexCall\",\n      \"code_step\": \"imdb.reset_index().groupby('Decade').max()\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 4\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 18\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"index_level\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"level\": 0\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"column\",\n            \"label\": \"Title\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Votes\",\n              \"Rating\",\n              \"Year\",\n              \"Decade\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              \"Title\"\n            ],\n            \"labels\": [\n              \"Akira\",\n              \"Per un pugno di dollari\",\n              \"Guardians of the Galaxy\",\n              \"The Man Who Shot Liberty Valance\",\n              \"Underground\",\n              \"Le samoura\\u00ef\",\n              \"Beauty and the Beast\",\n              \"La strada\",\n              \"The Avengers\",\n              \"The King's Speech\",\n              \"Barry Lyndon\",\n              \"Dog Day Afternoon\",\n              \"The Imitation Game\",\n              \"The Truman Show\",\n              \"Roman Holiday\",\n              \"The Hustler\",\n              \"X-Men: Days of Future Past\",\n              \"High Noon\",\n              \"Before Sunset\",\n              \"Monsters, Inc. (2001)\",\n              \"Harry Potter and the Deathly Hallows: Part 2\",\n              \"Lagaan: Once Upon a Time in India\",\n              \"Jurassic Park\",\n              \"Trois couleurs: Rouge\",\n              \"Fanny och Alexander\",\n              \"The Help\",\n              \"Who's Afraid of Virginia Woolf?\",\n              \"The Terminator\",\n              \"Twelve Monkeys\",\n              \"Kumonosu-j\\u00f4\",\n              \"Groundhog Day\",\n              \"Yip Man\",\n              \"Notorious\",\n              \"Pirates of the Caribbean: The Curse of the Black Pearl\",\n              \"La haine\",\n              \"Mou gaan dou\",\n              \"Salinui chueok\",\n              \"The Big Sleep\",\n              \"Paris, Texas (1984)\",\n              \"Rocky\",\n              \"Prisoners\",\n              \"The Killing\",\n              \"The Night of the Hunter\",\n              \"La battaglia di Algeri\",\n              \"Bom yeoreum gaeul gyeoul geurigo bom\",\n              \"Papillon\",\n              \"Before Sunrise\",\n              \"Tenk\\u00fb no shiro Rapyuta\",\n              \"Jaws\",\n              \"Relatos salvajes\",\n              \"Incendies\",\n              \"Hachi: A Dog's Tale\",\n              \"The Best Years of Our Lives\",\n              \"The Wizard of Oz\",\n              \"Cool Hand Luke\",\n              \"In the Name of the Father\",\n              \"The Grapes of Wrath\",\n              \"A Beautiful Mind\",\n              \"Donnie Darko\",\n              \"Mary and Max\",\n              \"Stand by Me\",\n              \"The Grand Budapest Hotel\",\n              \"Butch Cassidy and the Sundance Kid\",\n              \"The Sixth Sense\",\n              \"The Thing\",\n              \"Le salaire de la peur\",\n              \"Persona\",\n              \"Annie Hall\",\n              \"Ben-Hur\",\n              \"8\\u00bd\",\n              \"Fargo\",\n              \"Strangers on a Train\",\n              \"Gone Girl\",\n              \"Million Dollar Baby\",\n              \"Network\",\n              \"It Happened One Night\",\n              \"Trainspotting\",\n              \"Les diaboliques\",\n              \"There Will Be Blood\",\n              \"The Princess Bride\",\n              \"Dial M for Murder\",\n              \"Hotel Rwanda\",\n              \"Sin City\",\n              \"The Deer Hunter\",\n              \"How to Train Your Dragon\",\n              \"Into the Wild\",\n              \"Rush (2013/I)\",\n              \"Gandhi\",\n              \"The Maltese Falcon\",\n              \"Amores perros\",\n              \"12 Years a Slave\",\n              \"The Bourne Ultimatum\",\n              \"Kaze no tani no Naushika\",\n              \"Platoon\",\n              \"Finding Nemo\",\n              \"Stalker\",\n              \"Life of Brian\",\n              \"Gone with the Wind\",\n              \"Touch of Evil\",\n              \"No Country for Old Men\",\n              \"Les quatre cents coups\",\n              \"Shutter Island\",\n              \"Kill Bill: Vol. 1\",\n              \"El secreto de sus ojos\",\n              \"Gran Torino\",\n              \"Smultronst\\u00e4llet\",\n              \"Good Will Hunting\",\n              \"Jagten\",\n              \"The Bridge on the River Kwai\",\n              \"The Elephant Man\",\n              \"Casino\",\n              \"Lock, Stock and Two Smoking Barrels (1998)\",\n              \"The Wolf of Wall Street\",\n              \"Tonari no Totoro\",\n              \"3 Idiots\",\n              \"The General\",\n              \"Judgment at Nuremberg\",\n              \"The Gold Rush\",\n              \"The Big Lebowski\",\n              \"The Great Escape\",\n              \"Up\",\n              \"Warrior\",\n              \"Ran\",\n              \"Der Untergang\",\n              \"Hauru no ugoku shiro\",\n              \"V for Vendetta\",\n              \"Chinatown\",\n              \"El laberinto del fauno\",\n              \"Blade Runner\",\n              \"On the Waterfront\",\n              \"Raging Bull\",\n              \"Det sjunde inseglet\",\n              \"Ikiru\",\n              \"Mr. Smith Goes to Washington\",\n              \"Rebecca\",\n              \"Heat\",\n              \"Die Hard\",\n              \"Per qualche dollaro in pi\\u00f9\",\n              \"Mad Max: Fury Road\",\n              \"Monty Python and the Holy Grail\",\n              \"Witness for the Prosecution\",\n              \"A Clockwork Orange\",\n              \"The Third Man\",\n              \"Braveheart\",\n              \"Amadeus\",\n              \"Indiana Jones and the Last Crusade\",\n              \"The Kid\",\n              \"Y\\u00f4jinb\\u00f4\",\n              \"Jodaeiye Nader az Simin\",\n              \"Eternal Sunshine of the Spotless Mind\",\n              \"The Apartment\",\n              \"Requiem for a Dream\",\n              \"Rash\\u00f4mon\",\n              \"Toy Story 3\",\n              \"Double Indemnity\",\n              \"Metropolis\",\n              \"L.A. Confidential\",\n              \"Snatch.\",\n              \"Lawrence of Arabia\",\n              \"2001: A Space Odyssey\",\n              \"The Sting\",\n              \"The Treasure of the Sierra Madre\",\n              \"Ladri di biciclette\",\n              \"Toy Story\",\n              \"Inglourious Basterds\",\n              \"Singin' in the Rain\",\n              \"All About Eve\",\n              \"To Kill a Mockingbird\",\n              \"Scarface\",\n              \"Full Metal Jacket\",\n              \"Taxi Driver\",\n              \"Reservoir Dogs\",\n              \"Unforgiven\",\n              \"Some Like It Hot\",\n              \"Batman Begins\",\n              \"The Great Dictator\",\n              \"The Shining\",\n              \"Nuovo Cinema Paradiso\",\n              \"The Dark Knight Rises\",\n              \"WALL\\u00b7E\",\n              \"Citizen Kane\",\n              \"Once Upon a Time in America\",\n              \"The Lion King\",\n              \"Das Leben der Anderen\",\n              \"Oldeuboi\",\n              \"Mononoke-hime\",\n              \"Vertigo\",\n              \"Das Boot\",\n              \"M\",\n              \"Star Wars: Episode VI - Return of the Jedi\",\n              \"Le fabuleux destin d'Am\\u00e9lie Poulain\",\n              \"American Beauty\",\n              \"Django Unchained\",\n              \"North by Northwest\",\n              \"Aliens\",\n              \"Paths of Glory\",\n              \"Hotaru no haka\",\n              \"American History X\",\n              \"City Lights\",\n              \"Rear Window\",\n              \"Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb\",\n              \"Terminator 2: Judgment Day\",\n              \"Inside Out (2015/I)\",\n              \"Sen to Chihiro no Kamikakushi\",\n              \"Casablanca\",\n              \"Back to the Future\",\n              \"Modern Times\",\n              \"Saving Private Ryan\",\n              \"Gladiator\",\n              \"Raiders of the Lost Ark\",\n              \"Intouchables\",\n              \"Psycho\",\n              \"Whiplash\",\n              \"The Green Mile\",\n              \"Memento\",\n              \"The Prestige\",\n              \"The Departed\",\n              \"The Pianist\",\n              \"Alien\",\n              \"Apocalypse Now\",\n              \"Sunset Blvd.\",\n              \"Se7en\",\n              \"La vita \\u00e8 bella\",\n              \"Interstellar\",\n              \"It's a Wonderful Life\",\n              \"Cidade de Deus\",\n              \"L\\u00e9on\",\n              \"C'era una volta il West\",\n              \"The Usual Suspects\",\n              \"The Silence of the Lambs\",\n              \"Forrest Gump\",\n              \"The Lord of the Rings: The Two Towers\",\n              \"Star Wars: Episode V - The Empire Strikes Back\",\n              \"The Matrix\",\n              \"Star Wars\",\n              \"Goodfellas\",\n              \"One Flew Over the Cuckoo's Nest\",\n              \"Shichinin no samurai\",\n              \"Inception\",\n              \"Fight Club\",\n              \"The Lord of the Rings: The Fellowship of the Ring\",\n              \"Pulp Fiction\",\n              \"The Dark Knight\",\n              \"Il buono, il brutto, il cattivo (1966)\",\n              \"The Lord of the Rings: The Return of the King\",\n              \"Schindler's List\",\n              \"12 Angry Men\",\n              \"The Godfather: Part II\",\n              \"The Shawshank Redemption\",\n              \"The Godfather\"\n            ]\n          },\n          \"data\": [\n            [\n              91652.0,\n              8.0,\n              1988.0,\n              1980.0\n            ],\n            [\n              124671.0,\n              8.0,\n              1964.0,\n              1960.0\n            ],\n            [\n              527349.0,\n              8.0,\n              2014.0,\n              2010.0\n            ],\n            [\n              49135.0,\n              8.0,\n              1962.0,\n              1960.0\n            ],\n            [\n              39447.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              28012.0,\n              8.0,\n              1967.0,\n              1960.0\n            ],\n            [\n              268480.0,\n              8.0,\n              1991.0,\n              1990.0\n            ],\n            [\n              42446.0,\n              8.0,\n              1954.0,\n              1950.0\n            ],\n            [\n              862016.0,\n              8.0,\n              2012.0,\n              2010.0\n            ],\n            [\n              434906.0,\n              8.0,\n              2010.0,\n              2010.0\n            ],\n            [\n              89920.0,\n              8.0,\n              1975.0,\n              1970.0\n            ],\n            [\n              164419.0,\n              8.0,\n              1975.0,\n              1970.0\n            ],\n            [\n              321834.0,\n              8.0,\n              2014.0,\n              2010.0\n            ],\n            [\n              583004.0,\n              8.0,\n              1998.0,\n              1990.0\n            ],\n            [\n              87437.0,\n              8.0,\n              1953.0,\n              1950.0\n            ],\n            [\n              57842.0,\n              8.0,\n              1961.0,\n              1960.0\n            ],\n            [\n              427099.0,\n              8.0,\n              2014.0,\n              2010.0\n            ],\n            [\n              72007.0,\n              8.0,\n              1952.0,\n              1950.0\n            ],\n            [\n              147277.0,\n              8.0,\n              2004.0,\n              2000.0\n            ],\n            [\n              500576.0,\n              8.0,\n              2001.0,\n              2000.0\n            ],\n            [\n              466111.0,\n              8.0,\n              2011.0,\n              2010.0\n            ],\n            [\n              59377.0,\n              8.0,\n              2001.0,\n              2000.0\n            ],\n            [\n              520391.0,\n              8.0,\n              1993.0,\n              1990.0\n            ],\n            [\n              57644.0,\n              8.0,\n              1994.0,\n              1990.0\n            ],\n            [\n              37972.0,\n              8.0,\n              1982.0,\n              1980.0\n            ],\n            [\n              282774.0,\n              8.0,\n              2011.0,\n              2010.0\n            ],\n            [\n              50208.0,\n              8.0,\n              1966.0,\n              1960.0\n            ],\n            [\n              527274.0,\n              8.0,\n              1984.0,\n              1980.0\n            ],\n            [\n              415809.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              26012.0,\n              8.0,\n              1957.0,\n              1950.0\n            ],\n            [\n              384272.0,\n              8.0,\n              1993.0,\n              1990.0\n            ],\n            [\n              138240.0,\n              8.0,\n              2008.0,\n              2000.0\n            ],\n            [\n              66622.0,\n              8.0,\n              1946.0,\n              1940.0\n            ],\n            [\n              718645.0,\n              8.0,\n              2003.0,\n              2000.0\n            ],\n            [\n              90937.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              82440.0,\n              8.0,\n              2002.0,\n              2000.0\n            ],\n            [\n              54651.0,\n              8.0,\n              2003.0,\n              2000.0\n            ],\n            [\n              59578.0,\n              8.0,\n              1946.0,\n              1940.0\n            ],\n            [\n              43090.0,\n              8.0,\n              1984.0,\n              1980.0\n            ],\n            [\n              318041.0,\n              8.0,\n              1976.0,\n              1970.0\n            ],\n            [\n              312516.0,\n              8.0,\n              2013.0,\n              2010.0\n            ],\n            [\n              56671.0,\n              8.0,\n              1956.0,\n              1950.0\n            ],\n            [\n              57974.0,\n              8.0,\n              1955.0,\n              1950.0\n            ],\n            [\n              32385.0,\n              8.0,\n              1966.0,\n              1960.0\n            ],\n            [\n              55382.0,\n              8.0,\n              2003.0,\n              2000.0\n            ],\n            [\n              87591.0,\n              8.0,\n              1973.0,\n              1970.0\n            ],\n            [\n              158867.0,\n              8.0,\n              1995.0,\n              1990.0\n            ],\n            [\n              83941.0,\n              8.0,\n              1986.0,\n              1980.0\n            ],\n            [\n              364225.0,\n              8.0,\n              1975.0,\n              1970.0\n            ],\n            [\n              46987.0,\n              8.0,\n              2014.0,\n              2010.0\n            ],\n            [\n              65122.0,\n              8.1,\n              2010.0,\n              2010.0\n            ],\n            [\n              121599.0,\n              8.1,\n              2009.0,\n              2000.0\n            ],\n            [\n              35983.0,\n              8.1,\n              1946.0,\n              1940.0\n            ],\n            [\n              259235.0,\n              8.1,\n              1939.0,\n              1930.0\n            ],\n            [\n              108128.0,\n              8.1,\n              1967.0,\n              1960.0\n            ],\n            [\n              95212.0,\n              8.1,\n              1993.0,\n              1990.0\n            ],\n            [\n              55793.0,\n              8.1,\n              1940.0,\n              1940.0\n            ],\n            [\n              525515.0,\n              8.1,\n              2001.0,\n              2000.0\n            ],\n            [\n              522119.0,\n              8.1,\n              2001.0,\n              2000.0\n            ],\n            [\n              102735.0,\n              8.1,\n              2009.0,\n              2000.0\n            ],\n            [\n              242381.0,\n              8.1,\n              1986.0,\n              1980.0\n            ],\n            [\n              369141.0,\n              8.1,\n              2014.0,\n              2010.0\n            ],\n            [\n              134552.0,\n              8.1,\n              1969.0,\n              1960.0\n            ],\n            [\n              630994.0,\n              8.1,\n              1999.0,\n              1990.0\n            ],\n            [\n              228230.0,\n              8.1,\n              1982.0,\n              1980.0\n            ],\n            [\n              31003.0,\n              8.1,\n              1953.0,\n              1950.0\n            ],\n            [\n              52573.0,\n              8.1,\n              1966.0,\n              1960.0\n            ],\n            [\n              169982.0,\n              8.1,\n              1977.0,\n              1970.0\n            ],\n            [\n              141768.0,\n              8.1,\n              1959.0,\n              1950.0\n            ],\n            [\n              69988.0,\n              8.1,\n              1963.0,\n              1960.0\n            ],\n            [\n              395997.0,\n              8.1,\n              1996.0,\n              1990.0\n            ],\n            [\n              85012.0,\n              8.1,\n              1951.0,\n              1950.0\n            ],\n            [\n              441174.0,\n              8.1,\n              2014.0,\n              2010.0\n            ],\n            [\n              424957.0,\n              8.1,\n              2004.0,\n              2000.0\n            ],\n            [\n              91689.0,\n              8.1,\n              1976.0,\n              1970.0\n            ],\n            [\n              56842.0,\n              8.1,\n              1934.0,\n              1930.0\n            ],\n            [\n              419372.0,\n              8.1,\n              1996.0,\n              1990.0\n            ],\n            [\n              36725.0,\n              8.1,\n              1955.0,\n              1950.0\n            ],\n            [\n              331348.0,\n              8.1,\n              2007.0,\n              2000.0\n            ],\n            [\n              264285.0,\n              8.1,\n              1987.0,\n              1980.0\n            ],\n            [\n              92244.0,\n              8.1,\n              1954.0,\n              1950.0\n            ],\n            [\n              237292.0,\n              8.1,\n              2004.0,\n              2000.0\n            ],\n            [\n              596880.0,\n              8.1,\n              2005.0,\n              2000.0\n            ],\n            [\n              209677.0,\n              8.1,\n              1978.0,\n              1970.0\n            ],\n            [\n              415845.0,\n              8.1,\n              2010.0,\n              2010.0\n            ],\n            [\n              372803.0,\n              8.1,\n              2007.0,\n              2000.0\n            ],\n            [\n              258882.0,\n              8.1,\n              2013.0,\n              2010.0\n            ],\n            [\n              151697.0,\n              8.1,\n              1982.0,\n              1980.0\n            ],\n            [\n              101754.0,\n              8.1,\n              1941.0,\n              1940.0\n            ],\n            [\n              156030.0,\n              8.1,\n              2000.0,\n              2000.0\n            ],\n            [\n              359121.0,\n              8.1,\n              2013.0,\n              2010.0\n            ],\n            [\n              433487.0,\n              8.1,\n              2007.0,\n              2000.0\n            ],\n            [\n              81809.0,\n              8.1,\n              1984.0,\n              1980.0\n            ],\n            [\n              262404.0,\n              8.1,\n              1986.0,\n              1980.0\n            ],\n            [\n              591219.0,\n              8.1,\n              2003.0,\n              2000.0\n            ],\n            [\n              59272.0,\n              8.1,\n              1979.0,\n              1970.0\n            ],\n            [\n              242886.0,\n              8.1,\n              1979.0,\n              1970.0\n            ],\n            [\n              192791.0,\n              8.1,\n              1939.0,\n              1930.0\n            ],\n            [\n              65408.0,\n              8.1,\n              1958.0,\n              1950.0\n            ],\n            [\n              540744.0,\n              8.1,\n              2007.0,\n              2000.0\n            ],\n            [\n              61776.0,\n              8.1,\n              1959.0,\n              1950.0\n            ],\n            [\n              670328.0,\n              8.1,\n              2010.0,\n              2010.0\n            ],\n            [\n              647120.0,\n              8.1,\n              2003.0,\n              2000.0\n            ],\n            [\n              109254.0,\n              8.2,\n              2009.0,\n              2000.0\n            ],\n            [\n              502773.0,\n              8.2,\n              2008.0,\n              2000.0\n            ],\n            [\n              55861.0,\n              8.2,\n              1957.0,\n              1950.0\n            ],\n            [\n              529800.0,\n              8.2,\n              1997.0,\n              1990.0\n            ],\n            [\n              137310.0,\n              8.2,\n              2012.0,\n              2010.0\n            ],\n            [\n              132677.0,\n              8.2,\n              1957.0,\n              1950.0\n            ],\n            [\n              145514.0,\n              8.2,\n              1980.0,\n              1980.0\n            ],\n            [\n              294394.0,\n              8.2,\n              1995.0,\n              1990.0\n            ],\n            [\n              372863.0,\n              8.2,\n              1998.0,\n              1990.0\n            ],\n            [\n              624943.0,\n              8.2,\n              2013.0,\n              2010.0\n            ],\n            [\n              142605.0,\n              8.2,\n              1988.0,\n              1980.0\n            ],\n            [\n              167076.0,\n              8.2,\n              2009.0,\n              2000.0\n            ],\n            [\n              46332.0,\n              8.2,\n              1926.0,\n              1920.0\n            ],\n            [\n              37664.0,\n              8.2,\n              1961.0,\n              1960.0\n            ],\n            [\n              58506.0,\n              8.2,\n              1925.0,\n              1920.0\n            ],\n            [\n              473988.0,\n              8.2,\n              1998.0,\n              1990.0\n            ],\n            [\n              147372.0,\n              8.2,\n              1963.0,\n              1960.0\n            ],\n            [\n              566091.0,\n              8.2,\n              2009.0,\n              2000.0\n            ],\n            [\n              287727.0,\n              8.2,\n              2011.0,\n              2010.0\n            ],\n            [\n              69287.0,\n              8.2,\n              1985.0,\n              1980.0\n            ],\n            [\n              223834.0,\n              8.2,\n              2004.0,\n              2000.0\n            ],\n            [\n              182525.0,\n              8.2,\n              2004.0,\n              2000.0\n            ],\n            [\n              700999.0,\n              8.2,\n              2005.0,\n              2000.0\n            ],\n            [\n              192744.0,\n              8.2,\n              1974.0,\n              1970.0\n            ],\n            [\n              416467.0,\n              8.2,\n              2006.0,\n              2000.0\n            ],\n            [\n              415615.0,\n              8.2,\n              1982.0,\n              1980.0\n            ],\n            [\n              89233.0,\n              8.2,\n              1954.0,\n              1950.0\n            ],\n            [\n              210647.0,\n              8.2,\n              1980.0,\n              1980.0\n            ],\n            [\n              98949.0,\n              8.2,\n              1957.0,\n              1950.0\n            ],\n            [\n              36638.0,\n              8.2,\n              1952.0,\n              1950.0\n            ],\n            [\n              69510.0,\n              8.2,\n              1939.0,\n              1930.0\n            ],\n            [\n              78216.0,\n              8.2,\n              1940.0,\n              1940.0\n            ],\n            [\n              388239.0,\n              8.2,\n              1995.0,\n              1990.0\n            ],\n            [\n              529287.0,\n              8.2,\n              1988.0,\n              1980.0\n            ],\n            [\n              135556.0,\n              8.3,\n              1965.0,\n              1960.0\n            ],\n            [\n              262425.0,\n              8.3,\n              2015.0,\n              2010.0\n            ],\n            [\n              343300.0,\n              8.3,\n              1975.0,\n              1970.0\n            ],\n            [\n              53186.0,\n              8.3,\n              1957.0,\n              1950.0\n            ],\n            [\n              489807.0,\n              8.3,\n              1971.0,\n              1970.0\n            ],\n            [\n              101785.0,\n              8.3,\n              1949.0,\n              1940.0\n            ],\n            [\n              653769.0,\n              8.3,\n              1995.0,\n              1990.0\n            ],\n            [\n              240308.0,\n              8.3,\n              1984.0,\n              1980.0\n            ],\n            [\n              453748.0,\n              8.3,\n              1989.0,\n              1980.0\n            ],\n            [\n              55784.0,\n              8.3,\n              1921.0,\n              1920.0\n            ],\n            [\n              65370.0,\n              8.3,\n              1961.0,\n              1960.0\n            ],\n            [\n              133760.0,\n              8.3,\n              2011.0,\n              2010.0\n            ],\n            [\n              589477.0,\n              8.3,\n              2004.0,\n              2000.0\n            ],\n            [\n              96190.0,\n              8.3,\n              1960.0,\n              1960.0\n            ],\n            [\n              511177.0,\n              8.3,\n              2000.0,\n              2000.0\n            ],\n            [\n              90434.0,\n              8.3,\n              1950.0,\n              1950.0\n            ],\n            [\n              470158.0,\n              8.3,\n              2010.0,\n              2010.0\n            ],\n            [\n              86715.0,\n              8.3,\n              1944.0,\n              1940.0\n            ],\n            [\n              98794.0,\n              8.3,\n              1927.0,\n              1920.0\n            ],\n            [\n              376590.0,\n              8.3,\n              1997.0,\n              1990.0\n            ],\n            [\n              536053.0,\n              8.3,\n              2000.0,\n              2000.0\n            ],\n            [\n              170302.0,\n              8.3,\n              1962.0,\n              1960.0\n            ],\n            [\n              373482.0,\n              8.3,\n              1968.0,\n              1960.0\n            ],\n            [\n              155691.0,\n              8.3,\n              1973.0,\n              1970.0\n            ],\n            [\n              71003.0,\n              8.3,\n              1948.0,\n              1940.0\n            ],\n            [\n              81887.0,\n              8.3,\n              1948.0,\n              1940.0\n            ],\n            [\n              535249.0,\n              8.3,\n              1995.0,\n              1990.0\n            ],\n            [\n              755013.0,\n              8.3,\n              2009.0,\n              2000.0\n            ],\n            [\n              132823.0,\n              8.3,\n              1952.0,\n              1950.0\n            ],\n            [\n              74178.0,\n              8.3,\n              1950.0,\n              1950.0\n            ],\n            [\n              188160.0,\n              8.3,\n              1962.0,\n              1960.0\n            ],\n            [\n              475594.0,\n              8.3,\n              1983.0,\n              1980.0\n            ],\n            [\n              425461.0,\n              8.3,\n              1987.0,\n              1980.0\n            ],\n            [\n              444534.0,\n              8.3,\n              1976.0,\n              1970.0\n            ],\n            [\n              578684.0,\n              8.3,\n              1992.0,\n              1990.0\n            ],\n            [\n              248514.0,\n              8.3,\n              1992.0,\n              1990.0\n            ],\n            [\n              156432.0,\n              8.3,\n              1959.0,\n              1950.0\n            ],\n            [\n              850601.0,\n              8.3,\n              2005.0,\n              2000.0\n            ],\n            [\n              117590.0,\n              8.4,\n              1940.0,\n              1940.0\n            ],\n            [\n              532559.0,\n              8.4,\n              1980.0,\n              1980.0\n            ],\n            [\n              132121.0,\n              8.4,\n              1988.0,\n              1980.0\n            ],\n            [\n              997350.0,\n              8.4,\n              2012.0,\n              2010.0\n            ],\n            [\n              618914.0,\n              8.4,\n              2008.0,\n              2000.0\n            ],\n            [\n              268905.0,\n              8.4,\n              1941.0,\n              1940.0\n            ],\n            [\n              195266.0,\n              8.4,\n              1984.0,\n              1980.0\n            ],\n            [\n              548750.0,\n              8.4,\n              1994.0,\n              1990.0\n            ],\n            [\n              229533.0,\n              8.4,\n              2006.0,\n              2000.0\n            ],\n            [\n              311070.0,\n              8.4,\n              2003.0,\n              2000.0\n            ],\n            [\n              192165.0,\n              8.4,\n              1997.0,\n              1990.0\n            ],\n            [\n              218430.0,\n              8.4,\n              1958.0,\n              1950.0\n            ],\n            [\n              151256.0,\n              8.4,\n              1981.0,\n              1980.0\n            ],\n            [\n              88355.0,\n              8.4,\n              1931.0,\n              1930.0\n            ],\n            [\n              556811.0,\n              8.4,\n              1983.0,\n              1980.0\n            ],\n            [\n              474068.0,\n              8.4,\n              2001.0,\n              2000.0\n            ],\n            [\n              735056.0,\n              8.4,\n              1999.0,\n              1990.0\n            ],\n            [\n              804729.0,\n              8.4,\n              2012.0,\n              2010.0\n            ],\n            [\n              198795.0,\n              8.4,\n              1959.0,\n              1950.0\n            ],\n            [\n              436218.0,\n              8.4,\n              1986.0,\n              1980.0\n            ],\n            [\n              106038.0,\n              8.4,\n              1957.0,\n              1950.0\n            ],\n            [\n              115740.0,\n              8.4,\n              1988.0,\n              1980.0\n            ],\n            [\n              694602.0,\n              8.5,\n              1998.0,\n              1990.0\n            ],\n            [\n              92375.0,\n              8.5,\n              1931.0,\n              1930.0\n            ],\n            [\n              280432.0,\n              8.5,\n              1954.0,\n              1950.0\n            ],\n            [\n              309141.0,\n              8.5,\n              1964.0,\n              1960.0\n            ],\n            [\n              658564.0,\n              8.5,\n              1991.0,\n              1990.0\n            ],\n            [\n              79615.0,\n              8.5,\n              2015.0,\n              2010.0\n            ],\n            [\n              357355.0,\n              8.5,\n              2001.0,\n              2000.0\n            ],\n            [\n              350551.0,\n              8.5,\n              1942.0,\n              1940.0\n            ],\n            [\n              625463.0,\n              8.5,\n              1985.0,\n              1980.0\n            ],\n            [\n              121668.0,\n              8.5,\n              1936.0,\n              1930.0\n            ],\n            [\n              769893.0,\n              8.5,\n              1998.0,\n              1990.0\n            ],\n            [\n              862216.0,\n              8.5,\n              2000.0,\n              2000.0\n            ],\n            [\n              585474.0,\n              8.5,\n              1981.0,\n              1980.0\n            ],\n            [\n              417122.0,\n              8.5,\n              2011.0,\n              2010.0\n            ],\n            [\n              371791.0,\n              8.5,\n              1960.0,\n              1960.0\n            ],\n            [\n              264333.0,\n              8.5,\n              2014.0,\n              2010.0\n            ],\n            [\n              672878.0,\n              8.5,\n              1999.0,\n              1990.0\n            ],\n            [\n              752122.0,\n              8.5,\n              2000.0,\n              2000.0\n            ],\n            [\n              733490.0,\n              8.5,\n              2006.0,\n              2000.0\n            ],\n            [\n              763871.0,\n              8.5,\n              2006.0,\n              2000.0\n            ],\n            [\n              431856.0,\n              8.5,\n              2002.0,\n              2000.0\n            ],\n            [\n              496833.0,\n              8.5,\n              1979.0,\n              1970.0\n            ],\n            [\n              403754.0,\n              8.5,\n              1979.0,\n              1970.0\n            ],\n            [\n              123879.0,\n              8.5,\n              1950.0,\n              1950.0\n            ],\n            [\n              895411.0,\n              8.6,\n              1995.0,\n              1990.0\n            ],\n            [\n              358305.0,\n              8.6,\n              1997.0,\n              1990.0\n            ],\n            [\n              689541.0,\n              8.6,\n              2014.0,\n              2010.0\n            ],\n            [\n              242353.0,\n              8.6,\n              1946.0,\n              1940.0\n            ],\n            [\n              476501.0,\n              8.6,\n              2002.0,\n              2000.0\n            ],\n            [\n              635139.0,\n              8.6,\n              1994.0,\n              1990.0\n            ],\n            [\n              192206.0,\n              8.6,\n              1968.0,\n              1960.0\n            ],\n            [\n              656756.0,\n              8.6,\n              1995.0,\n              1990.0\n            ],\n            [\n              767224.0,\n              8.6,\n              1991.0,\n              1990.0\n            ],\n            [\n              1078416.0,\n              8.7,\n              1994.0,\n              1990.0\n            ],\n            [\n              967389.0,\n              8.7,\n              2002.0,\n              2000.0\n            ],\n            [\n              700283.0,\n              8.7,\n              1980.0,\n              1980.0\n            ],\n            [\n              1073043.0,\n              8.7,\n              1999.0,\n              1990.0\n            ],\n            [\n              770011.0,\n              8.7,\n              1977.0,\n              1970.0\n            ],\n            [\n              644556.0,\n              8.7,\n              1990.0,\n              1990.0\n            ],\n            [\n              606395.0,\n              8.7,\n              1975.0,\n              1970.0\n            ],\n            [\n              206216.0,\n              8.7,\n              1954.0,\n              1950.0\n            ],\n            [\n              1271949.0,\n              8.7,\n              2010.0,\n              2010.0\n            ],\n            [\n              1177098.0,\n              8.8,\n              1999.0,\n              1990.0\n            ],\n            [\n              1099087.0,\n              8.8,\n              2001.0,\n              2000.0\n            ],\n            [\n              1166532.0,\n              8.9,\n              1994.0,\n              1990.0\n            ],\n            [\n              1473049.0,\n              8.9,\n              2008.0,\n              2000.0\n            ],\n            [\n              447875.0,\n              8.9,\n              1966.0,\n              1960.0\n            ],\n            [\n              1074146.0,\n              8.9,\n              2003.0,\n              2000.0\n            ],\n            [\n              761224.0,\n              8.9,\n              1993.0,\n              1990.0\n            ],\n            [\n              384187.0,\n              8.9,\n              1957.0,\n              1950.0\n            ],\n            [\n              692753.0,\n              9.0,\n              1974.0,\n              1970.0\n            ],\n            [\n              1498733.0,\n              9.2,\n              1994.0,\n              1990.0\n            ],\n            [\n              1027398.0,\n              9.2,\n              1972.0,\n              1970.0\n            ]\n          ]\n        },\n        \"rhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Title\",\n              \"Votes\",\n              \"Rating\",\n              \"Year\",\n              \"Decade\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              0,\n              1,\n              2,\n              3,\n              4,\n              5,\n              6,\n              7,\n              8,\n              9,\n              10,\n              11,\n              12,\n              13,\n              14,\n              15,\n              16,\n              17,\n              18,\n              19,\n              20,\n              21,\n              22,\n              23,\n              24,\n              25,\n              26,\n              27,\n              28,\n              29,\n              30,\n              31,\n              32,\n              33,\n              34,\n              35,\n              36,\n              37,\n              38,\n              39,\n              40,\n              41,\n              42,\n              43,\n              44,\n              45,\n              46,\n              47,\n              48,\n              49,\n              50,\n              51,\n              52,\n              53,\n              54,\n              55,\n              56,\n              57,\n              58,\n              59,\n              60,\n              61,\n              62,\n              63,\n              64,\n              65,\n              66,\n              67,\n              68,\n              69,\n              70,\n              71,\n              72,\n              73,\n              74,\n              75,\n              76,\n              77,\n              78,\n              79,\n              80,\n              81,\n              82,\n              83,\n              84,\n              85,\n              86,\n              87,\n              88,\n              89,\n              90,\n              91,\n              92,\n              93,\n              94,\n              95,\n              96,\n              97,\n              98,\n              99,\n              100,\n              101,\n              102,\n              103,\n              104,\n              105,\n              106,\n              107,\n              108,\n              109,\n              110,\n              111,\n              112,\n              113,\n              114,\n              115,\n              116,\n              117,\n              118,\n              119,\n              120,\n              121,\n              122,\n              123,\n              124,\n              125,\n              126,\n              127,\n              128,\n              129,\n              130,\n              131,\n              132,\n              133,\n              134,\n              135,\n              136,\n              137,\n              138,\n              139,\n              140,\n              141,\n              142,\n              143,\n              144,\n              145,\n              146,\n              147,\n              148,\n              149,\n              150,\n              151,\n              152,\n              153,\n              154,\n              155,\n              156,\n              157,\n              158,\n              159,\n              160,\n              161,\n              162,\n              163,\n              164,\n              165,\n              166,\n              167,\n              168,\n              169,\n              170,\n              171,\n              172,\n              173,\n              174,\n              175,\n              176,\n              177,\n              178,\n              179,\n              180,\n              181,\n              182,\n              183,\n              184,\n              185,\n              186,\n              187,\n              188,\n              189,\n              190,\n              191,\n              192,\n              193,\n              194,\n              195,\n              196,\n              197,\n              198,\n              199,\n              200,\n              201,\n              202,\n              203,\n              204,\n              205,\n              206,\n              207,\n              208,\n              209,\n              210,\n              211,\n              212,\n              213,\n              214,\n              215,\n              216,\n              217,\n              218,\n              219,\n              220,\n              221,\n              222,\n              223,\n              224,\n              225,\n              226,\n              227,\n              228,\n              229,\n              230,\n              231,\n              232,\n              233,\n              234,\n              235,\n              236,\n              237,\n              238,\n              239,\n              240,\n              241,\n              242,\n              243,\n              244,\n              245,\n              246,\n              247,\n              248,\n              249\n            ]\n          },\n          \"data\": [\n            [\n              \"Akira\",\n              91652,\n              8.0,\n              1988,\n              1980\n            ],\n            [\n              \"Per un pugno di dollari\",\n              124671,\n              8.0,\n              1964,\n              1960\n            ],\n            [\n              \"Guardians of the Galaxy\",\n              527349,\n              8.0,\n              2014,\n              2010\n            ],\n            [\n              \"The Man Who Shot Liberty Valance\",\n              49135,\n              8.0,\n              1962,\n              1960\n            ],\n            [\n              \"Underground\",\n              39447,\n              8.0,\n              1995,\n              1990\n            ],\n            [\n              \"Le samoura\\u00ef\",\n              28012,\n              8.0,\n              1967,\n              1960\n            ],\n            [\n              \"Beauty and the Beast\",\n              268480,\n              8.0,\n              1991,\n              1990\n            ],\n            [\n              \"La strada\",\n              42446,\n              8.0,\n              1954,\n              1950\n            ],\n            [\n              \"The Avengers\",\n              862016,\n              8.0,\n              2012,\n              2010\n            ],\n            [\n              \"The King's Speech\",\n              434906,\n              8.0,\n              2010,\n              2010\n            ],\n            [\n              \"Barry Lyndon\",\n              89920,\n              8.0,\n              1975,\n              1970\n            ],\n            [\n              \"Dog Day Afternoon\",\n              164419,\n              8.0,\n              1975,\n              1970\n            ],\n            [\n              \"The Imitation Game\",\n              321834,\n              8.0,\n              2014,\n              2010\n            ],\n            [\n              \"The Truman Show\",\n              583004,\n              8.0,\n              1998,\n              1990\n            ],\n            [\n              \"Roman Holiday\",\n              87437,\n              8.0,\n              1953,\n              1950\n            ],\n            [\n              \"The Hustler\",\n              57842,\n              8.0,\n              1961,\n              1960\n            ],\n            [\n              \"X-Men: Days of Future Past\",\n              427099,\n              8.0,\n              2014,\n              2010\n            ],\n            [\n              \"High Noon\",\n              72007,\n              8.0,\n              1952,\n              1950\n            ],\n            [\n              \"Before Sunset\",\n              147277,\n              8.0,\n              2004,\n              2000\n            ],\n            [\n              \"Monsters, Inc. (2001)\",\n              500576,\n              8.0,\n              2001,\n              2000\n            ],\n            [\n              \"Harry Potter and the Deathly Hallows: Part 2\",\n              466111,\n              8.0,\n              2011,\n              2010\n            ],\n            [\n              \"Lagaan: Once Upon a Time in India\",\n              59377,\n              8.0,\n              2001,\n              2000\n            ],\n            [\n              \"Jurassic Park\",\n              520391,\n              8.0,\n              1993,\n              1990\n            ],\n            [\n              \"Trois couleurs: Rouge\",\n              57644,\n              8.0,\n              1994,\n              1990\n            ],\n            [\n              \"Fanny och Alexander\",\n              37972,\n              8.0,\n              1982,\n              1980\n            ],\n            [\n              \"The Help\",\n              282774,\n              8.0,\n              2011,\n              2010\n            ],\n            [\n              \"Who's Afraid of Virginia Woolf?\",\n              50208,\n              8.0,\n              1966,\n              1960\n            ],\n            [\n              \"The Terminator\",\n              527274,\n              8.0,\n              1984,\n              1980\n            ],\n            [\n              \"Twelve Monkeys\",\n              415809,\n              8.0,\n              1995,\n              1990\n            ],\n            [\n              \"Kumonosu-j\\u00f4\",\n              26012,\n              8.0,\n              1957,\n              1950\n            ],\n            [\n              \"Groundhog Day\",\n              384272,\n              8.0,\n              1993,\n              1990\n            ],\n            [\n              \"Yip Man\",\n              138240,\n              8.0,\n              2008,\n              2000\n            ],\n            [\n              \"Notorious\",\n              66622,\n              8.0,\n              1946,\n              1940\n            ],\n            [\n              \"Pirates of the Caribbean: The Curse of the Black Pearl\",\n              718645,\n              8.0,\n              2003,\n              2000\n            ],\n            [\n              \"La haine\",\n              90937,\n              8.0,\n              1995,\n              1990\n            ],\n            [\n              \"Mou gaan dou\",\n              82440,\n              8.0,\n              2002,\n              2000\n            ],\n            [\n              \"Salinui chueok\",\n              54651,\n              8.0,\n              2003,\n              2000\n            ],\n            [\n              \"The Big Sleep\",\n              59578,\n              8.0,\n              1946,\n              1940\n            ],\n            [\n              \"Paris, Texas (1984)\",\n              43090,\n              8.0,\n              1984,\n              1980\n            ],\n            [\n              \"Rocky\",\n              318041,\n              8.0,\n              1976,\n              1970\n            ],\n            [\n              \"Prisoners\",\n              312516,\n              8.0,\n              2013,\n              2010\n            ],\n            [\n              \"The Killing\",\n              56671,\n              8.0,\n              1956,\n              1950\n            ],\n            [\n              \"The Night of the Hunter\",\n              57974,\n              8.0,\n              1955,\n              1950\n            ],\n            [\n              \"La battaglia di Algeri\",\n              32385,\n              8.0,\n              1966,\n              1960\n            ],\n            [\n              \"Bom yeoreum gaeul gyeoul geurigo bom\",\n              55382,\n              8.0,\n              2003,\n              2000\n            ],\n            [\n              \"Papillon\",\n              87591,\n              8.0,\n              1973,\n              1970\n            ],\n            [\n              \"Before Sunrise\",\n              158867,\n              8.0,\n              1995,\n              1990\n            ],\n            [\n              \"Tenk\\u00fb no shiro Rapyuta\",\n              83941,\n              8.0,\n              1986,\n              1980\n            ],\n            [\n              \"Jaws\",\n              364225,\n              8.0,\n              1975,\n              1970\n            ],\n            [\n              \"Relatos salvajes\",\n              46987,\n              8.0,\n              2014,\n              2010\n            ],\n            [\n              \"Incendies\",\n              65122,\n              8.1,\n              2010,\n              2010\n            ],\n            [\n              \"Hachi: A Dog's Tale\",\n              121599,\n              8.1,\n              2009,\n              2000\n            ],\n            [\n              \"The Best Years of Our Lives\",\n              35983,\n              8.1,\n              1946,\n              1940\n            ],\n            [\n              \"The Wizard of Oz\",\n              259235,\n              8.1,\n              1939,\n              1930\n            ],\n            [\n              \"Cool Hand Luke\",\n              108128,\n              8.1,\n              1967,\n              1960\n            ],\n            [\n              \"In the Name of the Father\",\n              95212,\n              8.1,\n              1993,\n              1990\n            ],\n            [\n              \"The Grapes of Wrath\",\n              55793,\n              8.1,\n              1940,\n              1940\n            ],\n            [\n              \"A Beautiful Mind\",\n              525515,\n              8.1,\n              2001,\n              2000\n            ],\n            [\n              \"Donnie Darko\",\n              522119,\n              8.1,\n              2001,\n              2000\n            ],\n            [\n              \"Mary and Max\",\n              102735,\n              8.1,\n              2009,\n              2000\n            ],\n            [\n              \"Stand by Me\",\n              242381,\n              8.1,\n              1986,\n              1980\n            ],\n            [\n              \"The Grand Budapest Hotel\",\n              369141,\n              8.1,\n              2014,\n              2010\n            ],\n            [\n              \"Butch Cassidy and the Sundance Kid\",\n              134552,\n              8.1,\n              1969,\n              1960\n            ],\n            [\n              \"The Sixth Sense\",\n              630994,\n              8.1,\n              1999,\n              1990\n            ],\n            [\n              \"The Thing\",\n              228230,\n              8.1,\n              1982,\n              1980\n            ],\n            [\n              \"Le salaire de la peur\",\n              31003,\n              8.1,\n              1953,\n              1950\n            ],\n            [\n              \"Persona\",\n              52573,\n              8.1,\n              1966,\n              1960\n            ],\n            [\n              \"Annie Hall\",\n              169982,\n              8.1,\n              1977,\n              1970\n            ],\n            [\n              \"Ben-Hur\",\n              141768,\n              8.1,\n              1959,\n              1950\n            ],\n            [\n              \"8\\u00bd\",\n              69988,\n              8.1,\n              1963,\n              1960\n            ],\n            [\n              \"Fargo\",\n              395997,\n              8.1,\n              1996,\n              1990\n            ],\n            [\n              \"Strangers on a Train\",\n              85012,\n              8.1,\n              1951,\n              1950\n            ],\n            [\n              \"Gone Girl\",\n              441174,\n              8.1,\n              2014,\n              2010\n            ],\n            [\n              \"Million Dollar Baby\",\n              424957,\n              8.1,\n              2004,\n              2000\n            ],\n            [\n              \"Network\",\n              91689,\n              8.1,\n              1976,\n              1970\n            ],\n            [\n              \"It Happened One Night\",\n              56842,\n              8.1,\n              1934,\n              1930\n            ],\n            [\n              \"Trainspotting\",\n              419372,\n              8.1,\n              1996,\n              1990\n            ],\n            [\n              \"Les diaboliques\",\n              36725,\n              8.1,\n              1955,\n              1950\n            ],\n            [\n              \"There Will Be Blood\",\n              331348,\n              8.1,\n              2007,\n              2000\n            ],\n            [\n              \"The Princess Bride\",\n              264285,\n              8.1,\n              1987,\n              1980\n            ],\n            [\n              \"Dial M for Murder\",\n              92244,\n              8.1,\n              1954,\n              1950\n            ],\n            [\n              \"Hotel Rwanda\",\n              237292,\n              8.1,\n              2004,\n              2000\n            ],\n            [\n              \"Sin City\",\n              596880,\n              8.1,\n              2005,\n              2000\n            ],\n            [\n              \"The Deer Hunter\",\n              209677,\n              8.1,\n              1978,\n              1970\n            ],\n            [\n              \"How to Train Your Dragon\",\n              415845,\n              8.1,\n              2010,\n              2010\n            ],\n            [\n              \"Into the Wild\",\n              372803,\n              8.1,\n              2007,\n              2000\n            ],\n            [\n              \"Rush (2013/I)\",\n              258882,\n              8.1,\n              2013,\n              2010\n            ],\n            [\n              \"Gandhi\",\n              151697,\n              8.1,\n              1982,\n              1980\n            ],\n            [\n              \"The Maltese Falcon\",\n              101754,\n              8.1,\n              1941,\n              1940\n            ],\n            [\n              \"Amores perros\",\n              156030,\n              8.1,\n              2000,\n              2000\n            ],\n            [\n              \"12 Years a Slave\",\n              359121,\n              8.1,\n              2013,\n              2010\n            ],\n            [\n              \"The Bourne Ultimatum\",\n              433487,\n              8.1,\n              2007,\n              2000\n            ],\n            [\n              \"Kaze no tani no Naushika\",\n              81809,\n              8.1,\n              1984,\n              1980\n            ],\n            [\n              \"Platoon\",\n              262404,\n              8.1,\n              1986,\n              1980\n            ],\n            [\n              \"Finding Nemo\",\n              591219,\n              8.1,\n              2003,\n              2000\n            ],\n            [\n              \"Stalker\",\n              59272,\n              8.1,\n              1979,\n              1970\n            ],\n            [\n              \"Life of Brian\",\n              242886,\n              8.1,\n              1979,\n              1970\n            ],\n            [\n              \"Gone with the Wind\",\n              192791,\n              8.1,\n              1939,\n              1930\n            ],\n            [\n              \"Touch of Evil\",\n              65408,\n              8.1,\n              1958,\n              1950\n            ],\n            [\n              \"No Country for Old Men\",\n              540744,\n              8.1,\n              2007,\n              2000\n            ],\n            [\n              \"Les quatre cents coups\",\n              61776,\n              8.1,\n              1959,\n              1950\n            ],\n            [\n              \"Shutter Island\",\n              670328,\n              8.1,\n              2010,\n              2010\n            ],\n            [\n              \"Kill Bill: Vol. 1\",\n              647120,\n              8.1,\n              2003,\n              2000\n            ],\n            [\n              \"El secreto de sus ojos\",\n              109254,\n              8.2,\n              2009,\n              2000\n            ],\n            [\n              \"Gran Torino\",\n              502773,\n              8.2,\n              2008,\n              2000\n            ],\n            [\n              \"Smultronst\\u00e4llet\",\n              55861,\n              8.2,\n              1957,\n              1950\n            ],\n            [\n              \"Good Will Hunting\",\n              529800,\n              8.2,\n              1997,\n              1990\n            ],\n            [\n              \"Jagten\",\n              137310,\n              8.2,\n              2012,\n              2010\n            ],\n            [\n              \"The Bridge on the River Kwai\",\n              132677,\n              8.2,\n              1957,\n              1950\n            ],\n            [\n              \"The Elephant Man\",\n              145514,\n              8.2,\n              1980,\n              1980\n            ],\n            [\n              \"Casino\",\n              294394,\n              8.2,\n              1995,\n              1990\n            ],\n            [\n              \"Lock, Stock and Two Smoking Barrels (1998)\",\n              372863,\n              8.2,\n              1998,\n              1990\n            ],\n            [\n              \"The Wolf of Wall Street\",\n              624943,\n              8.2,\n              2013,\n              2010\n            ],\n            [\n              \"Tonari no Totoro\",\n              142605,\n              8.2,\n              1988,\n              1980\n            ],\n            [\n              \"3 Idiots\",\n              167076,\n              8.2,\n              2009,\n              2000\n            ],\n            [\n              \"The General\",\n              46332,\n              8.2,\n              1926,\n              1920\n            ],\n            [\n              \"Judgment at Nuremberg\",\n              37664,\n              8.2,\n              1961,\n              1960\n            ],\n            [\n              \"The Gold Rush\",\n              58506,\n              8.2,\n              1925,\n              1920\n            ],\n            [\n              \"The Big Lebowski\",\n              473988,\n              8.2,\n              1998,\n              1990\n            ],\n            [\n              \"The Great Escape\",\n              147372,\n              8.2,\n              1963,\n              1960\n            ],\n            [\n              \"Up\",\n              566091,\n              8.2,\n              2009,\n              2000\n            ],\n            [\n              \"Warrior\",\n              287727,\n              8.2,\n              2011,\n              2010\n            ],\n            [\n              \"Ran\",\n              69287,\n              8.2,\n              1985,\n              1980\n            ],\n            [\n              \"Der Untergang\",\n              223834,\n              8.2,\n              2004,\n              2000\n            ],\n            [\n              \"Hauru no ugoku shiro\",\n              182525,\n              8.2,\n              2004,\n              2000\n            ],\n            [\n              \"V for Vendetta\",\n              700999,\n              8.2,\n              2005,\n              2000\n            ],\n            [\n              \"Chinatown\",\n              192744,\n              8.2,\n              1974,\n              1970\n            ],\n            [\n              \"El laberinto del fauno\",\n              416467,\n              8.2,\n              2006,\n              2000\n            ],\n            [\n              \"Blade Runner\",\n              415615,\n              8.2,\n              1982,\n              1980\n            ],\n            [\n              \"On the Waterfront\",\n              89233,\n              8.2,\n              1954,\n              1950\n            ],\n            [\n              \"Raging Bull\",\n              210647,\n              8.2,\n              1980,\n              1980\n            ],\n            [\n              \"Det sjunde inseglet\",\n              98949,\n              8.2,\n              1957,\n              1950\n            ],\n            [\n              \"Ikiru\",\n              36638,\n              8.2,\n              1952,\n              1950\n            ],\n            [\n              \"Mr. Smith Goes to Washington\",\n              69510,\n              8.2,\n              1939,\n              1930\n            ],\n            [\n              \"Rebecca\",\n              78216,\n              8.2,\n              1940,\n              1940\n            ],\n            [\n              \"Heat\",\n              388239,\n              8.2,\n              1995,\n              1990\n            ],\n            [\n              \"Die Hard\",\n              529287,\n              8.2,\n              1988,\n              1980\n            ],\n            [\n              \"Per qualche dollaro in pi\\u00f9\",\n              135556,\n              8.3,\n              1965,\n              1960\n            ],\n            [\n              \"Mad Max: Fury Road\",\n              262425,\n              8.3,\n              2015,\n              2010\n            ],\n            [\n              \"Monty Python and the Holy Grail\",\n              343300,\n              8.3,\n              1975,\n              1970\n            ],\n            [\n              \"Witness for the Prosecution\",\n              53186,\n              8.3,\n              1957,\n              1950\n            ],\n            [\n              \"A Clockwork Orange\",\n              489807,\n              8.3,\n              1971,\n              1970\n            ],\n            [\n              \"The Third Man\",\n              101785,\n              8.3,\n              1949,\n              1940\n            ],\n            [\n              \"Braveheart\",\n              653769,\n              8.3,\n              1995,\n              1990\n            ],\n            [\n              \"Amadeus\",\n              240308,\n              8.3,\n              1984,\n              1980\n            ],\n            [\n              \"Indiana Jones and the Last Crusade\",\n              453748,\n              8.3,\n              1989,\n              1980\n            ],\n            [\n              \"The Kid\",\n              55784,\n              8.3,\n              1921,\n              1920\n            ],\n            [\n              \"Y\\u00f4jinb\\u00f4\",\n              65370,\n              8.3,\n              1961,\n              1960\n            ],\n            [\n              \"Jodaeiye Nader az Simin\",\n              133760,\n              8.3,\n              2011,\n              2010\n            ],\n            [\n              \"Eternal Sunshine of the Spotless Mind\",\n              589477,\n              8.3,\n              2004,\n              2000\n            ],\n            [\n              \"The Apartment\",\n              96190,\n              8.3,\n              1960,\n              1960\n            ],\n            [\n              \"Requiem for a Dream\",\n              511177,\n              8.3,\n              2000,\n              2000\n            ],\n            [\n              \"Rash\\u00f4mon\",\n              90434,\n              8.3,\n              1950,\n              1950\n            ],\n            [\n              \"Toy Story 3\",\n              470158,\n              8.3,\n              2010,\n              2010\n            ],\n            [\n              \"Double Indemnity\",\n              86715,\n              8.3,\n              1944,\n              1940\n            ],\n            [\n              \"Metropolis\",\n              98794,\n              8.3,\n              1927,\n              1920\n            ],\n            [\n              \"L.A. Confidential\",\n              376590,\n              8.3,\n              1997,\n              1990\n            ],\n            [\n              \"Snatch.\",\n              536053,\n              8.3,\n              2000,\n              2000\n            ],\n            [\n              \"Lawrence of Arabia\",\n              170302,\n              8.3,\n              1962,\n              1960\n            ],\n            [\n              \"2001: A Space Odyssey\",\n              373482,\n              8.3,\n              1968,\n              1960\n            ],\n            [\n              \"The Sting\",\n              155691,\n              8.3,\n              1973,\n              1970\n            ],\n            [\n              \"The Treasure of the Sierra Madre\",\n              71003,\n              8.3,\n              1948,\n              1940\n            ],\n            [\n              \"Ladri di biciclette\",\n              81887,\n              8.3,\n              1948,\n              1940\n            ],\n            [\n              \"Toy Story\",\n              535249,\n              8.3,\n              1995,\n              1990\n            ],\n            [\n              \"Inglourious Basterds\",\n              755013,\n              8.3,\n              2009,\n              2000\n            ],\n            [\n              \"Singin' in the Rain\",\n              132823,\n              8.3,\n              1952,\n              1950\n            ],\n            [\n              \"All About Eve\",\n              74178,\n              8.3,\n              1950,\n              1950\n            ],\n            [\n              \"To Kill a Mockingbird\",\n              188160,\n              8.3,\n              1962,\n              1960\n            ],\n            [\n              \"Scarface\",\n              475594,\n              8.3,\n              1983,\n              1980\n            ],\n            [\n              \"Full Metal Jacket\",\n              425461,\n              8.3,\n              1987,\n              1980\n            ],\n            [\n              \"Taxi Driver\",\n              444534,\n              8.3,\n              1976,\n              1970\n            ],\n            [\n              \"Reservoir Dogs\",\n              578684,\n              8.3,\n              1992,\n              1990\n            ],\n            [\n              \"Unforgiven\",\n              248514,\n              8.3,\n              1992,\n              1990\n            ],\n            [\n              \"Some Like It Hot\",\n              156432,\n              8.3,\n              1959,\n              1950\n            ],\n            [\n              \"Batman Begins\",\n              850601,\n              8.3,\n              2005,\n              2000\n            ],\n            [\n              \"The Great Dictator\",\n              117590,\n              8.4,\n              1940,\n              1940\n            ],\n            [\n              \"The Shining\",\n              532559,\n              8.4,\n              1980,\n              1980\n            ],\n            [\n              \"Nuovo Cinema Paradiso\",\n              132121,\n              8.4,\n              1988,\n              1980\n            ],\n            [\n              \"The Dark Knight Rises\",\n              997350,\n              8.4,\n              2012,\n              2010\n            ],\n            [\n              \"WALL\\u00b7E\",\n              618914,\n              8.4,\n              2008,\n              2000\n            ],\n            [\n              \"Citizen Kane\",\n              268905,\n              8.4,\n              1941,\n              1940\n            ],\n            [\n              \"Once Upon a Time in America\",\n              195266,\n              8.4,\n              1984,\n              1980\n            ],\n            [\n              \"The Lion King\",\n              548750,\n              8.4,\n              1994,\n              1990\n            ],\n            [\n              \"Das Leben der Anderen\",\n              229533,\n              8.4,\n              2006,\n              2000\n            ],\n            [\n              \"Oldeuboi\",\n              311070,\n              8.4,\n              2003,\n              2000\n            ],\n            [\n              \"Mononoke-hime\",\n              192165,\n              8.4,\n              1997,\n              1990\n            ],\n            [\n              \"Vertigo\",\n              218430,\n              8.4,\n              1958,\n              1950\n            ],\n            [\n              \"Das Boot\",\n              151256,\n              8.4,\n              1981,\n              1980\n            ],\n            [\n              \"M\",\n              88355,\n              8.4,\n              1931,\n              1930\n            ],\n            [\n              \"Star Wars: Episode VI - Return of the Jedi\",\n              556811,\n              8.4,\n              1983,\n              1980\n            ],\n            [\n              \"Le fabuleux destin d'Am\\u00e9lie Poulain\",\n              474068,\n              8.4,\n              2001,\n              2000\n            ],\n            [\n              \"American Beauty\",\n              735056,\n              8.4,\n              1999,\n              1990\n            ],\n            [\n              \"Django Unchained\",\n              804729,\n              8.4,\n              2012,\n              2010\n            ],\n            [\n              \"North by Northwest\",\n              198795,\n              8.4,\n              1959,\n              1950\n            ],\n            [\n              \"Aliens\",\n              436218,\n              8.4,\n              1986,\n              1980\n            ],\n            [\n              \"Paths of Glory\",\n              106038,\n              8.4,\n              1957,\n              1950\n            ],\n            [\n              \"Hotaru no haka\",\n              115740,\n              8.4,\n              1988,\n              1980\n            ],\n            [\n              \"American History X\",\n              694602,\n              8.5,\n              1998,\n              1990\n            ],\n            [\n              \"City Lights\",\n              92375,\n              8.5,\n              1931,\n              1930\n            ],\n            [\n              \"Rear Window\",\n              280432,\n              8.5,\n              1954,\n              1950\n            ],\n            [\n              \"Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb\",\n              309141,\n              8.5,\n              1964,\n              1960\n            ],\n            [\n              \"Terminator 2: Judgment Day\",\n              658564,\n              8.5,\n              1991,\n              1990\n            ],\n            [\n              \"Inside Out (2015/I)\",\n              79615,\n              8.5,\n              2015,\n              2010\n            ],\n            [\n              \"Sen to Chihiro no Kamikakushi\",\n              357355,\n              8.5,\n              2001,\n              2000\n            ],\n            [\n              \"Casablanca\",\n              350551,\n              8.5,\n              1942,\n              1940\n            ],\n            [\n              \"Back to the Future\",\n              625463,\n              8.5,\n              1985,\n              1980\n            ],\n            [\n              \"Modern Times\",\n              121668,\n              8.5,\n              1936,\n              1930\n            ],\n            [\n              \"Saving Private Ryan\",\n              769893,\n              8.5,\n              1998,\n              1990\n            ],\n            [\n              \"Gladiator\",\n              862216,\n              8.5,\n              2000,\n              2000\n            ],\n            [\n              \"Raiders of the Lost Ark\",\n              585474,\n              8.5,\n              1981,\n              1980\n            ],\n            [\n              \"Intouchables\",\n              417122,\n              8.5,\n              2011,\n              2010\n            ],\n            [\n              \"Psycho\",\n              371791,\n              8.5,\n              1960,\n              1960\n            ],\n            [\n              \"Whiplash\",\n              264333,\n              8.5,\n              2014,\n              2010\n            ],\n            [\n              \"The Green Mile\",\n              672878,\n              8.5,\n              1999,\n              1990\n            ],\n            [\n              \"Memento\",\n              752122,\n              8.5,\n              2000,\n              2000\n            ],\n            [\n              \"The Prestige\",\n              733490,\n              8.5,\n              2006,\n              2000\n            ],\n            [\n              \"The Departed\",\n              763871,\n              8.5,\n              2006,\n              2000\n            ],\n            [\n              \"The Pianist\",\n              431856,\n              8.5,\n              2002,\n              2000\n            ],\n            [\n              \"Alien\",\n              496833,\n              8.5,\n              1979,\n              1970\n            ],\n            [\n              \"Apocalypse Now\",\n              403754,\n              8.5,\n              1979,\n              1970\n            ],\n            [\n              \"Sunset Blvd.\",\n              123879,\n              8.5,\n              1950,\n              1950\n            ],\n            [\n              \"Se7en\",\n              895411,\n              8.6,\n              1995,\n              1990\n            ],\n            [\n              \"La vita \\u00e8 bella\",\n              358305,\n              8.6,\n              1997,\n              1990\n            ],\n            [\n              \"Interstellar\",\n              689541,\n              8.6,\n              2014,\n              2010\n            ],\n            [\n              \"It's a Wonderful Life\",\n              242353,\n              8.6,\n              1946,\n              1940\n            ],\n            [\n              \"Cidade de Deus\",\n              476501,\n              8.6,\n              2002,\n              2000\n            ],\n            [\n              \"L\\u00e9on\",\n              635139,\n              8.6,\n              1994,\n              1990\n            ],\n            [\n              \"C'era una volta il West\",\n              192206,\n              8.6,\n              1968,\n              1960\n            ],\n            [\n              \"The Usual Suspects\",\n              656756,\n              8.6,\n              1995,\n              1990\n            ],\n            [\n              \"The Silence of the Lambs\",\n              767224,\n              8.6,\n              1991,\n              1990\n            ],\n            [\n              \"Forrest Gump\",\n              1078416,\n              8.7,\n              1994,\n              1990\n            ],\n            [\n              \"The Lord of the Rings: The Two Towers\",\n              967389,\n              8.7,\n              2002,\n              2000\n            ],\n            [\n              \"Star Wars: Episode V - The Empire Strikes Back\",\n              700283,\n              8.7,\n              1980,\n              1980\n            ],\n            [\n              \"The Matrix\",\n              1073043,\n              8.7,\n              1999,\n              1990\n            ],\n            [\n              \"Star Wars\",\n              770011,\n              8.7,\n              1977,\n              1970\n            ],\n            [\n              \"Goodfellas\",\n              644556,\n              8.7,\n              1990,\n              1990\n            ],\n            [\n              \"One Flew Over the Cuckoo's Nest\",\n              606395,\n              8.7,\n              1975,\n              1970\n            ],\n            [\n              \"Shichinin no samurai\",\n              206216,\n              8.7,\n              1954,\n              1950\n            ],\n            [\n              \"Inception\",\n              1271949,\n              8.7,\n              2010,\n              2010\n            ],\n            [\n              \"Fight Club\",\n              1177098,\n              8.8,\n              1999,\n              1990\n            ],\n            [\n              \"The Lord of the Rings: The Fellowship of the Ring\",\n              1099087,\n              8.8,\n              2001,\n              2000\n            ],\n            [\n              \"Pulp Fiction\",\n              1166532,\n              8.9,\n              1994,\n              1990\n            ],\n            [\n              \"The Dark Knight\",\n              1473049,\n              8.9,\n              2008,\n              2000\n            ],\n            [\n              \"Il buono, il brutto, il cattivo (1966)\",\n              447875,\n              8.9,\n              1966,\n              1960\n            ],\n            [\n              \"The Lord of the Rings: The Return of the King\",\n              1074146,\n              8.9,\n              2003,\n              2000\n            ],\n            [\n              \"Schindler's List\",\n              761224,\n              8.9,\n              1993,\n              1990\n            ],\n            [\n              \"12 Angry Men\",\n              384187,\n              8.9,\n              1957,\n              1950\n            ],\n            [\n              \"The Godfather: Part II\",\n              692753,\n              9.0,\n              1974,\n              1970\n            ],\n            [\n              \"The Shawshank Redemption\",\n              1498733,\n              9.2,\n              1994,\n              1990\n            ],\n            [\n              \"The Godfather\",\n              1027398,\n              9.2,\n              1972,\n              1970\n            ]\n          ]\n        }\n      }\n    },\n    {\n      \"type\": \"GroupByCall\",\n      \"code_step\": \"imdb.reset_index().groupby('Decade').max()\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 18\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 36\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"using\",\n          \"pos\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"column\",\n            \"label\": \"Decade\"\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": \"prev_rhs\",\n        \"rhs\": {\n          \"type\": \"DataFrameGroupBy\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Title\",\n              \"Votes\",\n              \"Rating\",\n              \"Year\",\n              \"Decade\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              0,\n              1,\n              2,\n              3,\n              4,\n              5,\n              6,\n              7,\n              8,\n              9,\n              10,\n              11,\n              12,\n              13,\n              14,\n              15,\n              16,\n              17,\n              18,\n              19,\n              20,\n              21,\n              22,\n              23,\n              24,\n              25,\n              26,\n              27,\n              28,\n              29,\n              30,\n              31,\n              32,\n              33,\n              34,\n              35,\n              36,\n              37,\n              38,\n              39,\n              40,\n              41,\n              42,\n              43,\n              44,\n              45,\n              46,\n              47,\n              48,\n              49,\n              50,\n              51,\n              52,\n              53,\n              54,\n              55,\n              56,\n              57,\n              58,\n              59,\n              60,\n              61,\n              62,\n              63,\n              64,\n              65,\n              66,\n              67,\n              68,\n              69,\n              70,\n              71,\n              72,\n              73,\n              74,\n              75,\n              76,\n              77,\n              78,\n              79,\n              80,\n              81,\n              82,\n              83,\n              84,\n              85,\n              86,\n              87,\n              88,\n              89,\n              90,\n              91,\n              92,\n              93,\n              94,\n              95,\n              96,\n              97,\n              98,\n              99,\n              100,\n              101,\n              102,\n              103,\n              104,\n              105,\n              106,\n              107,\n              108,\n              109,\n              110,\n              111,\n              112,\n              113,\n              114,\n              115,\n              116,\n              117,\n              118,\n              119,\n              120,\n              121,\n              122,\n              123,\n              124,\n              125,\n              126,\n              127,\n              128,\n              129,\n              130,\n              131,\n              132,\n              133,\n              134,\n              135,\n              136,\n              137,\n              138,\n              139,\n              140,\n              141,\n              142,\n              143,\n              144,\n              145,\n              146,\n              147,\n              148,\n              149,\n              150,\n              151,\n              152,\n              153,\n              154,\n              155,\n              156,\n              157,\n              158,\n              159,\n              160,\n              161,\n              162,\n              163,\n              164,\n              165,\n              166,\n              167,\n              168,\n              169,\n              170,\n              171,\n              172,\n              173,\n              174,\n              175,\n              176,\n              177,\n              178,\n              179,\n              180,\n              181,\n              182,\n              183,\n              184,\n              185,\n              186,\n              187,\n              188,\n              189,\n              190,\n              191,\n              192,\n              193,\n              194,\n              195,\n              196,\n              197,\n              198,\n              199,\n              200,\n              201,\n              202,\n              203,\n              204,\n              205,\n              206,\n              207,\n              208,\n              209,\n              210,\n              211,\n              212,\n              213,\n              214,\n              215,\n              216,\n              217,\n              218,\n              219,\n              220,\n              221,\n              222,\n              223,\n              224,\n              225,\n              226,\n              227,\n              228,\n              229,\n              230,\n              231,\n              232,\n              233,\n              234,\n              235,\n              236,\n              237,\n              238,\n              239,\n              240,\n              241,\n              242,\n              243,\n              244,\n              245,\n              246,\n              247,\n              248,\n              249\n            ]\n          },\n          \"data\": [\n            [\n              \"Akira\",\n              91652,\n              8.0,\n              1988,\n              1980\n            ],\n            [\n              \"Per un pugno di dollari\",\n              124671,\n              8.0,\n              1964,\n              1960\n            ],\n            [\n              \"Guardians of the Galaxy\",\n              527349,\n              8.0,\n              2014,\n              2010\n            ],\n            [\n              \"The Man Who Shot Liberty Valance\",\n              49135,\n              8.0,\n              1962,\n              1960\n            ],\n            [\n              \"Underground\",\n              39447,\n              8.0,\n              1995,\n              1990\n            ],\n            [\n              \"Le samoura\\u00ef\",\n              28012,\n              8.0,\n              1967,\n              1960\n            ],\n            [\n              \"Beauty and the Beast\",\n              268480,\n              8.0,\n              1991,\n              1990\n            ],\n            [\n              \"La strada\",\n              42446,\n              8.0,\n              1954,\n              1950\n            ],\n            [\n              \"The Avengers\",\n              862016,\n              8.0,\n              2012,\n              2010\n            ],\n            [\n              \"The King's Speech\",\n              434906,\n              8.0,\n              2010,\n              2010\n            ],\n            [\n              \"Barry Lyndon\",\n              89920,\n              8.0,\n              1975,\n              1970\n            ],\n            [\n              \"Dog Day Afternoon\",\n              164419,\n              8.0,\n              1975,\n              1970\n            ],\n            [\n              \"The Imitation Game\",\n              321834,\n              8.0,\n              2014,\n              2010\n            ],\n            [\n              \"The Truman Show\",\n              583004,\n              8.0,\n              1998,\n              1990\n            ],\n            [\n              \"Roman Holiday\",\n              87437,\n              8.0,\n              1953,\n              1950\n            ],\n            [\n              \"The Hustler\",\n              57842,\n              8.0,\n              1961,\n              1960\n            ],\n            [\n              \"X-Men: Days of Future Past\",\n              427099,\n              8.0,\n              2014,\n              2010\n            ],\n            [\n              \"High Noon\",\n              72007,\n              8.0,\n              1952,\n              1950\n            ],\n            [\n              \"Before Sunset\",\n              147277,\n              8.0,\n              2004,\n              2000\n            ],\n            [\n              \"Monsters, Inc. (2001)\",\n              500576,\n              8.0,\n              2001,\n              2000\n            ],\n            [\n              \"Harry Potter and the Deathly Hallows: Part 2\",\n              466111,\n              8.0,\n              2011,\n              2010\n            ],\n            [\n              \"Lagaan: Once Upon a Time in India\",\n              59377,\n              8.0,\n              2001,\n              2000\n            ],\n            [\n              \"Jurassic Park\",\n              520391,\n              8.0,\n              1993,\n              1990\n            ],\n            [\n              \"Trois couleurs: Rouge\",\n              57644,\n              8.0,\n              1994,\n              1990\n            ],\n            [\n              \"Fanny och Alexander\",\n              37972,\n              8.0,\n              1982,\n              1980\n            ],\n            [\n              \"The Help\",\n              282774,\n              8.0,\n              2011,\n              2010\n            ],\n            [\n              \"Who's Afraid of Virginia Woolf?\",\n              50208,\n              8.0,\n              1966,\n              1960\n            ],\n            [\n              \"The Terminator\",\n              527274,\n              8.0,\n              1984,\n              1980\n            ],\n            [\n              \"Twelve Monkeys\",\n              415809,\n              8.0,\n              1995,\n              1990\n            ],\n            [\n              \"Kumonosu-j\\u00f4\",\n              26012,\n              8.0,\n              1957,\n              1950\n            ],\n            [\n              \"Groundhog Day\",\n              384272,\n              8.0,\n              1993,\n              1990\n            ],\n            [\n              \"Yip Man\",\n              138240,\n              8.0,\n              2008,\n              2000\n            ],\n            [\n              \"Notorious\",\n              66622,\n              8.0,\n              1946,\n              1940\n            ],\n            [\n              \"Pirates of the Caribbean: The Curse of the Black Pearl\",\n              718645,\n              8.0,\n              2003,\n              2000\n            ],\n            [\n              \"La haine\",\n              90937,\n              8.0,\n              1995,\n              1990\n            ],\n            [\n              \"Mou gaan dou\",\n              82440,\n              8.0,\n              2002,\n              2000\n            ],\n            [\n              \"Salinui chueok\",\n              54651,\n              8.0,\n              2003,\n              2000\n            ],\n            [\n              \"The Big Sleep\",\n              59578,\n              8.0,\n              1946,\n              1940\n            ],\n            [\n              \"Paris, Texas (1984)\",\n              43090,\n              8.0,\n              1984,\n              1980\n            ],\n            [\n              \"Rocky\",\n              318041,\n              8.0,\n              1976,\n              1970\n            ],\n            [\n              \"Prisoners\",\n              312516,\n              8.0,\n              2013,\n              2010\n            ],\n            [\n              \"The Killing\",\n              56671,\n              8.0,\n              1956,\n              1950\n            ],\n            [\n              \"The Night of the Hunter\",\n              57974,\n              8.0,\n              1955,\n              1950\n            ],\n            [\n              \"La battaglia di Algeri\",\n              32385,\n              8.0,\n              1966,\n              1960\n            ],\n            [\n              \"Bom yeoreum gaeul gyeoul geurigo bom\",\n              55382,\n              8.0,\n              2003,\n              2000\n            ],\n            [\n              \"Papillon\",\n              87591,\n              8.0,\n              1973,\n              1970\n            ],\n            [\n              \"Before Sunrise\",\n              158867,\n              8.0,\n              1995,\n              1990\n            ],\n            [\n              \"Tenk\\u00fb no shiro Rapyuta\",\n              83941,\n              8.0,\n              1986,\n              1980\n            ],\n            [\n              \"Jaws\",\n              364225,\n              8.0,\n              1975,\n              1970\n            ],\n            [\n              \"Relatos salvajes\",\n              46987,\n              8.0,\n              2014,\n              2010\n            ],\n            [\n              \"Incendies\",\n              65122,\n              8.1,\n              2010,\n              2010\n            ],\n            [\n              \"Hachi: A Dog's Tale\",\n              121599,\n              8.1,\n              2009,\n              2000\n            ],\n            [\n              \"The Best Years of Our Lives\",\n              35983,\n              8.1,\n              1946,\n              1940\n            ],\n            [\n              \"The Wizard of Oz\",\n              259235,\n              8.1,\n              1939,\n              1930\n            ],\n            [\n              \"Cool Hand Luke\",\n              108128,\n              8.1,\n              1967,\n              1960\n            ],\n            [\n              \"In the Name of the Father\",\n              95212,\n              8.1,\n              1993,\n              1990\n            ],\n            [\n              \"The Grapes of Wrath\",\n              55793,\n              8.1,\n              1940,\n              1940\n            ],\n            [\n              \"A Beautiful Mind\",\n              525515,\n              8.1,\n              2001,\n              2000\n            ],\n            [\n              \"Donnie Darko\",\n              522119,\n              8.1,\n              2001,\n              2000\n            ],\n            [\n              \"Mary and Max\",\n              102735,\n              8.1,\n              2009,\n              2000\n            ],\n            [\n              \"Stand by Me\",\n              242381,\n              8.1,\n              1986,\n              1980\n            ],\n            [\n              \"The Grand Budapest Hotel\",\n              369141,\n              8.1,\n              2014,\n              2010\n            ],\n            [\n              \"Butch Cassidy and the Sundance Kid\",\n              134552,\n              8.1,\n              1969,\n              1960\n            ],\n            [\n              \"The Sixth Sense\",\n              630994,\n              8.1,\n              1999,\n              1990\n            ],\n            [\n              \"The Thing\",\n              228230,\n              8.1,\n              1982,\n              1980\n            ],\n            [\n              \"Le salaire de la peur\",\n              31003,\n              8.1,\n              1953,\n              1950\n            ],\n            [\n              \"Persona\",\n              52573,\n              8.1,\n              1966,\n              1960\n            ],\n            [\n              \"Annie Hall\",\n              169982,\n              8.1,\n              1977,\n              1970\n            ],\n            [\n              \"Ben-Hur\",\n              141768,\n              8.1,\n              1959,\n              1950\n            ],\n            [\n              \"8\\u00bd\",\n              69988,\n              8.1,\n              1963,\n              1960\n            ],\n            [\n              \"Fargo\",\n              395997,\n              8.1,\n              1996,\n              1990\n            ],\n            [\n              \"Strangers on a Train\",\n              85012,\n              8.1,\n              1951,\n              1950\n            ],\n            [\n              \"Gone Girl\",\n              441174,\n              8.1,\n              2014,\n              2010\n            ],\n            [\n              \"Million Dollar Baby\",\n              424957,\n              8.1,\n              2004,\n              2000\n            ],\n            [\n              \"Network\",\n              91689,\n              8.1,\n              1976,\n              1970\n            ],\n            [\n              \"It Happened One Night\",\n              56842,\n              8.1,\n              1934,\n              1930\n            ],\n            [\n              \"Trainspotting\",\n              419372,\n              8.1,\n              1996,\n              1990\n            ],\n            [\n              \"Les diaboliques\",\n              36725,\n              8.1,\n              1955,\n              1950\n            ],\n            [\n              \"There Will Be Blood\",\n              331348,\n              8.1,\n              2007,\n              2000\n            ],\n            [\n              \"The Princess Bride\",\n              264285,\n              8.1,\n              1987,\n              1980\n            ],\n            [\n              \"Dial M for Murder\",\n              92244,\n              8.1,\n              1954,\n              1950\n            ],\n            [\n              \"Hotel Rwanda\",\n              237292,\n              8.1,\n              2004,\n              2000\n            ],\n            [\n              \"Sin City\",\n              596880,\n              8.1,\n              2005,\n              2000\n            ],\n            [\n              \"The Deer Hunter\",\n              209677,\n              8.1,\n              1978,\n              1970\n            ],\n            [\n              \"How to Train Your Dragon\",\n              415845,\n              8.1,\n              2010,\n              2010\n            ],\n            [\n              \"Into the Wild\",\n              372803,\n              8.1,\n              2007,\n              2000\n            ],\n            [\n              \"Rush (2013/I)\",\n              258882,\n              8.1,\n              2013,\n              2010\n            ],\n            [\n              \"Gandhi\",\n              151697,\n              8.1,\n              1982,\n              1980\n            ],\n            [\n              \"The Maltese Falcon\",\n              101754,\n              8.1,\n              1941,\n              1940\n            ],\n            [\n              \"Amores perros\",\n              156030,\n              8.1,\n              2000,\n              2000\n            ],\n            [\n              \"12 Years a Slave\",\n              359121,\n              8.1,\n              2013,\n              2010\n            ],\n            [\n              \"The Bourne Ultimatum\",\n              433487,\n              8.1,\n              2007,\n              2000\n            ],\n            [\n              \"Kaze no tani no Naushika\",\n              81809,\n              8.1,\n              1984,\n              1980\n            ],\n            [\n              \"Platoon\",\n              262404,\n              8.1,\n              1986,\n              1980\n            ],\n            [\n              \"Finding Nemo\",\n              591219,\n              8.1,\n              2003,\n              2000\n            ],\n            [\n              \"Stalker\",\n              59272,\n              8.1,\n              1979,\n              1970\n            ],\n            [\n              \"Life of Brian\",\n              242886,\n              8.1,\n              1979,\n              1970\n            ],\n            [\n              \"Gone with the Wind\",\n              192791,\n              8.1,\n              1939,\n              1930\n            ],\n            [\n              \"Touch of Evil\",\n              65408,\n              8.1,\n              1958,\n              1950\n            ],\n            [\n              \"No Country for Old Men\",\n              540744,\n              8.1,\n              2007,\n              2000\n            ],\n            [\n              \"Les quatre cents coups\",\n              61776,\n              8.1,\n              1959,\n              1950\n            ],\n            [\n              \"Shutter Island\",\n              670328,\n              8.1,\n              2010,\n              2010\n            ],\n            [\n              \"Kill Bill: Vol. 1\",\n              647120,\n              8.1,\n              2003,\n              2000\n            ],\n            [\n              \"El secreto de sus ojos\",\n              109254,\n              8.2,\n              2009,\n              2000\n            ],\n            [\n              \"Gran Torino\",\n              502773,\n              8.2,\n              2008,\n              2000\n            ],\n            [\n              \"Smultronst\\u00e4llet\",\n              55861,\n              8.2,\n              1957,\n              1950\n            ],\n            [\n              \"Good Will Hunting\",\n              529800,\n              8.2,\n              1997,\n              1990\n            ],\n            [\n              \"Jagten\",\n              137310,\n              8.2,\n              2012,\n              2010\n            ],\n            [\n              \"The Bridge on the River Kwai\",\n              132677,\n              8.2,\n              1957,\n              1950\n            ],\n            [\n              \"The Elephant Man\",\n              145514,\n              8.2,\n              1980,\n              1980\n            ],\n            [\n              \"Casino\",\n              294394,\n              8.2,\n              1995,\n              1990\n            ],\n            [\n              \"Lock, Stock and Two Smoking Barrels (1998)\",\n              372863,\n              8.2,\n              1998,\n              1990\n            ],\n            [\n              \"The Wolf of Wall Street\",\n              624943,\n              8.2,\n              2013,\n              2010\n            ],\n            [\n              \"Tonari no Totoro\",\n              142605,\n              8.2,\n              1988,\n              1980\n            ],\n            [\n              \"3 Idiots\",\n              167076,\n              8.2,\n              2009,\n              2000\n            ],\n            [\n              \"The General\",\n              46332,\n              8.2,\n              1926,\n              1920\n            ],\n            [\n              \"Judgment at Nuremberg\",\n              37664,\n              8.2,\n              1961,\n              1960\n            ],\n            [\n              \"The Gold Rush\",\n              58506,\n              8.2,\n              1925,\n              1920\n            ],\n            [\n              \"The Big Lebowski\",\n              473988,\n              8.2,\n              1998,\n              1990\n            ],\n            [\n              \"The Great Escape\",\n              147372,\n              8.2,\n              1963,\n              1960\n            ],\n            [\n              \"Up\",\n              566091,\n              8.2,\n              2009,\n              2000\n            ],\n            [\n              \"Warrior\",\n              287727,\n              8.2,\n              2011,\n              2010\n            ],\n            [\n              \"Ran\",\n              69287,\n              8.2,\n              1985,\n              1980\n            ],\n            [\n              \"Der Untergang\",\n              223834,\n              8.2,\n              2004,\n              2000\n            ],\n            [\n              \"Hauru no ugoku shiro\",\n              182525,\n              8.2,\n              2004,\n              2000\n            ],\n            [\n              \"V for Vendetta\",\n              700999,\n              8.2,\n              2005,\n              2000\n            ],\n            [\n              \"Chinatown\",\n              192744,\n              8.2,\n              1974,\n              1970\n            ],\n            [\n              \"El laberinto del fauno\",\n              416467,\n              8.2,\n              2006,\n              2000\n            ],\n            [\n              \"Blade Runner\",\n              415615,\n              8.2,\n              1982,\n              1980\n            ],\n            [\n              \"On the Waterfront\",\n              89233,\n              8.2,\n              1954,\n              1950\n            ],\n            [\n              \"Raging Bull\",\n              210647,\n              8.2,\n              1980,\n              1980\n            ],\n            [\n              \"Det sjunde inseglet\",\n              98949,\n              8.2,\n              1957,\n              1950\n            ],\n            [\n              \"Ikiru\",\n              36638,\n              8.2,\n              1952,\n              1950\n            ],\n            [\n              \"Mr. Smith Goes to Washington\",\n              69510,\n              8.2,\n              1939,\n              1930\n            ],\n            [\n              \"Rebecca\",\n              78216,\n              8.2,\n              1940,\n              1940\n            ],\n            [\n              \"Heat\",\n              388239,\n              8.2,\n              1995,\n              1990\n            ],\n            [\n              \"Die Hard\",\n              529287,\n              8.2,\n              1988,\n              1980\n            ],\n            [\n              \"Per qualche dollaro in pi\\u00f9\",\n              135556,\n              8.3,\n              1965,\n              1960\n            ],\n            [\n              \"Mad Max: Fury Road\",\n              262425,\n              8.3,\n              2015,\n              2010\n            ],\n            [\n              \"Monty Python and the Holy Grail\",\n              343300,\n              8.3,\n              1975,\n              1970\n            ],\n            [\n              \"Witness for the Prosecution\",\n              53186,\n              8.3,\n              1957,\n              1950\n            ],\n            [\n              \"A Clockwork Orange\",\n              489807,\n              8.3,\n              1971,\n              1970\n            ],\n            [\n              \"The Third Man\",\n              101785,\n              8.3,\n              1949,\n              1940\n            ],\n            [\n              \"Braveheart\",\n              653769,\n              8.3,\n              1995,\n              1990\n            ],\n            [\n              \"Amadeus\",\n              240308,\n              8.3,\n              1984,\n              1980\n            ],\n            [\n              \"Indiana Jones and the Last Crusade\",\n              453748,\n              8.3,\n              1989,\n              1980\n            ],\n            [\n              \"The Kid\",\n              55784,\n              8.3,\n              1921,\n              1920\n            ],\n            [\n              \"Y\\u00f4jinb\\u00f4\",\n              65370,\n              8.3,\n              1961,\n              1960\n            ],\n            [\n              \"Jodaeiye Nader az Simin\",\n              133760,\n              8.3,\n              2011,\n              2010\n            ],\n            [\n              \"Eternal Sunshine of the Spotless Mind\",\n              589477,\n              8.3,\n              2004,\n              2000\n            ],\n            [\n              \"The Apartment\",\n              96190,\n              8.3,\n              1960,\n              1960\n            ],\n            [\n              \"Requiem for a Dream\",\n              511177,\n              8.3,\n              2000,\n              2000\n            ],\n            [\n              \"Rash\\u00f4mon\",\n              90434,\n              8.3,\n              1950,\n              1950\n            ],\n            [\n              \"Toy Story 3\",\n              470158,\n              8.3,\n              2010,\n              2010\n            ],\n            [\n              \"Double Indemnity\",\n              86715,\n              8.3,\n              1944,\n              1940\n            ],\n            [\n              \"Metropolis\",\n              98794,\n              8.3,\n              1927,\n              1920\n            ],\n            [\n              \"L.A. Confidential\",\n              376590,\n              8.3,\n              1997,\n              1990\n            ],\n            [\n              \"Snatch.\",\n              536053,\n              8.3,\n              2000,\n              2000\n            ],\n            [\n              \"Lawrence of Arabia\",\n              170302,\n              8.3,\n              1962,\n              1960\n            ],\n            [\n              \"2001: A Space Odyssey\",\n              373482,\n              8.3,\n              1968,\n              1960\n            ],\n            [\n              \"The Sting\",\n              155691,\n              8.3,\n              1973,\n              1970\n            ],\n            [\n              \"The Treasure of the Sierra Madre\",\n              71003,\n              8.3,\n              1948,\n              1940\n            ],\n            [\n              \"Ladri di biciclette\",\n              81887,\n              8.3,\n              1948,\n              1940\n            ],\n            [\n              \"Toy Story\",\n              535249,\n              8.3,\n              1995,\n              1990\n            ],\n            [\n              \"Inglourious Basterds\",\n              755013,\n              8.3,\n              2009,\n              2000\n            ],\n            [\n              \"Singin' in the Rain\",\n              132823,\n              8.3,\n              1952,\n              1950\n            ],\n            [\n              \"All About Eve\",\n              74178,\n              8.3,\n              1950,\n              1950\n            ],\n            [\n              \"To Kill a Mockingbird\",\n              188160,\n              8.3,\n              1962,\n              1960\n            ],\n            [\n              \"Scarface\",\n              475594,\n              8.3,\n              1983,\n              1980\n            ],\n            [\n              \"Full Metal Jacket\",\n              425461,\n              8.3,\n              1987,\n              1980\n            ],\n            [\n              \"Taxi Driver\",\n              444534,\n              8.3,\n              1976,\n              1970\n            ],\n            [\n              \"Reservoir Dogs\",\n              578684,\n              8.3,\n              1992,\n              1990\n            ],\n            [\n              \"Unforgiven\",\n              248514,\n              8.3,\n              1992,\n              1990\n            ],\n            [\n              \"Some Like It Hot\",\n              156432,\n              8.3,\n              1959,\n              1950\n            ],\n            [\n              \"Batman Begins\",\n              850601,\n              8.3,\n              2005,\n              2000\n            ],\n            [\n              \"The Great Dictator\",\n              117590,\n              8.4,\n              1940,\n              1940\n            ],\n            [\n              \"The Shining\",\n              532559,\n              8.4,\n              1980,\n              1980\n            ],\n            [\n              \"Nuovo Cinema Paradiso\",\n              132121,\n              8.4,\n              1988,\n              1980\n            ],\n            [\n              \"The Dark Knight Rises\",\n              997350,\n              8.4,\n              2012,\n              2010\n            ],\n            [\n              \"WALL\\u00b7E\",\n              618914,\n              8.4,\n              2008,\n              2000\n            ],\n            [\n              \"Citizen Kane\",\n              268905,\n              8.4,\n              1941,\n              1940\n            ],\n            [\n              \"Once Upon a Time in America\",\n              195266,\n              8.4,\n              1984,\n              1980\n            ],\n            [\n              \"The Lion King\",\n              548750,\n              8.4,\n              1994,\n              1990\n            ],\n            [\n              \"Das Leben der Anderen\",\n              229533,\n              8.4,\n              2006,\n              2000\n            ],\n            [\n              \"Oldeuboi\",\n              311070,\n              8.4,\n              2003,\n              2000\n            ],\n            [\n              \"Mononoke-hime\",\n              192165,\n              8.4,\n              1997,\n              1990\n            ],\n            [\n              \"Vertigo\",\n              218430,\n              8.4,\n              1958,\n              1950\n            ],\n            [\n              \"Das Boot\",\n              151256,\n              8.4,\n              1981,\n              1980\n            ],\n            [\n              \"M\",\n              88355,\n              8.4,\n              1931,\n              1930\n            ],\n            [\n              \"Star Wars: Episode VI - Return of the Jedi\",\n              556811,\n              8.4,\n              1983,\n              1980\n            ],\n            [\n              \"Le fabuleux destin d'Am\\u00e9lie Poulain\",\n              474068,\n              8.4,\n              2001,\n              2000\n            ],\n            [\n              \"American Beauty\",\n              735056,\n              8.4,\n              1999,\n              1990\n            ],\n            [\n              \"Django Unchained\",\n              804729,\n              8.4,\n              2012,\n              2010\n            ],\n            [\n              \"North by Northwest\",\n              198795,\n              8.4,\n              1959,\n              1950\n            ],\n            [\n              \"Aliens\",\n              436218,\n              8.4,\n              1986,\n              1980\n            ],\n            [\n              \"Paths of Glory\",\n              106038,\n              8.4,\n              1957,\n              1950\n            ],\n            [\n              \"Hotaru no haka\",\n              115740,\n              8.4,\n              1988,\n              1980\n            ],\n            [\n              \"American History X\",\n              694602,\n              8.5,\n              1998,\n              1990\n            ],\n            [\n              \"City Lights\",\n              92375,\n              8.5,\n              1931,\n              1930\n            ],\n            [\n              \"Rear Window\",\n              280432,\n              8.5,\n              1954,\n              1950\n            ],\n            [\n              \"Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb\",\n              309141,\n              8.5,\n              1964,\n              1960\n            ],\n            [\n              \"Terminator 2: Judgment Day\",\n              658564,\n              8.5,\n              1991,\n              1990\n            ],\n            [\n              \"Inside Out (2015/I)\",\n              79615,\n              8.5,\n              2015,\n              2010\n            ],\n            [\n              \"Sen to Chihiro no Kamikakushi\",\n              357355,\n              8.5,\n              2001,\n              2000\n            ],\n            [\n              \"Casablanca\",\n              350551,\n              8.5,\n              1942,\n              1940\n            ],\n            [\n              \"Back to the Future\",\n              625463,\n              8.5,\n              1985,\n              1980\n            ],\n            [\n              \"Modern Times\",\n              121668,\n              8.5,\n              1936,\n              1930\n            ],\n            [\n              \"Saving Private Ryan\",\n              769893,\n              8.5,\n              1998,\n              1990\n            ],\n            [\n              \"Gladiator\",\n              862216,\n              8.5,\n              2000,\n              2000\n            ],\n            [\n              \"Raiders of the Lost Ark\",\n              585474,\n              8.5,\n              1981,\n              1980\n            ],\n            [\n              \"Intouchables\",\n              417122,\n              8.5,\n              2011,\n              2010\n            ],\n            [\n              \"Psycho\",\n              371791,\n              8.5,\n              1960,\n              1960\n            ],\n            [\n              \"Whiplash\",\n              264333,\n              8.5,\n              2014,\n              2010\n            ],\n            [\n              \"The Green Mile\",\n              672878,\n              8.5,\n              1999,\n              1990\n            ],\n            [\n              \"Memento\",\n              752122,\n              8.5,\n              2000,\n              2000\n            ],\n            [\n              \"The Prestige\",\n              733490,\n              8.5,\n              2006,\n              2000\n            ],\n            [\n              \"The Departed\",\n              763871,\n              8.5,\n              2006,\n              2000\n            ],\n            [\n              \"The Pianist\",\n              431856,\n              8.5,\n              2002,\n              2000\n            ],\n            [\n              \"Alien\",\n              496833,\n              8.5,\n              1979,\n              1970\n            ],\n            [\n              \"Apocalypse Now\",\n              403754,\n              8.5,\n              1979,\n              1970\n            ],\n            [\n              \"Sunset Blvd.\",\n              123879,\n              8.5,\n              1950,\n              1950\n            ],\n            [\n              \"Se7en\",\n              895411,\n              8.6,\n              1995,\n              1990\n            ],\n            [\n              \"La vita \\u00e8 bella\",\n              358305,\n              8.6,\n              1997,\n              1990\n            ],\n            [\n              \"Interstellar\",\n              689541,\n              8.6,\n              2014,\n              2010\n            ],\n            [\n              \"It's a Wonderful Life\",\n              242353,\n              8.6,\n              1946,\n              1940\n            ],\n            [\n              \"Cidade de Deus\",\n              476501,\n              8.6,\n              2002,\n              2000\n            ],\n            [\n              \"L\\u00e9on\",\n              635139,\n              8.6,\n              1994,\n              1990\n            ],\n            [\n              \"C'era una volta il West\",\n              192206,\n              8.6,\n              1968,\n              1960\n            ],\n            [\n              \"The Usual Suspects\",\n              656756,\n              8.6,\n              1995,\n              1990\n            ],\n            [\n              \"The Silence of the Lambs\",\n              767224,\n              8.6,\n              1991,\n              1990\n            ],\n            [\n              \"Forrest Gump\",\n              1078416,\n              8.7,\n              1994,\n              1990\n            ],\n            [\n              \"The Lord of the Rings: The Two Towers\",\n              967389,\n              8.7,\n              2002,\n              2000\n            ],\n            [\n              \"Star Wars: Episode V - The Empire Strikes Back\",\n              700283,\n              8.7,\n              1980,\n              1980\n            ],\n            [\n              \"The Matrix\",\n              1073043,\n              8.7,\n              1999,\n              1990\n            ],\n            [\n              \"Star Wars\",\n              770011,\n              8.7,\n              1977,\n              1970\n            ],\n            [\n              \"Goodfellas\",\n              644556,\n              8.7,\n              1990,\n              1990\n            ],\n            [\n              \"One Flew Over the Cuckoo's Nest\",\n              606395,\n              8.7,\n              1975,\n              1970\n            ],\n            [\n              \"Shichinin no samurai\",\n              206216,\n              8.7,\n              1954,\n              1950\n            ],\n            [\n              \"Inception\",\n              1271949,\n              8.7,\n              2010,\n              2010\n            ],\n            [\n              \"Fight Club\",\n              1177098,\n              8.8,\n              1999,\n              1990\n            ],\n            [\n              \"The Lord of the Rings: The Fellowship of the Ring\",\n              1099087,\n              8.8,\n              2001,\n              2000\n            ],\n            [\n              \"Pulp Fiction\",\n              1166532,\n              8.9,\n              1994,\n              1990\n            ],\n            [\n              \"The Dark Knight\",\n              1473049,\n              8.9,\n              2008,\n              2000\n            ],\n            [\n              \"Il buono, il brutto, il cattivo (1966)\",\n              447875,\n              8.9,\n              1966,\n              1960\n            ],\n            [\n              \"The Lord of the Rings: The Return of the King\",\n              1074146,\n              8.9,\n              2003,\n              2000\n            ],\n            [\n              \"Schindler's List\",\n              761224,\n              8.9,\n              1993,\n              1990\n            ],\n            [\n              \"12 Angry Men\",\n              384187,\n              8.9,\n              1957,\n              1950\n            ],\n            [\n              \"The Godfather: Part II\",\n              692753,\n              9.0,\n              1974,\n              1970\n            ],\n            [\n              \"The Shawshank Redemption\",\n              1498733,\n              9.2,\n              1994,\n              1990\n            ],\n            [\n              \"The Godfather\",\n              1027398,\n              9.2,\n              1972,\n              1970\n            ]\n          ],\n          \"group_data\": {\n            \"columns\": [\n              \"Decade\"\n            ],\n            \"groups\": [\n              {\n                \"name\": [\n                  1920\n                ],\n                \"labels\": [\n                  115,\n                  117,\n                  146,\n                  155\n                ]\n              },\n              {\n                \"name\": [\n                  1930\n                ],\n                \"labels\": [\n                  53,\n                  75,\n                  97,\n                  133,\n                  188,\n                  198,\n                  206\n                ]\n              },\n              {\n                \"name\": [\n                  1940\n                ],\n                \"labels\": [\n                  32,\n                  37,\n                  52,\n                  56,\n                  88,\n                  134,\n                  142,\n                  154,\n                  161,\n                  162,\n                  175,\n                  180,\n                  204,\n                  224\n                ]\n              },\n              {\n                \"name\": [\n                  1950\n                ],\n                \"labels\": [\n                  7,\n                  14,\n                  17,\n                  29,\n                  41,\n                  42,\n                  65,\n                  68,\n                  71,\n                  77,\n                  80,\n                  98,\n                  100,\n                  105,\n                  108,\n                  129,\n                  131,\n                  132,\n                  140,\n                  152,\n                  165,\n                  166,\n                  173,\n                  186,\n                  193,\n                  195,\n                  199,\n                  220,\n                  237,\n                  246\n                ]\n              },\n              {\n                \"name\": [\n                  1960\n                ],\n                \"labels\": [\n                  1,\n                  3,\n                  5,\n                  15,\n                  26,\n                  43,\n                  54,\n                  62,\n                  66,\n                  69,\n                  116,\n                  119,\n                  137,\n                  147,\n                  150,\n                  158,\n                  159,\n                  167,\n                  200,\n                  211,\n                  227,\n                  243\n                ]\n              },\n              {\n                \"name\": [\n                  1970\n                ],\n                \"labels\": [\n                  10,\n                  11,\n                  39,\n                  45,\n                  48,\n                  67,\n                  74,\n                  83,\n                  95,\n                  96,\n                  126,\n                  139,\n                  141,\n                  160,\n                  170,\n                  218,\n                  219,\n                  234,\n                  236,\n                  247,\n                  249\n                ]\n              },\n              {\n                \"name\": [\n                  1980\n                ],\n                \"labels\": [\n                  0,\n                  24,\n                  27,\n                  38,\n                  47,\n                  60,\n                  64,\n                  79,\n                  87,\n                  92,\n                  93,\n                  109,\n                  113,\n                  122,\n                  128,\n                  130,\n                  136,\n                  144,\n                  145,\n                  168,\n                  169,\n                  176,\n                  177,\n                  181,\n                  187,\n                  189,\n                  194,\n                  196,\n                  205,\n                  209,\n                  232\n                ]\n              },\n              {\n                \"name\": [\n                  1990\n                ],\n                \"labels\": [\n                  4,\n                  6,\n                  13,\n                  22,\n                  23,\n                  28,\n                  30,\n                  34,\n                  46,\n                  55,\n                  63,\n                  70,\n                  76,\n                  106,\n                  110,\n                  111,\n                  118,\n                  135,\n                  143,\n                  156,\n                  163,\n                  171,\n                  172,\n                  182,\n                  185,\n                  191,\n                  197,\n                  201,\n                  207,\n                  213,\n                  221,\n                  222,\n                  226,\n                  228,\n                  229,\n                  230,\n                  233,\n                  235,\n                  239,\n                  241,\n                  245,\n                  248\n                ]\n              },\n              {\n                \"name\": [\n                  2000\n                ],\n                \"labels\": [\n                  18,\n                  19,\n                  21,\n                  31,\n                  33,\n                  35,\n                  36,\n                  44,\n                  51,\n                  57,\n                  58,\n                  59,\n                  73,\n                  78,\n                  81,\n                  82,\n                  85,\n                  89,\n                  91,\n                  94,\n                  99,\n                  102,\n                  103,\n                  104,\n                  114,\n                  120,\n                  123,\n                  124,\n                  125,\n                  127,\n                  149,\n                  151,\n                  157,\n                  164,\n                  174,\n                  179,\n                  183,\n                  184,\n                  190,\n                  203,\n                  208,\n                  214,\n                  215,\n                  216,\n                  217,\n                  225,\n                  231,\n                  240,\n                  242,\n                  244\n                ]\n              },\n              {\n                \"name\": [\n                  2010\n                ],\n                \"labels\": [\n                  2,\n                  8,\n                  9,\n                  12,\n                  16,\n                  20,\n                  25,\n                  40,\n                  49,\n                  50,\n                  61,\n                  72,\n                  84,\n                  86,\n                  90,\n                  101,\n                  107,\n                  112,\n                  121,\n                  138,\n                  148,\n                  153,\n                  178,\n                  192,\n                  202,\n                  210,\n                  212,\n                  223,\n                  238\n                ]\n              }\n            ]\n          }\n        }\n      }\n    },\n    {\n      \"type\": \"AggCall\",\n      \"code_step\": \"imdb.reset_index().groupby('Decade').max()\",\n      \"fragment\": {\n        \"start\": {\n          \"line\": 0,\n          \"ch\": 36\n        },\n        \"end\": {\n          \"line\": 0,\n          \"ch\": 42\n        }\n      },\n      \"marks\": [\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 115\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1920\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 117\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1920\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 146\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1920\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 155\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1920\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 53\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1930\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 75\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1930\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 97\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1930\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 133\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1930\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 188\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1930\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 198\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1930\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 206\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1930\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 32\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 37\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 52\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 56\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 88\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 134\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 142\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 154\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 161\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 162\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 175\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 180\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 204\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 224\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1940\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 7\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 14\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 17\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 29\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 41\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 42\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 65\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 68\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 71\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 77\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 80\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 98\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 100\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 105\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 108\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 129\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 131\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 132\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 140\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 152\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 165\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 166\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 173\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 186\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 193\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 195\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 199\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 220\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 237\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 246\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1950\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 1\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 3\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 5\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 15\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 26\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 43\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 54\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 62\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 66\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 69\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 116\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 119\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 137\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 147\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 150\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 158\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 159\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 167\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 200\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 211\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 227\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 243\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1960\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 10\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 11\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 39\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 45\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 48\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 67\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 74\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 83\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 95\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 96\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 126\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 139\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 141\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 160\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 170\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 218\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 219\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 234\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 236\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 247\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 249\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1970\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 0\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 24\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 27\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 38\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 47\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 60\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 64\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 79\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 87\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 92\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 93\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 109\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 113\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 122\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 128\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 130\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 136\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 144\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 145\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 168\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 169\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 176\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 177\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 181\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 187\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 189\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 194\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 196\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 205\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 209\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 232\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1980\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 4\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 6\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 13\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 22\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 23\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 28\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 30\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 34\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 46\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 55\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 63\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 70\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 76\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 106\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 110\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 111\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 118\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 135\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 143\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 156\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 163\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 171\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 172\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 182\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 185\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 191\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 197\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 201\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 207\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 213\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 221\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 222\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 226\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 228\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 229\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 230\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 233\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 235\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 239\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 241\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 245\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 248\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 1990\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 18\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 19\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 21\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 31\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 33\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 35\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 36\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 44\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 51\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 57\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 58\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 59\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 73\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 78\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 81\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 82\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 85\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 89\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 91\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 94\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 99\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 102\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 103\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 104\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 114\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 120\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 123\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 124\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 125\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 127\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 149\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 151\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 157\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 164\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 174\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 179\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 183\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 184\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 190\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 203\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 208\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 214\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 215\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 216\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 217\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 225\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 231\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 240\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 242\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 244\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2000\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 2\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 8\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 9\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 12\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 16\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 20\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 25\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 40\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 49\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 50\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 61\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 72\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 84\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 86\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 90\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 101\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 107\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 112\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 121\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 138\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 148\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 153\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 178\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 192\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 202\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 210\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 212\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 223\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        },\n        {\n          \"type\": \"map\",\n          \"from\": {\n            \"type\": \"axis\",\n            \"anchor\": \"lhs\",\n            \"select\": \"row\",\n            \"label\": 238\n          },\n          \"to\": {\n            \"type\": \"axis\",\n            \"anchor\": \"rhs\",\n            \"select\": \"row\",\n            \"label\": 2010\n          }\n        }\n      ],\n      \"data\": {\n        \"lhs\": \"prev_rhs\",\n        \"rhs\": {\n          \"type\": \"DataFrame\",\n          \"columns\": {\n            \"names\": [\n              null\n            ],\n            \"labels\": [\n              \"Title\",\n              \"Votes\",\n              \"Rating\",\n              \"Year\"\n            ]\n          },\n          \"index\": {\n            \"names\": [\n              \"Decade\"\n            ],\n            \"labels\": [\n              1920,\n              1930,\n              1940,\n              1950,\n              1960,\n              1970,\n              1980,\n              1990,\n              2000,\n              2010\n            ]\n          },\n          \"data\": [\n            [\n              \"The Kid\",\n              98794,\n              8.3,\n              1927\n            ],\n            [\n              \"The Wizard of Oz\",\n              259235,\n              8.5,\n              1939\n            ],\n            [\n              \"The Treasure of the Sierra Madre\",\n              350551,\n              8.6,\n              1949\n            ],\n            [\n              \"Witness for the Prosecution\",\n              384187,\n              8.9,\n              1959\n            ],\n            [\n              \"Y\\u00f4jinb\\u00f4\",\n              447875,\n              8.9,\n              1969\n            ],\n            [\n              \"The Sting\",\n              1027398,\n              9.2,\n              1979\n            ],\n            [\n              \"Tonari no Totoro\",\n              700283,\n              8.7,\n              1989\n            ],\n            [\n              \"Unforgiven\",\n              1498733,\n              9.2,\n              1999\n            ],\n            [\n              \"Yip Man\",\n              1473049,\n              8.9,\n              2009\n            ],\n            [\n              \"X-Men: Days of Future Past\",\n              1271949,\n              8.7,\n              2015\n            ]\n          ]\n        }\n      }\n    }\n  ]\n}", {"nohover": true, "maxDisplayRows": 7, "maxDisplayCols": 5, "colorPalette": ["#e41a1c", "#377eb8", "#4daf4a", "#984ea3", "#ff7f00", "#a65628", "#f781bf", "#999999"]});
</script>



- Acontece que este método **não** produz a resposta correta.
- Quando usamos um método de agregação (por exemplo, `.max()`), a agregação é feita para cada coluna individualmente.
- Embora seja verdade que o filme com maior audiência da década de 1990 tem uma classificação de 9,2, esse filme **não** Imperdoável – em vez disso, Imperdoável é o filme mais recente no alfabeto entre todos os filmes da década de 1990.
- Usar o `max` não ajuda aqui.

## Problemas de desafio

Não abordaremos esses problemas em aula, mas eles estão aqui para você praticar com alguns exemplos mais difíceis. Para acessar as soluções, você precisará assistir [this solution walkthrough video](https://youtu.be/xg7rnjWnZ48) (início às 10h).

Antes de assistir ao vídeo, **tente resolver esses problemas por conta própria** – eles são uma ótima preparação para trabalhos de casa, projetos e exames!

### Pergunta: Há quantos anos mais de 3 filmes foram classificados acima de 8,5?


```python
#In: 

```

#### Além: Usando `.sum()` em um array/série booleana

- A soma de um array/série booleana fornece uma contagem do número de elementos `True`. Isso ocorre porque Python trata `True` como 1 e `False` como 0.
- Você pode usar esse fato aqui?


```python
#In: 

```

### Pergunta: Dos anos com mais de 3 filmes, qual teve a classificação média mais alta?


```python
#In: 

```

### Pergunta: Qual ano teve os títulos de filmes mais longos, em média?

_Dica:_ Use `.str.len()` na coluna ou índice que contém os nomes dos filmes.


```python
#In: 

```

### Pergunta: Qual é a classificação média dos filmes dos anos que tiveram pelo menos 3 filmes no Top 250?


```python
#In: 

```

## Resumo, da próxima vez

### Resumo

- Podemos escrever consultas que envolvam múltiplas condições, desde que:
- Coloque parênteses em todas as condições.
- Separe as condições usando `&` se você precisar que todas sejam verdadeiras, ou `|` se você precisar que pelo menos uma seja verdadeira.
- A chamada de método `df.groupby(column_name).agg_method()` **agrega** todas as linhas com o mesmo valor para `column_name` em uma única linha no DataFrame resultante, usando `agg_method()` para combinar valores.
- Métodos de agregação a serem conhecidos: `.count()`, `.sum()`, `.mean()`, `.median()`, `.max()` e `.min()`.

### Próxima vez

Uma imagem vale mais que 1000 palavras – é hora de visualizar!
