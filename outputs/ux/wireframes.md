# Wireframes - Sistema de Estudos para Desenvolvedores

## 1. Visão Geral

Este documento apresenta os wireframes do sistema, focando na estrutura, hierarquia de informação e fluxo de navegação. Os wireframes foram criados considerando as necessidades identificadas na pesquisa de usuário e os requisitos do Product Owner.

**Princípios de Design:**
- Simplicidade e clareza
- Hierarquia visual clara
- Navegação intuitiva
- Responsividade em todos os dispositivos
- Acessibilidade desde o início

---

## 2. Estrutura de Navegação

### 2.1 Arquitetura de Informação

```
Home (Página Inicial)
  ├── Lista de Assuntos
  │   └── Assunto Individual
  │       ├── Lista de Subtópicos
  │       │   └── Conteúdo do Subtópico
  │       └── [Futuro: Busca, Filtros]
  └── [Futuro: Histórico, Favoritos]
```

### 2.2 Componentes Principais

1. **Header/Navbar**: Navegação global e ações principais
2. **Breadcrumbs**: Navegação contextual
3. **Sidebar**: Navegação lateral (desktop)
4. **Content Area**: Área principal de conteúdo
5. **Footer**: Informações e links adicionais

---

## 3. Wireframes Detalhados

### 3.1 Página Inicial (Home)

**Objetivo**: Apresentar o sistema e permitir que usuários escolham um assunto para estudar.

**Layout Desktop:**

```
┌─────────────────────────────────────────────────────────────┐
│ [Logo] Sistema de Estudos          [Busca] [Menu]           │ Header
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Bem-vindo ao Sistema de Estudos                            │ Hero Section
│  Escolha um assunto para começar                            │
│                                                               │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐      │
│  │ JavaScript│ │  Python  │ │   React  │ │   Node   │      │ Grid de
│  │           │ │          │ │          │ │          │      │ Assuntos
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘      │ (4 colunas)
│                                                               │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐      │
│  │   Java   │ │    PHP   │ │  Angular │ │   Vue    │      │
│  │          │ │          │ │          │ │          │      │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘      │
│                                                               │
│  [Ver Todos os Assuntos →]                                  │
│                                                               │
├─────────────────────────────────────────────────────────────┤
│ Links | Sobre | Contato | Termos                            │ Footer
└─────────────────────────────────────────────────────────────┘
```

**Elementos Principais:**
- Header com logo e navegação
- Hero section com mensagem de boas-vindas
- Grid de assuntos em cards (4 colunas desktop)
- Botão "Ver Todos" para expandir lista completa
- Footer com links úteis

**Layout Mobile:**

```
┌─────────────────────┐
│ [☰] Logo    [🔍]    │ Header Compacto
├─────────────────────┤
│                     │
│  Bem-vindo!         │ Hero Section
│  Escolha um assunto │ (Texto menor)
│                     │
│  ┌──────────────┐   │
│  │ JavaScript   │   │ Cards em
│  └──────────────┘   │ coluna única
│                     │
│  ┌──────────────┐   │
│  │   Python     │   │
│  └──────────────┘   │
│                     │
│  ┌──────────────┐   │
│  │    React     │   │
│  └──────────────┘   │
│                     │
│  [Ver Mais ↓]       │
│                     │
└─────────────────────┘
```

**Considerações:**
- Cards empilhados verticalmente
- Header compacto com menu hamburger
- Touch targets mínimos de 44x44px
- Scroll infinito ou paginação

---

### 3.2 Página de Assunto

**Objetivo**: Exibir subtópicos de um assunto selecionado e permitir navegação.

**Layout Desktop:**

```
┌─────────────────────────────────────────────────────────────┐
│ [Logo] Sistema de Estudos          [Busca] [Menu]           │ Header
├─────────────────────────────────────────────────────────────┤
│ Home > JavaScript                                            │ Breadcrumbs
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  JavaScript                                                  │ Título do
│  Linguagem de programação para desenvolvimento web          │ Assunto
│                                                               │
│  ┌─────────────────────────────────────────────────────┐  │
│  │ Subtópicos                                            │  │ Sidebar
│  │                                                       │  │ (Desktop)
│  │ 1. Introdução                                         │  │
│  │ 2. Variáveis e Tipos                                  │  │
│  │ 3. Funções                                            │  │
│  │ 4. Arrays e Objetos                                   │  │
│  │ 5. Async/Await                                        │  │
│  │ 6. ES6+ Features                                       │  │
│  │ ...                                                    │  │
│  └─────────────────────────────────────────────────────┘  │
│                                                               │
│  [Área de Conteúdo - vazia inicialmente]                    │ Content Area
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

**Layout Mobile:**

```
┌─────────────────────┐
│ [←] JavaScript  [☰]  │ Header com Back
├─────────────────────┤
│ Home > JavaScript   │ Breadcrumbs
├─────────────────────┤
│                     │
│  JavaScript         │ Título
│                     │
│  ┌──────────────┐   │
│  │ Subtópicos  │   │ Accordion ou
│  │ ▼           │   │ Lista expansível
│  └──────────────┘   │
│                     │
│  ┌──────────────┐   │
│  │ 1. Introdução│   │ Lista de
│  └──────────────┘   │ Subtópicos
│                     │
│  ┌──────────────┐   │
│  │ 2. Variáveis │   │
│  └──────────────┘   │
│                     │
│  [Scroll para mais] │
│                     │
└─────────────────────┘
```

**Interações:**
- Clique em subtópico carrega conteúdo na área principal (desktop)
- Clique em subtópico navega para página de conteúdo (mobile)
- Sidebar fixa com scroll independente (desktop)
- Accordion ou lista expansível (mobile)

---

### 3.3 Página de Conteúdo do Subtópico

**Objetivo**: Exibir o conteúdo educacional de um subtópico para estudo.

**Layout Desktop:**

```
┌─────────────────────────────────────────────────────────────┐
│ [Logo] Sistema de Estudos          [Busca] [Menu]           │ Header
├─────────────────────────────────────────────────────────────┤
│ Home > JavaScript > Variáveis e Tipos                       │ Breadcrumbs
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌─────────────────────────────────────────────────────┐  │
│  │ Subtópicos                    Variáveis e Tipos    │  │ Sidebar +
│  │                                                      │  │ Content
│  │ 1. Introdução          ┌─────────────────────────┐  │  │
│  │ 2. Variáveis ✓        │ Conteúdo do Subtópico   │  │  │
│  │ 3. Funções            │                         │  │  │
│  │ 4. Arrays             │ Texto explicativo...     │  │  │
│  │ 5. Async/Await        │                         │  │  │
│  │                       │ Exemplo de código:      │  │  │
│  │ [Anterior] [Próximo]  │ ```javascript          │  │  │
│  │                       │ let x = 10;            │  │  │
│  │                       │ ```                    │  │  │
│  └───────────────────────┴─────────────────────────┘  │  │
│                                                               │
│  [← Anterior] [Marcar como Estudado] [Próximo →]            │ Navegação
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

**Elementos Principais:**
- Sidebar com lista de subtópicos (desktop)
- Área de conteúdo com texto e código
- Navegação anterior/próximo
- Botão "Marcar como Estudado"
- Indicador de progresso (checkmark em subtópicos concluídos)

**Layout Mobile:**

```
┌─────────────────────┐
│ [←] Variáveis  [☰]  │ Header
├─────────────────────┤
│ JS > Variáveis      │ Breadcrumbs
├─────────────────────┤
│                     │
│  Variáveis e Tipos  │ Título
│                     │
│  ┌──────────────┐   │
│  │ Conteúdo...  │   │ Conteúdo
│  │              │   │ (Scroll)
│  │ Texto...     │   │
│  │              │   │
│  │ Código:      │   │
│  │ ```          │   │
│  │ let x = 10;  │   │
│  │ ```          │   │
│  └──────────────┘   │
│                     │
│  [← Anterior]       │ Navegação
│  [✓ Estudado]       │
│  [Próximo →]        │
│                     │
│  [Ver Subtópicos]   │ Menu inferior
│                     │
└─────────────────────┘
```

**Considerações:**
- Conteúdo em coluna única
- Navegação fixa no rodapé (mobile)
- Botão para acessar lista de subtópicos
- Syntax highlighting para código

---

### 3.4 Onboarding (Primeira Visita)

**Objetivo**: Guiar novos usuários e explicar como usar o sistema.

**Fluxo de Onboarding:**

```
Tela 1: Boas-vindas
┌─────────────────────┐
│                     │
│    [Ilustração]     │
│                     │
│  Bem-vindo!         │
│  Vamos começar?     │
│                     │
│  [Pular] [Começar]  │
└─────────────────────┘

Tela 2: Escolha de Assunto
┌─────────────────────┐
│                     │
│  Escolha um assunto │
│  para começar       │
│                     │
│  [JavaScript]      │
│  [Python]            │
│  [React]             │
│                     │
│  [Voltar] [Próximo]  │
└─────────────────────┘

Tela 3: Navegação
┌─────────────────────┐
│                     │
│  [Highlight]         │
│  Use o menu lateral  │
│  para navegar        │
│                     │
│  [Entendi]           │
└─────────────────────┘
```

**Características:**
- Máximo 3-4 telas
- Opção de pular
- Interativo com highlights
- Não bloqueia acesso

---

### 3.5 Estados de Loading

**Objetivo**: Fornecer feedback visual durante carregamento.

**Estados:**

```
Loading de Conteúdo:
┌─────────────────────┐
│  [Skeleton]         │
│  ████████           │
│  ████               │
│  ██████████         │
│  ████               │
└─────────────────────┘

Loading de Geração IA:
┌─────────────────────┐
│  Gerando conteúdo...│
│  [Spinner]          │
│  Isso pode levar     │
│  alguns segundos     │
└─────────────────────┘
```

---

### 3.6 Estados de Erro

**Objetivo**: Comunicar erros de forma clara e oferecer soluções.

**Estados:**

```
Erro ao Carregar:
┌─────────────────────┐
│  [Ícone Erro]       │
│                     │
│  Ops! Algo deu      │
│  errado             │
│                     │
│  [Tentar Novamente] │
└─────────────────────┘

Conteúdo Não Disponível:
┌─────────────────────┐
│  [Ícone Info]        │
│                     │
│  Conteúdo ainda     │
│  não disponível     │
│                     │
│  [Gerar Conteúdo]   │
└─────────────────────┘
```

---

## 4. Responsividade

### 4.1 Breakpoints

- **Mobile**: < 768px
- **Tablet**: 768px - 1024px
- **Desktop**: > 1024px

### 4.2 Adaptações por Dispositivo

**Mobile:**
- Menu hamburger
- Cards em coluna única
- Navegação inferior fixa
- Touch targets grandes (44x44px mínimo)

**Tablet:**
- Layout híbrido
- Sidebar colapsável
- Grid de 2 colunas para assuntos

**Desktop:**
- Sidebar fixa
- Grid de 4 colunas para assuntos
- Navegação completa visível

---

## 5. Hierarquia de Informação

### 5.1 Níveis de Importância

1. **Primário**: Conteúdo atual sendo estudado
2. **Secundário**: Navegação e subtópicos relacionados
3. **Terciário**: Ações auxiliares (busca, favoritos, etc.)

### 5.2 Princípios Visuais

- **Tamanho**: Elementos importantes são maiores
- **Cor**: Destaque para ações primárias
- **Espaçamento**: Agrupamento visual relacionado
- **Contraste**: Texto legível em todos os backgrounds

---

## 6. Acessibilidade

### 6.1 Navegação por Teclado

- Tab: Navegar entre elementos interativos
- Enter/Space: Ativar elementos
- Esc: Fechar modais/menus
- Setas: Navegar em listas

### 6.2 Screen Readers

- Labels descritivos em todos os elementos
- ARIA labels onde necessário
- Estrutura semântica HTML
- Textos alternativos em imagens

### 6.3 Contraste

- Texto: Mínimo 4.5:1 (WCAG AA)
- Elementos interativos: Mínimo 3:1
- Estados de foco visíveis

---

## 7. Anotações Técnicas

### 7.1 Componentes Reutilizáveis

- Card de Assunto
- Card de Subtópico
- Breadcrumbs
- Botão de Navegação
- Loading Skeleton
- Mensagem de Erro

### 7.2 Interações

- Hover states em todos os elementos clicáveis
- Transições suaves (200-300ms)
- Feedback visual imediato
- Estados de loading claros

### 7.3 Performance

- Lazy loading de imagens
- Code splitting por rota
- Otimização de assets
- Cache de conteúdo

---

## 8. Próximos Passos

1. **Validação**: Testar wireframes com usuários
2. **Refinamento**: Ajustar baseado em feedback
3. **Prototipação**: Criar protótipos interativos
4. **Design Visual**: Aplicar design system
5. **Desenvolvimento**: Handoff para equipe técnica

---

**Versão**: 1.0  
**Data**: [Data atual]  
**Autor**: UX Designer  
**Status**: Aguardando Validação

