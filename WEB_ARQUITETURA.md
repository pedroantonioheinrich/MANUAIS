
# Manual Completo de Arquitetura Web

## Sumário
1.  [Fundamentos da Arquitetura Web](#1-fundamentos-da-arquitetura-web)
2.  [Arquitetura Cliente-Servidor](#2-arquitetura-cliente-servidor)
3.  [Protocolos e Comunicação](#3-protocolos-e-comunicação)
4.  [Modelos de Arquitetura](#4-modelos-de-arquitetura)
5.  [Componentes de Backend](#5-componentes-de-backend)
6.  [Bancos de Dados e Armazenamento](#6-bancos-de-dados-e-armazenamento)
7.  [Frontend e Experiência do Usuário](#7-frontend-e-experiência-do-usuário)
8.  [Segurança na Web](#8-segurança-na-web)
9.  [Escalabilidade e Performance](#9-escalabilidade-e-performance)
10. [DevOps, Cloud e Infraestrutura](#10-devops-cloud-e-infraestrutura)
11. [Arquiteturas Modernas](#11-arquiteturas-modernas)
12. [Checklists e Decisões Técnicas](#12-checklists-e-decisões-técnicas)

---

## 1. Fundamentos da Arquitetura Web

A arquitetura web é a disciplina que define a estrutura, o comportamento e a interação entre os componentes de um sistema web. Uma boa arquitetura visa atender aos **atributos de qualidade**:

- **Escalabilidade:** Capacidade de crescer (verticalmente ou horizontalmente) sob aumento de carga.
- **Disponibilidade:** Percentual de tempo que o sistema permanece operacional (ex: 99.9%, 99.99%).
- **Confiabilidade:** Capacidade de o sistema funcionar de forma correta e consistente.
- **Manutenibilidade:** Facilidade para fazer alterações, correções e evoluções.
- **Segurança:** Proteção contra ameaças e acessos não autorizados.

---

## 2. Arquitetura Cliente-Servidor

A base de toda aplicação web é o modelo Cliente-Servidor.

### 2.1. Cliente
- **Responsabilidade:** Interface com o usuário (UI/UX). Coleta inputs e renderiza outputs.
- **Tipos:**
    - **Thin Client:** Navegador tradicional. A maior parte da lógica fica no servidor (ex: aplicações server-side rendering).
    - **Rich Client:** Aplicações de Página Única (SPA - Single Page Application) ou Aplicações Web Progressivas (PWA). Grande parte da lógica e renderização ocorre no lado do cliente (JavaScript).

### 2.2. Servidor
- **Responsabilidade:** Processar lógica de negócio, autenticação, validação, acesso a dados e segurança.
- **Evolução:**
    - **Monolítico:** Tudo em um único deployable.
    - **Microserviços:** Conjunto de serviços pequenos e independentes.

---

## 3. Protocolos e Comunicação

A comunicação na web é padronizada para garantir interoperabilidade.

### 3.1. HTTP/HTTPS (Hypertext Transfer Protocol)
- **Métodos (Verbos):** `GET` (ler), `POST` (criar), `PUT/PATCH` (atualizar), `DELETE` (remover).
- **Códigos de Status:**
    - `1xx`: Informativo.
    - `2xx`: Sucesso (`200 OK`, `201 Created`).
    - `3xx`: Redirecionamento (`301 Moved Permanently`, `304 Not Modified`).
    - `4xx`: Erro do Cliente (`400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`).
    - `5xx`: Erro do Servidor (`500 Internal Server Error`, `502 Bad Gateway`).
- **HTTP/2 e HTTP/3:**
    - **HTTP/2:** Multiplexação (envio de múltiplos arquivos em uma única conexão), compressão de headers.
    - **HTTP/3:** Baseado em **QUIC** (UDP), elimina o *head-of-line blocking* do TCP, reduzindo latência.

### 3.2. APIs (Application Programming Interfaces)
- **REST (Representational State Transfer):** Estilo arquitetural que usa HTTP nativamente. Foco em recursos (substantivos), stateless, cacheable.
- **GraphQL:** Linguagem de consulta que permite ao cliente solicitar exatamente os dados que precisa, evitando *over-fetching* e *under-fetching*.
- **WebSockets:** Protocolo full-duplex para comunicação em tempo real (chats, notificações, jogos).

---

## 4. Modelos de Arquitetura

### 4.1. Monolítica
Uma única base de código e um único processo.
- **Vantagens:** Simplicidade inicial, deploy fácil, baixa latência interna.
- **Desvantagens:** Acoplamento forte, dificuldade em escalar partes específicas, curva de aprendizado alta à medida que cresce.

### 4.2. Microserviços
Divisão do sistema em serviços menores, cada um com seu próprio domínio (Domain-Driven Design - DDD).
- **Vantagens:** Escalabilidade granular, equipes autônomas, resiliência (falha isolada).
- **Desvantagens:** Complexidade operacional (orquestração), latência de rede, consistência eventual (teorema CAP).

### 4.3. Serverless (Funções como Serviço)
O desenvolvedor escreve funções (ex: AWS Lambda) que rodam em resposta a eventos, sem gerenciar servidores.
- **Vantagens:** Escala automática, cobrança por uso, redução de overhead de infra.
- **Desvantagens:** *Cold starts* (latência inicial), timeouts limitados, vendor lock-in.

---

## 5. Componentes de Backend

### 5.1. Load Balancer
Distribui o tráfego entre múltiplos servidores.
- **Algoritmos:** Round Robin, Least Connections, IP Hash (sticky sessions).
- **Ferramentas:** Nginx, HAProxy, AWS ELB/ALB.

### 5.2. Cache
Armazena resultados de operações custosas para acelerar respostas.
- **Camadas:**
    - **CDN (Content Delivery Network):** Cache de assets estáticos (imagens, CSS, JS) na borda (próximo ao usuário). Ex: Cloudflare, Fastly.
    - **Cache de Aplicação:** Redis, Memcached. Usado para sessões, queries de banco de dados e resultados computados.
    - **Cache de Banco de Dados:** Query cache ou buffer pool.

### 5.3. Filas e Mensageria (Assíncrono)
Utilizado para desacoplar serviços e processar tarefas pesadas em segundo plano.
- **Casos de uso:** Envio de e-mails, processamento de vídeos, notificações push.
- **Ferramentas:** RabbitMQ, Apache Kafka, Amazon SQS.

---

## 6. Bancos de Dados e Armazenamento

A escolha do banco de dados é crítica e depende do tipo de dado e consistência necessária.

### 6.1. SQL (Relacionais)
- **Características:** ACID (Atomicidade, Consistência, Isolamento, Durabilidade), esquema rígido, joins complexos.
- **Exemplos:** PostgreSQL, MySQL, Oracle.
- **Quando usar:** Transações financeiras, sistemas que exigem forte consistência.

### 6.2. NoSQL (Não Relacionais)
- **Documentos (MongoDB, Couchbase):** Dados semi-estruturados (JSON). Alta flexibilidade.
- **Chave-Valor (Redis, DynamoDB):** Alta performance para leitura/escrita simples.
- **Colunar (Cassandra, HBase):** Ideal para grandes volumes de dados com alta taxa de escrita.
- **Grafos (Neo4j):** Relacionamentos complexos (redes sociais, recomendações).

### 6.3. Estratégias de Dados
- **Sharding:** Particionamento horizontal dos dados entre diferentes instâncias de banco.
- **Replicação:** Mestre-Escravo (Master-Slave) para alta disponibilidade e distribuição de leitura.
- **CQRS (Command Query Responsibility Segregation):** Separa o modelo de escrita (Command) do modelo de leitura (Query).

---

## 7. Frontend e Experiência do Usuário

### 7.1. Renderização
- **SSR (Server-Side Rendering):** HTML gerado no servidor. Bom para SEO e primeira carga rápida (ex: Next.js, Nuxt).
- **CSR (Client-Side Rendering):** HTML vazio + JS que popula a página. Interações ricas, mas pode sofrer com SEO e *First Paint* lento (ex: React, Vue, Angular puros).
- **SSG (Static Site Generation):** HTML gerado em tempo de build. Extremamente rápido e seguro (ex: Jekyll, Gatsby, Next.js export).

### 7.2. PWA (Progressive Web Apps)
Combina o melhor da web com o de aplicativos nativos:
- Funciona offline (Service Workers).
- Instalável na tela inicial.
- Notificações push.

---

## 8. Segurança na Web

A segurança deve ser aplicada em camadas (Defense in Depth).

### 8.1. Autenticação vs. Autorização
- **Autenticação:** Quem é você?
    - **JWT (JSON Web Tokens):** Tokens assinados, stateless.
    - **OAuth2 / OpenID Connect:** Delegação de acesso (Login com Google, Facebook).
    - **Session Cookies:** Tradicional, stateful, geralmente mais seguro contra XSS.
- **Autorização:** O que você pode fazer? (RBAC - Role Based Access Control, ou ABAC - Attribute Based Access Control).

### 8.2. Principais Vulnerabilidades (OWASP Top 10)
1.  **Injeção (SQL, NoSQL, Command):** Nunca concatene strings de entrada diretamente em queries. Use ORMs ou Prepared Statements.
2.  **XSS (Cross-Site Scripting):** Sanitize outputs. Evite `innerHTML` com dados não confiáveis.
3.  **CSRF (Cross-Site Request Forgery):** Use tokens anti-CSRF ou cookies com atributo `SameSite=Strict`.
4.  **CORS (Cross-Origin Resource Sharing):** Configure rigorosamente quais domínios podem acessar sua API.

### 8.3. Criptografia
- **Em trânsito:** TLS 1.3 (HTTPS) obrigatório. Use HSTS (HTTP Strict Transport Security).
- **Em repouso:** Criptografia de disco (LUKS, EBS encryption) e criptografia de colunas para dados sensíveis (CPF, senhas - usando hashing + salt como bcrypt).

---

## 9. Escalabilidade e Performance

### 9.1. Estratégias de Escala
- **Escala Vertical:** Aumentar CPU/RAM do servidor. Tem limite físico/financeiro.
- **Escala Horizontal:** Adicionar mais instâncias de servidores. Requer arquitetura stateless.

### 9.2. Técnicas de Performance
- **Database Indexing:** Índices bem planejados são o maior ganho de performance.
- **Lazy Loading vs. Eager Loading:** Carregar dados relacionados sob demanda ou antecipadamente para evitar N+1 queries.
- **Compressão:** Gzip ou Brotli para reduzir tamanho do payload.
- **Observabilidade:** Implementar **Three Pillars**:
    - **Logs:** Agregados (ELK Stack - Elasticsearch, Logstash, Kibana).
    - **Métricas:** Prometheus, Grafana (CPU, memória, latência).
    - **Tracing:** Distributed Tracing (Jaeger, Zipkin) para visualizar fluxos entre microserviços.

---

## 10. DevOps, Cloud e Infraestrutura

### 10.1. Containers e Orquestração
- **Docker:** Padroniza o ambiente de desenvolvimento e produção.
- **Kubernetes (K8s):** Orquestrador de contêineres. Gerencia escalabilidade, rollouts, service discovery e auto-healing.

### 10.2. Infraestrutura como Código (IaC)
- **Terraform:** Provisionamento de recursos cloud (VPC, EC2, RDS) de forma declarativa.
- **Ansible / Chef / Puppet:** Configuração de servidores.

### 10.3. CI/CD (Integração e Entrega Contínua)
- **Pipeline:** Build -> Test (unitário, integração) -> Security Scan -> Deploy.
- **Estratégias de Deploy:** Blue/Green (zero downtime), Canary (lançamento gradual), Rollback automatizado.

---

## 11. Arquiteturas Modernas

### 11.1. Hexagonal Architecture (Ports & Adapters)
Isola a lógica de negócio (core) das tecnologias externas (banco de dados, APIs, UI). Aumenta a testabilidade e flexibilidade.

### 11.2. Event-Driven Architecture (EDA)
Os componentes se comunicam através de eventos assíncronos.
- **Componentes:** Producers, Brokers (Kafka), Consumers.
- **Vantagem:** Alta desacoplagem e resiliência.

### 11.3. Service Mesh
Camada de infraestrutura dedicada a gerenciar comunicação entre microserviços (ex: Istio, Linkerd). Oferece mTLS (segurança), retries, circuit breakers e observabilidade sem alterar o código da aplicação.

---

## 12. Checklists e Decisões Técnicas

### Checklist para Definição de Arquitetura:
1.  **Requisitos Funcionais:** O que o sistema precisa fazer?
2.  **Requisitos Não Funcionais:**
    - Qual a expectativa de usuários simultâneos? (Concorrência)
    - Qual a latência aceitável? (P95, P99)
    - Qual o SLA (Service Level Agreement)?
    - Qual o orçamento de infraestrutura?
3.  **Time:** O time tem expertise em Kubernetes ou é mais produtivo com Serverless?
4.  **Dados:** Os dados são fortemente transacionais (SQL) ou não estruturados (NoSQL)?

### Armadilhas Comuns a Evitar:
- **Over-engineering:** Usar microserviços para um MVP ou aplicação pequena.
- **Database as a bottleneck:** Não planejar cache e indexação desde o início.
- **Ignorar Segurança:** Deixar CORS aberto (`*`), não usar HTTPS ou armazenar senhas em texto puro.
- **Single Point of Failure:** Ter apenas uma instância de banco de dados ou load balancer.

---

Este manual serve como um guia de referência. A arquitetura web não é estática; ela deve evoluir conforme as necessidades do negócio, a maturidade do time e o crescimento da base de usuários. O princípio fundamental é sempre buscar o equilíbrio entre **simplicidade**, **flexibilidade** e **resiliência**.
