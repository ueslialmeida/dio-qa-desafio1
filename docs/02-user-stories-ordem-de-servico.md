# Documentação de User Stories – Módulo de Ordem de Serviço (O.S.)

> **Projeto:** OficinaTech Pro – Sistema Integrado de Gestão para Oficinas Mecânicas  
> **Módulo:** Gerenciamento de Ordens de Serviço (O.S.)  
> **Disciplina:** Engenharia de Requisitos e Qualidade de Software (QA)  
> **Versão:** 1.0  

---

## 1. Visão Geral das Histórias de Usuário

No ecossistema do **OficinaTech Pro**, o módulo de Ordem de Serviço é responsável por estruturar toda a jornada do veículo dentro da oficina, garantindo conformidade com o Código de Defesa do Consumidor (art. 40 - obrigatoriedade de orçamento prévio) e máxima eficiência operacional da equipe técnica.

As histórias de usuário documentadas a seguir cobrem os dois pilares mais críticos do módulo:
1. **US-OS-001:** Recepção técnica e abertura da O.S. (Check-in veicular e triagem).
2. **US-OS-002:** Emissão e aprovação digital de orçamento pelo proprietário via dispositivo móvel.

---

## 2. User Story 1: Abertura e Emissão de Nova Ordem de Serviço com Check-in Técnico

### 2.1. Identificação e Narrativa
* **ID:** `US-OS-001`
* **Título:** Abertura de Nova O.S. com Check-in Técnico e Validação Cadastral
* **Épico:** Gestão de Entrada e Triagem Veicular
* **Prioridade de Negócio:** Crítica (Alta)
* **Estimativa de Complexidade:** 8 Story Points

```text
COMO Consultor Técnico / Atendente da Oficina,
EU QUERO registrar uma nova Ordem de Serviço informando os dados cadastrais do cliente,
identificação do veículo (placa e hodômetro) e o relato inicial de queixas/sintomas,
PARA QUE o veículo seja formalmente admitido na oficina com número de protocolo único,
histórico rastreável e direcionamento imediato para a fila de diagnóstico técnico.
```

---

### 2.2. Regras de Negócio (RN)

* **RN01 - Unicidade e Identificador da O.S.:**
  - O sistema deve gerar automaticamente um identificador sequencial único no formato `OS-AAAA-XXXXX` (Exemplo: `OS-2026-00045`).
* **RN02 - Validação da Placa do Veículo:**
  - O sistema deve aceitar e validar os formatos padrão Brasil:
    - Mercosul: 3 letras, 1 número, 1 letra, 2 números (ex.: `BRA2E19`).
    - Padrão Antigo: 3 letras, traço opcional, 4 números (ex.: `ABC-1234`).
  - Placas com caracteres inválidos ou comprimento divergente devem ser bloqueadas com mensagem clara.
* **RN03 - Validação de Quilometragem (Hodômetro):**
  - O campo de quilometragem deve aceitar somente valores inteiros positivos superiores a 0.
  - Caso o veículo já possua O.S. anterior registrada na oficina, a nova quilometragem **não pode ser inferior** à última registrada.
  - Se for inferior, o sistema deve bloquear o cadastro e solicitar confirmação de justificativa técnica (ex.: substituição de painel/hodômetro com autorização de supervisor).
* **RN04 - Campos Obrigatórios para Abertura:**
  - Dados do Cliente: Nome completo, CPF/CNPJ e Telefone WhatsApp válido.
  - Dados do Veículo: Placa, Modelo, Ano e Quilometragem atual.
  - Dados do Check-in: Nível de combustível (1/4, 1/2, 3/4, Cheio), fotos/avarias visíveis de lataria e relato das queixas do cliente.
* **RN05 - Status Inicial da O.S.:**
  - Toda nova O.S. criada deve ser gravada com o status inicial: `Aguardando Diagnóstico`.
* **RN06 - Alerta de Garantia de Manutenção Anterior:**
  - Se a placa possuir serviços executados nos últimos 90 dias com garantia legal ativa, o sistema deve exibir um *banner* em destaque amarelo informando os itens ainda cobertos.

---

### 2.3. Critérios de Aceite (Acceptance Criteria)

#### Cenário 1: Abertura de O.S. para veículo com dados íntegros (Caminho Feliz)
* **Dado** que o Consultor Técnico está autenticado no sistema OficinaTech Pro,
* **E** acessou a tela de "Abertura de Nova Ordem de Serviço",
* **Quando** preenche os dados do cliente "Carlos Eduardo", CPF "123.456.789-00", placa "BRA2E19", quilometragem "45.000",
* **E** preenche o relato de queixa "Ruído agudo na frenagem dianteira e luz da injeção acesa no painel",
* **E** clica no botão "Gerar Ordem de Serviço",
* **Então** o sistema deve gerar o registro com status "Aguardando Diagnóstico",
* **E** emitir o código identificador sequencial único no padrão `OS-2026-XXXXX`,
* **E** exibir a opção de envio automático do comprovante digital via WhatsApp para o cliente.

#### Cenário 2: Bloqueio de quilometragem inferior ao histórico (Fluxo de Exceção)
* **Dado** que o veículo placa "BRA2E19" possui uma O.S. concluída com quilometragem registrada de "48.500 km",
* **Quando** o atendente tenta abrir uma nova O.S. informando a quilometragem "42.000 km",
* **E** tenta submeter o formulário,
* **Então** o sistema deve impedir o salvamento,
* **E** exibir a mensagem de erro: *"Inconsistência de Hodômetro: O valor informado (42.000 km) é inferior à última O.S. registrada (48.500 km). Verifique o hodômetro ou solicite autorização de gerência para substituição de painel."*.

#### Cenário 3: Validação de campos obrigatórios vazios
* **Dado** que o usuário está no formulário de abertura de O.S.,
* **Quando** clica em "Gerar Ordem de Serviço" sem preencher os campos de placa e relato de queixa,
* **Então** os campos obrigatórios vazios devem ser destacados em borda vermelha com texto de auxílio,
* **E** nenhum registro deve ser criado na base de dados.

---

### 2.4. Definition of Ready (DoR) e Definition of Done (DoD)

* **DoR da US-OS-001:** Requisitos validados com o consultor técnico piloto; formato de placa homologado; regras de validação de hodômetro aprovadas pelo QA; protótipo de tela de check-in revisado.
* **DoD da US-OS-001:** Formulário validado com testes unitários no front-end e validação estrita no back-end; validações de placa e KM cobertas por testes automatizados; persistência no banco de dados testada; envio de comprovante PDF/WhatsApp testado em Staging.

---

## 3. User Story 2: Elaboração e Envio de Orçamento Detalhado com Aprovação Digital pelo Cliente

### 3.1. Identificação e Narrativa
* **ID:** `US-OS-002`
* **Título:** Elaboração e Aprovação Digital de Orçamento de Peças e Serviços
* **Épico:** Orçamento e Autorização de Reparos
* **Prioridade de Negócio:** Crítica (Alta)
* **Estimativa de Complexidade:** 13 Story Points

```text
COMO Cliente proprietário de veículo em manutenção,
EU QUERO receber um orçamento discriminado contendo peças, serviços de mão de obra,
valores unitários, previsão de conclusão e formas de pagamento via link interativo no WhatsApp/SMS,
PARA QUE eu possa analisar, aprovar ou recusar o orçamento com segurança jurídica e transparência,
sem a necessidade de deslocamento físico até a oficina.
```

---

### 3.2. Regras de Negócio (RN)

* **RN01 - Discriminação Obrigatória de Itens:**
  - O orçamento deve separar claramente duas tabelas:
    1. **Peças e Materiais:** Código de referência da peça, descrição, marca, quantidade, garantia (dias), valor unitário e subtotal.
    2. **Mão de Obra e Serviços:** Descrição do serviço, tempo estimado em horas, valor por hora/serviço e subtotal.
* **RN02 - Prazo de Validade da Proposta:**
  - O orçamento deve exibir data de emissão e validade padrão de **10 dias corridos**, conforme artigo 40, § 2º do Código de Defesa do Consumidor. Após esse período, o link expira para renegociação de preços de autopeças.
* **RN03 - Link Seguro e Tokenizado:**
  - O link enviado ao cliente deve conter token criptográfico efêmero único, dispensando login complexo pelo cliente, mas garantindo acesso exclusivo aos dados do seu veículo.
* **RN04 - Modos de Decisão pelo Cliente:**
  - O cliente poderá selecionar:
    - **Aprovação Total:** Aprova todos os itens de peças e serviços orçados.
    - **Aprovação Parcial:** Permite selecionar itens de comodidade/opcionais para remoção (ex.: substituição de palhetas), mantendo os itens estruturais/obrigatórios de segurança.
    - **Recusa Total:** Rejeita a proposta e insere motivo/justificativa.
* **RN05 - Auditoria de Aceite:**
  - Ao confirmar a aprovação, o sistema deve registrar no log de auditoria: data, hora exata, endereço IP do cliente, geolocalização aproximada (quando autorizada) e identificador do dispositivo.
* **RN06 - Transição Automática de Status da O.S.:**
  - **Aprovado:** O status da O.S. passa de `Orçamento Pendente` para `Orçamento Aprovado - Em Execução`, disparando alerta sonoro e visual no painel Kanban dos mecânicos e reservando as peças no estoque.
  - **Recusado:** O status passa para `Orçamento Recusado - Aguardando Retirada`, liberando as peças reservadas.

---

### 3.3. Critérios de Aceite (Acceptance Criteria)

#### Cenário 1: Aprovação total do orçamento pelo cliente via portal web (Caminho Feliz)
* **Dado** que o orçamento da O.S. `OS-2026-00045` totalizando R$ 1.450,00 foi finalizado e enviado ao cliente,
* **E** a O.S. está com o status `Orçamento Enviado - Aguardando Aprovação`,
* **Quando** o cliente acessa o link tokenizado recebido em seu WhatsApp,
* **E** clica no botão "Aprovar Orçamento Integral",
* **E** digita seu CPF para assinatura digital e confirma,
* **Então** o sistema deve registrar o consentimento com carimbo de data, hora e IP,
* **E** atualizar o status da O.S. para `Orçamento Aprovado - Em Execução`,
* **E** notificar instantaneamente o mecânico responsável via painel da oficina,
* **E** reservar as peças no módulo de almoxarifado.

#### Cenário 2: Tentativa de acesso a link de orçamento expirado (Fluxo de Exceção)
* **Dado** que um orçamento foi enviado há 12 dias corridos e sua validade configurada era de 10 dias,
* **Quando** o cliente clica no link para visualização,
* **Então** o sistema deve exibir uma tela informativa: *"Este orçamento expirou em virtude da flutuação de preços de peças automotivas. Entre em contato com a oficina para revalidação dos valores."*,
* **E** os botões de aprovação e recusa devem estar desabilitados.

#### Cenário 3: Aprovação parcial com itens de segurança obrigatórios
* **Dado** que o orçamento contém itens marcados pelo mecânico como "Segurança Crítica" (ex.: Pastilhas de freio) e itens marcados como "Opcionais" (ex.: Higienização de ar-condicionado),
* **Quando** o cliente tenta desmarcar um item de "Segurança Crítica",
* **Então** o sistema deve emitir um aviso em destaque: *"Atenção: Este item é essencial para a segurança de tráfego do veículo. A não execução requer assinatura de termo de responsabilidade na retirada."*,
* **E** recalcular os subtotais dinamicamente na tela caso o cliente confirme a desmarcação de itens opcionais.

---

### 3.4. Definition of Ready (DoR) e Definition of Done (DoD)

* **DoR da US-OS-002:** Regras do CDC sobre orçamento orçamentário levantadas; integração de mensageria (WhatsApp/SMS) mapeada; layout responsivo mobile testado em protótipo; critérios de expiração de token definidos.
* **DoD da US-OS-002:** Interface responsiva mobile auditada; fluxo de aprovação com persistência de auditoria (IP, data/hora) coberto por testes automatizados; concorrência de estoque testada; notificações em tempo real validadas pelo QA.
