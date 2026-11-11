# Macro: Redefinir senha

**Descrição:** orienta o usuário que esqueceu a senha.

**Ações sugeridas:** Categoria = Conta e senha; Status = Resolvido.

**Texto da resposta:**

```
Olá, {{ticket.requester.first_name}}!

Para redefinir sua senha, acesse a página de login e clique em "Esqueci minha senha". Você receberá um e-mail com o link para criar uma nova senha. Se não chegar em alguns minutos, verifique o spam e me avise.

Por segurança, nunca compartilhe sua senha com ninguém.

Atenciosamente,
{{current_user.first_name}}
Suporte Técnico
```
