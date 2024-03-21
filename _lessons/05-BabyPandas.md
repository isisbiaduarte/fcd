---
layout: page
title: BabyPandas
nav_order: 4
---
[<img src="./colab_favicon_small.png" style="float: right;">](https://colab.research.google.com/github/flaviovdf/fcd/blob/master/_lessons/04-Arrays.ipynb)

# Aula 4 – DataFrames: Acessando DataFrames e Séries

### Agenda

Hoje, usaremos um conjunto de dados real e muitas perguntas motivadoras para ilustrar as principais técnicas de manipulação de DataFrame.

#### Observação:

- Alguns links importantes daqui para frente:
  -[Comandos e Conceitos Úteis](https://flaviovdf.io/fcd/guiarapido/).
  -[`babypandas` notes](https://notes.dsc10.com).
  -[`babypandas` documentation](https://babypandas.readthedocs.io/en/latest/index.html).

## Tabelas de Dados

### `pandas`

- DataFrames (tabelas) são fornecidos por um pacote chamado `pandas`.
- `pandas` é **a** ferramenta para fazer ciência de dados em Python.

<centro>
<img src='https://raw.githubusercontent.com/flaviovdf/fcd/master/assets/05-BabyPandas/images/pandas.png' width=500>
</center>

### Mas a biblioteca `pandas` padrão não é tão fofa...

<centro>
<img height=100% src="https://raw.githubusercontent.com/flaviovdf/fcd/master/assets/05-BabyPandas/images/angrypanda.jpg"/>
</center>

### Digite `babypandas`!

- Criada pela Universidade de California San Diego (UCSD) criamos uma versão menor e mais agradável de `pandas` chamada `babypandas`.
- Mantém as coisas importantes e tem mensagens de erro muito melhores.
- É mais fácil de aprender, mas ainda é um código `pandas` válido.

<centro>
<img height=75% src="https://raw.githubusercontent.com/flaviovdf/fcd/master/assets/05-BabyPandas/images/babypanda.jpg"/ width=500>
</center>

### DataFrames em `babypandas` 🐼

- As tabelas em `babypandas` (e `pandas`) são chamadas de "DataFrames".
- Para usar DataFrames, precisaremos importar `babypandas`. (Precisaremos de `numpy` também.)


```python
#In: 
import babypandas as bpd
import numpy as np
```

### Sobre os dados: Feira da Afonso Pena 👷

- Normalmente trabalharemos com dados armazenados no formato CSV. CSV significa “valores separados por vírgula”.
- O arquivo [afonso_pena.csv](https://raw.githubusercontent.com/flaviovdf/fcd/master/assets/05-BabyPandas/images/) contém informações sobre as barracas da feira. Tais dados foram coletados da Prefeitura de Belo Horizonte [Dados Abertos PBH](https://dados.pbh.gov.br/dataset/dicionario-de-dados-feira-afonso-pena-barraca).

<centro>
<img height=75% src="https://raw.githubusercontent.com/flaviovdf/fcd/master/assets/05-BabyPandas/images/afonsopena.webp"/ width=500>
</center>

### Lendo dados de um arquivo 📖

Podemos ler em um CSV usando `bpd.read_csv(...)`. Forneça o caminho para um arquivo relativo ao seu notebook (se o arquivo estiver na mesma pasta do seu notebook, esse é apenas o nome do arquivo).


```python
#In: 
afonso_pena = bpd.read_csv('afonso_pena.csv')
afonso_pena
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
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>83</td>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>85</td>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>86</td>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>87</td>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>7</td>
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
    </tr>
    <tr>
      <th>1524</th>
      <td>1525</td>
      <td>D.F1.V007</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Vestuário</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1525</th>
      <td>1526</td>
      <td>C.F4.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Cama, Mesa, Banho e Tapeçaria</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>1527</td>
      <td>F.F1.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Criança</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1527</th>
      <td>1528</td>
      <td>Y.F2.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Alimentação</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1528</th>
      <td>1529</td>
      <td>A.F3.V005</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Mobilário, Flores, Arranjos, Cestaria</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1529 rows × 7 columns</p>
</div>



### Estrutura de um DataFrame

- DataFrames possuem *colunas* e *linhas*.
- Pense em cada coluna como um array. As colunas contêm dados do mesmo `tipo`.
- Cada coluna possui um rótulo, por ex. `'NOME_SETOR'` e `'NOME_FEIRANTE'`.
- O rótulo de uma coluna é o seu nome.
- Os rótulos das colunas são armazenados como strings.
- Cada linha também possui um rótulo.
- Juntos, os rótulos das linhas são chamados de _índice_. O índice **não** é uma coluna!



```python
#In: 
afonso_pena
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
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>83</td>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>85</td>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>86</td>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>87</td>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>7</td>
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
    </tr>
    <tr>
      <th>1524</th>
      <td>1525</td>
      <td>D.F1.V007</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Vestuário</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1525</th>
      <td>1526</td>
      <td>C.F4.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Cama, Mesa, Banho e Tapeçaria</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>1527</td>
      <td>F.F1.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Criança</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1527</th>
      <td>1528</td>
      <td>Y.F2.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Alimentação</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1528</th>
      <td>1529</td>
      <td>A.F3.V005</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Mobilário, Flores, Arranjos, Cestaria</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1529 rows × 7 columns</p>
</div>



### Configurando um novo índice

- Podemos definir um índice melhor usando `.set_index(column_name)`.
- Os rótulos das linhas devem ser identificadores exclusivos.
- Os rótulos das linhas são nomes de linhas; idealmente, cada linha tem um nome descritivo diferente.
- ⚠️ Como a maioria dos métodos DataFrame, `.set_index` retorna um novo DataFrame; não modifica o DataFrame original.


```python
#In: 
afonso_pena.set_index('ID_FEIRA_AFONSO_PENA_BARRACA')
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
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
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
      <td>5</td>
    </tr>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>5</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>87</th>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>7</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1525</th>
      <td>D.F1.V007</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Vestuário</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>C.F4.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Cama, Mesa, Banho e Tapeçaria</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1527</th>
      <td>F.F1.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Criança</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1528</th>
      <td>Y.F2.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Alimentação</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1529</th>
      <td>A.F3.V005</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Mobilário, Flores, Arranjos, Cestaria</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1529 rows × 6 columns</p>
</div>




```python
#In: 
afonso_pena
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
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
      <th>CODIGO_VAGA</th>
      <th>NOME_FANTASIA</th>
      <th>NOME_FEIRANTE</th>
      <th>NOME_PREPOSTO</th>
      <th>NOME_SETOR</th>
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>83</td>
      <td>F.F2.V016</td>
      <td>BARRACA CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>CARMEN EMMANUEL DOS SANTOS SILVA</td>
      <td>JANA FONSECA VIEIRA</td>
      <td>Criança</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>85</td>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>86</td>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>87</td>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>7</td>
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
    </tr>
    <tr>
      <th>1524</th>
      <td>1525</td>
      <td>D.F1.V007</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Vestuário</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1525</th>
      <td>1526</td>
      <td>C.F4.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Cama, Mesa, Banho e Tapeçaria</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>1527</td>
      <td>F.F1.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Criança</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1527</th>
      <td>1528</td>
      <td>Y.F2.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Alimentação</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1528</th>
      <td>1529</td>
      <td>A.F3.V005</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Mobilário, Flores, Arranjos, Cestaria</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1529 rows × 7 columns</p>
</div>




```python
#In: 
afonso_pena = afonso_pena.set_index('ID_FEIRA_AFONSO_PENA_BARRACA')
afonso_pena
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
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
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
      <td>5</td>
    </tr>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>5</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>87</th>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>7</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1525</th>
      <td>D.F1.V007</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Vestuário</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>C.F4.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Cama, Mesa, Banho e Tapeçaria</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1527</th>
      <td>F.F1.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Criança</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1528</th>
      <td>Y.F2.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Alimentação</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1529</th>
      <td>A.F3.V005</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Mobilário, Flores, Arranjos, Cestaria</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1529 rows × 6 columns</p>
</div>



### Forma de um DataFrame

- `.shape` retorna o número de linhas e colunas em um determinado DataFrame.
- Acesse cada um com `[]`:
- `.shape[0]` para linhas.
- `.shape[1]` para colunas.


```python
#In: 
# There were 7 columns before, but one of them became the index, and the index is not a column!
afonso_pena.shape
```




    (1529, 6)




```python
#In: 
# Number of rows
afonso_pena.shape[0]
```




    1529




```python
#In: 
# Number of columns
afonso_pena.shape[1]
```




    6



## Exemplo 1: Total, Media e Mediana de Produtos

**Conceitos principais:** Acessar colunas, entender operações em colunas numéricas.

### Encontrando o total de solicitações

- **Pergunta:** Como sumarizar a quantidade de colunas?
- Obtenha a coluna
- Agregue o valor

#### Etapa 1 – Obtendo uma coluna

- Podemos obter uma coluna de um DataFrame usando `.get(column_name)`.
- ⚠️ Os nomes das colunas diferenciam maiúsculas de minúsculas!
- Os nomes das colunas são strings, então precisamos usar aspas.
- O resultado parece um DataFrame de 1 coluna, mas na verdade é uma *Série*.


```python
#In: 
afonso_pena
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
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
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
      <td>5</td>
    </tr>
    <tr>
      <th>84</th>
      <td>G.F3.V052</td>
      <td>BARRACA CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>CARMEN FERNANDA ROCHA DE ALCANTARA</td>
      <td>KARINA RODRIGUES BRANDORFI</td>
      <td>Bijouterias</td>
      <td>5</td>
    </tr>
    <tr>
      <th>85</th>
      <td>E.F4.V003</td>
      <td>BARRACA CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>CARMEN LÚCIA CARVALHO DE ALMEIDA</td>
      <td>BARBARA ISABELLE CARVALHO DE PAULA</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>86</th>
      <td>E.F2.V004</td>
      <td>BARRACA CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>CECÍLIA PAGANO NEVES SALAZAR</td>
      <td>GISELE PAGANO NEVES SALAZAR</td>
      <td>Vestuário Infantil</td>
      <td>3</td>
    </tr>
    <tr>
      <th>87</th>
      <td>D.F2.V016</td>
      <td>BARRACA CÉLIA APARECIDA DE SOUZA</td>
      <td>CÉLIA APARECIDA DE SOUZA</td>
      <td>EDSON PIRES DE SOUZA</td>
      <td>Vestuário</td>
      <td>7</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1525</th>
      <td>D.F1.V007</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Vestuário</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1526</th>
      <td>C.F4.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Cama, Mesa, Banho e Tapeçaria</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1527</th>
      <td>F.F1.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Criança</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1528</th>
      <td>Y.F2.V006</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Alimentação</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1529</th>
      <td>A.F3.V005</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Mobilário, Flores, Arranjos, Cestaria</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1529 rows × 6 columns</p>
</div>




```python
#In: 
afonso_pena.get('NUMERO_PRODUTOS_CADASTRADOS')
```




    ID_FEIRA_AFONSO_PENA_BARRACA
    83      5
    84      5
    85      3
    86      3
    87      7
           ..
    1525    1
    1526    1
    1527    1
    1528    1
    1529    1
    Name: NUMERO_PRODUTOS_CADASTRADOS, Length: 1529, dtype: int64



### Digressão: Série

- Uma *Série* é como um array, mas com um índice.
- Em particular, as séries suportam aritmética.


```python
#In: 
afonso_pena.get('NUMERO_PRODUTOS_CADASTRADOS')
```




    ID_FEIRA_AFONSO_PENA_BARRACA
    83      5
    84      5
    85      3
    86      3
    87      7
           ..
    1525    1
    1526    1
    1527    1
    1528    1
    1529    1
    Name: NUMERO_PRODUTOS_CADASTRADOS, Length: 1529, dtype: int64



#### Passo 2 – Calculando o total

- Assim como acontece com os arrays, podemos realizar operações aritméticas nas séries


```python
#In: 
afonso_pena.get('NUMERO_PRODUTOS_CADASTRADOS').sum()
```




    8727




```python
#In: 
afonso_pena.get('NUMERO_PRODUTOS_CADASTRADOS').max()
```




    21




```python
#In: 
afonso_pena.get('NUMERO_PRODUTOS_CADASTRADOS').mean()
```




    5.707652060170045



## Exemplo 3: Quais feirantes vendem mais produtos?

**Conceitos principais**: Classificação. Acessando usando posições inteiras.

#### Etapa 1 – Classificando o DataFrame

- Use o método `.sort_values(by=column_name)` para classificar.
- O `by=` pode ser omitido, mas ajuda na legibilidade.
- Como a maioria dos métodos DataFrame, retorna um novo DataFrame.


```python
#In: 
afonso_pena.sort_values(by='NUMERO_PRODUTOS_CADASTRADOS')
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
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
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
      <th>1529</th>
      <td>A.F3.V005</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Mobilário, Flores, Arranjos, Cestaria</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1148</th>
      <td>B.F2.V012</td>
      <td>BARRACA ELVIDIO ROCHA SILVA</td>
      <td>ELVIDIO ROCHA SILVA</td>
      <td>ADRIANE GISELLE DE ARAÚJO</td>
      <td>Decoração e Utilidades</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1142</th>
      <td>E.F1.V025</td>
      <td>BARRACA MARIA MARTHA DE FARIA STEIJVERS</td>
      <td>MARIA MARTHA DE FARIA STEIJVERS</td>
      <td>CARMEN LUCIA LOPES DE FARIA</td>
      <td>Vestuário Infantil</td>
      <td>1</td>
    </tr>
    <tr>
      <th>346</th>
      <td>I.F2.V021</td>
      <td>BARRACA LUZIA FRADE RIBEIRO</td>
      <td>LUZIA FRADE RIBEIRO</td>
      <td>ALEXANDRA PEREIRA SILVA</td>
      <td>Cintos, Bolsas e Acessórios</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1029</th>
      <td>P.F2.V005</td>
      <td>BARRACA VALTER APARECIDA DA SILVA</td>
      <td>VALTER APARECIDA DA SILVA</td>
      <td>NaN</td>
      <td>Artes e Pintura</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>336</th>
      <td>B.F1.V001</td>
      <td>BARRACA LUCY DOS SANTOS SEBASTIAO</td>
      <td>LUCY DOS SANTOS SEBASTIAO</td>
      <td>LAURO MARTINS DOS SANTOS</td>
      <td>Decoração e Utilidades</td>
      <td>20</td>
    </tr>
    <tr>
      <th>1406</th>
      <td>Z.F1.V007</td>
      <td>BARRACA FRANCINERE AMARAL CARDOSO RIBEIRO DE S...</td>
      <td>FRANCINERE AMARAL CARDOSO RIBEIRO DE SOUZA</td>
      <td>RAYKARD AGUIAR DE JESUS</td>
      <td>Alimentação</td>
      <td>20</td>
    </tr>
    <tr>
      <th>653</th>
      <td>F.F1.V015</td>
      <td>BARRACA SILVIA REGINA NOGUEIRA RIBEIRO</td>
      <td>SILVIA REGINA NOGUEIRA RIBEIRO</td>
      <td>LIGIA MARIA NOGUEIRA RIBEIRO</td>
      <td>Criança</td>
      <td>20</td>
    </tr>
    <tr>
      <th>109</th>
      <td>Y.F1.V012</td>
      <td>BARRACA DAYSE PINTO NORBERTO</td>
      <td>DAYSE PINTO NORBERTO</td>
      <td>DJALMA ANTÔNIO DE FREITAS</td>
      <td>Alimentação</td>
      <td>21</td>
    </tr>
    <tr>
      <th>491</th>
      <td>F.F2.V011</td>
      <td>BARRACA MARILEA IMACULADA MUNIZ COSTA</td>
      <td>MARILEA IMACULADA MUNIZ COSTA</td>
      <td>KELLINGTON NONATO MUNIZ COSTA</td>
      <td>Criança</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
<p>1529 rows × 6 columns</p>
</div>



Isso classifica, mas em ordem crescente (de pequeno para grande). Queremos o contrário!

#### Etapa 1 – Classificando o DataFrame em ordem *decrescente*

- Use `.sort_values(by=column_name, ascending=False)` para classificar em ordem *decrescente*.
- `ascendente` é um argumento opcional. Se omitido, será definido como `True` por padrão.
- Este é um exemplo de *argumento de palavra-chave* ou *argumento nomeado*.
- Se quisermos especificar a ordem de classificação, devemos usar a palavra-chave `ascendente=`.


```python
#In: 
ordenado = afonso_pena.sort_values(by='NUMERO_PRODUTOS_CADASTRADOS', ascending=False)
ordenado
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
      <th>NUMERO_PRODUTOS_CADASTRADOS</th>
    </tr>
    <tr>
      <th>ID_FEIRA_AFONSO_PENA_BARRACA</th>
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
      <th>491</th>
      <td>F.F2.V011</td>
      <td>BARRACA MARILEA IMACULADA MUNIZ COSTA</td>
      <td>MARILEA IMACULADA MUNIZ COSTA</td>
      <td>KELLINGTON NONATO MUNIZ COSTA</td>
      <td>Criança</td>
      <td>21</td>
    </tr>
    <tr>
      <th>109</th>
      <td>Y.F1.V012</td>
      <td>BARRACA DAYSE PINTO NORBERTO</td>
      <td>DAYSE PINTO NORBERTO</td>
      <td>DJALMA ANTÔNIO DE FREITAS</td>
      <td>Alimentação</td>
      <td>21</td>
    </tr>
    <tr>
      <th>1406</th>
      <td>Z.F1.V007</td>
      <td>BARRACA FRANCINERE AMARAL CARDOSO RIBEIRO DE S...</td>
      <td>FRANCINERE AMARAL CARDOSO RIBEIRO DE SOUZA</td>
      <td>RAYKARD AGUIAR DE JESUS</td>
      <td>Alimentação</td>
      <td>20</td>
    </tr>
    <tr>
      <th>336</th>
      <td>B.F1.V001</td>
      <td>BARRACA LUCY DOS SANTOS SEBASTIAO</td>
      <td>LUCY DOS SANTOS SEBASTIAO</td>
      <td>LAURO MARTINS DOS SANTOS</td>
      <td>Decoração e Utilidades</td>
      <td>20</td>
    </tr>
    <tr>
      <th>653</th>
      <td>F.F1.V015</td>
      <td>BARRACA SILVIA REGINA NOGUEIRA RIBEIRO</td>
      <td>SILVIA REGINA NOGUEIRA RIBEIRO</td>
      <td>LIGIA MARIA NOGUEIRA RIBEIRO</td>
      <td>Criança</td>
      <td>20</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1493</th>
      <td>G.F1.V031</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Bijouterias</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1492</th>
      <td>E.F2.V032</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Vestuário Infantil</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1491</th>
      <td>E.F1.V034</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Vestuário Infantil</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1490</th>
      <td>G.F2.V035</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Bijouterias</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1529</th>
      <td>A.F3.V005</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>ECONOMIA POPULAR SOLIDÁRIA</td>
      <td>NaN</td>
      <td>Mobilário, Flores, Arranjos, Cestaria</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1529 rows × 6 columns</p>
</div>



## Resumo

### Resumo

- Aprendemos alguns métodos e técnicas do DataFrame.
- Não sinta necessidade de memorizá-los todos imediatamente.
- Com o tempo, essas técnicas se tornarão cada vez mais familiares.
- **Pratique!** Elabore suas próprias perguntas usando este conjunto de dados e tente respondê-las.

### Próxima vez

- Responderemos a perguntas mais complicadas, que nos levarão a um novo método principal do DataFrame, `.groupby`, para organizar linhas de um DataFrame com o mesmo valor em uma coluna específica.
- Por exemplo, podemos querer organizar os dados por bairro, recolhendo todos os diferentes pedidos de serviço para cada bairro.
