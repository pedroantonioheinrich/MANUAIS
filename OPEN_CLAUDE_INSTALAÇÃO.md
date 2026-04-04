
```bash
#!/bin/bash

# ============================================
# INSTALAÇÃO COMPLETA: OLLAMA + OPENCLAUDE
# ============================================

set -e  # Para o script em caso de erro

echo "=========================================="
echo "🚀 INSTALANDO OLLAMA + OPENCLAUDE"
echo "=========================================="
echo ""

# Cores para output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# ============================================
# 1. INSTALAR OLLAMA
# ============================================
echo -e "${YELLOW}[1/6] Instalando Ollama...${NC}"

# Verificar se já está instalado
if command -v ollama &> /dev/null; then
    echo -e "${GREEN}✅ Ollama já está instalado${NC}"
else
    curl -fsSL https://ollama.com/install.sh | sh
    echo -e "${GREEN}✅ Ollama instalado com sucesso${NC}"
fi

# ============================================
# 2. BAIXAR MODELOS RECOMENDADOS
# ============================================
echo -e "${YELLOW}[2/6] Baixando modelos...${NC}"

# Modelos para 32GB RAM (recomendados)
MODELOS=(
    "llama3.2:3b"      # Leve e rápido, suporta tools
    "llama3.2:7b"      # Médio, melhor qualidade
    "qwen2.5:7b"       # Ótimo para código, suporta tools
    "nomic-embed-text" # Para embeddings (RAG)
)

for modelo in "${MODELOS[@]}"; do
    echo "📦 Baixando $modelo..."
    ollama pull "$modelo"
    echo -e "${GREEN}✅ $modelo baixado${NC}"
done

# ============================================
# 3. INSTALAR NODE.JS (se não tiver)
# ============================================
echo -e "${YELLOW}[3/6] Verificando Node.js...${NC}"

if command -v node &> /dev/null; then
    echo -e "${GREEN}✅ Node.js já instalado: $(node --version)${NC}"
else
    echo "📦 Instalando Node.js..."
    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt-get install -y nodejs
    echo -e "${GREEN}✅ Node.js instalado: $(node --version)${NC}"
fi

# ============================================
# 4. INSTALAR OPENCLAUDE
# ============================================
echo -e "${YELLOW}[4/6] Instalando OpenClaude...${NC}"

# Desinstalar versão anterior se existir
npm uninstall -g @gitlawb/openclaude 2>/dev/null || true

# Instalar versão estável
npm install -g @gitlawb/openclaude

echo -e "${GREEN}✅ OpenClaude instalado${NC}"

# ============================================
# 5. CONFIGURAR AMBIENTE
# ============================================
echo -e "${YELLOW}[5/6] Configurando ambiente...${NC}"

# Criar arquivo de configuração
cat > ~/.openclaude_config.sh << 'EOF'
# Configuração do OpenClaude
export CLAUDE_CODE_USE_OPENAI=1
export OPENAI_BASE_URL=http://localhost:11434/v1
export OPENAI_MODEL=llama3.2:7b  # Pode mudar para qwen2.5:7b
export ANTHROPIC_API_KEY=""
EOF

# Adicionar ao .bashrc se não existir
if ! grep -q "openclaude_config" ~/.bashrc; then
    echo "source ~/.openclaude_config.sh" >> ~/.bashrc
fi

source ~/.openclaude_config.sh

echo -e "${GREEN}✅ Configuração salva${NC}"

# ============================================
# 6. CRIAR MEMÓRIA PERMANENTE (opcional)
# ============================================
echo -e "${YELLOW}[6/6] Criando memória permanente...${NC}"

mkdir -p ~/ia_memoria/{preferencias,projetos,aprendizados}

cat > ~/ia_memoria/preferencias/usuario.txt << 'EOF'
# MINHAS PREFERÊNCIAS
- Linguagem: Python
- Framework: FastAPI / Django
- Banco de dados: PostgreSQL
- Sempre usar type hints
- Prefiro async/await
- Documentação em português
EOF

cat > ~/carregar_memoria.sh << 'EOF'
#!/bin/bash
echo "🧠 CARREGANDO MEMÓRIA PERMANENTE"
echo "================================="
cat ~/ia_memoria/preferencias/*.txt 2>/dev/null
echo "================================="
echo "✅ Memória carregada!"
EOF

chmod +x ~/carregar_memoria.sh

# ============================================
# 7. CRIAR ALIAS E COMANDOS RÁPIDOS
# ============================================
echo -e "${YELLOW}Criando aliases...${NC}"

cat >> ~/.bashrc << 'EOF'

# Aliases para OpenClaude
alias oc='openclaude'
alias ocm='openclaude && @bash "~/carregar_memoria.sh"'
alias oc7='export OPENAI_MODEL=llama3.2:7b && openclaude'
alias oc3='export OPENAI_MODEL=llama3.2:3b && openclaude'
alias ocq='export OPENAI_MODEL=qwen2.5:7b && openclaude'

# Gerenciamento de modelos
alias ollama-list='ollama list'
alias ollama-ps='ollama ps'
EOF

source ~/.bashrc

# ============================================
# 8. INICIAR SERVIÇOS
# ============================================
echo -e "${YELLOW}Iniciando serviços...${NC}"

# Iniciar Ollama como serviço
sudo systemctl enable ollama
sudo systemctl restart ollama

# Aguardar Ollama iniciar
sleep 5

# Verificar se está rodando
if curl -s http://localhost:11434/api/tags > /dev/null; then
    echo -e "${GREEN}✅ Ollama está rodando${NC}"
else
    echo -e "${RED}❌ Ollama não iniciou corretamente${NC}"
    echo "Execute manualmente: ollama serve"
fi

# ============================================
# 9. TESTE DE VERIFICAÇÃO
# ============================================
echo ""
echo "=========================================="
echo -e "${GREEN}✅ INSTALAÇÃO CONCLUÍDA!${NC}"
echo "=========================================="
echo ""
echo "📊 COMANDOS ÚTEIS:"
echo "  oc          - Iniciar OpenClaude"
echo "  oc3         - Usar modelo 3B (mais rápido)"
echo "  oc7         - Usar modelo 7B (melhor qualidade)"
echo "  ocq         - Usar Qwen 7B (ótimo para código)"
echo "  ocm         - Iniciar com memória permanente"
echo ""
echo "📦 MODELOS INSTALADOS:"
ollama list
echo ""
echo "🧠 MEMÓRIA PERMANENTE: ~/ia_memoria/"
echo ""
echo "🚀 PRÓXIMOS PASSOS:"
echo "  1. Reinicie o terminal ou execute: source ~/.bashrc"
echo "  2. Digite 'oc' para iniciar o OpenClaude"
echo "  3. Teste: '2+2'"
echo "  4. Teste com memória: @bash '~/carregar_memoria.sh'"
echo ""
echo "=========================================="
```

## 📝 **Como usar o script:**

### **1. Salvar o script:**
```bash
nano ~/install_ollama_openclaude.sh
```

### **2. Colar o conteúdo acima e salvar (Ctrl+X, Y, Enter)**

### **3. Tornar executável:**
```bash
chmod +x ~/install_ollama_openclaude.sh
```

### **4. Executar:**
```bash
./install_ollama_openclaude.sh
```

## 🚀 **Após a instalação, comandos rápidos:**

```bash
# Reiniciar o terminal ou:
source ~/.bashrc

# Iniciar OpenClaude (modo padrão - 7B)
oc

# Usar modelo mais rápido (3B)
oc3

# Usar Qwen (melhor para código)
ocq

# Iniciar com memória permanente
ocm
```

## ⚡ **Script de teste rápido após instalação:**

```bash
#!/bin/bash
# test_install.sh

echo "🧪 TESTANDO INSTALAÇÃO"

echo ""
echo "1. Verificando Ollama..."
ollama list

echo ""
echo "2. Testando API..."
curl -s http://localhost:11434/api/tags | head -c 200

echo ""
echo "3. Testando modelo 3B..."
ollama run llama3.2:3b "Diga 'OK'" 2>/dev/null

echo ""
echo "4. Testando OpenClaude..."
echo "2+2" | timeout 30 openclaude --clear || echo "Timeout - pode ser normal na primeira vez"

echo ""
echo "✅ Testes concluídos!"
```

## 📊 **Modelos recomendados para 32GB RAM:**

| Modelo | Uso | Comando |
|--------|-----|---------|
| llama3.2:3b | Rápido, tarefas simples | `oc3` |
| **llama3.2:7b** | **Melhor geral** | `oc` (padrão) |
| qwen2.5:7b | Código e programação | `ocq` |
| llama3.3:70b | Tarefas complexas (64GB+) | `ollama pull llama3.3:70b` |

## 🎯 **Primeiros testes após instalação:**

```bash
# 1. Abrir OpenClaude
oc

# 2. Teste básico
"2+2"

# 3. Teste de código
"Crie uma função em Python que calcula o fatorial"

# 4. Teste de arquivo (deve criar o arquivo!)
"Crie um arquivo chamado hello.py com 'print('Hello World')'"

# 5. Teste de execução
@bash "python3 hello.py"

# 6. Carregar memória
@bash "~/carregar_memoria.sh"
```
