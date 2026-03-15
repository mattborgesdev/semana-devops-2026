# AGENTS.md - Diretrizes para Codificação Agentica neste Repositório

## Visão Geral do Projeto

Este é o projeto **Semana DevOps Map Brasil** - um aplicativo interativo mostrando participantes DevOps pelo Brasil. A aplicação principal é uma API Node.js/Express com frontend em JavaScript puro.

**Stack Tecnológico:**
- Node.js com Express (backend)
- JavaScript puro + CSS (frontend)
- Docker & Kubernetes (deploy)
- node:test para testes

---

## Comandos de Build, Lint e Test

Todos os comandos devem ser executados no diretório `app/`:

```bash
# Desenvolvimento (com auto-reload)
npm run dev

# Iniciar produção
npm start

# Executar todos os testes
npm test

# Executar um único arquivo de teste
node --test test/app.test.js

# Lint (placeholder - adicionar eslint para produção)
npm run lint
```

---

## Diretrizes de Estilo de Código

### Princípios Gerais
- Mantenha o código simples e legível - este é um projeto demo focado em conceitos Docker/K8s
- Use CommonJS (`require`) - sem módulos ES
- Sem TypeScript - apenas JavaScript puro
- Use indentação com 4 espaços

### Estrutura de Arquivos
```
app/
├── server.js       # Aplicação Express principal
├── package.json    # Dependências
├── public/
│   ├── index.html  # Frontend
│   └── style.css  # Estilos
└── test/
    └── app.test.js # Testes
```

### Convenções de Nomenclatura
- **Arquivos**: kebab-case (`server.js`, `app.test.js`)
- **Variáveis/Constantes**: camelCase (`participantes`, `baseUrl`)
- **Funções**: camelCase (`startTestServer`, `fetch`)
- **Rotas**: minúsculas com barras (`/api/participante`, `/healthz`)

### Imports
```javascript
// Recursos nativos do Node
const path = require('path');
const http = require('node:http');

// Terceiros
const express = require('express');

// Módulos locais
const app = require('../server');
```

### Tratamento de Erros
- Use validação inline com retornos antecipados
- Retorne códigos de status HTTP adequados (400 para requisição inválida, 201 para criado)
- Inclua mensagens de erro na resposta JSON:
  ```javascript
  if (!nome || !estado) {
      return res.status(400).json({
          error: 'Campos obrigatórios: nome, estado, cargo',
      });
  }
  ```

### Rotas de API
- Use convenções RESTful
- Health check em `/healthz` (obrigatório para probes do K8s)
- Todos os endpoints retornam JSON
- Use prefixo `_req` para parâmetros não utilizados:
  ```javascript
  app.get('/api/participantes', (_req, res) => { ... })
  ```

### Comentários
- Use separadores de seção com linhas `---`:
  ```javascript
  // ---------------------------------------------------------------------------
  // Armazenamento in-memory
  // ---------------------------------------------------------------------------
  ```
- Mantenha comentários em Português/Inglês conforme apropriado ao contexto
- Documente o propósito, não detalhes de implementação

### Testes (node:test)
- Use `describe` e `it` do `node:test`
- Use `assert` para assertions
- Use casos de teste `setup` e `cleanup`
- Funções helper para requisições HTTP:
  ```javascript
  function fetch(url, options = {}) { ... }
  ```

### Diretrizes de CSS
- Propriedades customizadas de CSS para theming (veja `:root` no style.css)
- Nomenclatura estilo BEM: `.card__title`, `.stat-card--total`
- Use variáveis CSS para cores: `var(--accent)`, `var(--bg)`
- Responsivo mobile-first com queries `@media`

### Contexto Docker & Kubernetes
- Armazene nome do pod nas respostas: `process.env.HOSTNAME`
- Endpoints de health devem retornar 200 para probes do K8s
- Escute em `0.0.0.0` (não localhost) para rede de containers

---

## Tarefas Comuns

### Adicionando um novo endpoint de API
1. Adicione o handler de rota em `server.js` com validação
2. Adicione o teste correspondente em `test/app.test.js`
3. Teste com `npm test`

### Modificando o frontend
1. Edite `public/index.html` ou `public/style.css`
2. Não há step de build - arquivos estáticos servidos diretamente

### Executando localmente
```bash
cd app
npm run dev
# App roda em http://localhost:3000
```

---

## Contexto do Projeto

Esta é uma aplicação de ensino/demo da **LINUXtips** (Jeferson Fernando) para o evento Semana DevOps. O foco está em conceitos de containerização e orquestração, não em padrões de código de produção.
