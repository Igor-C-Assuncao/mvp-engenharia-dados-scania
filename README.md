# MVP – Engenharia de Dados (Databricks) | SCANIA Component X

Este repositório contém um MVP de **Engenharia de Dados na nuvem** usando **Databricks Free Edition**, com foco em **Indústria 4.0 / manutenção preditiva**.  
O projeto implementa um pipeline completo (busca → coleta/ingestão → modelagem → carga → análise), utilizando **arquitetura Medalhão (Bronze → Silver → Gold)** e **tabelas Delta Lake**, resultando em um **Esquema Estrela** e **dashboards** para responder perguntas de negócio.

---

## 🎯 Objetivo do MVP

Transformar dados brutos do **SCANIA Component X Dataset** em uma camada analítica estruturada (Esquema Estrela) para analisar:

1. **Distribuição do tempo até reparo (TTE)** do componente X  
2. **Especificações de veículo mais críticas** (por taxa de reparo e combinações de specs)  
3. **Comportamento temporal** de leituras operacionais conforme aproxima do evento  
4. **Distribuição das classes 0–4** (onde rótulos existem)  
5. **Perfis de risco** por combinações de especificações

> Observação importante: o dataset possui **anonimização** de variáveis operacionais; o catálogo descreve natureza/tipos/domínios observados sem inferir significado físico não documentado.

---

## 🧱 Arquitetura (Bronze → Silver → Gold)

- **Bronze**: ingestão dos CSVs brutos em tabelas Delta
- **Silver**: padronização de chaves, unificação e limpeza
- **Gold**: modelagem analítica em **Esquema Estrela**
  - `gold_dim_veiculo`
  - `gold_dim_tempo`
  - `gold_dim_classe_proximidade`
  - `gold_fato_snapshot`
  - `gold_fato_tempo_ate_evento`

---

## 📦 Dataset

Fonte oficial: https://researchdata.se/en/catalogue/dataset/2024-34/2

Arquivos utilizados (exemplos):
- `train_operational_readouts.csv` (~1.14 GB)
- `train_tte.csv`
- `train_specifications.csv`
- `validation_operational_readouts.csv`
- `validation_labels.csv`
- `validation_specifications.csv`
- `test_operational_readouts.csv`
- `test_labels.csv` (se aplicável ao pacote)
- `test_specifications.csv`

---

## ▶️ Como reproduzir (visão geral)

> **Pré-requisito:** Conta no Databricks Free Edition

1. **Criar Volume** no Unity Catalog (ex.: `scania_raw_data`)
2. **Upload** dos CSVs para o Volume
3. Executar os notebooks na ordem:
   1. `01_Ingestao_Bronze`
   2. `02_Tratamento_Silver`
   3. `03_Modelagem_Gold`
   4. `04_Analises_SQL`
4. Validar as tabelas no **Catalog** e gerar **dashboards** / visualizações

---

## 📒 Notebooks (com outputs em HTML)

Como o GitHub nem sempre preserva outputs de notebooks exportados do Databricks em `.ipynb`, os notebooks foram exportados em **HTML**:

- Notebook 01 — Ingestão Bronze: **Ver HTML**
- Notebook 02 — Tratamento Silver: **Ver HTML**
- Notebook 03 — Modelagem Gold: **Ver HTML**
- Notebook 04 — Análises SQL: **Ver HTML**

📌 Links diretos:
- 01: https://github.com/Igor-C-Assuncao/mvp-engenharia-dados-scania/blob/main/NotebooksComOutput/01_Ingestao_Bronze.html  
- 02: https://github.com/Igor-C-Assuncao/mvp-engenharia-dados-scania/blob/main/NotebooksComOutput/02_Tratamento_Silver.html  
- 03: https://github.com/Igor-C-Assuncao/mvp-engenharia-dados-scania/blob/main/NotebooksComOutput/03_Modelagem_Gold.html  
- 04: https://github.com/Igor-C-Assuncao/mvp-engenharia-dados-scania/blob/main/NotebooksComOutput/04_Analises_SQL.html  

---

## 📊 Dashboards e evidências

As evidências (prints) incluem:
- Tabelas no **Catalog** (Bronze/Silver/Gold)
- Dashboards:
  - Histograma do TTE
  - Taxa de reparo por `spec_0`
  - Heatmap `spec_1 × spec_2`
  - Distribuição de classes (onde disponível)
  - Evolução temporal de feature (ex.: `171_0`)
  - Perfis de risco por combinação de specs

---

## ✅ Qualidade de Dados (auditoria)

Além de checks de completude, domínio e integridade referencial, foi realizada auditoria de unicidade na camada **Silver** (`vehicle_id + time_step`), evidenciando casos duplicados/triplicados já na origem (antes da camada Gold), documentados no relatório.

---

## 📄 Relatório (PDF / Overleaf)

O relatório em LaTeX descreve:
- Objetivos e perguntas de negócio
- Implementação do pipeline (Bronze→Silver→Gold)
- Modelagem (Esquema Estrela)
- Catálogo de dados
- Qualidade dos dados
- Análises e dashboards
- Autoavaliação + trabalhos futuros

---

## 👤 Autor

Igor Cassimiro Assunção  
Ciência da Computação — UNICAP

---

## 📌 Licença
MIT
