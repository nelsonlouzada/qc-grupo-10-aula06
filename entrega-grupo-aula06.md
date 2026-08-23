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

### Passo 2 — Calcular o total
1. Role até o final para obter o total mensal em USD.
2. Converta para BRL (cotação atual ~5,3).
3. Anote os 3 itens mais caros.

### Passo 3 — Otimização hipotética
Para cada item do top 3, proponha uma otimização:

| Item caro | Otimização proposta | Economia estimada |
| :--- | :--- | :--- |
| | | |
| | | |
| | | |

*Exemplos comuns:*
* **AI Services Language:** trocar Sentiment por GPT-4o-mini (mais barato em escala se usado smart).
* **ML Endpoint 24/7:** scale-to-zero ou batch endpoint (drops 95% do custo).
* **Egress:** Azure Front Door reduz custos vs CDN externa.
* **Function Premium EP1:** ficar em Consumption Y1 se cold start aceitável (volta de $146/mês para $0).

---

### Passo 4 — Exportar
1. Clique em **"Export"** → Excel.
2. Salvar como `finops/estimativa-qc.xlsx` no repositório privado do grupo.
3. Clique em **"Share"** → copiar link.
4. Guardar o link no arquivo `finops/pricing-calculator-link.md` do projeto final.

### Passo 5 — Comparar com Reserved Instances
Para os itens **24/7** (SQL, Search, Function Premium se mantido, ML Endpoint):
1. Reabrir a estimativa.
2. Em cada item 24/7, mudar para *1 year reserved* ou *3 year reserved*.
3. Comparar a economia obtida.

---

## Reflexão (escrever em `finops/analise-otimizacao.md` do projeto final)

* **a)** Qual o TCO mensal estimado em USD da arquitetura QC sem otimização?
* **b)** Qual o TCO otimizado (com suas 3 propostas aplicadas)?
* **c)** Qual a economia % com Reserved Instances de 1 ano?
* **d)** Como você apresentaria esses números ao CFO da QC (em 1 parágrafo)?

> **Checkpoint L₂ + Exercício Final:** Você tem estimativa exportada (`.xlsx`) + análise de 3 otimizações + cenário Reserved?
