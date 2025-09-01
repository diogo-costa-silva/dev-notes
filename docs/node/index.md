# 🟢 Node.js Development

Setup completo e workflows modernos para desenvolvimento Node.js com foco em performance e best practices.

## 📦 **Package Management & Setup**

### Node.js Installation
```bash
# Install Node.js via Homebrew (recommended)
brew install node

# Verify installation
node --version
npm --version

# Alternative: Use nvm for version management
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
nvm install --lts
nvm use --lts
```

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

# Alternative: Use pnpm (recommended)
pnpm init
pnpm add -D typescript @types/node eslint prettier vitest
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

### Express.js Quick Setup
```bash
# Install Express with TypeScript
pnpm add express
pnpm add -D @types/express nodemon tsx

# Basic server structure
mkdir src
touch src/index.ts
```

```typescript
// src/index.ts
import express from 'express';

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(express.json());

// Routes
app.get('/', (req, res) => {
  res.json({ message: 'Hello, Node.js!' });
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

## 🧪 **Testing Strategy**

### Testing Setup
```bash
# Install testing dependencies
pnpm add -D vitest supertest @types/supertest

# Basic test setup
mkdir tests
touch tests/api.test.ts
```

```typescript
// tests/api.test.ts
import { describe, it, expect } from 'vitest';
import request from 'supertest';
import app from '../src/app';

describe('API Endpoints', () => {
  it('should return hello message', async () => {
    const response = await request(app)
      .get('/')
      .expect(200);
    
    expect(response.body.message).toBe('Hello, Node.js!');
  });
});
```

## 🚀 **Production Deployment**

### Docker Setup
```dockerfile
FROM node:18-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./
COPY pnpm-lock.yaml ./

# Install pnpm and dependencies
RUN npm install -g pnpm
RUN pnpm install --frozen-lockfile

# Copy source code
COPY . .

# Build application
RUN pnpm build

EXPOSE 3000

CMD ["node", "dist/index.js"]
```

### Environment Configuration
```bash
# .env
NODE_ENV=development
PORT=3000
DATABASE_URL=postgresql://user:password@localhost:5432/mydb
```

---

## 📋 **Best Practices Checklist**

### ✅ **Development**
- Use **TypeScript** para type safety
- Configure **ESLint + Prettier** para code quality
- Setup **hot reloading** com nodemon/tsx
- Implement **proper error handling**
- Use **environment variables** para configuration

### ✅ **Performance**
- Choose **pnpm** para faster installs
- Use **clustering** for multi-core utilization
- Implement **caching strategies**
- Optimize **database queries**
- Use **compression middleware**

### ✅ **Security**
- Validate **input data** sempre
- Use **helmet** para security headers
- Implement **rate limiting**
- Keep **dependencies updated**
- Never commit **secrets** to version control

---

> ⚡ **Performance Tip:** Usa pnpm para gestão de pacotes e tsx para desenvolvimento TypeScript direto. Para production, considera Fastify se performance é crítica!