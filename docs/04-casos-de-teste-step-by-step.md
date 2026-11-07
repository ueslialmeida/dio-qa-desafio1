# Casos de Teste Passo a Passo (Step-by-Step)

> **Projeto:** OficinaTech Pro – Sistema Integrado de Gestão para Oficinas Mecânicas  
> **Módulo:** Gerenciamento de Ordens de Serviço (O.S.)  
> **Técnica:** Especificação Detalhada Passo a Passo (Baseada na IEEE 829)  
> **Versão:** 1.0  

---

## 1. Visão Geral dos Casos de Teste Step-by-Step

A técnica de testes **Step-by-Step** (Passo a Passo) fornece instruções precisas, sequenciais e reproduzíveis para guiar a execução dos testes, sejam eles manuais ou base para scripts de automação.

Abaixo estão detalhados dois casos de teste completos do Módulo de Ordem de Serviço:
1. **CT-STEP-001:** Abertura de O.S. com Sucesso e Dados Consistentes (Caminho Feliz).
2. **CT-STEP-002:** Bloqueio de Abertura de O.S. com Hodômetro Inferior ao Histórico (Fluxo de Exceção / Regra de Negócio).

---

## 2. Caso de Teste 1: Abertura de O.S. com Sucesso (Caminho Feliz)

| Metadado | Descrição |
|---|---|
| **ID do Teste** | `CT-STEP-001` |
| **Título** | Emissão de Nova Ordem de Serviço com Check-in Técnico e Validação Cadastral Completa |
| **User Story** | `US-OS-001` (Abertura de Nova O.S. com Check-in Técnico) |
| **Regras Vinculadas** | `RN01` (ID sequencial), `RN02` (Placa), `RN03` (KM consistente), `RN04` (Obrigatórios), `RN05` (Status) |
| **Tipo de Teste** | Funcional / Caminho Feliz (Positivo) |
| **Severidade** | Crítica (Blocker) |
| **Prioridade** | P1 (Alta) |
| **Ambiente** | Staging / QA (Build v2.4.1) |
| **Autor do Teste** | Analista de QA |

### 2.1. Pré-condições
1. Usuário autenticado com perfil `Consultor Técnico` ou `Atendente`.
2. O cliente `Marcos Vinícius Silva` (CPF: `234.567.890-11`) deve estar previamente cadastrado na base de dados.
3. O veículo Honda Civic Sedan 2021 (Placa: `BRA2E19`) possui última O.S. arquivada com `38.200 km`.
4. O sistema de mensageria (Simulador de API do WhatsApp) deve estar operacional.

### 2.2. Massa de Dados de Teste
* **Cliente:** Marcos Vinícius Silva | CPF: `234.567.890-11` | Telefone: `(11) 98765-4321`
* **Veículo:** Honda Civic EXL 2.0 2021 | Placa: `BRA2E19` | Cor: Prata
* **Quilometragem Atual:** `44.800 km` (consistente, pois $44.800 > 38.200$)
* **Nível de Combustível:** `1/2 Tanque`
* **Relato do Cliente:** "Barulho metálico na suspensão dianteira ao passar por desníveis e pedal de freio vibrando em frenagens acima de 80 km/h."
* **Categoria:** Mecânica Geral (Suspensão e Freios)

### 2.3. Procedimento de Execução (Passo a Passo)

| Passo | Ação do Usuário | Dados de Entrada | Resultado Esperado | Status |
|:---:|---|---|---|:---:|
| **1** | Acessar o menu lateral esquerdo do sistema e clicar na opção "Ordens de Serviço". | N/A | O submenu expande exibindo as opções "Nova O.S.", "Painel Kanban" e "Consultar O.S.". | Pass |
| **2** | Clicar no botão "+ Nova Ordem de Serviço". | N/A | O sistema carrega a tela do formulário de check-in com o cursor posicionado no campo de busca de cliente. | Pass |
| **3** | No campo "Buscar Cliente", digitar o CPF de teste e pressionar Enter. | `234.567.890-11` | O sistema autocompleta os campos: Nome (`Marcos Vinícius Silva`), Telefone celular e lista os veículos vinculados. | Pass |
| **4** | Selecionar o veículo Honda Civic da lista ou digitar a placa no campo de identificação veicular. | `BRA2E19` | O sistema carrega os dados técnicos do veículo e exibe no card lateral: "Última O.S. #OS-2025-08912 em 15/10/2025 com 38.200 km". | Pass |
| **5** | Informar a quilometragem atual do veículo no campo "Hodômetro Atual". | `44800` | O sistema formata o valor como `44.800 km` e exibe ícone de validação verde (KM consistente com o histórico). | Pass |
| **6** | Selecionar o nível de combustível no seletor gráfico. | `1/2 Tanque` | O indicador visual preenche a metade do marcador de combustível. | Pass |
| **7** | No campo "Checklist de Lataria", marcar a opção "Avarias Preexistentes" e descrever o detalhe. | "Pequeno arranhão superficial no para-choque traseiro direito." | O texto é gravado e o marcador de avaria fica registrado no diagrama do carro. | Pass |
| **8** | No campo obrigatório "Relato das Queixas do Cliente", colar o texto com a queixa informada. | "Barulho metálico na suspensão dianteira..." | O contador de caracteres atualiza e a borda do campo permanece neutra/válida. | Pass |
| **9** | Selecionar a categoria de serviço e definir a prioridade técnica. | Categoria: `Mecânica Geral` / Prioridade: `Normal` | Os seletores refletem as opções marcadas sem erros visuais. | Pass |
| **10** | Clicar no botão principal "Gerar Ordem de Serviço". | Clique com mouse / Enter | O sistema exibe animação de carregamento (spinner) por menos de 1 segundo. | Pass |
| **11** | Verificar a mensagem de confirmação (Toast/Modal) e a página de destino. | N/A | Exibe mensagem de sucesso: *"Ordem de Serviço gerada com sucesso sob o protocolo OS-2026-00108"*. O status inicial é gravado como **Aguardando Diagnóstico**. | Pass |
| **12** | Validar a integração de envio de notificação digital ao cliente. | N/A | Modal exibe confirmação: *"Comprovante digital disparado via WhatsApp para (11) 98765-4321 com link de acompanhamento"*. | Pass |

### 2.4. Pós-condições
* A O.S. de código `OS-2026-00108` é criada no banco de dados com todos os metadados preservados.
* Um novo card é automaticamente inserido na primeira coluna ("Aguardando Diagnóstico") do Painel Kanban da oficina.

---

## 3. Caso de Teste 2: Bloqueio de Hodômetro Inferior ao Histórico (Fluxo de Exceção)

| Metadado | Descrição |
|---|---|
| **ID do Teste** | `CT-STEP-002` |
| **Título** | Validação de Regra de Negócio: Bloqueio de Hodômetro com Valor Inferior ao Histórico Registrado |
| **User Story** | `US-OS-001` (Abertura de Nova O.S. com Check-in Técnico) |
| **Regras Vinculadas** | `RN03` (Validação estrita de quilometragem e prevenção de fraude de hodômetro) |
| **Tipo de Teste** | Funcional / Negativo / Regra de Negócio |
| **Severidade** | Alta (Major) |
| **Prioridade** | P2 (Alta) |
| **Ambiente** | Staging / QA (Build v2.4.1) |
| **Autor do Teste** | Analista de QA |

### 3.1. Pré-condições
1. Veículo placa `BRA2E19` possui histórico de manutenções com última O.S. finalizada registrando `45.000 km`.
2. Usuário operador logado com perfil padrão `Atendente / Consultor Técnico` (sem privilégio de gerente/administrador).

### 3.2. Massa de Dados de Teste
* **Veículo:** Honda Civic 2021 | Placa: `BRA2E19` | Último KM registrado no banco: `45.000 km`
* **Quilometragem Inválida a Testar:** `41.200 km` (divergente, pois $41.200 < 45.000$)
* **Relato:** "Revisão geral de freios e troca de fluidos."

### 3.3. Procedimento de Execução (Passo a Passo)

| Passo | Ação do Usuário | Dados de Entrada | Resultado Esperado | Status |
|:---:|---|---|---|:---:|
| **1** | Acessar "Ordens de Serviço" > "+ Nova Ordem de Serviço". | N/A | Formulário de check-in é carregado em branco. | Pass |
| **2** | No campo "Placa do Veículo", inserir a placa de teste e acionar o botão de busca. | `BRA2E19` | O sistema localiza o histórico do veículo e exibe: *"Última quilometragem registrada: 45.000 km"*. | Pass |
| **3** | No campo "Hodômetro Atual", digitar propositalmente um valor inferior ao histórico. | `41200` | O campo formata para `41.200 km`. Ao perder o foco (evento onBlur), a borda do campo muda para cor vermelha. | Pass |
| **4** | Preencher os demais campos obrigatórios (Cliente, Queixas, Combustível). | Dados válidos genéricos | Campos preenchidos sem erros adicionais. | Pass |
| **5** | Clicar no botão "Gerar Ordem de Serviço". | Clique do mouse | O sistema **não** envia o formulário e bloqueia o salvamento da O.S. | Pass |
| **6** | Verificar o alerta de bloqueio e validação em tela. | N/A | Um modal de erro em vermelho é apresentado: *"Inconsistência de Hodômetro: O valor informado (41.200 km) é inferior ao último registro da oficina (45.000 km). Verifique a leitura física do painel."*. | Pass |
| **7** | Verificar a exibição do link de exceção "Substituição de Painel / Troca de Hodômetro". | N/A | O sistema informa que para prosseguir com KM inferior é obrigatório autorização com senha de usuário de perfil "Gerente Técnico" e anexação de laudo de substituição. | Pass |
| **8** | Fechar o modal e verificar se qualquer registro foi criado no banco de dados. | Consulta via API/BD | Nenhuma O.S. é gerada ou persistida. O sistema permanece na tela de edição mantendo a integridade dos dados. | Pass |

### 3.4. Critério de Aprovação / Reprovação (Pass/Fail)
* **Aprovado (Pass):** O sistema impede categoricamente a gravação de O.S. com KM retroativa sem aprovação de supervisão, exibindo mensagem instrutiva sem quebrar a interface nem causar erro 500 no back-end.
* **Reprovado (Fail):** O sistema grava a O.S. permitindo inconsistência histórica, ou a tela trava sem exibir mensagem amigável ao atendente.
