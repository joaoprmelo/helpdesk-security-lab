# Projeto Service Desk com Zendesk

Simulação de um service desk de TI (suporte N1 e N2) feita no Zendesk Support, com configuração da estrutura de atendimento, automações, SLA e atendimento de chamados simulados.

**Autor:** João Pedro Rodrigues Melo
**Ferramenta:** Zendesk Support (conta trial)
**Contexto:** projeto prático de portfólio para vagas de suporte de TI / help desk

---

## 1. Objetivo

Praticar, em uma ferramenta usada no mercado, o ciclo completo de um atendimento de suporte:

- estruturar equipes e níveis de atendimento (N1 e N2);
- organizar chamados por categoria, prioridade e status;
- automatizar notificações e respostas comuns;
- definir metas de SLA;
- atender chamados simulados, incluindo escalonamento.

## 2. Ambiente utilizado

| Papel | Equipamento | Uso |
|---|---|---|
| Agente de suporte | Notebook B | Atendimento no painel do Zendesk |
| Cliente / usuário | Notebook A | Envio de e-mails que viram chamados e recebimento das respostas |

O Notebook A enviava e-mails (via Gmail) ao endereço de suporte da conta, e cada e-mail virava um chamado no Zendesk.

## 3. Estrutura configurada

### 3.1 Grupos
- **Suporte N1:** atendimento inicial e problemas comuns.
- **Suporte N2:** casos escalados (acessos, VPN, segurança).

### 3.2 Campos de chamado
- **Prioridade** (padrão do Zendesk): Baixa, Normal, Alta, Urgente.
- **Tipo** (padrão do Zendesk): Pergunta, Incidente, Problema, Tarefa.
- **Categoria** (campo personalizado, lista suspensa), com tag associada a cada opção:

| Categoria | Tag |
|---|---|
| Conta e senha | `conta_senha` |
| Rede e Wi-Fi | `rede_wifi` |
| Impressora | `impressora` |
| Software e instalação | `software` |
| Acesso e permissões | `acesso` |

### 3.3 Views (filas)
- **Novos:** status Novo.
- **Em andamento:** status Aberto ou Pendente.
- **Urgentes:** prioridade Urgente e status anterior a Resolvido.

Colunas exibidas: ID, assunto, solicitante, prioridade, categoria e status.

### 3.4 Triggers
- **Confirmação de chamado recebido:** envia e-mail ao solicitante quando o chamado é criado.
- **Aviso de chamado resolvido:** envia e-mail ao solicitante quando o status muda para Resolvido.

### 3.5 Macros
- **Redefinir senha**
- **Problema de Wi-Fi**
- **Pedido de mais informações** (deixa o chamado como Pendente)

Os textos completos estão em [`macros/`](./macros) e em [`triggers/`](./triggers).

### 3.6 SLA

| Prioridade | Primeira resposta | Resolução |
|---|---|---|
| Urgente | 1 hora | 4 horas |
| Alta | 4 horas | 8 horas |
| Normal | 8 horas | 24 horas |
| Baixa | 24 horas | 72 horas |

Contagem em horas corridas. Também foi configurada a métrica de atualização periódica (2h, 4h, 8h e 24h, da prioridade mais alta para a mais baixa).

### 3.7 Base de conhecimento
Foram escritos 5 artigos de autoatendimento, disponíveis em [`base-de-conhecimento/`](./base-de-conhecimento):

1. Como redefinir sua senha
2. Como resolver problemas de Wi-Fi
3. Como configurar uma impressora
4. Como instalar e conectar à VPN
5. O que fazer quando o computador está lento

## 4. Chamados simulados

| # | Assunto | Categoria | Prioridade | Grupo | Solução aplicada |
|---|---|---|---|---|---|
| 1 | Esqueci minha senha | Conta e senha | Normal | N1 | Macro de redefinição de senha |
| 2 | Wi-Fi caindo toda hora | Rede e Wi-Fi | Alta | N1 | Macro de Wi-Fi e reinício do roteador |
| 3 | Impressora não imprime | Impressora | Normal | N1 | Reinício do spooler e readição da impressora |
| 4 | Preciso instalar o Zoom | Software e instalação | Normal | N1 | Orientação de download e instalação |
| 5 | Sem acesso à pasta do financeiro | Acesso e permissões | Alta | N2 | Escalado; liberação de permissão com confirmação do gestor |
| 6 | Computador muito lento | Software e instalação | Normal | N1 | Macro de pedido de informações; limpeza de inicialização e espaço em disco |
| 7 | VPN não conecta | Rede e Wi-Fi | Urgente | N2 | Escalado; redefinição das credenciais da VPN |
| 8 | Conta bloqueada após várias tentativas | Conta e senha | Alta | N1 | Desbloqueio da conta e orientação sobre senha |
| 9 | E-mail não envia anexos | Software e instalação | Baixa | N1 | Orientação para compactar o arquivo ou usar link de nuvem |
| 10 | Suspeita de e-mail de phishing | Acesso e permissões | Urgente | N2 | Escalado; troca de senha, revisão de acessos e orientação sobre phishing |

**Status final dos chamados** (13 no total, contando chamados de teste):

| Status | Quantidade |
|---|---|
| Resolvido | 9 |
| Pendente | 1 |
| Aberto | 2 |
| Novo | 1 |

## 5. Demonstração

### Vídeo
Video rapido resolvendo um chamado (demostração da abertura do chamado no e-mail, teste de triggers e aplicação da macro, resolução): **[https://youtu.be/RqNr7Q_u18Y]**


| Item | Imagem |
|---|---|
| Grupos | `![Grupos](prints/grupos.png)` |
| Campo Categoria | `![Categoria](prints/campo-categoria.png)` |
| Views | `![Views](prints/views.png)` |
| Triggers | `![Triggers](prints/triggers.png)` |
| Macros | `![Macros](prints/macros.png)` |
| Política de SLA | `![SLA](prints/sla.png)` |
| Fila com os chamados | `![Fila](prints/fila.png)` |
| Chamado com macro aplicada | `![Macro](prints/chamado-macro.png)` |
| Resposta recebida no e-mail do cliente | `![E-mail](prints/email-cliente.png)` |
| Chamado escalado para N2 | `![N2](prints/chamado-n2.png)` |

## 6. Limitações

- A conta de teste ficou temporariamente sem acesso (o código de verificação de login deixou de chegar), o que atrasou o atendimento dos chamados. O acesso foi liberado depois e o atendimento foi concluído.
- Os relatórios do **Explore** (tempo de resposta, volume por categoria e satisfação) ainda não estavam disponíveis na conta no momento da finalização. Os prints serão adicionados em uma atualização do projeto.
  
## 7. Lições aprendidas

- Contas de teste dependem da verificação de login por e-mail e podem perder o acesso por motivos fora do controle do usuário. Por isso, vale manter a configuração e os textos documentados em arquivos no repositório e ter uma ferramenta alternativa (como o Freshdesk) como plano B.
- Entender a diferença entre campos padrão do Zendesk (Tipo, Prioridade) e campos personalizados (Categoria).
- Usar macros e triggers reduz trabalho repetitivo e padroniza a comunicação com o cliente.
- Documentar escalonamentos com notas internas deixa o histórico do chamado claro para o próximo nível.
- Recursos como SLA, Explore e Guide podem variar conforme o plano da conta.

## 8. Habilidades demonstradas

Atendimento N1/N2, gestão de filas e priorização, escalonamento de chamados, criação de macros e automações, definição de SLA, escrita de documentação para usuários e comunicação clara com o cliente.

## 9. Estrutura do repositório

```
.
├── README.md
├── macros/
├── triggers/
├── base-de-conhecimento/
└── prints/
```
