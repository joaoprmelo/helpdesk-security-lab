# Trigger: Notificar escalonamento N1 para N2

- **Condição (atender todas):** Ticket alterado para Suporte N2 e Ticket menor que resolvido
- **Ação:** Notificar por e-mail o solicitante

**Assunto:**

```
Seu chamado #{{ticket.id}} foi escalado
```

**Corpo:**

```
Olá {{ticket.requester.first_name}},

Seu chamado #{{ticket.id}} foi encaminhado para a equipe de Suporte N2, que vai continuar o atendimento.

Motivo do encaminhamento:
{{ticket.latest_comment_html}}

Não é necessário fazer nada agora. Retornaremos assim que houver novidades.

Atenciosamente,
João Pedro
Suporte N1
```
