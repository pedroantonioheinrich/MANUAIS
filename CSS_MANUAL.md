
# 📘 Manual Completo de CSS - Português do Brasil

## 📌 Índice
1. [Introdução](#introdução)
2. [Sintaxe Básica](#sintaxe-básica)
3. [Seletores](#seletores)
4. [Propriedades de Texto](#propriedades-de-texto)
5. [Box Model](#box-model)
6. [Layout e Posicionamento](#layout-e-posicionamento)
7. [Cores e Backgrounds](#cores-e-backgrounds)
8. [Flexbox](#flexbox)
9. [Grid](#grid)
10. [Animações e Transições](#animações-e-transições)
11. [Responsividade](#responsividade)
12. [Variáveis CSS](#variáveis-css)

---

## 1. Introdução <a name="introdução"></a>

CSS (Cascading Style Sheets) é uma linguagem de estilo usada para descrever a apresentação de documentos HTML. Controla o layout, cores, fontes e outros aspectos visuais.

### Formas de Adicionar CSS

```css
/* 1. CSS Externo (Recomendado) */
<link rel="stylesheet" href="estilos.css">

/* 2. CSS Interno */
<style>
    seletor {
        propriedade: valor;
    }
</style>

/* 3. CSS Inline (Evitar quando possível) */
<div style="propriedade: valor;">
```

---

## 2. Sintaxe Básica <a name="sintaxe-básica"></a>

```css
seletor {
    propriedade: valor;
    outra-propriedade: valor;
}

/* Exemplo */
p {
    color: blue;
    font-size: 16px;
}
```

### Comentários
```css
/* Este é um comentário de uma linha */

/*
  Este é um comentário
  de múltiplas linhas
*/
```

---

## 3. Seletores <a name="seletores"></a>

### 3.1 Seletores Básicos

```css
/* Seletor de elemento */
p {
    color: red;
}

/* Seletor de classe */
.classe-exemplo {
    font-weight: bold;
}

/* Seletor de ID */
#id-unico {
    background: yellow;
}

/* Seletor universal */
* {
    margin: 0;
    padding: 0;
}
```

### 3.2 Seletores Compostos

```css
/* Múltiplos seletores */
h1, h2, h3 {
    font-family: Arial;
}

/* Seletor descendente */
div p {
    color: blue;
}

/* Seletor filho direto */
ul > li {
    list-style: none;
}

/* Seletor irmão adjacente */
h1 + p {
    margin-top: 0;
}

/* Seletor geral de irmãos */
h1 ~ p {
    color: gray;
}
```

### 3.3 Seletores de Atributo

```css
/* Elemento com atributo específico */
a[target] {
    color: purple;
}

/* Atributo com valor específico */
input[type="text"] {
    border: 1px solid #ccc;
}

/* Atributo contém palavra */
div[class~="importante"] {
    background: yellow;
}

/* Atributo começa com */
a[href^="https"] {
    color: green;
}

/* Atributo termina com */
img[src$=".jpg"] {
    border: 2px solid black;
}

/* Atributo contém substring */
a[href*="google"] {
    font-weight: bold;
}
```

### 3.4 Pseudo-classes

```css
/* Interação com usuário */
a:hover {
    color: red;
}

button:active {
    transform: scale(0.98);
}

input:focus {
    outline: 2px solid blue;
}

/* Estruturais */
li:first-child {
    font-weight: bold;
}

li:last-child {
    border-bottom: none;
}

li:nth-child(odd) {
    background: #f0f0f0;
}

li:nth-child(even) {
    background: #fff;
}

li:nth-child(3n) {
    color: blue;
}

/* Estado de formulários */
input:disabled {
    opacity: 0.5;
}

input:checked {
    accent-color: green;
}
```

### 3.5 Pseudo-elementos

```css
/* Adiciona conteúdo antes/depois */
p::before {
    content: "→ ";
    color: blue;
}

p::after {
    content: " ←";
    color: red;
}

/* Estilização de texto */
p::first-letter {
    font-size: 2em;
    color: red;
}

p::first-line {
    font-weight: bold;
}

/* Seleção de texto */
::selection {
    background: yellow;
    color: black;
}

/* Placeholder de inputs */
input::placeholder {
    color: #999;
    font-style: italic;
}
```

---

## 4. Propriedades de Texto <a name="propriedades-de-texto"></a>

### 4.1 Fonte

```css
.elemento {
    /* Família da fonte */
    font-family: Arial, sans-serif;
    
    /* Tamanho da fonte */
    font-size: 16px;        /* pixels */
    font-size: 1em;         /* relativo ao elemento pai */
    font-size: 1rem;        /* relativo ao elemento raiz */
    font-size: 100%;        /* porcentagem */
    
    /* Peso da fonte */
    font-weight: normal;    /* normal, bold, bolder, lighter */
    font-weight: 400;       /* 100-900 */
    
    /* Estilo da fonte */
    font-style: normal;     /* normal, italic, oblique */
    
    /* Variantes */
    font-variant: small-caps;
    
    /* Altura da linha */
    line-height: 1.5;       /* sem unidade - múltiplo */
    line-height: 24px;      /* com unidade */
    
    /* Shorthand */
    font: italic small-caps bold 16px/1.5 Arial, sans-serif;
}
```

### 4.2 Texto

```css
.elemento {
    /* Cor do texto */
    color: #333;
    color: rgb(51, 51, 51);
    color: rgba(51, 51, 51, 0.8);
    color: hsl(0, 0%, 20%);
    
    /* Alinhamento */
    text-align: left;       /* left, right, center, justify */
    
    /* Decoração */
    text-decoration: none;  /* none, underline, overline, line-through */
    
    /* Transformação */
    text-transform: none;   /* none, capitalize, uppercase, lowercase */
    
    /* Sombra no texto */
    text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
    
    /* Espaçamento entre letras */
    letter-spacing: 1px;
    letter-spacing: -0.5px;
    
    /* Espaçamento entre palavras */
    word-spacing: 2px;
    
    /* Quebra de texto */
    word-wrap: break-word;  /* normal, break-word */
    word-break: break-all;  /* normal, break-all, keep-all */
    overflow-wrap: normal;  /* normal, break-word */
    
    /* Direção do texto */
    direction: ltr;         /* ltr, rtl */
    
    /* Espaço em branco */
    white-space: normal;    /* normal, nowrap, pre, pre-wrap, pre-line */
    
    /* Indentação */
    text-indent: 20px;
}
```

### 4.3 Google Fonts

```css
/* Importando no CSS */
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

/* Usando a fonte */
body {
    font-family: 'Roboto', sans-serif;
}
```

---

## 5. Box Model <a name="box-model"></a>

Todo elemento HTML é uma "caixa" com as seguintes propriedades:

```
┌─────────────────────────────────────────┐
│                Margin                    │
│  ┌───────────────────────────────────┐  │
│  │             Border                │  │
│  │  ┌─────────────────────────────┐  │  │
│  │  │          Padding            │  │  │
│  │  │  ┌───────────────────────┐  │  │  │
│  │  │  │       Conteúdo        │  │  │  │
│  │  │  └───────────────────────┘  │  │  │
│  │  └─────────────────────────────┘  │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

### 5.1 Margin (Margem)

```css
.elemento {
    /* Individualmente */
    margin-top: 10px;
    margin-right: 15px;
    margin-bottom: 10px;
    margin-left: 15px;
    
    /* Shorthand (sentido horário: top, right, bottom, left) */
    margin: 10px;                    /* todos os lados */
    margin: 10px 20px;              /* top/bottom = 10px, right/left = 20px */
    margin: 10px 20px 15px;        /* top=10px, right/left=20px, bottom=15px */
    margin: 10px 20px 15px 5px;    /* top, right, bottom, left */
    
    /* Valores especiais */
    margin: auto;                   /* centraliza horizontalmente */
    margin: 0 auto;                /* remove margem vertical, centraliza horizontal */
}
```

### 5.2 Padding (Preenchimento)

```css
.elemento {
    /* Individualmente */
    padding-top: 10px;
    padding-right: 15px;
    padding-bottom: 10px;
    padding-left: 15px;
    
    /* Shorthand (mesma lógica do margin) */
    padding: 10px;
    padding: 10px 20px;
    padding: 10px 20px 15px;
    padding: 10px 20px 15px 5px;
}
```

### 5.3 Border (Borda)

```css
.elemento {
    /* Estilo da borda */
    border-style: solid;    /* none, hidden, dotted, dashed, solid, double, groove, ridge, inset, outset */
    
    /* Largura da borda */
    border-width: 2px;      /* px, em, rem, thin, medium, thick */
    
    /* Cor da borda */
    border-color: #333;
    
    /* Bordas individuais */
    border-top-style: dashed;
    border-right-width: 3px;
    border-bottom-color: red;
    border-left-color: blue;
    
    /* Shorthand */
    border: 2px solid #333; /* width style color */
    
    /* Bordas arredondadas */
    border-radius: 10px;           /* todos os cantos */
    border-radius: 10px 5px;      /* top-left/bottom-right = 10px, top-right/bottom-left = 5px */
    border-radius: 10px 5px 15px 20px; /* cada canto individualmente */
    border-radius: 50%;            /* círculo */
    
    /* Bordas individuais arredondadas */
    border-top-left-radius: 10px;
    border-top-right-radius: 5px;
    border-bottom-right-radius: 15px;
    border-bottom-left-radius: 20px;
}
```

### 5.4 Box-Sizing

```css
.elemento {
    /* Modelo tradicional: width/height = conteúdo apenas */
    box-sizing: content-box; /* padrão */
    
    /* Modelo moderno: width/height = conteúdo + padding + border */
    box-sizing: border-box; /* recomendado */
    
    /* Aplicando globalmente */
    * {
        box-sizing: border-box;
    }
}
```

### 5.5 Outline

```css
.elemento {
    outline: 2px solid blue;
    outline-offset: 2px; /* Espaço entre borda e outline */
}
```

---

## 6. Layout e Posicionamento <a name="layout-e-posicionamento"></a>

### 6.1 Display

```css
.elemento {
    /* Valores principais */
    display: block;           /* Ocupa toda a largura, quebra linha */
    display: inline;          /* Só ocupa espaço necessário, não quebra linha */
    display: inline-block;    /* Mix: inline com propriedades de block */
    display: none;            /* Remove do fluxo, invisível */
    
    /* Valores modernos */
    display: flex;            /* Container flexível */
    display: grid;            /* Container grid */
    display: inline-flex;     /* Flex inline */
    display: inline-grid;     /* Grid inline */
}
```

### 6.2 Position

```css
.elemento {
    /* Posicionamento estático (padrão) */
    position: static; /* Não aceita top, right, bottom, left, z-index */
    
    /* Posicionamento relativo */
    position: relative; /* Desloca em relação à sua posição original */
    top: 10px;
    left: 20px;
    
    /* Posicionamento absoluto */
    position: absolute; /* Em relação ao ancestral posicionado mais próximo */
    top: 0;
    right: 0;
    
    /* Posicionamento fixo */
    position: fixed; /* Em relação à viewport */
    bottom: 20px;
    right: 20px;
    
    /* Posicionamento sticky */
    position: sticky; /* Alterna entre relative e fixed */
    top: 0; /* Fixa quando atinge este ponto */
}
```

### 6.3 Float

```css
.imagem {
    float: left;      /* left, right, none */
    margin: 0 15px 15px 0;
}

/* Limpar floats */
.container::after {
    content: "";
    display: table;
    clear: both;
}
```

### 6.4 Z-Index

```css
.elemento {
    z-index: 10; /* Controla a profundidade (apenas para elementos posicionados) */
}
```

### 6.5 Overflow

```css
.container {
    overflow: visible;  /* padrão - conteúdo vaza */
    overflow: hidden;   /* esconde o conteúdo que vaza */
    overflow: scroll;   /* sempre mostra scrollbars */
    overflow: auto;     /* mostra scrollbars apenas se necessário */
    
    /* Eixos específicos */
    overflow-x: hidden;
    overflow-y: scroll;
}
```

### 6.6 Visibility

```css
.elemento {
    visibility: visible;   /* padrão - visível */
    visibility: hidden;    /* invisível mas mantém espaço */
    visibility: collapse;  /* para tabelas */
}
```

---

## 7. Cores e Backgrounds <a name="cores-e-backgrounds"></a>

### 7.1 Formatos de Cores

```css
.elemento {
    /* Nome da cor */
    color: red;
    
    /* Hexadecimal */
    color: #ff0000;        /* vermelho */
    color: #f00;           /* forma abreviada */
    color: #ff000080;      /* com transparência (8 dígitos) */
    
    /* RGB */
    color: rgb(255, 0, 0); /* vermelho */
    color: rgb(255 0 0);   /* nova sintaxe */
    
    /* RGBA */
    color: rgba(255, 0, 0, 0.5); /* vermelho 50% transparente */
    color: rgb(255 0 0 / 0.5);   /* nova sintaxe */
    
    /* HSL */
    color: hsl(0, 100%, 50%);     /* hsl(hue, saturation, lightness) */
    color: hsl(0 100% 50%);       /* nova sintaxe */
    color: hsl(0 100% 50% / 0.5); /* com transparência */
    
    /* Palavras-chave especiais */
    color: transparent;           /* totalmente transparente */
    color: currentColor;          /* usa a cor do elemento pai */
}
```

### 7.2 Background

```css
.elemento {
    /* Cor de fundo */
    background-color: #f0f0f0;
    
    /* Imagem de fundo */
    background-image: url('imagem.jpg');
    background-image: linear-gradient(to right, red, blue);
    background-image: radial-gradient(circle, red, blue);
    
    /* Repetição */
    background-repeat: repeat;    /* padrão - repete em ambos eixos */
    background-repeat: no-repeat; /* não repete */
    background-repeat: repeat-x;  /* repete apenas horizontalmente */
    background-repeat: repeat-y;  /* repete apenas verticalmente */
    background-repeat: space;     /* repete com espaçamento igual */
    background-repeat: round;     /* repete e redimensiona para caber */
    
    /* Posição */
    background-position: left top;       /* palavras-chave */
    background-position: 50% 50%;        /* porcentagens */
    background-position: 100px 200px;    /* valores absolutos */
    
    /* Tamanho */
    background-size: auto;               /* tamanho original */
    background-size: cover;              /* cobre todo o container */
    background-size: contain;            /* mostra imagem inteira */
    background-size: 100px 200px;        /* largura altura */
    background-size: 50% 75%;            /* porcentagem do container */
    
    /* Fixar imagem */
    background-attachment: scroll; /* padrão - rola com conteúdo */
    background-attachment: fixed;  /* fixa na viewport */
    background-attachment: local;  /* rola com conteúdo do elemento */
    
    /* Origem e recorte */
    background-origin: padding-box; /* padrão - relativo ao padding */
    background-origin: border-box;  /* relativo à borda */
    background-origin: content-box; /* relativo ao conteúdo */
    
    background-clip: border-box;   /* padrão - aparece atrás da borda */
    background-clip: padding-box;  /* corta no padding */
    background-clip: content-box;  /* corta no conteúdo */
    background-clip: text;         /* preenche o texto */
    
    /* Shorthand */
    background: #f0f0f0 url('imagem.jpg') no-repeat center/cover fixed;
    /* color image repeat attachment position/size origin clip */
}
```

### 7.3 Gradients

```css
.elemento {
    /* Linear Gradient */
    background: linear-gradient(45deg, red, blue);
    background: linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet);
    background: linear-gradient(to bottom, transparent, rgba(0,0,0,0.5));
    
    /* Radial Gradient */
    background: radial-gradient(circle, red, blue);
    background: radial-gradient(circle at top left, red, blue);
    
    /* Conic Gradient */
    background: conic-gradient(red, yellow, green, blue, red);
    background: conic-gradient(from 45deg, red, blue);
    
    /* Gradientes repetidos */
    background: repeating-linear-gradient(45deg, red, red 10px, blue 10px, blue 20px);
    background: repeating-radial-gradient(circle, red, red 10px, blue 10px, blue 20px);
}
```

---

## 8. Flexbox <a name="flexbox"></a>

### 8.1 Container Flex (Parent)

```css
.container {
    display: flex; /* ou inline-flex */
    
    /* Direção dos itens */
    flex-direction: row;           /* padrão - esquerda para direita */
    flex-direction: row-reverse;   /* direita para esquerda */
    flex-direction: column;        /* topo para base */
    flex-direction: column-reverse;/* base para topo */
    
    /* Quebra de linha */
    flex-wrap: nowrap;             /* padrão - uma linha */
    flex-wrap: wrap;               /* múltiplas linhas */
    flex-wrap: wrap-reverse;       /* múltiplas linhas reversas */
    
    /* Shorthand para direction + wrap */
    flex-flow: row wrap;
    
    /* Alinhamento horizontal (eixo principal) */
    justify-content: flex-start;   /* padrão - início */
    justify-content: flex-end;     /* fim */
    justify-content: center;       /* centro */
    justify-content: space-between;/* espaço entre itens */
    justify-content: space-around; /* espaço ao redor */
    justify-content: space-evenly; /* espaço igual */
    
    /* Alinhamento vertical (eixo cruzado) */
    align-items: stretch;          /* padrão - estica para preencher */
    align-items: flex-start;       /* início */
    align-items: flex-end;         /* fim */
    align-items: center;           /* centro */
    align-items: baseline;         /* alinha pela base do texto */
    
    /* Alinhamento de múltiplas linhas */
    align-content: stretch;        /* padrão */
    align-content: flex-start;
    align-content: flex-end;
    align-content: center;
    align-content: space-between;
    align-content: space-around;
    align-content: space-evenly;
}
```

### 8.2 Itens Flex (Children)

```css
.item {
    /* Ordem de exibição */
    order: 0; /* padrão - ordem no HTML */
    order: 1; /* números maiores aparecem depois */
    order: -1; /* números menores aparecem antes */
    
    /* Capacidade de crescer */
    flex-grow: 0; /* padrão - não cresce */
    flex-grow: 1; /* cresce para preencher espaço disponível */
    
    /* Capacidade de encolher */
    flex-shrink: 1; /* padrão - encolhe se necessário */
    flex-shrink: 0; /* não encolhe */
    
    /* Tamanho base */
    flex-basis: auto; /* padrão - tamanho natural */
    flex-basis: 200px; /* tamanho fixo */
    flex-basis: 50%; /* porcentagem do container */
    
    /* Shorthand para grow, shrink e basis */
    flex: 0 1 auto; /* padrão */
    flex: 1; /* flex-grow: 1, shrink: 1, basis: 0 */
    flex: 200px; /* basis: 200px */
    flex: 2 0 100px; /* grow: 2, shrink: 0, basis: 100px */
    
    /* Alinhamento individual */
    align-self: auto; /* segue o align-items do container */
    align-self: flex-start;
    align-self: flex-end;
    align-self: center;
    align-self: stretch;
    align-self: baseline;
}
```

---

## 9. Grid <a name="grid"></a>

### 9.1 Container Grid (Parent)

```css
.container {
    display: grid; /* ou inline-grid */
    
    /* Definição de colunas */
    grid-template-columns: 100px 200px 100px; /* 3 colunas fixas */
    grid-template-columns: 1fr 2fr 1fr;      /* frações do espaço */
    grid-template-columns: repeat(3, 1fr);   /* 3 colunas iguais */
    grid-template-columns: 200px auto 100px; /* colunas mistas */
    grid-template-columns: minmax(100px, 1fr) 2fr; /* mínimo e máximo */
    
    /* Definição de linhas */
    grid-template-rows: 100px 200px; /* 2 linhas fixas */
    grid-template-rows: repeat(3, minmax(100px, auto)); /* mínimo 100px */
    
    /* Definição de áreas */
    grid-template-areas:
        "header header header"
        "sidebar content content"
        "footer footer footer";
    
    /* Gaps (espaçamento entre células) */
    gap: 10px;               /* row-gap e column-gap */
    row-gap: 15px;
    column-gap: 20px;
    
    /* Alinhamento de conteúdo na grade */
    justify-content: start;  /* start, end, center, stretch, space-around, space-between, space-evenly */
    align-content: center;   /* start, end, center, stretch, space-around, space-between, space-evenly */
    
    /* Alinhamento de itens dentro das células */
    justify-items: stretch;  /* start, end, center, stretch */
    align-items: stretch;    /* start, end, center, stretch */
    
    /* Auto colunas e linhas */
    grid-auto-columns: 100px; /* tamanho de colunas criadas automaticamente */
    grid-auto-rows: minmax(100px, auto);
    grid-auto-flow: row;     /* row, column, row dense, column dense */
}
```

### 9.2 Itens Grid (Children)

```css
.item {
    /* Posicionamento por linha/coluna */
    grid-column-start: 1;
    grid-column-end: 3;
    grid-row-start: 1;
    grid-row-end: 2;
    
    /* Shorthand para coluna e linha */
    grid-column: 1 / 3;      /* start / end */
    grid-row: 1 / 2;
    
    /* Span (abrange células) */
    grid-column: 1 / span 2; /* começa na 1, abrange 2 colunas */
    grid-row: span 2;        /* abrange 2 linhas */
    
    /* Área nomeada */
    grid-area: header;       /* nome definido em grid-template-areas */
    
    /* Posicionamento individual na célula */
    justify-self: start;     /* start, end, center, stretch */
    align-self: center;      /* start, end, center, stretch */
    
    /* Shorthand para justify-self e align-self */
    place-self: center start;
}
```

---

## 10. Animações e Transições <a name="animações-e-transições"></a>

### 10.1 Transition

```css
.botao {
    background: blue;
    transition: background 0.3s ease;
}

.botao:hover {
    background: red;
}

.elemento {
    /* Propriedades individuais */
    transition-property: all;        /* todas as propriedades animáveis */
    transition-property: opacity, transform; /* propriedades específicas */
    
    transition-duration: 0.3s;       /* duração da transição */
    transition-duration: 300ms;      /* em milissegundos */
    
    transition-timing-function: ease;            /* padrão */
    transition-timing-function: linear;          /* velocidade constante */
    transition-timing-function: ease-in;         /* começa devagar */
    transition-timing-function: ease-out;        /* termina devagar */
    transition-timing-function: ease-in-out;     /* começa e termina devagar */
    transition-timing-function: cubic-bezier(0.1, 0.7, 1.0, 0.1); /* curva personalizada */
    
    transition-delay: 0s;            /* atraso para iniciar */
    transition-delay: 0.2s;
    
    /* Shorthand */
    transition: all 0.3s ease 0s;
    transition: opacity 0.3s ease, transform 0.5s linear;
}
```

### 10.2 Transform

```css
.elemento {
    /* Translação (movimento) */
    transform: translateX(100px);    /* horizontal */
    transform: translateY(-50px);    /* vertical */
    transform: translate(100px, 50px); /* ambas */
    transform: translate(50%, -50%); /* porcentagem relativa ao próprio tamanho */
    
    /* Escala */
    transform: scale(1.5);           /* ambas direções */
    transform: scaleX(2);            /* horizontal */
    transform: scaleY(0.5);          /* vertical */
    transform: scale(2, 0.5);        /* horizontal e vertical */
    
    /* Rotação */
    transform: rotate(45deg);        /* graus */
    transform: rotate(0.5turn);      /* voltas */
    transform: rotate(1.57rad);      /* radianos */
    
    /* Inclinação (skew) */
    transform: skewX(20deg);         /* horizontal */
    transform: skewY(10deg);         /* vertical */
    transform: skew(20deg, 10deg);   /* ambas */
    
    /* Ponto de origem da transformação */
    transform-origin: center;        /* padrão */
    transform-origin: top left;
    transform-origin: 100px 200px;
    transform-origin: 75% 25%;
    
    /* Múltiplas transformações */
    transform: translateX(100px) rotate(45deg) scale(1.5);
    
    /* 3D */
    transform: perspective(500px) rotateX(45deg) rotateY(30deg);
    transform-style: preserve-3d; /* mantém contexto 3D para filhos */
}
```

### 10.3 Animation

```css
@keyframes exemplo {
    /* Definição com from/to */
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
    
    /* Definição com porcentagens */
    0% {
        transform: translateX(0);
    }
    50% {
        transform: translateX(100px);
    }
    100% {
        transform: translateX(0);
    }
}

.elemento {
    /* Propriedades da animação */
    animation-name: exemplo;         /* nome do keyframe */
    animation-duration: 2s;          /* duração */
    animation-timing-function: ease; /* função de temporização */
    animation-delay: 1s;             /* atraso inicial */
    animation-iteration-count: 1;    /* número de repetições: 1, 2, infinite */
    animation-direction: normal;     /* normal, reverse, alternate, alternate-reverse */
    animation-fill-mode: none;       /* none, forwards, backwards, both */
    animation-play-state: running;   /* running, paused */
    
    /* Shorthand */
    animation: exemplo 2s ease 1s infinite alternate running;
}

/* Animações de entrada */
@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

@keyframes slideIn {
    from { transform: translateX(-100%); }
    to { transform: translateX(0); }
}

@keyframes bounce {
    0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
    40% { transform: translateY(-30px); }
    60% { transform: translateY(-15px); }
}
```

---

## 11. Responsividade <a name="responsividade"></a>

### 11.1 Media Queries

```css
/* Dispositivos móveis primeiro (mobile-first) */
.elemento {
    padding: 10px;
}

/* Tablets */
@media (min-width: 768px) {
    .elemento {
        padding: 20px;
    }
}

/* Desktops */
@media (min-width: 1024px) {
    .elemento {
        padding: 30px;
    }
}

/* Desktop grandes */
@media (min-width: 1200px) {
    .elemento {
        padding: 40px;
    }
}

/* Outros tipos de media queries */
@media screen and (max-width: 600px) {
    /* Apenas para telas com largura máxima de 600px */
}

@media print {
    /* Estilos para impressão */
    body {
        font-size: 12pt;
        color: black;
    }
}

@media (orientation: portrait) {
    /* Dispositivo em modo retrato */
}

@media (orientation: landscape) {
    /* Dispositivo em modo paisagem */
}

@media (prefers-color-scheme: dark) {
    /* Modo escuro preferido */
    body {
        background: #121212;
        color: white;
    }
}

@media (prefers-reduced-motion: reduce) {
    /* Usuário prefere reduzir animações */
    * {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
    }
}

/* Operadores lógicos */
@media (min-width: 600px) and (max-width: 1200px) {
    /* Entre 600px e 1200px */
}

@media (max-width: 600px), (min-width: 1200px) {
    /* Menor que 600px OU maior que 1200px */
}

@media not screen and (color) {
    /* Dispositivos sem cor */
}
```

### 11.2 Unidades Responsivas

```css
.elemento {
    /* Unidades relativas à viewport */
    width: 100vw;      /* 100% da largura da viewport */
    height: 50vh;      /* 50% da altura da viewport */
    font-size: 5vmin;  /* 5% da menor dimensão da viewport */
    font-size: 5vmax;  /* 5% da maior dimensão da viewport */
    
    /* Unidades relativas ao elemento pai */
    width: 50%;        /* 50% do elemento pai */
    font-size: 2em;    /* 2x o font-size do elemento pai */
    font-size: 2rem;   /* 2x o font-size do elemento raiz (html) */
    
    /* Unidades flexíveis */
    width: calc(100% - 20px); /* cálculo matemático */
    width: min(100%, 1200px); /* o menor valor */
    width: max(300px, 50%);   /* o maior valor */
    width: clamp(300px, 50%, 1200px); /* mínimo, ideal, máximo */
}
```

### 11.3 Imagens Responsivas

```css
img {
    max-width: 100%;   /* Nunca ultrapassa o container */
    height: auto;      /* Mantém proporção */
}

/* Picture element no HTML */
<picture>
    <source media="(min-width: 1200px)" srcset="imagem-grande.jpg">
    <source media="(min-width: 768px)" srcset="imagem-media.jpg">
    <img src="imagem-pequena.jpg" alt="Descrição">
</picture>

/* Background images responsivas */
.hero {
    background-image: url('imagem-pequena.jpg');
}

@media (min-width: 768px) {
    .hero {
        background-image: url('imagem-media.jpg');
    }
}

@media (min-width: 1200px) {
    .hero {
        background-image: url('imagem-grande.jpg');
    }
}
```

---

## 12. Variáveis CSS (Custom Properties) <a name="variáveis-css"></a>

```css
:root {
    /* Definindo variáveis */
    --cor-primaria: #3498db;
    --cor-secundaria: #2ecc71;
    --espacamento: 20px;
    --fonte-principal: 'Arial', sans-serif;
    --tamanho-fonte: 16px;
    --raio-borda: 8px;
    
    /* Tema escuro/claro */
    --bg-color: white;
    --text-color: black;
}

/* Usando variáveis */
.elemento {
    color: var(--cor-primaria);
    background: var(--bg-color);
    padding: var(--espacamento);
    font-family: var(--fonte-principal);
    font-size: var(--tamanho-fonte);
    border-radius: var(--raio-borda);
}

/* Fallback value */
.elemento {
    color: var(--cor-inexistente, blue); /* Se não existir, usa blue */
}

/* Alterando variáveis em diferentes contextos */
@media (prefers-color-scheme: dark) {
    :root {
        --bg-color: #121212;
        --text-color: white;
    }
}

.container {
    --espacamento: 10px; /* Sobrescreve para este container e filhos */
}

/* Variáveis em cálculos */
.elemento {
    --largura-base: 100px;
    width: calc(var(--largura-base) * 2);
}

/* JavaScript com variáveis CSS */
// Definir
element.style.setProperty('--cor-primaria', '#ff0000');

// Obter
getComputedStyle(element).getPropertyValue('--cor-primaria');
```

---

## 🎯 Boas Práticas

### 1. Organização do Código
```css
/* 1. Reset/Base */
* { box-sizing: border-box; }
html, body { margin: 0; padding: 0; }

/* 2. Variáveis */
:root { --cor-primaria: #333; }

/* 3. Tipografia */
body { font-family: Arial; }

/* 4. Componentes */
.botao { /* estilos */ }
.card { /* estilos */ }

/* 5. Layout */
.header { /* estilos */ }
.footer { /* estilos */ }

/* 6. Utilidades */
.text-center { text-align: center; }
.mt-20 { margin-top: 20px; }

/* 7. Media Queries */
@media (min-width: 768px) { /* estilos */ }
```

### 2. Nomenclatura (BEM - Block Element Modifier)
```css
/* Bloco */
.card { }

/* Elemento (parte do bloco) */
.card__imagem { }
.card__titulo { }
.card__texto { }

/* Modificador (variação do bloco/elemento) */
.card--destaque { }
.card__botao--grande { }
```

### 3. Performance
```css
/* Evitar */
* { } /* Seletor universal pesado */
div p span a { } /* Seletores muito específicos */

/* Preferir */
.classe-especifica { }
#id-unico { }

/* Minimizar reflows */
.elemento {
    transform: translateX(100px); /* Performance: usa GPU */
    /* Melhor que: */
    margin-left: 100px; /* Causa reflow */
}
```

---

## 📚 Recursos Adicionais

### Ferramentas Úteis
- [Can I Use](https://caniuse.com/) - Compatibilidade entre navegadores
- [CSS Tricks](https://css-tricks.com/) - Tutoriais e dicas
- [MDN Web Docs](https://developer.mozilla.org/pt-BR/docs/Web/CSS) - Documentação oficial
- [CSS Gradient Generator](https://cssgradient.io/) - Gerador de gradientes
- [Flexbox Froggy](https://flexboxfroggy.com/) - Aprenda Flexbox jogando
- [CSS Grid Garden](https://cssgridgarden.com/) - Aprenda Grid jogando

### Frameworks CSS Populares
- **Bootstrap** - Framework completo com componentes
- **Tailwind CSS** - Framework utility-first
- **Bulma** - Framework baseado em Flexbox
- **Foundation** - Framework responsivo
- **Materialize** - Baseado no Material Design do Google

---
