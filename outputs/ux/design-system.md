# Design System - Sistema de Estudos para Desenvolvedores

## 1. Visão Geral

Este documento define o Design System completo do Sistema de Estudos para Desenvolvedores. O design system foi criado para garantir consistência visual, melhorar eficiência de desenvolvimento e facilitar manutenção.

**Framework UI**: Chakra UI v3  
**Princípios de Design**: Simplicidade, Clareza, Acessibilidade  
**Foco**: Experiência de aprendizado otimizada

---

## 2. Princípios de Design

### 2.1 Simplicidade
- Interface limpa e despoluída
- Foco no conteúdo
- Redução de elementos desnecessários
- Hierarquia visual clara

### 2.2 Clareza
- Comunicação direta e objetiva
- Feedback visual imediato
- Estados claros e distintos
- Mensagens compreensíveis

### 2.3 Acessibilidade
- WCAG AA compliance
- Navegação por teclado
- Suporte a screen readers
- Contraste adequado

### 2.4 Consistência
- Padrões visuais uniformes
- Componentes reutilizáveis
- Linguagem consistente
- Comportamentos previsíveis

---

## 3. Cores

### 3.1 Paleta Principal

**Cor Primária:**
```
Azul Educacional
- Primary 500: #3182CE (cor principal)
- Primary 600: #2C5AA0 (hover/active)
- Primary 400: #4299E1 (light)
- Primary 700: #2B6CB0 (dark)
```

**Cor Secundária:**
```
Verde Sucesso
- Success 500: #38A169 (concluído, sucesso)
- Success 600: #2F855A (hover)
- Success 400: #48BB78 (light)
```

**Cor de Aviso:**
```
Amarelo Atenção
- Warning 500: #D69E2E (aviso, atenção)
- Warning 600: #B7791F (hover)
```

**Cor de Erro:**
```
Vermelho Erro
- Error 500: #E53E3E (erro, perigo)
- Error 600: #C53030 (hover)
```

### 3.2 Paleta Neutra

**Cinzas:**
```
- Gray 50: #F7FAFC (background claro)
- Gray 100: #EDF2F7 (background)
- Gray 200: #E2E8F0 (borders)
- Gray 300: #CBD5E0 (dividers)
- Gray 400: #A0AEC0 (texto secundário)
- Gray 500: #718096 (texto)
- Gray 600: #4A5568 (texto escuro)
- Gray 700: #2D3748 (texto muito escuro)
- Gray 800: #1A202C (texto quase preto)
- Gray 900: #171923 (texto preto)
```

### 3.3 Uso de Cores

**Backgrounds:**
- Background principal: `gray.50`
- Background cards: `white`
- Background hover: `gray.100`
- Background active: `primary.100`

**Textos:**
- Texto primário: `gray.700`
- Texto secundário: `gray.500`
- Texto desabilitado: `gray.400`
- Links: `primary.500`

**Estados:**
- Sucesso: `success.500`
- Erro: `error.500`
- Aviso: `warning.500`
- Info: `primary.500`

---

## 4. Tipografia

### 4.1 Fontes

**Fonte Principal:**
- Inter (sans-serif)
- Fallback: system-ui, -apple-system, sans-serif

**Fonte Código:**
- Fira Code (monospace)
- Fallback: 'Courier New', monospace

### 4.2 Escala Tipográfica

```
Heading 1 (h1): 2.5rem (40px) - Bold
Heading 2 (h2): 2rem (32px) - Bold
Heading 3 (h3): 1.5rem (24px) - Semibold
Heading 4 (h4): 1.25rem (20px) - Semibold
Body Large: 1.125rem (18px) - Regular
Body: 1rem (16px) - Regular
Body Small: 0.875rem (14px) - Regular
Caption: 0.75rem (12px) - Regular
```

### 4.3 Pesos de Fonte

- Light: 300
- Regular: 400
- Medium: 500
- Semibold: 600
- Bold: 700

### 4.4 Line Heights

- Tight: 1.2 (headings)
- Normal: 1.5 (body)
- Relaxed: 1.75 (long-form content)

---

## 5. Espaçamento

### 5.1 Sistema de Espaçamento

Baseado em escala de 4px:

```
0: 0px
1: 4px
2: 8px
3: 12px
4: 16px
5: 20px
6: 24px
8: 32px
10: 40px
12: 48px
16: 64px
20: 80px
24: 96px
```

### 5.2 Uso de Espaçamento

**Padding de Componentes:**
- Small: `2` (8px)
- Medium: `4` (16px)
- Large: `6` (24px)

**Gap entre Elementos:**
- Tight: `2` (8px)
- Normal: `4` (16px)
- Loose: `6` (24px)

**Margens de Seção:**
- Small: `6` (24px)
- Medium: `8` (32px)
- Large: `12` (48px)

---

## 6. Componentes

### 6.1 Botões

**Botão Primário:**
```jsx
<Button colorScheme="blue" size="md">
  Ação Principal
</Button>
```
- Cor: Primary 500
- Hover: Primary 600
- Active: Primary 700
- Padding: 12px 24px
- Border radius: 6px
- Font weight: 500

**Botão Secundário:**
```jsx
<Button variant="outline" colorScheme="blue">
  Ação Secundária
</Button>
```
- Background: Transparent
- Border: 1px solid Primary 500
- Text: Primary 500
- Hover: Primary 50 background

**Botão Ghost:**
```jsx
<Button variant="ghost">
  Ação Terciária
</Button>
```
- Background: Transparent
- Hover: Gray 100 background

**Tamanhos:**
- Small: height 32px, padding 8px 16px
- Medium: height 40px, padding 12px 24px
- Large: height 48px, padding 16px 32px

---

### 6.2 Cards

**Card de Assunto:**
```jsx
<Card>
  <CardHeader>
    <Heading size="md">JavaScript</Heading>
  </CardHeader>
  <CardBody>
    <Text>Linguagem de programação...</Text>
  </CardBody>
</Card>
```

**Estilos:**
- Background: White
- Border: 1px solid Gray 200
- Border radius: 8px
- Shadow: 0 1px 3px rgba(0,0,0,0.1)
- Hover: Shadow aumenta, scale 1.02
- Padding: 16px

---

### 6.3 Inputs

**Input Padrão:**
```jsx
<Input placeholder="Buscar..." />
```

**Estilos:**
- Height: 40px
- Border: 1px solid Gray 300
- Border radius: 6px
- Padding: 12px
- Focus: Border Primary 500, shadow outline

**Input de Busca:**
- Ícone de busca à esquerda
- Clear button à direita (quando tem valor)

---

### 6.4 Navegação

**Breadcrumbs:**
```jsx
<Breadcrumb>
  <BreadcrumbItem>
    <BreadcrumbLink href="/">Home</BreadcrumbLink>
  </BreadcrumbItem>
  <BreadcrumbItem>
    <BreadcrumbLink>JavaScript</BreadcrumbLink>
  </BreadcrumbItem>
</Breadcrumb>
```

**Sidebar:**
- Background: White
- Border right: 1px solid Gray 200
- Width: 280px (desktop)
- Fixed position
- Scroll independente

---

### 6.5 Badges e Tags

**Badge de Status:**
```jsx
<Badge colorScheme="green">Concluído</Badge>
<Badge colorScheme="blue">Em Progresso</Badge>
```

**Estilos:**
- Padding: 4px 8px
- Border radius: 4px
- Font size: 12px
- Font weight: 500

---

### 6.6 Loading States

**Spinner:**
```jsx
<Spinner size="md" color="blue.500" />
```

**Skeleton:**
```jsx
<Skeleton height="20px" />
<Skeleton height="20px" width="80%" />
```

**Progress Bar:**
```jsx
<Progress value={60} colorScheme="blue" />
```

---

### 6.7 Toast Notifications

**Toast de Sucesso:**
- Background: Success 500
- Text: White
- Icon: Checkmark
- Duration: 3000ms
- Position: Top right

**Toast de Erro:**
- Background: Error 500
- Text: White
- Icon: X
- Duration: 5000ms

---

## 7. Layout

### 7.1 Grid System

**Container:**
- Max width: 1280px
- Padding: 24px (mobile), 32px (desktop)
- Margin: 0 auto

**Grid de Assuntos:**
- Desktop: 4 colunas
- Tablet: 2 colunas
- Mobile: 1 coluna
- Gap: 24px

---

### 7.2 Breakpoints

```
sm: 480px
md: 768px
lg: 1024px
xl: 1280px
2xl: 1536px
```

---

### 7.3 Estrutura de Página

**Header:**
- Height: 64px
- Background: White
- Border bottom: 1px solid Gray 200
- Fixed position (desktop)
- Sticky position (mobile)

**Content Area:**
- Padding: 24px (mobile), 32px (desktop)
- Max width: Container

**Footer:**
- Background: Gray 50
- Padding: 32px
- Border top: 1px solid Gray 200

---

## 8. Ícones

### 8.1 Biblioteca de Ícones

**Fonte**: React Icons (ou similar)

**Ícones Principais:**
- Home: `FaHome`
- Search: `FaSearch`
- Menu: `FaBars`
- Close: `FaTimes`
- Check: `FaCheck`
- Arrow Right: `FaArrowRight`
- Arrow Left: `FaArrowLeft`
- Book: `FaBook`
- Code: `FaCode`

**Tamanhos:**
- Small: 16px
- Medium: 20px
- Large: 24px

---

## 9. Animações

### 9.1 Durações

- Fast: 150ms
- Normal: 300ms
- Slow: 500ms

### 9.2 Easing Functions

- Ease out: `cubic-bezier(0.0, 0, 0.2, 1)`
- Ease in: `cubic-bezier(0.4, 0, 1, 1)`
- Ease in-out: `cubic-bezier(0.4, 0, 0.2, 1)`

### 9.3 Transições Comuns

**Hover:**
```css
transition: all 200ms ease-out;
```

**Fade:**
```css
transition: opacity 300ms ease-in-out;
```

**Slide:**
```css
transition: transform 400ms ease-in-out;
```

---

## 10. Estados

### 10.1 Estados de Interação

**Default:**
- Cor normal
- Sem efeitos

**Hover:**
- Cor mais escura (600)
- Shadow aumentada
- Scale 1.02 (cards)

**Active/Pressed:**
- Cor mais escura (700)
- Scale 0.98

**Focus:**
- Outline: 2px solid Primary 500
- Shadow: 0 0 0 3px Primary 100

**Disabled:**
- Opacity: 0.5
- Cursor: not-allowed
- Pointer events: none

---

### 10.2 Estados de Conteúdo

**Loading:**
- Skeleton ou spinner
- Opacity reduzida

**Empty:**
- Ilustração
- Mensagem explicativa
- Call-to-action

**Error:**
- Ícone de erro
- Mensagem clara
- Ação de recuperação

**Success:**
- Ícone de sucesso
- Mensagem confirmativa
- Feedback visual

---

## 11. Acessibilidade

### 11.1 Contraste

- Texto normal: Mínimo 4.5:1
- Texto grande: Mínimo 3:1
- Elementos interativos: Mínimo 3:1

### 11.2 Navegação por Teclado

- Tab: Navegar entre elementos
- Enter/Space: Ativar
- Esc: Fechar modais
- Setas: Navegar listas

### 11.3 Screen Readers

- ARIA labels em todos os elementos interativos
- Estrutura semântica HTML
- Textos alternativos em imagens
- Landmarks apropriados

### 11.4 Focus States

- Sempre visíveis
- Contraste adequado
- Não removidos com CSS

---

## 12. Responsividade

### 12.1 Mobile First

- Design começa em mobile
- Progressive enhancement para desktop
- Touch targets mínimos: 44x44px

### 12.2 Adaptações por Breakpoint

**Mobile (< 768px):**
- Menu hamburger
- Cards em coluna única
- Navegação inferior (opcional)
- Font sizes reduzidos

**Tablet (768px - 1024px):**
- Grid de 2 colunas
- Sidebar colapsável
- Layout híbrido

**Desktop (> 1024px):**
- Sidebar fixa
- Grid de 4 colunas
- Navegação completa
- Hover states ativos

---

## 13. Código de Exemplo

### 13.1 Tema Chakra UI

```typescript
import { extendTheme } from '@chakra-ui/react'

const theme = extendTheme({
  colors: {
    brand: {
      50: '#EBF8FF',
      100: '#BEE3F8',
      200: '#90CDF4',
      300: '#63B3ED',
      400: '#4299E1',
      500: '#3182CE',
      600: '#2C5AA0',
      700: '#2B6CB0',
      800: '#2C5282',
      900: '#2A4365',
    },
  },
  fonts: {
    heading: 'Inter, system-ui, sans-serif',
    body: 'Inter, system-ui, sans-serif',
    mono: 'Fira Code, monospace',
  },
  components: {
    Button: {
      defaultProps: {
        colorScheme: 'brand',
      },
      baseStyle: {
        fontWeight: '500',
        borderRadius: '6px',
      },
    },
    Card: {
      baseStyle: {
        container: {
          boxShadow: 'sm',
          borderRadius: '8px',
        },
      },
    },
  },
})
```

---

## 14. Documentação de Componentes

### 14.1 Storybook (Recomendado)

Manter documentação de componentes no Storybook com:
- Exemplos de uso
- Props e variantes
- Estados diferentes
- Acessibilidade

### 14.2 Guia de Uso

Para cada componente, documentar:
- Quando usar
- Quando não usar
- Variantes disponíveis
- Exemplos de código
- Acessibilidade

---

## 15. Manutenção

### 15.1 Versionamento

- Versionar design system separadamente
- Breaking changes em major versions
- Changelog detalhado

### 15.2 Contribuição

- Guidelines para adicionar novos componentes
- Processo de revisão
- Testes obrigatórios

---

**Versão do Design System**: 1.0  
**Última Atualização**: [Data atual]  
**Mantido por**: Equipe de Design + Desenvolvimento  
**Próxima Revisão**: Após feedback de desenvolvimento

