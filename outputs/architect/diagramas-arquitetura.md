# Diagramas de Arquitetura - Sistema de Estudos para Desenvolvedores

## 1. Visão Geral da Arquitetura

Este documento apresenta os diagramas de arquitetura do sistema, mostrando a estrutura geral, componentes principais, fluxos de dados e integrações.

**Tipo de Arquitetura**: Monolito Modular (Next.js Full-Stack)  
**Padrão**: Server-Side Rendering (SSR) + Static Site Generation (SSG)  
**Deployment**: Vercel (recomendado) ou similar

---

## 2. Diagrama de Arquitetura Geral

### 2.1 Visão de Alto Nível

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENTE                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │   Desktop    │  │    Tablet     │  │    Mobile    │     │
│  │   Browser    │  │   Browser    │  │   Browser    │     │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘     │
└─────────┼─────────────────┼─────────────────┼──────────────┘
          │                 │                 │
          └─────────────────┼─────────────────┘
                            │
                            │ HTTPS
                            │
┌───────────────────────────▼──────────────────────────────────┐
│                    NEXT.JS APPLICATION                      │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              FRONTEND (React + Chakra UI)            │  │
│  │  - Pages (SSR/SSG)                                   │  │
│  │  - Components                                        │  │
│  │  - Hooks                                             │  │
│  │  - State Management                                  │  │
│  └──────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              API ROUTES (Backend)                     │  │
│  │  - /api/subjects                                      │  │
│  │  - /api/subtopics                                     │  │
│  │  - /api/content                                       │  │
│  │  - /api/generate                                      │  │
│  └──────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              BUSINESS LOGIC                            │  │
│  │  - Services                                          │  │
│  │  - Validators                                        │  │
│  │  - Utilities                                         │  │
│  └──────────────────────────────────────────────────────┘  │
└───────────────────────────┬──────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        │                   │                   │
┌───────▼────────┐  ┌───────▼────────┐  ┌───────▼────────┐
│   NEON DB      │  │   AI API        │  │   CACHE        │
│  (PostgreSQL)  │  │  (OpenAI/etc)   │  │  (Redis/Vercel)│
│                │  │                 │  │                │
│  - Subjects    │  │  - Content Gen  │  │  - API Cache   │
│  - Subtopics   │  │  - Subtopics    │  │  - Static Cache │
│  - Content     │  │                 │  │                │
└────────────────┘  └─────────────────┘  └────────────────┘
```

### 2.2 Componentes Principais

**Frontend Layer:**
- Next.js Pages (App Router)
- React Components
- Chakra UI v3
- Client-side state management

**Backend Layer:**
- Next.js API Routes
- Business logic services
- Data validation
- Error handling

**Data Layer:**
- Prisma ORM
- Neon PostgreSQL
- Connection pooling

**External Services:**
- AI API (OpenAI/Anthropic)
- Cache layer (Vercel Edge/Redis)

---

## 3. Diagrama de Fluxo de Dados

### 3.1 Fluxo: Visualização de Assuntos

```
┌─────────┐
│ Cliente │
└────┬────┘
     │
     │ 1. GET /subjects
     │
┌────▼─────────────────────────────────────┐
│     Next.js Page (SSR/SSG)              │
│  ┌──────────────────────────────────┐   │
│  │  Page Component                  │   │
│  │  - getServerSideProps()          │   │
│  │  - ou getStaticProps()           │   │
│  └────┬─────────────────────────────┘   │
│       │                                  │
│       │ 2. Call API Route                │
│       │                                  │
│  ┌────▼─────────────────────────────┐   │
│  │  API Route: /api/subjects         │   │
│  │  - Validate request               │   │
│  │  - Call service                   │   │
│  └────┬─────────────────────────────┘   │
│       │                                  │
│       │ 3. Query Database                │
│       │                                  │
│  ┌────▼─────────────────────────────┐   │
│  │  Prisma Service                   │   │
│  │  - prisma.subject.findMany()      │   │
│  └────┬─────────────────────────────┘   │
└───────┼──────────────────────────────────┘
        │
        │ 4. Query
        │
┌───────▼────────┐
│  Neon Database │
│  (PostgreSQL)  │
└───────┬────────┘
        │
        │ 5. Return Data
        │
┌───────▼──────────────────────────────────┐
│  Response: JSON Array of Subjects        │
│  [{id, name, description, ...}]          │
└───────┬──────────────────────────────────┘
        │
        │ 6. Render
        │
┌───────▼────────┐
│  React Render  │
│  + Chakra UI   │
└───────┬────────┘
        │
        │ 7. HTML Response
        │
┌───────▼────────┐
│    Cliente     │
└────────────────┘
```

### 3.2 Fluxo: Geração de Conteúdo via IA

```
┌─────────┐
│ Cliente │
└────┬────┘
     │
     │ 1. POST /api/generate
     │    { subjectId, subtopicId }
     │
┌────▼─────────────────────────────────────┐
│     API Route: /api/generate            │
│  ┌──────────────────────────────────┐   │
│  │  1. Validate request             │   │
│  │  2. Check cache                  │   │
│  │  3. Check if content exists       │   │
│  └────┬─────────────────────────────┘   │
│       │                                  │
│       │ Cache Miss?                      │
│       │                                  │
│  ┌────▼─────────────────────────────┐   │
│  │  AI Service                       │   │
│  │  - Build prompt                   │   │
│  │  - Call AI API                    │   │
│  │  - Process response               │   │
│  └────┬─────────────────────────────┘   │
│       │                                  │
│       │ 2. API Call                     │
│       │                                  │
│  ┌────▼─────────────────────────────┐   │
│  │  External AI API                  │   │
│  │  (OpenAI/Anthropic)               │   │
│  └────┬─────────────────────────────┘   │
│       │                                  │
│       │ 3. Generated Content            │
│       │                                  │
│  ┌────▼─────────────────────────────┐   │
│  │  Save to Database                 │   │
│  │  - prisma.content.create()        │   │
│  └────┬─────────────────────────────┘   │
│       │                                  │
│       │ 4. Save                         │
│       │                                  │
│  ┌────▼─────────────────────────────┐   │
│  │  Update Cache                     │   │
│  │  - Set cache key                  │   │
│  └────┬─────────────────────────────┘   │
└───────┼──────────────────────────────────┘
        │
        │ 5. Return Content
        │
┌───────▼────────┐
│    Cliente     │
│  (Streaming)   │
└────────────────┘
```

---

## 4. Diagrama de Componentes

### 4.1 Estrutura de Componentes Frontend

```
┌─────────────────────────────────────────────┐
│           APP LAYOUT                         │
│  ┌───────────────────────────────────────┐  │
│  │         HEADER                        │  │
│  │  - Logo                               │  │
│  │  - Navigation                         │  │
│  │  - Search (future)                    │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │         MAIN CONTENT                  │  │
│  │  ┌─────────────────────────────────┐ │  │
│  │  │   PAGE COMPONENTS                │ │  │
│  │  │   - HomePage                     │ │  │
│  │  │   - SubjectPage                  │ │  │
│  │  │   - ContentPage                  │ │  │
│  │  └─────────────────────────────────┘ │  │
│  │  ┌─────────────────────────────────┐ │  │
│  │  │   SHARED COMPONENTS              │ │  │
│  │  │   - SubjectCard                  │ │  │
│  │  │   - SubtopicList                 │ │  │
│  │  │   - ContentViewer                │ │  │
│  │  │   - Breadcrumbs                  │ │  │
│  │  │   - LoadingSkeleton              │ │  │
│  │  │   - ErrorMessage                 │ │  │
│  │  └─────────────────────────────────┘ │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │         SIDEBAR (Desktop)            │  │
│  │  - Navigation                        │  │
│  │  - Subtopics List                   │  │
│  │  - Progress Indicator                │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │         FOOTER                        │  │
│  └───────────────────────────────────────┘  │
└─────────────────────────────────────────────┘
```

### 4.2 Estrutura de Componentes Backend

```
┌─────────────────────────────────────────────┐
│         API ROUTES LAYER                    │
│  ┌───────────────────────────────────────┐ │
│  │  /api/subjects                         │ │
│  │  - GET: List all subjects              │ │
│  │  - GET /[id]: Get subject              │ │
│  └───────────────────────────────────────┘ │
│  ┌───────────────────────────────────────┐ │
│  │  /api/subtopics                        │ │
│  │  - GET: List by subject                │ │
│  │  - GET /[id]: Get subtopic             │ │
│  └───────────────────────────────────────┘ │
│  ┌───────────────────────────────────────┐ │
│  │  /api/content                          │ │
│  │  - GET /[id]: Get content              │ │
│  │  - POST: Create content                │ │
│  └───────────────────────────────────────┘ │
│  ┌───────────────────────────────────────┐ │
│  │  /api/generate                         │ │
│  │  - POST: Generate content via AI      │ │
│  └───────────────────────────────────────┘ │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│         SERVICES LAYER                       │
│  ┌───────────────────────────────────────┐  │
│  │  SubjectService                       │  │
│  │  - getAllSubjects()                   │  │
│  │  - getSubjectById()                   │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │  SubtopicService                       │  │
│  │  - getSubtopicsBySubject()             │  │
│  │  - getSubtopicById()                   │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │  ContentService                        │  │
│  │  - getContentBySubtopic()              │  │
│  │  - createContent()                     │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │  AIService                             │  │
│  │  - generateContent()                     │  │
│  │  - generateSubtopics()                 │  │
│  └───────────────────────────────────────┘  │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│         DATA ACCESS LAYER                    │
│  ┌───────────────────────────────────────┐  │
│  │  Prisma Client                        │  │
│  │  - Database queries                   │  │
│  │  - Transactions                       │  │
│  │  - Migrations                         │  │
│  └───────────────────────────────────────┘  │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│         NEON DATABASE                        │
│  (PostgreSQL)                                │
└──────────────────────────────────────────────┘
```

---

## 5. Diagrama de Sequência

### 5.1 Sequência: Carregar Página de Assunto

```
Cliente    Next.js Page    API Route    Service    Prisma    Database
   │            │              │           │          │          │
   │──GET /js──>│              │           │          │          │
   │            │              │           │          │          │
   │            │──GET /api───>│           │          │          │
   │            │  /subjects/1 │           │          │          │
   │            │              │           │          │          │
   │            │              │──getById()│          │          │
   │            │              │           │          │          │
   │            │              │           │──findUnique()──>│
   │            │              │           │          │          │
   │            │              │           │          │<──Data───│
   │            │              │           │<──Result──│          │
   │            │              │<──Subject──│          │          │
   │            │<──JSON───────│           │          │          │
   │<──HTML──────│              │           │          │          │
   │            │              │           │          │          │
```

### 5.2 Sequência: Gerar Conteúdo via IA

```
Cliente    API Route    AIService    AI API    Service    Prisma    Database    Cache
   │           │            │          │          │          │          │          │
   │──POST─────>│           │          │          │          │          │          │
   │/api/generate│           │          │          │          │          │          │
   │           │           │          │          │          │          │          │
   │           │──checkCache()───────────────>│          │          │          │
   │           │           │          │          │          │          │<──Miss───│
   │           │           │          │          │          │          │          │
   │           │──generate()│          │          │          │          │          │
   │           │           │          │          │          │          │          │
   │           │           │──API Call──────────>│          │          │          │
   │           │           │          │          │          │          │          │
   │           │           │          │<──Content│          │          │          │
   │           │           │<──Content│          │          │          │          │
   │           │           │          │          │          │          │          │
   │           │──save()───>│          │          │          │          │          │
   │           │           │          │          │          │          │          │
   │           │           │          │          │──create()──>│          │          │
   │           │           │          │          │          │          │          │
   │           │           │          │          │          │<──Saved───│          │
   │           │           │          │          │<──Result──│          │          │
   │           │           │          │          │          │          │          │
   │           │──setCache()──────────────────────────────────────────>│
   │           │           │          │          │          │          │          │
   │<──Content──│           │          │          │          │          │          │
   │           │           │          │          │          │          │          │
```

---

## 6. Diagrama de Deploy

### 6.1 Arquitetura de Deploy (Vercel)

```
┌─────────────────────────────────────────────┐
│         VERCEL PLATFORM                     │
│  ┌───────────────────────────────────────┐ │
│  │  EDGE NETWORK (CDN)                   │ │
│  │  - Static assets                      │ │
│  │  - Cached pages                       │ │
│  └───────────────────────────────────────┘ │
│  ┌───────────────────────────────────────┐ │
│  │  SERVERLESS FUNCTIONS                  │ │
│  │  - API Routes                          │ │
│  │  - SSR Pages                           │ │
│  │  - ISR Pages                           │ │
│  └───────────────────────────────────────┘ │
└───────────────────┬─────────────────────────┘
                    │
        ┌───────────┼───────────┐
        │           │           │
┌───────▼──────┐ ┌─▼────────┐ ┌▼──────────┐
│  NEON DB     │ │ AI API   │ │  CACHE    │
│  (PostgreSQL)│ │ (External)│ │ (Vercel)  │
└──────────────┘ └───────────┘ └───────────┘
```

### 6.2 Fluxo de CI/CD

```
┌─────────────┐
│  Git Push   │
└──────┬──────┘
       │
       │ Trigger
       │
┌──────▼──────────────────┐
│  Vercel CI/CD           │
│  ┌──────────────────┐   │
│  │ 1. Install Deps  │   │
│  │ 2. Run Tests     │   │
│  │ 3. Build         │   │
│  │ 4. Deploy        │   │
│  └──────────────────┘   │
└──────┬──────────────────┘
       │
       │ Deploy
       │
┌──────▼──────────┐
│  Production     │
│  Environment    │
└─────────────────┘
```

---

## 7. Diagrama de Segurança

### 7.1 Camadas de Segurança

```
┌─────────────────────────────────────────────┐
│         CLIENT LAYER                       │
│  - HTTPS Only                               │
│  - CSP Headers                              │
│  - XSS Protection                           │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│         API LAYER                            │
│  - Rate Limiting                             │
│  - Input Validation                          │
│  - Authentication (future)                   │
│  - CORS Configuration                        │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│         DATABASE LAYER                       │
│  - Prisma ORM (SQL Injection Protection)    │
│  - Connection Pooling                        │
│  - Encrypted Connections (SSL)               │
│  - Parameterized Queries                    │
└─────────────────────────────────────────────┘
```

---

## 8. Diagrama de Escalabilidade

### 8.1 Estratégias de Escalabilidade

```
┌─────────────────────────────────────────────┐
│         HORIZONTAL SCALING                  │
│  - Serverless Functions (Auto-scale)         │
│  - Edge Caching                              │
│  - CDN Distribution                          │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│         CACHING STRATEGY                    │
│  ┌───────────────────────────────────────┐ │
│  │  Static Pages (ISR)                   │ │
│  │  - Revalidate: 1 hour                 │ │
│  └───────────────────────────────────────┘ │
│  ┌───────────────────────────────────────┐ │
│  │  API Responses                        │ │
│  │  - Cache: 5 minutes                   │ │
│  └───────────────────────────────────────┘ │
│  ┌───────────────────────────────────────┐ │
│  │  Generated Content                    │ │
│  │  - Cache: 24 hours                   │ │
│  └───────────────────────────────────────┘ │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│         DATABASE OPTIMIZATION             │
│  - Indexes on frequently queried fields   │
│  - Connection pooling                     │
│  - Query optimization                     │
│  - Read replicas (future)                 │
└───────────────────────────────────────────┘
```

---

## 9. Notações Utilizadas

### 9.1 Símbolos

- **Retângulo**: Componente/Processo
- **Cilindro**: Banco de Dados
- **Nuvem**: Serviço Externo
- **Seta**: Fluxo de Dados
- **Linha Pontilhada**: Comunicação Assíncrona

### 9.2 Convenções

- Fluxo de cima para baixo (quando aplicável)
- Esquerda para direita (quando aplicável)
- Labels nas setas indicam ação/tipo de dados
- Agrupamentos indicam camadas lógicas

---

## 10. Próximos Passos

1. **Validação**: Revisar diagramas com equipe técnica
2. **Refinamento**: Ajustar baseado em feedback
3. **Implementação**: Usar como guia para desenvolvimento
4. **Atualização**: Manter diagramas atualizados com código

---

**Versão**: 1.0  
**Data**: [Data atual]  
**Autor**: Arquiteto de Software  
**Status**: Proposta Inicial

