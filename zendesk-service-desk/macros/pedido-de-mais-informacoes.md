# Macro: Pedido de mais informações

**Descrição:** coleta detalhes do problema antes de continuar o atendimento.

**Ações sugeridas:** Status = Pendente (aguardando resposta do cliente).

**Texto da resposta:**

```
Olá, {{ticket.requester.first_name}}!

Recebi seu chamado e, para ajudar mais rápido, preciso de alguns detalhes:

1. O que exatamente está acontecendo? Aparece alguma mensagem de erro? Se sim, qual?
2. Desde quando o problema ocorre?
3. O que você já tentou fazer para resolver?
4. Qual dispositivo e sistema você está usando (por exemplo, Windows 11 ou Android)?
5. Se possível, envie um print da tela com o problema.

Assim que eu receber essas informações, retomo o atendimento.

Atenciosamente,
{{current_user.first_name}}
Suporte Técnico
```

## Versão curta

```
Olá, {{ticket.requester.first_name}}! Para ajudar mais rápido, preciso saber:
1. Qual erro aparece?
2. Desde quando ocorre?
3. O que você já tentou?
4. Qual dispositivo e sistema você usa?
Se puder, envie um print.
{{current_user.first_name}} - Suporte Técnico
```
