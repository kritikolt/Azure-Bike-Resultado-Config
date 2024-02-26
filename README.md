# Configuração de Modelo de Aprendizado Automático no Azure Machine Learning Studio

Este repositório contém instruções passo a passo para configurar um modelo de aprendizado automático no Azure Machine Learning Studio, visando prever aluguéis de bicicletas com base em dados históricos.

## Passo a Passo

### 1. Configuração Inicial

- Acesse o Azure Machine Learning Studio e vá para a página Automated ML (em Authoring).

### 2. Configurações Básicas

- Job name: mslearn-bike-automl
- New experiment name: mslearn-bike-rental
- Description: Automated machine learning for bike rental prediction
- Tags: nenhum

### 3. Configurações de Tarefa e Dados

- Selecione *task type* como Regression.
- Selecione o dataset e crie um novo dataset chamado bike-rentals com as seguintes configurações:
  - Data type: Tabular
  - Data source: Select From web files
  - Web URL: https://aka.ms/bike-rentals
  - Settings:
    - File format: Delimited
    - Delimiter: Comma
    - Encoding: UTF-8
    - Column headers: Only first file has headers
  - Schema: Inclua todas as colunas exceto Path

### 4. Configurações de Tarefa

- Task type: Regression
- Dataset: bike-rentals
- Target column: Rentals (integer)
- Configurações adicionais:
  - Primary metric: Normalized root mean squared error
  - Explain best model: Unselected
  - Use all supported models: Unselected
  - Allowed models: RandomForest e LightGBM
 - Limits:
    - Max trials: 3
    - Max concurrent trials: 3
    - Max nodes: 3
    - Metric score threshold: 0.085
    - Timeout: 15
    - Iteration timeout: 15
    - Enable early termination: Selected
- Validation and test:
    - Validation type: Train-validation split
    - Percentage of validation data: 10
    - Test dataset: None

### 5. Configuração de Computação

- Select compute type: Serverless
- Virtual machine type: CPU
- Virtual machine tier: Dedicated
- Virtual machine size: Standard_DS3_V2*
- Number of instances: 1

### 6. Submissão do Job de Treinamento

- Submeta o job de treinamento e aguarde a conclusão.

### 7. Revisão do Melhor Modelo

- Quando o job de automação de machine learning for concluído, revise o melhor modelo na aba *Overview*.

### 8. Implantação e Teste do Modelo

- No tab *Model*, selecione Deploy e use a opção *Web service* para implantar o modelo com as seguintes configurações:
  - Name: predict-rentals
  - Description: Predict cycle rentals
  - Compute type: Azure Container Instance
  - Enable authentication: Selected

### 9. Teste o serviço implantado

- No estúdio Azure Machine Learning, no menu esquerdo, selecione Endpoints e abra o predict-rentals na aba real-time endpoint.
- Na página do *real-time endpoint* de *predict-rentals*, visualize a aba Test.
- No *Input data to test endpoint*, substitua o modelo JSON pelos seguintes dados de entrada:
```
 {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }
```
- Clique no botão *Test*.
- Revise os resultados do teste, que incluem um número previsto.

Este README fornece uma visão geral dos passos necessários para configurar e testar um modelo de aprendizado automático para prever aluguéis de bicicletas no Azure Machine Learning Studio.
