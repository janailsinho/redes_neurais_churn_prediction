# 📊 Previsão de Churn (Abandono) em Telecom

Repositório acadêmico desenvolvido para a disciplina de Redes Neurais (**IF702 - CIn/UFPE**) por **Caio Ferreira** e **José Janailson**. O projeto investiga, implementa e compara diferentes modelos de aprendizado de máquina e redes neurais profundas para prever a evasão de clientes (*churn*) em uma operadora de telecomunicações baseada em dados reais do Kaggle.

---

## 📋 Sumário
- [Visão Geral do Projeto](#-visão-geral-do-projeto)
- [Estrutura e Particionamento dos Dados](#-estrutura-e-particionamento-dos-dados)
- [Pré-processamento e Engenharia de Atributos](#-pré-processamento-e-engenharia-de-atributos)
- [Modelos Investigados](#-modelos-investigados)
- [Métricas de Desempenho](#-métricas-de-desempenho)
- [Resultados Comparativos](#-resultados-comparativos)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)

---

## 🚀 Visão Geral do Projeto
A evasão de clientes (*churn*) é um dos maiores desafios para empresas de telecomunicações. O objetivo deste projeto é construir, sintonizar (utilizando **Optuna**) e avaliar classificadores avançados capazes de identificar com precisão clientes com alta probabilidade de cancelamento de serviços, aplicando rigorosas técnicas para evitar vazamento de dados (*data leakage*).

---

## ⚙️ Estrutura e Particionamento dos Dados
- **Dataset:** ~7 mil registros com 19 variáveis independentes e a variável alvo binária (`Churn`: *Yes/No*).
- **Estratégia de Divisão e Balanceamento:**
  1. Separação inicial dos dados em duas classes (0: Não Churn, 1: Churn).
  2. Subdivisão de cada classe de forma estratificada e independente em **Treino (50%)**, **Validação (25%)** e **Teste (25%)**.
  3. Balanceamento via *repetitive oversampling* aplicado de forma isolada aos conjuntos de treinamento e validação para equiparar o volume de instâncias da classe minoritária.
  4. Reunião e aleatorização final dos blocos para formar os conjuntos definitivos, garantindo que **nenhuma informação do conjunto de teste interfira no treino/validação**.

---

## 🛠️ Pré-processamento e Engenharia de Atributos
- **Tratamento de Dados Ausentes e Inconsistências:** Remoção de registros nulos e conversão de colunas numéricas mal tipadas (ex: conversão de `TotalCharges` de *string* para numérico).
- **Codificação:** Conversão de atributos binários e nominais (como *gender* e binários em geral) para $1/0$, além de aplicação de *One-Hot Encoding* em variáveis categóricas de múltiplas classes (*InternetService*, *Contract*, *PaymentMethod*).
- **Criação de Novas Features:**
  - `num_services`: Agregação de múltiplos serviços contratados em uma única variável descritiva.
  - `AvgMonthlyCharge`: Média do valor pago mensalmente pelo cliente.
  - `is_new_customer`: Indicador binário para clientes com tempo de casa (*tenure*) igual ou inferior a 6 meses.
- **Escalonamento:** Normalização de atributos contínuos restrita estritamente ao conjunto de treinamento para impedir o vazamento de dados (*data leakage*).

---

## 🤖 Modelos Investigados
O estudo realizou uma ampla exploração de arquiteturas clássicas e modernas orientadas a dados tabulares:
1. **MLP (Multilayer Perceptron):** Investigação exaustiva de topologias (1 a 4 camadas ocultas), taxas de aprendizado, funções de ativação (*ReLU*, *Tanh*, *LeakyReLU*, *ELU*), otimizadores (*Adam*, *Nadam*, *RMSProp*, *SGD*, *LBFGS*), regularização L2 e *Dropout*, sintonizados via Optuna.
2. **XGBoost:** Gradient Boosting otimizado com foco na maximização do F1-Score e ajuste de pesos de classes (*scale_pos_weight*).
3. **STAB (Structured Transformer for Tabular Data):** Modelo baseado em atenção voltado para dados tabulares.
4. **TabPFN (Tabular Prior-Data Fitted Network):** Classificador baseado em *transformers* pré-treinados.
5. **Mitra (AutoGluon Tabular):** Framework automatizado para busca de arquiteturas baseadas em árvores de decisão otimizadas.
6. **KAN (Kolmogorov-Arnold Network):** Redes neurais baseadas em funções splines parametrizadas nas arestas.
7. **TabKANet:** Variação de arquitetura KAN adaptada a dados tabulares.

---

## 📐 Métricas de Desempenho
- **Estatística Kolmogorov-Smirnov (KS):** Principal métrica adotada no projeto para medir a separabilidade máxima entre as distribuições acumuladas da classe positiva e da classe negativa.
- **AUROC (Área sob a Curva ROC):** Capacidade discriminatória global do modelo.
- **Log Loss / Entropia Cruzada:** Avaliação probabilística do erro de predição.
- **Precision, Recall e F1-Score:** Detalhamento do desempenho preditivo por classe, com ênfase na classe minoritária (*Churn*).
- **Matriz de Confusão (TN, FP, FN, TP):** Análise detalhada dos acertos e erros de classificação.

---

## 📊 Resultados Comparativos (Conjunto de Teste)

A tabela a seguir resume o desempenho final obtido pelos principais modelos investigados no projeto:

| Parâmetros / Métrica | MLP (Melhor) | XGBoost | STAB | TabPFN | Mitra | KAN | TabKANet |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Precision** | 0.5197 | 0.8020 | 0.5120 | 0.7958 | 0.5300 | 0.5245 | 0.5200 |
| **Recall** | 0.7628 | 0.7384 | 0.8205 | 0.7491 | 0.6752 | 0.7991 | 0.8000 |
| **F1-Score** | 0.6182 | **0.7526** | 0.6305 | 0.7613 | 0.5974 | **0.6334** | 0.6300 |
| **Accuracy** | 0.7493 | 0.7400 | 0.7442 | 0.7500 | 0.7600 | 0.7500 | 0.7500 |
| **MSE** | 0.2507 | **0.1717** | 0.2558 | 0.2508 | 0.2451 | 0.2462 | 0.2497 |
| **Log Loss** | 0.4768 | 0.5104 | 0.5048 | 0.4869 | 0.4807 | **0.4986** | 0.4932 |
| **KS Statistic** | 0.5369 | **0.5421** | 0.5486 | 0.5328 | 0.5407 | **0.5443** | 0.5322 |
| **AUROC** | 0.8418 | **0.8420** | 0.8375 | 0.8416 | **0.8429** | 0.8421 | 0.8417 |

---

## 🛠️ Tecnologias Utilizadas
- **Linguagem:** Python
- **Redes Neurais e Deep Learning:** TensorFlow, Keras
- **Machine Learning Tradicional:** Scikit-Learn, XGBoost, AutoGluon
- **Otimização de Hiperparâmetros:** Optuna
- **Manipulação e Análise de Dados:** Pandas, NumPy
- **Visualização:** Matplotlib, Seaborn
