# Manual Completo de HTML

## Índice
1. [Introdução ao HTML](#introdução-ao-html)
2. [Estrutura Básica](#estrutura-básica)
3. [Elementos de Cabeçalho](#elementos-de-cabeçalho)
4. [Elementos de Texto](#elementos-de-texto)
5. [Listas](#listas)
6. [Links e Âncoras](#links-e-âncoras)
7. [Imagens e Mídia](#imagens-e-mídia)
8. [Tabelas](#tabelas)
9. [Formulários](#formulários)
10. [Elementos Semânticos](#elementos-semânticos)
11. [Elementos de Seção](#elementos-de-seção)
12. [Metadados](#metadados)
13. [Atributos Globais](#atributos-globais)
14. [HTML5 APIs](#html5-apis)
15. [Acessibilidade](#acessibilidade)
16. [SEO](#seo)
17. [Boas Práticas](#boas-práticas)
18. [Recursos e Ferramentas](#recursos-e-ferramentas)

## Introdução ao HTML

### O que é HTML?
HTML (HyperText Markup Language) é a linguagem de marcação padrão para criar páginas web. Descreve a estrutura de uma página web semanticamente e originalmente incluía elementos para a aparência do documento.

### História e Versões
- **HTML 1.0** (1993): Primeira versão
- **HTML 2.0** (1995): Primeira versão padrão
- **HTML 3.2** (1997): Suporte para tabelas, applets
- **HTML 4.01** (1999): Padrão final do HTML4
- **XHTML** (2000): HTML como aplicação XML
- **HTML5** (2014): Padrão atual com novos elementos e APIs

### Características do HTML5
- Novos elementos semânticos
- Suporte nativo a áudio e vídeo
- Canvas para gráficos
- APIs para aplicações web
- Melhor suporte a dispositivos móveis
- Armazenamento local

## Estrutura Básica

### Documento HTML Mínimo
```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Título da Página</title>
</head>
<body>
    <h1>Meu Primeiro HTML</h1>
    <p>Este é um parágrafo.</p>
</body>
</html>
```

### Declaração DOCTYPE
```html
<!DOCTYPE html>
```
- Deve ser a primeira linha do documento
- Define a versão do HTML (HTML5)
- Não diferencia maiúsculas/minúsculas
- Garante modo padrão nos navegadores

### Elemento HTML
```html
<html lang="pt-BR">
```
- Elemento raiz do documento
- `lang` define o idioma da página
- Atributos comuns: `lang`, `dir` (direção do texto)

### Cabeçalho (Head)
```html
<head>
    <meta charset="UTF-8">
    <title>Minha Página</title>
    <!-- Outros metadados -->
</head>
```
- Contém metadados sobre o documento
- Não é exibido na página
- Inclui título, codificação, links, scripts

### Corpo (Body)
```html
<body>
    <!-- Conteúdo visível da página -->
</body>
```
- Contém todo o conteúdo visível
- Pode ter atributos como `class`, `id`, `style`

## Elementos de Cabeçalho

### Títulos (h1-h6)
```html
<h1>Título Principal</h1>
<h2>Subtítulo</h2>
<h3>Sub-subtítulo</h3>
<h4>Título nível 4</h4>
<h5>Título nível 5</h5>
<h6>Título nível 6</h6>
```

**Melhores práticas:**
- Use apenas um `<h1>` por página
- Mantenha hierarquia lógica (não pule níveis)
- `<h1>` deve descrever o conteúdo principal

### Parágrafos
```html
<p>Este é um parágrafo de texto.</p>
<p>Outro parágrafo com <strong>texto importante</strong>.</p>
```

### Quebras de Linha
```html
<p>Primeira linha.<br>Segunda linha.</p>
<p>Terceira linha.<br>Quarta linha.</p>
```

### Linha Horizontal
```html
<p>Texto acima da linha.</p>
<hr>
<p>Texto abaixo da linha.</p>
```

### Pré-formatado
```html
<pre>
  function hello() {
    console.log("Hello World!");
  }
</pre>
```

### Citação em Bloco
```html
<blockquote cite="https://exemplo.com">
  <p>Esta é uma citação longa de outra fonte.</p>
  <footer>— Autor da Citação</footer>
</blockquote>
```

### Citação em Linha
```html
<p>Segundo <cite>Steve Jobs</cite>: 
<q>Design não é apenas como algo parece, mas como funciona.</q></p>
```

### Abreviação
```html
<p>A <abbr title="World Wide Web">WWW</abbr> foi inventada por Tim Berners-Lee.</p>
```

### Endereço
```html
<address>
  Escrito por <a href="mailto:autor@exemplo.com">João Silva</a>.<br>
  Visite-nos em:<br>
  Exemplo.com<br>
  Rua Principal, 123<br>
  São Paulo, SP
</address>
```

## Elementos de Texto

### Formatação Básica
```html
<p>Texto <b>negrito</b> (apenas visual)</p>
<p>Texto <strong>importante</strong> (semântico)</p>
<p>Texto <i>itálico</i> (apenas visual)</p>
<p>Texto <em>enfatizado</em> (semântico)</p>
<p>Texto <u>sublinhado</u></p>
<p>Texto <s>tachado</s></p>
<p>Texto <del>removido</del> e <ins>inserido</ins></p>
<p>Texto <mark>marcado</mark> (destaque)</p>
```

### Código e Teclado
```html
<p>Use <code>console.log()</code> para debug.</p>
<p>Pressione <kbd>Ctrl</kbd> + <kbd>C</kbd> para copiar.</p>
<pre><code>
  function hello() {
    return "Hello World";
  }
</code></pre>
```

### Subscrito e Sobrescrito
```html
<p>Fórmula da água: H<sub>2</sub>O</p>
<p>Equação: x<sup>2</sup> + y<sup>2</sup> = z<sup>2</sup></p>
```

### Tempo e Data
```html
<p>Evento em <time datetime="2024-12-25">Natal de 2024</time></p>
<p>Horário: <time datetime="20:00">20:00</time></p>
```

### Progresso e Medidores
```html
<p>Progresso: <progress value="75" max="100">75%</progress></p>
<p>Uso de disco: <meter value="0.6" min="0" max="1">60%</meter></p>
```

## Listas

### Listas Não Ordenadas
```html
<ul>
  <li>Item 1</li>
  <li>Item 2
    <ul>
      <li>Subitem 2.1</li>
      <li>Subitem 2.2</li>
    </ul>
  </li>
  <li>Item 3</li>
</ul>
```

### Listas Ordenadas
```html
<ol>
  <li>Primeiro item</li>
  <li>Segundo item</li>
  <li>Terceiro item</li>
</ol>

<ol type="A">
  <li>Item A</li>
  <li>Item B</li>
</ol>

<ol start="10">
  <li>Item 10</li>
  <li>Item 11</li>
</ol>

<ol reversed>
  <li>Item 3</li>
  <li>Item 2</li>
  <li>Item 1</li>
</ol>
```

### Listas de Definição
```html
<dl>
  <dt>HTML</dt>
  <dd>Linguagem de marcação para páginas web</dd>
  
  <dt>CSS</dt>
  <dd>Linguagem para estilização de páginas web</dd>
  
  <dt>JavaScript</dt>
  <dd>Linguagem de programação para interatividade</dd>
</dl>
```

### Listas Aninhadas
```html
<ul>
  <li>Frutas
    <ul>
      <li>Maçã</li>
      <li>Banana</li>
      <li>Laranja</li>
    </ul>
  </li>
  <li>Vegetais
    <ol>
      <li>Cenoura</li>
      <li>Batata</li>
    </ol>
  </li>
</ul>
```

## Links e Âncoras

### Link Básico
```html
<a href="https://exemplo.com">Visite Exemplo.com</a>
<a href="pagina.html">Página Local</a>
<a href="../pai/pagina.html">Página Relativa</a>
```

### Link com Atributos
```html
<a href="https://exemplo.com"
   target="_blank"
   rel="noopener noreferrer"
   title="Visite o site exemplo">
  Abrir em nova janela
</a>
```

### Âncora na Mesma Página
```html
<!-- Menu de navegação -->
<nav>
  <a href="#introducao">Introdução</a>
  <a href="#conteudo">Conteúdo</a>
  <a href="#contato">Contato</a>
</nav>

<!-- Seções da página -->
<section id="introducao">
  <h2>Introdução</h2>
  <p>Conteúdo da introdução...</p>
</section>

<section id="conteudo">
  <h2>Conteúdo</h2>
  <p>Conteúdo principal...</p>
</section>
```

### Links Especiais
```html
<!-- Email -->
<a href="mailto:email@exemplo.com">Enviar Email</a>
<a href="mailto:email@exemplo.com?subject=Assunto&body=Corpo">Email com assunto</a>

<!-- Telefone -->
<a href="tel:+5511999999999">Ligar para nós</a>

<!-- Download -->
<a href="arquivo.pdf" download>Baixar PDF</a>
<a href="arquivo.pdf" download="nome-personalizado.pdf">Baixar com nome personalizado</a>

<!-- Protocolos -->
<a href="javascript:void(0)">Link JavaScript</a>
<a href="ftp://ftp.exemplo.com">Link FTP</a>
```

### Rel e Target
```html
<!-- Valores comuns de rel -->
<a href="https://externo.com" rel="nofollow">Não seguir</a>
<a href="https://externo.com" rel="noopener">Segurança</a>
<a href="pagina2.html" rel="next">Próxima página</a>
<a href="pagina1.html" rel="prev">Página anterior</a>

<!-- Valores de target -->
<a href="pagina.html" target="_self">Mesma aba (padrão)</a>
<a href="pagina.html" target="_blank">Nova aba/janela</a>
<a href="pagina.html" target="_parent">Frame pai</a>
<a href="pagina.html" target="_top">Janela completa</a>
```

## Imagens e Mídia

### Imagens Básicas
```html
<img src="imagem.jpg" alt="Descrição da imagem">
<img src="imagem.jpg" alt="Descrição" width="300" height="200">
```

### Atributos de Imagem
```html
<img src="foto.jpg"
     alt="Foto de paisagem"
     width="800"
     height="600"
     loading="lazy"
     decoding="async"
     srcset="foto-320w.jpg 320w,
             foto-480w.jpg 480w,
             foto-800w.jpg 800w"
     sizes="(max-width: 600px) 480px,
            800px">
```

### Picture Element
```html
<picture>
  <source media="(min-width: 1200px)" srcset="grande.jpg">
  <source media="(min-width: 768px)" srcset="media.jpg">
  <img src="pequena.jpg" alt="Imagem responsiva">
</picture>

<!-- Formatos diferentes -->
<picture>
  <source type="image/webp" srcset="imagem.webp">
  <source type="image/jpeg" srcset="imagem.jpg">
  <img src="imagem.jpg" alt="Imagem em formato webp ou jpeg">
</picture>
```

### Figuras com Legenda
```html
<figure>
  <img src="diagrama.jpg" alt="Diagrama do sistema">
  <figcaption>Figura 1: Diagrama arquitetural do sistema</figcaption>
</figure>

<figure>
  <pre><code>
    function exemplo() {
      return "código";
    }
  </code></pre>
  <figcaption>Código exemplo em JavaScript</figcaption>
</figure>
```

### Áudio
```html
<audio controls>
  <source src="audio.mp3" type="audio/mpeg">
  <source src="audio.ogg" type="audio/ogg">
  Seu navegador não suporta áudio HTML5.
</audio>

<!-- Atributos -->
<audio controls 
       autoplay 
       loop 
       muted 
       preload="metadata">
  <source src="musica.mp3" type="audio/mpeg">
</audio>
```

### Vídeo
```html
<video controls width="640" height="360">
  <source src="video.mp4" type="video/mp4">
  <source src="video.webm" type="video/webm">
  Seu navegador não suporta vídeo HTML5.
</video>

<!-- Atributos -->
<video controls
       autoplay
       loop
       muted
       poster="thumbnail.jpg"
       preload="metadata">
  <source src="video.mp4" type="video/mp4">
</video>

<!-- Legendas -->
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track src="legendas.vtt" kind="subtitles" srclang="pt" label="Português">
  <track src="captions.vtt" kind="captions" srclang="pt" label="Legendas">
</video>
```

### SVG e Canvas
```html
<!-- SVG inline -->
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" fill="blue" />
</svg>

<!-- Canvas -->
<canvas id="meuCanvas" width="200" height="100"></canvas>
<script>
  const canvas = document.getElementById('meuCanvas');
  const ctx = canvas.getContext('2d');
  ctx.fillStyle = 'green';
  ctx.fillRect(10, 10, 150, 80);
</script>
```

## Tabelas

### Estrutura Básica
```html
<table>
  <tr>
    <th>Nome</th>
    <th>Idade</th>
    <th>Cidade</th>
  </tr>
  <tr>
    <td>João</td>
    <td>30</td>
    <td>São Paulo</td>
  </tr>
  <tr>
    <td>Maria</td>
    <td>25</td>
    <td>Rio de Janeiro</td>
  </tr>
</table>
```

### Tabela Semântica Completa
```html
<table>
  <caption>Lista de Funcionários</caption>
  <thead>
    <tr>
      <th scope="col">Nome</th>
      <th scope="col">Cargo</th>
      <th scope="col">Salário</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">João Silva</th>
      <td>Desenvolvedor</td>
      <td>R$ 5.000</td>
    </tr>
    <tr>
      <th scope="row">Maria Santos</th>
      <td>Designer</td>
      <td>R$ 4.500</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="2">Total</td>
      <td>R$ 9.500</td>
    </tr>
  </tfoot>
</table>
```

### Mesclagem de Células
```html
<table>
  <tr>
    <th colspan="2">Nome Completo</th>
    <th>Idade</th>
  </tr>
  <tr>
    <td>João</td>
    <td>Silva</td>
    <td rowspan="2">30</td>
  </tr>
  <tr>
    <td>Maria</td>
    <td>Santos</td>
  </tr>
</table>
```

### Atributos de Tabela
```html
<table border="1"
       cellspacing="0"
       cellpadding="10"
       width="100%"
       summary="Tabela de exemplo">
  <!-- conteúdo -->
</table>
```

### Tabelas Responsivas
```html
<div class="table-responsive">
  <table>
    <!-- Conteúdo da tabela -->
  </table>
</div>

<!-- CSS correspondente -->
<style>
.table-responsive {
  overflow-x: auto;
  max-width: 100%;
}
</style>
```

## Formulários

### Formulário Básico
```html
<form action="/processar" method="POST">
  <label for="nome">Nome:</label>
  <input type="text" id="nome" name="nome" required>
  
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required>
  
  <button type="submit">Enviar</button>
</form>
```

### Tipos de Input
```html
<!-- Texto -->
<input type="text" name="texto" placeholder="Digite algo">
<input type="password" name="senha" placeholder="Senha">

<!-- Email e URL -->
<input type="email" name="email" placeholder="email@exemplo.com">
<input type="url" name="website" placeholder="https://exemplo.com">

<!-- Números -->
<input type="number" name="idade" min="0" max="120" step="1">
<input type="range" name="volume" min="0" max="100">

<!-- Data e Hora -->
<input type="date" name="data">
<input type="time" name="hora">
<input type="datetime-local" name="datahora">
<input type="month" name="mes">
<input type="week" name="semana">

<!-- Cores e Arquivos -->
<input type="color" name="cor">
<input type="file" name="arquivo" accept=".pdf,.jpg">

<!-- Botões -->
<input type="submit" value="Enviar">
<input type="reset" value="Limpar">
<input type="button" value="Clique-me">
<input type="image" src="botao.jpg" alt="Enviar">

<!-- Especiais -->
<input type="search" name="busca" placeholder="Buscar...">
<input type="tel" name="telefone" pattern="[0-9]{11}">
<input type="hidden" name="token" value="abc123">
```

### Textarea e Select
```html
<!-- Textarea -->
<label for="mensagem">Mensagem:</label>
<textarea id="mensagem" 
          name="mensagem" 
          rows="4" 
          cols="50"
          placeholder="Digite sua mensagem..."></textarea>

<!-- Select -->
<label for="cidade">Cidade:</label>
<select id="cidade" name="cidade">
  <option value="">Selecione...</option>
  <option value="sp">São Paulo</option>
  <option value="rj">Rio de Janeiro</option>
  <option value="bh">Belo Horizonte</option>
</select>

<!-- Select múltiplo -->
<label for="interesses">Interesses:</label>
<select id="interesses" name="interesses[]" multiple size="4">
  <option value="tecnologia">Tecnologia</option>
  <option value="esportes">Esportes</option>
  <option value="musica">Música</option>
</select>

<!-- Datalist -->
<label for="browser">Navegador:</label>
<input list="browsers" id="browser" name="browser">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Safari">
</datalist>
```

### Checkboxes e Radios
```html
<!-- Checkboxes -->
<fieldset>
  <legend>Interesses</legend>
  
  <label>
    <input type="checkbox" name="interesses[]" value="tecnologia">
    Tecnologia
  </label>
  
  <label>
    <input type="checkbox" name="interesses[]" value="esportes">
    Esportes
  </label>
</fieldset>

<!-- Radio buttons -->
<fieldset>
  <legend>Sexo</legend>
  
  <label>
    <input type="radio" name="sexo" value="masculino" required>
    Masculino
  </label>
  
  <label>
    <input type="radio" name="sexo" value="feminino">
    Feminino
  </label>
</fieldset>
```

### Atributos de Formulário
```html
<form action="/submit" 
      method="POST"
      enctype="multipart/form-data"
      autocomplete="on"
      novalidate
      target="_blank">
  
  <input type="text" 
         name="usuario" 
         required
         placeholder="Digite seu nome"
         pattern="[A-Za-z]{3,}"
         minlength="3"
         maxlength="50"
         autocomplete="username"
         autofocus
         disabled
         readonly
         value="Valor padrão">
         
  <input type="email" 
         name="email"
         required
         multiple> <!-- Para múltiplos emails -->
</form>
```

### Validação HTML5
```html
<form>
  <!-- Obrigatório -->
  <input type="text" required>
  
  <!-- Padrão regex -->
  <input type="text" pattern="[A-Za-z]{3,}">
  
  <!-- Tamanho -->
  <input type="text" minlength="3" maxlength="50">
  
  <!-- Números -->
  <input type="number" min="0" max="100" step="5">
  
  <!-- Data -->
  <input type="date" min="2024-01-01" max="2024-12-31">
  
  <!-- Mensagem de validação -->
  <input type="text" 
         pattern="[A-Za-z]{3,}"
         title="Mínimo 3 letras (sem números ou caracteres especiais)">
</form>
```

### Output e Progress
```html
<form oninput="resultado.value = parseInt(a.value) + parseInt(b.value)">
  <input type="range" id="a" value="50"> +
  <input type="number" id="b" value="25"> =
  <output name="resultado" for="a b">75</output>
</form>

<!-- Progress bar -->
<progress id="progresso" value="75" max="100">75%</progress>
```

## Elementos Semânticos

### Cabeçalho e Rodapé
```html
<!-- Header principal -->
<header>
  <h1>Meu Site</h1>
  <nav>
    <!-- Menu de navegação -->
  </nav>
</header>

<!-- Rodapé principal -->
<footer>
  <p>&copy; 2024 Meu Site</p>
  <address>
    Contato: <a href="mailto:contato@exemplo.com">email@exemplo.com</a>
  </address>
</footer>
```

### Navegação
```html
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/sobre">Sobre</a></li>
    <li><a href="/contato">Contato</a></li>
  </ul>
</nav>

<!-- Múltiplas navegações -->
<nav aria-label="Navegação principal">
  <!-- Links principais -->
</nav>

<nav aria-label="Navegação secundária">
  <!-- Links secundários -->
</nav>
```

### Artigos e Seções
```html
<article>
  <header>
    <h2>Título do Artigo</h2>
    <p>Publicado em <time datetime="2024-01-15">15 de Janeiro de 2024</time></p>
  </header>
  
  <section>
    <h3>Introdução</h3>
    <p>Conteúdo da introdução...</p>
  </section>
  
  <section>
    <h3>Desenvolvimento</h3>
    <p>Conteúdo principal...</p>
  </section>
  
  <footer>
    <p>Autor: João Silva</p>
  </footer>
</article>
```

### Aside e Details
```html
<aside>
  <h3>Informações Adicionais</h3>
  <p>Conteúdo relacionado mas não essencial...</p>
</aside>

<!-- Details com summary -->
<details>
  <summary>Mais informações</summary>
  <p>Conteúdo detalhado que aparece quando expandido.</p>
</details>

<!-- Aberto por padrão -->
<details open>
  <summary>Informações Expandidas</summary>
  <p>Conteúdo visível por padrão.</p>
</details>
```

### Main e Div
```html
<!-- Conteúdo principal único -->
<main>
  <h1>Título Principal</h1>
  <p>Conteúdo principal da página...</p>
</main>

<!-- Div genérica -->
<div class="container">
  <p>Elemento de bloco sem significado semântico.</p>
</div>

<!-- Span inline -->
<p>Texto com <span class="destaque">ênfase visual</span>.</p>
```

## Elementos de Seção

### Estrutura de Página Completa
```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Site Completo</title>
</head>
<body>
    <!-- Cabeçalho -->
    <header>
        <h1>Logo do Site</h1>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#sobre">Sobre</a></li>
                <li><a href="#contato">Contato</a></li>
            </ul>
        </nav>
    </header>

    <!-- Conteúdo principal -->
    <main>
        <article>
            <header>
                <h2>Título do Artigo</h2>
            </header>
            
            <section>
                <h3>Seção 1</h3>
                <p>Conteúdo...</p>
            </section>
            
            <aside>
                <h4>Leia também</h4>
                <ul>
                    <li><a href="#">Artigo relacionado</a></li>
                </ul>
            </aside>
        </article>
    </main>

    <!-- Barra lateral -->
    <aside>
        <h3>Barra Lateral</h3>
        <p>Conteúdo relacionado...</p>
    </aside>

    <!-- Rodapé -->
    <footer>
        <p>&copy; 2024 Meu Site</p>
        <address>
            Contato: <a href="mailto:email@exemplo.com">email@exemplo.com</a>
        </address>
    </footer>
</body>
</html>
```

### Seções Aninhadas
```html
<section>
  <h2>Capítulo 1</h2>
  <p>Introdução do capítulo...</p>
  
  <section>
    <h3>1.1 Subtítulo</h3>
    <p>Conteúdo da seção 1.1...</p>
  </section>
  
  <section>
    <h3>1.2 Outro Subtítulo</h3>
    <p>Conteúdo da seção 1.2...</p>
  </section>
</section>
```

### Header e Footer em Múltiplos Contextos
```html
<article>
  <header>
    <h2>Título do Artigo</h2>
    <p>Por: Autor</p>
  </header>
  
  <p>Conteúdo do artigo...</p>
  
  <section>
    <header>
      <h3>Comentários</h3>
    </header>
    
    <article>
      <header>
        <h4>João disse:</h4>
        <p><time datetime="2024-01-15">15 de Janeiro</time></p>
      </header>
      <p>Ótimo artigo!</p>
      <footer>
        <button>Responder</button>
      </footer>
    </article>
  </section>
  
  <footer>
    <p>Tags: <a href="#">HTML</a>, <a href="#">Web</a></p>
  </footer>
</article>
```

## Metadados

### Metadados Básicos
```html
<head>
    <!-- Codificação de caracteres -->
    <meta charset="UTF-8">
    
    <!-- Viewport para responsividade -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Título da página -->
    <title>Título da Página - Site</title>
    
    <!-- Descrição para SEO -->
    <meta name="description" content="Descrição da página para SEO">
    
    <!-- Palavras-chave -->
    <meta name="keywords" content="HTML, CSS, JavaScript, Web">
    
    <!-- Autor -->
    <meta name="author" content="Seu Nome">
</head>
```

### Metadados Avançados
```html
<!-- Idioma alternativo -->
<link rel="alternate" hreflang="en" href="https://exemplo.com/en/">
<link rel="alternate" hreflang="es" href="https://exemplo.com/es/">

<!-- Canais RSS/Atom -->
<link rel="alternate" type="application/rss+xml" href="/rss.xml">
<link rel="alternate" type="application/atom+xml" href="/atom.xml">

<!-- Ícones -->
<link rel="icon" href="/favicon.ico" type="image/x-icon">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">

<!-- Open Graph (Facebook) -->
<meta property="og:title" content="Título da Página">
<meta property="og:description" content="Descrição para redes sociais">
<meta property="og:image" content="https://exemplo.com/imagem.jpg">
<meta property="og:url" content="https://exemplo.com/pagina">
<meta property="og:type" content="website">

<!-- Twitter Cards -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Título para Twitter">
<meta name="twitter:description" content="Descrição para Twitter">
<meta name="twitter:image" content="https://exemplo.com/imagem-twitter.jpg">

<!-- Outros metadados importantes -->
<meta name="robots" content="index, follow">
<meta name="theme-color" content="#4285f4">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

### Links de Recursos
```html
<!-- CSS -->
<link rel="stylesheet" href="estilos.css" media="screen">
<link rel="stylesheet" href="impressao.css" media="print">

<!-- Pré-carregamento -->
<link rel="preload" href="fonte.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="script.js" as="script">

<!-- Pré-conexão -->
<link rel="preconnect" href="https://api.exemplo.com">
<link rel="dns-prefetch" href="https://api.exemplo.com">

<!-- Manifest PWA -->
<link rel="manifest" href="/manifest.json">

<!-- Canonical URL -->
<link rel="canonical" href="https://exemplo.com/pagina-original">

<!-- Próxima/Anterior para paginação -->
<link rel="next" href="pagina2.html">
<link rel="prev" href="pagina1.html">
```

### Meta Tags para SEO
```html
<!-- Controle de indexação -->
<meta name="robots" content="index, follow">
<meta name="robots" content="noindex, nofollow">
<meta name="googlebot" content="index, follow">

<!-- Evitar snippets -->
<meta name="google" content="nositelinkssearchbox">

<!-- URL alternativas -->
<link rel="alternate" media="only screen and (max-width: 640px)" 
      href="https://m.exemplo.com/pagina">

<!-- Verificação de propriedade -->
<meta name="google-site-verification" content="chave-verificacao">
<meta name="facebook-domain-verification" content="chave-facebook">
```

## Atributos Globais

### Atributos Básicos
```html
<!-- class - múltiplas classes -->
<div class="container primary active"></div>

<!-- id - identificador único -->
<section id="introducao"></section>

<!-- style - CSS inline -->
<p style="color: blue; font-size: 16px;"></p>

<!-- title - informação adicional -->
<abbr title="HyperText Markup Language">HTML</abbr>

<!-- lang - idioma -->
<p lang="en">This is in English</p>
<p lang="pt">Isto está em Português</p>

<!-- dir - direção do texto -->
<p dir="ltr">Left to right</p>
<p dir="rtl">Right to left</p>
```

### Atributos de Acessibilidade
```html
<!-- ARIA roles -->
<div role="navigation" aria-label="Menu principal"></div>
<button role="switch" aria-checked="true"></button>

<!-- ARIA labels e descrições -->
<nav aria-label="Menu principal">
  <!-- menu -->
</nav>

<button aria-label="Fechar menu">
  <span aria-hidden="true">×</span>
</button>

<!-- ARIA estados e propriedades -->
<button aria-expanded="false" aria-controls="menu">
  Menu
</button>
<div id="menu" aria-hidden="true">
  <!-- conteúdo do menu -->
</div>

<!-- Tabindex -->
<button tabindex="0">Elemento focável</button>
<div tabindex="-1">Focável programaticamente</div>
<button tabindex="1">Primeiro na ordem</button>
```

### Atributos de Dados
```html
<!-- data-* attributes -->
<div data-id="123"
     data-usuario='{"nome":"João","idade":30}'
     data-categoria="tecnologia"
     data-toggle="modal">
</div>

<!-- Acesso via JavaScript -->
<script>
  const elemento = document.querySelector('div');
  console.log(elemento.dataset.id); // "123"
  console.log(elemento.dataset.usuario); // '{"nome":"João","idade":30}'
  console.log(elemento.dataset.toggle); // "modal"
</script>
```

### Atributos de Eventos
```html
<!-- Eventos de mouse -->
<button onclick="alert('Clicado!')">Clique</button>
<div onmouseover="this.style.color='red'"
     onmouseout="this.style.color='black'">
  Passe o mouse
</div>

<!-- Eventos de teclado -->
<input onkeydown="console.log('Tecla pressionada')"
       onkeyup="console.log('Tecla liberada')">

<!-- Eventos de formulário -->
<form onsubmit="return validar()">
  <input onfocus="this.style.background='yellow'"
         onblur="this.style.background='white'">
</form>

<!-- Eventos de mídia -->
<video onplay="console.log('Reproduzindo')"
       onpause="console.log('Pausado')">
</video>
```

### Atributos Booleanos
```html
<!-- Atributos booleanos (presença = true) -->
<input type="checkbox" checked>
<select>
  <option selected>Opção 1</option>
</select>
<button disabled>Desabilitado</button>
<video controls autoplay loop></video>
<form novalidate></form>
<details open></details>
```

### Atributos Especiais
```html
<!-- contenteditable -->
<div contenteditable="true">Edite este texto</div>

<!-- draggable -->
<div draggable="true">Arraste-me</div>

<!-- hidden -->
<p hidden>Este texto está oculto</p>

<!-- spellcheck -->
<textarea spellcheck="true">Texto com verificação ortográfica</textarea>

<!-- translate -->
<p translate="no">Não traduza este texto</p>
```

## HTML5 APIs

### Geolocalização
```html
<button onclick="obterLocalizacao()">Minha Localização</button>
<p id="localizacao"></p>

<script>
function obterLocalizacao() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      function(position) {
        const localizacao = document.getElementById('localizacao');
        localizacao.innerHTML = 
          `Latitude: ${position.coords.latitude}<br>
           Longitude: ${position.coords.longitude}`;
      },
      function(error) {
        console.error("Erro na geolocalização:", error);
      }
    );
  } else {
    alert("Geolocalização não suportada pelo navegador");
  }
}
</script>
```

### Armazenamento Local
```html
<input type="text" id="nome" placeholder="Digite seu nome">
<button onclick="salvarNome()">Salvar</button>
<button onclick="carregarNome()">Carregar</button>
<button onclick="limparNome()">Limpar</button>

<script>
function salvarNome() {
  const nome = document.getElementById('nome').value;
  localStorage.setItem('nomeUsuario', nome);
  alert('Nome salvo!');
}

function carregarNome() {
  const nome = localStorage.getItem('nomeUsuario');
  if (nome) {
    document.getElementById('nome').value = nome;
  }
}

function limparNome() {
  localStorage.removeItem('nomeUsuario');
  document.getElementById('nome').value = '';
}
</script>
```

### Web Workers
```html
<button onclick="iniciarWorker()">Iniciar Processamento</button>
<button onclick="pararWorker()">Parar Worker</button>
<p id="resultado"></p>

<script>
let worker;

function iniciarWorker() {
  if (typeof(Worker) !== "undefined") {
    if (!worker) {
      worker = new Worker("worker.js");
    }
    
    worker.onmessage = function(event) {
      document.getElementById("resultado").innerHTML = event.data;
    };
    
    worker.postMessage("Iniciar");
  }
}

function pararWorker() {
  if (worker) {
    worker.terminate();
    worker = undefined;
  }
}
</script>
```

### Drag and Drop
```html
<div id="div1" 
     ondrop="drop(event)" 
     ondragover="allowDrop(event)">
  <p id="drag1" draggable="true" ondragstart="drag(event)">
    Arraste-me para a caixa
  </p>
</div>

<div id="div2" 
     ondrop="drop(event)" 
     ondragover="allowDrop(event)">
</div>

<script>
function allowDrop(ev) {
  ev.preventDefault();
}

function drag(ev) {
  ev.dataTransfer.setData("text", ev.target.id);
}

function drop(ev) {
  ev.preventDefault();
  const data = ev.dataTransfer.getData("text");
  ev.target.appendChild(document.getElementById(data));
}
</script>
```

### API de Vídeo
```html
<video id="meuVideo" width="320" height="240" controls>
  <source src="video.mp4" type="video/mp4">
</video>

<div>
  <button onclick="playPause()">Play/Pause</button>
  <button onclick="telaCheia()">Tela Cheia</button>
  <input type="range" id="volume" min="0" max="1" step="0.1" 
         onchange="mudarVolume()">
</div>

<script>
const video = document.getElementById("meuVideo");

function playPause() {
  if (video.paused) {
    video.play();
  } else {
    video.pause();
  }
}

function telaCheia() {
  if (video.requestFullscreen) {
    video.requestFullscreen();
  } else if (video.webkitRequestFullscreen) {
    video.webkitRequestFullscreen();
  }
}

function mudarVolume() {
  const volume = document.getElementById("volume").value;
  video.volume = volume;
}
</script>
```

### Canvas API
```html
<canvas id="meuCanvas" width="400" height="200"></canvas>

<script>
const canvas = document.getElementById('meuCanvas');
const ctx = canvas.getContext('2d');

// Retângulo
ctx.fillStyle = 'blue';
ctx.fillRect(10, 10, 150, 80);

// Círculo
ctx.beginPath();
ctx.arc(300, 60, 40, 0, 2 * Math.PI);
ctx.fillStyle = 'red';
ctx.fill();

// Texto
ctx.font = '20px Arial';
ctx.fillStyle = 'green';
ctx.fillText('Canvas HTML5', 150, 150);

// Linha
ctx.beginPath();
ctx.moveTo(0, 0);
ctx.lineTo(400, 200);
ctx.strokeStyle = 'black';
ctx.lineWidth = 3;
ctx.stroke();
</script>
```

## Acessibilidade

### ARIA Roles
```html
<!-- Landmark roles -->
<header role="banner"></header>
<nav role="navigation"></nav>
<main role="main"></main>
<aside role="complementary"></aside>
<footer role="contentinfo"></footer>

<!-- Widget roles -->
<button role="button">Botão</button>
<a role="link" href="#">Link</a>
<input type="checkbox" role="checkbox">
<select role="listbox"></select>

<!-- Live region roles -->
<div role="alert">Mensagem importante</div>
<div role="status" aria-live="polite">Status atualizado</div>

<!-- Document structure roles -->
<div role="article">Artigo</div>
<div role="heading" aria-level="2">Título</div>
<div role="list">
  <div role="listitem">Item 1</div>
</div>
```

### ARIA States and Properties
```html
<!-- Estados -->
<button aria-pressed="false">Toggle</button>
<button aria-expanded="false" aria-controls="menu">Menu</button>
<div aria-hidden="true" id="menu">Conteúdo do menu</div>
<input aria-invalid="true" aria-describedby="erro">
<div id="erro" role="alert">Campo inválido</div>

<!-- Propriedades -->
<input aria-label="Buscar" placeholder="Buscar...">
<div aria-labelledby="titulo">
  <h2 id="titulo">Seção</h2>
</div>
<input aria-describedby="ajuda">
<div id="ajuda">Texto de ajuda</div>
<input aria-required="true">
<div aria-live="polite" aria-atomic="true">
  <!-- Conteúdo atualizado -->
</div>
```

### Tabelas Acessíveis
```html
<table>
  <caption>Lista de Produtos</caption>
  <thead>
    <tr>
      <th scope="col">Produto</th>
      <th scope="col">Preço</th>
      <th scope="col">Estoque</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Notebook</th>
      <td>R$ 2.500</td>
      <td>15 unidades</td>
    </tr>
    <tr>
      <th scope="row">Smartphone</th>
      <td>R$ 1.200</td>
      <td>30 unidades</td>
    </tr>
  </tbody>
</table>
```

### Formulários Acessíveis
```html
<form>
  <fieldset>
    <legend>Informações Pessoais</legend>
    
    <div>
      <label for="nome">Nome completo:</label>
      <input type="text" id="nome" name="nome" required
             aria-describedby="ajuda-nome">
      <div id="ajuda-nome" class="ajuda">
        Digite seu nome completo como no documento.
      </div>
    </div>
    
    <div>
      <label for="email">Email:</label>
      <input type="email" id="email" name="email" required
             aria-invalid="false"
             aria-describedby="erro-email">
      <div id="erro-email" role="alert" aria-live="polite"></div>
    </div>
  </fieldset>
  
  <button type="submit">Enviar</button>
</form>

<script>
document.getElementById('email').addEventListener('blur', function() {
  const email = this.value;
  const erro = document.getElementById('erro-email');
  
  if (!email.includes('@')) {
    this.setAttribute('aria-invalid', 'true');
    erro.textContent = 'Email inválido. Inclua @.';
  } else {
    this.setAttribute('aria-invalid', 'false');
    erro.textContent = '';
  }
});
</script>
```

### Navegação por Teclado
```html
<!-- Ordem de tabulação lógica -->
<nav>
  <a href="#conteudo" class="skip-link">Pular para conteúdo</a>
  
  <ul>
    <li><a href="/" tabindex="1">Home</a></li>
    <li><a href="/sobre" tabindex="2">Sobre</a></li>
    <li><a href="/contato" tabindex="3">Contato</a></li>
  </ul>
</nav>

<main id="conteudo" tabindex="-1">
  <!-- Conteúdo principal -->
</main>

<style>
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: white;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
</style>
```

### Imagens Acessíveis
```html
<!-- Imagem decorativa -->
<img src="decoracao.jpg" alt="" aria-hidden="true">

<!-- Imagem informativa -->
<img src="grafico.jpg" 
     alt="Gráfico de vendas 2024: Janeiro R$ 50.000, Fevereiro R$ 75.000"
     longdesc="#descricao-grafico">

<div id="descricao-grafico" class="visually-hidden">
  Gráfico de barras mostrando as vendas mensais de 2024.
  Janeiro: R$ 50.000, Fevereiro: R$ 75.000, Março: R$ 60.000.
</div>

<!-- Mapa de imagem -->
<img src="mapa.jpg" alt="Mapa do Brasil" usemap="#brasil">
<map name="brasil">
  <area shape="rect" coords="0,0,100,100" 
        href="sp.html" alt="Estado de São Paulo">
  <area shape="circle" coords="200,200,50" 
        href="rj.html" alt="Estado do Rio de Janeiro">
</map>

<style>
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
</style>
```

## SEO

### Estrutura Semântica para SEO
```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Melhor Prática de SEO - Meu Site</title>
    <meta name="description" content="Descrição clara e atraente da página com palavras-chave relevantes.">
    <meta name="keywords" content="palavra-chave1, palavra-chave2, palavra-chave3">
    
    <!-- Open Graph -->
    <meta property="og:title" content="Título para Redes Sociais">
    <meta property="og:description" content="Descrição para compartilhamento">
    <meta property="og:image" content="https://exemplo.com/imagem-destaque.jpg">
    <meta property="og:url" content="https://exemplo.com/pagina">
    
    <!-- Twitter Card -->
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="Título para Twitter">
    
    <!-- Canonical -->
    <link rel="canonical" href="https://exemplo.com/pagina-original">
    
    <!-- Schema Markup -->
    <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "WebPage",
      "name": "Título da Página",
      "description": "Descrição da página",
      "url": "https://exemplo.com/pagina"
    }
    </script>
</head>
<body>
    <header>
        <h1>Título Principal da Página (H1)</h1>
        <p>Descrição breve do conteúdo</p>
    </header>
    
    <main>
        <article>
            <h2>Subtítulo Importante (H2)</h2>
            <p>Conteúdo relevante com palavras-chave naturais.</p>
            
            <section>
                <h3>Subseção (H3)</h3>
                <p>Mais conteúdo detalhado.</p>
            </section>
        </article>
    </main>
</body>
</html>
```

### URLs Amigáveis
```html
<!-- URLs estrutura -->
<!-- RUIM -->
<a href="produto.php?id=123&cat=5">Produto</a>

<!-- BOM -->
<a href="/produtos/eletronicos/iphone-15">iPhone 15</a>
```

### Meta Tags Específicas
```html
<!-- Controle de indexação -->
<meta name="robots" content="index, follow, max-snippet:50, max-image-preview:large">
<meta name="googlebot" content="index, follow">

<!-- Idioma alternativo -->
<link rel="alternate" hreflang="en" href="https://exemplo.com/en/">
<link rel="alternate" hreflang="pt-br" href="https://exemplo.com/">

<!-- Sitemap -->
<link rel="sitemap" type="application/xml" href="/sitemap.xml">

<!-- Breadcrumbs schema -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [{
    "@type": "ListItem",
    "position": 1,
    "name": "Home",
    "item": "https://exemplo.com/"
  },{
    "@type": "ListItem",
    "position": 2,
    "name": "Produtos",
    "item": "https://exemplo.com/produtos/"
  },{
    "@type": "ListItem",
    "position": 3,
    "name": "Eletrônicos",
    "item": "https://exemplo.com/produtos/eletronicos/"
  }]
}
</script>
```

### Performance e SEO
```html
<!-- Lazy loading images -->
<img src="imagem.jpg" alt="Descrição" loading="lazy">

<!-- Preconnect para recursos externos -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

<!-- Preload fontes críticas -->
<link rel="preload" href="fonte.woff2" as="font" type="font/woff2" crossorigin>

<!-- Defer scripts -->
<script src="script.js" defer></script>

<!-- CSS crítico inline -->
<style>
/* CSS crítico para primeira renderização */
</style>

<!-- CSS não crítico carregado normalmente -->
<link rel="stylesheet" href="estilos.css" media="print" onload="this.media='all'">
```

## Boas Práticas

### Validação HTML
```html
<!-- Documento válido -->
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Título</title>
</head>
<body>
    <!-- Elementos semânticos corretos -->
    <main>
        <article>
            <h1>Título</h1>
            <p>Parágrafo válido</p>
            
            <!-- Listas estruturadas corretamente -->
            <ul>
                <li>Item 1</li>
                <li>Item 2</li>
            </ul>
            
            <!-- Formulários corretos -->
            <form>
                <label for="nome">Nome:</label>
                <input type="text" id="nome" name="nome">
                <button type="submit">Enviar</button>
            </form>
        </article>
    </main>
</body>
</html>
```

### Acessibilidade
```html
<!-- Use elementos semânticos -->
<nav aria-label="Menu principal">
  <ul>
    <li><a href="/">Home</a></li>
  </ul>
</nav>

<!-- Labels para todos os inputs -->
<label for="email">Email:</label>
<input type="email" id="email" name="email">

<!-- Alt para imagens significativas -->
<img src="grafico.jpg" alt="Gráfico de crescimento anual">

<!-- Texto alternativo para botões com ícones -->
<button aria-label="Fechar">
  <span aria-hidden="true">×</span>
</button>

<!-- Contraste adequado -->
<p style="color: #000; background: #fff;">
  Texto com bom contraste
</p>
```

### Performance
```html
<!-- Otimize imagens -->
<img src="imagem-otimizada.jpg" 
     width="800" 
     height="600" 
     alt="Descrição"
     loading="lazy">

<!-- Use SVG para ícones -->
<svg width="24" height="24">
  <use href="#icone-menu"></use>
</svg>

<!-- Defer scripts não críticos -->
<script src="analytics.js" defer></script>

<!-- CSS crítico inline -->
<style>
/* Estilos críticos para primeira renderização */
</style>

<!-- Fontes web otimizadas -->
<link rel="preload" 
      href="fonte.woff2" 
      as="font" 
      type="font/woff2" 
      crossorigin>

<!-- Evite redirects desnecessários -->
<!-- Use URLs diretas -->
```

### SEO
```html
<!-- Títulos únicos e descritivos -->
<title>Produto X - Loja Online - Melhor Preço</title>

<!-- Meta descrições atraentes -->
<meta name="description" content="Compre Produto X com frete grátis. 
10x sem juros. Garantia de 1 ano. Entrega para todo Brasil.">

<!-- URLs amigáveis -->
<a href="/produtos/categoria/produto-x">Produto X</a>

<!-- Heading hierarchy -->
<h1>Produto X</h1>
<h2>Características</h2>
<h3>Especificações Técnicas</h3>

<!-- Schema markup -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Produto X",
  "description": "Descrição do produto",
  "offers": {
    "@type": "Offer",
    "price": "199.90",
    "priceCurrency": "BRL"
  }
}
</script>
```

### Segurança
```html
<!-- CSP via meta tag -->
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self' https:">

<!-- Proteção X-Frame -->
<meta http-equiv="X-Frame-Options" content="DENY">

<!-- Proteção XSS -->
<meta http-equiv="X-XSS-Protection" content="1; mode=block">

<!-- No referrer -->
<meta name="referrer" content="no-referrer-when-downgrade">

<!-- Segurança em formulários -->
<form method="POST">
  <!-- CSRF token -->
  <input type="hidden" name="_csrf" value="token-aqui">
  
  <!-- Autocomplete seguro -->
  <input type="password" autocomplete="current-password">
</form>

<!-- Links seguros -->
<a href="https://exemplo.com" 
   rel="noopener noreferrer"
   target="_blank">Link Seguro</a>
```

### Código Limpo
```html
<!-- Indentação consistente -->
<!DOCTYPE html>
<html>
<head>
    <title>Título</title>
</head>
<body>
    <main>
        <article>
            <h1>Título</h1>
            <p>Parágrafo</p>
        </article>
    </main>
</body>
</html>

<!-- Use elementos semânticos -->
<!-- RUIM -->
<div class="header"></div>
<div class="content"></div>

<!-- BOM -->
<header></header>
<main></main>

<!-- Comentários úteis -->
<!-- Navegação principal -->
<nav>
    <!-- Logo -->
    <a href="/" class="logo">Site</a>
    
    <!-- Menu -->
    <ul class="menu">
        <li><a href="/sobre">Sobre</a></li>
    </ul>
</nav>

<!-- Atributos em ordem consistente -->
<img src="imagem.jpg" 
     alt="Descrição" 
     width="800" 
     height="600" 
     class="destaque" 
     loading="lazy">
```

## Recursos e Ferramentas

### Validadores
```html
<!-- W3C Validator -->
<!-- https://validator.w3.org/ -->

<!-- HTML5 Outliner -->
<!-- https://gsnedders.html5.org/outliner/ -->

<!-- Acessibilidade -->
<!-- https://wave.webaim.org/ -->
<!-- https://deque.com/axe/ -->
```

### Ferramentas de Desenvolvimento
```html
<!-- Editores recomendados -->
<!-- VS Code: https://code.visualstudio.com/ -->
<!-- Sublime Text: https://www.sublimetext.com/ -->

<!-- Extensões úteis -->
<!-- Live Server (VS Code) -->
<!-- Emmet -->
<!-- Prettier -->
<!-- HTMLHint -->

<!-- Navegadores para teste -->
<!-- Chrome DevTools -->
<!-- Firefox Developer Edition -->
<!-- Safari Web Inspector -->
```

### Recursos de Aprendizado
```html
<!-- Documentação oficial -->
<!-- MDN Web Docs: https://developer.mozilla.org/pt-BR/docs/Web/HTML -->
<!-- W3Schools: https://www.w3schools.com/html/ -->

<!-- Tutoriais e Cursos -->
<!-- freeCodeCamp: https://www.freecodecamp.org/ -->
<!-- Codecademy: https://www.codecademy.com/ -->
<!-- Udemy: https://www.udemy.com/ -->

<!-- Prática -->
<!-- Codepen: https://codepen.io/ -->
<!-- JSFiddle: https://jsfiddle.net/ -->
<!-- CodeSandbox: https://codesandbox.io/ -->
```

### Frameworks e Bibliotecas
```html
<!-- Frameworks CSS -->
<!-- Bootstrap: https://getbootstrap.com/ -->
<!-- Tailwind CSS: https://tailwindcss.com/ -->
<!-- Bulma: https://bulma.io/ -->

<!-- Meta frameworks -->
<!-- Next.js: https://nextjs.org/ -->
<!-- Nuxt.js: https://nuxtjs.org/ -->
<!-- Gatsby: https://www.gatsbyjs.com/ -->

<!-- Ferramentas de build -->
<!-- Vite: https://vitejs.dev/ -->
<!-- Webpack: https://webpack.js.org/ -->
<!-- Parcel: https://parceljs.org/ -->
```

### APIs Úteis
```html
<!-- APIs do navegador -->
<!-- Geolocation API -->
<!-- Web Storage API -->
<!-- Fetch API -->
<!-- Canvas API -->
<!-- Web Audio API -->

<!-- APIs externas populares -->
<!-- Google Maps API -->
<!-- Stripe API -->
<!-- Twilio API -->
<!-- SendGrid API -->
```

---

**Nota:** Este manual cobre HTML5 e práticas modernas de desenvolvimento web. Mantenha-se atualizado com as especificações do W3C e boas práticas da comunidade.
