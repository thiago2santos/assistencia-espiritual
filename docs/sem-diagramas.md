Perfeito — agora vamos fechar isso do jeito certo: **um artefato pronto pra IDE + agentes + IA**.

Já incorporei:

* ✅ wireframe de **Registrar diálogo**
* ✅ melhoria da **tela de chamar senha (tabela melhorada)**
* ✅ padronização consistente em Markdown

***

# 📄 ✅ VERSÃO FINAL (Markdown exportável)

Pode copiar direto 👇

````markdown
# Sistema de Controle de Presença - CEOV

---

# 1. Visão Geral

Sistema para gestão de atendimento espiritual incluindo recepção, entrevistas, acompanhamento e controle de presença.

Objetivo principal: permitir operação rápida, simples e confiável em ambiente movimentado, com foco mobile/tablet-first.

---

# 2. Fluxo de Atendimento (Visão Macro)

1. Assistido chega  
2. Recepção gera senha  
3. Assistido entra na fila  
4. Entrevistador chama senha  
5. Entrevista é realizada  
6. Assistência é criada  
7. Assistido participa das sessões  
8. Presenças são registradas  
9. A cada 4 presenças → diálogo necessário  
10. Após diálogo → ciclo continua  

---

# 3. Contextos de Domínio

## 3.1 Recepção
- Geração de senha  
- Controle de fila  

## 3.2 Atendimento Espiritual
- Entrevista  
- Assistência  
- Presenças  
- Diálogos  

---

# 4. Linguagem Ubíqua

### Assistência
Processo contínuo de acompanhamento espiritual. Representa o agregado principal (equivalente ao cartão físico).

### Atendimento
Participação completa em um dia (palestra + passe).

### Presença
Registro de um atendimento.

### Entrevista
Primeiro contato que inicia a assistência.

### Diálogo
Conversa de acompanhamento após ciclos de presença.

### Trabalhador
Usuário autenticado via Keycloak.

---

# 5. Modelo de Domínio

## 5.0 Princípio Central
Assistência é o agregado raiz do sistema.  
Todas as operações (entrevista, presença, diálogo) acontecem dentro dela.

## 5.1 Aggregate Root: Assistência

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
````

***

# 6. Regras de Negócio

* Não existe presença sem assistência ativa
* Presença representa um atendimento completo
* Trabalhador sempre identificado via JWT
* Data da presença é sempre gerada no backend

## Regra de ciclo

* Ao registrar a 4ª presença:
  * status → AGUARDANDO\_DIALOGO
  * bloquear novas presenças
* Após diálogo:
  * status → ATIVA

***

# 7. Máquina de Estados

## Estados

* INICIADA
* ATIVA
* AGUARDANDO\_DIALOGO
* FINALIZADA

## Transições

* INICIADA → ATIVA
* ATIVA → AGUARDANDO\_DIALOGO
* AGUARDANDO\_DIALOGO → ATIVA
* ATIVA → FINALIZADA

## Restrições

* Não registrar presença se FINALIZADA
* Não registrar presença se AGUARDANDO\_DIALOGO

***

# 8. Jornadas e Casos de Uso

## 8.1 Acesso ao Sistema

### Cadastro

Fluxo:

1. Usuário acessa tela
2. Preenche dados
3. Envia para Keycloak
4. Conta criada

Regras:

* Validação obrigatória

UI:

```
CADASTRO DE USUÁRIO

Nome:
[.....................]

Email:
[.....................]

Senha:
[.....................]

[ Cadastrar ]
```

***

### Login

Fluxo:

* Autenticação via Keycloak

***

### Recuperação de senha

Fluxo:

* Delegado ao Keycloak

***

## 8.2 Recepção

### Gerar senha

Fluxo:

1. POST /v1/senhas
2. Retorna número

Regras:

* Número incremental

UI:

```
EMISSÃO DE SENHA

[ GERAR NOVA SENHA ]

Últimas:
101
102
```

***

### Chamar próxima senha

Fluxo:

1. GET /v1/senhas/aguardando
2. Selecionar senha
3. POST /v1/senhas/chamar

Regras:

* FIFO
* Apenas AGUARDANDO
* Atualiza para EM\_ATENDIMENTO

UI:

```
CHAMAR SENHA

Senhas aguardando:

Nº   | Tipo         | Tempo | Status       | Ação
--------------------------------------------------------
101  | Normal       | 5 min | AGUARDANDO   | [Chamar]
102  | Preferencial | 2 min | AGUARDANDO   | [Chamar]
103  | Normal       | 10min | AGUARDANDO   | [Chamar]

Selecionada:
Senha 101

[ CHAMAR SENHA ]
```

***

## 8.3 Entrevista

### Registrar entrevista

Fluxo:

1. Preencher dados
2. POST /v1/entrevistas
3. Criar assistência

Regras:

* Associar trabalhador

UI:

```
ENTREVISTA

Nome:
[.....................]

Idade:
[...]

Observações:
[.....................]

[ Finalizar Entrevista ]
```

***

## 8.4 Acompanhamento

### Registrar presença

Fluxo:

1. GET /v1/assistidos
2. Selecionar
3. POST /v1/presencas

Regras:

* status = ATIVA
* bloquear em AGUARDANDO\_DIALOGO

UI:

```
REGISTRO DE PRESENÇA

Buscar:
[ Nome ou nº cartão ]

Resultados:
João (1234) [Selecionar]

Selecionado:
João

[ Registrar Presença ]
```

***

### Registrar diálogo

Fluxo:

1. Selecionar assistência
2. POST /v1/dialogos
3. status → ATIVA

Regras:

* Apenas AGUARDANDO\_DIALOGO
* Libera novas presenças

UI:

```
REGISTRAR DIÁLOGO

Assistido:
João (Cartão 1234)

Status:
AGUARDANDO_DIALOGO

Observações:
[.........................]
[.........................]

[ Registrar Diálogo ]
```

***

# 9. APIs

Base: `/v1`

## Senhas

* POST /senhas
* GET /senhas/aguardando
* POST /senhas/chamar

## Assistidos

* GET /assistidos?busca=
* POST /assistidos

## Entrevistas

* POST /entrevistas

## Presenças

* POST /presencas

## Diálogos

* POST /dialogos

## Trabalhador

* GET /trabalhadores/me

***

# 10. Diretrizes UX

* Mobile-first
* Tablet-first
* Botões grandes (\~44px)
* Uma ação principal por tela

Prioridade:

* Velocidade > estética

***

# 11. Arquitetura

* Backend: Quarkus
* Frontend: Angular
* Banco: MongoDB
* Auth: Keycloak (JWT)

***

# 12. Evolução

## MVP

* Fila
* Presença

## Futuro

* Relatórios
* Notificações
* Multi-tenant

***

# 13. Observações Finais

Este documento é a fonte única de verdade do sistema.

Permite:

* Geração de backend
* Geração de frontend
* Geração de testes
* Uso por agentes de IA

```

---

# ✅ Resultado final (importante)

Agora você tem um doc que é:

✅ ótimo pra IDE  
✅ ótimo pra IA (structure-first)  
✅ ótimo pra Angular dev  
✅ sem redundância  
✅ sem perda de informação  

---

# 🚀 Próximo passo natural

Agora sim faz sentido você:

👉 jogar esse `.md` no projeto  
👉 usar isso como input pra gerar telas Angular  

Se quiser, próxima jogada:

> “gera o componente Angular da tela de presença baseado nisso”

Aí você entra no modo builder real 😄
```
