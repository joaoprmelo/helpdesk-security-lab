# Trigger: Aviso de chamado resolvido

- **Condições (atender todas):** Status é Resolvido; Status foi alterado
- **Ação:** Notificar por e-mail o solicitante

**Assunto:**

```
Seu chamado #{{ticket.id}} foi resolvido
```

**Corpo:**

```
Olá {{ticket.requester.first_name}}!

Seu chamado  #{{ticket.id}} foi marcado como resolvido.

{{ticket.latest_comment_html}}

 Caso o problema persista, é só responder este e-mail que reabrimos o atendimento.

Atenciosamente,
João Pedro 
Suporte N1.
```
