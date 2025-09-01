# 🟢 Node.js Development

Setup completo e workflows modernos para desenvolvimento Node.js com foco em performance e best practices.

## 📦 **Package Management & Setup**

- **[Node.js Configuration](node.md)** - Setup completo do ambiente Node.js moderno

## ⚡ **Modern JavaScript Stack**

### Essential Tools
- **Node.js LTS** com version management (nvm/fnm)
- **pnpm** para gestão de pacotes eficiente
- **TypeScript** para type safety
- **ESLint + Prettier** para code quality
- **Vitest** para testing moderno

### Project Setup
```bash
# Inicializar projeto moderno
npm init -y
npm install -D typescript @types/node
npm install -D eslint prettier vitest
npx tsc --init
```

## 🏗️ **Development Workflow**

### Package Manager Comparison
| Tool | Speed | Disk Usage | Monorepos | Lock File |
|------|-------|------------|-----------|-----------|
| **pnpm** | ⚡⚡⚡ | 🟢 Minimal | ✅ | ✅ |
| npm | ⚡⚡ | 🟡 Medium | ✅ | ✅ |
| yarn | ⚡⚡ | 🟡 Medium | ✅ | ✅ |

### Modern Scripts
```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "test": "vitest",
    "lint": "eslint . --fix",
    "format": "prettier --write ."
  }
}
```

## 🚀 **Performance & Optimization**

### Runtime Alternatives
- **Bun** - Runtime ultra-rápido com bundler integrado
- **Deno** - Runtime seguro com TypeScript nativo
- **Node.js** - Runtime maduro e estável

### Development Tools
- **tsx** para execução TypeScript direta
- **nodemon** para hot reloading
- **concurrently** para executar múltiplos comandos

## 📊 **Backend Frameworks**

### Popular Choices
- **Express.js** - Minimalista e flexível
- **Fastify** - Performance-focused
- **Nest.js** - Enterprise-grade com decorators
- **Hapi.js** - Configuration-centric

### API Development
```javascript
// Fastify example - high performance
const fastify = require('fastify')({ logger: true })

fastify.get('/', async (request, reply) => {
  return { hello: 'world' }
})

const start = async () => {
  await fastify.listen({ port: 3000 })
}
start()
```

## 🧪 **Testing Strategy**

- **Vitest** - Testing framework ultra-rápido
- **Jest** - Testing framework popular
- **Supertest** para API testing
- **Playwright** para E2E testing

---

> ⚡ **Performance Tip:** Usa pnpm para gestão de pacotes e tsx para desenvolvimento TypeScript direto!