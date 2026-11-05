# 🚚 Previsão de Tempo de Entrega (ETA) em Operações de Delivery

> **Objetivo do Projeto:** Desenvolver um modelo de Machine Learning para prever o tempo exato de entrega de pedidos de food delivery, visando otimizar o roteamento da frota, reduzir o rompimento de SLAs e melhorar a experiência do cliente.

Este repositório documenta todo o processo de análise de dados, engenharia de variáveis e treinamento do algoritmo utilizando Python e dados reais de logística (DoorDash). Abaixo, detalho o passo a passo da construção do modelo.

---

## 🛠️ Passo a Passo do Desenvolvimento

### Passo 1: Importação e Carregamento dos Dados
O primeiro passo foi configurar o ambiente e importar a base de dados histórica contendo informações sobre os pedidos, horários e disponibilidade da frota.

![Imagem do Código 1: Importação do Pandas e Leitura do CSV](https://github.com/user-attachments/assets/fac238cb-61c4-4c74-bec0-505474378e7a)

**O que foi feito:**
* Importação da biblioteca `pandas` para manipulação de dados em formato tabular.
* Leitura do arquivo `historical_data.csv` para iniciar a exploração das variáveis operacionais.

### Passo 2: Limpeza de Dados (Data Cleaning) e Conversão de Horários
Dados brutos de logística frequentemente possuem ruídos ou informações faltantes. O foco aqui foi garantir a integridade da variável principal: o horário em que a entrega realmente aconteceu.

![Imagem do Código 2: Limpeza e datetime](https://github.com/user-attachments/assets/684be175-f1a0-4154-b47e-0eaf221fd677)

**O que foi feito:**
* **Remoção de falhas:** Exclusão de linhas onde o `actual_delivery_time` estava vazio (pedidos cancelados ou com erro de sistema).
* **Padronização temporal:** Conversão das colunas de texto (`created_at` e `actual_delivery_time`) para o formato `datetime`, permitindo cálculos matemáticos de tempo.

### Passo 3: Engenharia de Variáveis (Feature Engineering)
Para o algoritmo aprender, ele precisa de um "alvo" (target). Criei o KPI logístico central do projeto: o tempo total decorrido entre a criação do pedido e a entrega.

![Imagem do Código 3: Criação da variável alvo e tratamento de nulos](https://github.com/user-attachments/assets/4be483cf-2f1a-4449-94dd-bf2b49cf1687)

**O que foi feito:**
* **Criação do Target:** Subtração do horário de entrega pelo horário do pedido, convertendo o resultado final para **minutos** (`tempo_real_entrega_minutos`).
* **Tratamento de Frota:** Valores ausentes nas colunas de entregadores no turno (`total_onshift_dashers`) e ocupados (`total_busy_dashers`) foram preenchidos com `0`, assumindo que a falta de registro indica ausência de frota disponível no momento.

### Passo 4: Seleção de Features e Divisão da Base
Com os dados limpos, separei quais informações o modelo usaria para fazer a previsão e dividi a base para testar a precisão do algoritmo depois.

![Imagem do Código 4: Seleção de variáveis e Train Test Split](https://github.com/user-attachments/assets/e5211aa9-8ea6-44b0-978f-0a564f60be13)

**O que foi feito:**
* Seleção de variáveis preditivas (Features), como: subtotal do pedido, total de itens, entregadores disponíveis e estimativa de tempo de direção.
* Divisão da base usando `train_test_split`: **80%** dos dados foram separados para treinar o algoritmo e **20%** foram ocultados para testar se ele realmente aprendeu ou apenas decorou os resultados.

### Passo 5: Treinamento do Modelo (Random Forest)
A escolha do modelo caiu sobre o **Random Forest Regressor**, um algoritmo de aprendizado de máquina excelente para lidar com variáveis não-lineares, muito comuns em logística e roteamento.

![Imagem do Código 5: Treinamento e MAE](https://github.com/user-attachments/assets/88281183-8e84-47ed-952f-00d09a03bb68)

**O que foi feito:**
* Configuração do algoritmo com 50 árvores de decisão (`n_estimators=50`) para balancear tempo de processamento e precisão.
* **Avaliação de Desempenho:** Utilização da métrica **MAE (Erro Médio Absoluto)**. O modelo alcançou um erro médio de **15,8 minutos**, indicando uma previsão altamente utilizável para ajustes de painel e alinhamento de SLA com o cliente final.

### Passo 6: Exportação para o Power BI
O modelo matemático só gera valor se for acionável para o time de operações. 

![Imagem do Código 6: Exportação do CSV Final](https://github.com/user-attachments/assets/73adadad-bd94-447c-a5b6-b45278f54ad4)

**O que foi feito:**
* Unificação dos dados reais e das previsões feitas pela Inteligência Artificial em uma única tabela.
* Exportação dos resultados em `.csv` para alimentar um Dashboard logístico de acompanhamento de KPIs no Power BI.

---

## 📊 Impacto no Negócio

![Dashboard de KPIs Logísticos](https://github.com/user-attachments/assets/3883451b-7002-4f28-a19a-de3688a46262)

Com este modelo implementado, a coordenação logística consegue prever gargalos antes que eles ocorram, alocando entregadores extras em horários de pico previstos e ajustando o tempo informado ao cliente no aplicativo, reduzindo drasticamente o volume de chamados de suporte por atraso.

## 💻 Como Executar
Todo o projeto foi desenvolvido em nuvem utilizando o Google Colab. Para reproduzir o pipeline de dados:

1. Acesse o notebook completo clicando no botão abaixo:
   
<a href="https://colab.research.google.com/drive/1lPVmGOZexU-V3EFAOIVfXtpj-aAE79MI?usp=sharing"><img src="https://img.shields.io/badge/Abrir_Notebook_no-Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" alt="Abrir no Colab"></a>

2. Faça o upload do arquivo `.csv` do dataset.
3. Execute as células sequencialmente para replicar a limpeza e o treinamento do modelo.

## 👩🏻‍💻 Autora
**Mayara Andrade**  
Conecte-se comigo no [LinkedIn](https://www.linkedin.com/in/mayara-andrade-a279271b6) para trocarmos ideias sobre Logística, Dados e Inteligência Artificial.
