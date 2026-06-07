# Sistema de Controle de Presença - CEOV

## Casos de Uso Detalhados (Base para Evolução)

---

## 1. Cadastro no Sistema

### Objetivo
Permitir que um trabalhador crie sua conta de acesso ao sistema.

### Ator
- Trabalhador

### Pré-condições
- Nenhuma (acesso inicial)

### Fluxo principal
1. Trabalhador acessa tela de cadastro
2. Informa dados necessários
3. Sistema cria usuário no Keycloak
4. Sistema confirma cadastro

### Tela: Cadastro

#### Contexto
- Baixa frequência
- Usuário comum

#### Prioridade
- simplicidade > velocidade

#### Layout
- formulário central
- botão de ação ao final

#### Componentes
- input nome
- input email
- input senha
- botão cadastrar

#### Ações
- preencher formulário
- submeter cadastro

#### Regras
- validação de campos obrigatórios

#### Estilo
- simples

---

## 2. Login

### Objetivo
Autenticar trabalhador no sistema

### Ator
- Trabalhador

### Fluxo principal
1. Trabalhador acessa sistema
2. Sistema redireciona para Keycloak
3. Trabalhador autentica
4. Retorna autenticado

### Tela: Login

#### Contexto
- executado frequentemente

#### Prioridade
- confiabilidade

#### Layout
- externo (Keycloak)

#### Observações
- gerenciado pelo Keycloak

---

## 3. Atribuição de Roles

### Objetivo
Permitir gerenciar permissões

### Ator
- Admin

### Tela

#### Layout
- lista de usuários
- seleção de roles

#### Componentes
- tabela usuários
- seletor de roles

#### Regras
- apenas admin pode acessar

---

## 4. Emissão de Senha

### Objetivo
Gerar senha de atendimento

### Ator
- Trabalhador

### Tela

#### Contexto
- ambiente movimentado

#### Prioridade
- velocidade

#### Layout
- botão principal em destaque

#### Componentes
- botão gerar senha
- lista de senhas

#### Regras
- trabalhador autenticado

---

## 5. Entrevista e Criação da Assistência

### Objetivo
Registrar entrevista inicial e iniciar assistência

### Ator
- Trabalhador

### Tela

#### Layout
- formulário

#### Componentes
- dados assistido
- observações

#### Regras
- associar trabalhador

---

## 6. Registro de Presença

### Objetivo
Registrar presença rapidamente

### Ator
- Trabalhador

### Pré-condições
- trabalhador autenticado

### Fluxo principal
1. Buscar assistido
2. Selecionar assistido
3. Registrar presença

### Tela: Registro de Presença

#### Contexto
- ambiente movimentado
- uso frequente

#### Prioridade
- velocidade de uso > estética

#### Layout
- topo: busca
- meio: lista
- rodapé: ação

#### Componentes
- input busca
- lista resultados
- botão registrar

#### Ações
- buscar
- selecionar
- registrar

#### Regras
- não registrar sem seleção
- associar trabalhador

#### Estilo
- simples

---

## Observação
Todos os casos são base inicial e devem ser refinados posteriormente.
