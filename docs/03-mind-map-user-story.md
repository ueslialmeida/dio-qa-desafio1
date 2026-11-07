# Documentação de Testes: Mind-Map da User Story (US-OS-001)

> **Projeto:** OficinaTech Pro – Sistema Integrado de Gestão para Oficinas Mecânicas  
> **Módulo:** Gerenciamento de Ordens de Serviço (O.S.)  
> **História Mapeada:** `US-OS-001` – Abertura de Nova O.S. com Check-in Técnico e Validação Cadastral  
> **Técnica de QA:** Mapeamento Mental e Exploração Visual de Cenários de Teste  
> **Versão:** 1.0  

---

## 1. Objetivo do Mapa Mental no Processo de QA

O Mapa Mental (Mind-Map) é uma técnica fundamental utilizada pela Engenharia de Qualidade durante a fase de **Shift-Left Testing** para dissecar a história de usuário, quebrando-a visualmente em dimensões funcionais, regras de negócio, dados de entrada, caminhos felizes, fluxos de exceção, segurança e integrações.

Essa técnica permite ao QA:
- Obter **100% de cobertura conceitual** dos cenários antes do início do desenvolvimento.
- Identificar dependências e riscos técnicos ocultos (ex.: inconsistência de hodômetro).
- Servir como base direta para a elaboração dos Casos de Teste Step-by-Step e BDD.

---

## 2. Diagrama de Mapa Mental (Mermaid)

```mermaid
mindmap
  root((US-OS-001: Abertura de O.S. & Check-in))
    Dados do Cliente
      Cliente Cadastrado
        Busca automatizada por CPF ou CNPJ
        Autocompletar de nome e histórico de veículos
      Cliente Novo
        Cadastro rápido obrigatório
        Validação de formato CPF/CNPJ
        Validação de telefone celular com DDD
        Opt-in de notificações WhatsApp
      Tratamento de Erros
        CPF/CNPJ inválido ou incompleto
        Telefone sem DDD ou com caracteres especiais
    Identificação do Veículo
      Placa do Veículo
        Formato Mercosul ABC1D23
        Formato Antigo ABC-1234
        Validação de caracteres alfanuméricos válidos
        Conversão automática para caixa alta
      Quilometragem Hodometro
        Valor numérico inteiro positivo maior que 0
        Regra de consistência maior ou igual a ultima OS
        Exceção bloqueio quando KM menor que histórico
        Override com justificativa de troca de painel
      Check-in Fisico
        Nível de combustível 1/4, 1/2, 3/4, Tanque Cheio
        Checklist de avarias na lataria
        Registro de itens deixados no interior estepe, macaco
    Queixas e Sintomas
      Relato do Proprietário
        Campo de texto aberto min 15 caracteres
        Bloqueio de submissão sem queixa descrita
      Categorização do Defeito
        Mecânica Motor, Câmbio, Suspensão, Freios
        Autoelétrica Bateria, Alternador, Chicote
        Revisão Periódica Troca de óleo, filtros
      Prioridade Operacional
        Urgência Guinchado / Pane total
        Rotina Manutenção agendada
    Regras de Negócio e Validações
      Alerta de Garantia Ativa
        Consulta de serviços efetuados nos ultimos 90 dias
        Exibição de banner de garantia legal CDC
      Prevenção de Duplicidade
        Bloqueio de nova OS se placa já possui OS aberta
      Campos Obrigatórios
        Destaque visual em vermelho para campos vazios
        Foco automático no primeiro campo inválido
    Saídas e Pós-Condições
      Identificador Único
        Geração de protocolo sequencial OS-AAAA-XXXXX
      Transição de Estado
        Status inicial gravado como Aguardando Diagnóstico
      Comunicação e Notificação
        Disparo de mensagem de confirmação no WhatsApp
        Emissão de comprovante de entrada em PDF
        Card inserido no painel Kanban dos mecânicos
```

---

## 3. Matriz de Cobertura Derivada do Mapa Mental

A partir das ramificações do Mapa Mental, a equipe de QA derivou as seguintes frentes de teste:

| Ramificação do Mapa | Tipo de Teste Aplicado | Objetivos e Cenários Cobertos |
|---|---|---|
| **1. Dados do Cliente** | Funcional / Validação de Dados | Testar busca por CPF, autocompletar de dados, cadastro de novo cliente, máscara de telefone e rejeição de CPFs inválidos. |
| **2. Identificação do Veículo** | Regra de Negócio / Limite | Testar formatos de placa (Mercosul e Antiga), validação de hodômetro vs. histórico da oficina e consistência do checklist. |
| **3. Queixas e Sintomas** | Interface / Usabilidade | Validar tamanho mínimo do relato, seleção de categorias automotivas e preenchimento de checklist de avarias. |
| **4. Validações Críticas** | Negativo / Segurança | Tentar cadastrar O.S. duplicada para veículo já presente na oficina; validar disparo de alerta de garantia legal de 90 dias. |
| **5. Saídas e Integrações** | Integração / E2E | Validar geração do número sequencial `OS-2026-XXXXX`, gravação do status inicial e envio de evento para Kanban e WhatsApp. |

---

## 4. Árvore de Ideias de Teste (Test Idea Tree)

* **Eixo 1: Caminho Feliz (Happy Paths)**
  - [x] Abertura com cliente existente + placa Mercosul + KM consistente + geração de O.S.
  - [x] Abertura com cliente novo cadastrado na hora + placa padrão antigo + relato preenchido.
  - [x] Envio de notificação no WhatsApp com link de acompanhamento do status.
* **Eixo 2: Cenários Negativos e de Exceção (Negative Testing)**
  - [x] Hodômetro com valor inferior ao da última O.S. finalizada (Bloqueio obrigatório).
  - [x] Placa com caracteres inválidos (ex.: símbolos, espaços ou menos de 7 dígitos).
  - [x] Veículo com O.S. em andamento na oficina (Bloqueio de duplicidade).
  - [x] Submissão com campos obrigatórios vazios.
* **Eixo 3: Regras Legais e de Negócio da Oficina**
  - [x] Identificação de peças/serviços trocados há menos de 90 dias para aplicação de garantia.
  - [x] Registro fotográfico das avarias de lataria para prevenção de litígios no check-in.
