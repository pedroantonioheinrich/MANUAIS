# Manual Completo sobre RSA em Python

## Índice
1. [Introdução ao RSA](#introdução-ao-rsa)
2. [Fundamentos Matemáticos](#fundamentos-matemáticos)
3. [Geração de Chaves](#geração-de-chaves)
4. [Criptografia e Descriptografia](#criptografia-e-descriptografia)
5. [Assinaturas Digitais](#assinaturas-digitais)
6. [Implementação em Python (Passo a Passo)](#implementação-em-python-passo-a-passo)
7. [Esquemas de Preenchimento (Padding)](#esquemas-de-preenchimento-padding)
8. [Considerações de Segurança](#considerações-de-segurança)
9. [Exemplo Completo com PyCryptodome](#exemplo-completo-com-pycryptodome)
10. [Conclusão](#conclusão)

---

## 1. Introdução ao RSA

O **RSA** (Rivest–Shamir–Adleman) é um dos primeiros e mais amplamente utilizados sistemas de criptografia de chave pública. Ele permite:
- **Criptografia** de mensagens (confidencialidade)
- **Assinaturas digitais** (autenticidade e integridade)

O funcionamento baseia-se na dificuldade de fatorar números inteiros grandes (o produto de dois números primos grandes). A segurança do RSA depende da escolha adequada dos parâmetros e do uso de esquemas de preenchimento seguros.

Neste manual, exploraremos cada fluxo do algoritmo, desde os conceitos matemáticos até a implementação prática em Python, incluindo as melhores práticas de segurança.

---

## 2. Fundamentos Matemáticos

### 2.1 Números Primos
Números primos são aqueles divisíveis apenas por 1 e por eles mesmos. No RSA, usamos dois primos gigantescos, geralmente denotados por **p** e **q**.

### 2.2 Aritmética Modular
Trabalhamos com operações módulo **n**. Dizemos que `a ≡ b (mod n)` se `n` divide `(a - b)`. As operações de exponenciação, como `c = m^e mod n`, são a base do RSA.

### 2.3 Função Totiente de Euler (φ)
Para um número `n = p * q` (com p e q primos), a função totiente é:
```
φ(n) = (p - 1) * (q - 1)
```
Ela conta quantos números entre 1 e n são coprimos com n (não compartilham fatores).

### 2.4 Teorema de Euler
Se `a` e `n` são coprimos, então:
```
a^{φ(n)} ≡ 1 (mod n)
```
Esse teorema é a base matemática que garante o funcionamento do RSA.

### 2.5 Inverso Multiplicativo Modular
Dados `a` e `m`, o inverso de `a` módulo `m` é um número `b` tal que:
```
a * b ≡ 1 (mod m)
```
No RSA, precisamos do inverso de `e` módulo `φ(n)` para obter o expoente privado `d`.

---

## 3. Geração de Chaves

O processo de geração de chaves é o coração do RSA. Vamos detalhar cada passo.

### 3.1 Passo a Passo

1. **Escolha dois números primos grandes, `p` e `q`**  
   Eles devem ser mantidos em segredo. O tamanho recomendado atualmente é de pelo menos 1024 bits cada (total de 2048 bits para `n`).

2. **Calcule `n = p * q`**  
   `n` é o módulo, parte da chave pública e privada.

3. **Calcule a função totiente: `φ(n) = (p - 1) * (q - 1)`**

4. **Escolha um expoente público `e`**  
   Deve satisfazer:  
   - `1 < e < φ(n)`  
   - `e` e `φ(n)` devem ser coprimos (mdc(e, φ(n)) = 1)  
   - Valores comuns: 3, 17, 65537 (o último é o mais usado por segurança e eficiência)

5. **Calcule o expoente privado `d`**  
   `d` é o inverso multiplicativo de `e` módulo `φ(n)`, ou seja:
   ```
   d * e ≡ 1 (mod φ(n))
   ```

6. **Chave pública**: (n, e)  
   **Chave privada**: (n, d) – opcionalmente, podem-se armazenar também `p`, `q` e outros valores para acelerar a descriptografia usando o Teorema Chinês do Resto (CRT).

### 3.2 Exemplo Numérico Pequeno (apenas para entendimento)

```
p = 61, q = 53
n = 61 * 53 = 3233
φ(n) = 60 * 52 = 3120
e = 17 (mdc(17,3120)=1)
d = 2753 (pois 17 * 2753 = 46801 ≡ 1 mod 3120)
Chave pública: (3233, 17)
Chave privada: (3233, 2753)
```

### 3.3 Considerações Práticas
- Os primos `p` e `q` devem ser escolhidos aleatoriamente e serem grandes o suficiente para resistir à fatoração.
- `e` geralmente é fixo (65537) para todos os usuários.
- `d` é calculado uma única vez e mantido em segredo.

---

## 4. Criptografia e Descriptografia

### 4.1 Criptografia
- **Entrada**: mensagem `m` (representada como um número inteiro menor que `n`) e chave pública `(n, e)`
- **Cálculo**:
  ```
  c = m^e mod n
  ```
- **Saída**: texto cifrado `c`

### 4.2 Descriptografia
- **Entrada**: texto cifrado `c` e chave privada `(n, d)`
- **Cálculo**:
  ```
  m = c^d mod n
  ```
- **Saída**: mensagem original `m`

### 4.3 Por que funciona?
Pelo Teorema de Euler, como `e*d ≡ 1 (mod φ(n))`, temos:
```
c^d ≡ (m^e)^d ≡ m^(e*d) ≡ m^(k*φ(n)+1) ≡ m (mod n)
```
(desde que `m` seja coprimo com `n`; caso não seja, o resultado ainda vale por outros teoremas, mas a chance é desprezível para `n` grande.)

### 4.4 Requisitos sobre a Mensagem
- A mensagem `m` deve ser **menor que n**. Caso seja maior, deve-se dividir em blocos.
- A mensagem **não pode ser 0 ou 1** (pois a cifragem seria previsível). O esquema de preenchimento resolve isso.

---

## 5. Assinaturas Digitais

O RSA também pode ser usado para assinar digitalmente uma mensagem, garantindo que ela foi criada por quem possui a chave privada.

### 5.1 Geração da Assinatura
- O remetente calcula o hash da mensagem (ex: SHA-256) e o converte em um número `h`.
- Com a chave privada `(n, d)`, calcula:
  ```
  s = h^d mod n
  ```
- A assinatura `s` é anexada à mensagem.

### 5.2 Verificação da Assinatura
- O destinatário recebe a mensagem e a assinatura `s`.
- Calcula o hash `h` da mensagem (usando o mesmo algoritmo).
- Com a chave pública `(n, e)`, calcula:
  ```
  h' = s^e mod n
  ```
- Se `h' == h`, a assinatura é válida.

### 5.3 Por que usar hash?
Assinar diretamente a mensagem seria muito lento para mensagens grandes. O hash reduz a mensagem a um tamanho fixo e pequeno, além de evitar ataques de extensão de mensagem.

---

## 6. Implementação em Python (Passo a Passo)

Vamos implementar uma versão simplificada do RSA para entendimento. **ATENÇÃO**: Esta implementação é apenas didática e **não é segura** para uso real (falta padding, os primos são pequenos, etc.).

### 6.1 Geração de Primos Grandes
Em Python, podemos usar a biblioteca `sympy` para gerar primos de forma confiável. Instale com `pip install sympy`.

```python
import random
from sympy import isprime, randprime

def gerar_primo(tamanho_bits):
    # Gera um primo aleatório com aproximadamente 'tamanho_bits' bits
    limite_inferior = 2**(tamanho_bits - 1)
    limite_superior = 2**tamanho_bits - 1
    p = randprime(limite_inferior, limite_superior)
    return p
```

### 6.2 Algoritmo de Euclides Estendido para Inverso Modular
Precisamos calcular o inverso de `e` módulo `φ(n)`.

```python
def egcd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, y, x = egcd(b % a, a)
        return (g, x - (b // a) * y, y)

def inverso_modular(e, phi):
    g, x, y = egcd(e, phi)
    if g != 1:
        raise Exception('O inverso modular não existe')
    else:
        return x % phi
```

### 6.3 Geração de Chaves

```python
def gerar_chaves(tamanho_bits=1024):
    # Escolher p e q com metade dos bits cada
    p = gerar_primo(tamanho_bits // 2)
    q = gerar_primo(tamanho_bits // 2)
    while p == q:
        q = gerar_primo(tamanho_bits // 2)
    
    n = p * q
    phi = (p - 1) * (q - 1)
    
    # Escolher e (comum: 65537)
    e = 65537
    # Verificar se e e phi são coprimos
    while e < phi:
        if egcd(e, phi)[0] == 1:
            break
        e += 2  # tentar próximo ímpar
    
    d = inverso_modular(e, phi)
    
    # Chave pública: (e, n); chave privada: (d, n)
    return ((e, n), (d, n))
```

### 6.4 Criptografia e Descriptografia (textbook)

```python
def criptografar(mensagem_int, chave_publica):
    e, n = chave_publica
    if mensagem_int >= n:
        raise ValueError("Mensagem muito longa para o módulo n")
    return pow(mensagem_int, e, n)  # pow com três argumentos é eficiente

def descriptografar(cifrado_int, chave_privada):
    d, n = chave_privada
    return pow(cifrado_int, d, n)
```

### 6.5 Exemplo de Uso

```python
# Gerar chaves (usando bits pequenos para teste)
pub, priv = gerar_chaves(tamanho_bits=256)

mensagem = 42
cifrado = criptografar(mensagem, pub)
print("Cifrado:", cifrado)

decifrado = descriptografar(cifrado, priv)
print("Decifrado:", decifrado)
```

**Nota**: O uso de `pow(base, exp, mod)` é muito mais eficiente do que `(base**exp) % mod`, especialmente para números grandes.

---

## 7. Esquemas de Preenchimento (Padding)

A implementação acima é conhecida como **RSA textbook** e é **extremamente insegura**. Ataques como:
- **Determinístico**: a mesma mensagem sempre produz o mesmo cifrado.
- **Mensagens pequenas**: se `m^e < n`, a raiz e-ésima pode ser extraída.
- **Ataque de multiplicidade**: propriedade homomórfica permite manipular cifrados.

Para resolver, usamos **esquemas de preenchimento** que adicionam aleatoriedade e estrutura à mensagem antes da exponenciação.

### 7.1 PKCS#1 v1.5
- Adiciona bytes aleatórios de preenchimento.
- Formato: `0x00 || 0x02 || [bytes aleatórios] || 0x00 || mensagem`.
- Embora amplamente usado, possui vulnerabilidades (ex: ataque de Bleichenbacher). **Não recomendado para novos sistemas**.

### 7.2 OAEP (Optimal Asymmetric Encryption Padding)
- Mais seguro, baseado em funções de hash e máscaras.
- Garante segurança semântica (IND-CCA2).
- É o padrão atual para criptografia RSA.

### 7.3 PSS (Probabilistic Signature Scheme)
- Para assinaturas, equivalente ao OAEP.

Na prática, **nunca implemente seu próprio padding**. Use bibliotecas confiáveis como `PyCryptodome` ou `cryptography`.

---

## 8. Considerações de Segurança

### 8.1 Tamanho da Chave
- 1024 bits: **obsoleto** (quebrado em 2010).
- 2048 bits: mínimo aceitável hoje.
- 3072 ou 4096 bits: recomendado para segurança a longo prazo.

### 8.2 Geração de Primos
- Devem ser gerados aleatoriamente, com boa entropia.
- Evitar primos muito próximos um do outro (pode facilitar fatoração).

### 8.3 Ataques de Canal Lateral
- Tempo, consumo de energia, radiação eletromagnética podem vazar informações.
- Bibliotecas como PyCryptodome já implementam contramedidas (ex: exponenciação constante).

### 8.4 Escolha do Expoente Público e
- `e = 65537` (0x10001) é uma boa escolha: grande o suficiente para evitar ataques de pequeno expoente, mas com poucos bits 1 para eficiência.

### 8.5 Proteção da Chave Privada
- Armazenar de forma segura, possivelmente cifrada com senha.

---

## 9. Exemplo Completo com PyCryptodome

A biblioteca `pycryptodome` oferece implementações seguras de RSA com padding. Instale com:
```bash
pip install pycryptodome
```

### 9.1 Geração de Par de Chaves

```python
from Crypto.PublicKey import RSA

# Gera chave de 2048 bits
key = RSA.generate(2048)

# Exporta chave privada (PEM)
private_key = key.export_key()
with open("private.pem", "wb") as f:
    f.write(private_key)

# Exporta chave pública
public_key = key.publickey().export_key()
with open("public.pem", "wb") as f:
    f.write(public_key)
```

### 9.2 Criptografia com OAEP

```python
from Crypto.Cipher import PKCS1_OAEP
from Crypto.PublicKey import RSA
from Crypto.Hash import SHA256

# Carregar chave pública
with open("public.pem", "rb") as f:
    public_key = RSA.import_key(f.read())

cipher = PKCS1_OAEP.new(public_key, hashAlgo=SHA256)

mensagem = b"Mensagem secreta"
cifrado = cipher.encrypt(mensagem)
print("Cifrado:", cifrado.hex())
```

### 9.3 Descriptografia com OAEP

```python
from Crypto.Cipher import PKCS1_OAEP
from Crypto.PublicKey import RSA
from Crypto.Hash import SHA256

with open("private.pem", "rb") as f:
    private_key = RSA.import_key(f.read())

cipher = PKCS1_OAEP.new(private_key, hashAlgo=SHA256)

decifrado = cipher.decrypt(cifrado)
print("Decifrado:", decifrado.decode())
```

### 9.4 Assinatura com PSS

```python
from Crypto.Signature import pss
from Crypto.Hash import SHA256
from Crypto.PublicKey import RSA

# Carregar chave privada
with open("private.pem", "rb") as f:
    private_key = RSA.import_key(f.read())

# Mensagem a assinar
mensagem = b"Documento importante"
h = SHA256.new(mensagem)

# Assinar
signer = pss.new(private_key)
assinatura = signer.sign(h)

# Verificar
with open("public.pem", "rb") as f:
    public_key = RSA.import_key(f.read())

verifier = pss.new(public_key)
try:
    verifier.verify(h, assinatura)
    print("Assinatura válida!")
except (ValueError, TypeError):
    print("Assinatura inválida!")
```

