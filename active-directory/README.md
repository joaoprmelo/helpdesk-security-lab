# Onboarding e offboarding de contas no Active Directory

Documento de processo sobre o ciclo de vida de contas de usuário no Active Directory (AD DS): entrada de um colaborador, mudança de departamento, redefinição de senha e saída.

> **Escopo:** este material vem do meu estudo dos módulos de Active Directory do Microsoft Learn e é um **processo de referência**. Não foi executado em um controlador de domínio real (meu computador de estudo tem 4 GB de RAM). Os nomes usados (Contoso, Jane Dow) são fictícios.

## Conceitos usados
| Conceito | Resumo |
|---|---|
| **Conta de usuário** | Objeto com nome de usuário, senha e participação em grupos |
| **Grupo de segurança** | Atribui permissões a recursos. O grupo de distribuição serve só para e-mail |
| **UO (unidade organizacional)** | Contêiner onde se vinculam GPOs e se delega a administração. Os contêineres genéricos (Users e Computers) não aceitam GPO |
| **GPO** | Conjunto de configurações aplicado a usuários e computadores de um site, domínio ou UO |
| **Política de Domínio Padrão** | Define senha, bloqueio de conta e Kerberos para todo o domínio |

## Princípios que guiam o processo
- **Privilégio mínimo:** só o acesso necessário, pelo tempo necessário.
- **Acesso por grupo, não por pessoa:** para mudar o acesso, adiciona-se ou remove-se do grupo.
- **Aprovação registrada:** acesso a pastas e sistemas só com a aprovação do gestor ou do dono do recurso.
- **Nunca pedir nem registrar senhas** em chamados, e-mails ou documentos.
- **Documentar** toda alteração no chamado.

---

## 1. Onboarding: novo colaborador

### Antes de criar
- [ ] Chamado aberto com nome, departamento, gestor e data de início
- [ ] Aprovação do gestor registrada
- [ ] Conferir a **UO** do departamento e os **grupos** padrão da função

### Criar a conta
No **Centro Administrativo do Active Directory** (ADAC), ou em Usuários e Computadores do Active Directory:
- [ ] Criar o usuário **na UO do departamento** (não no contêiner Users)
- [ ] Definir o nome de usuário padrão da empresa
- [ ] Definir uma senha inicial por canal seguro e marcar **"O usuário precisa alterar a senha no próximo login"**
- [ ] Adicionar aos **grupos de segurança** da função
- [ ] Se o colaborador for temporário (estagiário, terceiro), definir **"A conta expira"** com a data final
- [ ] Mover o computador dele para a **UO correta**, para receber as GPOs

### Referência em PowerShell (módulo do Active Directory)
```powershell
# Exemplos de referência, com nomes fictícios
New-ADUser -Name "Jane Dow" -SamAccountName "jdow" -Path "OU=Vendas,DC=contoso,DC=com"
Add-ADGroupMember -Identity "Vendas-Pasta-Leitura" -Members "jdow"
Set-ADAccountExpiration -Identity "jdow" -DateTime "2027-01-31"
Set-ADUser -Identity "jdow" -ChangePasswordAtLogon $true
```

### Depois
- [ ] Confirmar que o usuário consegue entrar e que o acesso funciona
- [ ] Registrar no chamado quem aprovou, o que foi criado e a data

---

## 2. Mudança de departamento
- [ ] Mover o **usuário** (e o computador) para a UO do novo departamento, para as GPOs mudarem junto
- [ ] **Remover dos grupos antigos** e adicionar aos do novo departamento
- [ ] Registrar a aprovação dos dois gestores

---

## 3. Offboarding: saída do colaborador
- [ ] Chamado aberto pelo RH ou pelo gestor, com a data de saída
- [ ] **Desabilitar a conta** ("A conta está desabilitada"). Ela deixa de permitir login, mas continua no diretório
- [ ] Remover dos grupos de acesso, conforme o procedimento da empresa
- [ ] Registrar a alteração para consulta e conformidade
- [ ] A **exclusão** definitiva segue a política de retenção da empresa

Se uma conta for excluída por engano, a **Lixeira do Active Directory** (recurso do ADAC) permite recuperar o objeto.

```powershell
Disable-ADAccount -Identity "jdow"
Remove-ADGroupMember -Identity "Vendas-Pasta-Leitura" -Members "jdow"
```

---

## 4. Esqueci a senha e conta bloqueada
- [ ] **Confirmar a identidade** do usuário pelo procedimento da empresa
- [ ] Redefinir a senha e marcar **"O usuário precisa alterar a senha no próximo login"**
- [ ] Se a conta estiver bloqueada por tentativas, lembrar que a **política de bloqueio** vem da Política de Domínio Padrão
- [ ] Nunca registrar a senha no chamado
- [ ] Se houver suspeita de conta comprometida: **desabilitar, escalar e avisar a segurança**

---

## 5. Problemas comuns

| Situação | O que verificar |
|---|---|
| Computador novo não recebeu as políticas da empresa | Se está em uma **UO** (e não no contêiner Computers) |
| Usuário não acessa uma pasta | Se está no **grupo** certo e se a aprovação foi registrada |
| Mudou de departamento e perdeu acesso | Se foi movido de UO e se os grupos foram atualizados |
| Política nova não aparece | Forçar a atualização com `gpupdate /force` e conferir com `gpresult /r` (comandos comuns do dia a dia) |
| Usuário foi excluído por engano | Recuperar pela Lixeira do Active Directory |

---

## 6. Modelo de registro no chamado
```
Solicitante: [nome]
Aprovador: [nome e data]
Ação: [onboarding / mudança / offboarding / redefinição de senha]
Usuário (fictício): [login]
UO e grupos: [quais]
Observações: [expiração, troca de senha no primeiro login]
Status: [resolvido / escalado]
```

---

## 7. Ligação com o Microsoft 365
Em **ambientes híbridos**, as contas do AD local são sincronizadas com o **Microsoft Entra ID** (Entra Connect Sync ou Cloud Sync). O acesso aos serviços do Microsoft 365, o **MFA** e o **Acesso Condicional** são tratados no Entra ID, e por isso o offboarding também deve considerar o acesso na nuvem.

## Fonte de estudo
Microsoft Learn: Introdução ao AD DS, Criar e gerenciar objetos do Active Directory e Implementar Objetos de Política de Grupo.
