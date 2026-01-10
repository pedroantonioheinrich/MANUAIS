# Manual Completo do Tailwind CSS

## Índice

1. [Introdução ao Tailwind CSS](#introdução-ao-tailwind-css)
2. [Por que Usar Tailwind?](#por-que-usar-tailwind)
3. [Instalação e Configuração](#instalação-e-configuração)
4. [Conceitos Fundamentais](#conceitos-fundamentais)
5. [Classes de Utilidade](#classes-de-utilidade)
6. [Responsividade](#responsividade)
7. [Estados e Interações](#estados-e-interações)
8. [Customização](#customização)
9. [Componentes com @apply](#componentes-com-apply)
10. [Plugins Oficiais](#plugins-oficiais)
11. [Performance](#performance)
12. [Workflow e Boas Práticas](#workflow-e-boas-práticas)
13. [Integração com Frameworks](#integração-com-frameworks)
14. [Recursos e Ferramentas](#recursos-e-ferramentas)

## Introdução ao Tailwind CSS

### O que é Tailwind CSS?
Tailwind CSS é um framework CSS utility-first que permite construir designs customizados rapidamente sem sair do seu HTML. Ao invés de componentes pré-construídos, Tailwind fornece classes de baixo nível que podem ser compostas para criar qualquer design.

### Filosofia do Tailwind
```markdown
🧱 Utility-First
- Classes pequenas e específicas
- Composição sobre herança
- Sem nomes de classes semânticos
- Estilo diretamente no HTML

🎨 Design in Browser
- Iteração rápida
- Feedback visual imediato
- Menos contexto switching
- Design system consistente

⚡ Produtividade
- Menos CSS customizado
- Menos decisões de nomenclatura
- Construção mais rápida
- Manutenção simplificada
```

### História e Versões
```markdown
📅 Timeline:
- 2017: Tailwind v0.1 lançado por Adam Wathan
- 2019: Tailwind v1.0 (estável)
- 2020: Tailwind v2.0 (JIT engine)
- 2021: Tailwind v3.0 (JIT por padrão)
- 2023: Tailwind v3.3 (ESM, subpaths)

🎯 Versão Atual: 3.4.1
- JIT (Just-In-Time) engine
- Suporte CSS moderno
- Melhor performance
- Novas features
```

## Por que Usar Tailwind?

### Vantagens
```markdown
✅ Benefícios Técnicos:
- Tamanho de CSS muito menor
- CSS consistente e sem duplicação
- Design system implícito
- Responsividade fácil
- Acessibilidade built-in
- Performance otimizada

🚀 Benefícios de Produtividade:
- Desenvolvimento mais rápido
- Menor curva de aprendizado
- Menos naming paralysis
- Design no browser
- Hot reload automático

🎨 Benefícios de Design:
- Design system consistente
- Constraints úteis
- Escala previsível
- Customização fácil
- Design tokens
```

### Comparação com Outras Abordagens
```markdown
Tailwind vs Bootstrap:
┌─────────────┬─────────────────┐
│ Bootstrap   │ Tailwind        │
├─────────────┼─────────────────┤
│ Componentes │ Utilities       │
│ Opinionado  │ Flexível        │
│ Menos CSS   │ Sem CSS custom  │
│ Mais peso   │ Menor peso      │
│ Override    │ Customização    │
└─────────────┴─────────────────┘

Tailwind vs CSS-in-JS:
┌─────────────┬─────────────────┐
│ CSS-in-JS   │ Tailwind        │
├─────────────┼─────────────────┤
│ Runtime     │ Build-time      │
│ JS Bundle   │ CSS Bundle      │
│ Dinâmico    │ Estático        │
│ Themes      │ Config          │
│ CSS props   │ Classes         │
└─────────────┴─────────────────┘

Tailwind vs CSS Puro:
┌─────────────┬─────────────────┐
│ CSS Puro    │ Tailwind        │
├─────────────┼─────────────────┤
│ Flexível    │ Constrained     │
│ Manutenção  │ Produtividade   │
│ Especific.  │ Consistência    │
│ Custom      │ Utilities       │
│ Cascata     │ Flat hierarchy  │
└─────────────┴─────────────────┘
```

### Quando Usar Tailwind
```markdown
🎯 Casos de Uso Ideais:
- Aplicações web modernas
- Design systems
- Prototipação rápida
- Projetos com equipe
- Design no browser
- MVP e startups

⚠️ Quando Evitar:
- Projetos com CSS legacy
- Times resistentes a mudanças
- Projetos muito pequenos
- Quando precisa de componentes prontos
- Sistemas com themes dinâmicos
```

## Instalação e Configuração

### Instalação via npm/yarn
```bash
# Instalação básica
npm install -D tailwindcss
npx tailwindcss init

# Instalação com PostCSS
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Estrutura de arquivos gerada
project/
├── tailwind.config.js
├── postcss.config.js
├── src/
│   ├── input.css
│   └── index.html
└── dist/
    └── output.css
```

### Configuração do PostCSS
```javascript
// postcss.config.js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

### Configuração do Tailwind
```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{html,js,jsx,ts,tsx,vue}",
    "./public/index.html",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### Arquivo CSS Principal
```css
/* src/input.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

/* CSS customizado (opcional) */
@layer base {
  :root {
    --primary: #3b82f6;
  }
}

@layer components {
  .btn-primary {
    @apply bg-blue-500 text-white font-bold py-2 px-4 rounded;
  }
}

@layer utilities {
  .scrollbar-hide {
    -ms-overflow-style: none;
    scrollbar-width: none;
  }
}
```

### Build Process
```bash
# Desenvolvimento com watch
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch

# Produção com minificação
NODE_ENV=production npx tailwindcss -i ./src/input.css -o ./dist/output.css --minify

# Configuração do package.json
{
  "scripts": {
    "dev": "tailwindcss -i ./src/input.css -o ./dist/output.css --watch",
    "build": "NODE_ENV=production tailwindcss -i ./src/input.css -o ./dist/output.css --minify"
  }
}
```

### CDN (Apenas Desenvolvimento)
```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Configuração via CDN -->
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            clifford: '#da373d',
          }
        }
      }
    }
  </script>
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    Hello Tailwind!
  </h1>
</body>
</html>
```

## Conceitos Fundamentais

### Utility-First
```html
<!-- Abordagem tradicional -->
<style>
  .card {
    padding: 1rem;
    background: white;
    border-radius: 0.5rem;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  }
</style>
<div class="card">Conteúdo</div>

<!-- Abordagem Tailwind -->
<div class="p-4 bg-white rounded-lg shadow">
  Conteúdo
</div>
```

### Sistema de Design
```markdown
📏 Espaçamento (Spacing)
Base: 0.25rem = 4px
┌─────────┬─────────┬──────┐
│ Classe  │ Valor   │ Px   │
├─────────┼─────────┼──────┤
│ p-0     │ 0px     │ 0    │
│ p-1     │ 0.25rem │ 4    │
│ p-2     │ 0.5rem  │ 8    │
│ p-3     │ 0.75rem │ 12   │
│ p-4     │ 1rem    │ 16   │
│ p-8     │ 2rem    │ 32   │
│ p-12    │ 3rem    │ 48   │
│ p-16    │ 4rem    │ 64   │
└─────────┴─────────┴──────┘

🎨 Cores (Colors)
Paleta padrão:
- gray, red, yellow, green, blue, indigo, purple, pink
- 50 (mais claro) a 900 (mais escuro)
- Ex: bg-blue-500, text-gray-700, border-red-300

🔤 Tipografia (Typography)
- Font families
- Font sizes (text-xs a text-9xl)
- Font weights (font-thin a font-black)
- Line heights (leading-tight a leading-loose)
- Letter spacing (tracking-tighter a tracking-widest)
```

### Classes Responsivas
```html
<!-- Mobile first -->
<div class="text-sm md:text-base lg:text-lg">
  <!-- Pequeno em mobile, médio em tablet, grande em desktop -->
</div>

<!-- Hide/show responsivo -->
<div class="hidden md:block">
  <!-- Visível apenas em desktop -->
</div>

<div class="block md:hidden">
  <!-- Visível apenas em mobile -->
</div>
```

### Estados e Pseudo-classes
```html
<!-- Hover -->
<button class="bg-blue-500 hover:bg-blue-700">
  Botão
</button>

<!-- Focus -->
<input class="focus:ring-2 focus:ring-blue-500">

<!-- Active -->
<button class="active:scale-95">
  Clique
</button>

<!-- Disabled -->
<button disabled class="disabled:opacity-50">
  Desabilitado
</button>
```

### Classes Condicionais
```html
<!-- Dark mode -->
<div class="bg-white dark:bg-gray-800">
  <!-- Branco no light, cinza escuro no dark -->
</div>

<!-- Print styles -->
<div class="print:hidden">
  <!-- Escondido na impressão -->
</div>

<!-- Reduced motion -->
<div class="motion-safe:animate-spin">
  <!-- Animação apenas se motion safe -->
</div>
```

## Classes de Utilidade

### Layout
```html
<!-- Container -->
<div class="container mx-auto px-4">
  <!-- Container centralizado com padding -->
</div>

<!-- Display -->
<div class="block md:inline lg:flex">
  <!-- Display responsivo -->
</div>

<div class="hidden sm:block">
  <!-- Escondido apenas em mobile -->
</div>

<!-- Overflow -->
<div class="overflow-auto">
  <!-- Scroll se necessário -->
</div>

<div class="overflow-hidden">
  <!-- Esconde conteúdo extra -->
</div>

<div class="overflow-x-auto">
  <!-- Scroll horizontal -->
</div>

<!-- Position -->
<div class="relative">
  <div class="absolute top-0 left-0">
    <!-- Posicionamento absoluto -->
  </div>
</div>

<div class="fixed top-0 left-0">
  <!-- Fixed position -->
</div>

<div class="sticky top-0">
  <!-- Sticky position -->
</div>

<!-- Z-index -->
<div class="z-10">
  <!-- Controla stack order -->
</div>

<div class="z-50">
  <!-- Alto z-index -->
</div>
```

### Flexbox
```html
<!-- Flex Container -->
<div class="flex">
  <!-- Display flex -->
</div>

<div class="inline-flex">
  <!-- Inline flex -->
</div>

<!-- Direction -->
<div class="flex-row">
  <!-- Padrão: horizontal -->
</div>

<div class="flex-col">
  <!-- Vertical -->
</div>

<!-- Wrap -->
<div class="flex-wrap">
  <!-- Permite quebra de linha -->
</div>

<div class="flex-nowrap">
  <!-- Sem quebra (padrão) -->
</div>

<!-- Justify Content -->
<div class="justify-start">
  <!-- Alinhamento início -->
</div>

<div class="justify-center">
  <!-- Centralizado -->
</div>

<div class="justify-between">
  <!-- Espaço entre -->
</div>

<div class="justify-around">
  <!-- Espaço ao redor -->
</div>

<div class="justify-evenly">
  <!-- Espaço uniforme -->
</div>

<!-- Align Items -->
<div class="items-start">
  <!-- Topo -->
</div>

<div class="items-center">
  <!-- Centro vertical -->
</div>

<div class="items-end">
  <!-- Base -->
</div>

<div class="items-baseline">
  <!-- Baseline -->
</div>

<div class="items-stretch">
  <!-- Esticado (padrão) -->
</div>

<!-- Align Self -->
<div class="self-start">
  <!-- Alinhamento individual -->
</div>

<!-- Flex Grow/Shrink -->
<div class="flex-1">
  <!-- Cresce para preencher -->
</div>

<div class="flex-none">
  <!-- Sem flexibilidade -->
</div>

<div class="flex-grow">
  <!-- Pode crescer -->
</div>

<div class="flex-shrink">
  <!-- Pode encolher -->
</div>

<!-- Gap -->
<div class="gap-4">
  <!-- Espaço entre itens -->
</div>

<div class="gap-x-4 gap-y-2">
  <!-- Gap específico por eixo -->
</div>
```

### Grid
```html
<!-- Grid Container -->
<div class="grid">
  <!-- Display grid -->
</div>

<div class="grid grid-cols-3 gap-4">
  <!-- 3 colunas com gap -->
</div>

<!-- Columns -->
<div class="grid-cols-1">
  <!-- 1 coluna -->
</div>

<div class="grid-cols-2">
  <!-- 2 colunas -->
</div>

<div class="grid-cols-3">
  <!-- 3 colunas -->
</div>

<div class="grid-cols-4">
  <!-- 4 colunas -->
</div>

<div class="grid-cols-12">
  <!-- 12 colunas (como Bootstrap) -->
</div>

<!-- Responsive Columns -->
<div class="grid-cols-1 md:grid-cols-2 lg:grid-cols-4">
  <!-- Colunas responsivas -->
</div>

<!-- Auto Columns -->
<div class="grid-cols-auto">
  <!-- Colunas automáticas -->
</div>

<div class="grid-cols-auto-fill">
  <!-- Preenche automaticamente -->
</div>

<div class="grid-cols-auto-fit">
  <!-- Ajusta automaticamente -->
</div>

<!-- Column Span -->
<div class="col-span-1">
  <!-- Ocupa 1 coluna -->
</div>

<div class="col-span-2">
  <!-- Ocupa 2 colunas -->
</div>

<div class="col-span-full">
  <!-- Ocupa todas as colunas -->
</div>

<div class="col-start-2 col-end-4">
  <!-- Começa na coluna 2, termina na 4 -->
</div>

<!-- Rows -->
<div class="grid-rows-3">
  <!-- 3 linhas -->
</div>

<div class="grid-rows-auto">
  <!-- Linhas automáticas -->
</div>

<!-- Row Span -->
<div class="row-span-2">
  <!-- Ocupa 2 linhas -->
</div>

<div class="row-start-2 row-end-4">
  <!-- Linhas específicas -->
</div>

<!-- Grid Flow -->
<div class="grid-flow-row">
  <!-- Padrão: preenche por linha -->
</div>

<div class="grid-flow-col">
  <!-- Preenche por coluna -->
</div>

<div class="grid-flow-row-dense">
  <!-- Dense packing -->
</div>
```

### Espaçamento (Spacing)
```html
<!-- Margin -->
<div class="m-4">
  <!-- Margin todos os lados: 1rem -->
</div>

<div class="mx-4 my-2">
  <!-- Margin horizontal e vertical -->
</div>

<div class="mt-4 mb-2 ml-3 mr-1">
  <!-- Margin específica por lado -->
</div>

<div class="-m-4">
  <!-- Margin negativo -->
</div>

<!-- Padding -->
<div class="p-4">
  <!-- Padding todos os lados -->
</div>

<div class="px-4 py-2">
  <!-- Padding horizontal e vertical -->
</div>

<div class="pt-4 pb-2 pl-3 pr-1">
  <!-- Padding específico por lado -->
</div>

<!-- Space Between -->
<div class="space-x-4">
  <!-- Espaço entre filhos horizontais -->
</div>

<div class="space-y-4">
  <!-- Espaço entre filhos verticais -->
</div>

<div class="space-y-reverse">
  <!-- Ordem reversa -->
</div>
```

### Tamanhos (Sizing)
```html
<!-- Width -->
<div class="w-4">
  <!-- Largura: 1rem -->
</div>

<div class="w-1/2">
  <!-- 50% da largura -->
</div>

<div class="w-1/3">
  <!-- 33.333% -->
</div>

<div class="w-2/3">
  <!-- 66.666% -->
</div>

<div class="w-1/4">
  <!-- 25% -->
</div>

<div class="w-3/4">
  <!-- 75% -->
</div>

<div class="w-full">
  <!-- 100% -->
</div>

<div class="w-screen">
  <!-- 100vw -->
</div>

<div class="w-min">
  <!-- min-content -->
</div>

<div class="w-max">
  <!-- max-content -->
</div>

<div class="w-fit">
  <!-- fit-content -->
</div>

<div class="w-auto">
  <!-- Automático -->
</div>

<!-- Max Width -->
<div class="max-w-xs">
  <!-- Extra pequeno: 20rem -->
</div>

<div class="max-w-sm">
  <!-- Pequeno: 24rem -->
</div>

<div class="max-w-md">
  <!-- Médio: 28rem -->
</div>

<div class="max-w-lg">
  <!-- Grande: 32rem -->
</div>

<div class="max-w-xl">
  <!-- Extra grande: 36rem -->
</div>

<div class="max-w-2xl">
  <!-- 2x large: 42rem -->
</div>

<div class="max-w-3xl">
  <!-- 3x large: 48rem -->
</div>

<div class="max-w-4xl">
  <!-- 4x large: 56rem -->
</div>

<div class="max-w-5xl">
  <!-- 5x large: 64rem -->
</div>

<div class="max-w-6xl">
  <!-- 6x large: 72rem -->
</div>

<div class="max-w-7xl">
  <!-- 7x large: 80rem -->
</div>

<div class="max-w-full">
  <!-- 100% -->
</div>

<div class="max-w-screen-sm">
  <!-- Tamanho de tela pequeno -->
</div>

<div class="max-w-screen-md">
  <!-- Tamanho de tela médio -->
</div>

<div class="max-w-screen-lg">
  <!-- Tamanho de tela grande -->
</div>

<div class="max-w-screen-xl">
  <!-- Tamanho de tela extra grande -->
</div>

<div class="max-w-screen-2xl">
  <!-- 2x extra grande -->
</div>

<!-- Min Width -->
<div class="min-w-0">
  <!-- 0px -->
</div>

<div class="min-w-full">
  <!-- 100% -->
</div>

<div class="min-w-min">
  <!-- min-content -->
</div>

<div class="min-w-max">
  <!-- max-content -->
</div>

<!-- Height -->
<div class="h-4">
  <!-- Altura: 1rem -->
</div>

<div class="h-1/2">
  <!-- 50% da altura -->
</div>

<div class="h-1/3">
  <!-- 33.333% -->
</div>

<div class="h-2/3">
  <!-- 66.666% -->
</div>

<div class="h-1/4">
  <!-- 25% -->
</div>

<div class="h-3/4">
  <!-- 75% -->
</div>

<div class="h-full">
  <!-- 100% -->
</div>

<div class="h-screen">
  <!-- 100vh -->
</div>

<div class="h-min">
  <!-- min-content -->
</div>

<div class="h-max">
  <!-- max-content -->
</div>

<div class="h-fit">
  <!-- fit-content -->
</div>

<div class="h-auto">
  <!-- Automático -->
</div>

<!-- Max Height -->
<div class="max-h-32">
  <!-- 8rem -->
</div>

<div class="max-h-full">
  <!-- 100% -->
</div>

<div class="max-h-screen">
  <!-- 100vh -->
</div>

<!-- Min Height -->
<div class="min-h-0">
  <!-- 0px -->
</div>

<div class="min-h-full">
  <!-- 100% -->
</div>

<div class="min-h-screen">
  <!-- 100vh -->
</div>
```

### Tipografia (Typography)
```html
<!-- Font Family -->
<div class="font-sans">
  <!-- Sans-serif (padrão) -->
</div>

<div class="font-serif">
  <!-- Serif -->
</div>

<div class="font-mono">
  <!-- Monospace -->
</div>

<!-- Font Size -->
<div class="text-xs">
  <!-- Extra pequeno: 0.75rem (12px) -->
</div>

<div class="text-sm">
  <!-- Pequeno: 0.875rem (14px) -->
</div>

<div class="text-base">
  <!-- Base: 1rem (16px) -->
</div>

<div class="text-lg">
  <!-- Grande: 1.125rem (18px) -->
</div>

<div class="text-xl">
  <!-- Extra grande: 1.25rem (20px) -->
</div>

<div class="text-2xl">
  <!-- 2xl: 1.5rem (24px) -->
</div>

<div class="text-3xl">
  <!-- 3xl: 1.875rem (30px) -->
</div>

<div class="text-4xl">
  <!-- 4xl: 2.25rem (36px) -->
</div>

<div class="text-5xl">
  <!-- 5xl: 3rem (48px) -->
</div>

<div class="text-6xl">
  <!-- 6xl: 3.75rem (60px) -->
</div>

<div class="text-7xl">
  <!-- 7xl: 4.5rem (72px) -->
</div>

<div class="text-8xl">
  <!-- 8xl: 6rem (96px) -->
</div>

<div class="text-9xl">
  <!-- 9xl: 8rem (128px) -->
</div>

<!-- Font Weight -->
<div class="font-thin">
  <!-- 100 -->
</div>

<div class="font-extralight">
  <!-- 200 -->
</div>

<div class="font-light">
  <!-- 300 -->
</div>

<div class="font-normal">
  <!-- 400 -->
</div>

<div class="font-medium">
  <!-- 500 -->
</div>

<div class="font-semibold">
  <!-- 600 -->
</div>

<div class="font-bold">
  <!-- 700 -->
</div>

<div class="font-extrabold">
  <!-- 800 -->
</div>

<div class="font-black">
  <!-- 900 -->
</div>

<!-- Font Style -->
<div class="italic">
  <!-- Itálico -->
</div>

<div class="not-italic">
  <!-- Não itálico -->
</div>

<!-- Letter Spacing -->
<div class="tracking-tighter">
  <!-- -0.05em -->
</div>

<div class="tracking-tight">
  <!-- -0.025em -->
</div>

<div class="tracking-normal">
  <!-- 0em (padrão) -->
</div>

<div class="tracking-wide">
  <!-- 0.025em -->
</div>

<div class="tracking-wider">
  <!-- 0.05em -->
</div>

<div class="tracking-widest">
  <!-- 0.1em -->
</div>

<!-- Line Height -->
<div class="leading-3">
  <!-- .75rem (12px) -->
</div>

<div class="leading-4">
  <!-- 1rem (16px) -->
</div>

<div class="leading-5">
  <!-- 1.25rem (20px) -->
</div>

<div class="leading-6">
  <!-- 1.5rem (24px) -->
</div>

<div class="leading-7">
  <!-- 1.75rem (28px) -->
</div>

<div class="leading-8">
  <!-- 2rem (32px) -->
</div>

<div class="leading-9">
  <!-- 2.25rem (36px) -->
</div>

<div class="leading-10">
  <!-- 2.5rem (40px) -->
</div>

<div class="leading-none">
  <!-- 1 -->
</div>

<div class="leading-tight">
  <!-- 1.25 -->
</div>

<div class="leading-snug">
  <!-- 1.375 -->
</div>

<div class="leading-normal">
  <!-- 1.5 (padrão) -->
</div>

<div class="leading-relaxed">
  <!-- 1.625 -->
</div>

<div class="leading-loose">
  <!-- 2 -->
</div>

<!-- Text Align -->
<div class="text-left">
  <!-- Alinhado à esquerda -->
</div>

<div class="text-center">
  <!-- Centralizado -->
</div>

<div class="text-right">
  <!-- Alinhado à direita -->
</div>

<div class="text-justify">
  <!-- Justificado -->
</div>

<div class="text-start">
  <!-- Início (direção do texto) -->
</div>

<div class="text-end">
  <!-- Fim (direção do texto) -->
</div>

<!-- Text Color -->
<div class="text-black">
  <!-- Preto -->
</div>

<div class="text-white">
  <!-- Branco -->
</div>

<div class="text-gray-500">
  <!-- Cinza médio -->
</div>

<div class="text-red-500">
  <!-- Vermelho -->
</div>

<div class="text-blue-500">
  <!-- Azul -->
</div>

<div class="text-green-500">
  <!-- Verde -->
</div>

<div class="text-yellow-500">
  <!-- Amarelo -->
</div>

<div class="text-purple-500">
  <!-- Roxo -->
</div>

<div class="text-pink-500">
  <!-- Rosa -->
</div>

<!-- Text Decoration -->
<div class="underline">
  <!-- Sublinhado -->
</div>

<div class="overline">
  <!-- Sobrelinhado -->
</div>

<div class="line-through">
  <!-- Tachado -->
</div>

<div class="no-underline">
  <!-- Sem decoração -->
</div>

<!-- Text Transform -->
<div class="uppercase">
  <!-- MAIÚSCULAS -->
</div>

<div class="lowercase">
  <!-- minúsculas -->
</div>

<div class="capitalize">
  <!-- Primeira Letra Maiúscula -->
</div>

<div class="normal-case">
  <!-- Case normal -->
</div>

<!-- Text Overflow -->
<div class="truncate">
  <!-- Truncate com ellipsis -->
</div>

<div class="overflow-ellipsis">
  <!-- Ellipsis -->
</div>

<div class="overflow-clip">
  <!-- Clip -->
</div>

<!-- Vertical Align -->
<div class="align-baseline">
  <!-- Baseline -->
</div>

<div class="align-top">
  <!-- Topo -->
</div>

<div class="align-middle">
  <!-- Meio -->
</div>

<div class="align-bottom">
  <!-- Base -->
</div>

<div class="align-text-top">
  <!-- Topo do texto -->
</div>

<div class="align-text-bottom">
  <!-- Base do texto -->
</div>

<!-- Whitespace -->
<div class="whitespace-normal">
  <!-- Normal -->
</div>

<div class="whitespace-nowrap">
  <!-- Sem quebra -->
</div>

<div class="whitespace-pre">
  <!-- Preserva espaços -->
</div>

<div class="whitespace-pre-line">
  <!-- Quebra de linha, colapsa espaços -->
</div>

<div class="whitespace-pre-wrap">
  <!-- Preserva espaços e quebra -->
</div>

<!-- Word Break -->
<div class="break-normal">
  <!-- Normal -->
</div>

<div class="break-words">
  <!-- Quebra palavras longas -->
</div>

<div class="break-all">
  <!-- Quebra qualquer palavra -->
</div>

<div class="break-keep">
  <!-- Mantém palavras -->
</div>

<!-- List Style -->
<ul class="list-disc">
  <!-- Marcadores redondos -->
</ul>

<ul class="list-decimal">
  <!-- Números -->
</ul>

<ul class="list-none">
  <!-- Sem marcadores -->
</ul>

<!-- List Style Position -->
<ul class="list-inside">
  <!-- Marcador dentro -->
</ul>

<ul class="list-outside">
  <!-- Marcador fora (padrão) -->
</ul>
```

### Cores (Colors)
```html
<!-- Background Colors -->
<div class="bg-white">
  <!-- Fundo branco -->
</div>

<div class="bg-black">
  <!-- Fundo preto -->
</div>

<div class="bg-transparent">
  <!-- Transparente -->
</div>

<div class="bg-current">
  <!-- Cor atual (herda text color) -->
</div>

<div class="bg-gray-50">
  <!-- Cinza muito claro -->
</div>

<div class="bg-gray-100">
  <!-- Cinza claro -->
</div>

<div class="bg-gray-200">
  <!-- Cinza médio claro -->
</div>

<div class="bg-gray-300">
  <!-- Cinza médio -->
</div>

<div class="bg-gray-400">
  <!-- Cinza médio escuro -->
</div>

<div class="bg-gray-500">
  <!-- Cinza escuro -->
</div>

<div class="bg-gray-600">
  <!-- Cinza mais escuro -->
</div>

<div class="bg-gray-700">
  <!-- Cinza muito escuro -->
</div>

<div class="bg-gray-800">
  <!-- Cinza quase preto -->
</div>

<div class="bg-gray-900">
  <!-- Cinza preto -->
</div>

<!-- Text Colors (já mostrado acima) -->
<!-- Border Colors -->
<div class="border border-gray-300">
  <!-- Borda cinza claro -->
</div>

<div class="border-2 border-blue-500">
  <!-- Borda azul -->
</div>

<!-- Divide Colors -->
<div class="divide-gray-200">
  <!-- Cor entre elementos filhos -->
</div>

<!-- Outline Colors -->
<button class="outline outline-blue-500">
  <!-- Outline azul -->
</button>

<!-- Ring Colors -->
<button class="focus:ring-2 focus:ring-blue-500">
  <!-- Ring azul no focus -->
</button>

<!-- Gradient Backgrounds -->
<div class="bg-gradient-to-r from-blue-500 to-purple-500">
  <!-- Gradiente azul para roxo -->
</div>

<div class="bg-gradient-to-br from-green-400 to-blue-500">
  <!-- Gradiente diagonal -->
</div>

<!-- Via Colors (gradientes) -->
<div class="bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500">
  <!-- Gradiente com cor intermediária -->
</div>
```

### Bordas (Borders)
```html
<!-- Border Width -->
<div class="border">
  <!-- 1px (padrão) -->
</div>

<div class="border-0">
  <!-- 0px -->
</div>

<div class="border-2">
  <!-- 2px -->
</div>

<div class="border-4">
  <!-- 4px -->
</div>

<div class="border-8">
  <!-- 8px -->
</div>

<!-- Border Width por Lado -->
<div class="border-t-2">
  <!-- Top: 2px -->
</div>

<div class="border-r-2">
  <!-- Right: 2px -->
</div>

<div class="border-b-2">
  <!-- Bottom: 2px -->
</div>

<div class="border-l-2">
  <!-- Left: 2px -->
</div>

<!-- Border Style -->
<div class="border-solid">
  <!-- Solida (padrão) -->
</div>

<div class="border-dashed">
  <!-- Tracejada -->
</div>

<div class="border-dotted">
  <!-- Pontilhada -->
</div>

<div class="border-double">
  <!-- Dupla -->
</div>

<div class="border-none">
  <!-- Sem borda -->
</div>

<!-- Border Radius -->
<div class="rounded">
  <!-- 0.25rem (4px) -->
</div>

<div class="rounded-sm">
  <!-- 0.125rem (2px) -->
</div>

<div class="rounded-md">
  <!-- 0.375rem (6px) -->
</div>

<div class="rounded-lg">
  <!-- 0.5rem (8px) -->
</div>

<div class="rounded-xl">
  <!-- 0.75rem (12px) -->
</div>

<div class="rounded-2xl">
  <!-- 1rem (16px) -->
</div>

<div class="rounded-3xl">
  <!-- 1.5rem (24px) -->
</div>

<div class="rounded-full">
  <!-- 9999px (círculo) -->
</div>

<div class="rounded-none">
  <!-- 0px -->
</div>

<!-- Border Radius por Canto -->
<div class="rounded-t-lg">
  <!-- Topo arredondado -->
</div>

<div class="rounded-r-lg">
  <!-- Direita arredondada -->
</div>

<div class="rounded-b-lg">
  <!-- Base arredondada -->
</div>

<div class="rounded-l-lg">
  <!-- Esquerda arredondada -->
</div>

<div class="rounded-tl-lg">
  <!-- Topo esquerdo -->
</div>

<div class="rounded-tr-lg">
  <!-- Topo direito -->
</div>

<div class="rounded-br-lg">
  <!-- Base direita -->
</div>

<div class="rounded-bl-lg">
  <!-- Base esquerda -->
</div>

<!-- Divide Width -->
<div class="divide-x-2">
  <!-- Borda vertical entre filhos -->
</div>

<div class="divide-y-2">
  <!-- Borda horizontal entre filhos -->
</div>

<!-- Outline -->
<div class="outline">
  <!-- Outline padrão -->
</div>

<div class="outline-0">
  <!-- Sem outline -->
</div>

<div class="outline-2">
  <!-- 2px -->
</div>

<div class="outline-4">
  <!-- 4px -->
</div>

<div class="outline-offset-2">
  <!-- Offset do outline -->
</div>

<!-- Ring -->
<div class="ring-2 ring-blue-500">
  <!-- Ring azul -->
</div>

<div class="ring-inset">
  <!-- Ring interno -->
</div>

<div class="ring-offset-2 ring-offset-white">
  <!-- Offset do ring -->
</div>
```

### Efeitos (Effects)
```html
<!-- Box Shadow -->
<div class="shadow-sm">
  <!-- Sombra pequena -->
</div>

<div class="shadow">
  <!-- Sombra padrão -->
</div>

<div class="shadow-md">
  <!-- Sombra média -->
</div>

<div class="shadow-lg">
  <!-- Sombra grande -->
</div>

<div class="shadow-xl">
  <!-- Sombra extra grande -->
</div>

<div class="shadow-2xl">
  <!-- Sombra 2x grande -->
</div>

<div class="shadow-inner">
  <!-- Sombra interna -->
</div>

<div class="shadow-none">
  <!-- Sem sombra -->
</div>

<!-- Opacity -->
<div class="opacity-0">
  <!-- 0% (transparente) -->
</div>

<div class="opacity-25">
  <!-- 25% -->
</div>

<div class="opacity-50">
  <!-- 50% -->
</div>

<div class="opacity-75">
  <!-- 75% -->
</div>

<div class="opacity-100">
  <!-- 100% (opaco) -->
</div>

<!-- Mix Blend Mode -->
<div class="mix-blend-normal">
  <!-- Normal -->
</div>

<div class="mix-blend-multiply">
  <!-- Multiply -->
</div>

<div class="mix-blend-screen">
  <!-- Screen -->
</div>

<div class="mix-blend-overlay">
  <!-- Overlay -->
</div>

<div class="mix-blend-darken">
  <!-- Darken -->
</div>

<div class="mix-blend-lighten">
  <!-- Lighten -->
</div>

<div class="mix-blend-color-dodge">
  <!-- Color dodge -->
</div>

<div class="mix-blend-color-burn">
  <!-- Color burn -->
</div>

<div class="mix-blend-hard-light">
  <!-- Hard light -->
</div>

<div class="mix-blend-soft-light">
  <!-- Soft light -->
</div>

<div class="mix-blend-difference">
  <!-- Difference -->
</div>

<div class="mix-blend-exclusion">
  <!-- Exclusion -->
</div>

<div class="mix-blend-hue">
  <!-- Hue -->
</div>

<div class="mix-blend-saturation">
  <!-- Saturation -->
</div>

<div class="mix-blend-color">
  <!-- Color -->
</div>

<div class="mix-blend-luminosity">
  <!-- Luminosity -->
</div>

<!-- Background Blend Mode -->
<div class="bg-blend-normal">
  <!-- Normal -->
</div>

<div class="bg-blend-multiply">
  <!-- Multiply -->
</div>

<div class="bg-blend-screen">
  <!-- Screen -->
</div>

<div class="bg-blend-overlay">
  <!-- Overlay -->
</div>

<div class="bg-blend-darken">
  <!-- Darken -->
</div>

<div class="bg-blend-lighten">
  <!-- Lighten -->
</div>
```

### Filtros (Filters)
```html
<!-- Blur -->
<div class="blur-none">
  <!-- Sem blur -->
</div>

<div class="blur-sm">
  <!-- Blur pequeno -->
</div>

<div class="blur">
  <!-- Blur padrão -->
</div>

<div class="blur-md">
  <!-- Blur médio -->
</div>

<div class="blur-lg">
  <!-- Blur grande -->
</div>

<div class="blur-xl">
  <!-- Blur extra grande -->
</div>

<div class="blur-2xl">
  <!-- Blur 2x grande -->
</div>

<div class="blur-3xl">
  <!-- Blur 3x grande -->
</div>

<!-- Brightness -->
<div class="brightness-0">
  <!-- 0% -->
</div>

<div class="brightness-50">
  <!-- 50% -->
</div>

<div class="brightness-75">
  <!-- 75% -->
</div>

<div class="brightness-90">
  <!-- 90% -->
</div>

<div class="brightness-95">
  <!-- 95% -->
</div>

<div class="brightness-100">
  <!-- 100% (normal) -->
</div>

<div class="brightness-105">
  <!-- 105% -->
</div>

<div class="brightness-110">
  <!-- 110% -->
</div>

<div class="brightness-125">
  <!-- 125% -->
</div>

<div class="brightness-150">
  <!-- 150% -->
</div>

<div class="brightness-200">
  <!-- 200% -->
</div>

<!-- Contrast -->
<div class="contrast-0">
  <!-- 0% -->
</div>

<div class="contrast-50">
  <!-- 50% -->
</div>

<div class="contrast-75">
  <!-- 75% -->
</div>

<div class="contrast-100">
  <!-- 100% (normal) -->
</div>

<div class="contrast-125">
  <!-- 125% -->
</div>

<div class="contrast-150">
  <!-- 150% -->
</div>

<div class="contrast-200">
  <!-- 200% -->
</div>

<!-- Grayscale -->
<div class="grayscale-0">
  <!-- 0% (normal) -->
</div>

<div class="grayscale">
  <!-- 100% -->
</div>

<!-- Hue Rotate -->
<div class="hue-rotate-0">
  <!-- 0deg -->
</div>

<div class="hue-rotate-15">
  <!-- 15deg -->
</div>

<div class="hue-rotate-30">
  <!-- 30deg -->
</div>

<div class="hue-rotate-60">
  <!-- 60deg -->
</div>

<div class="hue-rotate-90">
  <!-- 90deg -->
</div>

<div class="hue-rotate-180">
  <!-- 180deg -->
</div>

<!-- Invert -->
<div class="invert-0">
  <!-- 0% (normal) -->
</div>

<div class="invert">
  <!-- 100% -->
</div>

<!-- Saturate -->
<div class="saturate-0">
  <!-- 0% -->
</div>

<div class="saturate-50">
  <!-- 50% -->
</div>

<div class="saturate-100">
  <!-- 100% (normal) -->
</div>

<div class="saturate-150">
  <!-- 150% -->
</div>

<div class="saturate-200">
  <!-- 200% -->
</div>

<!-- Sepia -->
<div class="sepia-0">
  <!-- 0% (normal) -->
</div>

<div class="sepia">
  <!-- 100% -->
</div>

<!-- Drop Shadow -->
<div class="drop-shadow-sm">
  <!-- Sombra pequena -->
</div>

<div class="drop-shadow">
  <!-- Sombra padrão -->
</div>

<div class="drop-shadow-md">
  <!-- Sombra média -->
</div>

<div class="drop-shadow-lg">
  <!-- Sombra grande -->
</div>

<div class="drop-shadow-xl">
  <!-- Sombra extra grande -->
</div>

<div class="drop-shadow-2xl">
  <!-- Sombra 2x grande -->
</div>

<div class="drop-shadow-none">
  <!-- Sem sombra -->
</div>
```

### Transições e Animações
```html
<!-- Transition Property -->
<div class="transition-none">
  <!-- Sem transição -->
</div>

<div class="transition-all">
  <!-- Todas propriedades -->
</div>

<div class="transition">
  <!-- Propriedades padrão -->
</div>

<div class="transition-colors">
  <!-- Apenas cores -->
</div>

<div class="transition-opacity">
  <!-- Apenas opacidade -->
</div>

<div class="transition-shadow">
  <!-- Apenas sombra -->
</div>

<div class="transition-transform">
  <!-- Apenas transform -->
</div>

<!-- Transition Duration -->
<div class="duration-75">
  <!-- 75ms -->
</div>

<div class="duration-100">
  <!-- 100ms -->
</div>

<div class="duration-150">
  <!-- 150ms -->
</div>

<div class="duration-200">
  <!-- 200ms -->
</div>

<div class="duration-300">
  <!-- 300ms -->
</div>

<div class="duration-500">
  <!-- 500ms -->
</div>

<div class="duration-700">
  <!-- 700ms -->
</div>

<div class="duration-1000">
  <!-- 1000ms -->
</div>

<!-- Transition Timing Function -->
<div class="ease-linear">
  <!-- Linear -->
</div>

<div class="ease-in">
  <!-- Ease in -->
</div>

<div class="ease-out">
  <!-- Ease out -->
</div>

<div class="ease-in-out">
  <!-- Ease in-out (padrão) -->
</div>

<!-- Transition Delay -->
<div class="delay-75">
  <!-- 75ms -->
</div>

<div class="delay-100">
  <!-- 100ms -->
</div>

<div class="delay-150">
  <!-- 150ms -->
</div>

<div class="delay-200">
  <!-- 200ms -->
</div>

<div class="delay-300">
  <!-- 300ms -->
</div>

<div class="delay-500">
  <!-- 500ms -->
</div>

<div class="delay-700">
  <!-- 700ms -->
</div>

<div class="delay-1000">
  <!-- 1000ms -->
</div>

<!-- Animation -->
<div class="animate-none">
  <!-- Sem animação -->
</div>

<div class="animate-spin">
  <!-- Rotação infinita -->
</div>

<div class="animate-ping">
  <!-- Ping (pulso) -->
</div>

<div class="animate-pulse">
  <!-- Pulse (pulsar) -->
</div>

<div class="animate-bounce">
  <!-- Bounce (quicar) -->
</div>
```

### Transformações
```html
<!-- Scale -->
<div class="scale-0">
  <!-- Escala 0 -->
</div>

<div class="scale-50">
  <!-- Escala 0.5 -->
</div>

<div class="scale-75">
  <!-- Escala 0.75 -->
</div>

<div class="scale-90">
  <!-- Escala 0.9 -->
</div>

<div class="scale-95">
  <!-- Escala 0.95 -->
</div>

<div class="scale-100">
  <!-- Escala 1 (normal) -->
</div>

<div class="scale-105">
  <!-- Escala 1.05 -->
</div>

<div class="scale-110">
  <!-- Escala 1.1 -->
</div>

<div class="scale-125">
  <!-- Escala 1.25 -->
</div>

<div class="scale-150">
  <!-- Escala 1.5 -->
</div>

<!-- Scale por Eixo -->
<div class="scale-x-0">
  <!-- Escala X: 0 -->
</div>

<div class="scale-y-0">
  <!-- Escala Y: 0 -->
</div>

<!-- Rotate -->
<div class="rotate-0">
  <!-- 0deg -->
</div>

<div class="rotate-1">
  <!-- 1deg -->
</div>

<div class="rotate-2">
  <!-- 2deg -->
</div>

<div class="rotate-3">
  <!-- 3deg -->
</div>

<div class="rotate-6">
  <!-- 6deg -->
</div>

<div class="rotate-12">
  <!-- 12deg -->
</div>

<div class="rotate-45">
  <!-- 45deg -->
</div>

<div class="rotate-90">
  <!-- 90deg -->
</div>

<div class="rotate-180">
  <!-- 180deg -->
</div>

<!-- Translate -->
<div class="translate-x-0">
  <!-- Translate X: 0 -->
</div>

<div class="translate-x-1">
  <!-- 0.25rem -->
</div>

<div class="translate-x-2">
  <!-- 0.5rem -->
</div>

<div class="translate-x-4">
  <!-- 1rem -->
</div>

<div class="translate-x-8">
  <!-- 2rem -->
</div>

<div class="translate-y-0">
  <!-- Translate Y: 0 -->
</div>

<div class="translate-y-1">
  <!-- 0.25rem -->
</div>

<!-- Skew -->
<div class="skew-x-0">
  <!-- Skew X: 0deg -->
</div>

<div class="skew-x-1">
  <!-- 1deg -->
</div>

<div class="skew-x-2">
  <!-- 2deg -->
</div>

<div class="skew-x-3">
  <!-- 3deg -->
</div>

<div class="skew-x-6">
  <!-- 6deg -->
</div>

<div class="skew-x-12">
  <!-- 12deg -->
</div>

<div class="skew-y-0">
  <!-- Skew Y: 0deg -->
</div>

<!-- Transform Origin -->
<div class="origin-center">
  <!-- Centro -->
</div>

<div class="origin-top">
  <!-- Topo -->
</div>

<div class="origin-top-right">
  <!-- Topo direito -->
</div>

<div class="origin-right">
  <!-- Direita -->
</div>

<div class="origin-bottom-right">
  <!-- Base direita -->
</div>

<div class="origin-bottom">
  <!-- Base -->
</div>

<div class="origin-bottom-left">
  <!-- Base esquerda -->
</div>

<div class="origin-left">
  <!-- Esquerda -->
</div>

<div class="origin-top-left">
  <!-- Topo esquerdo -->
</div>
```

### Interatividade
```html
<!-- Cursor -->
<div class="cursor-auto">
  <!-- Auto -->
</div>

<div class="cursor-default">
  <!-- Default -->
</div>

<div class="cursor-pointer">
  <!-- Pointer (link) -->
</div>

<div class="cursor-wait">
  <!-- Wait (carregando) -->
</div>

<div class="cursor-text">
  <!-- Text -->
</div>

<div class="cursor-move">
  <!-- Move (arrastar) -->
</div>

<div class="cursor-help">
  <!-- Help -->
</div>

<div class="cursor-not-allowed">
  <!-- Not allowed -->
</div>

<div class="cursor-none">
  <!-- None -->
</div>

<div class="cursor-context-menu">
  <!-- Context menu -->
</div>

<div class="cursor-progress">
  <!-- Progress -->
</div>

<div class="cursor-cell">
  <!-- Cell -->
</div>

<div class="cursor-crosshair">
  <!-- Crosshair -->
</div>

<div class="cursor-vertical-text">
  <!-- Vertical text -->
</div>

<div class="cursor-alias">
  <!-- Alias -->
</div>

<div class="cursor-copy">
  <!-- Copy -->
</div>

<div class="cursor-no-drop">
  <!-- No drop -->
</div>

<div class="cursor-grab">
  <!-- Grab -->
</div>

<div class="cursor-grabbing">
  <!-- Grabbing -->
</div>

<div class="cursor-all-scroll">
  <!-- All scroll -->
</div>

<div class="cursor-col-resize">
  <!-- Column resize -->
</div>

<div class="cursor-row-resize">
  <!-- Row resize -->
</div>

<div class="cursor-n-resize">
  <!-- North resize -->
</div>

<div class="cursor-e-resize">
  <!-- East resize -->
</div>

<div class="cursor-s-resize">
  <!-- South resize -->
</div>

<div class="cursor-w-resize">
  <!-- West resize -->
</div>

<div class="cursor-ne-resize">
  <!-- Northeast resize -->
</div>

<div class="cursor-nw-resize">
  <!-- Northwest resize -->
</div>

<div class="cursor-se-resize">
  <!-- Southeast resize -->
</div>

<div class="cursor-sw-resize">
  <!-- Southwest resize -->
</div>

<div class="cursor-ew-resize">
  <!-- East-west resize -->
</div>

<div class="cursor-ns-resize">
  <!-- North-south resize -->
</div>

<div class="cursor-nesw-resize">
  <!-- Northeast-southwest resize -->
</div>

<div class="cursor-nwse-resize">
  <!-- Northwest-southeast resize -->
</div>

<div class="cursor-zoom-in">
  <!-- Zoom in -->
</div>

<div class="cursor-zoom-out">
  <!-- Zoom out -->
</div>

<!-- User Select -->
<div class="select-none">
  <!-- Não selecionável -->
</div>

<div class="select-text">
  <!-- Seleciona texto -->
</div>

<div class="select-all">
  <!-- Seleciona tudo com um clique -->
</div>

<div class="select-auto">
  <!-- Auto (padrão) -->
</div>

<!-- Resize -->
<div class="resize-none">
  <!-- Não redimensionável -->
</div>

<div class="resize">
  <!-- Redimensionável -->
</div>

<div class="resize-y">
  <!-- Apenas vertical -->
</div>

<div class="resize-x">
  <!-- Apenas horizontal -->
</div>

<!-- Scroll Behavior -->
<div class="scroll-auto">
  <!-- Scroll automático -->
</div>

<div class="scroll-smooth">
  <!-- Scroll suave -->
</div>

<!-- Scroll Margin -->
<div class="scroll-m-4">
  <!-- Margin de scroll -->
</div>

<div class="scroll-mt-4">
  <!-- Top margin de scroll -->
</div>

<!-- Scroll Padding -->
<div class="scroll-p-4">
  <!-- Padding de scroll -->
</div>

<div class="scroll-pt-4">
  <!-- Top padding de scroll -->
</div>

<!-- Snap Alignment -->
<div class="snap-start">
  <!-- Snap no início -->
</div>

<div class="snap-end">
  <!-- Snap no fim -->
</div>

<div class="snap-center">
  <!-- Snap no centro -->
</div>

<div class="snap-align-none">
  <!-- Sem snap -->
</div>

<!-- Snap Stop -->
<div class="snap-normal">
  <!-- Snap normal -->
</div>

<div class="snap-always">
  <!-- Snap sempre -->
</div>

<!-- Touch Action -->
<div class="touch-auto">
  <!-- Touch auto -->
</div>

<div class="touch-none">
  <!-- Desabilita touch -->
</div>

<div class="touch-pan-x">
  <!-- Pan horizontal -->
</div>

<div class="touch-pan-left">
  <!-- Pan esquerda -->
</div>

<div class="touch-pan-right">
  <!-- Pan direita -->
</div>

<div class="touch-pan-y">
  <!-- Pan vertical -->
</div>

<div class="touch-pan-up">
  <!-- Pan cima -->
</div>

<div class="touch-pan-down">
  <!-- Pan baixo -->
</div>

<div class="touch-pinch-zoom">
  <!-- Pinch zoom -->
</div>

<div class="touch-manipulation">
  <!-- Manipulação -->
</div>

<!-- Will Change -->
<div class="will-change-auto">
  <!-- Auto -->
</div>

<div class="will-change-scroll">
  <!-- Scroll -->
</div>

<div class="will-change-contents">
  <!-- Contents -->
</div>

<div class="will-change-transform">
  <!-- Transform -->
</div>
```

### SVG
```html
<!-- Fill -->
<svg class="fill-current">
  <!-- Preenche com cor atual -->
</svg>

<svg class="fill-none">
  <!-- Sem preenchimento -->
</svg>

<!-- Stroke -->
<svg class="stroke-current">
  <!-- Traço com cor atual -->
</svg>

<svg class="stroke-0">
  <!-- Traço 0px -->
</svg>

<svg class="stroke-1">
  <!-- Traço 1px -->
</svg>

<svg class="stroke-2">
  <!-- Traço 2px -->
</div>
```

## Responsividade

### Breakpoints
```html
<!-- Prefixos de Breakpoint -->
<div class="sm:text-sm md:text-base lg:text-lg xl:text-xl 2xl:text-2xl">
  <!-- Tamanhos responsivos -->
</div>

<!-- Valores padrão dos breakpoints:
sm: 640px      (mobile landscape)
md: 768px      (tablet)
lg: 1024px     (laptop)
xl: 1280px     (desktop)
2xl: 1536px    (large desktop)

Mobile First:
- Estilos sem prefixo: mobile
- sm: telas >= 640px
- md: telas >= 768px
- lg: telas >= 1024px
- xl: telas >= 1280px
- 2xl: telas >= 1536px
-->
```

### Exemplos de Responsividade
```html
<!-- Grid responsivo -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
  <!-- 1 coluna mobile, 2 tablet, 4 desktop -->
</div>

<!-- Display responsivo -->
<div class="hidden md:block">
  <!-- Escondido em mobile, visível em tablet+ -->
</div>

<div class="block md:hidden">
  <!-- Visível em mobile, escondido em tablet+ -->
</div>

<!-- Flexbox responsivo -->
<div class="flex flex-col md:flex-row">
  <!-- Coluna em mobile, linha em tablet+ -->
</div>

<!-- Padding responsivo -->
<div class="p-4 md:p-8 lg:p-12">
  <!-- Padding crescente com tamanho de tela -->
</div>

<!-- Texto responsivo -->
<h1 class="text-2xl sm:text-3xl md:text-4xl lg:text-5xl">
  Título responsivo
</h1>
```

### Container
```html
<!-- Container responsivo -->
<div class="container">
  <!-- Largura máxima com base no breakpoint -->
</div>

<!-- Container com padding -->
<div class="container px-4">
  <!-- Container com padding horizontal -->
</div>

<!-- Container centralizado -->
<div class="container mx-auto">
  <!-- Centralizado horizontalmente -->
</div>

<!-- Container fluido -->
<div class="max-w-full">
  <!-- Container que usa toda largura -->
</div>
```

### Orientation
```html
<!-- Orientação do dispositivo -->
<div class="portrait:hidden">
  <!-- Escondido em modo retrato -->
</div>

<div class="landscape:hidden">
  <!-- Escondido em modo paisagem -->
</div>
```

## Estados e Interações

### Pseudo-classes
```html
<!-- Hover -->
<button class="bg-blue-500 hover:bg-blue-700">
  <!-- Cor muda no hover -->
</button>

<!-- Focus -->
<input class="focus:ring-2 focus:ring-blue-500">
  <!-- Anel azul no focus -->
</input>

<!-- Active -->
<button class="active:scale-95">
  <!-- Escala reduzida enquanto clicado -->
</button>

<!-- Visited -->
<a class="visited:text-purple-500">
  <!-- Cor roxa após visita -->
</a>

<!-- Checked -->
<input type="checkbox" class="checked:bg-blue-500">
  <!-- Fundo azul quando marcado -->
</input>

<!-- Disabled -->
<button disabled class="disabled:opacity-50">
  <!-- Opacidade reduzida quando desabilitado -->
</button>
```

### Group e Peer
```html
<!-- Group -->
<div class="group">
  <div class="group-hover:bg-blue-500">
    <!-- Estilo aplicado quando parent tem hover -->
  </div>
</div>

<!-- Peer -->
<div class="peer">
  <!-- Elemento irmão -->
</div>
<div class="peer-hover:bg-blue-500">
  <!-- Estilo aplicado quando irmão tem hover -->
</div>

<!-- Group com estados -->
<div class="group">
  <div class="group-hover:bg-blue-500 group-focus:ring-2">
    <!-- Múltiplos estados -->
  </div>
</div>
```

### Dark Mode
```html
<!-- Classes dark: -->
<div class="bg-white dark:bg-gray-800">
  <!-- Fundo branco no light, cinza escuro no dark -->
</div>

<!-- Sistema de preferência -->
<!-- tailwind.config.js -->
darkMode: 'media' // baseado em preferência do sistema
darkMode: 'class' // baseado em classe no html

<!-- Usando classe -->
<html class="dark">
  <!-- Ativa dark mode -->
</html>

<!-- Toggle manual -->
<button onclick="document.documentElement.classList.toggle('dark')">
  Toggle Dark Mode
</button>
```

### Reduced Motion
```html
<!-- Preferência reduced-motion -->
<div class="motion-safe:animate-spin">
  <!-- Animação apenas se motion safe -->
</div>

<div class="motion-reduce:animate-none">
  <!-- Sem animação se reduced motion -->
</div>
```

## Customização

### Configuração do Tema
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    // Extender tema padrão
    extend: {
      colors: {
        'primary': '#3b82f6',
        'secondary': '#10b981',
        'custom-blue': {
          100: '#dbeafe',
          500: '#3b82f6',
          900: '#1e3a8a',
        }
      },
      spacing: {
        '128': '32rem',
        '144': '36rem',
      },
      fontFamily: {
        'sans': ['Inter', 'system-ui'],
        'mono': ['Fira Code', 'monospace'],
      },
      borderRadius: {
        '4xl': '2rem',
      }
    },
    
    // Sobrescrever tema padrão
    colors: {
      transparent: 'transparent',
      current: 'currentColor',
      black: '#000',
      white: '#fff',
      gray: {
        100: '#f7fafc',
        // ... até 900
      },
    },
  }
}
```

### Screen Customization
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      'xs': '475px',
      'sm': '640px',
      'md': '768px',
      'lg': '1024px',
      'xl': '1280px',
      '2xl': '1536px',
      // Custom breakpoint
      '3xl': '1920px',
      
      // Mobile first por padrão, mas pode ser max-width
      'max-md': {'max': '767px'},
      'max-lg': {'max': '1023px'},
      
      // Range específico
      'tablet': {'min': '640px', 'max': '1023px'},
    }
  }
}
```

### Plugins Customizados
```javascript
// Criando plugin customizado
const plugin = require('tailwindcss/plugin')

module.exports = {
  plugins: [
    plugin(function({ addUtilities, addComponents, addBase, theme }) {
      // Adicionar utilities customizadas
      addUtilities({
        '.scrollbar-hide': {
          '-ms-overflow-style': 'none',
          'scrollbar-width': 'none',
          '&::-webkit-scrollbar': {
            display: 'none'
          }
        },
        '.text-shadow': {
          'text-shadow': '2px 2px 4px rgba(0,0,0,0.5)',
        }
      })
      
      // Adicionar componentes
      addComponents({
        '.btn': {
          padding: theme('spacing.2') + ' ' + theme('spacing.4'),
          borderRadius: theme('borderRadius.md'),
          fontWeight: theme('fontWeight.semibold'),
        },
        '.btn-primary': {
          backgroundColor: theme('colors.blue.500'),
          color: theme('colors.white'),
          '&:hover': {
            backgroundColor: theme('colors.blue.600'),
          }
        }
      })
      
      // Adicionar estilos base
      addBase({
        'h1': {
          fontSize: theme('fontSize.2xl'),
          fontWeight: theme('fontWeight.bold'),
        },
        'h2': {
          fontSize: theme('fontSize.xl'),
          fontWeight: theme('fontWeight.bold'),
        }
      })
    })
  ]
}
```

### JIT (Just-In-Time) Mode
```javascript
// JIT é ativado por padrão no Tailwind v3+
// Configurações específicas do JIT
module.exports = {
  mode: 'jit', // Não necessário no v3+
  
  // Content paths para o JIT escanear
  content: [
    './src/**/*.{html,js,jsx,ts,tsx,vue}',
    './public/index.html',
  ],
  
  // Safelist para classes dinâmicas
  safelist: [
    'bg-red-500',
    'text-3xl',
    {
      pattern: /bg-(red|green|blue)-(100|500|900)/,
      variants: ['lg', 'hover', 'focus'],
    },
  ],
  
  // Otimizações
  purge: {
    enabled: process.env.NODE_ENV === 'production',
    content: ['./src/**/*.{html,js}'],
    options: {
      safelist: ['dark'], // Preserva classe dark
    }
  }
}
```

## Componentes com @apply

### Criando Componentes
```css
/* input.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .btn {
    @apply px-4 py-2 rounded font-semibold transition-colors;
  }
  
  .btn-primary {
    @apply bg-blue-500 text-white hover:bg-blue-600;
  }
  
  .btn-secondary {
    @apply bg-gray-200 text-gray-800 hover:bg-gray-300;
  }
  
  .card {
    @apply bg-white rounded-lg shadow p-6;
  }
  
  .input {
    @apply border border-gray-300 rounded px-3 py-2 
           focus:outline-none focus:ring-2 focus:ring-blue-500;
  }
}
```

```html
<!-- Usando componentes -->
<button class="btn btn-primary">
  Botão Primário
</button>

<button class="btn btn-secondary">
  Botão Secundário
</button>

<div class="card">
  Conteúdo do card
</div>

<input class="input" placeholder="Digite algo">
```

### Extendendo Componentes
```css
@layer components {
  .btn {
    @apply inline-flex items-center justify-center 
           px-4 py-2 border border-transparent 
           text-sm font-medium rounded-md 
           focus:outline-none focus:ring-2 
           focus:ring-offset-2 transition-colors;
  }
  
  .btn-lg {
    @apply px-6 py-3 text-base;
  }
  
  .btn-sm {
    @apply px-3 py-1 text-xs;
  }
  
  /* Variantes com classes combinadas */
  .btn.btn-outline {
    @apply border-gray-300 bg-transparent 
           text-gray-700 hover:bg-gray-50;
  }
  
  .btn.btn-primary.btn-outline {
    @apply border-blue-500 text-blue-600 
           hover:bg-blue-50;
  }
}
```

### Responsividade em Componentes
```css
@layer components {
  .responsive-card {
    @apply bg-white rounded shadow p-4
           md:p-6 lg:p-8;
  }
  
  .responsive-grid {
    @apply grid grid-cols-1 gap-4
           sm:grid-cols-2 md:grid-cols-3
           lg:grid-cols-4;
  }
  
  .hero-section {
    @apply py-8 px-4
           sm:py-12 sm:px-6
           lg:py-16 lg:px-8;
  }
}
```

### Estados em Componentes
```css
@layer components {
  .nav-link {
    @apply px-3 py-2 rounded-md text-sm font-medium
           text-gray-700 hover:text-gray-900 
           hover:bg-gray-50 transition-colors;
  }
  
  .nav-link.active {
    @apply bg-gray-100 text-gray-900;
  }
  
  .dropdown {
    @apply hidden absolute z-10 mt-2 w-48 
           rounded-md shadow-lg bg-white ring-1 
           ring-black ring-opacity-5;
  }
  
  .dropdown.show {
    @apply block;
  }
  
  .modal-backdrop {
    @apply fixed inset-0 bg-gray-500 bg-opacity-75 
           transition-opacity;
  }
  
  .modal {
    @apply relative transform overflow-hidden 
           rounded-lg bg-white px-4 pb-4 pt-5 
           shadow-xl transition-all sm:p-6;
  }
}
```

## Plugins Oficiais

### @tailwindcss/forms
```bash
npm install @tailwindcss/forms
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/forms')
  ]
}
```

```html
<!-- Estilização automática de forms -->
<form>
  <label class="block">
    <span class="text-gray-700">Nome</span>
    <input type="text" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm">
  </label>
  
  <label class="block">
    <span class="text-gray-700">Email</span>
    <input type="email" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm">
  </label>
  
  <button type="submit" class="mt-4 bg-blue-500 text-white px-4 py-2 rounded">
    Enviar
  </button>
</form>
```

### @tailwindcss/typography
```bash
npm install @tailwindcss/typography
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/typography')
  ]
}
```

```html
<!-- Estilização para conteúdo markdown -->
<article class="prose">
  <h1>Título do Artigo</h1>
  <p>Parágrafo com <strong>texto em negrito</strong> e <em>itálico</em>.</p>
  
  <ul>
    <li>Item da lista</li>
    <li>Outro item</li>
  </ul>
  
  <blockquote>
    Citação estilizada automaticamente
  </blockquote>
</article>

<!-- Tamanhos diferentes -->
<article class="prose prose-sm">
  <!-- Texto pequeno -->
</article>

<article class="prose prose-lg">
  <!-- Texto grande -->
</article>

<article class="prose prose-xl">
  <!-- Texto extra grande -->
</article>

<!-- Cores -->
<article class="prose prose-gray">
  <!-- Tema cinza -->
</article>

<article class="prose prose-blue">
  <!-- Tema azul -->
</article>

<!-- Dark mode -->
<article class="prose dark:prose-invert">
  <!-- Inverte cores no dark mode -->
</article>
```

### @tailwindcss/line-clamp
```bash
npm install @tailwindcss/line-clamp
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/line-clamp')
  ]
}
```

```html
<!-- Limitar número de linhas -->
<p class="line-clamp-1">
  Texto muito longo que será truncado após uma linha...
</p>

<p class="line-clamp-2">
  Texto que será truncado após duas linhas...
</p>

<p class="line-clamp-3">
  Texto que será truncado após três linhas...
</p>

<p class="line-clamp-none">
  Texto sem truncamento
</p>
```

### @tailwindcss/aspect-ratio
```bash
npm install @tailwindcss/aspect-ratio
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/aspect-ratio')
  ]
}
```

```html
<!-- Aspect ratios comuns -->
<div class="aspect-w-16 aspect-h-9">
  <img src="image.jpg" alt="16:9">
</div>

<div class="aspect-w-1 aspect-h-1">
  <img src="square.jpg" alt="1:1">
</div>

<div class="aspect-w-4 aspect-h-3">
  <img src="image.jpg" alt="4:3">
</div>

<!-- Com conteúdo overlay -->
<div class="aspect-w-16 aspect-h-9">
  <img src="background.jpg" alt="Background">
  <div class="absolute inset-0 bg-black bg-opacity-50 flex items-center justify-center">
    <p class="text-white text-lg">Overlay content</p>
  </div>
</div>
```

## Performance

### Purge CSS (Production)
```javascript
// tailwind.config.js
module.exports = {
  content: [
    './src/**/*.{html,js,jsx,ts,tsx,vue}',
    './public/index.html',
  ],
  
  // Em produção, o JIT remove automaticamente CSS não usado
  // Configurações específicas para produção
  purge: {
    enabled: process.env.NODE_ENV === 'production',
    content: ['./src/**/*.{html,js}'],
    options: {
      safelist: [
        'dark', // Preserva classe dark
        /^bg-/, // Preserva todas as classes bg-*
        /^text-/, // Preserva todas as classes text-*
      ],
      blocklist: [
        'hidden', // Remove classe hidden (se não quiser)
      ],
      keyframes: true, // Remove keyframes não usados
      fontFace: true, // Remove font-face não usados
    }
  }
}
```

### Otimização de Build
```bash
# Scripts de build otimizados
{
  "scripts": {
    "dev": "tailwindcss -i ./src/input.css -o ./dist/output.css --watch",
    "build": "NODE_ENV=production tailwindcss -i ./src/input.css -o ./dist/output.css --minify",
    "build:analyze": "TAILWIND_MODE=analyze tailwindcss -i ./src/input.css -o ./dist/output.css"
  }
}
```

### Análise de Bundle
```bash
# Instalar analyzer
npm install @tailwindcss/oxide

# Gerar análise
npx tailwindcss --help
# Mostra opções de análise

# Ver tamanho aproximado
npx tailwindcss -i ./src/input.css -o ./dist/output.css --minify --dry-run
```

### Performance Tips
```markdown
✅ Boas Práticas:
1. Use JIT (padrão no v3+)
2. Configure content paths corretamente
3. Use @apply para componentes reutilizáveis
4. Evite muitas classes dinâmicas
5. Use purge em produção

⚠️ O que Evitar:
1. Muitas classes no safelist
2. Classes geradas dinamicamente sem pattern
3. @apply para tudo (use com moderação)
4. Muitas customizações desnecessárias

🚀 Otimizações Avançadas:
1. Use CDN em desenvolvimento apenas
2. Separe CSS crítico
3. Use tree-shaking no JS
4. Configure caching apropriado
```

## Workflow e Boas Práticas

### Estrutura de Projeto
```markdown
projeto-tailwind/
├── src/
│   ├── index.html
│   ├── input.css          # Tailwind imports + CSS custom
│   ├── components/        # Componentes reutilizáveis
│   │   ├── Button.jsx/Button.vue/etc.
│   │   └── Card.jsx
│   ├── styles/           # CSS customizado adicional
│   │   └── custom.css
│   └── pages/            # Páginas/views
├── public/
│   └── index.html
├── dist/                 # Build output
│   ├── output.css
│   └── index.html
├── tailwind.config.js
├── postcss.config.js
└── package.json
```

### Convenções de Nomenclatura
```markdown
🔤 Classes:
- Use kebab-case para classes customizadas
- Prefixe com namespace (ex: tw-button)
- Mantenha consistência

🎨 Components:
- Button: .btn, .btn-primary
- Card: .card, .card-header
- Input: .input, .input-error
- Alert: .alert, .alert-success

📁 Estrutura de CSS:
@layer base { }
@layer components { }
@layer utilities { }

/* Ordem recomendada */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base { /* estilos base custom */ }
@layer components { /* componentes */ }
@layer utilities { /* utilities custom */ }
```

### Desenvolvimento com Components
```jsx
// React Component com Tailwind
function Button({ children, variant = 'primary', size = 'md' }) {
  const baseClasses = "inline-flex items-center justify-center font-medium rounded focus:outline-none focus:ring-2 focus:ring-offset-2 transition-colors"
  
  const variants = {
    primary: "bg-blue-600 text-white hover:bg-blue-700",
    secondary: "bg-gray-200 text-gray-900 hover:bg-gray-300",
    outline: "border border-gray-300 text-gray-700 hover:bg-gray-50"
  }
  
  const sizes = {
    sm: "px-3 py-1.5 text-sm",
    md: "px-4 py-2 text-base",
    lg: "px-6 py-3 text-lg"
  }
  
  const className = `${baseClasses} ${variants[variant]} ${sizes[size]}`
  
  return (
    <button className={className}>
      {children}
    </button>
  )
}
```

### Design Tokens e Configuração
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      // Design Tokens
      colors: {
        brand: {
          50: '#eff6ff',
          100: '#dbeafe',
          500: '#3b82f6',
          600: '#2563eb',
          900: '#1e3a8a',
        }
      },
      spacing: {
        '4.5': '1.125rem',
        '18': '4.5rem',
      },
      fontFamily: {
        'display': ['Inter', 'system-ui'],
        'body': ['Inter', 'system-ui'],
      },
      // Configurações específicas do projeto
      boxShadow: {
        'soft': '0 2px 15px -3px rgba(0, 0, 0, 0.07)',
        'hard': '0 4px 6px -1px rgba(0, 0, 0, 0.1)',
      }
    }
  }
}
```

## Integração com Frameworks

### React
```bash
# Create React App
npx create-react-app my-app
cd my-app
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```css
/* src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```javascript
// tailwind.config.js
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

```jsx
// Componente React
function App() {
  return (
    <div className="min-h-screen bg-gray-100">
      <header className="bg-white shadow">
        <div className="max-w-7xl mx-auto py-6 px-4">
          <h1 className="text-3xl font-bold text-gray-900">
            Tailwind com React
          </h1>
        </div>
      </header>
      <main>
        <div className="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
          <div className="px-4 py-6 sm:px-0">
            <div className="border-4 border-dashed border-gray-200 rounded-lg h-96"></div>
          </div>
        </div>
      </main>
    </div>
  )
}
```

### Vue.js
```bash
# Vue 3 com Vite
npm create vue@latest my-vue-app
cd my-vue-app
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```css
/* src/style.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```javascript
// tailwind.config.js
module.exports = {
  content: [
    "./index.html",
    "./src/**/*.{vue,js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

```vue
<!-- Componente Vue -->
<template>
  <div class="min-h-screen bg-gray-100">
    <header class="bg-white shadow">
      <div class="max-w-7xl mx-auto py-6 px-4">
        <h1 class="text-3xl font-bold text-gray-900">
          Tailwind com Vue
        </h1>
      </div>
    </header>
    <main>
      <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="px-4 py-6 sm:px-0">
          <div class="border-4 border-dashed border-gray-200 rounded-lg h-96"></div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
// Lógica do componente
</script>
```

### Next.js
```bash
# Next.js com Tailwind
npx create-next-app@latest my-next-app
# Durante setup, escolha "Yes" para Tailwind CSS
```

```css
/* app/globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      backgroundImage: {
        'gradient-radial': 'radial-gradient(var(--tw-gradient-stops))',
        'gradient-conic':
          'conic-gradient(from 180deg at 50% 50%, var(--tw-gradient-stops))',
      },
    },
  },
  plugins: [],
}
```

```jsx
// app/page.js
export default function Home() {
  return (
    <main className="flex min-h-screen flex-col items-center justify-between p-24">
      <div className="z-10 max-w-5xl w-full items-center justify-between font-mono text-sm lg:flex">
        <h1 className="text-4xl font-bold">
          Next.js + Tailwind CSS
        </h1>
      </div>
    </main>
  )
}
```

### Svelte/SvelteKit
```bash
# SvelteKit
npm create svelte@latest my-svelte-app
cd my-svelte-app
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```css
/* src/app.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```javascript
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{html,js,svelte,ts}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

```svelte
<!-- Componente Svelte -->
<script>
  // Lógica do componente
</script>

<main class="min-h-screen bg-gray-100">
  <header class="bg-white shadow">
    <div class="max-w-7xl mx-auto py-6 px-4">
      <h1 class="text-3xl font-bold text-gray-900">
        Tailwind com Svelte
      </h1>
    </div>
  </header>
  <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
    <div class="px-4 py-6 sm:px-0">
      <div class="border-4 border-dashed border-gray-200 rounded-lg h-96" />
    </div>
  </div>
</main>
```

## Recursos e Ferramentas

### Extensões VS Code
```markdown
🔧 Essenciais:
- Tailwind CSS IntelliSense
- PostCSS Language Support
- Auto Rename Tag
- IntelliSense for CSS class names

🎨 Produtividade:
- Tailwind Fold (collapse classes)
- Inline Fold (hide long classes)
- Tailwind Documentation
- CSS Peek

⚙️ Configuração:
{
  "tailwindCSS.includeLanguages": {
    "html": "html",
    "javascript": "javascript",
    "vue": "vue"
  },
  "tailwindCSS.emmetCompletions": true,
  "editor.quickSuggestions": {
    "strings": true
  }
}
```

### Ferramentas Online
```markdown
🎨 Playgrounds:
- Tailwind Play: https://play.tailwindcss.com
- Tailwind.run: https://tailwind.run
- CodePen: https://codepen.io/collection/DqBvNZ
- JSFiddle: Templates prontos

📊 Geradores:
- UI Devtools: https://ui.devtools.tech
- Tailwind Components: https://tailwindcomponents.com
- Meraki UI: https://merakiui.com
- Flowbite: https://flowbite.com

🔧 Utilitários:
- Tailwind Color Shades: https://javisperez.github.io/tailwindcolorshades
- Tailwind Cheat Sheet: https://tailwindcomponents.com/cheatsheet
- Tailwind Grid Generator: https://tailwindgrid.com
```

### Bibliotecas de Componentes
```markdown
🎯 Populares:
- Headless UI: Componentes acessíveis sem estilos
- DaisyUI: Componentes prontos para Tailwind
- Flowbite: Componentes e templates
- Tailwind UI: Componentes premium pelos criadores

🆓 Gratuitas:
- Meraki UI: Componentes free
- Tailblocks: Blocos prontos
- Kitwind: Templates gratuitos
- Preline UI: Components da Tailwind

💎 Premium:
- Tailwind UI: Oficial, por Adam Wathan
- Tailkit: Templates premium
- Cruip: Templates clean
- WickedBlocks: Templates variados
```

### Comunidades
```markdown
💬 Discord:
- Tailwind CSS: https://tailwindcss.com/discord
- Tailwind Components: Comunidade ativa
- Dev.to Tailwind: Discussões

🐦 Twitter:
- @tailwindcss
- @adamwathan (criador)
- @reinink
- #tailwindcss

📚 Aprendizado:
- Tailwind CSS Docs: Excelente documentação
- Tailwind Labs: YouTube channel
- Build UI: Cursos focados
- Refactoring UI: Livro sobre design
```

### Templates e Starters
```markdown
🚀 Starters Populares:
- Vite + Tailwind: https://github.com/vitejs/vite/tree/main/packages/create-vite
- Next.js + Tailwind: https://github.com/vercel/next.js/tree/canary/examples/with-tailwindcss
- Create React App + Tailwind: Templates oficiais
- Vue 3 + Tailwind: Starter templates

🎨 Templates Completos:
- Tailwind Toolbox: Templates free
- Tailwind Templates: Vários estilos
- Windstatic: Templates estáticos
- Astro + Tailwind: Para sites estáticos

🏢 Para Produção:
- SaaS Starter: https://shipfa.st
- Dashboard Template: https://daisyui.com
- Admin Dashboard: Várias opções
- Landing Pages: Templates otimizados
```

---

## 🎯 Roadmap de Aprendizado Tailwind

### Semana 1: Fundamentos
- [ ] Instalação e configuração
- [ ] Classes de utilitário básicas
- [ ] Layout com Flexbox e Grid
- [ ] Espaçamento e tipografia

### Semana 2: Intermediário
- [ ] Responsividade (breakpoints)
- [ ] Estados (hover, focus)
- [ ] Personalização do tema
- [ ] Componentes com @apply

### Semana 3: Avançado
- [ ] Dark mode
- [ ] Plugins customizados
- [ ] Otimização de performance
- [ ] Integração com frameworks

### Semana 4: Projeto Prático
- [ ] Criar landing page completa
- [ ] Implementar design system
- [ ] Otimizar para produção
- [ ] Deploy e análise

---

## 📈 Estatísticas e Tendências

```markdown
📊 Adoção (2024):
- 4M+ downloads semanais (npm)
- 70% dos devs frontend já usaram
- 2º framework CSS mais popular
- Crescimento de 300% desde 2020

🎯 Casos de Uso:
- Startups: 85% usam Tailwind
- Empresas: Vercel, Netflix, Shopify
- Agências: Preferido para prototipagem
- Freelancers: Maior produtividade

🚀 Tendências:
- JIT por padrão (v3+)
- Mais plugins oficiais
- Melhor suporte a CSS moderno
- Integração com mais ferramentas
```

---

## ❓ FAQ Comum

### É muito código no HTML?
```markdown
✅ Vantagens:
- Local styling (não precisa procurar CSS)
- Menor bundle size (apenas classes usadas)
- Manutenção mais fácil
- Design consistente

🔄 Alternativas:
- Use @apply para componentes reutilizáveis
- Componentes em frameworks (React/Vue)
- Extensões VS Code para organização
```

### Performance em produção?
```markdown
⚡ JIT Engine:
- Gera apenas CSS necessário
- Builds extremamente rápidos
- Tamanho otimizado automaticamente
- Cache-friendly

📦 Bundle Size:
- Projeto médio: 10-50KB (gzip)
- Bootstrap: 25-50KB + custom CSS
- CSS custom: 50-200KB+
```

### Acessibilidade?
```markdown
♿ Built-in:
- Classes para focus states
- Screen reader utilities
- Reduced motion support
- Semântica preservada

🔧 Plugins:
- @tailwindcss/forms: forms acessíveis
- Headless UI: componentes acessíveis
- ARIA attributes suportados
```

---

## 🎉 Conclusão

Tailwind CSS revolucionou como desenvolvedores trabalham com CSS, oferecendo uma abordagem utility-first que combina produtividade com flexibilidade. Com sua filosofia de "design no browser", JIT engine para performance, e ecossistema crescente, Tailwind se estabeleceu como uma das ferramentas mais importantes no desenvolvimento frontend moderno.

**Próximos Passos Recomendados:**
1. Pratique com o [Tailwind Play](https://play.tailwindcss.com)
2. Explore [Tailwind UI](https://tailwindui.com) para inspiração
3. Siga [@tailwindcss](https://twitter.com/tailwindcss) no Twitter
4. Participe da [comunidade no Discord](https://tailwindcss.com/discord)
5. Construa um projeto real para consolidar conhecimento

Lembre-se: Tailwind é uma ferramenta, não uma religião. Use o que funciona para você e seu time!
