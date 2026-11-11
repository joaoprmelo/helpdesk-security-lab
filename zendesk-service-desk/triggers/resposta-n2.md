# Trigger: Resposta do N2

- **Condições (atender todas):** Ticket é atualizado, Comentário é público, Grupo é Suporte N2 e Tags contém ao menos uma dessas "escalado_n2"
- **Ação:** Notificar por e-mail o solicitante

**Assunto:**

```
Seu chamado #{{ticket.id}} foi resolvido
```

**Corpo:**

```
Olá, {{ticket.requester.first_name}}! Seu chamado foi marcado como resolvido. Se o problema continuar, é só responder este e-mail que reabrimos o atendimento.
```
