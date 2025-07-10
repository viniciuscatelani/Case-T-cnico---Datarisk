# Case-Técnico--Datarisk

# Modelo de Previsão de Inadimplência

Este projeto tem como objetivo desenvolver um modelo de aprendizado de máquina capaz de prever a inadimplência de clientes com base em informações cadastrais, transacionais e histórico de relacionamento.

## 📊 Descrição do Problema

Dada uma base de dados de clientes com múltiplas características (como localização, valor a pagar, porte da empresa, entre outras), o objetivo é prever a probabilidade de inadimplência de cada cliente. O modelo será treinado apenas com informações disponíveis no momento da **emissão** do documento, sem depender de dados futuros (como pagamento).

---

## 🧪 Abordagem

1. **Análise Exploratória de Dados (EDA)**  
   Foram identificados padrões relevantes entre as variáveis e a inadimplência. No entanto, variáveis diretamente relacionadas à data de pagamento foram **excluídas da modelagem** para evitar vazamento de informação, já que essas informações não estão disponíveis na base de teste.

2. **Pré-processamento**
   - Conversão e tratamento de variáveis categóricas.
   - Log-transformações para lidar com variáveis assimétricas.
   - Criação de novas features como `log_VALOR_A_PAGAR` e `DEFASAGEM_SAFRA_VENCIMENTO`.

3. **Modelagem**
   Três modelos foram testados e avaliados:
   - Regressão Logística
   - LightGBM
   - CatBoost (modelo final escolhido)

   As métricas utilizadas foram:
   - **AUC-ROC**: para medir a capacidade de discriminação entre adimplentes e inadimplentes.
   - **Log Loss**: para penalizar previsões com alta incerteza.

4. **Validação**
   A base foi dividida em treino e validação (holdout), e os modelos foram comparados com base no desempenho em ambas.

5. **Feature Importance**
   A importância das variáveis foi analisada a partir do CatBoost, destacando as mais relevantes para a decisão final do modelo.

---

## ✅ Resultados

| Modelo                | AUC-ROC (Validação) | Log Loss (Validação) |
|-----------------------|---------------------|------------------------|
| Regressão Logística   | 0.8755              | 0.4339                 |
| LightGBM              | 0.9443              | 0.2505                 |
| **CatBoost (final)**  | **0.9544**          | **0.2155**             |

---

## 🧬 Variáveis mais relevantes

De acordo com a análise de importância das variáveis no CatBoost, destacam-se:

- `CEP_2_DIG`
- `DDD`
- `MES`
- `log_VALOR_A_PAGAR`
- `DOMINIO_EMAIL`
- `SEGMENTO_INDUSTRIAL`
- `PORTE`

---

## 🛠️ Configuração do ambiente

Para replicar o ambiente de desenvolvimento, siga os passos abaixo (utilizando Conda):

```bash
# 1. Criar o ambiente
conda create -n <my_env> python=3.10 -y

# 2. Ativar o ambiente (Windows PowerShell)
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
conda activate <my_env>

# 3. Instalar as dependências
conda install --file requirements.txt -c conda-forge
```
---

## 🗂️ Arquivos importantes
- risco_credito.ipynb — desenvolvimento do projeto

- requirements.txt — lista de pacotes utilizados

- submissao_case.csv — arquivo de submissão com as previsões do modelo final

--- 

## 📌 Observações finais
- Variáveis com nulos na base de teste (DDD, FLAG_PF, etc.) foram preenchidas com a categoria "NULO" para garantir compatibilidade com o modelo.

- Todas as variáveis que dependem da data de pagamento foram descartadas para evitar data leakage.

- O CatBoost foi o modelo final escolhido por apresentar o melhor desempenho geral.

---

Este projeto foi desenvolvido como parte de um desafio de modelagem preditiva, com foco em boas práticas de ciência de dados, interpretação e responsabilidade na utilização de variáveis sensíveis.