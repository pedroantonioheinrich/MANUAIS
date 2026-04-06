## 📚 **Manual Completo: React + Vite + TypeScript (TSX)**

---

## 1. **Configuração Inicial**

### Criando o projeto
```bash
# Criar projeto com Vite + React + TypeScript
npm create vite@latest meu-projeto -- --template react-ts

# Ou com yarn
yarn create vite meu-projeto --template react-ts

# Instalar dependências
cd meu-projeto
npm install

# Iniciar desenvolvimento
npm run dev
```

### Estrutura de pastas recomendada
```
src/
├── components/     # Componentes reutilizáveis
├── pages/         # Páginas da aplicação
├── hooks/         # Custom hooks
├── services/      # API e serviços
├── types/         # Definições de tipos
├── utils/         # Funções utilitárias
├── styles/        # Arquivos CSS/SCSS
├── App.tsx        # Componente principal
├── main.tsx       # Entry point
└── vite-env.d.ts  # Tipos do Vite
```

---

## 2. **Tipos e Interfaces**

### Definindo tipos básicos
```tsx
// types/User.ts
export interface User {
  id: number;
  name: string;
  email: string;
  age?: number; // Opcional
  role: 'admin' | 'user' | 'guest';
}

export type Status = 'loading' | 'success' | 'error';

export interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}
```

---

## 3. **Componentes Funcionais**

### Componente básico
```tsx
// components/Greeting.tsx
import React from 'react';

interface GreetingProps {
  name: string;
  age?: number;
  onGreet?: (message: string) => void;
}

const Greeting: React.FC<GreetingProps> = ({ name, age, onGreet }) => {
  const handleClick = () => {
    if (onGreet) {
      onGreet(`Olá ${name}!`);
    }
  };

  return (
    <div>
      <h1>Olá, {name}!</h1>
      {age && <p>Idade: {age}</p>}
      <button onClick={handleClick}>Saudar</button>
    </div>
  );
};

export default Greeting;
```

### Componente com children
```tsx
// components/Card.tsx
interface CardProps {
  title: string;
  children: React.ReactNode;
  variant?: 'primary' | 'secondary';
}

const Card: React.FC<CardProps> = ({ title, children, variant = 'primary' }) => {
  return (
    <div className={`card card--${variant}`}>
      <h2>{title}</h2>
      <div className="card-content">{children}</div>
    </div>
  );
};
```

---

## 4. **Hooks com TypeScript**

### useState
```tsx
import { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState<number>(0);
  const [user, setUser] = useState<User | null>(null);
  const [status, setStatus] = useState<Status>('loading');

  return (
    <div>
      <p>Contador: {count}</p>
      <button onClick={() => setCount(count + 1)}>Incrementar</button>
    </div>
  );
};
```

### useEffect
```tsx
import { useEffect, useState } from 'react';

const UserList = () => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState<boolean>(true);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const response = await api.get<User[]>('/users');
        setUsers(response.data);
      } catch (error) {
        console.error('Erro ao carregar usuários:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []); // Array de dependências vazio

  if (loading) return <div>Carregando...</div>;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};
```

### useReducer
```tsx
import { useReducer } from 'react';

interface State {
  count: number;
  step: number;
}

type Action = 
  | { type: 'INCREMENT' }
  | { type: 'DECREMENT' }
  | { type: 'SET_STEP'; payload: number };

const initialState: State = { count: 0, step: 1 };

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + state.step };
    case 'DECREMENT':
      return { ...state, count: state.count - state.step };
    case 'SET_STEP':
      return { ...state, step: action.payload };
    default:
      return state;
  }
};

const CounterWithReducer = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>+</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-</button>
    </div>
  );
};
```

### useRef
```tsx
import { useRef, useEffect } from 'react';

const InputFocus = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    inputRef.current?.focus();
  }, []);

  return <input ref={inputRef} type="text" placeholder="Auto-focus" />;
};
```

### Custom Hook
```tsx
// hooks/useLocalStorage.ts
import { useState, useEffect } from 'react';

function useLocalStorage<T>(key: string, initialValue: T): [T, (value: T) => void] {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });

  const setValue = (value: T) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue];
}

// Uso
const App = () => {
  const [theme, setTheme] = useLocalStorage<'light' | 'dark'>('theme', 'light');
  
  return (
    <div className={`app theme-${theme}`}>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Trocar tema
      </button>
    </div>
  );
};
```

---

## 5. **Eventos e Formulários**

### Eventos tipados
```tsx
import { ChangeEvent, FormEvent, MouseEvent } from 'react';

const FormExample = () => {
  const [inputValue, setInputValue] = useState<string>('');
  const [formData, setFormData] = useState({ email: '', password: '' });

  // Input change
  const handleInputChange = (e: ChangeEvent<HTMLInputElement>) => {
    setInputValue(e.target.value);
  };

  // Form submit
  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
  };

  // Button click
  const handleClick = (e: MouseEvent<HTMLButtonElement>) => {
    e.preventDefault();
    console.log('Button clicked');
  };

  // Input com nome dinâmico
  const handleFormChange = (e: ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={inputValue}
        onChange={handleInputChange}
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleFormChange}
      />
      <input
        type="password"
        name="password"
        value={formData.password}
        onChange={handleFormChange}
      />
      <button type="submit" onClick={handleClick}>
        Enviar
      </button>
    </form>
  );
};
```

---

## 6. **Context API**

### Criando e usando Context
```tsx
// contexts/AuthContext.tsx
import React, { createContext, useContext, useState, ReactNode } from 'react';

interface AuthContextType {
  user: User | null;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

interface AuthProviderProps {
  children: ReactNode;
}

export const AuthProvider: React.FC<AuthProviderProps> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);

  const login = async (email: string, password: string) => {
    // Lógica de login
    const loggedUser = { id: 1, name: 'João', email };
    setUser(loggedUser);
  };

  const logout = () => {
    setUser(null);
  };

  const value = {
    user,
    login,
    logout,
    isAuthenticated: !!user
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
};

// Hook customizado
export const useAuth = () => {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};

// Uso no componente
const Profile = () => {
  const { user, logout } = useAuth();
  
  return (
    <div>
      <p>Bem-vindo, {user?.name}!</p>
      <button onClick={logout}>Sair</button>
    </div>
  );
};
```

---

## 7. **Requisições HTTP**

### Configurando Axios
```tsx
// services/api.ts
import axios, { AxiosInstance, AxiosResponse, AxiosError } from 'axios';

const api: AxiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Interceptors
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error: AxiosError) => Promise.reject(error)
);

api.interceptors.response.use(
  (response: AxiosResponse) => response,
  (error: AxiosError) => {
    if (error.response?.status === 401) {
      // Redirecionar para login
    }
    return Promise.reject(error);
  }
);

// Services tipados
export const userService = {
  getAll: () => api.get<User[]>('/users'),
  getById: (id: number) => api.get<User>(`/users/${id}`),
  create: (user: Omit<User, 'id'>) => api.post<User>('/users', user),
  update: (id: number, user: Partial<User>) => api.put<User>(`/users/${id}`, user),
  delete: (id: number) => api.delete(`/users/${id}`),
};

export default api;
```

### Hook para requisições
```tsx
// hooks/useFetch.ts
import { useState, useEffect } from 'react';
import api from '../services/api';

interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: string | null;
  refetch: () => void;
}

function useFetch<T>(url: string): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  const fetchData = async () => {
    try {
      setLoading(true);
      const response = await api.get<T>(url);
      setData(response.data);
      setError(null);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Erro desconhecido');
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, [url]);

  return { data, loading, error, refetch: fetchData };
}

// Uso
const UserPage = () => {
  const { data: users, loading, error, refetch } = useFetch<User[]>('/users');

  if (loading) return <div>Carregando...</div>;
  if (error) return <div>Erro: {error}</div>;

  return (
    <div>
      <button onClick={refetch}>Recarregar</button>
      {users?.map(user => <div key={user.id}>{user.name}</div>)}
    </div>
  );
};
```

---

## 8. **Rotas com React Router**

### Configuração de rotas
```tsx
// main.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

### Definindo rotas
```tsx
// App.tsx
import { Routes, Route, Navigate } from 'react-router-dom';
import { Home } from './pages/Home';
import { About } from './pages/About';
import { UserProfile } from './pages/UserProfile';
import { PrivateRoute } from './components/PrivateRoute';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      <Route path="/user/:id" element={<UserProfile />} />
      <Route path="/dashboard" element={
        <PrivateRoute>
          <Dashboard />
        </PrivateRoute>
      } />
      <Route path="*" element={<Navigate to="/" replace />} />
    </Routes>
  );
};

// Componente com parâmetros
const UserProfile = () => {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();
  const location = useLocation();

  useEffect(() => {
    if (!id) {
      navigate('/');
    }
  }, [id, navigate]);

  return <div>Perfil do usuário: {id}</div>;
};

// Rota privada
interface PrivateRouteProps {
  children: React.ReactNode;
}

const PrivateRoute: React.FC<PrivateRouteProps> = ({ children }) => {
  const { isAuthenticated } = useAuth();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  return <>{children}</>;
};
```

---

## 9. **Renderização Condicional e Listas**

### Renderização condicional
```tsx
const ConditionalRender = ({ status }: { status: Status }) => {
  // Opção 1: if
  if (status === 'loading') return <Spinner />;
  
  // Opção 2: operador ternário
  return (
    <div>
      {status === 'success' ? (
        <SuccessMessage />
      ) : (
        <ErrorMessage />
      )}
    </div>
  );
};

// Opção 3: && operator
const Notification = ({ message }: { message?: string }) => {
  return (
    <div>
      {message && <div className="alert">{message}</div>}
      {!message && <div>Sem mensagens</div>}
    </div>
  );
};
```

### Renderização de listas
```tsx
interface ItemListProps {
  items: User[];
  onItemClick?: (item: User) => void;
}

const ItemList: React.FC<ItemListProps> = ({ items, onItemClick }) => {
  return (
    <ul>
      {items.map((item) => (
        <li 
          key={item.id}
          onClick={() => onItemClick?.(item)}
          className="cursor-pointer hover:bg-gray-100"
        >
          {item.name} - {item.email}
        </li>
      ))}
    </ul>
  );
};
```

---

## 10. **Props e Children Avançados**

### Props com defaults e validação
```tsx
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'danger';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  onClick?: () => void;
  children: React.ReactNode;
}

const Button: React.FC<ButtonProps> = ({ 
  variant = 'primary',
  size = 'md',
  disabled = false,
  onClick,
  children 
}) => {
  const className = `btn btn-${variant} btn-${size} ${disabled ? 'btn-disabled' : ''}`;
  
  return (
    <button className={className} disabled={disabled} onClick={onClick}>
      {children}
    </button>
  );
};
```

### Render Props e Children como função
```tsx
interface ListProps<T> {
  items: T[];
  renderItem: (item: T, index: number) => React.ReactNode;
  emptyMessage?: React.ReactNode;
}

function List<T>({ items, renderItem, emptyMessage = 'Lista vazia' }: ListProps<T>) {
  if (items.length === 0) {
    return <div>{emptyMessage}</div>;
  }
  
  return <>{items.map((item, index) => renderItem(item, index))}</>;
}

// Uso
const UserList = () => {
  const users: User[] = [
    { id: 1, name: 'João', email: 'joao@email.com', role: 'user' },
    { id: 2, name: 'Maria', email: 'maria@email.com', role: 'admin' }
  ];

  return (
    <List 
      items={users}
      renderItem={(user, index) => (
        <div key={user.id}>
          {index + 1}. {user.name} - {user.email}
        </div>
      )}
      emptyMessage="Nenhum usuário encontrado"
    />
  );
};
```

---

## 11. **Estilização com CSS Modules**

### Configurando CSS Modules
```tsx
// components/Button.module.css
.button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.primary {
  background-color: #007bff;
  color: white;
}

.secondary {
  background-color: #6c757d;
  color: white;
}

// components/Button.tsx
import styles from './Button.module.css';

interface ButtonProps {
  variant?: 'primary' | 'secondary';
  children: React.ReactNode;
}

const Button: React.FC<ButtonProps> = ({ variant = 'primary', children }) => {
  return (
    <button className={`${styles.button} ${styles[variant]}`}>
      {children}
    </button>
  );
};
```

### Styled Components com TypeScript
```tsx
import styled from 'styled-components';

interface ButtonProps {
  $primary?: boolean;
  $size?: 'small' | 'medium' | 'large';
}

const StyledButton = styled.button<ButtonProps>`
  background: ${props => props.$primary ? '#007bff' : 'white'};
  color: ${props => props.$primary ? 'white' : '#007bff'};
  font-size: ${props => {
    switch(props.$size) {
      case 'small': return '12px';
      case 'large': return '18px';
      default: return '14px';
    }
  }};
  padding: 10px 20px;
  border: 1px solid #007bff;
  border-radius: 4px;
  cursor: pointer;
  
  &:hover {
    opacity: 0.9;
  }
`;

const Button: React.FC<ButtonProps> = ({ children, ...props }) => {
  return <StyledButton {...props}>{children}</StyledButton>;
};
```

---

## 12. **Configuração do Vite**

### vite.config.ts
```tsx
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@utils': path.resolve(__dirname, './src/utils'),
      '@hooks': path.resolve(__dirname, './src/hooks'),
      '@services': path.resolve(__dirname, './src/services'),
      '@types': path.resolve(__dirname, './src/types'),
    }
  },
  server: {
    port: 3000,
    open: true,
    proxy: {
      '/api': {
        target: 'http://localhost:5000',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, '')
      }
    }
  },
  build: {
    outDir: 'dist',
    sourcemap: true,
    minify: 'terser',
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom', 'react-router-dom'],
        }
      }
    }
  },
  define: {
    'process.env': process.env
  }
});
```

### Variáveis de ambiente
```bash
# .env.development
VITE_API_URL=http://localhost:5000/api
VITE_APP_NAME=Meu App Dev

# .env.production
VITE_API_URL=https://api.meuapp.com/api
VITE_APP_NAME=Meu App Prod
```

```tsx
// Usando variáveis de ambiente
const apiUrl = import.meta.env.VITE_API_URL;
const appName = import.meta.env.VITE_APP_NAME;

console.log(`Executando ${appName} com API em ${apiUrl}`);
```

---

## 13. **Tipos Utilitários**

### Tipos comuns
```tsx
// Partial - Torna todas as propriedades opcionais
type PartialUser = Partial<User>;

// Required - Torna todas as propriedades obrigatórias
type RequiredUser = Required<User>;

// Pick - Seleciona propriedades específicas
type UserBasicInfo = Pick<User, 'id' | 'name'>;

// Omit - Remove propriedades específicas
type UserWithoutId = Omit<User, 'id'>;

// Record - Cria um tipo com chaves e valores
type UserMap = Record<string, User>;

// Exclude - Exclui tipos de uma união
type NonAdminRole = Exclude<User['role'], 'admin'>; // 'user' | 'guest'

// ReturnType - Obtém o tipo de retorno de uma função
function getUser(): User {
  return { id: 1, name: 'João', email: 'joao@email.com', role: 'user' };
}
type UserReturnType = ReturnType<typeof getUser>;

// ComponentProps - Obtém as props de um componente
type ButtonProps = ComponentProps<typeof Button>;
```

---

## 14. **Boas Práticas e Padrões**

### Organização de imports
```tsx
// 1. React e bibliotecas externas
import React, { useState, useEffect } from 'react';
import { useNavigate } from 'react-router-dom';

// 2. Componentes internos
import { Header } from '@components/Header';
import { Footer } from '@components/Footer';

// 3. Hooks
import { useAuth } from '@hooks/useAuth';

// 4. Tipos
import type { User } from '@types/User';

// 5. Utilitários
import { formatDate } from '@utils/date';

// 6. Estilos
import styles from './Page.module.css';
```

### Padrão de exportação
```tsx
// components/index.ts - Barrel file
export { Button } from './Button';
export { Card } from './Card';
export { Modal } from './Modal';

// Uso
import { Button, Card, Modal } from '@components';
```

### Tipagem de eventos
```tsx
const EventHandlers = () => {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {};
  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {};
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {};
  const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {};
  const handleDragStart = (e: React.DragEvent<HTMLDivElement>) => {};
};
```

---

## 15. **Exemplo Prático Completo**

### Todo App com todas as funcionalidades
```tsx
// types/Todo.ts
export interface Todo {
  id: number;
  text: string;
  completed: boolean;
  createdAt: Date;
}

export type TodoFilter = 'all' | 'active' | 'completed';

// hooks/useTodos.ts
import { useState, useEffect } from 'react';
import type { Todo, TodoFilter } from '@types/Todo';

export const useTodos = () => {
  const [todos, setTodos] = useState<Todo[]>(() => {
    const saved = localStorage.getItem('todos');
    return saved ? JSON.parse(saved) : [];
  });
  const [filter, setFilter] = useState<TodoFilter>('all');

  useEffect(() => {
    localStorage.setItem('todos', JSON.stringify(todos));
  }, [todos]);

  const addTodo = (text: string) => {
    const newTodo: Todo = {
      id: Date.now(),
      text,
      completed: false,
      createdAt: new Date(),
    };
    setTodos([...todos, newTodo]);
  };

  const toggleTodo = (id: number) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };

  const deleteTodo = (id: number) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });

  const stats = {
    total: todos.length,
    active: todos.filter(t => !t.completed).length,
    completed: todos.filter(t => t.completed).length,
  };

  return {
    todos: filteredTodos,
    stats,
    filter,
    setFilter,
    addTodo,
    toggleTodo,
    deleteTodo,
  };
};

// components/TodoItem.tsx
import React from 'react';
import type { Todo } from '@types/Todo';

interface TodoItemProps {
  todo: Todo;
  onToggle: (id: number) => void;
  onDelete: (id: number) => void;
}

const TodoItem: React.FC<TodoItemProps> = ({ todo, onToggle, onDelete }) => {
  return (
    <div className="todo-item">
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}>
        {todo.text}
      </span>
      <button onClick={() => onDelete(todo.id)}>Excluir</button>
    </div>
  );
};

// components/TodoForm.tsx
import React, { useState, FormEvent } from 'react';

interface TodoFormProps {
  onAdd: (text: string) => void;
}

const TodoForm: React.FC<TodoFormProps> = ({ onAdd }) => {
  const [text, setText] = useState('');

  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    if (text.trim()) {
      onAdd(text);
      setText('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Adicionar nova tarefa..."
      />
      <button type="submit">Adicionar</button>
    </form>
  );
};

// App.tsx - Aplicação completa
import React from 'react';
import { TodoForm } from '@components/TodoForm';
import { TodoItem } from '@components/TodoItem';
import { TodoStats } from '@components/TodoStats';
import { TodoFilter as FilterComponent } from '@components/TodoFilter';
import { useTodos } from '@hooks/useTodos';
import type { TodoFilter } from '@types/Todo';
import './App.css';

const App: React.FC = () => {
  const {
    todos,
    stats,
    filter,
    setFilter,
    addTodo,
    toggleTodo,
    deleteTodo,
  } = useTodos();

  return (
    <div className="app">
      <h1>Lista de Tarefas</h1>
      
      <TodoForm onAdd={addTodo} />
      
      <TodoStats stats={stats} />
      
      <FilterComponent 
        currentFilter={filter} 
        onFilterChange={setFilter} 
      />
      
      <div className="todo-list">
        {todos.map(todo => (
          <TodoItem
            key={todo.id}
            todo={todo}
            onToggle={toggleTodo}
            onDelete={deleteTodo}
          />
        ))}
      </div>
      
      {todos.length === 0 && (
        <p className="empty-message">
          Nenhuma tarefa encontrada
        </p>
      )}
    </div>
  );
};

export default App;
```

