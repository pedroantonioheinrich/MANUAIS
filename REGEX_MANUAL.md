# Manual de Regex: Aprendendo a Encontrar Padrões de Texto

## O Que é Regex?

Regex (ou Expressão Regular) é como uma **lupa mágica** para encontrar palavras e padrões em textos. Imagine que você tem um livro gigante e quer encontrar todas as vezes que aparece a palavra "gato" seguida de um número - o regex te ajuda a fazer isso!

## Por Que Usar Regex?

- **Procurar informações** em textos grandes
- **Validar dados** (como verificar se um email está bem escrito)
- **Organizar informações** separando partes importantes
- **Substituir trechos** de texto automaticamente

---

## Primeiros Passos: Letras e Números

### Procurando Palavras Simples

A maneira mais fácil é procurar por palavras exatas:

```
gato
```
Encontra: `gato`, mas **não** encontra: `Gato`, `gatos`, `gat o`

> As letras maiúsculas e minúsculas são diferentes para o regex!

### O Ponto: Um Coringa Mágico

O ponto `.` significa **qualquer letra, número ou espaço**:

```
g.to
```
Encontra: `gato`, `guto`, `g1to`, `g to`

### A Escapada: Quando Queremos o Ponto de Verdade

Se você quer encontrar um ponto real (.), use a barra invertida:

```
gato\.com
```
Encontra: `gato.com` mas não `gatocom`

---

## Quantificadores: Contando Quantas Vezes

### O Asterisco: Zero ou Mais Vezes

```
ga*to
```
Encontra: `gto` (zero 'a'), `gato` (um 'a'), `gaato` (dois 'a'), etc.

### O Mais: Uma ou Mais Vezes

```
ga+to
```
Encontra: `gato` (um 'a'), `gaato` (dois 'a'), mas **não** `gto`

### As Chaves: Número Exato de Vezes

```
ga{3}to
```
Encontra apenas: `gaaato` (exatamente três 'a')

```
ga{2,4}to
```
Encontra: `gaato` (2 'a'), `gaaato` (3 'a'), `gaaaato` (4 'a')

---

## Conjuntos: Escolhendo entre Opções

### Colchetes: Um Conjunto de Letras

```
g[au]to
```
Encontra: `gato` **ou** `guto`

```
[a-z]
```
Encontra qualquer letra minúscula de 'a' até 'z'

```
[0-9]
```
Encontra qualquer número de 0 a 9

### O Circunflexo Dentro de Colchetes: "Tudo Menos"

```
g[^au]to
```
Encontra: `geto`, `gito`, `goto`, mas **não** `gato` ou `guto`

---

## Atalhos Úteis

### Para Números

```
\d  = qualquer número (igual a [0-9])
\D  = qualquer coisa que NÃO seja número
```

### Para Letras, Números e Underlines

```
\w  = qualquer letra, número ou underline (igual a [a-zA-Z0-9_])
\W  = qualquer coisa que NÃO seja letra, número ou underline
```

### Para Espaços

```
\s  = qualquer espaço em branco (espaço, tab, nova linha)
\S  = qualquer coisa que NÃO seja espaço em branco
```

---

## Âncoras: Marcando o Começo e Fim

### Circunflexo Fora de Colchetes: Começo da Linha

```
^gato
```
Encontra `gato` apenas se estiver no **começo** da linha

### Cifrão: Fim da Linha

```
gato$
```
Encontra `gato` apenas se estiver no **final** da linha

---

## Grupos: Lembrando o que Encontramos

### Parênteses: Criando Grupos

```
(ga)+to
```
Encontra: `gato`, `gagato`, `gagagato`

Os parênteses também **lembram** o que foi encontrado para usar depois!

---

## Exemplos Práticos

### Procurando um Telefone

```
\(\d{2}\) \d{4,5}-\d{4}
```
Encontra: `(11) 91234-5678` ou `(21) 1234-5678`

### Procurando um Email

```
\w+@\w+\.\w+
```
Encontra: `nome@site.com` ou `pessoa123@email.org`

### Procurando uma Data

```
\d{2}/\d{2}/\d{4}
```
Encontra: `25/12/2023` ou `01/01/2024`

---

## Dicas para Praticar

1. **Comece simples** - teste primeiro com palavras normais
2. **Use ferramentas online** como Regex101 ou RegExr para testar
3. **Pratique com seus próprios textos**
4. **Não desanime** - regex é como aprender um novo idioma, leva tempo!

## Exercícios para Tentar

1. Encontre todas as palavras que começam com "ca": `^ca\w*`
2. Encontre números de 3 dígitos: `\b\d{3}\b`
3. Encontre palavras terminadas em "ção": `\w*ção\b`

---

Lembre-se: Regex é uma ferramenta poderosa! Com prática, você conseguirá encontrar padrões em textos como um detetive de palavras! 🔍

**Divirta-se explorando textos!**
