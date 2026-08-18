# Tech Challenge - Fase 2

## Classificação da Qualidade de Vinhos com Machine Learning

Projeto desenvolvido como parte do **Tech Challenge - Fase 2** da **Pós-Tech em Data Analytics da FIAP**.

O objetivo deste projeto é aplicar técnicas de **Análise Exploratória de Dados (EDA)** e **Machine Learning** para desenvolver modelos capazes de classificar a qualidade de vinhos a partir de suas características físico-químicas.

---

## Objetivo

Desenvolver modelos de classificação capazes de prever se um vinho possui **Alta Qualidade** ou **Baixa/Média Qualidade** com base em suas características físico-químicas.

Para o problema de classificação, a variável `quality` foi transformada em duas classes:

- **Alta Qualidade:** nota maior ou igual a 7
- **Baixa/Média Qualidade:** nota menor que 7

---

## Dataset

O projeto utiliza o dataset **WineQT**, que contém informações sobre características físico-químicas dos vinhos e suas respectivas avaliações de qualidade.

Entre as variáveis analisadas estão:

- Fixed Acidity
- Volatile Acidity
- Citric Acid
- Residual Sugar
- Chlorides
- Free Sulfur Dioxide
- Total Sulfur Dioxide
- Density
- pH
- Sulphates
- Alcohol
- Quality

A coluna `Id` foi removida da análise por não representar uma característica relevante para a classificação da qualidade do vinho.

---

## Análise Exploratória dos Dados

Durante a Análise Exploratória de Dados foram utilizadas diferentes técnicas de visualização e análise estatística, incluindo:

- Histogramas
- Boxplots
- Matriz de correlação (Heatmap)
- Pairplot
- Análise da distribuição das classes
- Análise de outliers

A análise mostrou que as variáveis **Alcohol, Volatile Acidity, Sulphates e Citric Acid** apresentam maior relação com a qualidade dos vinhos.

O teor alcoólico (`alcohol`) apresentou a maior correlação positiva com a qualidade:

**r = 0,49**

Já a acidez volátil (`volatile_acidity`) apresentou a maior correlação negativa:

**r = -0,41**

Apesar dessas relações, nenhuma variável apresentou correlação superior a 0,50 em magnitude com a qualidade, indicando que a classificação depende da combinação de múltiplas características físico-químicas.

Também foi identificado um **desbalanceamento entre as classes**:

- **Baixa/Média Qualidade:** 86,54%
- **Alta Qualidade:** 13,46%

Por esse motivo, a avaliação dos modelos não foi baseada apenas em acurácia. Também foram consideradas métricas como **Precision, Recall, F1-score, ROC-AUC e Matriz de Confusão**.

---

## Pré-processamento

As variáveis preditoras foram separadas da variável alvo e o conjunto de dados foi dividido em:

- **70% para treinamento**
- **30% para teste**

A divisão foi realizada de forma **estratificada**, preservando a proporção das classes nos conjuntos de treino e teste.

Optou-se por não aplicar técnicas de reamostragem, como **oversampling** ou **SMOTE**, mantendo a distribuição original dos dados.

Para o modelo **KNN**, foi aplicado o `StandardScaler`, pois o algoritmo utiliza medidas de distância e é sensível à escala das variáveis.

---

## Modelos de Machine Learning

Foram avaliados três algoritmos de classificação:

1. **K-Nearest Neighbors (KNN)**
2. **Random Forest Classifier**
3. **XGBoost**

### K-Nearest Neighbors - KNN

Foram testados diferentes valores para o hiperparâmetro `k`.

O valor selecionado foi:

**k = 5**

Resultados:

- Accuracy: **≈ 89%**
- Precision: **≈ 68%**
- Recall: **≈ 37%**
- F1-score: **≈ 48%**
- ROC-AUC: **0,85**

O KNN apresentou bom desempenho geral, porém demonstrou limitações na identificação da classe minoritária, correspondente aos vinhos de Alta Qualidade.

---

### Random Forest Classifier

O Random Forest foi treinado inicialmente com **100 árvores de decisão (`n_estimators=100`)** e `random_state=42`.

Resultados:

- Accuracy: **≈ 89%**
- Precision: **≈ 77%**
- Recall: **≈ 24%**
- F1-score: **≈ 37%**
- ROC-AUC: **0,92**

O modelo apresentou a **maior Precision** entre os algoritmos avaliados e excelente capacidade global de discriminação.

Entretanto, apresentou baixo Recall para a classe Alta Qualidade, deixando de identificar uma parcela significativa dos vinhos pertencentes à classe minoritária.

A análise de importância das variáveis destacou principalmente:

- Alcohol
- Volatile Acidity
- Sulphates
- Citric Acid

---

### XGBoost

O XGBoost foi configurado utilizando parâmetros voltados ao controle do aprendizado e ao tratamento do desbalanceamento entre as classes, incluindo:

- `n_estimators = 300`
- `learning_rate = 0.05`
- `max_depth = 3`
- `subsample = 0.8`
- `colsample_bytree = 0.8`
- `scale_pos_weight`
- `random_state = 42`

Resultados:

- Accuracy: **≈ 89%**
- Precision: **≈ 57%**
- Recall: **≈ 63%**
- F1-score: **≈ 60%**
- ROC-AUC: **0,92**

O XGBoost apresentou o **maior Recall e F1-score para a classe Alta Qualidade**, demonstrando maior capacidade de identificar corretamente os vinhos pertencentes à classe minoritária.

---

##Comparação dos Modelos

| Modelo | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| KNN | ≈ 89% | 68% | 37% | 48% | 0,85 |
| Random Forest | ≈ 89% | **77%** | 24% | 37% | **0,92** |
| XGBoost | ≈ 89% | 57% | **63%** | **60%** | **0,92** |

Embora os três modelos tenham apresentado acurácia semelhante, as demais métricas demonstraram diferenças importantes em relação à identificação da classe minoritária.

O **Random Forest** apresentou a maior Precision e excelente ROC-AUC, porém teve o menor Recall.

O **XGBoost**, por sua vez, apresentou o melhor equilíbrio entre as métricas, principalmente por alcançar o maior **Recall (63%)** e **F1-score (60%)**, mantendo ROC-AUC de **0,92**.

---

##Modelo Selecionado

Considerando o desbalanceamento do conjunto de dados e a importância de identificar corretamente os vinhos classificados como **Alta Qualidade**, o **XGBoost apresentou o melhor desempenho geral**.

Mesmo apresentando Precision inferior ao Random Forest, o modelo conseguiu reduzir significativamente os falsos negativos e identificar uma quantidade maior de vinhos pertencentes à classe minoritária.

Portanto, o **XGBoost foi considerado o modelo mais adequado para o problema analisado**, apresentando o melhor compromisso entre:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC

---

## Tecnologias e Bibliotecas Utilizadas

O projeto foi desenvolvido em **Python**, utilizando o **Google Colab**.

Principais bibliotecas:

- Pandas
- NumPy
- Matplotlib
- Seaborn
- Plotly
- Missingno
- Scikit-learn
- XGBoost

---

## Estrutura do Projeto

```text
├── README.md
├── requirements.txt
├── WineQT.csv
└── tech_challenge_fase2.ipynb
