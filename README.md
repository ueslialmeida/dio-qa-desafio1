# Desafio de Projeto: Engenharia de Qualidade de Software (QA)

[![QA Shift-Left](https://img.shields.io/badge/QA-Shift--Left%20Testing-blue.svg)](#)
[![Metodologia](https://img.shields.io/badge/Metodologia-BDD%20%7C%20IEEE%20829-green.svg)](#)
[![Documentação](https://img.shields.io/badge/Documentos-5%20PDFs%20Anexados-orange.svg)](#)
[![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-success.svg)](#)

Repositório dedicado à entrega do **Desafio de Qualidade de Software**, contendo o planejamento de testes, fluxo de trabalho ágil, ciclo de vida do bug, especificação de requisitos e suítes de testes para uma aplicação corporativa voltada para o setor automotivo.

---

## 🚗 Aplicação Conceitual: OficinaTech Pro

Para a elaboração de todos os artefatos teóricos e conceituais exigidos no desafio, foi adotada a aplicação fictícia **OficinaTech Pro**, um sistema de gestão integrado para oficinas mecânicas automotivas.

O foco central da modelagem de testes e requisitos é o **Módulo de Ordem de Serviço (O.S.)**, que controla:
* Check-in técnico e triagem de avarias veiculares;
* Validação rigorosa de hodômetro para prevenção de fraudes e litígios;
* Elaboração de orçamentos discriminados de peças e mão de obra (em conformidade com o Código de Defesa do Consumidor);
* Aprovação digital remota pelo cliente via WhatsApp e link tokenizado;
* Alocação de mecânicos e controle de estoque de autopeças.

---

## 📁 Estrutura do Repositório

Os artefatos foram organizados em duas vias: arquivos fonte em **Markdown** (com suporte a diagramas Mermaid interativos) e arquivos consolidados em **PDF**, prontos para entrega e impressão:

```text
dio-qa-desafio1/
├── README.md                                          <- Visão geral do projeto e guia de navegação
├── docs/                                              <- Documentação técnica completa em Markdown
│   ├── 01-fluxo-desenvolvimento-e-ciclo-do-bug.md
│   ├── 02-user-stories-ordem-de-servico.md
│   ├── 03-mind-map-user-story.md
│   ├── 04-casos-de-teste-step-by-step.md
│   └── 05-casos-de-teste-bdd.md
├── pdfs/                                              <- Documentos oficiais gerados em formato PDF
│   ├── 01-Plano-Fluxo-Desenvolvimento-e-Ciclo-de-Vida-do-Bug.pdf
│   ├── 02-User-Stories-Modulo-Ordem-de-Servico.pdf
│   ├── 03-Mind-Map-User-Story-Checkin-OS.pdf
│   ├── 04-Casos-de-Teste-Step-by-Step.pdf
│   └── 05-Casos-de-Teste-BDD.pdf
```

---

## 📑 Documentação Entregue (PDFs e Markdown)

A tabela abaixo sintetiza a missão cumprida conforme as diretrizes do desafio de projeto:

| # | Documento Exigido | Arquivo PDF | Fonte Markdown | Resumo do Conteúdo |
|---|---|---|---|---|
| **1** | **Plano de Fluxo de Trabalho & Ciclo de Vida do Bug** | [Acessar PDF](./pdfs/01-Plano-Fluxo-Desenvolvimento-e-Ciclo-de-Vida-do-Bug.pdf) | [Ver Documento](./docs/01-fluxo-desenvolvimento-e-ciclo-do-bug.md) | Fluxo ágil com Shift-Left QA (Refinamento aos 3 Amigos até Deploy/CD), Critérios DoR/DoD, Estados do Defeito (Novo a Fechado), Matriz de Severidade vs Prioridade e Template Corporativo de Bug Report. |
| **2** | **Documento de User Stories (Mínimo 2)** | [Acessar PDF](./pdfs/02-User-Stories-Modulo-Ordem-de-Servico.pdf) | [Ver Documento](./docs/02-user-stories-ordem-de-servico.md) | **US-OS-001:** Abertura de O.S. com Check-in Técnico e Validação Cadastral.<br>**US-OS-002:** Elaboração e Aprovação Digital de Orçamento pelo Cliente via WhatsApp/Portal Web. Ambas com Regras de Negócio, DoR, DoD e Critérios de Aceite. |
| **3** | **Mind-Map de User Story** | [Acessar PDF](./pdfs/03-Mind-Map-User-Story-Checkin-OS.pdf) | [Ver Documento](./docs/03-mind-map-user-story.md) | Mapeamento mental visual da história US-OS-001 em 5 eixos: Dados do Cliente, Identificação do Veículo, Queixas & Sintomas, Regras Críticas e Saídas/Integrações. |
| **4** | **2 Casos de Teste Step-by-Step** | [Acessar PDF](./pdfs/04-Casos-de-Teste-Step-by-Step.pdf) | [Ver Documento](./docs/04-casos-de-teste-step-by-step.md) | **CT-STEP-001:** Emissão de Nova O.S. com Sucesso (Caminho Feliz).<br>**CT-STEP-002:** Bloqueio de Abertura com Hodômetro Inferior ao Histórico (Regra de Negócio). Ambos no padrão IEEE 829. |
| **5** | **2 Casos de Teste em BDD** | [Acessar PDF](./pdfs/05-Casos-de-Teste-BDD.pdf) | [Ver Documento](./docs/05-casos-de-teste-bdd.md) | Especificação em sintaxe Gherkin oficial (`Funcionalidade`, `Contexto`, `Cenário`, `Dado/Quando/Então`):<br>**Cenário 1:** Aprovação Integral de Orçamento com Reserva de Peças.<br>**Cenário 2:** Aprovação Parcial com Exclusão de Opcionais e Recálculo.<br>Bônus: Esquema de Cenário (Scenario Outline) para prazos do CDC. |

---

## 🛠️ Metodologia e Boas Práticas Aplicadas

1. **Shift-Left Testing:** O QA atua desde a fase de especificação e refinamento das User Stories, evitando que ambiguidades ou regras de negócio falhas alcancem a codificação.
2. **Critérios de Qualidade Rígidos:** Adoção de **Definition of Ready (DoR)** para entrada de itens em sprint e **Definition of Done (DoD)** para encerramento de entregas.
3. **Padrão IEEE 829:** Casos de teste estruturados com pré-condições, dados de teste reais, passos sequenciais numerados com ação e resultado esperado, pós-condições e critérios de aprovação.
4. **Behavior Driven Development (BDD):** Uso de Gherkin com português estruturado (`# language: pt`), facilitando a comunicação clara entre negócios, desenvolvimento e qualidade.
5. **Autonomia:** Cada documentação possui vida própria e foi gerada individualmente em PDF para submissão conforme os requisitos do curso.

---

*Projeto acadêmico desenvolvido para a trilha de Qualidade de Software.*
