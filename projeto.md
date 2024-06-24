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

---
---
**Sumário**
1. TOC
{:toc}
---

## Análise dos Acidentes de Trânsito de Belo Horizonte

Neste projeto vamos usar as técnicas que aprendemos em sala
de aula em alguns problemas de ciência de dados. Nosso
projeto tem um tema fixo, pense no mesmo como um
laboratório mais complexo. O importante do projeto é que
você siga todo o ciclo de trabalho de um cientista de
dados. Ou seja, é uma análise além do trivial (computar uma
média) de uma base que necessita de das duas partes da
matéria.

### Base de Dados

A base de dados que vocês devem analisar consiste de
acidentes de trânsito na cidade de Belo Horizonte. A base
é separada em vários arquivos `csv` que podem ser lidos
diretamente com `babypandas`. Para tal, use a chamada
abaixo:

```python
import babypandas as bpd
import glob
import pandas as pd

df = bpd.DataFrame()
for f in glob.glob('*.csv'):
    aux = bpd.read_csv(f, sep=';')
    df = df.append(aux)

data_correta = pd.to_datetime(
    df.get('data hora_boletim').values
)

df = df.assign(
    data_boletim = data_correta
)
```

Observe como a mesma faz uso de uma biblioteca chamada de
`glob`. A biblioteca glob serve para ler todos os arquivos
de uma data pasta. Por exemplo, se sua pasta tem os
seguintes arquivos:

```
$ ls
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2011.csv
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
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2023.csv
-rw-r--r-- 1 flaviovdf flaviovdf 4683223 Jun 24 08:59 si-bol-2024.csv
-rw-r--r-- 1 flaviovdf flaviovdf    3354 Jun 24 09:06 Projeto.ipynb
```

A linha do código `glob.glob('*.csv')` retornar todos os
arquivos que terminam em `.csv`. Isto é:

```
['si-bol-2016.csv',
 'si-bol-2018.csv',
 'si-bol-2021.csv',
 'si-bol-2023.csv',
 'si-bol-2011.csv',
 'si-bol-2017.csv',
 'si-bol-2015.csv',
 'si-bol-2019.csv',
 'si-bol-2012.csv',
 'si-bol-2024.csv',
 'si-bol-2022.csv',
 'si-bol-2014.csv',
 'si-bol-2013.csv']
```

Problemas.
1. Explicar a chamada do clean
2. Mapear colunas para ids corretos
3. Limpar x e y estranho
5. Plotar mapa de BH por tipo de acidente
6. Plotar série temporal de acidentes por ano/mês
7. Remover 2020 e 2021 da base, pandemia
8. Plotar IC via bootstrap de número de acidentes por mês
9. Usar número de acidentes de 2022 para prever 2023
10. Fazer análises adicionais que tenha interesse
