### **🚀 MVP Churn Explorer -- Análise de Churn e Cenários Preditivos sobre a Base de Clientes de um ISP**

Desenvolvido por: Marcelo Santos Araujo
Matrícula: 4052024002227
Data: 28/08/2025

### **💔 O Churn: A Dor da Despedida e o Cuidado com o Cliente**

Para uma empresa de telecomunicações, cada cliente é um relacionamento. Quando um cliente cancela seu serviço (o que chamamos de "churn"), é como uma "despedida" dolorosa. O churn é um indicador temido por seu **impacto financeiro direto**, a **erosão da base de clientes** e por ser um **sinal de alerta** sobre a saúde da empresa.

O objetivo principal deste projeto é **identificar proativamente os clientes em maior risco de churn** antes que eles decidam cancelar. Ao prever quem está propenso a "se despedir", a empresa pode agir a tempo com estratégias de retenção personalizadas.

Este é um problema de **classificação supervisionada**, onde o modelo aprende a prever se um cliente irá permanecer (`0`) ou dar churn (`1`) com base em dados históricos.

### **🎯 As Perguntas que Guiam Nossa Análise**

* A probabilidade de churn pode ser prevista com base em dados demográficos, comportamento de uso e histórico de cobrança?
* Qual a correlação entre o tempo de relacionamento e a probabilidade de churn?
* Como a **Satisfação do Cliente** e as **discrepâncias de cobrança** influenciam a decisão de cancelar?

### **📊 A Base de Dados: Nossos Clientes "Digitais"**

O coração da nossa análise é o dataset **Telco Customer Churn**, um conjunto de dados de clientes de serviços de telecomunicações obtido da Kaggle.

* **Fonte**: [Kaggle](https://www.kaggle.com/datasets/alfathterry/telco-customer-churn-11-1-3)
* **Link da Base de Dados**: `https://raw.githubusercontent.com/SpeedofLight007/churn-explorer/main/telco_dataset.csv`
* **Tamanho**: 7.043 amostras, com 1.869 casos de churn e 5.174 de não churn.
* **Variável Alvo**: `Churn Label` (Sim/Não).

### **Nossas Features Criadas (Engenharia de Atributos)**

Para aprofundar a análise, criamos novas features que complementam os dados originais:

* **`EffectiveMonthlyCharge`**: Custo mensal real do cliente, considerando o total pago ao longo do tempo.
* **`BillingDiscrepancy`**: A diferença entre a cobrança mensal atual e o `EffectiveMonthlyCharge`, indicando possíveis cobranças extras inesperadas.
* **`HasUnexpectedExtraCharge`**: Um indicador binário que sinaliza se houve uma discrepância positiva na cobrança.
* **`DiscrepancySeverity`**: A "gravidade" da discrepância de cobrança, ponderada pelo tempo de contrato, destacando seu impacto em clientes mais novos.

### **🧠 Como Nosso "Detetive de Churn" Funciona (Metodologia)**

Nosso projeto utiliza um modelo de **Classificação Supervisionada** para prever o churn.

1.  **Reprodutibilidade**: Usamos uma `seed` global (`SEED = 42`) para garantir que os resultados possam ser replicados por qualquer pessoa.
2.  **Pipeline de Pré-processamento**: As etapas de tratamento dos dados (como imputação e codificação de variáveis) são encapsuladas em um `Pipeline`, o que garante reprodutibilidade e previne vazamento de dados.
3.  **Modelagem**: Começamos com um **modelo baseline** simples (`DummyClassifier`) como ponto de partida. Em seguida, treinamos modelos candidatos mais sofisticados, como **Regressão Logística** e **Random Forest**.
4.  **Otimização**: O modelo **Random Forest** foi otimizado com `RandomizedSearchCV` para encontrar os melhores hiperparâmetros.

### **📈 Análise e Resultados**

O modelo **Random Forest** se destacou, superando o baseline e o modelo de Regressão Logística. Após a otimização de hiperparâmetros, ele alcançou um **score de 0.9164 (F1-weighted)**.

A **análise de importância de features** revelou os principais fatores que influenciam o churn:

1.  **`Churn Score`**: De longe, o preditor mais relevante, mostrando que a pontuação interna da empresa é um forte indicador.
2.  **`Contract_Month-to-Month`**: O tipo de contrato de curto prazo foi o segundo fator mais importante, confirmando a alta vulnerabilidade desses clientes.
3.  **`Contract_Two Year`** e **`Contract_One Year`**: O tempo de contrato também se mostrou crucial, com contratos mais longos indicando maior estabilidade.

A análise de outras visualizações, como o `heatmap` de correlação, mostrou que o **`Tenure in Months`** (tempo de permanência) tem uma correlação negativa com o churn, indicando que quanto mais tempo o cliente fica, menor a chance de ele sair.

### **✅ Conclusões e Próximos Passos**

Este MVP de churn demonstrou que é possível prever com alta acurácia quais clientes estão em risco de cancelar, com base em um conjunto de dados tabulares. O modelo **Random Forest** se mostrou o mais eficaz para a tarefa.

Para evoluir este projeto, sugerimos os seguintes passos:

* **Explorar Engenharia de Atributos Adicional**: Incluir as novas features criadas no pipeline de pré-processamento.
* **Testar Outros Modelos**: Avaliar outros algoritmos robustos como Gradient Boosting (XGBoost, LightGBM).
* **Análise de Erros Detalhada**: Investigar os casos de falsos positivos e falsos negativos para refinar o modelo.
* **Interpretabilidade**: Explorar técnicas como SHAP para entender as contribuições individuais das features para as previsões.
