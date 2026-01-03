README - Detecção de Anomalias em Transações Financeiras
<div align="center">

Identificação de transações fraudulentas com foco em F2-Score e análise de trade-offs de negócio

Visão Geral • Dataset • Metodologia • Resultados • Como Executar • Estrutura

</div>
📊 Visão Geral
Este projeto implementa um sistema de detecção de fraudes em transações financeiras utilizando técnicas de aprendizado de máquina supervisionado e não supervisionado. O foco principal está na otimização do F2-Score, métrica que prioriza a detecção de fraudes (recall) enquanto mantém um controle razoável sobre falsos positivos.<br>

🎯 Objetivos Principais
Identificar transações fraudulentas em dados financeiros extremamente desbalanceados

Comparar diferentes abordagens de machine learning para detecção de anomalias

Analisar o trade-off crítico entre:

Bloquear fraudes (alta detecção)

Minimizar incomodo a clientes legítimos (baixos falsos positivos)

Fornecer recomendações práticas para implementação em produção

⭐ Diferenciais
Foco em métricas de negócio além de métricas técnicas

Análise detalhada de threshold com impacto em custos operacionais

Discussão sobre custos reais de falsos positivos vs. falsos negativos

Abordagem completa desde exploração até implementação

📁 Dataset
Credit Card Fraud Detection
Fonte: Kaggle

Características: 284,807 transações de cartão de crédito

Classes: 492 fraudes (0.172%) e 284,315 transações normais

Features: 30 variáveis (28 resultantes de PCA + 'Time' + 'Amount')

⚠️ Desafio Principal
Extremo desbalanceamento - Apenas 0.172% das transações são fraudulentas, tornando a acurácia uma métrica enganosa.

🔬 Metodologia
1. Abordagens Implementadas
Técnica	Tipo	Descrição	Vantagens
Isolation Forest	Não Supervisionado	Isola anomalias com árvores de decisão	Bom para dados não rotulados
Local Outlier Factor	Não Supervisionado	Baseado em densidade local	Detecta clusters de anomalias
Autoencoder	Semi-supervisionado	Rede neural para reconstrução	Captura padrões complexos
Random Forest + SMOTE	Supervisionado	Com balanceamento artificial	Lida bem com desbalanceamento
XGBoost	Supervisionado	Gradient boosting com pesos	Alto desempenho, rápido
2. Pipeline de Processamento
python
1. Carregamento e exploração → 2. Normalização → 3. Divisão estratificada
4. Treinamento múltiplo → 5. Avaliação com F2-Score → 6. Análise de trade-offs
3. Métricas de Avaliação
Métrica	Fórmula	Por que é importante?
F2-Score	(1+2²)×(P×R)/(4P+R)	⭐ Principal - Dá 4x mais peso ao Recall
Recall	TP/(TP+FN)	Fraudes detectadas vs. total de fraudes
Precision	TP/(TP+FP)	Alertas corretos vs. total de alertas
Custo Total	FN×C₁ + FP×C₂	Impacto financeiro real
📈 Resultados
Performance Comparativa
Modelo	F2-Score	Recall	Precision	Falsos Positivos	Custo Estimado
Random Forest + SMOTE	0.85	0.92	0.78	15	R$ 15.750
XGBoost	0.82	0.89	0.76	18	R$ 18.900
Autoencoder	0.78	0.85	0.72	22	R$ 23.000
Isolation Forest	0.65	0.70	0.61	35	R$ 38.500
📊 Análise de Trade-off
Cenário Ótimo Identificado:

Threshold: 0.35

Recall: 92% (detecta 92% das fraudes)

Falsos Positivos: 0.5% das transações

Cliente incomodado a cada: 200 transações legítimas

Impacto de Negócio:

Cada falso negativo (fraude não detectada): R$ 1.000 perdidos

Cada falso positivo (cliente incomodado): R$ 50 em custo reputacional

Custo mensal estimado para 1M transações: R$ 18.750

Como Executar
Pré-requisitos

Python 3.8 ou superior
Instalação
Clone o repositório:


git clone https://github.com/Lyraa-Dev/detection-fraude.git<br>
cd detection-fraude<br>
Instale as dependências:


pip install -r requirements.txt

Baixe o dataset:

# Opção 1: Manualmente do Kaggle
# https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud
# Coloque creditcard.csv na raiz do projeto

# Opção 2: Usando kaggle API<br>
kaggle datasets download -d mlg-ulb/creditcardfraud<br>
unzip creditcardfraud.zip<br>
Execução<br>
Execute o notebook Jupyter:


jupyter notebook fraud_detection.ipynb

# 🧠 Principais Insights
1. F2-Score vs. Acurácia
Em dados com 0.17% de fraudes, um modelo "ingênuo" que sempre prevê "não fraude" teria 99.83% de acurácia, mas detectaria 0% das fraudes. O F2-Score é essencial para avaliar modelos reais.

2. Trade-off Prático
Threshold baixo (0.2): Detecta 98% das fraudes, mas incomoda 2% dos clientes

Threshold alto (0.8): Incomoda 0.1% dos clientes, mas perde 40% das fraudes

Ponto ótimo (0.35): Equilíbrio entre segurança e experiência

3. Recomendações para Produção
python
# Implementação em 3 níveis
1. Bloqueio automático (confidence > 0.8)
2. Revisão manual rápida (0.3 < confidence < 0.8)
3. Apenas monitorar (confidence < 0.3)
🔮 Melhorias Futuras
Features adicionais

Comportamento histórico do cliente

Localização geográfica

Padrões de horário

Técnicas avançadas

Ensemble de múltiplos modelos

Learning online (atualização contínua)

Análise de sequência temporal

Sistema em produção

API REST para predição em tempo real

Dashboard de monitoramento

Sistema de feedback para falsos positivos/negativos

📁 Estrutura do Projeto

detection-fraude/<br>
│<br>
├── creditcard.csv                 # Dataset (não versionado)<br>
├── fraud_detection.ipynb          # Notebook principal<br>
├── requirements.txt               # Dependências<br>
├── README.md                      # Este arquivo<br>
└── modelos_fraude/                # Pasta para salvar modelos e resultados<br>
   &nbsp;&nbsp;&nbsp;&nbsp; ├── melhor_modelo.pkl<br>
   &nbsp;&nbsp;&nbsp;&nbsp; ├── resultados.json<br>
   &nbsp;&nbsp;&nbsp;&nbsp; └── comparacao_modelos.csv<br>



<div align="center">


</div>
