---
layout: page
title: Referência
description: Guia Rápido de Referência do Curso.
---

# Referência do Curso FCD.

---

Abaixo, `df` é um DataFrame, `ser` é uma Série, `arr` é um array. `babypandas` foi importado como `bpd` e `numpy` como `np`.

## Construindo e Organizando DataFrames

Cada função/método abaixo cria um novo DataFrame. Lembre-se de salvá-lo!

- `bpd.DataFrame()`: Cria um DataFrame vazio.
- `bpd.read_csv(caminho_para_arquivo)`: Cria um DataFrame a partir de um arquivo CSV.
- `df.assign(nome_da_coluna=dados_da_coluna)`: Adiciona/substitui uma coluna.
- `df.drop(columns=nome_da_coluna ou [col_1_nome, ... col_k_nome])`: Remove coluna(s).
- `df.set_index(nome_da_coluna)`: Move uma coluna para o índice.
- `df.reset_index()`: Move o índice para uma coluna.
- `df.sort_values(by=nome_da_coluna, ascending=True)`: Ordena o DataFrame.
- `left.merge(right, left_on=coluna_esquerda, right_on=coluna_direita):` Junta os DataFrames left e right pelas colunas especificadas. Pode usar on em vez de left_on e right_on se os nomes das colunas forem iguais.

## Arrays e NumPy

- `arr[indice]`: Acesso por índice.
- `np.append(arr, valor)`: Adiciona `valor` ao final de `arr`.

## Plotagem

- `df.plot(kind=tipo, x=col_x, y=col_y)`: Desenha um gráfico (`scatter`, `line`, `bar`, `barh`).

## Acessando Dados

- `df.shape[]`: Número de linhas e colunas.
- `df.get(nome_da_coluna)`: Acesso a uma coluna como Série.

## Métodos de Série

- `.count()`, `.max()`, `.min()`, `.sum()`, `.mean()`, `.median()`, `.unique()`

## Consultando

- `df[df.get(nome_da_coluna)] > 42`: Filtra linhas.

## Agrupando

- `df.groupby(nome_da_coluna)`: Seguido por `.mean()`, `.median()`, etc.

## Escrevendo Funções

- `def nome_da_função(argumento1, ... argumentoK): corpo_da_função`

## Aplicando Funções

- `df.get(nome_da_coluna).apply(nome_da_função)`: Aplica função a cada entrada.

## Estatísticas e Teste de Hipóteses

- Conceitos de experimento, modelo, teste de hipótese, p-valor, etc.

## Unidades Padrão, Correlação, Regressão

- Conversão para unidades padrão, coeficiente de correlação, linha de regressão.

## Distribuição de Espalhamento

- Variância, desvio padrão, desigualdade de Chebyshev, distribuição normal.
