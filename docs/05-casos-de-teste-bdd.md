# Casos de Teste em BDD (Behavior Driven Development)

> **Projeto:** OficinaTech Pro – Sistema Integrado de Gestão para Oficinas Mecânicas  
> **Módulo:** Gerenciamento de Ordens de Serviço (O.S.)  
> **Técnica:** Desenvolvimento Orientado por Comportamento (BDD / Gherkin)  
> **User Story Associada:** `US-OS-002` – Elaboração e Aprovação Digital de Orçamento de Peças e Serviços  
> **Versão:** 1.0  

---

## 1. Visão Geral dos Testes Orientados por Comportamento (BDD)

O BDD (Behavior Driven Development) promove uma linguagem ubíqua e compartilhada entre os Three Amigos (Negócios/PO, Desenvolvimento e Qualidade/QA). Utilizando a sintaxe declarativa do **Gherkin** (`Dado`, `Quando`, `Então`), os cenários documentam simultaneamente os requisitos de negócio e servem como especificações executáveis para automação de testes (Cucumber, SpecFlow, Playwright ou Cypress).

Abaixo estão formalizados dois cenários de teste essenciais para o fluxo de aprovação de orçamentos da oficina mecânica:
1. **Cenário 1:** Aprovação Integral de Orçamento pelo Cliente via Link Seguro com Transição de Status e Reserva de Peças.
2. **Cenário 2:** Aprovação Parcial com Exclusão de Item Opcional, Recálculo em Tempo Real e Emissão de Alerta.

---

## 2. Especificação BDD da Funcionalidade

```gherkin
# language: pt
Funcionalidade: Aprovação Digital de Orçamento de Ordem de Serviço
  Como cliente proprietário de um veículo em manutenção na OficinaTech Pro
  Eu quero visualizar o orçamento detalhado de peças e serviços pelo navegador
  Para que eu possa aprovar ou ajustar os itens com segurança jurídica e transparência

  Contexto:
    Dado que a oficina possui a Ordem de Serviço "OS-2026-00045" cadastrada
    E a O.S. está associada ao veículo "Honda Civic 2021" de placa "BRA2E19"
    E o mecânico líder finalizou o diagnóstico com a seguinte discriminação de itens:
      | Tipo    | Descrição                        | Categoria         | Qtd | Valor Unitário | Subtotal    |
      | Peça    | Jogo de Pastilhas de Freio Bosch | Segurança Crítica | 1   | R$ 320,00      | R$ 320,00   |
      | Peça    | Par de Discos de Freio Dianteiro | Segurança Crítica | 1   | R$ 580,00      | R$ 580,00   |
      | Peça    | Par de Palhetas de Silicone      | Conforto/Opcional | 1   | R$ 120,00      | R$ 120,00   |
      | Serviço | Mão de obra de substituição      | Serviço Mecânico  | 2h  | R$ 150,00      | R$ 300,00   |
    E o valor total inicial orçado é de "R$ 1.320,00"
    E a O.S. está com o status "Orçamento Pendente de Aprovação"
    E o cliente recebeu via WhatsApp um link seguro contendo o token ativo "tok_sec_987xyz"
```

---

## 3. Cenário 1: Aprovação Integral de Orçamento (Caminho Feliz)

```gherkin
  @regressao @critico @smoke @aprovacao_orcamento
  Cenário: Cliente aprova o orçamento integralmente via portal web móvel
    Dado que o cliente acessa o endereço "/portal-cliente/orcamento?token=tok_sec_987xyz"
    E a página carrega a discriminação completa das 3 peças e 1 serviço totalizando "R$ 1.320,00"
    Quando o cliente seleciona a opção "Aprovar Orçamento Completo"
    E preenche o campo de confirmação com seu CPF "123.456.789-00"
    E clica no botão "Confirmar e Autorizar Reparos"
    Então o sistema deve registrar a aprovação com sucesso
    E deve salvar a auditoria contendo:
      | Campo                  | Valor Esperado                                |
      | Status da O.S.         | Orçamento Aprovado - Em Execução              |
      | Tipo de Aprovação      | Integral                                      |
      | Valor Total Aprovado   | R$ 1.320,00                                   |
      | Timestamp do Aceite    | Data e hora corrente da transação             |
      | Endereço IP do Cliente | Registrado no log de conformidade jurídica    |
    E o estoque do almoxarifado deve reservar automaticamente:
      | Código Peça | Quantidade Reservada |
      | PST-BOSCH-01| 1                    |
      | DSC-DIANT-02| 1                    |
      | PLH-SILIC-03| 1                    |
    E deve emitir um alerta sonoro e visual no painel Kanban da oficina para o mecânico responsável
    E o cliente deve visualizar a mensagem: "Orçamento autorizado! A equipe mecânica já iniciou os preparativos para o seu veículo."
```

---

## 4. Cenário 2: Aprovação Parcial com Desmarcação de Item Opcional (Fluxo Alternativo)

```gherkin
  @regressao @regras_negocio @aprovacao_parcial
  Cenário: Cliente desmarca item opcional de conforto e aprova apenas os itens de segurança
    Dado que o cliente acessa o link do orçamento com os itens orçados
    Quando o cliente desmarca a caixa de seleção do item "Par de Palhetas de Silicone" no valor de "R$ 120,00"
    Então o sistema deve recalcular instantaneamente os valores na tela:
      | Totalizador           | Novo Valor Calculado |
      | Subtotal de Peças     | R$ 900,00            |
      | Subtotal de Serviços  | R$ 300,00            |
      | Valor Geral da O.S.   | R$ 1.200,00          |
    E os itens de "Segurança Crítica" devem permanecer marcados e protegidos contra remoção acidental
    Quando o cliente clica em "Confirmar e Autorizar Reparos" com o valor de "R$ 1.200,00"
    E confirma com seu CPF "123.456.789-00"
    Então o status da O.S. deve mudar para "Orçamento Parcial Aprovado - Em Execução"
    E as palhetas de silicone não devem ser debitadas nem reservadas no estoque
    E uma notificação interna deve alertar o consultor técnico:
      "Atenção: O cliente desmarcou o item 'Par de Palhetas de Silicone'. Valor ajustado para R$ 1.200,00."
    E o comprovante final em PDF deve registrar o status "Item Declinado pelo Cliente" na linha correspondente às palhetas
```

---

## 5. Cenário Adicional com Esquema de Cenário (Scenario Outline)

Demonstração de cobertura por tabela de dados para validação de prazo de validade de orçamentos (art. 40 do CDC):

```gherkin
  @regras_cdc @limite
  Esquema do Cenário: Validação do prazo legal de expiração do orçamento (10 dias corridos)
    Dado que um orçamento de O.S. foi gerado há <dias_decorridos> dias atrás
    Quando o cliente tenta abrir o link do orçamento no portal
    Então o sistema deve aplicar o comportamento "<comportamento_esperado>"
    E os botões de ação devem estar "<estado_botoes>"

    Exemplos:
      | dias_decorridos | comportamento_esperado                 | estado_botoes |
      | 1               | Exibir orçamento válido normalmente    | Habilitados   |
      | 5               | Exibir orçamento válido normalmente    | Habilitados   |
      | 10              | Exibir aviso: Último dia de validade   | Habilitados   |
      | 11              | Exibir aviso: Proposta expirada (CDC)  | Desabilitados |
      | 30              | Exibir aviso: Proposta expirada (CDC)  | Desabilitados |
```
