### Otimização hipotética
Top 3 caros:

| Item caro | Otimização proposta | Economia estimada |
| --- | --- | --- |
| **Azure AI Search (Standard S1)** | Reduzir a quantidade de Search Units (SUs) ajustando o número de réplicas/partições para a demanda real e utilizar autoscaling dinâmico ou a camada de preços adequada ao tamanho do índice. | **~60% a 75%** do valor (redução de US$ 8.830,08/mês para ~$2.200,00/mês) |
| **AI Services - Speech (Fala do Azure)** | Processar chamadas em lote (*Batch Transcription*) em vez de tempo real e aplicar amostragem/downsampling nos arquivos de áudio antes de enviar à API. | **~50% a 80%** (redução de até US$ 4.000,00/mês sobre os US$ 5.000,00 atuais) |
| **AI Services - Language (IA do Azure Idioma)** | Substituir chamadas nativas de Sentiment por **GPT-4o-mini** com prompt engenheirado eficiente e aplicar *caching* de requisições recorrentes. | **~40% a 70%** (redução significativa sobre os US$ 875,00/mês) |

### Comparar com Reserved Instances

### 1. Análise dos Itens 24/7

#### **A. Azure SQL Database (Hyperscale - 4 vCores Gen5)**

* **Sob Demanda (PAYG):** ~**R$ 2.775,25 / mês**
* **Reserva de 1 Ano:** ~**R$ 1.952,23 / mês** *(Economia de ~30%)*
* **Reserva de 3 Anos:** **R$ 1.361,42 / mês** *(Economia de ~51%)*
* **Análise:** A reserva de 3 anos gera uma economia de **R$ 1.413,83 / mês** (R$ 16.965,96 / ano) em relação ao modelo sob demanda.

#### **B. Azure ML Endpoint (Online Endpoint 24/7 - VM `Standard_DS3_v2`)**

* **Sob Demanda (PAYG):** **R$ 858,83 / mês**
* **Reserva de 1 Ano:** ~**R$ 532,50 / mês** *(Economia de ~38%)*
* **Reserva de 3 Anos:** ~**R$ 326,33 / mês** *(Economia de ~62%)*
* **Análise:** Ao comprometer o endpoint de inferência por 3 anos, o custo mensal cai **62%**, gerando uma economia de **R$ 532,50 / mês** por instância.

#### **C. Azure Functions (Plano Premium EP1 - 1 Instância *Always Warm*)**

* **Sob Demanda (PAYG):** **R$ 749,69 / mês**
* **Savings Plan (1 Ano ou 3 Anos):** ~**R$ 622,25 / mês** *(Economia de ~17%)*
* **Análise:** O plano Premium suporta o *Azure Savings Plan*, garantindo uma economia de **R$ 127,44 / mês**.

#### **D. Azure AI Search (Standard S1 - 36 Search Units)**

* **Sob Demanda (PAYG):** **R$ 45.364,09 / mês**
* **Análise de Reserva:** O Azure AI Search opera continuamente (24/7), porém não possui a modalidade clássica de *Reserved Instances* por nó como instâncias de computação. A redução de custo deve ser feita ajustando a topologia (redimensionando réplicas e partições).

---

### 2. Tabela Comparativa dos Recursos de Computação/BD 24/7 (BRL)

| Serviço 24/7 | Sob Demanda (PAYG) | Reserva 1 Ano | Reserva 3 Anos | Economia Max (%) |
| --- | --- | --- | --- | --- |
| **Azure SQL Database (4 vCores)** | R$ 2.775,25 | R$ 1.952,23 | **R$ 1.361,42** | **~51%** |
| **Azure ML Endpoint (`DS3_v2`)** | R$ 858,83 | R$ 532,50 | **R$ 326,33** | **~62%** |
| **Azure Functions Premium (EP1)** | R$ 749,69 | R$ 622,25 | **R$ 622,25** | **~17%** |
| **Total Combinado (Compute/BD)** | **R$ 4.383,77** | **R$ 3.106,98** | **R$ 2.310,00** | **~47,3%** |

---

### 3. Conclusão FinOps

1. **Economia Líquida Anual em R$:** Ao optar por **3-Year Reserved Instances / Savings Plans** nos recursos de computação/BD contínuos (SQL, ML Endpoint e Functions), o custo mensal desses três serviços cai de **R$ 4.383,77** para **R$ 2.310,00**, gerando uma economia total de **R$ 2.073,77 / mês** (ou **R$ 24.885,24 / ano**).
2. **Recomendação:** Contratar o compromisso de 3 anos para o banco de dados principal (SQL Hyperscale) e endpoints fixos do Azure ML, além de aplicar *Savings Plan* para o Function App.

---

### Reflexão e Análise FinOps — Quantum Commerce (QC)

#### **a) TCO mensal estimado em USD da arquitetura QC sem otimização**

O Total Cost of Ownership (TCO) mensal baseline sem otimizações é de **US$ 16.618,93 / mês** (aproximadamente **R$ 85.378,94 / mês** na cotação da planilha). Os custos de Inteligência Artificial e Pesquisa representam mais de 88% dessa carga total.

#### **b) TCO otimizado (com as 3 propostas aplicadas)**

Com a aplicação das três otimizações nos serviços *Top 3* (redimensionamento de Search Units no AI Search, transição para *Batch Transcription* no AI Speech e adoção do *GPT-4o-mini* com caching no AI Language), o TCO mensal cai para aproximadamente **US$ 7.000,37 / mês** (cerca de **R$ 35.963,80 / mês**), gerando uma economia bruta mensal de **US$ 9.618,56** (**~57,9% de redução do TCO total**).

#### **c) Economia % com Reserved Instances de 1 ano**

Focando nos recursos de banco de dados e computação que operam 24/7 (Azure SQL Database, ML Endpoint e Functions Premium), a migração do modelo *Pay-As-You-Go* para **Reserved Instances / Savings Plan de 1 ano** reduz os custos dessa camada específica de **US$ 853,30/mês** para **US$ 604,77/mês**, representando uma economia direta de **29,1%** sobre esses ativos contínuos.

---

#### **d) Apresentação Executiva para o CFO da Quantum Commerce**

> "A arquitetura inicial da Quantum Commerce está projetada em **US$ 16.618,93/mês**, mas apresenta um potencial imediato de redução de custos de **57,9%** com ajustes finos de engenharia e FinOps. A maior oportunidade reside no dimensionamento das APIs de IA e do Azure Search, onde a substituição de chamadas síncronas por processamento em lote e a adequação do índice reduzem a fatura em **US$ 9.618,56/mês**. Adicionalmente, o compromisso de **Reserved Instances (1 a 3 anos)** para nossos bancos de dados e instâncias 24/7 garante uma economia incremental de até **51%** na camada de infraestrutura fixa, reduzindo nosso TCO recorrente para o patamar sustentável de **~US$ 7.000/mês** sem qualquer prejuízo à performance da plataforma."
