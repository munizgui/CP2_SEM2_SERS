Integrantes: Gustavo Russo, Guilherme Muniz, Ryan Barreto, Fernando Lembo
RM: 569283, 569521, 574126, 570228

# Avaliação — APIs de Energia Renovável e Aprendizado de Máquina

Repositório desenvolvido para a avaliação prática integrando consultas a APIs públicas de dados abertos, pré-processamento de dados e modelagem preditiva utilizando Machine Learning em Python.

---

##  Visão Geral do Projeto

Este projeto aborda duas tarefas principais de aprendizado de máquina aplicadas ao setor de energia renovável:
1. **Classificação (ANEEL):** Classificar a fonte de geração de empreendimentos cadastrados na ANEEL (Solar, Eólica ou Hidráulica) com base em sua potência e localização geográfica.
2. **Regressão (Open-Meteo):** Estimar a radiação solar horizontal global em Petrolina (PE) com base em dados meteorológicos horários.

---

##  Estrutura do Repositório

- `CP2_SEM2_SERS.ipynb`: Notebook executável contendo todas as etapas de consulta às APIs, tratamento de dados, treinamento dos modelos e análises detalhadas.
- `aneel_classificacao_orange.csv`: Conjunto de dados tratado extraído da API da ANEEL.
- `meteo_regressao_orange.csv`: Conjunto de dados histórico extraído da API Open-Meteo para Petrolina (PE).
- `README.md`: Documentação oficial do projeto.

---

##  Tarefa 1 — Classificação de Fontes de Energia (ANEEL)

### Objetivo
A partir da potência outorgada e das coordenadas geográficas (latitude e longitude) de um empreendimento, prever se a sua fonte de geração pertence à categoria **Solar**, **Eólica** ou **Hidráulica** (agrupando UHE, PCH e CGH).

### Metodologia e Modelos Treinados
- **Divisão dos Dados:** Divisão estratificada de 80% para treino e 20% para teste (`random_state=42`).
- **Pré-processamento:** Padronização das variáveis com `StandardScaler` (ajustado exclusivamente no conjunto de treino).
- **Algoritmos Avaliados:**
  1. *Regressão Logística* (Baseline linear)
  2. *K-Nearest Neighbors (KNN)* (Abordagem baseada em proximidade espacial)
  3. *Random Forest Classifier* (Modelo de ensemble baseado em árvores)

### Resultados Comparativos (Métricas Macro)

| Modelo | Acurácia | Precision (Macro) | Recall (Macro) | F1-Score (Macro) |
| :--- | :---: | :---: | :---: | :---: |
| **Regressão Logística** | 78.96% | 0.8000 | 0.7799 | 0.7840 |
| **K-Nearest Neighbors (KNN)** | 95.41% | 0.9523 | 0.9549 | 0.9534 |
| **Random Forest** | **98.10%** | **0.9802** | **0.9817** | **0.9809** |

* **Destaque:** O **Random Forest** obteve o melhor desempenho preditivo geral (ultrapassando 98% de acurácia e F1-score), capturando eficientemente as relações não lineares espaciais e de potência.
* **Limitações Práticas:** Utilizar apenas coordenadas e potência outorgada não é suficiente para aplicações industriais reais, pois fatores regulatórios, sazonais, de disponibilidade primária de recursos e restrições de transmissão na rede elétrica exercem papéis determinantes.

---

##  Tarefa 2 — Regressão de Radiação Solar (Open-Meteo)

### Objetivo
Prever numericamente a radiação solar global horizontal média (`radiacao_w_m2`) em W/m² em Petrolina (PE), utilizando dados meteorológicos horários (temperatura, umidade, nuvens, vento e hora local) coletados entre **1º de abril e 30 de junho de 2025** (recorte das 7h às 17h).

### Metodologia e Modelos Treinados
- **Divisão Temporal:** Divisão mantendo a ordem cronológica estrita (80% iniciais para treino, 20% finais para teste, evitando o embaralhamento temporal)
- **Pré-processamento:** Escalonamento de features estritamente com base no conjunto de treino.
- **Algoritmos Avaliados:**
  1. *Regressão Linear*
  2. *SVR (Support Vector Regressor)*
  3. *Random Forest Regressor*

### Resultados Comparativos

| Modelo | MAE (W/m²) | MSE ((W/m²)²) | $R^2$ (Coeficiente de Determinação) |
| :--- | :---: | :---: | :---: |
| **Regressão Linear** | 145.20 | 30034.20 | 0.3598 |
| **SVR** | 181.08 | 47441.64 | -0.0112 |
| **Random Forest Regressor** | **66.80** | **7307.42** | **0.8442** |

* **Destaque:** O **Random Forest Regressor** apresentou superioridade expressiva, explicando cerca de **84% da variância dos dados ($R^2 = 0.844$)**, enquanto modelos lineares tradicionais e SVR sofreram com a forte não-linearidade das condições meteorológicas.
* **Variável Crítica:** A `hora` do dia atua como o eixo direcional astronômico base para a predição da radiação, enquanto temperatura, umidade e cobertura de nuvens modulam as variações locais.
* **Radiação vs. Geração Fotovoltaica:** Estimar a radiação em $W/m^2$ **não equivale** de forma direta à energia elétrica produzida (kWh) por um sistema fotovoltaico. Fatores como eficiência de conversão dos painéis, perdas por temperatura de operação dos módulos, perdas no inversor, sombreamento e acúmulo de poeira interferem diretamente na geração real.

---

##  Instruções para Execução

1. Clone o repositório em sua máquina ou abra-o diretamente no Google Colab:
   ```bash
   git clone [https://github.com/munizgui/CP2_SEM2_SERS.git](https://github.com/munizgui/CP2_SEM2_SERS.git)

Instale as dependências padrão necessárias (geralmente pré-inclusas em ambientes como Jupyter/Colab):
    pip install pandas numpy matplotlib seaborn scikit-learn

##  Fontes Oficiais de Dados
ANEEL — Sistema de Informações de Geração (SIGA)

Open-Meteo — Historical Weather API
