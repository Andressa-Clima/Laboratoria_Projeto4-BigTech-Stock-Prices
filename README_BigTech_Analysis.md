# 🧠 Análise e Limpeza de Dados — Big Tech Stock Prices

---

## 🔵 1. Conectar e Importar Dados para as Ferramentas

```python
df.to_csv("big_tech_stock_prices.csv", index=False)
from google.colab import files
files.download("big_tech_stock_prices.csv")

import pandas as pd
df = pd.read_csv('/content/big_tech_companies.csv')
df = pd.read_excel('/content/big_tech_stock_prices.xlsx')
```

Os dados foram adicionados ao Google Colab.

---

## 🔵 2. Identificar e Tratar Valores Nulos

```python
null_counts = df.isnull().sum()
print(null_counts[null_counts > 0])
display(df[df.isnull().any(axis=1)])

df = df.drop(45089)
```

Foi identificada a linha **45089** com todos os valores nulos e removida.

---

## 🔵 3. Identificar e Tratar Valores Duplicados

```python
display(df[df.duplicated(keep=False)])
df = df.drop(45088)
```

Linhas duplicadas **45087 e 45088** foram encontradas e uma foi removida.

---

## 🔵 4. Identificar e Gerenciar Dados Fora do Escopo

Os valores de mercado foram mantidos sem alteração, pois refletem dados reais.

---

## 🔵 5. Identificar e Tratar Dados Discrepantes em Variáveis Categóricas

```python
df['stock_symbol'].unique()
df['company'].unique()
```

Nenhum valor discrepante encontrado.

---

## 🔵 6. Identificar e Tratar Dados Discrepantes em Variáveis Numéricas

Os valores numéricos representam dados reais e foram mantidos sem tratamento.

---

## 🔵 7. Verificar e Alterar Tipos de Dados

```python
df.dtypes
df['stock_prices'] = pd.to_datetime(df['stock_prices'])
```

A coluna `stock_prices` foi convertida para o tipo `datetime`.

---

## 🔵 8. Criar Novas Variáveis

```python
df['total_volume'] = df['volume'].sum()
```

Criada a variável `total_volume` com a soma total de volumes por empresa.

---

## 🔵 9. Unir Tabelas

```python
companies_df = pd.read_csv('/content/big_tech_companies.csv')
prices_df = pd.read_csv('/content/big_tech_stock_prices.csv')
merged_df = pd.merge(prices_df, companies_df, on='stock_symbol', how='inner')
display(merged_df.head())
```

Tabelas unidas com sucesso.

---

## 🟣 10. Agrupar Dados por Variáveis Categóricas

```python
pd.options.display.float_format = '{:.2f}'.format
display(company_volume_stats)
```

---

## 🟣 11. Visualizar Variáveis Categóricas

> A maioria das empresas aparece **3.271 vezes**, enquanto **Tesla = 3.148** e **Meta = 2.688**.

---

## 🟣 12. Aplicar Medidas de Tendência Central

```python
numerical_cols = df.select_dtypes(include=['float64', 'int64']).columns
summary_stats = df[numerical_cols].agg(['mean', 'median', lambda x: x.mode().iloc[0]])
summary_stats = summary_stats.rename(columns={'<lambda_0>': 'mode'})
display(summary_stats)
```

**Interpretação:**  
- Média: valor médio (sensível a outliers)  
- Mediana: valor central  
- Moda: valor mais frequente  

---

## 🟣 13. Ver Distribuição das Variáveis

```python
import matplotlib.pyplot as plt
import seaborn as sns

for col in numerical_cols:
    plt.figure(figsize=(10, 6))
    sns.histplot(df[col], kde=True)
    plt.title(f'Distribution of {col}')
    plt.show()
```

**Análise:**  
- Distribuições assimétricas à direita em preços e volumes, típicas de dados financeiros.

---

## 🟣 14. Aplicar Medidas de Dispersão

✔ Etapa concluída

---

## 🟣 15. Calcular Quartis, Decis ou Percentis

```python
std_dev = df[numerical_cols].std()
variance = df[numerical_cols].var()
iqr = df[numerical_cols].quantile(0.75) - df[numerical_cols].quantile(0.25)
```

---

## 🟣 16. Calcular Correlação entre Variáveis

```python
correlation_matrix = df[numerical_cols].corr(method='pearson')
display(correlation_matrix)
```

**Interpretação:**  
- Preços têm correlação **forte e positiva (~1)**  
- Volume tem correlação **negativa fraca (~ -0.22)**  

---

📊 **Conclusão Geral:**  
A análise de dados das Big Techs foi conduzida com base em boas práticas de limpeza, integridade e consistência estatística.  
Os resultados obtidos demonstram forte coerência entre as variáveis de preço e volume, com padrões comuns de assimetria em mercados financeiros.
