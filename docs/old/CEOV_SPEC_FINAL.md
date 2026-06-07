# Sistema de Controle de Presença - CEOV

## Visão Geral
Sistema para gestão de atendimento espiritual incluindo recepção, entrevistas, acompanhamento e presença, com foco mobile/tablet-first e integração com autenticação via Keycloak.

---

## Ubiquitous Language (Linguagem do Domínio)

### Assistência
Processo contínuo de acompanhamento espiritual do assistido. Representa o agregado principal do sistema e corresponde ao cartão físico.

### Atendimento
Participação completa em um dia:
- Palestra
- Passe

No sistema atual: **1 presença = 1 atendimento**

### Presença
Registro de participação em um atendimento.

### Entrevista
Primeiro contato onde a assistência é criada.

### Diálogo
Conversa de acompanhamento após ciclos de presenças.

### Trabalhador
Usuário autenticado via Keycloak responsável pelas ações.

### Tipos de Assistência
- A1
- A2

### Status da Assistência
- INICIADA
- ATIVA
- AGUARDANDO_DIALOGO
- FINALIZADA

---

## Modelo de Dados - Assistência (Aggregate Root)

```json
{
  "id": "uuid",
  "numeroCartao": 4293,

  "tipo": "A1",
  "status": "ATIVA",

  "assistido": {
    "nome": "Joao",
    "idade": 21
  },

  "entrevista": {
    "data": "2026-06-01",
    "realizadoPor": "keycloak-sub"
  },

  "presencas": [
    {
      "data": "2026-06-02",
      "registradoPor": "keycloak-sub"
    }
  ],

  "dialogos": [
    {
      "id": "uuid",
      "data": "2026-06-10",
      "realizadoPor": "keycloak-sub"
    }
  ]
}
```

---

## Regras de Negócio (Invariantes)

- Não existe presença sem assistência ativa
- Presença representa atendimento completo (palestra + passe)
- Trabalhador é sempre obtido via JWT
- Data da presença é sempre atual (backend)

### Regra de Ciclo

- A cada 4 presenças → status = AGUARDANDO_DIALOGO
- Bloquear novas presenças neste estado
- Após diálogo → status volta para ATIVA

---

## Máquina de Estados

### Estados
- INICIADA
- ATIVA
- AGUARDANDO_DIALOGO
- FINALIZADA

### Transições

INICIADA → ATIVA  
ATIVA → AGUARDANDO_DIALOGO (4 presenças)  
AGUARDANDO_DIALOGO → ATIVA (após diálogo)  
ATIVA → FINALIZADA  

### Restrições
- Não registrar presença se FINALIZADA
- Não registrar presença se AGUARDANDO_DIALOGO

---

## APIs (Resumo Integrado)

### POST /v1/senhas
Cria senha de atendimento

### POST /v1/entrevistas
Cria assistência

### GET /v1/assistencias?busca=
Busca assistidos

### POST /v1/presencas
Registra presença

### POST /v1/dialogos
Registra diálogo

### GET /v1/trabalhadores/me
Obtém usuário atual

---

## Diretrizes Mobile/Tablets First

- Interface otimizada para toque
- Botões grandes
- Fluxo operacional rápido
- Prioridade: velocidade > estética

---

## Integração com IA (Spec-Driven Design)

Este documento serve como base para geração automatizada por IA.

### Princípios
- Especificação clara e determinística
- Estados explícitos
- Regras formalizadas

### Permite
- Geração de backend
- Geração de testes
- Geração de frontend
- Validação automática de regras

---

## Conclusão

Este documento representa a especificação base do sistema e pode ser usado como fonte única de verdade para desenvolvimento, testes, documentação e automação com IA.
