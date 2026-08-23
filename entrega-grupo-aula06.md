# Entrega Aula 06 — Grupo 10

**Disciplina:** Cloud & Cognitive Environments — FIAP MBA AI Engineering & Multi-Agents
**Turma:** <EA2>
**Data de entrega:** <30/09/2026>

## Grupo

| # | Nome completo | GitHub | E-mail FIAP |
|---|---------------|--------|-------------|
| 1 | Nelson Antonio Louzada | https://github.com/nelsonlouzada | RM375148@fiap.com.br |
| 2 | Marcelo Luiz Pelegrini | https://github.com/mlpelegrini | rm373354@fiap.com.br |

# LAB 2 + Exercício - Pricing Calculator: TCO da QC

**AVISO:** Este laboratório vale como entrega final de FinOps!
Deve ser feito em grupos nas Breakout rooms ou posteriormente, a critério dos grupos.

**Objetivo:** Estimar o custo mensal da arquitetura completa da Quantum Commerce em escala real (não free tier). Este resultado entra no projeto integrado final (seção `finops/`).

---

## Cenário

Você é o arquiteto cloud apresentando à diretoria da QC. Estime o TCO mensal considerando os seguintes componentes:

| Componente | Volume mensal esperado |
| :--- | :--- |
| **Blob Storage** | 10 TB (5 TB Hot + 3 TB Cool + 2 TB Archive) - imagens + logs |
| **Azure SQL Database** | Hyperscale 4 vCores + 100 GB (não free para escala real) |
| **Cosmos DB** | 4.000 RU/s + 50 GB |
| **Azure AI Search** | Standard S1 (3 réplicas, 12 partitions) |
| **Function App** | Premium EP1 (sempre warm 5M req/mês) |
| **Azure AI Services** | Multi-service S0: 1M chamadas Language + 5.000h Speech + 500k chamadas Vision |
| **Azure ML** | Workspace + Compute B2s ocasional + 1 Online Endpoint Standard_DS3_v2 (24/7) |
| **Application Insights** | 5 GB de logs/mês |
| **Egress** | 500 GB/mês saindo para CDN externa |

---

## Passo a Passo da Atividade

### Passo 1 — Adicionar cada serviço
1. Em [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator), adicione um por um todos os 9 itens acima.
2. Configure `East US 2` como região para tudo (padrão da disciplina).
3. Preencha os volumes conforme a tabela do cenário.

**FEITO**

### Passo 2 — Calcular o total
1. Role até o final para obter o total mensal em USD.
2. Converta para BRL (cotação atual ~5,3).
3. Anote os 3 itens mais caros.

**FEITO**

Estimativa pague conforme uso
finops_estimativa-qc.xlsx -- Aba Estimativa v1
https://azure.com/e/1e102a27b541497097a5d2912a8eff58

Estimativa reserva 1 ano
finops_estimativa-qc.xlsx -- Aba Estimativa v2
https://azure.com/e/a0f965e2cc40474ab04c0fb9f022b776

Estimativa reserva 3 ano
finops_estimativa-qc.xlsx -- Aba Estimativa v3
https://azure.com/e/c44de8f1964f41cc970dfa8d2e9d30d5

Estimativa consolidada com os 3 cenários
finops_estimativa-qc.xlsx -- Estimativa QC - Consolidada

### Passo 3 — Otimização hipotética
Para cada item do top 3, proponha uma otimização:

| Item caro | Otimização proposta | Economia estimada |
| --- | --- | --- |
| **Azure AI Search (Standard S1)** | Reduzir a quantidade de Search Units (SUs) ajustando o número de réplicas/partições para a demanda real e utilizar autoscaling dinâmico ou a camada de preços adequada ao tamanho do índice. | **~60% a 75%** do valor (redução de US$ 8.830,08/mês para ~$2.200,00/mês) |
| **AI Services - Speech (Fala do Azure)** | Processar chamadas em lote (*Batch Transcription*) em vez de tempo real e aplicar amostragem/downsampling nos arquivos de áudio antes de enviar à API. | **~50% a 80%** (redução de até US$ 4.000,00/mês sobre os US$ 5.000,00 atuais) |
| **AI Services - Language (IA do Azure Idioma)** | Substituir chamadas nativas de Sentiment por **GPT-4o-mini** com prompt engenheirado eficiente e aplicar *caching* de requisições recorrentes. | **~40% a 70%** (redução significativa sobre os US$ 875,00/mês) |

---

### Passo 4 — Exportar
1. Clique em **"Export"** → Excel.
2. Salvar como `finops/estimativa-qc.xlsx` no repositório privado do grupo.
3. Clique em **"Share"** → copiar link.
4. Guardar o link no arquivo `finops/pricing-calculator-link.md` do projeto final.

**FEITO**

### Passo 5 — Comparar com Reserved Instances
Para os itens **24/7** (SQL, Search, Function Premium se mantido, ML Endpoint):
1. Reabrir a estimativa.
2. Em cada item 24/7, mudar para *1 year reserved* ou *3 year reserved*.
3. Comparar a economia obtida.

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

**Economia Líquida Anual em R$:** Ao optar por **3-Year Reserved Instances / Savings Plans** nos recursos de computação/BD contínuos (SQL, ML Endpoint e Functions), o custo mensal desses três serviços cai de **R$ 4.383,77** para **R$ 2.310,00**, gerando uma economia total de **R$ 2.073,77 / mês** (ou **R$ 24.885,24 / ano**).

---

**FEITO**

---

## Reflexão (escrever em `finops/analise-otimizacao.md` do projeto final)

* **a)** Qual o TCO mensal estimado em USD da arquitetura QC sem otimização?
* **b)** Qual o TCO otimizado (com suas 3 propostas aplicadas)?
* **c)** Qual a economia % com Reserved Instances de 1 ano?
* **d)** Como você apresentaria esses números ao CFO da QC (em 1 parágrafo)?
**FEITO**

> **Checkpoint L₂ + Exercício Final:** Você tem estimativa exportada (`.xlsx`) + análise de 3 otimizações + cenário Reserved?
