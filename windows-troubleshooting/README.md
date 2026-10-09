# Guia de troubleshooting de Windows

Problemas comuns de suporte, com sintoma, causa provável, diagnóstico e solução. Cada entrada foi testada por mim em ambiente próprio, sem dados reais de terceiros.

## Índice
1. [Computador lento ou demorando para ligar](#1-computador-lento-ou-demorando-para-ligar)
2. [Programa travado ou congelado](#2-programa-travado-ou-congelado)
3. [Problema de driver ou dispositivo](#3-problema-de-driver-ou-dispositivo)
4. [Sem acesso à internet](#4-sem-acesso-à-internet)
5. [Arquivos do sistema corrompidos ou travamentos frequentes](#5-arquivos-do-sistema-corrompidos-ou-travamentos-frequentes)
6. [Impressora não imprime](#6-impressora-não-imprime)
7. [Esqueci a senha / ativar a verificação em duas etapas (MFA)](#7-esqueci-a-senha--ativar-a-verificação-em-duas-etapas-mfa)

---

## 1. Computador lento ou demorando para ligar
- **Sintoma:** o Windows demora para iniciar e fica pesado nos primeiros minutos.
- **Causas prováveis:** muitos programas abrindo junto com o sistema, pouca memória livre, disco cheio ou atualizações pendentes.
- **Diagnóstico:**
  1. Abri o Gerenciador de Tarefas (Ctrl+Shift+Esc) > aba **Inicializar** e olhei a coluna **Impacto na inicialização**.
  2. Abri o `msconfig` > aba **Serviços**, marquei "Ocultar todos os serviços da Microsoft" e vi o que é de terceiros.
  3. Conferi o espaço livre em Configurações > Sistema > Armazenamento e se havia atualizações pendentes.
- **Solução:** desativei na aba Inicializar apenas o que reconheci e não era necessário. Reiniciei e comparei o tempo de inicialização.
- **Cuidado:** não desativei serviços ou programas que não conheço.


---

## 2. Programa travado ou congelado
- **Sintoma:** o programa não responde e a janela não fecha.
- **Diagnóstico e solução, em ordem:**
  1. Esperei um pouco, porque às vezes o status "Não está respondendo" passa sozinho.
  2. Ctrl+Shift+Esc > **Processos** > selecionei o programa > **Finalizar tarefa**.
  3. Se não fechar: aba **Detalhes** > **Finalizar árvore de processos**.
  4. Se ainda não fechar: Prompt de Comando com `taskkill /f /im nomedoprograma.exe`.
- **Descobrindo o motivo:** abri o Visualizador de Eventos (`eventvwr.msc`) > **Logs do Windows > Aplicativo** e procurei um evento de **Erro** perto do horário da falha.
- **Em atendimento real:** avisar o usuário de que dados não salvos podem ser perdidos antes de forçar o fechamento.


---

## 3. Problema de driver ou dispositivo
- **Sintoma:** um dispositivo (som, vídeo, Wi-Fi, impressora) não funciona ou apresenta erro depois de uma atualização.
- **Diagnóstico:**
  1. Abri o Gerenciador de Dispositivos (`devmgmt.msc`) e procurei um **triângulo amarelo** ou ícone de erro.
  2. Abri as Propriedades do dispositivo > aba **Geral** para ler o código de erro. 
- **Solução, conforme o caso:**
  - **Atualizar driver:** baixar apenas pelo Windows Update ou pelo site do fabricante.
  - **Reverter driver:** Propriedades > aba **Driver** > Reverter driver, se o problema começou depois de uma atualização.
  - **Desabilitar e habilitar** o dispositivo, ou **desinstalar** e reiniciar para o Windows reinstalar.
- **Cuidado:** não desabilitei nenhum dispositivo que não conheço.


---

## 4. Sem acesso à internet
- **Sintoma:** o Wi-Fi ou o cabo aparece conectado, mas as páginas não abrem.
- **Diagnóstico, em ordem:**
  1. `ipconfig /all` para ver se o computador recebeu IP, gateway e DNS.
  2. `ping 8.8.8.8` para testar a conectividade com a internet.
  3. `ping google.com` para testar o DNS.
- **Solução:**
  - Se o ping para 8.8.8.8 funciona e o de google.com não: `ipconfig /flushdns` e verificar o servidor DNS.
  - Se não recebeu IP: `ipconfig /release` e depois `ipconfig /renew`, e reiniciar o roteador.

---

## 5. Arquivos do sistema corrompidos ou travamentos frequentes
- **Sintoma:** erros ao abrir programas do Windows, telas azuis ou travamentos frequentes.
- **Diagnóstico e solução (Prompt de Comando como administrador):**
  1. `sfc /scannow` para verificar e reparar arquivos do sistema.
  2. `DISM /Online /Cleanup-Image /RestoreHealth` se o passo anterior não resolver.
  3. `chkdsk C: /f` para verificar o disco (pede reinicialização).

---

## 6. Impressora não imprime
- **Sintoma:** o documento é enviado, mas nada sai da impressora.
- **Diagnóstico, em ordem:**
  1. Confirmei que a impressora está ligada, com papel e toner, e conectada (cabo ou rede).
  2. Verifiquei se está definida como **impressora padrão** e se há documentos presos na fila.
  3. Reiniciei o serviço de spooler (Prompt como administrador): `net stop spooler` e depois `net start spooler`.
  4. Conferi no Gerenciador de Dispositivos se o driver está com erro.
- **Solução:** limpar a fila e reiniciar o spooler resolve a maioria dos casos. Se persistir, atualizar ou reinstalar o driver do fabricante.

---

## 7. Esqueci a senha / ativar a verificação em duas etapas (MFA)
- **Sintoma:** o usuário não lembra a senha da conta Microsoft ou quer proteger a conta com MFA.
- **Esqueci a senha:**
  1. Na tela de login, clicar em **Esqueci minha senha**.
  2. Confirmar a identidade por um método já cadastrado (telefone, e-mail alternativo ou aplicativo autenticador).
  3. Criar uma nova senha e testar o login.
- **Ativar o MFA (Microsoft Authenticator):**
  1. Instalar o Microsoft Authenticator no celular.
  2. Na página de informações de segurança da conta, adicionar um método de entrada e escolher o aplicativo autenticador.
  3. Escanear o QR code e confirmar com o código gerado.
  4. Guardar os códigos de recuperação em local seguro.
     
- **Boas práticas de atendimento:**
  - Nunca pedir nem registrar a senha do usuário.
  - Confirmar a identidade antes de qualquer reset.
  - Orientar o usuário a cadastrar mais de um método de verificação.
  - Se o usuário perdeu o celular e não consegue aprovar o MFA, escalar para a equipe de identidade.

---

## Como usar este guia
Cada problema segue a mesma estrutura: sintoma, causa provável, diagnóstico e solução.
