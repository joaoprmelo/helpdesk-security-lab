# 🐧 Linux Essentials — Guia de Estudos

Anotações e resumo prático do curso **Cisco NDG Linux Essentials** (Cisco Networking Academy), organizados por laboratório. Este guia reúne os principais comandos que pratiquei, com explicações curtas e exemplos, para servir de consulta rápida.

> 🎓 Curso concluído com certificado. Este repositório é parte do meu portfólio de estudos em **Suporte de TI, Redes e Segurança da Informação**.

---

## 📑 Sumário

- [Lab 5 — Habilidades de linha de comando](#lab-5--habilidades-de-linha-de-comando)
- [Lab 6 — Obtendo ajuda](#lab-6--obtendo-ajuda)
- [Lab 7 — Navegando pelo sistema de arquivos](#lab-7--navegando-pelo-sistema-de-arquivos)
- [Lab 8 — Gerenciando arquivos e diretórios](#lab-8--gerenciando-arquivos-e-diretórios)
- [Lab 9 — Arquivamento e compressão](#lab-9--arquivamento-e-compressão)
- [Lab 10 — Trabalhando com texto](#lab-10--trabalhando-com-texto)
- [Lab 11 — Scripts básicos e editor vi](#lab-11--scripts-básicos-e-editor-vi)
- [Lab 12 — Hardware do computador](#lab-12--hardware-do-computador)
- [Lab 13 — Onde os dados ficam armazenados](#lab-13--onde-os-dados-ficam-armazenados)
- [Lab 14 — Configuração de rede](#lab-14--configuração-de-rede)
- [Lab 15 — Segurança do sistema e dos usuários](#lab-15--segurança-do-sistema-e-dos-usuários)
- [Lab 16 — Criando usuários e grupos](#lab-16--criando-usuários-e-grupos)
- [Lab 17 — Propriedade e permissões](#lab-17--propriedade-e-permissões)
- [Lab 18 — Diretórios e arquivos especiais](#lab-18--diretórios-e-arquivos-especiais)

---

## Lab 5 — Habilidades de linha de comando

**Objetivos:** explorar recursos do Bash, usar variáveis de shell e entender o uso de aspas (*quoting*).

```bash
echo $HOME                 # exibe o valor de uma variável
NOME="Linux"               # cria uma variável de shell (sem espaços ao redor do =)
echo "Olá, $NOME"          # aspas duplas: a variável é expandida
echo 'Olá, $NOME'          # aspas simples: o texto é literal
export NOME                # torna a variável disponível para processos filhos
env                        # lista as variáveis de ambiente
```

| Aspas | Comportamento |
|-------|---------------|
| `" "` | Expande variáveis (`$VAR`) e comandos (`$(cmd)`) |
| `' '` | Tudo é literal, nada é expandido |
| `\`   | Escapa o próximo caractere |

---

## Lab 6 — Obtendo ajuda

**Objetivos:** usar diferentes sistemas de ajuda e localizar comandos.

```bash
man ls                     # manual completo do comando
man -k copy                # busca páginas do manual por palavra-chave (igual a apropos)
ls --help                  # resumo rápido das opções
info ls                    # documentação em formato info
whatis ls                  # descrição de uma linha
which ls                   # mostra o caminho do executável
type cd                    # indica se é comando interno, alias ou arquivo
whereis ls                 # localiza binário, manual e fontes
```

---

## Lab 7 — Navegando pelo sistema de arquivos

**Objetivos:** navegar pelos diretórios pessoal e do sistema e listar arquivos e diretórios.

```bash
pwd                        # mostra o diretório atual
cd /etc                    # vai para um diretório (caminho absoluto)
cd ..                      # sobe um nível
cd ~                       # vai para o diretório home (cd sozinho também)
cd -                       # volta ao diretório anterior
ls                         # lista arquivos
ls -l                      # lista detalhada (permissões, dono, tamanho, data)
ls -a                      # inclui arquivos ocultos (começam com ponto)
ls -lh                     # tamanhos legíveis (KB, MB)
```

---

## Lab 8 — Gerenciando arquivos e diretórios

**Objetivos:** entender *globbing* e criar, mover e apagar arquivos e diretórios.

**Globbing (curingas):**

| Caractere | Significado |
|-----------|-------------|
| `*` | Qualquer sequência de caracteres |
| `?` | Exatamente um caractere |
| `[abc]` | Um caractere dentre os listados |
| `[a-z]` | Um caractere dentro do intervalo |

```bash
touch arquivo.txt          # cria um arquivo vazio
mkdir projetos             # cria um diretório
mkdir -p a/b/c             # cria diretórios aninhados
cp origem.txt destino.txt  # copia arquivo
cp -r pasta/ copia/        # copia diretório recursivamente
mv antigo.txt novo.txt     # move ou renomeia
rm arquivo.txt             # apaga arquivo
rm -r pasta/               # apaga diretório e conteúdo
rmdir pasta_vazia          # apaga diretório vazio
ls *.txt                   # exemplo de globbing: todos os .txt
```

> ⚠️ O `rm` não manda para a lixeira. Use com cuidado, principalmente `rm -r`.

---

## Lab 9 — Arquivamento e compressão

**Objetivos:** arquivar e comprimir arquivos com `tar`, `gzip`, `bzip2`, `xz` e `zip`.

**Opções do `tar`:**

| Opção | Função |
|-------|--------|
| `-c` | Cria um arquivo |
| `-x` | Extrai |
| `-t` | Lista o conteúdo |
| `-v` | Modo verboso (mostra o que está fazendo) |
| `-f` | Nome do arquivo (*filename*) |
| `-z` / `-j` / `-J` | Comprime com gzip / bzip2 / xz |

```bash
tar -cvf backup.tar pasta/       # cria o arquivo .tar
tar -tvf backup.tar              # lista o conteúdo
tar -xvf backup.tar              # extrai
tar -czvf backup.tar.gz pasta/   # cria e comprime com gzip

gzip arquivo.txt                 # gera arquivo.txt.gz
gunzip arquivo.txt.gz            # descomprime
bzip2 arquivo.txt                # gera arquivo.txt.bz2
bunzip2 arquivo.txt.bz2
xz arquivo.txt                   # gera arquivo.txt.xz
unxz arquivo.txt.xz

zip pacote.zip arq1 arq2         # cria um .zip
unzip pacote.zip                 # extrai
```

---

## Lab 10 — Trabalhando com texto

**Objetivos:** redirecionar saídas, filtrar e transformar texto com os comandos mais usados no dia a dia.

### Redirecionamento e pipes

| Símbolo | Função |
|---------|--------|
| `>` | Redireciona a saída do `stdout` (terminal) para um arquivo, **sobrescrevendo** |
| `>>` | **Acrescenta** ao arquivo, preservando os dados existentes |
| `2>` | Redireciona o `stderr` (canal 2) |
| `2>&1` | Envia o `stderr` (canal 2) para o mesmo lugar do `stdout` (canal 1) |
| `\|` | Pipe: usa a saída de um comando como entrada do próximo |

```bash
ls > lista.txt             # salva a saída em um arquivo
echo "nova linha" >> lista.txt
comando > saida.txt 2>&1   # stdout e stderr no mesmo arquivo
```

### Exibindo e navegando em arquivos

```bash
cat arquivo.txt            # exibe o conteúdo do arquivo
cat arq1 arq2 > junto.txt  # concatena arquivos
more arquivo.txt           # exibe uma página por vez
ls -l /etc | more          # usando pipe com more
less arquivo.txt           # como o more, mas permite voltar
```

> 💡 Dentro do `more` e do `less`, pesquise um padrão digitando `/` seguido do texto.

```bash
head arquivo.txt           # mostra as 10 primeiras linhas
head -5 arquivo.txt        # mostra as 5 primeiras (qualquer número: -#)
tail arquivo.txt           # mostra as 10 últimas linhas
tail -5 arquivo.txt        # mostra as 5 últimas
tail -f /var/log/syslog    # acompanha o arquivo em tempo real
```

### Transformando e organizando

```bash
tr 'a-z' 'A-Z'             # traduz caracteres (minúsculas para maiúsculas)
cut -d: -f1 /etc/passwd    # extrai o 1º campo (nome de usuário), delimitador ":"
sort arquivo.txt           # ordena a saída
sort -r arquivo.txt        # ordem reversa
sort -n arquivo.txt        # ordem numérica
```

> 💡 O `tr` lê da entrada padrão. Para encerrar a entrada manualmente, pressione **Ctrl+D**.

### Procurando arquivos

```bash
find /home -name "*.txt"   # busca no diretório e em todos os subdiretórios
find . -type d             # apenas diretórios
```

### A família grep

| Comando | Descrição |
|---------|-----------|
| `grep` | Usa **expressões regulares básicas** (caracteres especiais como curingas) e retorna a **linha inteira** que contém o padrão |
| `egrep` (ou `grep -E`) | Usa **expressões regulares estendidas**, mais poderosas |
| `fgrep` (ou `grep -F`) | Busca **caracteres literais**, ignorando o significado especial dos caracteres de regex |

```bash
grep root /etc/passwd          # linhas que contêm "root"
grep -i erro log.txt           # ignora maiúsculas/minúsculas
grep -v erro log.txt           # linhas que NÃO contêm o padrão
grep -n erro log.txt           # mostra o número da linha
egrep "erro|falha" log.txt     # um padrão OU outro
fgrep "a.b" arquivo.txt        # procura exatamente "a.b" (o ponto não é curinga)
```

---

## Lab 11 — Scripts básicos e editor vi

**Objetivos:** editar arquivos com o `vi` e criar um primeiro script Bash.

### Editor vi

O `vi` possui dois modos principais:

- **Modo de inserção (insert):** onde você digita e adiciona texto. Pressione `Esc` para sair dele.
- **Modo de comando (command):** onde você navega, pesquisa, salva e sai do editor.

#### Entrando no modo de inserção

| Tecla | Ação |
|-------|------|
| `i` | Insere antes do cursor |
| `a` | Insere após o cursor |
| `A` (Shift+a) | Adiciona texto no **final da linha** |
| `o` | Abre uma linha em branco **abaixo** da linha atual |

#### Salvando e saindo (modo de comando)

| Comando | Ação |
|---------|------|
| `:w` | Salva o arquivo |
| `:q` | Sai |
| `:wq` | Salva e sai |
| `:x` ou `ZZ` | Salva e fecha |
| `:q!` | Sai **sem salvar** |
| `:w!` | Força a gravação em arquivo somente leitura (se possível) |
| `:wq!` | Força gravar em arquivo somente leitura (se possível) e sai |
| `:e!` | Descarta as alterações e recarrega o arquivo |

#### Editando (modo de comando)

| Comando | Ação |
|---------|------|
| `x` | Apaga um caractere |
| `X` (Shift+x) | Apaga o caractere à esquerda do cursor (`nX` apaga *n* caracteres) |
| `dw` | Apaga uma palavra |
| `dd` | Apaga a linha atual |
| `D` (Shift+d) | Apaga da posição atual até o fim da linha |
| `cw` | Altera (change) uma palavra |
| `J` (Shift+j) | Junta a linha atual com a próxima |
| `yw` | Copia (yank) uma palavra |
| `yy` | Copia a linha inteira |
| `p` | Cola após o cursor (também cola linhas apagadas) |
| `P` (Shift+p) | Cola antes do cursor |
| `u` | Desfaz |
| `:%s/texto//g` | Busca e **remove** a palavra em todo o arquivo |
| `:%s/velho/novo/g` | Substitui "velho" por "novo" em todo o arquivo |

### Primeiro script Bash

```bash
#!/bin/bash
# sample.sh — meu primeiro script
echo "Olá, mundo!"
for i in $(seq 1 10); do
    echo "Número: $i"
done
```

- `#!/bin/bash` (*shebang*): primeira linha do arquivo, deixa claro que é um script de **shell Bash**.
- `chmod a+x sample.sh`: torna o arquivo **executável** para todos, evitando digitar `bash` na frente do nome.
- `seq 1 10`: gera uma lista de números inteiros, neste caso de 1 a 10.

```bash
chmod a+x sample.sh
./sample.sh
```

---

## Lab 12 — Hardware do computador

**Objetivos:** identificar CPU, memória, dispositivos e discos pelo terminal.

| Comando | Para que serve |
|---------|----------------|
| `lscpu` | Determina o tipo de CPU |
| `cat /proc/cpuinfo` | Detalhes ainda mais completos sobre a(s) CPU(s) |
| `free` | Mostra o uso de RAM e swap (`-m` em megabytes, `-g` em gigabytes) |
| `lspci` | Lista os dispositivos conectados ao barramento PCI (`-k` mostra também o módulo do kernel em uso) |
| `lsusb` | Lista os dispositivos USB conectados |
| `lsmod` | Exibe os módulos do kernel atualmente carregados |
| `fdisk` | Identifica e manipula dispositivos de armazenamento |

```bash
lscpu
free -m
lspci -k
sudo fdisk -l              # -l lista os dispositivos de bloco (discos e volumes lógicos) sem modo interativo
```

> ⚠️ O `fdisk` **sem** `-l` entra em modo interativo, normalmente usado para **modificar partições**. Cuidado ao usá-lo em discos reais.

---

## Lab 13 — Onde os dados ficam armazenados

**Objetivos:** entender o diretório `/proc` e gerenciar processos.

| Comando | Função |
|---------|--------|
| `/proc` | Sistema de arquivos virtual com informações do kernel e dos processos em execução |
| `ps` | Lista processos (`ps aux` mostra todos) |
| `top` | Monitora processos em tempo real (CPU, memória) |
| `jobs` | Lista tarefas em segundo plano da sessão atual |
| `kill` | Envia um sinal a um processo pelo PID |
| `killall` | Envia um sinal a processos pelo **nome** |
| `ping` | Testa a conectividade com outro host |
| `free` | Mostra o uso de memória |

```bash
ls /proc
cat /proc/meminfo
ps aux
top
sleep 300 &                # executa em segundo plano
jobs
kill 1234                  # encerra o processo de PID 1234
kill -9 1234               # força o encerramento (SIGKILL)
killall firefox
ping -c 4 8.8.8.8          # envia 4 pacotes
```

---

## Lab 14 — Configuração de rede

**Objetivos:** consultar e diagnosticar a configuração de rede.

| Comando | Função |
|---------|--------|
| `ifconfig` | Determina o endereço IP (e outros dados da interface) |
| `route` | Exibe e manipula a tabela de roteamento |
| `dig` | Consulta DNS, por exemplo, para resolver um nome em endereço IP |
| `netstat` | Executa várias tarefas relacionadas à rede (conexões, portas, rotas) |

```bash
ifconfig
ip addr show               # alternativa moderna ao ifconfig
route -n
dig localhost.localdomain  # resolve o nome em um endereço IP
netstat -tuln              # portas TCP/UDP em escuta, em formato numérico
```

> ℹ️ Em distribuições mais novas, `ifconfig`, `route` e `netstat` foram substituídos por `ip` e `ss`, mas ainda caem em provas e aparecem em muitos sistemas.

---

## Lab 15 — Segurança do sistema e dos usuários

**Objetivos:** acessar a conta root com segurança e consultar informações de usuários e sessões.

| Comando | Função |
|---------|--------|
| `su` | Troca de usuário (`su -` abre um shell de login como root) |
| `sudo` | Executa um comando com privilégios de administrador |
| `getent` | Consulta bases de dados do sistema (ex.: `getent passwd`) |
| `who` | Mostra quem está logado |
| `w` | Mostra quem está logado e o que está fazendo |
| `last` | Exibe o histórico de logins |

```bash
su -
sudo apt update
getent passwd joao
who
w
last
```

> 🔐 **Boa prática:** prefira `sudo` a trabalhar como root o tempo todo. Menos privilégio significa menos risco.

---

## Lab 16 — Criando usuários e grupos

**Objetivos:** administrar contas de usuários e grupos.

| Comando | Função |
|---------|--------|
| `groupadd` | Cria um grupo |
| `groupmod` | Modifica um grupo (`-n` muda o nome, `-g` muda o GID) |
| `groupdel` | Remove um grupo |
| `useradd` | Cria um usuário |
| `usermod` | Modifica um usuário |

```bash
sudo groupadd suporte
sudo groupmod -n helpdesk suporte        # renomeia o grupo
sudo groupmod -g 2000 helpdesk           # altera o GID
sudo useradd -m -G helpdesk maria        # cria usuário com home e grupo secundário
sudo usermod -aG sudo maria              # adiciona a um grupo sem remover dos outros
sudo passwd maria                        # define a senha
sudo groupdel helpdesk
```

---

## Lab 17 — Propriedade e permissões

**Objetivos:** ler e alterar permissões e donos de arquivos.

**Classes:** `u` = usuário (dono), `g` = grupo, `o` = outros, `a` = todos.

**Permissões:**

| Permissão | Letra | Valor octal |
|-----------|-------|-------------|
| Leitura | `r` | 4 |
| Escrita | `w` | 2 |
| Execução | `x` | 1 |

```bash
ls -l arquivo.sh
# -rwxr-xr-- 1 joao suporte 120 nov 2 10:00 arquivo.sh
#  │└┬┘└┬┘└┬┘
#  │ │  │  └── outros (o): r--
#  │ │  └───── grupo (g):  r-x
#  │ └──────── dono (u):   rwx
#  └────────── tipo (- arquivo, d diretório)
```

```bash
chmod u+x script.sh        # adiciona execução para o dono
chmod a+r arquivo.txt      # leitura para todos
chmod 754 script.sh        # rwx r-x r--  (7=4+2+1, 5=4+1, 4)
stat arquivo.txt           # mostra permissões, dono, datas e inode
sudo chown maria arquivo.txt         # altera o dono
sudo chown maria:helpdesk arquivo.txt  # altera dono e grupo
chgrp helpdesk arquivo.txt # altera apenas o grupo
```

---

## Lab 18 — Diretórios e arquivos especiais

**Objetivos:** entender links (atalhos) e arquivos especiais.

| Tipo | Comando | Característica |
|------|---------|----------------|
| **Hard link** (físico) | `ln original.txt link_fisico` | Aponta para o mesmo inode; continua funcionando mesmo se o original for apagado |
| **Soft link** (simbólico) | `ln -s original.txt link_simbolico` | Atalho que aponta para o **caminho**; quebra se o original for removido |

```bash
ln arquivo.txt hardlink.txt
ln -s arquivo.txt softlink.txt
ls -li                     # -i mostra o inode (hard links compartilham o mesmo)
```

---

## 🧰 Cola rápida

```text
Navegação:     pwd · cd · ls
Arquivos:      touch · mkdir · cp · mv · rm · rmdir
Texto:         cat · head · tail · grep · cut · sort · tr
Compactação:   tar · gzip · bzip2 · xz · zip · unzip
Hardware:      lscpu · free · lspci · lsusb · lsmod · fdisk -l
Processos:     ps · top · jobs · kill · killall
Rede:          ifconfig · route · dig · netstat · ping
Usuários:      su · sudo · who · w · last · useradd · groupadd
Permissões:    chmod · chown · chgrp · stat
Links:         ln · ln -s
```

---

## 🎯 O que aprendi

- Usar a linha de comando com segurança e confiança.
- Navegar, criar e organizar arquivos e diretórios.
- Filtrar e processar texto com `grep`, `cut`, `sort`, pipes e redirecionamentos.
- Editar arquivos no `vi` e escrever scripts Bash simples.
- Consultar hardware, processos, rede e logins pelo terminal.
- Gerenciar usuários, grupos e permissões, a base da segurança em Linux.

---

## 📚 Referências

- [Cisco Networking Academy — NDG Linux Essentials](https://www.netacad.com/)
- Páginas do manual do Linux (`man`)

---

## 👤 Autor

**João Pedro Rodrigues Melo**
Estudante de Ciência da Computação | Suporte de TI, Redes e Segurança da Informação

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/joaoprmelo)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/joaoprmelo)

⭐ Se este guia te ajudou, deixe uma estrela no repositório!
