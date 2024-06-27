---
layout: page
title: Projeto
description: Sobre o Projeto.
nav_order: 2
---

# Análise dos Acidentes de Trânsito de Belo Horizonte

{: .no_toc .mb-2 }

Praticar a matéria fazendo uma análise não trivial
{: .fs-6 .fw-300 }

**Sumário**
1. TOC
{:toc}
---

## Introdução

Neste projeto, vamos aplicar as técnicas aprendidas em sala de aula a alguns problemas de ciência de dados. Nosso projeto tem um tema fixo, funcionando como um laboratório mais complexo. O importante é que você siga todo o ciclo de trabalho de um cientista de dados, realizando uma análise além do trivial (como computar uma média) em uma base de dados que requer as duas partes da matéria.

Para baixar a base, [clique aqui](http://flaviovdf.io/fcd/assets/99-Projeto/dados.zip).

## Base de Dados

A base de dados a ser analisada consiste em registros de acidentes de trânsito na cidade de Belo Horizonte. A base é separada em vários arquivos csv que podem ser lidos diretamente com babypandas. Para isso, use o código abaixo:

```python
import babypandas as bpd
import glob
import pandas as pd

df = bpd.DataFrame()
for f in glob.glob('*.csv'):
    aux = bpd.read_csv(f, sep=';')
    df = df.append(aux)
```

### Lendo vários arquivos

Observe que o código faz uso da biblioteca glob, que serve para ler todos os arquivos de uma determinada pasta. Por exemplo, se sua pasta contiver os seguintes arquivos:

```
$ ls
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2016.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2017.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2018.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2019.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2021.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2022.csv
-rw-r--r-- 1 flaviovdf flaviovdf    3354 Jun 24 09:06 Projeto.ipynb
```
A linha de código glob.glob('*.csv') retornará todos os arquivos que terminam em .csv, como:

```python
['si-bol-2016.csv',
 'si-bol-2018.csv',
 'si-bol-2021.csv',
 'si-bol-2017.csv',
 'si-bol-2015.csv',
 'si-bol-2019.csv',
 'si-bol-2012.csv',
 'si-bol-2020.csv',
 'si-bol-2022.csv']
```

### Colando um índice de datas 

Infelizmente, o BabyPandas não lida muito bem com datas. Por isso, vamos tratar as datas usando pandas. A função pd.to_datetime converte texto em datas, funcionando bem no nosso caso.

```python
import pandas as pd

data_correta = pd.to_datetime(
    df.get('data hora_boletim').values
)
```

Agora basta colocar a data na coluna correta

```python
df = df.assign(
    data_boletim = data_correta
)
```

### Removendo as colunas que não precisamos

Por fim, vamos remover todas as colunas desnecessárias e configurar um índice.

```python
df = df.drop(
    columns=['data hora_boletim',
             'data_inclusao',
             'valor_ups',
             'valor_ups_antiga',
             'data_alteracao_smsa',
             'descricao_ups_antiga']
).sort_values(by='data_boletim').set_index('data_boletim')
```

## Problemas

Agora é com vocês. Com o novo DataFrame, vocês devem abordar os 8 problemas abaixo:

1. Mapear Colunas para IDs Corretos:
    - Identifique as colunas no DataFrame que representam categorias mas estão codificadas com números ou códigos como `H06002`. Renomeie ou mapeie essas colunas para IDs corretos que facilitem a análise e manipulação dos dados. Para mapear as colunas para nomes interessante, vocês devem fazer uso do dicionário de varáveis disponível [aqui](https://dados.pbh.gov.br/dataset/relacao-de-ocorrencias-de-acidentes-de-transito-com-vitima). Abra o mesmo em excell ou google sheets.
    - Dica 1: Use um `if` ou se quiser se souber, um dicionário, para mapear cada categoria no texto correto.
    - Dica 2: Salve o excell como csv e carregue em BabyPandas
    - Dica 3: Procure no material da disciplina o local que falamos de funções e apply

1. Faça um gráfico **de linhas** por ano mês indicando o número de acidentes naquele ano mês.
    - Dica 1: Comece com um `df.groupby(['ano', 'mes'].size()`
    - Dica 2: Faça o plot usando a resposta da chamada acima
    - Dica 3: Olhe a aula de Visualizações, a chamada `.plot`. 

1. Repita o gráfico acima por **ano** apenas.

1. Faça um gráfico de barras **por ano** indicandos os tipos de acidentes mais comuns no ano.
    - Dica: Vá para a aula de visualizações e para a aula de groupby

1. Repita o gráfico acima considerando gráficos fatais e não fatais.
    - Dica: Vá para a aula de visualizações e para a aula de groupby
  
1. Faça a mesma análise por bairro e por acidentes fatais e não fatais
    
1. Plotar Mapa de Belo Horizonte por Tipo de Acidente:
    - Faça um gráfico de dispersão das latitudes e longitudes. O mesmo deve parecer com o mapa de BH. Para entender bem os tipos de acidentes, faça gráficos por tipos diferentes de acidentes.
    - Dica 1: use o plot hexbin conforme abaixo
    - Dica 2: faça um gráfico diferente por tipo, se colocar todos no mesmo gráfico vai ser impossível ver
    ```
    df.plot(
        kind='hexbin',
        x='lat',
        y='lon',
        mincnt=1,
        gridsize=75,
        bins='log'
    )
    ```
      
1. Plotar Intervalo de Confiança via Bootstrap do Número de Acidentes por Mês:
    - Utilize a técnica de bootstrap para calcular intervalos de confiança para o número de acidentes em cada mês. Plote esses intervalos juntamente com a média mensal dos acidentes para visualizar a variabilidade e a confiança das estimativas.


1. Vamos brincar de regressão. Inicialmente, faça um gráfico de dispersão onde o eixo X é o número de acidentes em 2019, 2020 ou 2021. Serão três gráficos. Cada ponto no gráfico vai corresponder a um bairro.
    - Dica 1: Faça um `df.groupby(['bairro', 'ano']).size()`
    - Dica 2: Faça um merge para juntar os dois anos
    ```python
    cont = df_filt.groupby(['bairro', 'ano']).size().reset_index()
    dados_2019 = cont[cont.get('ano') == 2021]
    dados_2022 = cont[cont.get('ano') == 2022]
    dados_2019.merge(
        dados_2022,
        on='bairro'
    ```
      
1. Use o Número de Acidentes por bairro de 2019 para Prever 2022:
    - Também use os de 2020
    - Por fim use os de 2022
    - Aplique métodos de previsão (como modelos de regressão) para estimar o número de acidentes em 2023, utilizando os dados passados.
    - Avalie os erros do modelo e discuta as limitações.
    - Compare os três modelos. A pandemia teve algum impacto, qual?
      
1. Fazer Análises Adicionais de Interesse:
    - Realize análises adicionais que sejam de seu interesse ou relevância para o projeto. Isso pode incluir a correlação entre diferentes variáveis, a análise de hotspots de acidentes, ou a investigação de fatores contribuintes para a gravidade dos acidentes.
