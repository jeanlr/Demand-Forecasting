# 📊 Demand Forecasting - Previsão de Demanda com Séries Temporais

![Python](https://img.shields.io/badge/Python-3.11%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![License](https://img.shields.io/badge/License-MIT-green)


Modelo de *Machine Learning* para previsão de demanda de vendas utilizando técnicas avançadas de séries temporais. Desenvolvido para auxiliar empresas na otimização de estoque, planejamento de produção e tomada de decisões estratégicas com base em previsões mais precisas.

---

## 🎯 Visão Geral

Este projeto implementa um **pipeline completo de previsão de demanda**, desde a análise exploratória dos dados até a construção, validação e geração de previsões com modelos preditivos.  

O objetivo é prever vendas futuras a partir de dados históricos, considerando:

- 📆 Sazonalidade  
- 📈 Tendências  
- 🔁 Comportamentos cíclicos  

---

## ✨ Funcionalidades

- 📊 **Análise Exploratória de Dados (EDA)**: visualização de padrões, tendências e sazonalidade  
- 🏗️ **Pré-processamento**: limpeza e transformação dos dados  
- 🧠 **Engenharia de Features**: criação de lags e variáveis temporais  
- 🤖 **Modelagem**: CatBoostRegressor aplicado a séries temporais via *MLForecast*  
- 📈 **Avaliação**: métricas de erro e análise de desempenho  
- 🔮 **Previsões**: geração de forecasts com intervalos de confiança  

---

## 📁 Estrutura do Repositório

```text
Demand-Forecasting/
│
├── model.ipynb        # Notebook principal com análise e modelagem
├── README.md         # Documentação do projeto
├── pyproject.toml    # Dependências do projeto
│
└── data/             # Pasta para os datasets
    └── train.csv     # Dados de treinamento
```

## ⚙️ Instalação

### Pré-requisitos
- Python 3.11 ou superior
- UV (gerenciador de pacotes Python)

### Passos de Instalação

1. Clone o repositório:
```bash
git clone https://github.com/jeanlr/Demand-Forecasting.git
cd Demand-Forecasting

uv init
uv sync
```

## 📊 Metodologia
1. Coleta e Pré-processamento dos Dados

    Os dados de vendas foram obtidos a partir do conjunto público Demand Forecasting do Kaggle.

    Os dados diários foram agregados em nível mensal, somando as vendas de todas as lojas e produtos para cada mês.

    A coluna de data foi convertida para o formato datetime e renomeada para ds (data) e y (valor das vendas), seguindo a convenção do Prophet e MLForecast.

    Foi adicionada uma coluna unique_id para identificar a série temporal única (nesse caso, apenas uma série agregada).

2. Divisão dos Dados

    Os dados foram divididos em conjunto de treino (de janeiro de 2013 a junho de 2017) e teste (de julho a dezembro de 2017).

    O objetivo é avaliar a performance do modelo em prever os últimos 6 meses de vendas.

3. Engenharia de Features

Foram criadas diversas features temporais e de lag para capturar padrões na série temporal:
Lags

    Lags mensais: [12, 9, 6, 3, 1] para capturar sazonalidade e tendências recentes.

Transformações de Lag

    Médias Móveis (Rolling Mean) com janelas: 3, 6, 9 e 12 meses.

    Desvio Padrão Móvel (Rolling Std) com janela de 3 meses.

    Máximo e Mínimo Móvel (Rolling Max/Min) com janela de 3 meses.

    Média Expansiva (Expanding Mean) para capturar tendência de longo prazo.

Features Temporais

    month: mês do ano (1–12)

    quarter: trimestre (1–4)

4. Modelagem

    Foi utilizado o framework MLForecast, que permite a criação de modelos de machine learning para séries temporais com suporte a múltiplas séries e features de lag.

    Modelo utilizado: CatBoostRegressor com 200 estimadores e random state fixo.

    Frequência: Mensal (freq='MS').

    Intervalos de Predição: Configurados com PredictionIntervals usando o método de distribuição conformal e 3 janelas de validação.

5. Treinamento

    O modelo foi treinado no conjunto de treino.

    Foram utilizadas as features de lag e temporais criadas automaticamente pelo MLForecast.

    O modelo também foi configurado para gerar intervalos de confiança para as previsões.

6. Avaliação

    A avaliação do modelo foi realizada comparando as previsões com os dados reais do conjunto de teste.

    Foram utilizadas métricas como:

        MAE (Erro Absoluto Médio)

        MAPE (Erro Percentual Absoluto Médio)

        MASE (Erro Absoluto Escalonado Médio)

        Cobertura dos Intervalos de Predição
