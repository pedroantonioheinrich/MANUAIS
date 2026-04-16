```markdown
# 📚 Manual Definitivo de Comandos Git

Este manual contém todos os comandos do Git, desde os mais básicos até os de baixo nível (plumbing), organizados por categorias com explicações diretas e objetivas.

---

## 🛠️ 1. Configuração Básica
Comandos para configurar sua identidade e preferências no Git.

*   `git config --global user.name "Nome"`: Define seu nome de usuário global.
*   `git config --global user.email "email"`: Define seu e-mail global.
*   `git config --local user.name "Nome"`: Define seu nome apenas para o repositório atual.
*   `git config --list`: Lista todas as configurações atuais do Git.
*   `git config core.editor "code --wait"`: Define o VS Code como editor padrão de mensagens de commit.
*   `git config init.defaultBranch main`: Muda o nome da branch padrão de `master` para `main`.

---

## 🗂️ 2. Criação e Clonagem
Comandos para iniciar ou copiar um repositório.

*   `git init`: Inicia um novo repositório Git na pasta atual.
*   `git init [pasta]`: Inicia um repositório dentro de uma pasta específica.
*   `git clone [url]`: Clona um repositório remoto para a sua máquina.
*   `git clone [url] [nome_da_pasta]`: Clona o repositório e renomeia a pasta de destino.
*   `git clone --branch [branch] [url]`: Clona o repositório já trazendo uma branch específica.

---

## 📝 3. Alterações Básicas (Snapshot)
Comandos para rastrear e salvar mudanças nos arquivos.

*   `git status`: Mostra o estado atual dos arquivos (modificados, em staging, não rastreados).
*   `git add [arquivo]`: Adiciona um arquivo específico à área de staging (preparação para commit).
*   `git add .`: Adiciona todas as mudanças da pasta atual ao staging.
*   `git add -A` (ou `--all`): Adiciona todas as mudanças (incluindo deleções) de todo o projeto ao staging.
*   `git add -p`: Adiciona mudanças interativamente, permitindo escolher quais trechos de um arquivo incluir.
*   `git commit -m "mensagem"`: Salva as mudanças em staging no histórico com uma mensagem.
*   `git commit -am "mensagem"`: Adiciona todos os arquivos *já rastreados* que foram modificados e faz o commit de uma vez.
*   `git commit --amend`: Altera o último commit (adiciona arquivos esquecidos ou muda a mensagem).
*   `git rm [arquivo]`: Remove o arquivo do projeto e do rastreamento do Git.
*   `git rm --cached [arquivo]`: Remove o arquivo do rastreamento do Git, mas mantém o arquivo físico na sua pasta.
*   `git mv [antigo] [novo]`: Renomeia ou move um arquivo, registrando a mudança no Git.

---

## 🌿 4. Branches e Merging
Comandos para trabalhar com linhas do tempo separadas.

*   `git branch`: Lista todas as branches locais.
*   `git branch [nome]`: Cria uma nova branch.
*   `git branch -d [nome]`: Deleta uma branch (apenas se já foi mesclada).
*   `git branch -D [nome]`: Força a deleção de uma branch (mesmo que não tenha sido mesclada).
*   `git branch -m [novo_nome]`: Renomeia a branch atual.
*   `git checkout [nome]`: Muda para a branch especificada.
*   `git checkout -b [nome]`: Cria e muda para a nova branch em um único comando.
*   `git switch [nome]`: **(Recomendado)** Muda para a branch especificada (mais seguro que o checkout).
*   `git switch -c [nome]`: **(Recomendado)** Cria e muda para a nova branch.
*   `git merge [nome]`: Mescla a branch especificada na branch atual.
*   `git merge --no-ff [nome]`: Mescla criando um commit de merge (mesmo que seja um fast-forward), preservando o histórico.
*   `git cherry-pick [hash_do_commit]`: Aplica as mudanças de um commit específico na branch atual.
*   `git rebase [branch]`: Reposiciona os commits da branch atual sobre o topo da branch especificada (evita commits de merge desnecessários).
*   `git rebase --continue`: Continua o rebase após resolver conflitos.
*   `git rebase --abort`: Cancela o rebase em andamento e volta ao estado anterior.

---

## ☁️ 5. Repositórios Remotos
Comandos para interagir com servidores como GitHub, GitLab, etc.

*   `git remote -v`: Mostra os repositórios remotos conectados.
*   `git remote add [nome] [url]`: Conecta um repositório remoto (geralmente chamado de `origin`).
*   `git remote remove [nome]`: Remove a conexão com um repositório remoto.
*   `git remote rename [antigo] [novo]`: Renomeia um remoto.
*   `git fetch [remoto]`: Baixa as informações e objetos do remoto, mas **não** altera seus arquivos locais.
*   `git pull [remoto] [branch]`: Baixa as mudanças e já as aplica (faz um fetch + merge automaticamente).
*   `git pull --rebase [remoto] [branch]`: Baixa as mudanças e faz um rebase em vez de merge (mantém o histórico linear).
*   `git push [remoto] [branch]`: Envia seus commits locais para o repositório remoto.
*   `git push -u [remoto] [branch]`: Envia os commits e configura a branch local para rastrear a branch remota (upstream).
*   `git push --force`: **(Perigoso)** Força o envio, sobrescrevendo o histórico do remoto. Evite usar em branches compartilhadas.
*   `git push --force-with-lease`: Versão mais segura do force push; falha se alguém mais tiver enviado commits para o remoto.

---

## 🔍 6. Inspeção e Comparação
Comandos para ver diferenças e ler o histórico.

*   `git log`: Mostra o histórico de commits.
*   `git log --oneline`: Mostra o histórico de forma resumida (uma linha por commit).
*   `git log --graph`: Mostra o histórico em formato de gráfico (excelente para ver merges).
*   `git log --decorate`: Mostra de qual branch ou tag cada commit faz parte.
*   `git log -p`: Mostra o histórico junto com as diferenças (diff) introduzidas em cada commit.
*   `git log --stat`: Mostra o histórico com estatísticas de quantas linhas foram alteradas em cada arquivo.
*   `git log -[n]`: Mostra apenas os últimos *n* commits (ex: `git log -3`).
*   `git show [hash]`: Mostra os detalhes e as mudanças de um commit específico.
*   `git show [hash]:[arquivo]`: Mostra como o arquivo era em um commit específico sem alterar sua pasta.
*   `git diff`: Mostra mudanças feitas nos arquivos que ainda não foram adicionadas ao staging.
*   `git diff --staged`: Mostra mudanças que já estão no staging (prontas para commit).
*   `git diff [branch1] [branch2]`: Mostra a diferença entre duas branches.
*   `git diff [commit1] [commit2]`: Mostra a diferença entre dois commits específicos.
*   `git shortlog`: Agrupa os commits por autor (útil para ver quem fez o quê).
*   `git blame [arquivo]`: Mostra quem foi o autor da última alteração de cada linha do arquivo.

---

## ⏪ 7. Desfazendo e Corrigindo
Comandos para reverter erros (usados com cuidado).

*   `git restore [arquivo]`: Descarta mudanças no arquivo (volta ao estado do último commit).
*   `git restore --staged [arquivo]`: Retira o arquivo do staging, mas mantém as modificações nele.
*   `git reset [hash]`: Volta o ponteiro da branch para o commit informado, mantendo as mudanças nos arquivos (staged).
*   `git reset --soft [hash]`: Volta apenas o ponteiro, mantendo tudo no staging (bom para refazer o último commit).
*   `git reset --mixed [hash]`: Volta o ponteiro e tira do staging, mas mantém as mudanças nos arquivos (padrão do `git reset`).
*   `git reset --hard [hash]`: **(Perigoso)** Volta o ponteiro, tira do staging e **apaga** as mudanças nos arquivos.
*   `git revert [hash]`: Cria um *novo commit* que desfaz as mudanças do commit especificado (seguro para usar em equipes).
*   `git clean -n`: Mostra quais arquivos não rastreados seriam apagados (simulação).
*   `git clean -f`: Apaga os arquivos não rastreados da pasta.
*   `git clean -fd`: Apaga os arquivos e pastas não rastreadas.
*   `git stash`: Salva temporariamente as mudanças modificadas/não commitadas.
*   `git stash list`: Lista todas as mudanças salvas no stash.
*   `git stash pop`: Aplica a última mudança salva do stash e a remove da lista.
*   `git stash apply`: Aplica a última mudança salva, mas a mantém na lista do stash.
*   `git stash drop`: Remove a última entrada do stash.
*   `git stash clear`: Apaga todas as entradas do stash.
*   `git reflog`: Mostra um diário de *absolutamente todos* os movimentos do ponteiro HEAD (a "máquina do tempo" do Git).

---

## 🏷️ 8. Tags
Comandos para marcar versões específicas (ex: v1.0.0).

*   `git tag`: Lista todas as tags existentes.
*   `git tag [nome]`: Cria uma tag leve (lightweight) no commit atual.
*   `git tag -a [nome] -m "mensagem"`: Cria uma tag anotada (com data, autor e mensagem - recomendada para versões).
*   `git tag [nome] [hash]`: Cria uma tag em um commit específico do passado.
*   `git push [remoto] [nome]`: Envia uma tag específica para o remoto.
*   `git push [remoto] --tags`: Envia todas as tags locais para o remoto.
*   `git tag -d [nome]`: Deleta uma tag local.
*   `git push [remoto] --delete [nome]`: Deleta uma tag no repositório remoto.

---

## 🔎 9. Busca e Filtragem
Comandos para achar coisas dentro do repositório.

*   `git grep "[texto]"`: Busca por um texto dentro do código-fonte dos arquivos rastreados.
*   `git grep -n "[texto]"`: Busca o texto e mostra o número da linha.
*   `git grep -e "[padrao1]" --and -e "[padrao2]"`: Busca usando múltiplos padrões.
*   `git log -S "[texto]"`: Mostra os commits onde a quantidade de ocorrências de um texto específico mudou (útil para achar quando um recurso foi adicionado/removido).
*   `git log -G "[regex]"`: Igual ao `-S`, mas usa expressões regulares.
*   `git log --grep="[texto]"`: Busca texto dentro das *mensagens de commit*.

---

## 📦 10. Submódulos e Subárvores
Comandos para gerenciar repositórios dentro de outros repositórios.

*   `git submodule add [url] [pasta]`: Adiciona um repositório como submódulo dentro do seu projeto.
*   `git submodule init`: Inicializa o arquivo de configuração local de submódulos.
*   `git submodule update`: Baixa os dados dos submódulos definidos.
*   `git submodule update --init --recursive`: Comando mágico que clona e inicializa todos os submódulos e os submódulos deles.
*   `git subtree add --prefix=[pasta] [url] [branch]`: Adiciona um repositório dentro de uma pasta específica mesclando seus históricos (alternativa ao submodule).
*   `git subtree pull --prefix=[pasta] [url] [branch]`: Puxa as atualizações do repositório externo.

---

## 🧰 11. Comandos de Baixo Nível (Plumbing)
*Estes comandos interagem diretamente com o banco de dados interno do Git (objects). Raramente são usados no dia a dia, mas são a base de tudo e úteis para criar scripts.*

*   `git hash-object -w [arquivo]`: Calcula o hash SHA-1 de um arquivo e o salva no banco de dados do Git.
*   `git cat-file -p [hash]`: Mostra o conteúdo de um objeto (commit, blob, tree) armazenado no Git.
*   `git cat-file -t [hash]`: Mostra o tipo de um objeto (blob, tree, commit, tag).
*   `git update-index --add [arquivo]`: Adiciona um arquivo manualmente ao índice (staging area de baixo nível).
*   `git ls-files --stage`: Lista os arquivos que estão no índice (staging) e seus hashes.
*   `git write-tree`: Pega o índice atual e escreve um objeto do tipo "tree" no banco de dados do Git.
*   `git commit-tree [tree_hash] -p [parent_hash] -m "msg"`: Cria um objeto do tipo "commit" manualmente usando uma árvore e um commit pai.
*   `git update-ref refs/heads/[branch] [commit_hash]`: Move o ponteiro de uma branch manualmente para um commit específico.
*   `git show-ref`: Mostra todas as referências (branches, tags) no repositório.
*   `git for-each-ref`: Formata e lista as referências (muito usado em scripts).
*   `git rev-parse [branch]`: Retorna o hash SHA-1 de uma referência (ex: transforma "main" no hash do último commit).
*   `git fsck`: Verifica a integridade do banco de dados do Git e procura por objetos corrompidos.

---

## 💡 12. Ajuda e Atalhos
*   `git [comando] --help`: Abre o manual oficial detalhado desse comando no navegador ou terminal.
*   `git help [comando]`: Mesma função do comando acima.
*   `git alias [nome] [comando]`: (Uso via config) Ex: `git config alias.s "status -sb"` cria o atalho `git s`.

---

## 🛡️ 13. Segurança e Assinatura
*   `git tag -v [nome]`: Verifica a assinatura GPG de uma tag.
*   `git commit -S`: Assina o commit usando sua chave GPG.
*   `git log --show-signature`: Mostra as assinaturas GPG dos commits no log.

---

## 🧹 14. Manutenção e Limpeza
*   `git gc` (Garbage Collector): Limpa objetos inalcançáveis e otimiza o banco de dados local.
*   `git prune`: Remove objetos órfãos que não são alcançáveis por nenhum commit (geralmente chamado pelo `gc`).
*   `git fsck --unreachable`: Encontra objetos que estão no Git mas não fazem parte de nenhum histórico.
*   `git count-objects -v`: Mostra estatísticas de quanto espaço o banco de dados do Git está ocupando e quantos objetos estão empacotados.
*   `git rerere`: Reuse Recorded Resolution (Reutiliza resolução de conflitos gravadas anteriormente - precisa ser ativado via config).
