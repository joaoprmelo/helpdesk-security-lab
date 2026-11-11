# Trigger: Confirmação de chamado recebido

- **Condição (atender todas):** Ticket é Criado
- **Ação:** Notificar por e-mail o solicitante

**Assunto:**

```
Recebemos seu chamado #{{ticket.id}}
```

**Corpo:**

```
Olá, {{ticket.requester.first_name}}! Recebemos seu chamado e logo um de nossos técnicos vai atendê-lo. Número do chamado: #{{ticket.id}}.
```
