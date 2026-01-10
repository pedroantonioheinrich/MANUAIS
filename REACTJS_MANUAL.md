# Manual Completo de ReactJS

## O que é React?

Imagine que você está construindo uma casa com blocos de montar (como LEGO). Cada bloco tem uma função específica: uma janela, uma porta, um telhado. React é uma ferramenta que ajuda programadores a criar websites usando "blocos" chamados **componentes**.

React é uma **biblioteca JavaScript** criada pelo Facebook para construir interfaces de usuário (a parte que você vê e interage em um site ou aplicativo).

## Por que usar React?

1. **Organização**: Assim como blocos de montar organizados por tipo, React organiza seu código em partes reutilizáveis
2. **Atualização inteligente**: Quando algo muda no site, React atualiza apenas a parte necessária, não a página inteira
3. **Comunidade grande**: Muitas pessoas usam React, então há muitas soluções e ajuda disponível

## Pré-requisitos

Antes de começar com React, você precisa saber:
- HTML (estrutura das páginas)
- CSS (estilo das páginas)
- JavaScript (linguagem de programação)

Se você já sabe essas três coisas, está pronto para React!

## Configurando o Ambiente

### Instalação do Node.js

React precisa do Node.js para funcionar. É como o motor que faz tudo funcionar.

1. Vá para [nodejs.org](https://nodejs.org)
2. Baixe a versão "LTS" (mais estável)
3. Instale como qualquer outro programa

### Criando seu primeiro projeto React

Abra o terminal (Prompt de Comando no Windows, Terminal no Mac/Linux) e digite:

```bash
npx create-react-app meu-primeiro-app
cd meu-primeiro-app
npm start
```

Pronto! Seu navegador vai abrir automaticamente mostrando seu aplicativo React.

## Conceitos Fundamentais

### 1. Componentes

Um componente é como um bloco de LEGO. Cada componente tem:
- Uma função específica
- Pode ser usado várias vezes
- Pode ser combinado com outros componentes

**Exemplo de componente simples:**
```jsx
function Saudacao() {
  return <h1>Olá, mundo!</h1>;
}
```

### 2. JSX

JSX parece HTML, mas é JavaScript. É a linguagem que usamos para descrever como os componentes devem aparecer na tela.

**Comparação:**
```javascript
// JavaScript normal
const elemento = document.createElement('h1');
elemento.textContent = 'Olá';

// JSX (React)
const elemento = <h1>Olá</h1>;
```

### 3. Props (Propriedades)

Props são como instruções que você passa para um componente. Imagine que você tem um componente "Botão" e quer que ele tenha cores diferentes em lugares diferentes.

**Exemplo:**
```jsx
function Botao(props) {
  return <button style={{backgroundColor: props.cor}}>
    {props.texto}
  </button>;
}

// Usando o botão com props diferentes
<Botao cor="azul" texto="Clique aqui" />
<Botao cor="vermelho" texto="Perigo!" />
```

### 4. Estado (State)

O estado é a memória do componente. Ele lembra informações que podem mudar com o tempo.

**Exemplo - contador:**
```jsx
import { useState } from 'react';

function Contador() {
  // "contador" é o valor, "setContador" é a função para mudá-lo
  const [contador, setContador] = useState(0);

  return (
    <div>
      <p>Você clicou {contador} vezes</p>
      <button onClick={() => setContador(contador + 1)}>
        Clique para aumentar
      </button>
    </div>
  );
}
```

## Estrutura de um Projeto React

Quando você cria um projeto React, ele vem organizado assim:

```
meu-primeiro-app/
├── public/
│   ├── index.html        # Página principal
│   └── ...
├── src/
│   ├── App.js           # Componente principal
│   ├── index.js         # Ponto de entrada
│   └── ...
├── package.json         # Informações do projeto
└── README.md            # Documentação
```

## Primeiro Componente Completo

Vamos criar um componente que mostra uma lista de tarefas (to-do list):

```jsx
import { useState } from 'react';

function ListaDeTarefas() {
  const [tarefas, setTarefas] = useState([
    'Aprender React',
    'Fazer exercícios',
    'Ler documentação'
  ]);
  const [novaTarefa, setNovaTarefa] = useState('');

  function adicionarTarefa() {
    if (novaTarefa.trim() !== '') {
      setTarefas([...tarefas, novaTarefa]);
      setNovaTarefa('');
    }
  }

  return (
    <div>
      <h2>Minhas Tarefas</h2>
      
      <div>
        <input
          type="text"
          value={novaTarefa}
          onChange={(e) => setNovaTarefa(e.target.value)}
          placeholder="Digite uma nova tarefa"
        />
        <button onClick={adicionarTarefa}>Adicionar</button>
      </div>

      <ul>
        {tarefas.map((tarefa, index) => (
          <li key={index}>{tarefa}</li>
        ))}
      </ul>
    </div>
  );
}

export default ListaDeTarefas;
```

## Eventos no React

Eventos são ações do usuário, como clicar, digitar, ou passar o mouse. No React, lidamos com eventos assim:

```jsx
function ManipuladorEventos() {
  function handleClick() {
    alert('Botão clicado!');
  }

  function handleChange(event) {
    console.log('Texto alterado:', event.target.value);
  }

  return (
    <div>
      <button onClick={handleClick}>Clique-me</button>
      <input onChange={handleChange} placeholder="Digite algo" />
    </div>
  );
}
```

## Renderização Condicional

Às vezes você quer mostrar coisas diferentes dependendo de uma condição:

```jsx
function Mensagem({ estaLogado }) {
  if (estaLogado) {
    return <h1>Bem-vindo de volta!</h1>;
  } else {
    return <h1>Por favor, faça login</h1>;
  }
}

// Ou de forma mais curta:
function MensagemCurta({ estaLogado }) {
  return (
    <div>
      {estaLogado ? (
        <h1>Bem-vindo de volta!</h1>
      ) : (
        <h1>Por favor, faça login</h1>
      )}
    </div>
  );
}
```

## Listas no React

Para mostrar listas, usamos o método `map()`:

```jsx
function ListaDeFrutas() {
  const frutas = ['Maçã', 'Banana', 'Laranja', 'Uva'];

  return (
    <ul>
      {frutas.map((fruta, index) => (
        // A "key" ajuda o React a identificar cada item
        <li key={index}>{fruta}</li>
      ))}
    </ul>
  );
}
```

## Ciclo de Vida dos Componentes

Componentes têm um ciclo de vida como seres vivos:
1. **Nascimento** (montagem) - `useEffect(() => {}, [])`
2. **Atualização** - `useEffect(() => {})`
3. **Morte** (desmontagem) - `useEffect(() => { return () => {} }, [])`

**Exemplo:**
```jsx
import { useState, useEffect } from 'react';

function Relogio() {
  const [hora, setHora] = useState(new Date());

  useEffect(() => {
    // Isso roda quando o componente nasce
    const intervalo = setInterval(() => {
      setHora(new Date());
    }, 1000);

    // Isso roda quando o componente morre
    return () => {
      clearInterval(intervalo);
    };
  }, []); // Array vazio = roda apenas uma vez

  return <div>Hora atual: {hora.toLocaleTimeString()}</div>;
}
```

## Hooks (Ganchos)

Hooks são funções especiais que permitem "conectar-se" aos recursos do React. Os mais importantes são:

### useState
Gerencia estado (informações que mudam).

### useEffect
Executa código quando algo acontece (componente monta, estado muda, etc.).

### useContext
Compartilha informações entre componentes sem precisar passar props manualmente.

**Exemplo de useContext:**
```jsx
import { createContext, useContext } from 'react';

// 1. Criar o contexto
const TemaContext = createContext();

// 2. Provedor (fornece o valor)
function App() {
  return (
    <TemaContext.Provider value="escuro">
      <ComponenteFilho />
    </TemaContext.Provider>
  );
}

// 3. Consumidor (usa o valor)
function ComponenteFilho() {
  const tema = useContext(TemaContext);
  return <div>Tema atual: {tema}</div>;
}
```

## Projeto Prático: Jogo da Velha

Vamos criar um jogo completo para praticar:

```jsx
import { useState } from 'react';

function JogoDaVelha() {
  const [tabuleiro, setTabuleiro] = useState(Array(9).fill(null));
  const [jogadorX, setJogadorX] = useState(true);
  const [vencedor, setVencedor] = useState(null);

  function handleClick(index) {
    if (tabuleiro[index] || vencedor) return;

    const novoTabuleiro = [...tabuleiro];
    novoTabuleiro[index] = jogadorX ? 'X' : 'O';
    
    setTabuleiro(novoTabuleiro);
    setJogadorX(!jogadorX);
    
    // Verificar se há vencedor
    const resultado = verificarVencedor(novoTabuleiro);
    if (resultado) {
      setVencedor(resultado);
    }
  }

  function verificarVencedor(tab) {
    const linhasVencedoras = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8], // Horizontal
      [0, 3, 6], [1, 4, 7], [2, 5, 8], // Vertical
      [0, 4, 8], [2, 4, 6]             // Diagonal
    ];

    for (let linha of linhasVencedoras) {
      const [a, b, c] = linha;
      if (tab[a] && tab[a] === tab[b] && tab[a] === tab[c]) {
        return tab[a];
      }
    }
    return null;
  }

  function reiniciarJogo() {
    setTabuleiro(Array(9).fill(null));
    setJogadorX(true);
    setVencedor(null);
  }

  return (
    <div style={{textAlign: 'center'}}>
      <h1>Jogo da Velha</h1>
      
      {vencedor ? (
        <h2>Vencedor: {vencedor}!</h2>
      ) : (
        <h2>Próximo jogador: {jogadorX ? 'X' : 'O'}</h2>
      )}
      
      <div style={{
        display: 'grid',
        gridTemplateColumns: 'repeat(3, 100px)',
        justifyContent: 'center',
        gap: '5px',
        margin: '20px auto'
      }}>
        {tabuleiro.map((valor, index) => (
          <button
            key={index}
            onClick={() => handleClick(index)}
            style={{
              width: '100px',
              height: '100px',
              fontSize: '2em',
              backgroundColor: '#f0f0f0'
            }}
          >
            {valor}
          </button>
        ))}
      </div>
      
      <button onClick={reiniciarJogo} style={{padding: '10px 20px'}}>
        Reiniciar Jogo
      </button>
    </div>
  );
}

export default JogoDaVelha;
```

## Dicas Importantes

### 1. Componentes Puros
Crie componentes que, com as mesmas props, sempre renderizam o mesmo resultado.

### 2. Keys em Listas
Sempre use `key` única em listas para ajudar o React a identificar itens.

### 3. Estado Imutável
Nunca modifique o estado diretamente. Sempre use a função setter.

**❌ ERRADO:**
```jsx
const [lista, setLista] = useState([]);
lista.push('novo item'); // NUNCA FAÇA ISSO
```

**✅ CERTO:**
```jsx
const [lista, setLista] = useState([]);
setLista([...lista, 'novo item']);
```

### 4. Separação de Responsabilidades
Divida componentes grandes em menores.

## Próximos Passos

Depois de dominar o básico, explore:

1. **React Router** - Para navegação entre páginas
2. **Redux ou Context API** - Para gerenciamento de estado global
3. **Styled Components ou Tailwind CSS** - Para estilização
4. **Next.js** - Framework React para produção
5. **React Native** - Para criar aplicativos móveis

## Recursos de Aprendizado

### Oficiais
- [Documentação do React](https://reactjs.org/docs/getting-started.html) (em inglês)
- [Tutorial Oficial](https://reactjs.org/tutorial/tutorial.html)

### Em Português
- [Brazilian JS](https://braziljs.org/react/)
- [Rocketseat](https://www.rocketseat.com.br/)

### Prática
- [CodeSandbox](https://codesandbox.io/) - Para testar código online
- [React Challenges](https://react.challenges/) - Desafios para praticar

## Glossário

- **Componente**: Bloco de construção do React
- **JSX**: Sintaxe que parece HTML mas é JavaScript
- **Props**: Propriedades passadas para componentes
- **Estado**: Dados que podem mudar dentro de um componente
- **Hook**: Função especial que "conecta" você aos recursos do React
- **Renderização**: Processo de mostrar componentes na tela
- **Virtual DOM**: Cópia leve do DOM real que o React usa para atualizações eficientes

---

## Exercício Final

Crie um aplicativo de previsão do tempo que:
1. Permita ao usuário digitar o nome de uma cidade
2. Mostre a temperatura atual (pode ser fixa para teste)
3. Mostre se está ensolarado, nublado ou chovendo
4. Tenha um botão para alternar entre Celsius e Fahrenheit

**Dica:** Comece com dados fixos e depois adicione funcionalidades uma por uma.

---

Lembre-se: A prática leva à perfeição! Comece com projetos pequenos e vá aumentando a complexidade gradualmente. React parece complicado no início, mas com tempo e prática, você dominará essa ferramenta poderosa.

Boa codificação! 🚀
