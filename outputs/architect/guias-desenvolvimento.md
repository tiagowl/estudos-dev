# Guias de Desenvolvimento - Sistema de Estudos para Desenvolvedores

## 1. Introdução

Este documento contém guias e padrões de desenvolvimento para o projeto, garantindo consistência, qualidade e manutenibilidade do código.

**Público-Alvo**: Desenvolvedores da equipe  
**Objetivo**: Padronizar práticas de desenvolvimento  
**Última Atualização**: [Data atual]

---

## 2. Setup do Ambiente de Desenvolvimento

### 2.1 Pré-requisitos

- **Node.js**: 18.x ou superior
- **npm**: 9.x ou superior (ou yarn/pnpm)
- **Git**: Última versão
- **Editor**: VS Code (recomendado) com extensões:
  - ESLint
  - Prettier
  - Prisma
  - TypeScript

### 2.2 Instalação

```bash
# 1. Clone o repositório
git clone [repository-url]
cd estudos-dev

# 2. Instale dependências
npm install

# 3. Configure variáveis de ambiente
cp .env.example .env.local
# Edite .env.local com suas credenciais

# 4. Configure o banco de dados
npx prisma generate
npx prisma migrate dev

# 5. Inicie o servidor de desenvolvimento
npm run dev
```

### 2.3 Scripts Disponíveis

```json
{
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint",
  "type-check": "tsc --noEmit",
  "db:generate": "prisma generate",
  "db:migrate": "prisma migrate dev",
  "db:studio": "prisma studio",
  "test": "jest",
  "test:watch": "jest --watch"
}
```

---

## 3. Convenções de Código

### 3.1 Nomenclatura

**Arquivos e Diretórios:**
- **Componentes**: PascalCase (`SubjectCard.tsx`)
- **Hooks**: camelCase com prefixo `use` (`useSubjects.ts`)
- **Services**: camelCase com sufixo `.service.ts` (`subject.service.ts`)
- **Types**: camelCase com sufixo `.types.ts` (`subject.types.ts`)
- **Utils**: camelCase com sufixo `.ts` (`utils.ts`)
- **Constantes**: UPPER_SNAKE_CASE (`API_BASE_URL`)

**Variáveis e Funções:**
- **Variáveis**: camelCase (`const subjectName = 'JavaScript'`)
- **Funções**: camelCase (`function getSubjectById()`)
- **Componentes**: PascalCase (`function SubjectCard()`)
- **Constantes**: UPPER_SNAKE_CASE (`const MAX_RETRIES = 3`)

**Types e Interfaces:**
- **Types**: PascalCase (`type Subject = {...}`)
- **Interfaces**: PascalCase com prefixo `I` opcional (`interface ISubject`)

### 3.2 Estrutura de Arquivos

**Componente React:**
```typescript
// src/components/subjects/SubjectCard.tsx

import { Card, CardBody, Heading, Text } from '@chakra-ui/react';
import type { Subject } from '@/types/subject.types';

interface SubjectCardProps {
  subject: Subject;
  onClick?: (subject: Subject) => void;
}

export function SubjectCard({ subject, onClick }: SubjectCardProps) {
  return (
    <Card onClick={() => onClick?.(subject)}>
      <CardBody>
        <Heading size="md">{subject.name}</Heading>
        <Text>{subject.description}</Text>
      </CardBody>
    </Card>
  );
}
```

**Service:**
```typescript
// src/services/subject.service.ts

import { prisma } from '@/lib/prisma';
import type { Subject } from '@/types/subject.types';

export class SubjectService {
  static async getAll(): Promise<Subject[]> {
    return prisma.subject.findMany({
      orderBy: { name: 'asc' },
    });
  }

  static async getById(id: string): Promise<Subject | null> {
    return prisma.subject.findUnique({
      where: { id },
    });
  }
}
```

**API Route:**
```typescript
// src/app/api/subjects/route.ts

import { NextRequest, NextResponse } from 'next/server';
import { SubjectService } from '@/services/subject.service';
import { z } from 'zod';

export async function GET(request: NextRequest) {
  try {
    const subjects = await SubjectService.getAll();
    
    return NextResponse.json({
      data: subjects,
      meta: {
        total: subjects.length,
      },
    });
  } catch (error) {
    return NextResponse.json(
      {
        error: {
          code: 'INTERNAL_ERROR',
          message: 'Failed to fetch subjects',
        },
      },
      { status: 500 }
    );
  }
}
```

---

## 4. Padrões de Design

### 4.1 Componentes React

**Regras:**
- Use functional components
- Use TypeScript para props
- Extraia lógica complexa para hooks customizados
- Mantenha componentes pequenos e focados
- Use Chakra UI components quando possível

**Exemplo:**
```typescript
// ✅ Bom
export function SubjectList({ subjects }: SubjectListProps) {
  if (subjects.length === 0) {
    return <EmptyState message="No subjects found" />;
  }

  return (
    <Stack spacing={4}>
      {subjects.map((subject) => (
        <SubjectCard key={subject.id} subject={subject} />
      ))}
    </Stack>
  );
}

// ❌ Evitar
export function SubjectList({ subjects }: SubjectListProps) {
  return (
    <div>
      {subjects.length === 0 ? (
        <div>No subjects</div>
      ) : (
        subjects.map((subject) => (
          <div key={subject.id}>{subject.name}</div>
        ))
      )}
    </div>
  );
}
```

### 4.2 Hooks Customizados

**Padrão:**
```typescript
// src/hooks/useSubjects.ts

import { useSWR } from 'swr';
import type { Subject } from '@/types/subject.types';

const fetcher = (url: string) => fetch(url).then((res) => res.json());

export function useSubjects() {
  const { data, error, isLoading } = useSWR<{ data: Subject[] }>(
    '/api/subjects',
    fetcher
  );

  return {
    subjects: data?.data ?? [],
    isLoading,
    isError: error,
  };
}
```

### 4.3 Error Handling

**Padrão:**
```typescript
// ✅ Bom
try {
  const result = await someAsyncOperation();
  return NextResponse.json({ data: result });
} catch (error) {
  console.error('Error:', error);
  
  if (error instanceof ValidationError) {
    return NextResponse.json(
      { error: { code: 'VALIDATION_ERROR', message: error.message } },
      { status: 400 }
    );
  }

  return NextResponse.json(
    { error: { code: 'INTERNAL_ERROR', message: 'Something went wrong' } },
    { status: 500 }
  );
}

// ❌ Evitar
try {
  const result = await someAsyncOperation();
  return NextResponse.json({ data: result });
} catch (error) {
  return NextResponse.json({ error: 'Error' }, { status: 500 });
}
```

### 4.4 Validação de Dados

**Usar Zod:**
```typescript
import { z } from 'zod';

const CreateContentSchema = z.object({
  subjectId: z.string().min(1),
  subtopicId: z.string().min(1),
});

export async function POST(request: NextRequest) {
  const body = await request.json();
  
  const validation = CreateContentSchema.safeParse(body);
  if (!validation.success) {
    return NextResponse.json(
      {
        error: {
          code: 'VALIDATION_ERROR',
          message: 'Invalid request data',
          details: validation.error.errors,
        },
      },
      { status: 400 }
    );
  }

  const { subjectId, subtopicId } = validation.data;
  // ... continue
}
```

---

## 5. Estrutura de Pastas Detalhada

### 5.1 Componentes

```
src/components/
├── ui/              # Componentes base do Chakra UI (wrappers)
├── layout/          # Componentes de layout
│   ├── Header.tsx
│   ├── Footer.tsx
│   └── Sidebar.tsx
├── subjects/        # Componentes relacionados a assuntos
│   ├── SubjectCard.tsx
│   ├── SubjectList.tsx
│   └── SubjectGrid.tsx
├── subtopics/       # Componentes relacionados a subtópicos
│   ├── SubtopicList.tsx
│   └── SubtopicItem.tsx
├── content/         # Componentes relacionados a conteúdo
│   ├── ContentViewer.tsx
│   └── CodeBlock.tsx
└── common/          # Componentes compartilhados
    ├── Breadcrumbs.tsx
    ├── LoadingSkeleton.tsx
    └── ErrorMessage.tsx
```

### 5.2 Services

```
src/services/
├── subject.service.ts
├── subtopic.service.ts
├── content.service.ts
└── ai.service.ts
```

**Padrão de Service:**
```typescript
export class ServiceName {
  // Métodos estáticos
  static async methodName(params: Type): Promise<ReturnType> {
    // Implementation
  }
}
```

### 5.3 Types

```
src/types/
├── subject.types.ts
├── subtopic.types.ts
├── content.types.ts
└── api.types.ts
```

**Padrão de Types:**
```typescript
// src/types/subject.types.ts

export type Subject = {
  id: string;
  name: string;
  slug: string;
  description: string | null;
  category: string | null;
  createdAt: Date;
  updatedAt: Date;
};

export type SubjectWithSubtopics = Subject & {
  subtopics: Subtopic[];
};
```

---

## 6. Git Workflow

### 6.1 Branching Strategy

**Branches:**
- `main`: Produção (protegida)
- `develop`: Desenvolvimento (protegida)
- `feature/*`: Features novas
- `fix/*`: Correções de bugs
- `hotfix/*`: Correções urgentes em produção

**Exemplo:**
```bash
# Criar feature branch
git checkout -b feature/add-search

# Fazer commits
git commit -m "feat: add search functionality"

# Push
git push origin feature/add-search

# Criar PR para develop
```

### 6.2 Commit Messages

**Formato:**
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: Nova feature
- `fix`: Correção de bug
- `docs`: Documentação
- `style`: Formatação
- `refactor`: Refatoração
- `test`: Testes
- `chore`: Tarefas de manutenção

**Exemplos:**
```bash
feat(subjects): add subject filtering
fix(content): resolve content loading issue
docs(readme): update setup instructions
refactor(services): simplify subject service
```

---

## 7. Testes

### 7.1 Estratégia de Testes

**Pirâmide de Testes:**
- **Unit Tests**: Services, utils, helpers (70%)
- **Integration Tests**: API routes, database (20%)
- **E2E Tests**: Fluxos principais (10%)

### 7.2 Exemplo de Teste Unitário

```typescript
// src/services/__tests__/subject.service.test.ts

import { SubjectService } from '../subject.service';
import { prisma } from '@/lib/prisma';

jest.mock('@/lib/prisma', () => ({
  prisma: {
    subject: {
      findMany: jest.fn(),
      findUnique: jest.fn(),
    },
  },
}));

describe('SubjectService', () => {
  describe('getAll', () => {
    it('should return all subjects', async () => {
      const mockSubjects = [
        { id: '1', name: 'JavaScript' },
        { id: '2', name: 'Python' },
      ];

      (prisma.subject.findMany as jest.Mock).mockResolvedValue(mockSubjects);

      const result = await SubjectService.getAll();

      expect(result).toEqual(mockSubjects);
      expect(prisma.subject.findMany).toHaveBeenCalledWith({
        orderBy: { name: 'asc' },
      });
    });
  });
});
```

### 7.3 Exemplo de Teste de API

```typescript
// src/app/api/subjects/__tests__/route.test.ts

import { GET } from '../route';
import { SubjectService } from '@/services/subject.service';

jest.mock('@/services/subject.service');

describe('GET /api/subjects', () => {
  it('should return subjects', async () => {
    const mockSubjects = [{ id: '1', name: 'JavaScript' }];
    
    (SubjectService.getAll as jest.Mock).mockResolvedValue(mockSubjects);

    const request = new Request('http://localhost/api/subjects');
    const response = await GET(request);
    const data = await response.json();

    expect(response.status).toBe(200);
    expect(data.data).toEqual(mockSubjects);
  });
});
```

---

## 8. Performance

### 8.1 Otimizações de Frontend

**Imagens:**
```typescript
// ✅ Usar Next.js Image
import Image from 'next/image';

<Image
  src="/logo.png"
  alt="Logo"
  width={200}
  height={200}
  priority // Para above-the-fold
/>
```

**Code Splitting:**
```typescript
// ✅ Lazy load componentes pesados
import dynamic from 'next/dynamic';

const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <LoadingSkeleton />,
});
```

**Memoização:**
```typescript
// ✅ Memoizar componentes pesados
import { memo } from 'react';

export const ExpensiveComponent = memo(({ data }: Props) => {
  // Component logic
});
```

### 8.2 Otimizações de Backend

**Database Queries:**
```typescript
// ✅ Selecionar apenas campos necessários
const subjects = await prisma.subject.findMany({
  select: {
    id: true,
    name: true,
    slug: true,
  },
});

// ✅ Usar includes com cuidado
const subject = await prisma.subject.findUnique({
  where: { id },
  include: {
    subtopics: {
      take: 10, // Limitar resultados
    },
  },
});
```

**Cache:**
```typescript
// ✅ Cache de respostas
import { unstable_cache } from 'next/cache';

const getCachedSubjects = unstable_cache(
  async () => SubjectService.getAll(),
  ['subjects'],
  { revalidate: 3600 } // 1 hora
);
```

---

## 9. Segurança

### 9.1 Boas Práticas

**Input Validation:**
- Sempre validar input com Zod
- Sanitizar dados do usuário
- Usar parameterized queries (Prisma faz isso)

**Environment Variables:**
- Nunca commitar `.env.local`
- Usar `.env.example` como template
- Validar variáveis na inicialização

**API Security:**
- Rate limiting em APIs públicas
- CORS configurado adequadamente
- Headers de segurança (via next.config.js)

**Error Messages:**
- Não expor detalhes técnicos em erros
- Logs detalhados apenas no servidor
- Mensagens genéricas para usuários

---

## 10. Documentação

### 10.1 Comentários no Código

**Quando comentar:**
- Lógica complexa que não é óbvia
- Decisões arquiteturais importantes
- TODOs e FIXMEs
- Explicações de workarounds

**Exemplo:**
```typescript
// TODO: Implementar cache para reduzir chamadas à API de IA
// FIXME: Tratar edge case quando subjectId é null
// NOTE: Usamos unstable_cache porque revalidate não está disponível no App Router ainda
```

### 10.2 README

Manter README.md atualizado com:
- Setup do projeto
- Scripts disponíveis
- Estrutura do projeto
- Como contribuir

---

## 11. Code Review

### 11.1 Checklist

**Funcionalidade:**
- [ ] Código funciona como esperado?
- [ ] Edge cases tratados?
- [ ] Error handling adequado?

**Código:**
- [ ] Segue convenções do projeto?
- [ ] TypeScript sem erros?
- [ ] Testes passam?
- [ ] Lint passa?

**Performance:**
- [ ] Queries otimizadas?
- [ ] Sem re-renders desnecessários?
- [ ] Imagens otimizadas?

**Segurança:**
- [ ] Input validado?
- [ ] Secrets não expostos?
- [ ] SQL injection protegido?

---

## 12. Troubleshooting

### 12.1 Problemas Comuns

**Prisma:**
```bash
# Reset database
npx prisma migrate reset

# Regenerate client
npx prisma generate
```

**Next.js:**
```bash
# Limpar cache
rm -rf .next
npm run build
```

**TypeScript:**
```bash
# Verificar tipos
npm run type-check
```

---

## 13. Recursos Úteis

### 13.1 Documentação

- [Next.js Docs](https://nextjs.org/docs)
- [Prisma Docs](https://www.prisma.io/docs)
- [Chakra UI Docs](https://chakra-ui.com)
- [TypeScript Docs](https://www.typescriptlang.org/docs)

### 13.2 Ferramentas

- **Prisma Studio**: `npx prisma studio`
- **Next.js Debug**: `NODE_OPTIONS='--inspect' npm run dev`

---

**Versão**: 1.0  
**Data**: [Data atual]  
**Autor**: Arquiteto de Software  
**Status**: Guia Ativo

