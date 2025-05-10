Integrantes

Andre Rovai        RM555848
Lancelot Chagas    RM554707

# 📘 README - Explicação das Mudanças e Refinamentos no Código do Colab

Link do colab: https://colab.research.google.com/drive/1jRnfc7cWyBFYIvCrf0WFjuRGRSWtnbuQ?usp=sharing

## 📂 Estrutura do Notebook

### 🔄 Mudança
Adotou-se a estrutura lógica do notebook de referência Case_Ifood_Colearning_Parte4.ipynb:  
*Carregamento ➝ Limpeza ➝ Engenharia de Atributos ➝ Divisão Treino/Validação ➝ Configuração PyCaret ➝ Comparação/Tuning/Seleção ➝ Avaliação ➝ Previsão Final*

### ✅ Por quê?
Melhora a organização, legibilidade e reprodutibilidade do processo de modelagem.

---

## 📥 Carregamento de Dados

### 🔄 Mudança
Carregamento explícito de data (2).csv (treino) e Xtest.csv (teste final).  
Verificação de erro adicionada caso os arquivos não estejam no ambiente.

### ✅ Por quê?
Separar dados de treino/teste é prática padrão. A verificação de erro auxilia no diagnóstico de problemas.

---

## 🧹 Limpeza e Pré-processamento

### 🔄 Remoção de Colunas
- Colunas irrelevantes (ex: ID) e constantes (Z_CostContact, Z_Revenue) foram removidas.

### 🔄 Tratamento de NaNs
- Linhas com Income ausente foram removidas (via dropna).

### 🔄 Correção de Marital_Status
- Agrupamento de valores raros ou inconsistentes (Alone, Absurd, YOLO) em Single.

### 🔄 Ajuste de Tipos
- Uso de convert_dtypes() e ajustes:
  - Dt_Customer → datetime  
  - Response → int  
  - Income → float

### ✅ Por quê?
Evita problemas com o PyCaret e garante consistência dos dados.

---

## 🛠️ Engenharia de Atributos

### 🔄 Cálculo de Age
- A idade foi derivada de Year_Birth, que foi removida.

### 🔄 Cálculo de Time_Customer
- Calculado em relação à *data máxima do dataset* (não today()).
- Dt_Customer removida.

### 🔄 Criação de Novas Features
- Children_Total = Kidhome + Teenhome  
- Total_Spending = soma de colunas Mnt*  
- Total_Purchases = soma de colunas *Purchases  
- Total_Acc_Cmp = soma de colunas AcceptedCmp1 a AcceptedCmp5  
- Income_Per_Capita = renda per capita (baseado em Marital_Status e Children_Total)

### ✅ Por quê?
Adiciona valor preditivo e refina os dados para melhor performance do modelo.

---

## 🔀 Divisão Treino/Validação

### 🔄 Mudança
- Divisão: 95% (df_treino) + 5% (df_validacao), mantendo df_validacao como *hold-out*.

### ✅ Por quê?
Evita vazamento de dados e permite avaliação realista.

---

## ⚙️ Setup PyCaret

### 🔄 Mudança
Configuração com:
- fix_imbalance=True  
- remove_outliers=True  
- normalize=True  
- Definição explícita de categorical_features e numeric_features

### ✅ Por quê?
Garante preparação adequada dos dados e boas práticas para modelagem robusta.

---

## ⚖️ Comparação de Modelos

### 🔄 Mudança
Uso de compare_models(sort='AUC') para validação cruzada.

### ✅ Por quê?
Identifica rapidamente os modelos com maior potencial preditivo (AUC).

---

## 🔧 Tuning de Hiperparâmetros

### 🔄 Mudança
Uso de tune_model para os modelos mais promissores (ex: LightGBM, XGBoost), otimizando por AUC.

### ✅ Por quê?
Maximiza o desempenho ao buscar os melhores hiperparâmetros.

---

## 🤝 Blending de Modelos (Opcional)

### 🔄 Mudança
Uso de blend_models para combinar previsões dos melhores modelos tunados.  
Lógica adicional para selecionar o melhor modelo final (blend ou individual).

### ✅ Por quê?
Técnicas de ensemble tendem a aumentar a robustez e a performance do modelo.

---

## ✅ Finalização e Validação

### 🔄 Mudança
Uso de finalize_model com df_treino, avaliação em df_validacao.

### 🔄 Métricas calculadas:
- AUC  
- Acurácia  
- Precisão  
- Recall  
- F1  
- Relatório de classificação

### ✅ Por quê?
Garante avaliação realista com dados nunca vistos durante o tuning.

---

## 🐛 Correção de Erro - prediction_score_1

### 🔄 Mudança
Uso de raw_score=True em predict_model (etapas 10 e 11).

### ✅ Por quê?
Garante que as probabilidades da classe positiva sejam incluídas para cálculo do AUC.

---

## 📊 Previsão no Xtest.csv

### 🔄 Mudança
- Aplicados os *mesmos passos* de pré-processamento e feature engineering do treino.
- Utilização do modelo final para previsão.

### ✅ Por quê?
Garante consistência nos dados de entrada do modelo, simulando aplicação real.

---

## 💬 Comentários e Clareza

### 🔄 Mudança
- Comentários explicativos adicionados ao código.
- Uso de títulos/subtítulos Markdown para organização.

### ✅ Por quê?
Facilita o entendimento do processo e melhora a experiência do usuário com o notebook.

---

## 🧠 Conclusão

O código refinado aplica:
- Estrutura sequencial e lógica
- Pré-processamento robusto
- Feature engineering estratégica
- Setup otimizado do PyCaret
- Avaliação realista com hold-out
- Correções que garantem métricas corretas

🔁 Tudo isso garante reprodutibilidade, melhor desempenho do modelo e aplicação segura em dados reais.
