# Detecção de Anomalias em Transações Financeiras

> Este projeto foi desenvolvido durante um bootcamp da DIO, com o objetivo de aplicar conceitos de análise de dados e Machine Learning em um problema de detecção de fraudes em transações financeiras.

![Python](https://img.shields.io/badge/Python-3.11-blue?style=for-the-badge)
![Jupyter Notebook](https://img.shields.io/badge/Jupyter%20Notebook-Análise%20interativa-orange?style=for-the-badge)
![Pandas](https://img.shields.io/badge/Pandas-Manipulação%20de%20dados-purple?style=for-the-badge)
![Scikit--learn](https://img.shields.io/badge/Scikit--learn-Machine%20Learning-yellow?style=for-the-badge)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualização%20de%20dados-green?style=for-the-badge)

## Sobre o projeto

Este projeto foi desenvolvido com o objetivo de analisar transações financeiras e construir modelos capazes de identificar possíveis fraudes.

O principal desafio do problema está no forte desbalanceamento entre as classes: a quantidade de transações legítimas é muito maior do que a quantidade de transações fraudulentas. Por isso, a avaliação dos modelos não pode depender apenas da acurácia.

Ao longo do notebook, foram realizadas etapas de carregamento dos dados, análise inicial, preparação para modelagem, treinamento de modelos de classificação e comparação de métricas voltadas para a classe de fraude.

## Dataset utilizado

O conjunto de dados utilizado contém transações realizadas por cartões de crédito. Por questões de privacidade, a maioria das variáveis foi transformada por PCA e aparece no formato `V1`, `V2`, ..., `V28`.

As principais colunas utilizadas são:

| Coluna | Descrição |
|---|---|
| `Time` | Tempo decorrido desde a primeira transação registrada |
| `V1` a `V28` | Variáveis numéricas anonimizadas |
| `Amount` | Valor da transação |
| `Class` | Variável alvo: `0` para transação normal e `1` para fraude |

O dataset é carregado diretamente por URL no notebook, evitando que um arquivo grande precise ser armazenado no repositório.

## Como o projeto foi desenvolvido

O projeto foi organizado em etapas progressivas:

1. **Carregamento dos dados**  
   Leitura do arquivo CSV diretamente de uma URL pública.

2. **Compreensão inicial do dataset**  
   Análise de dimensões, colunas, tipos de dados, estatísticas descritivas e valores ausentes.

3. **Análise da variável alvo**  
   Verificação da distribuição entre transações normais e fraudulentas.

4. **Preparação dos dados**  
   Separação entre variáveis preditoras (`X`) e variável alvo (`y`), além da divisão entre treino e teste.

5. **Modelo baseline**  
   Treinamento de uma Regressão Logística como primeiro modelo de referência.

6. **Avaliação do modelo**  
   Uso de métricas como precision, recall, F1-score, ROC-AUC e Average Precision.

7. **Ajuste de threshold**  
   Redução do limiar de decisão da Regressão Logística para aumentar a identificação de fraudes.

8. **Modelo Random Forest**  
   Treinamento de um modelo baseado em árvores com `class_weight="balanced"` para lidar melhor com o desbalanceamento.

9. **Comparação final**  
   Organização dos resultados em uma tabela comparativa.

10. **Importância das variáveis**  
    Análise das variáveis mais relevantes para o modelo Random Forest.

## Métricas utilizadas

Como o dataset é altamente desbalanceado, a acurácia pode ser enganosa. Um modelo poderia acertar quase todas as transações apenas classificando a maioria como normal.

Por isso, foram priorizadas métricas mais adequadas para problemas de fraude:

| Métrica | Interpretação |
|---|---|
| Precision | Entre as transações classificadas como fraude, quantas realmente eram fraude |
| Recall | Entre todas as fraudes reais, quantas o modelo conseguiu identificar |
| F1-score | Equilíbrio entre precision e recall |
| ROC-AUC | Capacidade geral de separação entre as classes |
| Average Precision | Métrica útil para avaliar modelos em bases desbalanceadas |

Neste problema, o **recall da classe fraude** é especialmente importante, pois uma fraude não identificada representa uma ocorrência que passou despercebida pelo modelo.

## Resultados obtidos

A tabela abaixo compara os modelos testados, considerando apenas as métricas mais relevantes para a classe de fraude.

| Modelo | Precision (fraude) | Recall (fraude) | F1-score (fraude) | ROC-AUC | Average Precision |
|---|---:|---:|---:|---:|---:|
| Regressão Logística | 0.8505 | 0.6149 | 0.7137 | 0.9567 | 0.7080 |
| Regressão Logística - Threshold 0.3 | 0.7823 | 0.6554 | 0.7132 | 0.9567 | 0.7080 |
| Random Forest Balanceado | 0.3416 | 0.8378 | 0.4853 | 0.9693 | 0.6979 |

## Análise dos resultados

A Regressão Logística funcionou como um bom modelo de referência. Ela apresentou alta precision para a classe fraude, indicando que, quando o modelo classificava uma transação como fraude, geralmente estava correto. No entanto, seu recall foi mais limitado, deixando parte das fraudes reais sem identificação.

Ao reduzir o threshold da Regressão Logística para `0.3`, o recall aumentou de `0.6149` para `0.6554`. Isso significa que o modelo passou a identificar mais fraudes. Em contrapartida, a precision caiu de `0.8505` para `0.7823`, indicando aumento nos falsos positivos. Esse comportamento é esperado, pois reduzir o threshold torna o modelo menos conservador.

O Random Forest Balanceado apresentou o maior recall entre os modelos testados, alcançando `0.8378`. Isso significa que ele foi o modelo que mais conseguiu capturar fraudes reais. Porém, sua precision caiu para `0.3416`, mostrando que ele também classificou muitas transações normais como fraude.

Portanto, a escolha do melhor modelo depende do objetivo do negócio:

- se a prioridade for **evitar falsos alarmes**, a Regressão Logística é mais conservadora;
- se a prioridade for **capturar a maior quantidade possível de fraudes**, o Random Forest Balanceado é mais adequado;
- se o objetivo for buscar um meio-termo simples, o ajuste de threshold na Regressão Logística oferece uma alternativa interessante.

## Estrutura do repositório

```text
deteccao-de-fraudes-dio/
│
├── notebooks/
│   └── deteccao_anomalias.ipynb
│
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## Como executar o projeto

1. Clone o repositório:

```bash
git clone <url-do-repositorio>
cd deteccao-de-fraudes-dio
```

2. Crie e ative um ambiente virtual:

```bash
python -m venv .venv
```

No Windows:

```bash
.venv\Scripts\activate
```

3. Instale as dependências:

```bash
pip install -r requirements.txt
```

4. Abra o notebook no VS Code ou Jupyter Notebook:

```text
notebooks/deteccao_anomalias.ipynb
```

5. Execute as células em ordem.

## Próximos passos

Algumas melhorias possíveis para versões futuras do projeto:

- testar técnicas de balanceamento, como undersampling e SMOTE;
- avaliar modelos mais avançados, como XGBoost;
- realizar otimização de hiperparâmetros com GridSearch;
- aplicar técnicas de explicabilidade, como SHAP;
- salvar os principais gráficos em uma pasta `images/` para enriquecer o README.

## Conclusão

O projeto mostrou que, em problemas de fraude, a acurácia não é suficiente para avaliar a qualidade de um modelo. Como o conjunto de dados é desbalanceado, métricas como recall, precision, F1-score, ROC-AUC e Average Precision são mais importantes para entender o desempenho real.

A Regressão Logística apresentou um comportamento mais conservador, com maior precision. O ajuste de threshold permitiu aumentar a quantidade de fraudes detectadas sem alterar o treinamento do modelo. Já o Random Forest Balanceado obteve o maior recall, sendo mais eficiente para capturar fraudes reais, embora com maior quantidade de falsos positivos.

Assim, o projeto reforça a importância de escolher métricas de avaliação de acordo com o problema de negócio e demonstra um fluxo completo de análise, modelagem e avaliação para detecção de anomalias em transações financeiras.
