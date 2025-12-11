# Objetivo do MVP – Pipeline de Dados para Manutenção Preditiva com SCANIA Component X

## 1. Contextualização

A Indústria 4.0 trouxe uma forte digitalização do chão de fábrica e da operação de frotas, com grande volume de dados coletados continuamente por sensores, sistemas embarcados e plataformas de monitoramento. Esses dados permitem a construção de soluções de **manutenção preditiva**, capazes de identificar padrões de degradação de componentes antes da falha e, assim, reduzir paradas não planejadas, custos de reparo e riscos operacionais.

O SCANIA Component X Dataset reúne dados reais de operação de caminhões pesados, contendo:

- Leituras operacionais multivariadas, em série temporal, de diversos sensores e contadores;
- Informações de tempo até o evento de reparo (*time-to-event*) do componente X;
- Especificações dos veículos (diferentes configurações e características);
- Rótulos que indicam quão próximo o veículo está de necessitar reparo do componente X, em janelas de tempo discretizadas.

Esse cenário é representativo de um problema típico de Indústria 4.0, no qual há grande volume de dados, variáveis anonimizadas por questões de confidencialidade industrial e a necessidade de transformar dados brutos em informações úteis para tomada de decisão.

---

## 2. Problema de negócio

Do ponto de vista de negócio, uma montadora ou operadora de frota precisa responder perguntas como:

- Em que momento um componente tende a falhar ou exigir reparo?
- Quais tipos de veículo ou configurações estão mais expostos a falhas precoces?
- Há padrões operacionais que indicam maior risco para o componente X?

Falhas inesperadas do componente X podem levar a:

- Paradas não planejadas de caminhões;
- Atrasos em entregas e perda de confiabilidade;
- Aumento de custos de manutenção corretiva e logística;
- Potenciais riscos de segurança.

O problema de negócio central deste MVP é, portanto:

> **Compreender como o componente X de caminhões pesados se degrada ao longo do tempo, identificando perfis de veículos e padrões operacionais mais associados à proximidade de falha, de forma a subsidiar decisões de manutenção mais inteligentes.**

---

## 3. Objetivo geral

O objetivo geral deste MVP é:

> **Construir um pipeline de dados em nuvem, utilizando a plataforma Databricks Community Edition, para ingestão, modelagem, carga e análise do SCANIA Component X Dataset, com foco em manutenção preditiva de caminhões pesados. O pipeline deverá transformar os dados brutos em uma camada analítica estruturada (esquema estrela) que permita analisar padrões de degradação do componente X, perfis de risco por especificação de veículo e comportamento operacional ao longo do tempo.**

---

## 4. Objetivos específicos / Perguntas de negócio

A partir do objetivo geral, este trabalho se propõe a responder, na medida do possível, as seguintes perguntas de negócio:

1. **Distribuição do tempo até reparo do componente X**

   - Como se distribui o *tempo até reparo* (time-to-event) do componente X na frota?
   - Qual é a mediana, média e variabilidade desse tempo?
   - Qual a proporção de veículos que chegam ao fim do estudo sem reparo registrado do componente X?

2. **Especificações de veículo mais críticas**

   - Certas combinações de especificações dos caminhões (por exemplo, diferentes valores de `spec_0`, `spec_1` e demais atributos de configuração) estão associadas a **menor tempo até reparo** do componente X?
   - É possível identificar grupos de especificações com comportamento claramente mais crítico (degradação mais rápida)?

3. **Comportamento temporal da degradação**

   - À medida que o componente X se aproxima do reparo/falha, como evoluem algumas leituras operacionais (sensores, contadores, variáveis de uso)?
   - Existem padrões temporais característicos nas séries de leituras em janelas mais próximas da falha em comparação com janelas mais distantes?

4. **Distribuição das classes de proximidade de falha (0–4)**

   - Como se distribuem as classes de proximidade da falha (0, 1, 2, 3, 4) nos conjuntos de validação e teste?
   - Há desbalanceamento importante (por exemplo, muitas instâncias em classe 0 e poucas em classe 4)? Como isso impacta a interpretação dos dados?

5. **Perfis operacionais de maior risco**

   - É possível caracterizar perfis operacionais de maior risco para o componente X, combinando:
     - especificações de veículo;
     - tempo até reparo;
     - classes de proximidade (0–4);
     - e padrões nas leituras operacionais?
   - Esses perfis podem ser usados como base para políticas de monitoramento e agendamento de manutenção mais inteligentes do que a manutenção puramente corretiva?

> Observação importante:  
> Não é obrigatório que todas as perguntas sejam completamente respondidas. Caso alguma delas não possa ser respondida com o nível de detalhe desejado, essa limitação será discutida na seção de autoavaliação, mantendo este documento de objetivo **intacto**, conforme orientação da disciplina.

---

## 5. Justificativa da escolha do dataset

O SCANIA Component X Dataset foi escolhido por apresentar características alinhadas aos objetivos do trabalho:

- **Relevância para Indústria 4.0**: dados reais de uma frota de caminhões, em contexto de monitoramento e manutenção preditiva;
- **Volume de dados adequado**: tamanho suficientemente grande para justificar o uso de uma plataforma de processamento distribuído (Databricks/Spark), mas ainda dentro de limites viáveis para a edição Community;
- **Estrutura rica**: presença de múltiplas tabelas (leituras operacionais, especificações, time-to-event, rótulos de classes), permitindo a construção de um modelo analítico em esquema estrela;
- **Anonimização realista**: variáveis operacionais anonimizadas, refletindo um cenário real de dados industriais em que detalhes de processo e sensores são protegidos por confidencialidade.

Essas características tornam o dataset adequado para demonstrar, em um único MVP, a construção de um pipeline de dados completo, desde a ingestão até a análise de negócio, em um contexto atual e relevante de Indústria 4.0.

---

## 6. Escopo e limitações

Este MVP terá o seguinte escopo:

- **Incluído no escopo**
  - Construção de pipeline em nuvem (Databricks CE) com camadas Bronze, Silver e Gold;
  - Modelagem de dados analítica em **esquema estrela**;
  - Construção de um **Catálogo de Dados**, incluindo descrição e domínios observados das variáveis (mesmo quando anonimizadas);
  - Análise de qualidade dos dados;
  - Análises exploratórias e agregadas para responder, na medida do possível, às perguntas de negócio formuladas.

- **Fora do escopo deste MVP (pode ser sugerido como trabalho futuro)**
  - Desenvolvimento de modelos preditivos complexos (por exemplo, modelos de machine learning ou deep learning para previsão de falha);
  - Integração com dados externos (web scraping, dados de custo, dados de clima, etc.);
  - Implementação de arquiteturas de streaming em tempo real.

Essas limitações não diminuem o valor do MVP, mas reforçam seu foco: **construir e documentar um pipeline de dados robusto e bem modelado, capaz de gerar insights claros sobre degradação e risco de falha do componente X.**
