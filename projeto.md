---
layout: page
title: Projeto
description: Sobre o Projeto.
nav_order: 2
---

# Projeto

{: .no_toc .mb-2 }

Praticar a matéria fazendo uma análise não trivial
{: .fs-6 .fw-300 }

**Sumário**
1. TOC
{:toc}
---

## Análise dos Acidentes de Trânsito de Belo Horizonte

Neste projeto, vamos aplicar as técnicas aprendidas em sala de aula a alguns problemas de ciência de dados. Nosso projeto tem um tema fixo, funcionando como um laboratório mais complexo. O importante é que você siga todo o ciclo de trabalho de um cientista de dados, realizando uma análise além do trivial (como computar uma média) em uma base de dados que requer as duas partes da matéria.

Para baixar a base, [clique aqui](http://flaviovdf.io/fcd/assets/99-Projeto/dados.zip).

### Base de Dados

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

#### Lendo vários arquivos

Observe que o código faz uso da biblioteca glob, que serve para ler todos os arquivos de uma determinada pasta. Por exemplo, se sua pasta contiver os seguintes arquivos:

```
$ ls
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2012.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2013.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2014.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2015.csv
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
 'si-bol-2022.csv',
 'si-bol-2014.csv',
 'si-bol-2013.csv']
```

#### Colando um índice de datas 

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

#### Removendo as colunas que não precisamos

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
    - Dica 2: Procure no material da disciplina o local que falamos de funções e apply

1. Limpar Valores Estranhos nas Colunas X e Y:
    - Inspecione as colunas que representam coordenadas geográficas (x e y). Identifique e corrija valores anômalos ou fora do esperado. Considere valores nulos ou que estejam fora do alcance geográfico da cidade de Belo Horizonte. Por exemplo, os valores de eixo x ou y igual a zero tem que sair. Além do mais, existem valores extremos. Faça um gráfico de dispersão, próximo item, indentifique os mesmos e remova-os.
    - Dica 1: Filtre qualquer x ou y menor do que dez milhões (10_000_000). A latitudes e longitudes de BH não é menor do que isso.
    - Dica 2: Vá para a aula de filtro e queries.

1. Faça um gráfico de barras **por ano** indicandos os tipos de acidentes mais comuns no ano.
    - Dica: Vá para a aula de visualizações e para a aula de groupby

1. Repita o gráfico acima considerando gráficos fatais e não fatais.
    
1. Plotar Mapa de Belo Horizonte por Tipo de Acidente:
    - Faça um gráfico de dispersão das coordenadas X e Y dos acidentes. O mesmo deve parecer com o mapa de BH. Para entender bem os tipos de acidentes, plote os locais dos acidentes e categorize-os por tipo. Utilize cores ou marcadores distintos para cada tipo de acidente.
      
1. Plotar Série Temporal de Acidentes por Ano/Mês:
    - Crie gráficos de séries temporais que mostrem a quantidade de acidentes ao longo do tempo, agregando os dados por ano e mês. Identifique tendências, picos e padrões sazonais.
      
1. Remover Dados de 2020 e 2021 da Base, Devido à Pandemia:
    - Exclua os dados referentes aos anos de 2020 e 2021, pois os padrões de acidentes nesses anos podem ter sido fortemente influenciados pela pandemia de COVID-19 e podem não refletir a tendência geral.
      
1. Plotar Intervalo de Confiança via Bootstrap do Número de Acidentes por Mês:
    - Utilize a técnica de bootstrap para calcular intervalos de confiança para o número de acidentes em cada mês. Plote esses intervalos juntamente com a média mensal dos acidentes para visualizar a variabilidade e a confiança das estimativas.
      
1. Usar o Número de Acidentes de 2021 para Prever 2022:
    - Aplique métodos de previsão (como modelos de regressão) para estimar o número de acidentes em 2023, utilizando os dados de 2022 como base. Avalie a precisão do modelo e discuta suas limitações.
      
1. Fazer Análises Adicionais de Interesse:
    - Realize análises exploratórias adicionais que sejam de seu interesse ou relevância para o projeto. Isso pode incluir a correlação entre diferentes variáveis, a análise de hotspots de acidentes, ou a investigação de fatores contribuintes para a gravidade dos acidentes.
      
1. Fazer análises adicionais que tenha interesse
