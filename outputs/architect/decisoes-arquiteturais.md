# Decisões Arquiteturais (ADRs) - Sistema de Estudos para Desenvolvedores

## 1. Introdução

Este documento registra as decisões arquiteturais importantes do projeto, seguindo o formato ADR (Architecture Decision Record). Cada ADR documenta o contexto, a decisão tomada e suas consequências.

**Formato ADR:**
- **Status**: Proposta | Aceita | Rejeitada | Deprecada
- **Contexto**: Situação que levou à decisão
- **Decisão**: Solução escolhida
- **Consequências**: Impactos positivos e negativos

---

## ADR-001: Escolha do Next.js como Framework Principal

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, Tech Lead

### Contexto

Precisávamos escolher um framework full-stack que permitisse:
- Desenvolvimento rápido do MVP
- SSR/SSG para performance
- API Routes integradas
- TypeScript nativo
- Boa experiência de desenvolvimento
- Comunidade ativa e suporte

### Decisão

Escolhemos **Next.js 14+ com App Router** como framework principal.

**Justificativas:**
1. **Full-Stack**: Permite frontend e backend no mesmo projeto
2. **API Routes**: Backend integrado sem necessidade de servidor separado
3. **SSR/SSG/ISR**: Múltiplas estratégias de renderização
4. **TypeScript**: Suporte nativo e excelente
5. **Ecosystem**: Grande comunidade e ecossistema
6. **Vercel**: Deploy otimizado (mas não obrigatório)
7. **Performance**: Otimizações built-in

### Consequências

**Positivas:**
- ✅ Desenvolvimento mais rápido
- ✅ Menos complexidade (monorepo)
- ✅ Melhor performance out-of-the-box
- ✅ Fácil deploy no Vercel
- ✅ Boa documentação e comunidade

**Negativas:**
- ⚠️ Vendor lock-in parcial (mas pode rodar em outros lugares)
- ⚠️ Curva de aprendizado para App Router (novo)
- ⚠️ Menos flexibilidade que arquitetura separada

**Alternativas Consideradas:**
- **Remix**: Similar, mas menor comunidade
- **SvelteKit**: Boa performance, mas menos maduro
- **Separar Frontend/Backend**: Mais complexidade, não necessário para MVP

---

## ADR-002: Uso do Prisma como ORM

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, Backend Dev

### Contexto

Precisávamos de uma solução para:
- Type-safe database access
- Migrations gerenciadas
- Boa DX (Developer Experience)
- Suporte a PostgreSQL
- Schema management

### Decisão

Escolhemos **Prisma 5+** como ORM.

**Justificativas:**
1. **Type Safety**: Gera tipos TypeScript automaticamente
2. **Migrations**: Sistema robusto de migrations
3. **DX**: Excelente experiência de desenvolvimento
4. **Schema First**: Schema como fonte da verdade
5. **Query Builder**: Sintaxe intuitiva
6. **Comunidade**: Grande comunidade e suporte

### Consequências

**Positivas:**
- ✅ Type safety end-to-end
- ✅ Migrations automáticas
- ✅ IntelliSense excelente
- ✅ Proteção contra SQL injection
- ✅ Fácil manutenção do schema

**Negativas:**
- ⚠️ Overhead em queries muito complexas
- ⚠️ Curva de aprendizado inicial
- ⚠️ Pode ser overkill para queries simples

**Alternativas Consideradas:**
- **TypeORM**: Mais features, mas mais complexo
- **Drizzle**: Mais leve, mas menos features
- **SQL Raw**: Mais controle, mas menos segurança

---

## ADR-003: Neon PostgreSQL como Banco de Dados

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, DevOps

### Contexto

Precisávamos de um banco de dados que:
- Suporte PostgreSQL (requisito do Prisma)
- Seja serverless/gerenciado
- Escale automaticamente
- Tenha bom custo-benefício
- Permita branching para dev/staging

### Decisão

Escolhemos **Neon PostgreSQL** como banco de dados.

**Justificativas:**
1. **Serverless**: Não precisa gerenciar servidor
2. **Auto-scaling**: Escala automaticamente
3. **Branching**: Permite branches de database (dev/staging)
4. **Custo**: Free tier generoso, pricing justo
5. **Performance**: Boa performance e latência
6. **PostgreSQL**: Versão moderna do PostgreSQL

### Consequências

**Positivas:**
- ✅ Sem gerenciamento de servidor
- ✅ Escalabilidade automática
- ✅ Branching de database (dev/staging)
- ✅ Free tier para começar
- ✅ Boa integração com Vercel

**Negativas:**
- ⚠️ Vendor lock-in (mas pode migrar)
- ⚠️ Cold starts em serverless (minimizado)
- ⚠️ Menos controle que self-hosted

**Alternativas Consideradas:**
- **Supabase**: Similar, mas mais features (não precisamos)
- **PlanetScale**: MySQL, não PostgreSQL
- **AWS RDS**: Mais complexo, mais caro
- **Self-hosted**: Muito trabalho para MVP

---

## ADR-004: Chakra UI v3 como Framework de UI

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, Frontend Dev, UX Designer

### Contexto

Precisávamos de um framework de UI que:
- Seja acessível (WCAG AA)
- Tenha componentes prontos
- Seja customizável
- Tenha boa performance
- Suporte TypeScript
- Seja requisito do projeto (definido pelo PO)

### Decisão

Escolhemos **Chakra UI v3** como framework de UI.

**Justificativas:**
1. **Acessibilidade**: Componentes acessíveis por padrão
2. **Design System**: Sistema de design consistente
3. **Customização**: Fácil customizar tema
4. **Performance**: Boa performance
5. **TypeScript**: Suporte completo
6. **Requisito**: Definido pelo Product Owner

### Consequências

**Positivas:**
- ✅ Desenvolvimento rápido de UI
- ✅ Acessibilidade built-in
- ✅ Consistência visual
- ✅ Boa documentação
- ✅ Comunidade ativa

**Negativas:**
- ⚠️ Bundle size maior que soluções mais leves
- ⚠️ Curva de aprendizado (v3 é relativamente novo)
- ⚠️ Menos flexibilidade que CSS puro

**Alternativas Consideradas:**
- **Tailwind CSS**: Mais flexível, mas mais trabalho
- **Material UI**: Mais pesado, design diferente
- **Radix UI**: Mais leve, mas menos componentes

---

## ADR-005: API de IA Externa para Geração de Conteúdo

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, Product Owner

### Contexto

Precisávamos gerar conteúdo educacional de forma:
- Automatizada
- Atualizada
- Escalável
- Com boa qualidade

Opções: construir modelo próprio ou usar API externa.

### Decisão

Escolhemos usar **API externa de IA** (OpenAI GPT-4 ou Anthropic Claude).

**Justificativas:**
1. **Rapidez**: Implementação rápida
2. **Qualidade**: Modelos state-of-the-art
3. **Custo**: Pay-per-use, sem infraestrutura
4. **Manutenção**: Sem necessidade de treinar/manter modelo
5. **Escalabilidade**: Escala automaticamente

### Consequências

**Positivas:**
- ✅ Implementação rápida
- ✅ Alta qualidade de conteúdo
- ✅ Sem infraestrutura de ML
- ✅ Atualizações automáticas (novos modelos)
- ✅ Flexibilidade (pode trocar provider)

**Negativas:**
- ⚠️ Custo pode ser alto com muitos usuários
- ⚠️ Dependência externa
- ⚠️ Latência de API
- ⚠️ Rate limits
- ⚠️ Privacidade de dados (enviar para API externa)

**Mitigações:**
- Cache agressivo de conteúdo gerado
- Rate limiting por usuário
- Monitoramento de custos
- Considerar modelo próprio no futuro se escalar

**Alternativas Consideradas:**
- **Modelo Próprio**: Muito trabalho, custo alto de infra
- **Fine-tuning**: Interessante, mas complexo para MVP
- **Open Source Models**: Qualidade menor, precisa de infra

---

## ADR-006: Arquitetura Monolítica Modular vs Microserviços

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, Tech Lead

### Contexto

Precisávamos decidir entre:
- Monolito (tudo em um projeto)
- Microserviços (serviços separados)

Considerando: tamanho da equipe, complexidade, velocidade de desenvolvimento.

### Decisão

Escolhemos **Arquitetura Monolítica Modular** (Next.js full-stack).

**Justificativas:**
1. **Equipe Pequena**: Equipe pequena, monolito mais fácil
2. **MVP**: Foco em velocidade, não escalabilidade prematura
3. **Complexidade**: Microserviços adicionam complexidade desnecessária
4. **Deploy**: Deploy único é mais simples
5. **Debugging**: Mais fácil debugar monolito
6. **Custo**: Menor custo de infraestrutura

### Consequências

**Positivas:**
- ✅ Desenvolvimento mais rápido
- ✅ Deploy mais simples
- ✅ Debugging mais fácil
- ✅ Menor complexidade operacional
- ✅ Menor custo inicial

**Negativas:**
- ⚠️ Escalabilidade limitada (mas suficiente para MVP)
- ⚠️ Acoplamento maior
- ⚠️ Deploy único (mas pode usar feature flags)

**Evolução Futura:**
- Se necessário, pode evoluir para microserviços
- Next.js permite separar em monorepo
- Pode extrair serviços quando necessário

**Alternativas Consideradas:**
- **Microserviços**: Over-engineering para MVP
- **Serverless Functions**: Interessante, mas complexidade adicional

---

## ADR-007: Estratégia de Cache

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, Backend Dev

### Contexto

Precisávamos definir estratégia de cache para:
- Melhorar performance
- Reduzir custos de API de IA
- Reduzir carga no banco de dados

### Decisão

Estratégia de cache em múltiplas camadas:

1. **Static Pages (ISR)**: Revalidate 1 hora
2. **API Responses**: Cache 5 minutos (Vercel Edge)
3. **Generated Content**: Cache 24 horas (database + edge)
4. **AI API Responses**: Cache indefinido (já salvo no DB)

**Justificativas:**
1. **ISR**: Next.js built-in, excelente para conteúdo estático
2. **API Cache**: Reduz carga no backend
3. **Content Cache**: Conteúdo gerado não muda frequentemente
4. **AI Cache**: Evita regenerar conteúdo já gerado

### Consequências

**Positivas:**
- ✅ Melhor performance
- ✅ Menor custo de API
- ✅ Menor carga no banco
- ✅ Melhor experiência do usuário

**Negativas:**
- ⚠️ Complexidade de invalidação
- ⚠️ Possível conteúdo desatualizado (mitigado com revalidate)
- ⚠️ Mais pontos de falha

**Implementação:**
- Usar Vercel Edge Cache
- Cache keys bem definidas
- Estratégia de invalidação clara

---

## ADR-008: TypeScript vs JavaScript

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, Tech Lead

### Contexto

Precisávamos escolher entre TypeScript e JavaScript para o projeto.

### Decisão

Escolhemos **TypeScript** como linguagem principal.

**Justificativas:**
1. **Type Safety**: Previne erros em tempo de compilação
2. **DX**: Melhor IntelliSense e autocomplete
3. **Refactoring**: Mais seguro refatorar
4. **Documentação**: Types servem como documentação
5. **Padrão**: Prisma gera tipos, faz sentido usar TS
6. **Comunidade**: Padrão da indústria

### Consequências

**Positivas:**
- ✅ Menos bugs em produção
- ✅ Melhor experiência de desenvolvimento
- ✅ Refactoring mais seguro
- ✅ Documentação implícita

**Negativas:**
- ⚠️ Curva de aprendizado (mas vale a pena)
- ⚠️ Build time ligeiramente maior
- ⚠️ Mais verboso que JS

**Alternativas Consideradas:**
- **JavaScript**: Mais rápido para começar, mas mais erros

---

## ADR-009: Estratégia de Deploy

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, DevOps

### Contexto

Precisávamos escolher plataforma de deploy considerando:
- Facilidade de setup
- Custo
- Performance
- Integração com Next.js

### Decisão

Escolhemos **Vercel** como plataforma principal de deploy.

**Justificativas:**
1. **Otimizado para Next.js**: Criado pelos mesmos desenvolvedores
2. **Zero Config**: Deploy automático
3. **Performance**: Edge network global
4. **Custo**: Free tier generoso
5. **CI/CD**: Integração com Git
6. **Preview Deployments**: Deploys de PR automaticamente

### Consequências

**Positivas:**
- ✅ Deploy muito simples
- ✅ Performance excelente
- ✅ Preview deployments
- ✅ Integração perfeita com Next.js
- ✅ Free tier para começar

**Negativas:**
- ⚠️ Vendor lock-in (mas pode migrar)
- ⚠️ Menos controle que self-hosted
- ⚠️ Custo pode aumentar com escala

**Alternativas Consideradas:**
- **AWS/Azure/GCP**: Mais controle, mais complexo
- **Self-hosted**: Muito trabalho para MVP
- **Netlify**: Similar, mas Vercel melhor para Next.js

---

## ADR-010: Estrutura de Código Modular

**Status**: ✅ Aceita  
**Data**: [Data atual]  
**Decisores**: Arquiteto, Tech Lead

### Contexto

Precisávamos definir estrutura de código que:
- Seja escalável
- Seja fácil de navegar
- Separe responsabilidades
- Facilite manutenção

### Decisão

Estrutura modular baseada em features e responsabilidades:

```
src/
├── app/          # Next.js App Router (routes)
├── components/   # React components (por feature)
├── services/     # Business logic
├── lib/          # Utilities e configs
├── types/        # TypeScript types
└── hooks/        # Custom React hooks
```

**Justificativas:**
1. **Separação de Responsabilidades**: Cada pasta tem propósito claro
2. **Escalabilidade**: Fácil adicionar novas features
3. **Manutenibilidade**: Fácil encontrar código
4. **Padrão**: Segue convenções do Next.js

### Consequências

**Positivas:**
- ✅ Código organizado
- ✅ Fácil navegação
- ✅ Escalável
- ✅ Manutenível

**Negativas:**
- ⚠️ Pode ser over-engineering inicialmente
- ⚠️ Requer disciplina da equipe

---

## 11. Resumo das Decisões

| ADR | Decisão | Status |
|-----|---------|--------|
| ADR-001 | Next.js como Framework | ✅ Aceita |
| ADR-002 | Prisma como ORM | ✅ Aceita |
| ADR-003 | Neon PostgreSQL | ✅ Aceita |
| ADR-004 | Chakra UI v3 | ✅ Aceita |
| ADR-005 | API de IA Externa | ✅ Aceita |
| ADR-006 | Monolito Modular | ✅ Aceita |
| ADR-007 | Cache Multi-camada | ✅ Aceita |
| ADR-008 | TypeScript | ✅ Aceita |
| ADR-009 | Vercel para Deploy | ✅ Aceita |
| ADR-010 | Estrutura Modular | ✅ Aceita |

---

## 12. Próximas Decisões a Tomar

1. **Autenticação**: NextAuth.js vs Auth0 vs Custom
2. **Monitoramento**: Sentry vs Datadog vs Custom
3. **Analytics**: Google Analytics vs Plausible vs Custom
4. **Testing Strategy**: Jest + RTL vs Vitest
5. **CI/CD**: GitHub Actions vs Vercel built-in

---

**Versão**: 1.0  
**Data**: [Data atual]  
**Autor**: Arquiteto de Software  
**Status**: Decisões Iniciais

