# API REST de Transações

Uma aplicação RESTful para gerenciamento de transações financeiras, construída com Fastify e TypeScript.

## 📋 Descrição

Esta API permite criar, listar e gerenciar transações financeiras mantendo um histórico separado por sessão de usuário. O sistema utiliza cookies para identificar sessões únicas e registra transações de crédito e débito.

## 🚀 Tecnologias

- **Node.js** - Runtime JavaScript server-side
- **TypeScript** - Linguagem tipada para JavaScript
- **Fastify** - Framework web rápido e eficiente
- **Knex** - Query builder SQL e gerenciamento de migrações
- **PostgreSQL/SQLite** - Banco de dados relacional
- **Zod** - Validação de schemas
- **Vitest** - Framework de testes unitários
- **Biome** - Formatador e linter de código

## 📦 Pré-requisitos

- Node.js >= 18
- npm ou yarn
- PostgreSQL (opcional - SQLite disponível para desenvolvimento)

## 🔧 Instalação

1. Clone o repositório:
```bash
git clone <repository-url>
cd API\ REST
```

2. Instale as dependências:
```bash
npm install
```

3. Configure as variáveis de ambiente criando um arquivo `.env`:
```env
DATABASE_CLIENT=sqlite
DATABASE_URL=./db.sqlite
NODE_ENV=development
```

Para usar PostgreSQL:
```env
DATABASE_CLIENT=pg
DATABASE_URL=postgresql://usuario:senha@localhost:5432/api_transacoes
NODE_ENV=development
```

4. Execute as migrações:
```bash
npm run knex migrate:latest
```

## 📚 Scripts Disponíveis

| Comando | Descrição |
|---------|-----------|
| `npm run dev` | Inicia o servidor em modo desenvolvimento com hot-reload |
| `npm run build` | Compila o projeto TypeScript para JavaScript |
| `npm test` | Executa a suite de testes |
| `npm run format` | Formata o código usando Biome |
| `npm run knex` | Acesso direto ao CLI do Knex para migrações |

## 🔌 Endpoints da API

### GET `/transactions`
Lista todas as transações da sessão atual.

**Headers:** Cookie `sessionId`

**Resposta (200):**
```json
{
  "transactions": [
    {
      "id": "uuid",
      "title": "Salário",
      "amount": 5000,
      "session_id": "uuid",
      "created_at": "2026-03-09T10:00:00Z"
    }
  ]
}
```

### GET `/transactions/:id`
Obtém uma transação específica.

**Headers:** Cookie `sessionId`

**Resposta (200):**
```json
{
  "transaction": {
    "id": "uuid",
    "title": "Salário",
    "amount": 5000,
    "session_id": "uuid",
    "created_at": "2026-03-09T10:00:00Z"
  }
}
```

### GET `/transactions/summary`
Retorna o resumo (soma) das transações da sessão.

**Headers:** Cookie `sessionId`

**Resposta (200):**
```json
{
  "summary": {
    "amount": 5000
  }
}
```

### POST `/transactions`
Cria uma nova transação.

**Body:**
```json
{
  "title": "Salário",
  "amount": 5000,
  "type": "credit"
}
```

**Parâmetros:**
- `title` (string, obrigatório): Descrição da transação
- `amount` (number, obrigatório): Valor da transação em centavos/unidades
- `type` (enum: "credit" | "debit", obrigatório): Tipo da transação

**Resposta (201):** Sem corpo

> **Nota:** Se não existir `sessionId` nos cookies, um novo será criado e definido como um cookie com validade de 7 dias.

## 🧪 Testes

Execute a suite de testes com:
```bash
npm test
```

Os testes estão localizados em `test/` e cobrem os endpoints e funcionalidades da API.

## 🔐 Autenticação

A API utiliza um sistema de sessão baseado em cookies:
- Cada requisição requer um `sessionId` válido (exceto POST de nova transação)
- Se não houver sessão, uma nova é criada automaticamente na primeira transação
- O cookie tem validade de 7 dias

## 🛠️ Desenvolvimento

Para começar a desenvolver:

1. Inicie o servidor em modo watch:
```bash
npm run dev
```

2. O servidor estará disponível em `http://localhost:3000`

3. Para formatar o código:
```bash
npm run format
```

## 📝 Migrações

Criar uma nova migração:
```bash
npm run knex migrate:make <nome_da_migracao>
```

Aplicar migrações:
```bash
npm run knex migrate:latest
```

Reverter última migração:
```bash
npm run knex migrate:rollback
```
