# Fluxo de Machine Learning para Previsão de Desempenho na Copa do Mundo

## Integrantes do Grupo
* **Jonas Evangelista dos Santos**
* **Pedro Henrique Adorno Malagutti**

## Instituição & Curso
* **Faculdade de Tecnologia de Jundiaí (Fatec Jundiaí)**
* **Curso Superior de Tecnologia em Ciência de Dados**
* **Disciplina:** Inteligência Computacional
* **Professor:** Me. Mateus Guilherme Fuini

---

## 1. O Problema e o Dataset
O objetivo deste projeto é construir um pipeline robusto de Machine Learning capaz de prever se uma seleção nacional vencerá uma determinada partida eliminatória de Copa do Mundo (problema de classificação binária), baseando-se em métricas históricas, demográficas e de desempenho recente. 

Para viabilizar o projeto, foi modelado e gerado um dataset exclusivo denominado `world_cup_analytics.csv` contendo **650 registros** de partidas históricas simuladas. O conjunto de dados foi intencionalmente estruturado com inconsistências típicas do mundo real para validar nossa capacidade de tratamento de dados:
* **Variáveis Categóricas:** `confederation` (UEFA, CONMEBOL, etc.) e `host_continent`.
* **Dados Ausentes:** Inseridos aleatoriamente nas colunas de idade média do elenco e média de gols.
* **Outliers:** Inseridos de forma intencional na métrica de público do estádio (`stadium_attendance_thousands`) e idade do elenco para simular falhas de digitação ou ruídos de coleta.

---

## 2. Abordagem Metodológica

### Análise Exploratória (EDA)
Realizamos o diagnóstico estatístico das distribuições utilizando histogramas, boxplots para detecção visual de outliers, scatterplots para dispersão de variáveis e um heatmap de correlação de Pearson para avaliar a colinearidade entre os atributos preditores.

### Engenharia de Atributos (Feature Engineering)
Criamos duas novas features de negócios com forte embasamento esportivo:
1. `squad_experience_ratio`: Razão entre títulos históricos e a idade média do elenco. Reflete o equilíbrio entre maturidade histórica e vigor físico.
2. `ranking_dominance_factor`: Interação matemática que pondera a posse de bola média com a força do Ranking FIFA da seleção.

### Prevenção de Data Leakage via Pipelines
A divisão entre treino e teste foi realizada antes de qualquer transformação de dados. Para mitigar o vazamento de dados (*data leakage*), o fluxo de imputação, codificação categórica (`OneHotEncoder`) e escalonamento (`StandardScaler`) foi encapsulado dentro de um `ColumnTransformer` integrado ao `Pipeline` principal do Scikit-learn. Dessa forma, os parâmetros estatísticos (como média e variância) são aprendidos exclusivamente no conjunto de treino.

### Validação e Hiperparâmetros
O modelo preditivo adotado foi a **Regressão Logística**. Para garantir a generalização do modelo, aplicamos a validação cruzada **K-Fold (5 folds)**. O ajuste fino dos hiperparâmetros (como o termo de regularização `C` e o tipo de penalidade `l1` ou `l2`) foi executado de forma exaustiva através do **GridSearchCV**.

---

## 3. Resultados Obtidos
* **Melhor Configuração de Hiperparâmetros:** `C: 0.1`, `penalty: 'l2'`, `solver: 'lbfgs'`.
* **Acurácia Média na Validação Cruzada:** ~74.5%
* **Métricas no Teste:** O modelo apresentou um comportamento equilibrado, demonstrando excelente capacidade de discriminação na Matriz de Confusão entre vitórias reais e derrotas/empates, comprovando a eficácia do tratamento de dados.

## 4. Estrutura do Repositório
* `README.md` - Descrição metodológica do projeto.
* `projeto_inteligencia_computacional.ipynb` - Notebook Jupyter contendo a implementação passo a passo com explicações detalhadas em formato Markdown.
