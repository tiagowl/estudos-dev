# Documentação Técnica - Sistema de Estudos para Desenvolvedores

## 1. Visão Geral

Esta documentação técnica descreve a arquitetura, stack tecnológica, estrutura de código e padrões de desenvolvimento do Sistema de Estudos para Desenvolvedores.

**Versão**: 1.0  
**Última Atualização**: [Data atual]  
**Arquiteto**: [Nome]

---

## 2. Stack Tecnológica

### 2.1 Frontend

**Framework:**
- **Next.js 14+** (App Router)
  - Server-Side Rendering (SSR)
  - Static Site Generation (SSG)
  - Incremental Static Regeneration (ISR)
  - API Routes

**UI Framework:**
- **Chakra UI v3**
  - Componentes acessíveis
  - Sistema de design consistente
  - Responsividade built-in

**Linguagem:**
- **TypeScript 5+**
  - Type safety
  - Melhor DX (Developer Experience)
  - IntelliSense

**Gerenciamento de Estado:**
- **React Hooks** (useState, useEffect, useContext)
- **SWR** ou **React Query** (para data fetching)
- Context API (para estado global)

---

### 2.2 Backend

**Runtime:**
- **Node.js 18+** (via Next.js API Routes)

**ORM:**
- **Prisma 5+**
  - Type-safe database client
  - Migrations
  - Schema management

**Validação:**
- **Zod**
  - Schema validation
  - Type inference
  - Runtime validation

**Autenticação (Futuro):**
- **NextAuth.js** ou **Auth0**

---

### 2.3 Banco de Dados

**Database:**
- **Neon PostgreSQL**
  - Serverless PostgreSQL
  - Auto-scaling
  - Branching para dev/staging

**Versão PostgreSQL:**
- 15+ (recomendado)

---

### 2.4 Serviços Externos

**IA para Geração de Conteúdo:**
- **OpenAI GPT-4** (recomendado) ou
- **Anthropic Claude** (alternativa)
- **API Key** via environment variables

**Cache:**
- **Vercel Edge Cache** (built-in)
- **Redis** (opcional, para cache distribuído)

**Deploy:**
- **Vercel** (recomendado) ou
- **AWS/Azure/GCP** (alternativas)

---

## 3. Estrutura de Diretórios

### 3.1 Estrutura Proposta

```
estudos-dev/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── (routes)/           # Route groups
│   │   │   ├── page.tsx        # Home page
│   │   │   ├── subjects/
│   │   │   │   ├── [id]/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   └── subtopics/
│   │   │   │   │       └── [subtopicId]/
│   │   │   │   │           └── page.tsx
│   │   │   │   └── layout.tsx
│   │   │   └── layout.tsx
│   │   ├── api/                # API Routes
│   │   │   ├── subjects/
│   │   │   │   ├── route.ts
│   │   │   │   └── [id]/
│   │   │   │       └── route.ts
│   │   │   ├── subtopics/
│   │   │   │   └── route.ts
│   │   │   ├── content/
│   │   │   │   └── route.ts
│   │   │   └── generate/
│   │   │       └── route.ts
│   │   ├── layout.tsx          # Root layout
│   │   └── globals.css
│   ├── components/             # React Components
│   │   ├── ui/                 # Chakra UI components
│   │   ├── layout/             # Layout components
│   │   │   ├── Header.tsx
│   │   │   ├── Footer.tsx
│   │   │   └── Sidebar.tsx
│   │   ├── subjects/           # Subject-related
│   │   │   ├── SubjectCard.tsx
│   │   │   └── SubjectList.tsx
│   │   ├── subtopics/          # Subtopic-related
│   │   │   ├── SubtopicList.tsx
│   │   │   └── SubtopicItem.tsx
│   │   ├── content/            # Content-related
│   │   │   ├── ContentViewer.tsx
│   │   │   └── CodeBlock.tsx
│   │   └── common/             # Shared components
│   │       ├── Breadcrumbs.tsx
│   │       ├── LoadingSkeleton.tsx
│   │       └── ErrorMessage.tsx
│   ├── lib/                    # Utilities & Config
│   │   ├── prisma.ts           # Prisma client
│   │   ├── ai/                 # AI service
│   │   │   └── client.ts
│   │   ├── cache.ts             # Cache utilities
│   │   └── utils.ts            # Helper functions
│   ├── services/               # Business Logic
│   │   ├── subject.service.ts
│   │   ├── subtopic.service.ts
│   │   ├── content.service.ts
│   │   └── ai.service.ts
│   ├── types/                  # TypeScript types
│   │   ├── subject.types.ts
│   │   ├── subtopic.types.ts
│   │   └── content.types.ts
│   └── hooks/                  # Custom React hooks
│       ├── useSubjects.ts
│       ├── useSubtopics.ts
│       └── useContent.ts
├── prisma/
│   ├── schema.prisma           # Database schema
│   └── migrations/             # Migration files
├── public/                     # Static assets
│   ├── images/
│   └── icons/
├── .env.local                  # Environment variables
├── .env.example                # Example env file
├── next.config.js              # Next.js config
├── tsconfig.json               # TypeScript config
├── package.json
└── README.md
```

---

## 4. Schema do Banco de Dados

### 4.1 Prisma Schema

```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model Subject {
  id          String     @id @default(cuid())
  name        String     @unique
  slug        String     @unique
  description String?
  category    String?
  icon        String?
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
  
  subtopics   Subtopic[]
  
  @@index([slug])
  @@index([category])
}

model Subtopic {
  id          String     @id @default(cuid())
  subjectId   String
  name        String
  slug        String
  order       Int        @default(0)
  description String?
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
  
  subject     Subject    @relation(fields: [subjectId], references: [id], onDelete: Cascade)
  content     Content?
  
  @@unique([subjectId, slug])
  @@index([subjectId])
  @@index([subjectId, order])
}

model Content {
  id          String     @id @default(cuid())
  subtopicId  String     @unique
  text        String     @db.Text
  codeExamples Json?     // Array of code examples
  metadata    Json?      // Additional metadata
  generatedAt DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
  isGenerated Boolean    @default(false)
  
  subtopic    Subtopic   @relation(fields: [subtopicId], references: [id], onDelete: Cascade)
  
  @@index([subtopicId])
}
```

### 4.2 Relacionamentos

- **Subject** 1:N **Subtopic**
- **Subtopic** 1:1 **Content**

### 4.3 Índices

- `Subject.slug` - Para busca rápida
- `Subject.category` - Para filtros
- `Subtopic.subjectId` - Para queries por assunto
- `Subtopic(subjectId, order)` - Para ordenação
- `Content.subtopicId` - Para busca de conteúdo

---

## 5. API Routes

### 5.1 Estrutura de API Routes

**Convenção:**
- RESTful design
- JSON responses
- Error handling padronizado
- Validação de input

### 5.2 Endpoints

#### GET /api/subjects
**Descrição**: Lista todos os assuntos disponíveis

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "slug": "string",
      "description": "string",
      "category": "string"
    }
  ],
  "meta": {
    "total": 0
  }
}
```

#### GET /api/subjects/[id]
**Descrição**: Obtém um assunto específico

**Response:**
```json
{
  "data": {
    "id": "string",
    "name": "string",
    "slug": "string",
    "description": "string",
    "category": "string",
    "subtopics": []
  }
}
```

#### GET /api/subtopics?subjectId=[id]
**Descrição**: Lista subtópicos de um assunto

**Query Params:**
- `subjectId` (required): ID do assunto

**Response:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "slug": "string",
      "order": 0,
      "description": "string"
    }
  ]
}
```

#### GET /api/content/[subtopicId]
**Descrição**: Obtém conteúdo de um subtópico

**Response:**
```json
{
  "data": {
    "id": "string",
    "text": "string",
    "codeExamples": [],
    "metadata": {},
    "generatedAt": "ISO date"
  }
}
```

#### POST /api/generate
**Descrição**: Gera conteúdo via IA

**Body:**
```json
{
  "subjectId": "string",
  "subtopicId": "string"
}
```

**Response:**
```json
{
  "data": {
    "id": "string",
    "text": "string",
    "codeExamples": [],
    "generatedAt": "ISO date"
  }
}
```

### 5.3 Error Handling

**Formato Padrão de Erro:**
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable message",
    "details": {}
  }
}
```

**Códigos de Erro:**
- `VALIDATION_ERROR`: Erro de validação
- `NOT_FOUND`: Recurso não encontrado
- `INTERNAL_ERROR`: Erro interno do servidor
- `RATE_LIMIT_EXCEEDED`: Limite de requisições excedido
- `AI_SERVICE_ERROR`: Erro na API de IA

---

## 6. Serviços (Business Logic)

### 6.1 Subject Service

```typescript
// src/services/subject.service.ts

import { prisma } from '@/lib/prisma';

export class SubjectService {
  static async getAll() {
    return prisma.subject.findMany({
      orderBy: { name: 'asc' },
    });
  }

  static async getById(id: string) {
    return prisma.subject.findUnique({
      where: { id },
      include: {
        subtopics: {
          orderBy: { order: 'asc' },
        },
      },
    });
  }

  static async getBySlug(slug: string) {
    return prisma.subject.findUnique({
      where: { slug },
      include: {
        subtopics: {
          orderBy: { order: 'asc' },
        },
      },
    });
  }
}
```

### 6.2 Content Service

```typescript
// src/services/content.service.ts

import { prisma } from '@/lib/prisma';
import { AIService } from './ai.service';

export class ContentService {
  static async getBySubtopicId(subtopicId: string) {
    return prisma.content.findUnique({
      where: { subtopicId },
    });
  }

  static async generateContent(
    subjectId: string,
    subtopicId: string
  ) {
    // 1. Get subject and subtopic
    const subject = await prisma.subject.findUnique({
      where: { id: subjectId },
    });
    const subtopic = await prisma.subtopic.findUnique({
      where: { id: subtopicId },
    });

    if (!subject || !subtopic) {
      throw new Error('Subject or subtopic not found');
    }

    // 2. Generate content via AI
    const generatedContent = await AIService.generateContent({
      subject: subject.name,
      subtopic: subtopic.name,
    });

    // 3. Save to database
    const content = await prisma.content.upsert({
      where: { subtopicId },
      update: {
        text: generatedContent.text,
        codeExamples: generatedContent.codeExamples,
        metadata: generatedContent.metadata,
        isGenerated: true,
        updatedAt: new Date(),
      },
      create: {
        subtopicId,
        text: generatedContent.text,
        codeExamples: generatedContent.codeExamples,
        metadata: generatedContent.metadata,
        isGenerated: true,
      },
    });

    return content;
  }
}
```

### 6.3 AI Service

```typescript
// src/services/ai.service.ts

import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

export class AIService {
  static async generateContent(params: {
    subject: string;
    subtopic: string;
  }) {
    const prompt = this.buildPrompt(params.subject, params.subtopic);

    const response = await openai.chat.completions.create({
      model: 'gpt-4',
      messages: [
        {
          role: 'system',
          content: 'You are an expert programming educator...',
        },
        {
          role: 'user',
          content: prompt,
        },
      ],
      temperature: 0.7,
      max_tokens: 2000,
    });

    return this.parseResponse(response.choices[0].message.content);
  }

  private static buildPrompt(subject: string, subtopic: string): string {
    return `Create educational content about ${subtopic} in ${subject}...`;
  }

  private static parseResponse(content: string) {
    // Parse AI response into structured format
    return {
      text: content,
      codeExamples: [],
      metadata: {},
    };
  }
}
```

---

## 7. Configuração e Variáveis de Ambiente

### 7.1 Arquivo .env.example

```env
# Database
DATABASE_URL="postgresql://user:password@host:port/database?sslmode=require"

# AI Service
OPENAI_API_KEY="sk-..."
# or
ANTHROPIC_API_KEY="sk-ant-..."

# Next.js
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# Cache (optional)
REDIS_URL="redis://localhost:6379"
```

### 7.2 next.config.js

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
  
  // ISR Configuration
  experimental: {
    isrMemoryCacheSize: 50 * 1024 * 1024, // 50MB
  },
  
  // Headers for security
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'X-XSS-Protection',
            value: '1; mode=block',
          },
        ],
      },
    ];
  },
};

module.exports = nextConfig;
```

---

## 8. Performance e Otimização

### 8.1 Estratégias de Cache

**Static Pages (ISR):**
```typescript
// app/subjects/[id]/page.tsx
export const revalidate = 3600; // 1 hour

export async function generateStaticParams() {
  const subjects = await SubjectService.getAll();
  return subjects.map((subject) => ({
    id: subject.id,
  }));
}
```

**API Cache:**
```typescript
// app/api/subjects/route.ts
export async function GET() {
  const cacheKey = 'subjects:all';
  // Check cache
  // Return cached or fetch fresh
}
```

### 8.2 Otimizações de Banco

- **Connection Pooling**: Prisma gerencia automaticamente
- **Indexes**: Definidos no schema
- **Query Optimization**: Usar `select` para campos específicos
- **Pagination**: Para listas grandes

### 8.3 Otimizações Frontend

- **Code Splitting**: Automático com Next.js
- **Image Optimization**: Next.js Image component
- **Font Optimization**: Next.js Font optimization
- **Lazy Loading**: Componentes e rotas

---

## 9. Segurança

### 9.1 Medidas Implementadas

- **HTTPS Only**: Enforce SSL/TLS
- **Input Validation**: Zod schemas
- **SQL Injection Protection**: Prisma ORM
- **XSS Protection**: React escaping automático
- **CORS**: Configurado adequadamente
- **Rate Limiting**: Para APIs públicas
- **Environment Variables**: Secrets não expostos

### 9.2 Headers de Segurança

- Content-Security-Policy
- X-Content-Type-Options
- X-Frame-Options
- X-XSS-Protection
- Strict-Transport-Security

---

## 10. Testes

### 10.1 Estratégia de Testes

**Unit Tests:**
- Services
- Utilities
- Helpers

**Integration Tests:**
- API Routes
- Database operations

**E2E Tests:**
- Fluxos principais
- User journeys

### 10.2 Ferramentas

- **Jest**: Test runner
- **React Testing Library**: Component tests
- **Playwright**: E2E tests
- **Prisma Test Utils**: Database tests

---

## 11. Monitoramento e Logging

### 11.1 Logging

- **Structured Logging**: JSON format
- **Log Levels**: Error, Warn, Info, Debug
- **Error Tracking**: Sentry (recomendado)

### 11.2 Métricas

- **Performance**: Web Vitals
- **Errors**: Error rates
- **Usage**: API usage, page views
- **Database**: Query performance

---

## 12. Deploy

### 12.1 Vercel (Recomendado)

1. Conectar repositório Git
2. Configurar environment variables
3. Deploy automático em push
4. Preview deployments para PRs

### 12.2 Checklist de Deploy

- [ ] Environment variables configuradas
- [ ] Database migrations executadas
- [ ] Build passa sem erros
- [ ] Testes passam
- [ ] Performance OK
- [ ] Monitoramento ativo

---

## 13. Próximos Passos

1. **Setup Inicial**: Configurar projeto base
2. **Database**: Criar schema e migrations
3. **API Routes**: Implementar endpoints básicos
4. **Frontend**: Criar páginas principais
5. **AI Integration**: Integrar API de IA
6. **Testing**: Configurar testes
7. **Deploy**: Deploy em staging/production

---

**Versão**: 1.0  
**Data**: [Data atual]  
**Autor**: Arquiteto de Software  
**Status**: Documentação Inicial

