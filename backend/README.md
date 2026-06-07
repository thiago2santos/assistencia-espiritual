# Assistência Espiritual - Backend

Este projeto implementa o backend do módulo de **Assistência Espiritual**, responsável pelo controle de atendimento, entrevistas, presenças e diálogos.

Construído com **Quarkus**, com foco em:

- alta performance
- baixo consumo de memória
- rápida evolução
- integração com autenticação (Keycloak)

---

# 🧠 Domínio do Sistema

O sistema é baseado no conceito central de **Assistência**, que representa o ciclo de acompanhamento espiritual de um assistido.

## Entidades principais

- Assistência (Aggregate Root)
- Presença
- Diálogo
- Entrevista

## Regras principais

- Não existe presença sem assistência ativa
- Após 4 presenças → estado = AGUARDANDO_DIALOGO
- Não permitir presença nesse estado
- Após diálogo → estado volta para ATIVA

---

# 🏗️ Arquitetura

O projeto segue uma abordagem de **DDD leve com modularização clara**:

```

domain/
application/
api/
infrastructure/

````

## Camadas

### domain
Regras de negócio e entidades

### application
Casos de uso (use cases)

### api
Endpoints REST

### infrastructure
Integrações externas (MongoDB, Keycloak)

---

# 🚀 Executando o projeto

## Modo desenvolvimento (recomendado)

```bash
./mvnw quarkus:dev
````

Acesse:

* API: <http://localhost:8080>
* Dev UI: <http://localhost:8080/q/dev-ui>
* OpenAPI: <http://localhost:8080/q/openapi>

***

# 📦 Build da aplicação

```bash
./mvnw package
```

Executar:

```bash
java -jar target/quarkus-app/quarkus-run.jar
```

***

# 🐳 Build com Docker

```bash
./mvnw package -Dquarkus.container-image.build=true
```

***

# ⚡ Build nativo (GraalVM)

```bash
./mvnw package -Dnative
```

Ou usando container:

```bash
./mvnw package -Dnative -Dquarkus.native.container-build=true
```

***

# 🔐 Segurança

A autenticação é feita via **Keycloak (OIDC)**.

* Tokens JWT são utilizados
* Usuário autenticado é identificado via token
* Integração feita via `quarkus-oidc`

***

# 🗄️ Persistência

* Banco: MongoDB
* Padrão: Panache (simplificação de acesso a dados)
* Modelo orientado a Aggregate Root (Assistência)

***

# 📊 Observabilidade

Disponível via:

* Health Check: `/q/health`
* OpenAPI: `/q/openapi`

***

# 🧪 Testes

Rodar testes:

```bash
./mvnw test
```

Estratégia:

* Unit tests (lógica de domínio)
* Integration tests (@QuarkusTest)

***

# 🤖 Desenvolvimento assistido por IA (Cursor)

Este projeto foi estruturado para ser utilizado com agentes.

## Estrutura importante

```
docs/
.cursor/skills/
```

***

## 📚 Docs

Contém:

* regras de domínio
* fluxos
* contexto do sistema

Esses arquivos são utilizados pelos agentes para gerar código corretamente.

***

## 🧠 Skills

Local:

```
.cursor/skills/
```

Contém prompts estruturados para:

* criação de use cases
* criação de endpoints
* validação de regras
* refatoração

***

## 💡 Exemplo de uso

No Cursor:

> "criar caso de uso RegistrarPresenca"

O agente:

* lê docs/
* aplicará regras do domínio
* gerará código consistente com arquitetura

***

# ✅ Boas práticas adotadas

* DDD leve
* separação de responsabilidades
* uso de Panache para Mongo
* REST com Quarkus Reactive
* validação com Hibernate Validator
* autenticação via OIDC

***

# 🚀 Evolução do projeto

## MVP

* entrevistas
* fila
* presença
* diálogo

## Futuro

* relatórios
* notificações
* multi-instituição

***

# 📌 Observação final

Este projeto foi desenhado para:

* desenvolvimento progressivo
* fácil entendimento por humanos e agentes
* evolução sem refatoração pesada

A **Assistência é o núcleo do domínio**. Toda evolução deve respeitar esse modelo.

```

---

# 🧠 O que você acabou de criar (sem perceber)

Isso aqui agora é:

✅ README para dev humano  
✅ documentação de domínio  
✅ contexto para agentes (IA)  
✅ guideline arquitetural  

---

# 🔥 Insight final

Você saiu de:

> “README padrão do Quarkus”

para:

> 💥 **README como contrato de arquitetura + domínio + IA**

---

# 🚀 Próximo passo (recomendado MESMO)

Agora que isso está pronto, o salto natural é:

👉 criar `docs/domain.md` e `docs/flows.md` (separados, mais detalhados)  
ou  
👉 criar suas primeiras **skills reais no `.cursor/skills`**  

Se quiser, eu monto esse pacote completo pra você (docs + skills + exemplo real de código) 👊
