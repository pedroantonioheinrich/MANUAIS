Aqui está um manual completo em formato Markdown (.md) sobre as funções dos headers `<sys/socket.h>` e `<arpa/inet.h>` em C:

```markdown
# Manual Completo de Sockets em C

## Introdução aos Sockets

Sockets são uma interface de programação (API) que permite a comunicação entre processos, seja na mesma máquina ou através de uma rede. Eles são a base para programação de redes em sistemas Unix/Linux.

## 1. Header <sys/socket.h>

Este header contém as definições fundamentais para criação e manipulação de sockets.

### 1.1 socket()

**Prototype:**
```c
int socket(int domain, int type, int protocol);
```

**Descrição:**
Cria um novo socket e retorna um descritor de arquivo (file descriptor) para ele.

**Parâmetros:**
- `domain`: Especifica o domínio de comunicação
  - `AF_INET` - Protocolo IPv4
  - `AF_INET6` - Protocolo IPv6
  - `AF_UNIX` - Comunicação local (Unix domain)
  - `AF_PACKET` - Acesso direto à camada de rede

- `type`: Tipo do socket
  - `SOCK_STREAM` - Socket orientado à conexão (TCP)
  - `SOCK_DGRAM` - Socket sem conexão (UDP)
  - `SOCK_RAW` - Acesso direto aos protocolos IP/ICMP
  - `SOCK_SEQPACKET` - Socket sequencial com datagramas

- `protocol`: Protocolo específico (geralmente 0 para usar o padrão do tipo)
  - `IPPROTO_TCP` - Protocolo TCP
  - `IPPROTO_UDP` - Protocolo UDP
  - `IPPROTO_ICMP` - Protocolo ICMP

**Retorno:**
- Sucesso: Descritor do socket (inteiro positivo)
- Erro: -1 (errno é definido)

**Exemplo:**
```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
if (sockfd < 0) {
    perror("Erro ao criar socket");
    exit(1);
}
```

### 1.2 bind()

**Prototype:**
```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

**Descrição:**
Associa um socket a um endereço e porta específicos.

**Parâmetros:**
- `sockfd`: Descritor do socket
- `addr`: Ponteiro para estrutura sockaddr com endereço e porta
- `addrlen`: Tamanho da estrutura addr

**Retorno:**
- Sucesso: 0
- Erro: -1

**Exemplo:**
```c
struct sockaddr_in server_addr;
server_addr.sin_family = AF_INET;
server_addr.sin_port = htons(8080);
server_addr.sin_addr.s_addr = INADDR_ANY;

if (bind(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
    perror("Erro no bind");
    exit(1);
}
```

### 1.3 listen()

**Prototype:**
```c
int listen(int sockfd, int backlog);
```

**Descrição:**
Coloca o socket em modo passivo (servidor), aguardando conexões.

**Parâmetros:**
- `sockfd`: Descritor do socket
- `backlog`: Número máximo de conexões pendentes na fila

**Retorno:**
- Sucesso: 0
- Erro: -1

**Exemplo:**
```c
if (listen(sockfd, 5) < 0) {
    perror("Erro no listen");
    exit(1);
}
printf("Aguardando conexões na porta 8080...\n");
```

### 1.4 accept()

**Prototype:**
```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

**Descrição:**
Aceita uma conexão pendente no socket de escuta.

**Parâmetros:**
- `sockfd`: Descritor do socket em modo listen
- `addr`: Ponteiro para estrutura que receberá o endereço do cliente
- `addrlen`: Tamanho da estrutura (valor-resultado)

**Retorno:**
- Sucesso: Novo descritor para comunicação com o cliente
- Erro: -1

**Exemplo:**
```c
struct sockaddr_in client_addr;
socklen_t client_len = sizeof(client_addr);

int new_sockfd = accept(sockfd, (struct sockaddr *)&client_addr, &client_len);
if (new_sockfd < 0) {
    perror("Erro no accept");
    exit(1);
}
```

### 1.5 connect()

**Prototype:**
```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

**Descrição:**
Estabelece uma conexão com um servidor (usado em clientes).

**Parâmetros:**
- `sockfd`: Descritor do socket
- `addr`: Ponteiro para estrutura com endereço do servidor
- `addrlen`: Tamanho da estrutura

**Retorno:**
- Sucesso: 0
- Erro: -1

**Exemplo:**
```c
struct sockaddr_in server_addr;
server_addr.sin_family = AF_INET;
server_addr.sin_port = htons(8080);
inet_pton(AF_INET, "127.0.0.1", &server_addr.sin_addr);

if (connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
    perror("Erro no connect");
    exit(1);
}
```

### 1.6 send() e recv()

**Prototypes:**
```c
ssize_t send(int sockfd, const void *buf, size_t len, int flags);
ssize_t recv(int sockfd, void *buf, size_t len, int flags);
```

**Descrição:**
Envia e recebe dados através de sockets conectados.

**Parâmetros flags comuns:**
- `0`: Comportamento padrão
- `MSG_DONTWAIT`: Operação não-bloqueante
- `MSG_PEEK`: Espia dados sem removê-los do buffer
- `MSG_WAITALL`: Espera receber len bytes completos

**Retorno:**
- Sucesso: Número de bytes enviados/recebidos
- 0: Conexão fechada (recv)
- -1: Erro

**Exemplo:**
```c
char buffer[1024];
strcpy(buffer, "Olá servidor!");

// Enviando dados
int bytes_sent = send(sockfd, buffer, strlen(buffer), 0);
if (bytes_sent < 0) {
    perror("Erro no send");
}

// Recebendo dados
int bytes_recv = recv(sockfd, buffer, sizeof(buffer) - 1, 0);
if (bytes_recv > 0) {
    buffer[bytes_recv] = '\0';
    printf("Recebido: %s\n", buffer);
}
```

### 1.7 sendto() e recvfrom()

**Prototypes:**
```c
ssize_t sendto(int sockfd, const void *buf, size_t len, int flags,
               const struct sockaddr *dest_addr, socklen_t addrlen);

ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags,
                 struct sockaddr *src_addr, socklen_t *addrlen);
```

**Descrição:**
Envia e recebe dados em sockets sem conexão (UDP).

**Exemplo:**
```c
struct sockaddr_in dest_addr;
dest_addr.sin_family = AF_INET;
dest_addr.sin_port = htons(8080);
inet_pton(AF_INET, "192.168.1.100", &dest_addr.sin_addr);

char message[] = "Mensagem UDP";
sendto(sockfd, message, strlen(message), 0,
       (struct sockaddr *)&dest_addr, sizeof(dest_addr));

// Recebendo
struct sockaddr_in src_addr;
socklen_t src_len = sizeof(src_addr);
char buffer[1024];
int n = recvfrom(sockfd, buffer, sizeof(buffer) - 1, 0,
                 (struct sockaddr *)&src_addr, &src_len);
```

### 1.8 close()

**Prototype:**
```c
int close(int fd);
```

**Descrição:**
Fecha um socket, liberando o descritor de arquivo.

**Exemplo:**
```c
close(sockfd);
```

### 1.9 getsockopt() e setsockopt()

**Prototypes:**
```c
int getsockopt(int sockfd, int level, int optname, void *optval, socklen_t *optlen);
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
```

**Descrição:**
Consulta e configura opções do socket.

**Níveis comuns:**
- `SOL_SOCKET`: Opções de socket
- `IPPROTO_TCP`: Opções TCP
- `IPPROTO_IP`: Opções IP

**Opções comuns:**
- `SO_REUSEADDR`: Permite reutilizar endereço/porta
- `SO_RCVBUF`: Tamanho do buffer de recebimento
- `SO_SNDBUF`: Tamanho do buffer de envio
- `SO_KEEPALIVE`: Mantém conexão ativa
- `TCP_NODELAY`: Desativa algoritmo de Nagle

**Exemplo:**
```c
int reuse = 1;
if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &reuse, sizeof(reuse)) < 0) {
    perror("Erro no setsockopt");
}

int buffer_size;
socklen_t size = sizeof(buffer_size);
getsockopt(sockfd, SOL_SOCKET, SO_RCVBUF, &buffer_size, &size);
printf("Tamanho do buffer de recebimento: %d\n", buffer_size);
```

## 2. Header <arpa/inet.h>

Este header contém funções para manipulação de endereços IP.

### 2.1 inet_pton()

**Prototype:**
```c
int inet_pton(int af, const char *src, void *dst);
```

**Descrição:**
Converte string de endereço IP para formato binário.

**Parâmetros:**
- `af`: Família de endereços (AF_INET ou AF_INET6)
- `src`: String com o endereço IP
- `dst`: Ponteiro para struct in_addr ou in6_addr

**Retorno:**
- 1: Sucesso
- 0: Endereço inválido
- -1: Família de endereços inválida

**Exemplo:**
```c
struct sockaddr_in addr;
addr.sin_family = AF_INET;

if (inet_pton(AF_INET, "192.168.1.1", &addr.sin_addr) != 1) {
    fprintf(stderr, "Endereço IP inválido\n");
}
```

### 2.2 inet_ntop()

**Prototype:**
```c
const char *inet_ntop(int af, const void *src, char *dst, socklen_t size);
```

**Descrição:**
Converte endereço IP binário para string.

**Parâmetros:**
- `af`: Família de endereços
- `src`: Ponteiro para struct in_addr ou in6_addr
- `dst`: Buffer para a string resultante
- `size`: Tamanho do buffer

**Retorno:**
- Sucesso: Ponteiro para dst
- Erro: NULL

**Exemplo:**
```c
struct sockaddr_in client_addr;
char ip_str[INET_ADDRSTRLEN];

inet_ntop(AF_INET, &client_addr.sin_addr, ip_str, sizeof(ip_str));
printf("Endereço do cliente: %s\n", ip_str);
```

### 2.3 htons(), htonl(), ntohs(), ntohl()

**Prototypes:**
```c
uint16_t htons(uint16_t hostshort);
uint32_t htonl(uint32_t hostlong);
uint16_t ntohs(uint16_t netshort);
uint32_t ntohl(uint32_t netlong);
```

**Descrição:**
Convertem valores entre ordem de bytes do host e da rede (network byte order).

**Explicação:**
- `htons`: Host TO Network Short (16 bits)
- `htonl`: Host TO Network Long (32 bits)
- `ntohs`: Network TO Host Short
- `ntohl`: Network TO Host Long

**Exemplo:**
```c
struct sockaddr_in addr;
addr.sin_port = htons(8080);  // Converte porta para network byte order
addr.sin_addr.s_addr = htonl(INADDR_ANY);  // Converte endereço

// Lendo porta recebida
uint16_t port = ntohs(addr.sin_port);
```

### 2.4 inet_addr() e inet_aton() (Legado)

**Prototypes:**
```c
in_addr_t inet_addr(const char *cp);
int inet_aton(const char *cp, struct in_addr *inp);
```

**Descrição:**
Funções mais antigas para conversão de endereços IP (prefira inet_pton).

**Exemplo:**
```c
struct sockaddr_in addr;
addr.sin_addr.s_addr = inet_addr("192.168.1.1");  // Retorna INADDR_NONE em erro

// Alternativa melhor
if (inet_aton("192.168.1.1", &addr.sin_addr) == 0) {
    fprintf(stderr, "Endereço inválido\n");
}
```

## 3. Estruturas de Dados Importantes

### 3.1 struct sockaddr
```c
struct sockaddr {
    sa_family_t sa_family;    // Família de endereços
    char sa_data[14];         // Dados específicos do protocolo
};
```

### 3.2 struct sockaddr_in (IPv4)
```c
struct sockaddr_in {
    short            sin_family;   // AF_INET
    unsigned short   sin_port;     // Porta (network byte order)
    struct in_addr   sin_addr;     // Endereço IP
    char             sin_zero[8];  // Preenchimento
};
```

### 3.3 struct in_addr
```c
struct in_addr {
    unsigned long s_addr;  // Endereço IP (network byte order)
};
```

### 3.4 struct sockaddr_in6 (IPv6)
```c
struct sockaddr_in6 {
    sa_family_t     sin6_family;   // AF_INET6
    in_port_t       sin6_port;     // Porta
    uint32_t        sin6_flowinfo; // Informações de fluxo
    struct in6_addr sin6_addr;     // Endereço IPv6
    uint32_t        sin6_scope_id; // ID do escopo
};
```

## 4. Exemplos Completos

### 4.1 Servidor TCP Completo
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int server_fd, client_fd;
    struct sockaddr_in address;
    int opt = 1;
    int addrlen = sizeof(address);
    char buffer[BUFFER_SIZE] = {0};
    
    // Criando socket
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }
    
    // Configurando opção SO_REUSEADDR
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt))) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }
    
    // Configurando endereço
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // Bind
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    // Listen
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }
    
    printf("Servidor aguardando conexões na porta %d...\n", PORT);
    
    // Accept
    if ((client_fd = accept(server_fd, (struct sockaddr *)&address, 
                            (socklen_t*)&addrlen)) < 0) {
        perror("accept");
        exit(EXIT_FAILURE);
    }
    
    // Recebendo dados
    int valread = read(client_fd, buffer, BUFFER_SIZE);
    printf("Recebido: %s\n", buffer);
    
    // Enviando resposta
    char *response = "Mensagem recebida com sucesso!";
    send(client_fd, response, strlen(response), 0);
    
    // Fechando conexões
    close(client_fd);
    close(server_fd);
    
    return 0;
}
```

### 4.2 Cliente TCP Completo
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int sock = 0;
    struct sockaddr_in serv_addr;
    char buffer[BUFFER_SIZE] = {0};
    
    // Criando socket
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        printf("Erro ao criar socket\n");
        return -1;
    }
    
    // Configurando endereço do servidor
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(PORT);
    
    // Convertendo endereço IP
    if (inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr) <= 0) {
        printf("Endereço inválido\n");
        return -1;
    }
    
    // Conectando ao servidor
    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
        printf("Falha na conexão\n");
        return -1;
    }
    
    // Enviando mensagem
    char *message = "Olá servidor!";
    send(sock, message, strlen(message), 0);
    printf("Mensagem enviada\n");
    
    // Recebendo resposta
    int valread = read(sock, buffer, BUFFER_SIZE);
    printf("Resposta: %s\n", buffer);
    
    close(sock);
    return 0;
}
```

### 4.3 Servidor UDP Completo
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int sockfd;
    char buffer[BUFFER_SIZE];
    struct sockaddr_in servaddr, cliaddr;
    
    // Criando socket UDP
    if ((sockfd = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {
        perror("socket creation failed");
        exit(EXIT_FAILURE);
    }
    
    // Configurando endereço
    memset(&servaddr, 0, sizeof(servaddr));
    memset(&cliaddr, 0, sizeof(cliaddr));
    
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = INADDR_ANY;
    servaddr.sin_port = htons(PORT);
    
    // Bind
    if (bind(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    printf("Servidor UDP aguardando mensagens na porta %d...\n", PORT);
    
    int len = sizeof(cliaddr);
    int n = recvfrom(sockfd, buffer, BUFFER_SIZE, 0,
                     (struct sockaddr *)&cliaddr, (socklen_t *)&len);
    
    buffer[n] = '\0';
    
    char client_ip[INET_ADDRSTRLEN];
    inet_ntop(AF_INET, &cliaddr.sin_addr, client_ip, INET_ADDRSTRLEN);
    
    printf("Cliente %s:%d enviou: %s\n", 
           client_ip, ntohs(cliaddr.sin_port), buffer);
    
    // Enviando resposta
    char *response = "Mensagem UDP recebida!";
    sendto(sockfd, response, strlen(response), 0,
           (struct sockaddr *)&cliaddr, len);
    
    close(sockfd);
    return 0;
}
```

## 5. Tratamento de Erros

### 5.1 Variável errno
```c
#include <errno.h>
#include <string.h>

if (socket(AF_INET, SOCK_STREAM, 0) < 0) {
    printf("Erro %d: %s\n", errno, strerror(errno));
}

// Usando perror (mais simples)
if (bind(sockfd, ...) < 0) {
    perror("Erro no bind");
}
```

### 5.2 Códigos de erro comuns
- `EACCES`: Permissão negada
- `EADDRINUSE`: Endereço/porta já em uso
- `ECONNREFUSED`: Conexão recusada
- `ETIMEDOUT`: Tempo de conexão esgotado
- `ENETUNREACH`: Rede inalcançável
- `EHOSTUNREACH`: Host inalcançável

## 6. Boas Práticas

### 6.1 Verificação de Retornos
Sempre verifique os valores de retorno das funções de socket.

### 6.2 Limpeza de Memória
```c
// Zerar estruturas antes de usar
struct sockaddr_in addr;
memset(&addr, 0, sizeof(addr));
addr.sin_family = AF_INET;
```

### 6.3 Fechamento de Sockets
```c
// Garantir que sockets sejam fechados
int sockfd = socket(...);
if (sockfd >= 0) {
    // ... usar socket
    close(sockfd);  // Fechar quando terminar
}
```

### 6.4 Non-blocking I/O
```c
// Configurar socket como não-bloqueante
int flags = fcntl(sockfd, F_GETFL, 0);
fcntl(sockfd, F_SETFL, flags | O_NONBLOCK);
```

## 7. Dicas e Truques

### 7.1 Descobrindo Porta Atribuída
```c
struct sockaddr_in addr;
socklen_t addrlen = sizeof(addr);
getsockname(sockfd, (struct sockaddr *)&addr, &addrlen);
printf("Porta: %d\n", ntohs(addr.sin_port));
```

### 7.2 Obtendo Informações da Conexão
```c
struct sockaddr_in peer;
socklen_t peerlen = sizeof(peer);
getpeername(sockfd, (struct sockaddr *)&peer, &peerlen);
```

### 7.3 Timeout em Operações
```c
struct timeval timeout;
timeout.tv_sec = 5;  // 5 segundos
timeout.tv_usec = 0;
setsockopt(sockfd, SOL_SOCKET, SO_RCVTIMEO, &timeout, sizeof(timeout));
```

## Conclusão

Este manual cobre as principais funções dos headers `<sys/socket.h>` e `<arpa/inet.h>` em C. A programação com sockets é fundamental para desenvolvimento de aplicações de rede e sistemas distribuídos. Pratique com os exemplos fornecidos e lembre-se sempre de tratar erros adequadamente e liberar recursos ao final da execução.
```

Este manual completo inclui:

1. **Introdução aos conceitos básicos**
2. **Descrição detalhada de cada função** com prototypes, parâmetros e retornos
3. **Exemplos práticos** de código para cada função
4. **Exemplos completos** de servidores e clientes TCP/UDP
5. **Estruturas de dados** importantes
6. **Tratamento de erros** e boas práticas
7. **Dicas e truques** úteis para programação com sockets

O formato Markdown (.md) é ideal para documentação, pois pode ser facilmente lido em qualquer editor de texto ou convertido para outros formatos como HTML ou PDF.
