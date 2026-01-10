# Manual Completo de ReactJS

## Sumário

1. [Introdução ao React](#introdução-ao-react)
2. [Configuração do Ambiente](#configuração-do-ambiente)
3. [Fundamentos do React](#fundamentos-do-react)
4. [JSX e Renderização](#jsx-e-renderização)
5. [Componentes](#componentes)
6. [Props (Propriedades)](#props-propriedades)
7. [Estado (State)](#estado-state)
8. [Eventos](#eventos)
9. [Renderização Condicional](#renderização-condicional-1)
10. [Listas e Keys](#listas-e-keys)
11. [Formulários](#formulários)
12. [Hooks](#hooks)
13. [Ciclo de Vida](#ciclo-de-vida)
14. [Context API](#context-api)
15. [React Router](#react-router)
16. [Hooks Avançados](#hooks-avançados)
17. [Performance](#performance)
18. [Boas Práticas](#boas-práticas)
19. [Projetos Práticos](#projetos-práticos)
20. [Ecossistema React](#ecossistema-react)

---

## Introdução ao React

### O que é React?

React é uma biblioteca JavaScript de código aberto desenvolvida pelo Facebook para construir interfaces de usuário (UI). Foi lançada em 2013 e rapidamente se tornou uma das ferramentas mais populares para desenvolvimento front-end.

### Por que usar React?

- **Componentização**: Permite dividir a UI em componentes independentes e reutilizáveis
- **Virtual DOM**: Otimiza o desempenho ao atualizar apenas o necessário
- **Declarativo**: Você descreve como a UI deve ser em cada estado
- **Unidirecional**: Fluxo de dados previsível (one-way data flow)
- **Ecosistema rico**: Vasta coleção de bibliotecas e ferramentas

### React vs ReactDOM

- **React**: Lógica e componentes
- **ReactDOM**: Renderização no navegador

```javascript
// React (lógica)
import { useState, createElement } from 'react';

// ReactDOM (renderização)
import { render } from 'react-dom';
```

---

## Configuração do Ambiente

### Métodos de Instalação

#### 1. Create React App (Recomendado para iniciantes)

```bash
npx create-react-app meu-app
cd meu-app
npm start
```

#### 2. Vite (Mais rápido)

```bash
npm create vite@latest meu-app -- --template react
cd meu-app
npm install
npm run dev
```

#### 3. Configuração Manual

```json
// package.json básico
{
  "name": "react-app",
  "version": "1.0.0",
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/react": "^18.0.0",
    "@types/react-dom": "^18.0.0",
    "typescript": "^4.9.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test"
  }
}
```

### Estrutura de Projeto

```
src/
├── components/         # Componentes reutilizáveis
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.module.css
│   │   └── index.js
│   └── Header/
├── pages/             # Componentes de página
│   ├── Home.jsx
│   └── About.jsx
├── hooks/             # Custom hooks
├── context/           # Context providers
├── utils/             # Funções utilitárias
├── styles/            # Estilos globais
├── App.jsx            # Componente principal
└── index.js           # Ponto de entrada
```

---

## Fundamentos do React

### Elementos React

```jsx
// Elemento React (não é um DOM real ainda)
const elemento = <h1>Olá, mundo!</h1>;

// Equivalente em JavaScript puro:
const elemento = React.createElement('h1', null, 'Olá, mundo!');
```

### Componentes Básicos

```jsx
// 1. Componente de Função
function Welcome() {
  return <h1>Olá, React!</h1>;
}

// 2. Componente de Classe (legado)
class Welcome extends React.Component {
  render() {
    return <h1>Olá, React!</h1>;
  }
}

// 3. Arrow Function
const Welcome = () => <h1>Olá, React!</h1>;
```

---

## JSX e Renderização

### Sintaxe JSX

```jsx
// Expressões JavaScript em JSX
const nome = 'Maria';
const elemento = <h1>Olá, {nome}!</h1>;

// Atributos JSX
const elemento = <img src={url} alt={descricao} className="imagem" />;

// JSX é uma expressão
function getGreeting(usuario) {
  if (usuario) {
    return <h1>Olá, {usuario.nome}!</h1>;
  }
  return <h1>Olá, estranho.</h1>;
}
```

### Regras do JSX

1. **Sempre feche tags**
   ```jsx
   <img src="..." />  // Correto
   <img src="...">    // Errado
   ```

2. **className em vez de class**
   ```jsx
   <div className="container">  // Correto
   <div class="container">      // Errado
   ```

3. **CamelCase para eventos**
   ```jsx
   <button onClick={handleClick}>  // Correto
   <button onclick={handleClick}>  // Errado
   ```

4. **Fragmentos para múltiplos elementos**
   ```jsx
   <>
     <h1>Título</h1>
     <p>Parágrafo</p>
   </>
   ```

### Renderizando no DOM

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

function App() {
  return <h1>Olá, React!</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

---

## Componentes

### Componentes Funcionais

```jsx
// Componente simples
function Card({ titulo, conteudo }) {
  return (
    <div className="card">
      <h3>{titulo}</h3>
      <p>{conteudo}</p>
    </div>
  );
}

// Componente com children
function Container({ children }) {
  return <div className="container">{children}</div>;
}

// Uso
<Container>
  <Card titulo="React" conteudo="Biblioteca JavaScript" />
</Container>
```

### Componentes de Classe (Legado)

```jsx
class Contador extends React.Component {
  constructor(props) {
    super(props);
    this.state = { contador: 0 };
  }

  incrementar = () => {
    this.setState({ contador: this.state.contador + 1 });
  };

  render() {
    return (
      <div>
        <p>Contagem: {this.state.contador}</p>
        <button onClick={this.incrementar}>Incrementar</button>
      </div>
    );
  }
}
```

### Composição de Componentes

```jsx
// Componente pai
function App() {
  return (
    <Layout>
      <Header />
      <Sidebar />
      <MainContent>
        <Article titulo="React é incrível!" />
        <CommentSection />
      </MainContent>
      <Footer />
    </Layout>
  );
}
```

---

## Props (Propriedades)

### Passando Props

```jsx
// Componente que recebe props
function UserCard({ nome, idade, email }) {
  return (
    <div className="user-card">
      <h2>{nome}</h2>
      <p>Idade: {idade}</p>
      <p>Email: {email}</p>
    </div>
  );
}

// Passando props
<UserCard nome="João Silva" idade={25} email="joao@email.com" />
```

### Props Default

```jsx
// Valores padrão
function Botao({ texto, cor = 'azul', tamanho = 'medio' }) {
  return (
    <button className={`botao ${cor} ${tamanho}`}>
      {texto}
    </button>
  );
}

// Uso
<Botao texto="Clique-me" />
<Botao texto="Salvar" cor="verde" tamanho="grande" />
```

### Children Prop

```jsx
function Modal({ children, titulo }) {
  return (
    <div className="modal">
      <div className="modal-header">
        <h2>{titulo}</h2>
      </div>
      <div className="modal-body">
        {children}
      </div>
    </div>
  );
}

// Uso
<Modal titulo="Confirmação">
  <p>Tem certeza que deseja excluir?</p>
  <button>Sim</button>
  <button>Não</button>
</Modal>
```

### Prop Types (Validação)

```jsx
import PropTypes from 'prop-types';

function Product({ nome, preco, emEstoque }) {
  return (
    <div className="product">
      <h3>{nome}</h3>
      <p>Preço: R${preco.toFixed(2)}</p>
      <p>{emEstoque ? 'Disponível' : 'Esgotado'}</p>
    </div>
  );
}

Product.propTypes = {
  nome: PropTypes.string.isRequired,
  preco: PropTypes.number.isRequired,
  emEstoque: PropTypes.bool
};

Product.defaultProps = {
  emEstoque: false
};
```

---

## Estado (State)

### useState Hook

```jsx
import { useState } from 'react';

function Contador() {
  // Estado simples
  const [contador, setContador] = useState(0);
  
  // Estado com objeto
  const [usuario, setUsuario] = useState({
    nome: '',
    email: '',
    idade: 0
  });
  
  // Estado com array
  const [tarefas, setTarefas] = useState([]);

  // Atualizando estado
  const incrementar = () => {
    setContador(contador + 1);
    // ou função de atualização
    setContador(prev => prev + 1);
  };

  // Atualizando objeto
  const atualizarNome = (novoNome) => {
    setUsuario(prev => ({
      ...prev,
      nome: novoNome
    }));
  };

  // Atualizando array
  const adicionarTarefa = (novaTarefa) => {
    setTarefas(prev => [...prev, novaTarefa]);
  };

  return (
    <div>
      <p>Contador: {contador}</p>
      <button onClick={incrementar}>+</button>
    </div>
  );
}
```

### Regras do Estado

1. **Não modifique diretamente**
   ```jsx
   // ❌ ERRADO
   usuario.nome = 'Novo nome';
   
   // ✅ CORRETO
   setUsuario({ ...usuario, nome: 'Novo nome' });
   ```

2. **Atualizações assíncronas**
   ```jsx
   // Estado pode não estar atualizado imediatamente
   console.log(contador); // Valor antigo
   setContador(contador + 1);
   console.log(contador); // Ainda valor antigo
   
   // Use função de atualização para o valor mais recente
   setContador(prevContador => prevContador + 1);
   ```

3. **Merge superficial**
   ```jsx
   const [objeto, setObjeto] = useState({ a: 1, b: 2 });
   
   // Perde a propriedade 'b'
   setObjeto({ a: 3 }); // {a: 3}
   
   // Mantém todas as propriedades
   setObjeto(prev => ({ ...prev, a: 3 })); // {a: 3, b: 2}
   ```

### Estado Compartilhado

```jsx
// Componente pai gerencia estado
function App() {
  const [contador, setContador] = useState(0);

  return (
    <div>
      <Exibidor contador={contador} />
      <Controlador setContador={setContador} />
    </div>
  );
}

function Exibidor({ contador }) {
  return <h2>Contagem: {contador}</h2>;
}

function Controlador({ setContador }) {
  return (
    <div>
      <button onClick={() => setContador(prev => prev + 1)}>+</button>
      <button onClick={() => setContador(prev => prev - 1)}>-</button>
    </div>
  );
}
```

---

## Eventos

### Manipuladores de Eventos

```jsx
function EventosComponent() {
  // Handler inline
  const handleClick = () => {
    console.log('Clicado!');
  };

  // Handler com parâmetros
  const handleSubmit = (event, tipo) => {
    event.preventDefault();
    console.log('Formulário', tipo, 'enviado');
  };

  // Handler com estado
  const [texto, setTexto] = useState('');
  const handleChange = (event) => {
    setTexto(event.target.value);
  };

  return (
    <div>
      {/* Clique */}
      <button onClick={handleClick}>Clique</button>
      
      {/* Parâmetros */}
      <form onSubmit={(e) => handleSubmit(e, 'login')}>
        <input type="text" onChange={handleChange} value={texto} />
        <button type="submit">Enviar</button>
      </form>
      
      {/* Eventos de teclado */}
      <input onKeyDown={(e) => {
        if (e.key === 'Enter') {
          console.log('Enter pressionado');
        }
      }} />
    </div>
  );
}
```

### Eventos Comuns

```jsx
// Eventos de Mouse
onClick        // Clique
onDoubleClick  // Duplo clique
onMouseEnter   // Mouse entra
onMouseLeave   // Mouse sai
onMouseMove    // Mouse move

// Eventos de Teclado
onKeyDown      // Tecla pressionada
onKeyUp        // Tecla liberada
onKeyPress     // Tecla pressionada e liberada

// Eventos de Formulário
onChange       // Valor mudou
onSubmit       // Formulário enviado
onFocus        // Elemento focado
onBlur         // Elemento perdeu foco

// Eventos de Drag & Drop
onDragStart    // Começou a arrastar
onDragOver     // Arrastando sobre
onDrop         // Soltou

// Eventos de Mídia
onPlay         // Mídia começou
onPause        // Mídia pausada
onEnded        // Mídia terminou
```

### Prevenindo Comportamento Padrão

```jsx
function Formulario() {
  const handleSubmit = (event) => {
    event.preventDefault(); // Evita recarregar a página
    console.log('Formulário enviado');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

---

## Renderização Condicional

### If/Else

```jsx
function Saudacao({ usuario }) {
  if (usuario) {
    return <h1>Olá, {usuario.nome}!</h1>;
  } else {
    return <h1>Olá, visitante!</h1>;
  }
}
```

### Operador Ternário

```jsx
function Mensagem({ error }) {
  return (
    <div>
      {error ? (
        <p className="error">{error}</p>
      ) : (
        <p className="success">Operação bem-sucedida!</p>
      )}
    </div>
  );
}
```

### Operador &&

```jsx
function Notificacao({ mensagem }) {
  return (
    <div>
      {mensagem && (
        <div className="notificacao">
          <p>{mensagem}</p>
        </div>
      )}
    </div>
  );
}
```

### Switch

```jsx
function StatusIndicator({ status }) {
  switch (status) {
    case 'loading':
      return <Spinner />;
    case 'success':
      return <Checkmark />;
    case 'error':
      return <ErrorIcon />;
    default:
      return <InfoIcon />;
  }
}
```

### Renderização Condicional em Elementos

```jsx
function UserProfile({ user }) {
  return (
    <div>
      <h1>Perfil do Usuário</h1>
      {user && (
        <>
          <p>Nome: {user.name}</p>
          <p>Email: {user.email}</p>
          {user.isAdmin && <AdminPanel />}
        </>
      )}
      {!user && <p>Por favor, faça login</p>}
    </div>
  );
}
```

---

## Listas e Keys

### Renderizando Arrays

```jsx
function ListaDeNumeros() {
  const numeros = [1, 2, 3, 4, 5];
  
  return (
    <ul>
      {numeros.map((numero) => (
        <li key={numero}>{numero}</li>
      ))}
    </ul>
  );
}
```

### Keys

```jsx
function ListaDeTarefas() {
  const [tarefas, setTarefas] = useState([
    { id: 1, texto: 'Aprender React', concluida: true },
    { id: 2, texto: 'Criar projeto', concluida: false },
    { id: 3, texto: 'Deploy', concluida: false }
  ]);

  return (
    <ul>
      {tarefas.map((tarefa) => (
        <li key={tarefa.id}>
          <input
            type="checkbox"
            checked={tarefa.concluida}
            onChange={() => toggleTarefa(tarefa.id)}
          />
          {tarefa.texto}
        </li>
      ))}
    </ul>
  );
}
```

### Regras para Keys

1. **Únicas entre irmãos**
2. **Estáveis** (não mude entre renderizações)
3. **Evite índices como keys** (exceto para listas estáticas)

```jsx
// ❌ RUIM (se a ordem pode mudar)
{tarefas.map((tarefa, index) => (
  <li key={index}>{tarefa.texto}</li>
))}

// ✅ BOM (ID único)
{tarefas.map((tarefa) => (
  <li key={tarefa.id}>{tarefa.texto}</li>
))}

// ✅ ACEITÁVEL (listas estáticas)
{['Leite', 'Ovos', 'Pão'].map((item, index) => (
  <li key={index}>{item}</li>
))}
```

### Componente de Lista

```jsx
function ItemLista({ itens, onRemoverItem }) {
  return (
    <ul className="lista">
      {itens.map((item) => (
        <Item
          key={item.id}
          item={item}
          onRemover={() => onRemoverItem(item.id)}
        />
      ))}
    </ul>
  );
}

function Item({ item, onRemover }) {
  return (
    <li className="item">
      <span>{item.nome}</span>
      <button onClick={onRemover}>Remover</button>
    </li>
  );
}
```

---

## Formulários

### Formulário Controlado

```jsx
function FormularioControado() {
  const [formData, setFormData] = useState({
    nome: '',
    email: '',
    senha: '',
    termos: false
  });

  const handleChange = (event) => {
    const { name, value, type, checked } = event.target;
    
    setFormData(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log('Dados do formulário:', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>
          Nome:
          <input
            type="text"
            name="nome"
            value={formData.nome}
            onChange={handleChange}
          />
        </label>
      </div>
      
      <div>
        <label>
          Email:
          <input
            type="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
          />
        </label>
      </div>
      
      <div>
        <label>
          Senha:
          <input
            type="password"
            name="senha"
            value={formData.senha}
            onChange={handleChange}
          />
        </label>
      </div>
      
      <div>
        <label>
          <input
            type="checkbox"
            name="termos"
            checked={formData.termos}
            onChange={handleChange}
          />
          Aceito os termos
        </label>
      </div>
      
      <button type="submit">Enviar</button>
    </form>
  );
}
```

### Formulário Não Controlado

```jsx
function FormularioNaoControlado() {
  const nomeInput = useRef();
  const emailInput = useRef();

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log('Nome:', nomeInput.current.value);
    console.log('Email:', emailInput.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" ref={nomeInput} placeholder="Nome" />
      <input type="email" ref={emailInput} placeholder="Email" />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

### Validação de Formulários

```jsx
function FormularioValidado() {
  const [formData, setFormData] = useState({
    email: '',
    senha: ''
  });
  
  const [erros, setErros] = useState({});
  const [tocado, setTocado] = useState({});

  const validar = () => {
    const novosErros = {};
    
    if (!formData.email) {
      novosErros.email = 'Email é obrigatório';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      novosErros.email = 'Email inválido';
    }
    
    if (!formData.senha) {
      novosErros.senha = 'Senha é obrigatória';
    } else if (formData.senha.length < 6) {
      novosErros.senha = 'Senha deve ter pelo menos 6 caracteres';
    }
    
    return novosErros;
  };

  const handleChange = (event) => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    // Validação em tempo real após toque
    if (tocado[name]) {
      const errosAtuais = validar();
      setErros(prev => ({ ...prev, [name]: errosAtuais[name] || '' }));
    }
  };

  const handleBlur = (event) => {
    const { name } = event.target;
    setTocado(prev => ({ ...prev, [name]: true }));
    
    const errosAtuais = validar();
    setErros(prev => ({ ...prev, [name]: errosAtuais[name] || '' }));
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    
    // Marcar todos os campos como tocados
    setTocado({
      email: true,
      senha: true
    });
    
    const errosAtuais = validar();
    setErros(errosAtuais);
    
    if (Object.keys(errosAtuais).length === 0) {
      console.log('Formulário válido:', formData);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Email:</label>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          onBlur={handleBlur}
        />
        {erros.email && <span className="erro">{erros.email}</span>}
      </div>
      
      <div>
        <label>Senha:</label>
        <input
          type="password"
          name="senha"
          value={formData.senha}
          onChange={handleChange}
          onBlur={handleBlur}
        />
        {erros.senha && <span className="erro">{erros.senha}</span>}
      </div>
      
      <button type="submit">Enviar</button>
    </form>
  );
}
```

### Biblioteca Formik (Opcional)

```jsx
import { useFormik } from 'formik';

function FormularioFormik() {
  const formik = useFormik({
    initialValues: {
      nome: '',
      email: '',
    },
    onSubmit: values => {
      console.log('Dados:', values);
    },
  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <input
        name="nome"
        value={formik.values.nome}
        onChange={formik.handleChange}
      />
      <input
        name="email"
        value={formik.values.email}
        onChange={formik.handleChange}
      />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

---

## Hooks

### useState

```jsx
// Uso básico
const [estado, setEstado] = useState(valorInicial);

// Estado com função inicial (lazy initialization)
const [contador, setContador] = useState(() => {
  const inicial = localStorage.getItem('contador');
  return inicial ? parseInt(inicial) : 0;
});

// Estado com objeto
const [usuario, setUsuario] = useState({
  nome: '',
  idade: 0,
  email: ''
});

// Atualização funcional
setUsuario(prev => ({
  ...prev,
  nome: 'Novo Nome'
}));
```

### useEffect

```jsx
import { useEffect } from 'react';

function ExemploUseEffect() {
  // Executa em toda renderização
  useEffect(() => {
    console.log('Renderizou!');
  });

  // Executa apenas no mount
  useEffect(() => {
    console.log('Componente montou');
    
    // Cleanup no unmount
    return () => {
      console.log('Componente desmontou');
    };
  }, []); // Array de dependências vazio

  // Executa quando 'prop' mudar
  useEffect(() => {
    console.log('Prop mudou:', prop);
  }, [prop]); // Lista de dependências

  // Executa quando múltiplas dependências mudarem
  useEffect(() => {
    // Fetch data quando userId ou filtro mudar
    fetchData(userId, filtro);
  }, [userId, filtro]);
}
```

### Exemplos Práticos de useEffect

```jsx
// 1. Fetch de dados
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    let mounted = true;
    
    async function fetchUser() {
      setLoading(true);
      try {
        const response = await fetch(`/api/users/${userId}`);
        const data = await response.json();
        
        if (mounted) {
          setUser(data);
          setLoading(false);
        }
      } catch (error) {
        if (mounted) {
          console.error('Erro:', error);
          setLoading(false);
        }
      }
    }

    fetchUser();

    // Cleanup para evitar state update após unmount
    return () => {
      mounted = false;
    };
  }, [userId]); // Re-executa quando userId mudar

  if (loading) return <div>Carregando...</div>;
  return <div>{user.name}</div>;
}

// 2. Event listeners
function WindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });

  useEffect(() => {
    function handleResize() {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    }

    window.addEventListener('resize', handleResize);
    
    // Cleanup
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []); // Executa apenas no mount

  return (
    <div>
      Largura: {size.width}px, Altura: {size.height}px
    </div>
  );
}

// 3. Timer
function Timer() {
  const [segundos, setSegundos] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSegundos(s => s + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  return <div>Segundos: {segundos}</div>;
}
```

### useContext

```jsx
// 1. Criar Contexto
const TemaContext = createContext('claro');

// 2. Provider
function App() {
  const [tema, setTema] = useState('claro');
  
  return (
    <TemaContext.Provider value={{ tema, setTema }}>
      <ComponenteFilho />
    </TemaContext.Provider>
  );
}

// 3. Consumir com useContext
function ComponenteFilho() {
  const { tema, setTema } = useContext(TemaContext);
  
  return (
    <div className={`app ${tema}`}>
      <button onClick={() => setTema(tema === 'claro' ? 'escuro' : 'claro')}>
        Alternar Tema
      </button>
    </div>
  );
}
```

### useRef

```jsx
function ExemploUseRef() {
  // 1. Referência a elemento DOM
  const inputRef = useRef(null);
  
  // 2. Valores mutáveis que não causam re-render
  const renderCount = useRef(0);
  const prevValue = useRef('');

  useEffect(() => {
    renderCount.current += 1;
    console.log(`Renderizou ${renderCount.current} vezes`);
  });

  const focusInput = () => {
    inputRef.current.focus();
    inputRef.current.value = 'Focado!';
  };

  // 3. Guardar valor anterior
  useEffect(() => {
    prevValue.current = inputRef.current?.value || '';
  });

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focar Input</button>
      <p>Valor anterior: {prevValue.current}</p>
    </div>
  );
}
```

### useReducer

```jsx
// Reducer function
function contadorReducer(state, action) {
  switch (action.type) {
    case 'incrementar':
      return { contador: state.contador + 1 };
    case 'decrementar':
      return { contador: state.contador - 1 };
    case 'resetar':
      return { contador: 0 };
    case 'definir':
      return { contador: action.valor };
    default:
      throw new Error(`Ação desconhecida: ${action.type}`);
  }
}

function Contador() {
  const [state, dispatch] = useReducer(contadorReducer, { contador: 0 });

  return (
    <div>
      <p>Contagem: {state.contador}</p>
      <button onClick={() => dispatch({ type: 'incrementar' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrementar' })}>-</button>
      <button onClick={() => dispatch({ type: 'resetar' })}>Resetar</button>
      <button onClick={() => dispatch({ type: 'definir', valor: 10 })}>
        Definir para 10
      </button>
    </div>
  );
}
```

### useMemo

```jsx
function ListaComputada({ itens, filtro }) {
  // Só recalcula quando 'itens' ou 'filtro' mudar
  const itensFiltrados = useMemo(() => {
    console.log('Filtrando itens...');
    return itens.filter(item => 
      item.nome.toLowerCase().includes(filtro.toLowerCase())
    );
  }, [itens, filtro]);

  return (
    <ul>
      {itensFiltrados.map(item => (
        <li key={item.id}>{item.nome}</li>
      ))}
    </ul>
  );
}
```

### useCallback

```jsx
function Pai() {
  const [contador, setContador] = useState(0);
  
  // Memoiza a função para não criar nova em cada render
  const incrementar = useCallback(() => {
    setContador(c => c + 1);
  }, []); // Dependências vazias = função nunca muda

  return (
    <div>
      <p>Contador: {contador}</p>
      <Filho onIncrementar={incrementar} />
    </div>
  );
}

// Componente Filho otimizado
const Filho = React.memo(function Filho({ onIncrementar }) {
  console.log('Filho renderizou');
  return <button onClick={onIncrementar}>Incrementar</button>;
});
```

---

## Ciclo de Vida

### Componentes de Classe (Legado)

```jsx
class ComponenteComCicloDeVida extends React.Component {
  // 1. Montagem
  constructor(props) {
    super(props);
    this.state = { data: null };
  }

  componentDidMount() {
    // Executa após montagem no DOM
    console.log('Componente montou');
    this.fetchData();
  }

  // 2. Atualização
  componentDidUpdate(prevProps, prevState) {
    // Executa após atualização
    if (this.props.id !== prevProps.id) {
      this.fetchData();
    }
  }

  // 3. Desmontagem
  componentWillUnmount() {
    // Executa antes do componente ser removido
    console.log('Componente desmontará');
    this.cleanup();
  }

  // Métodos auxiliares
  fetchData() {
    // Fetch data
  }

  cleanup() {
    // Limpar event listeners, timers, etc.
  }

  render() {
    return <div>Componente</div>;
  }
}
```

### Equivalente em Hooks

```jsx
function ComponenteComHooks({ id }) {
  const [data, setData] = useState(null);

  // componentDidMount + componentWillUnmount
  useEffect(() => {
    console.log('Componente montou');
    fetchData();
    
    return () => {
      console.log('Componente desmontará');
      cleanup();
    };
  }, []);

  // componentDidUpdate para 'id'
  useEffect(() => {
    if (id) {
      fetchData(id);
    }
  }, [id]); // Executa quando 'id' mudar

  const fetchData = async (currentId) => {
    // Fetch data
  };

  const cleanup = () => {
    // Limpar resources
  };

  return <div>Componente</div>;
}
```

### Diagrama do Ciclo de Vida

```
Montagem:
constructor() → render() → componentDidMount()

Atualização:
nova props/state → render() → componentDidUpdate()

Desmontagem:
componentWillUnmount()
```

---

## Context API

### Criando Contexto

```jsx
// contexts/TemaContext.js
import { createContext, useState, useContext } from 'react';

const TemaContext = createContext({
  tema: 'claro',
  alternarTema: () => {}
});

export const useTema = () => useContext(TemaContext);

export function TemaProvider({ children }) {
  const [tema, setTema] = useState('claro');

  const alternarTema = () => {
    setTema(temaAtual => temaAtual === 'claro' ? 'escuro' : 'claro');
  };

  return (
    <TemaContext.Provider value={{ tema, alternarTema }}>
      {children}
    </TemaContext.Provider>
  );
}
```

### Usando Contexto

```jsx
// App.js
import { TemaProvider } from './contexts/TemaContext';

function App() {
  return (
    <TemaProvider>
      <Header />
      <MainContent />
      <Footer />
    </TemaProvider>
  );
}

// Componente que usa o contexto
function Header() {
  const { tema, alternarTema } = useTema();

  return (
    <header className={`header ${tema}`}>
      <h1>Meu App</h1>
      <button onClick={alternarTema}>
        Alternar para {tema === 'claro' ? 'Escuro' : 'Claro'}
      </button>
    </header>
  );
}
```

### Contexto com Reducer

```jsx
// contexts/CarrinhoContext.js
import { createContext, useReducer, useContext } from 'react';

const CarrinhoContext = createContext();

const carrinhoReducer = (state, action) => {
  switch (action.type) {
    case 'ADICIONAR_ITEM':
      const itemExistente = state.itens.find(item => item.id === action.item.id);
      if (itemExistente) {
        return {
          ...state,
          itens: state.itens.map(item =>
            item.id === action.item.id
              ? { ...item, quantidade: item.quantidade + 1 }
              : item
          )
        };
      }
      return {
        ...state,
        itens: [...state.itens, { ...action.item, quantidade: 1 }]
      };
      
    case 'REMOVER_ITEM':
      return {
        ...state,
        itens: state.itens.filter(item => item.id !== action.itemId)
      };
      
    case 'LIMPAR_CARRINHO':
      return { ...state, itens: [] };
      
    default:
      return state;
  }
};

export function CarrinhoProvider({ children }) {
  const [state, dispatch] = useReducer(carrinhoReducer, {
    itens: [],
    total: 0
  });

  const valorTotal = state.itens.reduce(
    (total, item) => total + (item.preco * item.quantidade),
    0
  );

  const valor = {
    ...state,
    total: valorTotal,
    dispatch
  };

  return (
    <CarrinhoContext.Provider value={valor}>
      {children}
    </CarrinhoContext.Provider>
  );
}

export const useCarrinho = () => useContext(CarrinhoContext);
```

---

## React Router

### Instalação

```bash
npm install react-router-dom
```

### Configuração Básica

```jsx
// App.js
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/sobre">Sobre</Link>
        <Link to="/contato">Contato</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/sobre" element={<Sobre />} />
        <Route path="/contato" element={<Contato />} />
        <Route path="/usuario/:id" element={<Usuario />} />
        <Route path="*" element={<Pagina404 />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### Navegação Programática

```jsx
import { useNavigate, useParams, useLocation } from 'react-router-dom';

function Usuario() {
  const navigate = useNavigate();
  const { id } = useParams();
  const location = useLocation();

  const handleVoltar = () => {
    navigate(-1); // Volta uma página
    // ou navigate('/home');
  };

  return (
    <div>
      <h1>Usuário ID: {id}</h1>
      <p>Localização atual: {location.pathname}</p>
      <button onClick={handleVoltar}>Voltar</button>
    </div>
  );
}
```

### Rotas Aninhadas

```jsx
// App.js
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<Home />} />
    <Route path="produtos" element={<Produtos />}>
      <Route index element={<ListaProdutos />} />
      <Route path=":id" element={<DetalheProduto />} />
      <Route path="novo" element={<NovoProduto />} />
    </Route>
    <Route path="*" element={<Pagina404 />} />
  </Route>
</Routes>

// Layout.js
import { Outlet } from 'react-router-dom';

function Layout() {
  return (
    <div>
      <Header />
      <main>
        <Outlet /> {/* Aqui as rotas filhas serão renderizadas */}
      </main>
      <Footer />
    </div>
  );
}
```

### Rotas Protegidas

```jsx
// PrivateRoute.js
import { Navigate } from 'react-router-dom';

function PrivateRoute({ children }) {
  const { usuario } = useAuth(); // Hook de autenticação

  if (!usuario) {
    return <Navigate to="/login" replace />;
  }

  return children;
}

// Uso
<Route
  path="/dashboard"
  element={
    <PrivateRoute>
      <Dashboard />
    </PrivateRoute>
  }
/>
```

### Navegação com Query Params

```jsx
import { useSearchParams } from 'react-router-dom';

function Produtos() {
  const [searchParams, setSearchParams] = useSearchParams();
  const filtro = searchParams.get('filtro') || '';

  const handleFilterChange = (novoFiltro) => {
    setSearchParams({ filtro: novoFiltro });
  };

  return (
    <div>
      <input
        value={filtro}
        onChange={(e) => handleFilterChange(e.target.value)}
        placeholder="Filtrar produtos"
      />
      {/* Lista de produtos filtrada */}
    </div>
  );
}
```

---

## Hooks Avançados

### useLayoutEffect

```jsx
import { useLayoutEffect, useRef } from 'react';

function MedirElemento() {
  const ref = useRef();
  const [tamanho, setTamanho] = useState({});

  useLayoutEffect(() => {
    // Executa sincronamente após mutações do DOM
    // Antes do navegador pintar a tela
    const { width, height } = ref.current.getBoundingClientRect();
    setTamanho({ width, height });
  }, []);

  return (
    <div ref={ref}>
      Largura: {tamanho.width}, Altura: {tamanho.height}
    </div>
  );
}
```

### useImperativeHandle

```jsx
import { forwardRef, useImperativeHandle, useRef } from 'react';

const InputPersonalizado = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
    limpar: () => {
      inputRef.current.value = '';
    },
    getValue: () => {
      return inputRef.current.value;
    }
  }));

  return <input ref={inputRef} {...props} />;
});

// Uso no componente pai
function Formulario() {
  const inputRef = useRef();

  const handleClick = () => {
    inputRef.current.focus();
    console.log('Valor:', inputRef.current.getValue());
  };

  return (
    <div>
      <InputPersonalizado ref={inputRef} />
      <button onClick={handleClick}>Focar e Logar</button>
    </div>
  );
}
```

### useDebugValue

```jsx
import { useDebugValue, useState } from 'react';

// Custom Hook
function useContador(initialValue = 0) {
  const [contador, setContador] = useState(initialValue);
  
  // Mostra valor no React DevTools
  useDebugValue(`Contador: ${contador}`);
  
  const incrementar = () => setContador(c => c + 1);
  const decrementar = () => setContador(c => c - 1);
  const resetar = () => setContador(initialValue);
  
  return { contador, incrementar, decrementar, resetar };
}

// Uso
function Componente() {
  const { contador, incrementar } = useContador(0);
  
  return (
    <div>
      <p>Contador: {contador}</p>
      <button onClick={incrementar}>+</button>
    </div>
  );
}
```

### Custom Hooks

```jsx
// hooks/useLocalStorage.js
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  // Estado para armazenar valor
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });

  // Atualizar localStorage quando valor mudar
  useEffect(() => {
    try {
      window.localStorage.setItem(key, JSON.stringify(storedValue));
    } catch (error) {
      console.error(error);
    }
  }, [key, storedValue]);

  return [storedValue, setStoredValue];
}

// hooks/useFetch.js
import { useState, useEffect } from 'react';

function useFetch(url, options = {}) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let mounted = true;

    async function fetchData() {
      try {
        setLoading(true);
        const response = await fetch(url, options);
        
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const result = await response.json();
        
        if (mounted) {
          setData(result);
          setError(null);
        }
      } catch (err) {
        if (mounted) {
          setError(err.message);
        }
      } finally {
        if (mounted) {
          setLoading(false);
        }
      }
    }

    fetchData();

    return () => {
      mounted = false;
    };
  }, [url]); // Re-executa quando URL mudar

  return { data, loading, error };
}

// hooks/useInterval.js
import { useEffect, useRef } from 'react';

function useInterval(callback, delay) {
  const savedCallback = useRef();

  // Salvar callback mais recente
  useEffect(() => {
    savedCallback.current = callback;
  }, [callback]);

  // Configurar intervalo
  useEffect(() => {
    function tick() {
      savedCallback.current();
    }
    
    if (delay !== null) {
      const id = setInterval(tick, delay);
      return () => clearInterval(id);
    }
  }, [delay]);
}

// Uso dos Custom Hooks
function App() {
  // useLocalStorage
  const [nome, setNome] = useLocalStorage('nome', 'Visitante');
  
  // useFetch
  const { data: usuario, loading, error } = useFetch('/api/usuario/1');
  
  // useInterval
  const [contador, setContador] = useState(0);
  useInterval(() => {
    setContador(c => c + 1);
  }, 1000);

  return (
    <div>
      <p>Nome salvo: {nome}</p>
      <p>Contador: {contador}</p>
      {loading && <p>Carregando...</p>}
      {error && <p>Erro: {error}</p>}
      {usuario && <p>Usuário: {usuario.nome}</p>}
    </div>
  );
}
```

---

## Performance

### React.memo

```jsx
import { memo } from 'react';

// Componente otimizado
const ItemLista = memo(function ItemLista({ item, onRemover }) {
  console.log('ItemLista renderizou:', item.id);
  
  return (
    <li>
      {item.nome}
      <button onClick={() => onRemover(item.id)}>Remover</button>
    </li>
  );
}, (prevProps, nextProps) => {
  // Função de comparação personalizada
  return prevProps.item.id === nextProps.item.id &&
         prevProps.onRemover === nextProps.onRemover;
});
```

### useMemo para Cálculos Caros

```jsx
function ListaCalculada({ dados, filtro }) {
  // Cálculo caro - memoizado
  const resultado = useMemo(() => {
    console.log('Calculando resultado...');
    return dados.filter(item => {
      // Cálculo complexo
      return item.nome.includes(filtro);
    }).map(item => ({
      ...item,
      pontuacao: calcularPontuacao(item)
    }));
  }, [dados, filtro]);

  return (
    <ul>
      {resultado.map(item => (
        <li key={item.id}>{item.nome} - {item.pontuacao}</li>
      ))}
    </ul>
  );
}
```

### Code Splitting e Lazy Loading

```jsx
import { lazy, Suspense } from 'react';

// Lazy loading de componentes
const Galeria = lazy(() => import('./components/Galeria'));
const Dashboard = lazy(() => import('./pages/Dashboard'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Carregando...</div>}>
        <Routes>
          <Route path="/galeria" element={<Galeria />} />
          <Route path="/dashboard" element={<Dashboard />} />
        </Routes>
      </Suspense>
    </div>
  );
}
```

### Virtualização de Listas

```jsx
// Usando react-window para listas grandes
import { FixedSizeList } from 'react-window';

function ListaGrande({ itens }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      Item {index}: {itens[index]}
    </div>
  );

  return (
    <FixedSizeList
      height={400}
      width={300}
      itemCount={itens.length}
      itemSize={35}
    >
      {Row}
    </FixedSizeList>
  );
}
```

### Profiler API

```jsx
import { Profiler } from 'react';

function App() {
  const handleRender = (
    id, // "App"
    phase, // "mount" ou "update"
    actualDuration, // tempo gasto renderizando
    baseDuration, // tempo estimado sem otimizações
    startTime, // quando React começou renderizar
    commitTime, // quando React commitou
    interactions // Set de interações
  ) => {
    console.log({
      id,
      phase,
      actualDuration,
      baseDuration,
      startTime,
      commitTime,
      interactions
    });
  };

  return (
    <Profiler id="App" onRender={handleRender}>
      <ComponentePesado />
    </Profiler>
  );
}
```

---

## Boas Práticas

### Estrutura de Projeto

```
src/
├── assets/           # Imagens, fonts, etc.
├── components/       # Componentes reutilizáveis
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.test.js
│   │   ├── Button.module.css
│   │   └── index.js
│   └── ...
├── hooks/           # Custom hooks
├── context/         # Context providers
├── services/        # API calls, services
├── utils/           # Funções utilitárias
├── pages/           # Componentes de página
├── layouts/         # Layout components
├── routes/          # Configuração de rotas
├── styles/          # Estilos globais
├── App.jsx
└── main.jsx
```

### Convenções de Nomeação

```jsx
// Componentes: PascalCase
function UserProfile() { }

// Hooks: camelCase com prefixo 'use'
function useFetch() { }

// Event handlers: handle + Event
function handleClick() { }
function onSubmit() { }

// Props: camelCase
<UserProfile userName="João" isActive={true} />

// Boolean props prefixados
<Button isDisabled={true} hasError={false} />
```

### Gerenciamento de Estado

```jsx
// 1. Local quando possível
function ComponenteLocal() {
  const [estado, setEstado] = useState();
}

// 2. Levantar estado para o pai comum mais próximo
function Pai() {
  const [estadoCompartilhado, setEstadoCompartilhado] = useState();
  return (
    <>
      <FilhoA estado={estadoCompartilhado} />
      <FilhoB estado={estadoCompartilhado} />
    </>
  );
}

// 3. Context API para estado global
function App() {
  return (
    <ContextoProvider>
      <Componente />
    </ContextoProvider>
  );
}

// 4. Biblioteca externa para estado complexo
// (Redux, Zustand, MobX)
```

### Testes

```jsx
// Componente.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Componente from './Componente';

describe('Componente', () => {
  test('renderiza corretamente', () => {
    render(<Componente />);
    expect(screen.getByText('Texto esperado')).toBeInTheDocument();
  });

  test('manipula clique', async () => {
    const user = userEvent.setup();
    render(<Componente />);
    
    const button = screen.getByRole('button');
    await user.click(button);
    
    expect(screen.getByText('Clicado!')).toBeInTheDocument();
  });

  test('chama callback com valores corretos', () => {
    const mockCallback = jest.fn();
    render(<Componente onSubmit={mockCallback} />);
    
    fireEvent.submit(screen.getByRole('form'));
    expect(mockCallback).toHaveBeenCalledWith(expectedValues);
  });
});
```

### TypeScript com React

```tsx
// Componente com TypeScript
interface UserCardProps {
  nome: string;
  idade: number;
  email?: string; // Opcional
  onSelect: (id: number) => void;
}

const UserCard: React.FC<UserCardProps> = ({
  nome,
  idade,
  email,
  onSelect
}) => {
  return (
    <div onClick={() => onSelect(1)}>
      <h2>{nome}</h2>
      <p>Idade: {idade}</p>
      {email && <p>Email: {email}</p>}
    </div>
  );
};

// Hook com TypeScript
function useContador(initialValue: number = 0) {
  const [contador, setContador] = useState<number>(initialValue);
  
  const incrementar = useCallback(() => {
    setContador(c => c + 1);
  }, []);
  
  return { contador, incrementar };
}
```

### Error Boundaries

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.error('Erro capturado:', error, errorInfo);
    // Enviar para serviço de logging
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-boundary">
          <h2>Algo deu errado</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Tentar novamente
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// Uso
<ErrorBoundary>
  <ComponenteQuePodeFalhar />
</ErrorBoundary>
```

---

## Projetos Práticos

### 1. Todo List Avançada

```jsx
// hooks/useTodos.js
function useTodos() {
  const [todos, setTodos] = useLocalStorage('todos', []);
  const [filtro, setFiltro] = useState('all');

  const todosFiltrados = useMemo(() => {
    switch (filtro) {
      case 'active':
        return todos.filter(todo => !todo.completed);
      case 'completed':
        return todos.filter(todo => todo.completed);
      default:
        return todos;
    }
  }, [todos, filtro]);

  const addTodo = (text) => {
    setTodos(prev => [...prev, {
      id: Date.now(),
      text,
      completed: false,
      createdAt: new Date().toISOString()
    }]);
  };

  const toggleTodo = (id) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };

  const deleteTodo = (id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  };

  const clearCompleted = () => {
    setTodos(prev => prev.filter(todo => !todo.completed));
  };

  const todosLeft = todos.filter(todo => !todo.completed).length;

  return {
    todos: todosFiltrados,
    addTodo,
    toggleTodo,
    deleteTodo,
    clearCompleted,
    setFiltro,
    todosLeft,
    filtro
  };
}

// components/TodoApp.jsx
function TodoApp() {
  const {
    todos,
    addTodo,
    toggleTodo,
    deleteTodo,
    clearCompleted,
    setFiltro,
    todosLeft,
    filtro
  } = useTodos();

  const [novoTodo, setNovoTodo] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (novoTodo.trim()) {
      addTodo(novoTodo.trim());
      setNovoTodo('');
    }
  };

  return (
    <div className="todo-app">
      <header>
        <h1>TODOs</h1>
        <form onSubmit={handleSubmit}>
          <input
            type="text"
            value={novoTodo}
            onChange={(e) => setNovoTodo(e.target.value)}
            placeholder="O que precisa ser feito?"
            autoFocus
          />
          <button type="submit">Adicionar</button>
        </form>
      </header>

      <section className="todo-list">
        {todos.length === 0 ? (
          <p className="empty">Nenhuma tarefa. Adicione uma!</p>
        ) : (
          <ul>
            {todos.map(todo => (
              <TodoItem
                key={todo.id}
                todo={todo}
                onToggle={toggleTodo}
                onDelete={deleteTodo}
              />
            ))}
          </ul>
        )}
      </section>

      <footer>
        <span>{todosLeft} itens restantes</span>
        <div className="filters">
          {['all', 'active', 'completed'].map(f => (
            <button
              key={f}
              className={filtro === f ? 'active' : ''}
              onClick={() => setFiltro(f)}
            >
              {f.charAt(0).toUpperCase() + f.slice(1)}
            </button>
          ))}
        </div>
        <button onClick={clearCompleted}>Limpar completas</button>
      </footer>
    </div>
  );
}
```

### 2. Dashboard com Gráficos

```jsx
// Dashboard.jsx
function Dashboard() {
  const { data, loading, error } = useFetch('/api/dashboard');
  const [periodo, setPeriodo] = useState('month');
  const [metricasSelecionadas, setMetricasSelecionadas] = useState(['vendas', 'usuarios']);

  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorMessage error={error} />;

  return (
    <div className="dashboard">
      <header className="dashboard-header">
        <h1>Dashboard</h1>
        <PeriodoSelector periodo={periodo} onChange={setPeriodo} />
      </header>

      <div className="dashboard-grid">
        <MetricCard
          titulo="Vendas"
          valor={data.vendas.total}
          variacao={data.vendas.variacao}
          icone="💰"
        />
        <MetricCard
          titulo="Usuários"
          valor={data.usuarios.total}
          variacao={data.usuarios.variacao}
          icone="👥"
        />
        <MetricCard
          titulo="Receita"
          valor={data.receita.total}
          variacao={data.receita.variacao}
          icone="💵"
        />
        <MetricCard
          titulo="Satisfação"
          valor={data.satisfacao.media}
          variacao={data.satisfacao.variacao}
          icone="⭐"
        />
      </div>

      <div className="dashboard-charts">
        <Chart
          dados={data.graficoVendas}
          tipo="line"
          titulo="Vendas ao Longo do Tempo"
        />
        <Chart
          dados={data.graficoCategorias}
          tipo="bar"
          titulo="Vendas por Categoria"
        />
        <Chart
          dados={data.graficoRegioes}
          tipo="pie"
          titulo="Distribuição por Região"
        />
      </div>

      <TabelaRecentes
        pedidos={data.pedidosRecentes}
        onPedidoClick={(id) => navigate(`/pedidos/${id}`)}
      />
    </div>
  );
}
```

### 3. Chat em Tempo Real

```jsx
// hooks/useChat.js
function useChat(salaId) {
  const [mensagens, setMensagens] = useState([]);
  const [usuarios, setUsuarios] = useState([]);
  const [conectado, setConectado] = useState(false);
  const socketRef = useRef();

  useEffect(() => {
    // Conectar ao WebSocket
    socketRef.current = new WebSocket(`wss://api.example.com/chat/${salaId}`);

    socketRef.current.onopen = () => {
      setConectado(true);
      console.log('Conectado ao chat');
    };

    socketRef.current.onmessage = (event) => {
      const mensagem = JSON.parse(event.data);
      
      switch (mensagem.tipo) {
        case 'nova_mensagem':
          setMensagens(prev => [...prev, mensagem.dados]);
          break;
        case 'lista_usuarios':
          setUsuarios(mensagem.dados);
          break;
        case 'usuario_entrou':
          setUsuarios(prev => [...prev, mensagem.usuario]);
          break;
        case 'usuario_saiu':
          setUsuarios(prev => prev.filter(u => u.id !== mensagem.usuarioId));
          break;
      }
    };

    socketRef.current.onclose = () => {
      setConectado(false);
      console.log('Desconectado do chat');
    };

    return () => {
      socketRef.current?.close();
    };
  }, [salaId]);

  const enviarMensagem = (texto, usuario) => {
    if (socketRef.current?.readyState === WebSocket.OPEN) {
      const mensagem = {
        tipo: 'enviar_mensagem',
        dados: {
          texto,
          usuario,
          timestamp: Date.now()
        }
      };
      socketRef.current.send(JSON.stringify(mensagem));
    }
  };

  return {
    mensagens,
    usuarios,
    conectado,
    enviarMensagem
  };
}

// ChatApp.jsx
function ChatApp() {
  const { salaId } = useParams();
  const { usuario } = useAuth();
  const [novaMensagem, setNovaMensagem] = useState('');
  const chatContainerRef = useRef();
  
  const {
    mensagens,
    usuarios,
    conectado,
    enviarMensagem
  } = useChat(salaId);

  // Scroll automático para última mensagem
  useEffect(() => {
    if (chatContainerRef.current) {
      chatContainerRef.current.scrollTop = 
        chatContainerRef.current.scrollHeight;
    }
  }, [mensagens]);

  const handleEnviar = (e) => {
    e.preventDefault();
    if (novaMensagem.trim() && usuario) {
      enviarMensagem(novaMensagem.trim(), usuario);
      setNovaMensagem('');
    }
  };

  return (
    <div className="chat-app">
      <aside className="chat-sidebar">
        <h3>Usuários Online ({usuarios.length})</h3>
        <ul>
          {usuarios.map(user => (
            <li key={user.id}>
              <span className={`status ${user.online ? 'online' : 'offline'}`} />
              {user.nome}
            </li>
          ))}
        </ul>
        <div className="connection-status">
          Status: {conectado ? '🟢 Conectado' : '🔴 Desconectado'}
        </div>
      </aside>

      <main className="chat-main">
        <div className="chat-messages" ref={chatContainerRef}>
          {mensagens.map(msg => (
            <Mensagem
              key={msg.id}
              mensagem={msg}
              isOwn={msg.usuario.id === usuario?.id}
            />
          ))}
        </div>

        <form className="chat-input" onSubmit={handleEnviar}>
          <input
            type="text"
            value={novaMensagem}
            onChange={(e) => setNovaMensagem(e.target.value)}
            placeholder="Digite uma mensagem..."
            disabled={!conectado}
          />
          <button type="submit" disabled={!conectado}>
            Enviar
          </button>
        </form>
      </main>
    </div>
  );
}
```

---

## Ecossistema React

### Ferramentas de Build
- **Vite**: Build tool rápida
- **Webpack**: Mais configuração, mais poder
- **Parcel**: Zero configuration

### Gerenciamento de Estado
- **Redux Toolkit**: Padrão para gerenciamento de estado
- **Zustand**: Leve e simples
- **MobX**: Orientado a objetos
- **Recoil**: Desenvolvido pelo Facebook

### Roteamento
- **React Router**: Padrão do ecossistema
- **Wouter**: Alternativa leve

### UI Frameworks
- **Material-UI**: Componentes Google Material Design
- **Ant Design**: Framework empresarial
- **Chakra UI**: Acessível e customizável
- **Tailwind CSS**: Utility-first CSS

### Testes
- **Jest**: Framework de testes
- **React Testing Library**: Testes de componentes
- **Cypress**: Testes end-to-end

### Formulários
- **Formik**: Popular e completo
- **React Hook Form**: Performance otimizada
- **Final Form**: Extensível

### Internacionalização
- **react-i18next**: Popular para i18n
- **FormatJS**: Suíte completa

### Animações
- **Framer Motion**: Mais popular
- **React Spring**: Baseado em física
- **React Transition Group**: Transições CSS

### Data Fetching
- **React Query**: Cache e sincronização
- **SWR**: Stale-while-revalidate
- **Apollo Client**: Para GraphQL

### Mobile
- **React Native**: Apps nativos
- **Expo**: Plataforma React Native

### SSR/SSG
- **Next.js**: Framework completo
- **Gatsby**: Para sites estáticos
- **Remix**: Focado em web standards

---

## Recursos para Aprender Mais

### Documentação Oficial
- [React Docs](https://reactjs.org/docs/getting-started.html)
- [React Beta Docs](https://beta.reactjs.org/) (Novo)
- [Create React App](https://create-react-app.dev/)

### Cursos
- [React Official Tutorial](https://reactjs.org/tutorial/tutorial.html)
- [Epic React](https://epicreact.dev/) (Pago)
- [FreeCodeCamp React Course](https://www.freecodecamp.org/learn/front-end-development-libraries/react/)

### Comunidades
- [Reactiflux Discord](https://www.reactiflux.com/)
- [Stack Overflow - React](https://stackoverflow.com/questions/tagged/reactjs)
- [DEV Community - React](https://dev.to/t/react)

### Blogs e Newsletters
- [React Status](https://react.statuscode.com/)
- [This Week In React](https://thisweekinreact.com/)
- [Overreacted](https://overreacted.io/) (Dan Abramov)

### Ferramentas
- [React DevTools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
- [CodeSandbox](https://codesandbox.io/)
- [StackBlitz](https://stackblitz.com/)

---

## Conclusão

React é uma biblioteca poderosa e flexível que continua a evoluir. A chave para dominar React é:

1. **Entender os fundamentos**: Componentes, props, estado
2. **Praticar hooks**: useState, useEffect, useContext
3. **Aprender patterns**: Composição, lifting state up
4. **Explorar o ecossistema**: Router, state management, testing
5. **Construir projetos**: Aplicar conhecimento em projetos reais

Lembre-se: React é apenas uma ferramenta. O importante é resolver problemas reais e criar experiências incríveis para os usuários.

**Próximos passos recomendados:**
1. Dominar TypeScript com React
2. Aprender Next.js para SSR/SSG
3. Explorar React Native para mobile
4. Estudar padrões avançados (render props, HOCs, compound components)
5. Contribuir para projetos open source

Boa jornada no aprendizado de React! 🚀
