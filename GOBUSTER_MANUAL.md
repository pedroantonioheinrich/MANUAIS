# Manual Completo do GoBuster

## Índice
1. [Visão Geral](#visão-geral)
2. [Instalação](#instalação)
3. [Modos de Operação](#modos-de-operação)
4. [Uso Básico](#uso-básico)
5. [Opções Principais](#opções-principais)
6. [Exemplos de Uso](#exemplos-de-uso)
7. [Wordlists Recomendadas](#wordlists-recomendadas)
8. [Dicas e Boas Práticas](#dicas-e-boas-práticas)
9. [Solução de Problemas](#solução-de-problemas)
10. [Alternativas](#alternativas)

---

## Visão Geral

**Gobuster** é uma ferramenta de brute-force escrita em Go, utilizada principalmente para:
- **Diretórios e arquivos** em servidores web (modo `dir`)
- **Subdomínios** em domínios (modo `dns`)
- **Nomes de buckets S3** (modo `s3`)
- **Nomes de usuários** em aplicações web (modo `vhost`)

É uma ferramenta essencial para testes de penetração e auditorias de segurança.

## Instalação

### Linux/Unix/macOS
```bash
# Via pacote (Kali Linux)
sudo apt install gobuster

# Via Go
go install github.com/OJ/gobuster/v3@latest

# Via git clone
git clone https://github.com/OJ/gobuster.git
cd gobuster
go build
sudo mv gobuster /usr/local/bin/
```

### Windows
```bash
# Via Chocolatey
choco install gobuster

# Via Scoop
scoop install gobuster

# Via Go
go install github.com/OJ/gobuster/v3@latest
```

### Docker
```bash
docker pull ghcr.io/oj/gobuster:latest
docker run --rm ghcr.io/oj/gobuster:latest --help
```

## Modos de Operação

### 1. Modo DIR (Diretórios/Arquivos)
```bash
gobuster dir -u http://alvo.com -w wordlist.txt
```

### 2. Modo DNS (Subdomínios)
```bash
gobuster dns -d alvo.com -w wordlist.txt
```

### 3. Modo VHOST (Virtual Hosts)
```bash
gobuster vhost -u http://alvo.com -w wordlist.txt
```

### 4. Modo S3 (Buckets AWS)
```bash
gobuster s3 -w wordlist.txt
```

## Opções Principais

### Opções Comuns a Todos os Modos

| Flag | Descrição | Padrão |
|------|-----------|---------|
| `-h, --help` | Ajuda do modo específico | - |
| `-t, --threads` | Número de threads concorrentes | 10 |
| `-v, --verbose` | Modo verboso | false |
| `-q, --quiet` | Modo silencioso (apenas resultados) | false |
| `-o, --output` | Arquivo de saída | - |
| `-s, --status` | Códigos de status para mostrar (vírgula) | - |
| `-b, --status-blacklist` | Códigos para ignorar (vírgula) | - |
| `-c, --cookies` | Cookies para requisições | - |
| `-H, --headers` | Headers HTTP (array) | - |
| `-x, --extensions` | Extensões para testar (vírgula) | - |
| `-k, --no-tls-validation` | Desabilitar validação TLS | false |
| `-n, --no-progress` | Não mostrar barra de progresso | false |
| `-z, --no-color` | Desabilitar cores na saída | false |

### Opções Específicas do Modo DIR

| Flag | Descrição | Padrão |
|------|-----------|---------|
| `-u, --url` | URL alvo | - |
| `-w, --wordlist` | Caminho da wordlist | - |
| `-l, --include-length` | Incluir tamanho do corpo | false |
| `-r, --follow-redirect` | Seguir redirecionamentos | false |
| `-f, --add-slash` | Adicionar barra no final | false |
| `-P, --password` | Senha para Basic Auth | - |
| `-U, --username` | Usuário para Basic Auth | - |
| `-p, --proxy` | Proxy para usar (http://ip:porta) | - |
| `-d, --discover-backup` | Descobrir arquivos de backup | false |
| `--timeout` | Timeout HTTP | 10s |
| `--useragent` | User-Agent personalizado | GoBuster |
| `--random-agent` | User-Agent aleatório | false |
| `--retry` | Número de tentativas em caso de erro | 0 |
| `--retry-on` | Códigos para retentar (vírgula) | - |
| `--wildcard` | Forçar detecção de wildcard | false |

### Opções Específicas do Modo DNS

| Flag | Descrição | Padrão |
|------|-----------|---------|
| `-d, --domain` | Domínio alvo | - |
| `-w, --wordlist` | Caminho da wordlist | - |
| `-r, --resolver` | Servidores DNS (vírgula) | Sistema |
| `-c, --show-cname` | Mostrar registros CNAME | false |
| `-i, --show-ips` | Mostrar endereços IP | false |
| `--timeout` | Timeout DNS | 1s |

### Opções Específicas do Modo VHOST

| Flag | Descrição | Padrão |
|------|-----------|---------|
| `-u, --url` | URL alvo | - |
| `-w, --wordlist` | Caminho da wordlist | - |
| `-r, --follow-redirect` | Seguir redirecionamentos | false |

### Opções Específicas do Modo S3

| Flag | Descrição | Padrão |
|------|-----------|---------|
| `-w, --wordlist` | Caminho da wordlist | - |
| `-r, --region` | Região AWS (ex: us-east-1) | us-east-1 |

## Exemplos de Uso

### 1. Varredura Básica de Diretórios
```bash
# Scan básico
gobuster dir -u http://alvo.com -w /usr/share/wordlists/dirb/common.txt

# Com extensões específicas
gobuster dir -u http://alvo.com -w wordlist.txt -x php,html,js

# Com filtro de status code
gobuster dir -u http://alvo.com -w wordlist.txt -s 200,204,301,302,307,403

# Ignorando status codes específicos
gobuster dir -u http://alvo.com -w wordlist.txt -b 404,500

# Com autenticação básica
gobuster dir -u http://alvo.com -w wordlist.txt -U admin -P senha123

# Com headers personalizados
gobuster dir -u http://alvo.com -w wordlist.txt -H "X-API-Key: valor" -H "User-Agent: MeuScanner"
```

### 2. Scan de Subdomínios
```bash
# Scan básico de DNS
gobuster dns -d alvo.com -w /usr/share/wordlists/dns/subdomains-top1million-5000.txt

# Com resolução de IPs
gobuster dns -d alvo.com -w wordlist.txt -i

# Com servidores DNS específicos
gobuster dns -d alvo.com -w wordlist.txt -r 8.8.8.8,1.1.1.1

# Mostrando registros CNAME
gobuster dns -d alvo.com -w wordlist.txt -c
```

### 3. Discovery de VHosts
```bash
gobuster vhost -u http://alvo.com -w /usr/share/wordlists/dirb/big.txt
```

### 4. Busca de Buckets S3
```bash
gobuster s3 -w /usr/share/wordlists/S3.txt

# Com região específica
gobuster s3 -w wordlist.txt -r sa-east-1
```

### 5. Exemplos Avançados
```bash
# Scan completo com múltiplas extensões
gobuster dir -u http://alvo.com \
  -w /usr/share/wordlists/dirb/big.txt \
  -x php,html,js,txt,json,xml \
  -t 50 \
  -v \
  -o resultado.txt

# Scan com proxy e timeout aumentado
gobuster dir -u https://alvo.com \
  -w wordlist.txt \
  -p http://127.0.0.1:8080 \
  --timeout 30s \
  -k

# Scan recursivo (com script)
for dir in $(gobuster dir -u http://alvo.com -w wordlist.txt -q); do
  gobuster dir -u "http://alvo.com/$dir" -w wordlist.txt -q
done
```

## Wordlists Recomendadas

### Diretórios/Arquivos
```bash
# Diretórios padrão do Kali
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirb/big.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# Wordlists do SecLists
/usr/share/seclists/Discovery/Web-Content/common.txt
/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt

# Wordlists personalizadas
rockyou.txt (convertido para diretórios)
```

### Subdomínios
```bash
# Kali Linux
/usr/share/wordlists/dns/subdomains-top1million-5000.txt
/usr/share/wordlists/dns/subdomains-top1million-20000.txt

# SecLists
/usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt

# Wordlists especializadas
assetnote.io - wordlists atualizadas
```

### S3 Buckets
```bash
# Wordlists comuns
/usr/share/wordlists/S3.txt

# Gerar wordlist baseada no alvo
echo "empresa-dev" >> buckets.txt
echo "empresa-prod" >> buckets.txt
echo "empresa-backup" >> buckets.txt
```

## Dicas e Boas Práticas

### 1. Otimização de Performance
```bash
# Ajuste de threads (cuidado com overload)
-t 50  # Para conexões rápidas
-t 20  # Para conexões lentas/limitadas

# Use timeout apropriado
--timeout 15s  # Para redes lentas
--timeout 5s   # Para redes rápidas
```

### 2. Evitando Detecção
```bash
# User-Agent aleatório
--random-agent

# Throttle manual com delay
# (Use com script externo ou modifique o código)

# Use proxies rotativos
-p http://proxy1:porta
# (Implemente rotação com script)
```

### 3. Filtragem Inteligente
```bash
# Combine blacklist e whitelist
-s 200,301,302 -b 404,500

# Verifique tamanhos de resposta para filtrar falsos positivos
-l  # Mostra tamanhos para análise manual
```

### 4. Output e Análise
```bash
# Salve em formato específico
-o resultado.txt
# ou
-o resultado.json  # Se suportado

# Combine com outras ferramentas
gobuster dir -u http://alvo.com -w wordlist.txt -q | tee resultado.txt
```

### 5. Scripts Úteis

**Scan Recursivo:**
```bash
#!/bin/bash
URL=$1
WORDLIST=$2

scan() {
    local url=$1
    local depth=$2
    
    if [ $depth -gt 3 ]; then
        return
    fi
    
    echo "[*] Scanning: $url"
    gobuster dir -u "$url" -w "$WORDLIST" -q | while read result; do
        echo "$url/$result"
        scan "$url/$result" $((depth + 1))
    done
}

scan "$URL" 1
```

**Multi-extensão com Saída Organizada:**
```bash
#!/bin/bash
extensions=("php" "html" "js" "txt" "json" "xml" "asp" "aspx" "jsp")

for ext in "${extensions[@]}"; do
    echo "[*] Testing .$ext files"
    gobuster dir -u "$1" -w "$2" -x "$ext" -q | sed "s/$/.$ext/"
done
```

## Solução de Problemas

### Problemas Comuns

1. **"Connection refused" ou timeouts**
   ```bash
   # Verifique conectividade
   curl -I http://alvo.com
   
   # Aumente timeout
   --timeout 30s
   
   # Desabilite validação TLS se necessário
   -k
   ```

2. **Muitos falsos positivos (wildcard DNS)**
   ```bash
   # Detecte wildcard primeiro
   dig random123.alvo.com
   
   # Force detecção de wildcard
   --wildcard
   ```

3. **Rate limiting ou bloqueio**
   ```bash
   # Reduza threads
   -t 5
   
   # Adicione delay (se suportado)
   # Considere usar ferramentas com rate limiting nativo
   ```

4. **Saída confusa ou formatada incorretamente**
   ```bash
   # Desabilite cores
   -z
   
   # Use modo quiet para parsing
   -q
   ```

### Debugging
```bash
# Verbose máximo
-v

# Capture tráfego com proxy
-p http://127.0.0.1:8080
# E analise com Burp Suite ou ZAP
```

## Alternativas ao GoBuster

| Ferramenta | Linguagem | Vantagens |
|-----------|-----------|-----------|
| **Dirb** | C | Mais antigo, estável |
| **Dirbuster** | Java | Interface gráfica, recursivo |
| **FFuF** | Go | Mais rápido, mais features |
| **Wfuzz** | Python | Extremamente flexível |
| **Assetnote** | Online | Wordlists atualizadas |
| **Amass** | Go | Focado em reconhecimento |

## Referências e Recursos

- **Repositório Oficial**: https://github.com/OJ/gobuster
- **Documentação**: https://github.com/OJ/gobuster/wiki
- **Wordlists**: https://github.com/danielmiessler/SecLists
- **Tutorials**: https://www.hackingarticles.in/comprehensive-guide-on-gobuster-tool/

---

**Nota**: Use o GoBuster apenas em sistemas que você possui permissão explícita para testar. Varreduras não autorizadas são ilegais e antiéticas.

**Última atualização**: Novembro 2023  
**Versão do GoBuster**: v3.5.0  
**Autor**: OJ Reeves  
**Licença**: Apache 2.0
