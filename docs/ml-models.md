# Modelos de Machine Learning — Análise de Fluxo Turístico Sustentável em Angola

Este documento descreve os modelos de previsão e análise utilizados no estudo do fluxo turístico em Angola, com foco em identificar padrões de entrada e saída de turistas por província entre 2019 e 2022.  

Os dados analisados provêm dos ficheiros:
- `turismo_angola_2019_2022.csv`
- `turismo_angola_provincias_2019_2022.csv`

---

## 1. Modelos de Séries Temporais

### 🔹 ARIMA
- **Tipo:** Modelo estatístico autoregressivo integrado de médias móveis.  
- **Uso no projeto:** Prever o fluxo mensal de turistas por província, considerando tendências e variações históricas.  
- **Parâmetros principais:** p (autoregressão), d (diferença), q (média móvel).  
- **Vantagens:**  
  - Boa interpretação dos componentes temporais.  
  - Adequado para séries estacionárias e dados com padrão estável.  
- **Limitações:**  
  - Requer estacionarização prévia dos dados.  
  - Sensível a grandes variações sazonais.

### 🔹 Prophet
- **Tipo:** Modelo aditivo de séries temporais desenvolvido pela Meta (Facebook).  
- **Uso no projeto:** Identificar e prever tendências sazonais (ex.: épocas de alta e baixa turística).  
- **Componentes considerados:** tendência, sazonalidade anual e eventos específicos (feriados nacionais).  
- **Vantagens:**  
  - Robusto a dados ausentes.  
  - Captura facilmente sazonalidades complexas.  
- **Limitações:**  
  - Menor precisão em dados com variações abruptas e curtos períodos históricos.

---

## 2. Modelos de Regressão Tabular

### 🔹 Random Forest
- **Tipo:** Ensemble de árvores de decisão.  
- **Uso no projeto:** Estimar o número de turistas com base em variáveis socioeconómicas e regionais (ex.: PIB provincial, infraestrutura, número de eventos culturais, etc.).  
- **Vantagens:**  
  - Capta relações não lineares entre variáveis.  
  - Menos sensível a outliers.  
- **Limitações:**  
  - Menos interpretável.  
  - Pode exigir mais tempo de treino.

### 🔹 XGBoost
- **Tipo:** Técnica de *gradient boosting* baseada em árvores.  
- **Uso no projeto:** Regressão para prever fluxos turísticos, otimizando desempenho em comparação com Random Forest.  
- **Vantagens:**  
  - Alta performance em dados estruturados.  
  - Controlo de overfitting através de *regularization*.  
- **Limitações:**  
  - Requer ajuste cuidadoso de hiperparâmetros.  
  - Mais complexo de interpretar.

---

## 3. Métricas de Avaliação

| Métrica | Fórmula | Interpretação |
|----------|----------|----------------|
| **RMSE** (Root Mean Squared Error) | √(Σ(ŷ - y)² / n) | Penaliza erros grandes. Mede precisão geral do modelo. |
| **MAE** (Mean Absolute Error) | Σ|ŷ - y| / n | Média absoluta dos erros. Mostra o desvio médio das previsões. |
| **MAPE** (Mean Absolute Percentage Error) | (100/n) * Σ(|ŷ - y| / y) | Percentagem média de erro em relação aos valores reais. |

Essas métricas serão aplicadas tanto aos modelos de séries temporais quanto aos modelos tabulares, permitindo comparar o desempenho preditivo entre abordagens.

---

## 4. Plano de Validação

### 4.1 Rolling Forecast Origin
- O conjunto de treino expande-se progressivamente, e o conjunto de teste move-se no tempo.  
- Aplicado aos modelos **ARIMA** e **Prophet**.  
- Exemplo de iteração:
  - Treino: 2019–2020 → Teste: 2021  
  - Treino: 2019–2021 → Teste: 2022  

**Objectivo:** Simular a previsão futura com base em dados históricos sem violar a ordem temporal.

---

### 4.2 Cross-Validation Temporal
- Os dados são divididos em blocos cronológicos respeitando a sequência temporal.  
- Aplicado aos modelos **Random Forest** e **XGBoost** com *features* temporais (ex.: mês, estação, ano).  
- Cada divisão é usada como conjunto de validação sem embaralhar a ordem dos registos.  

**Objetivo:** Avaliar a capacidade do modelo de generalizar para períodos futuros mantendo a coerência temporal.

---

## 5. Próximos Passos

- **Ajustar hiperparâmetros** de cada modelo (p, d, q no ARIMA; parâmetros de sazonalidade no Prophet; número de árvores e *learning rate* nos modelos de regressão).  
- **Comparar desempenho** entre as abordagens (temporal vs. tabular).  
- **Selecionar o modelo com menor RMSE/MAE/MAPE** para previsão futura do fluxo turístico.  
- **Documentar resultados** no ficheiro `/docs/results.md`.

---

*Última atualização:* Outubro de 2025  
*Autor:* Avindo dos Santos e Leopoldo Francisco — Projeto Capstone: Análise de Fluxo Turístico Sustentável em Angola
