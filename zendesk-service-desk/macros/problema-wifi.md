# Macro: Problema de Wi-Fi

**Descrição:** orienta o usuário com conexão Wi-Fi instável.

**Ações sugeridas:** Categoria = Rede e Wi-Fi.

**Texto da resposta:**

```
Olá, {{ticket.requester.first_name}}!

Sinto muito pela dificuldade com a conexão. Vamos tentar este passo a passo:

1. Desligue o roteador da tomada, aguarde 30 segundos e ligue novamente.
2. No seu dispositivo, vá nas configurações de Wi-Fi, selecione a rede, clique em "Esquecer rede" e conecte de novo digitando a senha.
3. Aproxime-se do roteador para testar se o problema é de sinal.
4. Se possível, teste a conexão em outro dispositivo para sabermos se o problema é só nesse aparelho.

Se depois disso a conexão continuar instável, me responda informando em quais dispositivos o problema acontece e qual mensagem aparece na tela.

Atenciosamente,
{{current_user.first_name}}
Suporte Técnico
```

## Versão curta

```
Olá, {{ticket.requester.first_name}}! Vamos tentar:
1. Desligue o roteador por 30 segundos e ligue de novo.
2. No aparelho, use "Esquecer rede" e reconecte com a senha.
3. Teste em outro dispositivo.
Se continuar, me diga em quais aparelhos ocorre e qual mensagem aparece.
{{current_user.first_name}} - Suporte Técnico
```
