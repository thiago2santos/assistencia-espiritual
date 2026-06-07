# Sistema de Controle de Presença - CEOV

## 1. Visão Geral
Sistema para gestão de atendimento espiritual incluindo recepção, entrevistas, acompanhamento e presença.

## 2. Fluxo de Atendimento
```mermaid
flowchart TD
A[Assistido chega] --> B[Recepção registra interesse]
B --> C[Gera senha]
C --> D[Fila de atendimento]
D --> E[Entrevistador chama senha]
E --> F[Realiza entrevista]
F --> G[Cria assistido]
G --> H[Inicia assistência]
H --> I[Participa de sessões]
I --> J[Registro de presença]
J --> K{A cada 4 presenças}
K -->|Sim| L[Agendar diálogo]
K -->|Não| I
L --> M[Realiza diálogo]
M --> I
```

## 3. Contextos de Domínio

### Recepção
- Geração de senha
- Controle de fila

### Atendimento Espiritual
- Entrevista
- Assistido
- Presenças
- Diálogos

## 4. Modelo de Dados (Visão ER)
```mermaid
erDiagram
RECEPCIONISTA ||--o{ SENHA : gera
SENHA ||--o| ENTREVISTA : origina
ENTREVISTA ||--|| ASSISTIDO : cria
ASSISTIDO ||--o{ ASSISTENCIA : possui
ASSISTENCIA ||--o{ PRESENCA : registra
ASSISTENCIA ||--o{ DIALOGO : possui
```

## 5. Estrutura das Entidades (Classe)
```mermaid
classDiagram

class Trabalhador {
  id
  nome
  keycloakId
}

class Senha {
  id
  numero
  status
  tipo
  criadoEm
}

class Entrevista {
  id
  data
  observacoes
  realizadoPor
}

class Assistido {
  id
  nome
  idade
  preferencial
}

class Assistencia {
  id
}

class Presenca {
  id
  data
  sessaoId
  registradoPor
}

class Dialogo {
  id
  data
  observacoes
}

Trabalhador --> Senha : gera
Trabalhador --> Presenca : registra
Trabalhador --> Entrevista : realiza

Senha "1" --> "0..1" Entrevista : origina
Entrevista "1" --> "1" Assistido : cria
Assistido "1" --> "0..*" Assistencia : possui
Assistencia "1" --> "0..*" Presenca : registra
Assistencia "1" --> "0..*" Dialogo : possui
```

## 6. APIs

### Recepção
- POST /senhas
- GET /senhas/aguardando
- POST /senhas/chamar

### Entrevista
- POST /entrevistas

### Presença
- POST /presencas

## 7. Arquitetura
- Backend: Quarkus
- Frontend: Angular + PrimeNG
- Banco: MongoDB
- Auth: Keycloak

## 8. Segurança
- Autenticação via Keycloak (OIDC)
- RBAC via roles no Keycloak
- Backend valida JWT

## 9. Observabilidade
- Logs estruturados
- OpenTelemetry
- Prometheus + Grafana

## 10. Diferenciais
- Fila digital
- QR Code para presença
- Métricas de atendimento

## 11. Roadmap Produto

### MVP
- Cadastro
- Fila
- Presença

### Evolução
- Relatórios
- Notificações
- Multi-tenant

## 12. Comercialização
- SaaS multi-tenant
- Plano gratuito limitado
- Plano pago por instituição

## 13. Casos de Uso (a detalhar)
- Cadastro no sistema  
  - Trabalhador realiza cadastro criando credenciais de acesso  

- Login  
  - Trabalhador autentica antes de utilizar o sistema  

- Atribuição / atualização de roles  
  - Trabalhador com role admin gerencia permissões  

- Emissão de senha  
  - Trabalhador autenticado gera senha de atendimento  

- Entrevista e criação da assistência  
  - Trabalhador realiza entrevista e cria assistência  

- Registro de presença  
  - Trabalhador registra presença do assistido  
