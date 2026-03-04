# 📊 Telecom X: Previsão de Evasão de Clientes (Churn Prediction) - Parte 2

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine%20Learning-orange.svg)](https://scikit-learn.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-Gradient%20Boosting-green.svg)](https://xgboost.ai/)

## 📌 O Propósito da Análise
O objetivo principal deste projeto é **prever a evasão (churn) de clientes** da empresa de telecomunicações fictícia **Telecom X**, com base em variáveis comportamentais, demográficas e financeiras. A intenção é antecipar o cancelamento de serviços e fornecer à empresa uma ferramenta de inteligência preditiva, permitindo a criação de estratégias de retenção focadas e eficientes.

## ⚙️ Processo de Preparação dos Dados

Para garantir a qualidade dos modelos, os dados passaram por um rigoroso pipeline de pré-processamento:

1. **Classificação das Variáveis:**
* **Categóricas:** Gênero, Serviços de Internet (Fibra/DSL), Método de Pagamento, Tipo de Contrato, etc.
* **Numéricas:** Tempo de contrato (`tenure`), Mensalidade (`MonthlyCharges`), Encargos Totais (`TotalCharges`).


2. **Codificação e Normalização:**
* **One-Hot Encoding:** Aplicado às variáveis categóricas usando `drop='if_binary'` para evitar multicolinearidade perfeita em categorias de "Sim/Não".
* **Normalização (StandardScaler):** Aplicada às variáveis numéricas contínuas para que algoritmos sensíveis à escala (como Regressão Logística) performem adequadamente. *Nota: O ajuste (`fit`) do Scaler foi feito apenas nos dados de treino para evitar vazamento de dados (Data Leakage).*


3. **Separação de Treino e Teste:**
* Os dados foram divididos na proporção de **70% para treino e 30% para teste**.
* Utilizou-se o parâmetro `stratify=y` para garantir que a proporção de clientes que cancelaram (Churn) se mantivesse idêntica em ambos os conjuntos.



## 🧠 Justificativas de Modelagem

O problema de Churn é naturalmente desbalanceado (menos pessoas cancelam do que permanecem).

* **Escolha de Modelos:** Foram testados algoritmos de natureza diferente para comparação: **Regressão Logística** (foco em interpretabilidade), **Random Forest** (foco em lidar com não-linearidade) e **XGBoost** (foco em performance bruta).
* **Lidando com o Desbalanceamento:** Em vez de usar técnicas de amostragem como SMOTE, optou-se por aplicar pesos às classes (`class_weight='balanced'` e cálculo manual de `scale_pos_weight` no XGBoost). Isso força o modelo a penalizar mais severamente os erros cometidos na classe minoritária (Churn).
* **Métrica de Avaliação:** O foco principal foi a métrica de **Recall** (Revocação) para a classe 1, pois para o negócio é preferível ter alguns Falsos Positivos (oferecer desconto a quem não ia sair) do que Falsos Negativos (perder o cliente sem tentar intervir).

## 📊 Análise Exploratória de Dados (EDA) e Insights

Durante a fase exploratória, extraímos gráficos que direcionaram a modelagem:

* **Distribuição do Churn (Gráfico de Pizza):** Revelou o forte desbalanceamento da base de dados.
* **Boxplot de Mensalidades:** Clientes que cancelaram o serviço tendem a ter mensalidades (`MonthlyCharges`) visivelmente mais altas.
* **Histograma de Tempo de Cliente (`tenure`):** Demonstrou que a grande maioria das evasões ocorre nos primeiros meses de contrato. Clientes com mais de 2 anos de casa raramente cancelam.
* **Mapa de Calor (Correlação):** Mostrou a forte relação entre contratos do tipo "Month-to-month" e a decisão de cancelamento.

## 🚀 Como Executar o Projeto

1. **Clone este repositório:**
```bash
git clone [https://github.com/Enrico3115/telecomX-churn-prediction.git](https://github.com/Enrico3115/telecomX-churn-prediction.git)
cd telecomX-churn-prediction

```


2. **Crie e ative um ambiente virtual:**
```bash
# Windows
python -m venv venv
.\venv\Scripts\activate

```


3. **Instale as bibliotecas necessárias:**
```bash
pip install -r requirements.txt

```

4. **Execute o Jupyter Notebook:**
```bash
jupyter notebook notebooks/TelecomX_Parte2.ipynb
```
