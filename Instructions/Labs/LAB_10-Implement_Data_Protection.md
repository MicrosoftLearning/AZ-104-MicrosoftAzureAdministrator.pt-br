---
lab:
  title: 'Laboratório: implementar a proteção de dados'
  module: Administer Data Protection
---

# Laboratório 10 - Backup de máquinas virtuais
# Manual do aluno

## Cenário do laboratório

Você foi encarregado de avaliar o uso dos Serviços de Recuperação do Azure para backup e restauração de arquivos hospedados em máquinas virtuais do Azure e computadores locais. Além disso, você deseja identificar métodos de proteção de dados armazenados no cofre dos Serviços de Recuperação contra perda acidental ou mal-intencionada de dados.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: provisionar o ambiente de laboratório.
+ Tarefa 2: criar um cofre dos Serviços de Recuperação.
+ Tarefa 3: implementar o backup no nível da máquina virtual do Azure.
+ Tarefa 4: implantar o backup de arquivos e pastas.
+ Tarefa 5: executar a recuperação de arquivo usando o agente dos Serviços de Recuperação do Azure.
+ Tarefa 6: executar a recuperação de arquivo usando instantâneos de máquina virtual do Azure (opcional).
+ Tarefa 7: revise a funcionalidade de exclusão temporária dos Serviços de Recuperação do Azure (opcional).

## Tempo estimado: 50 minutos

## Diagrama de arquitetura

![imagem](../media/lab10.png)

### Instruções

## Exercício 1

## Tarefa 1: provisionar o ambiente de laboratório.

Nesta tarefa, você implantará duas máquinas virtuais que serão usadas para testar diferentes cenários de backup.

1. Entre no [portal do Azure](https://portal.azure.com).

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento.

1. Na barra de ferramentas do painel Cloud Shell, clique no **ícone Upload/Download files**, no menu suspenso, clique em **Upload** and upload the **\\files Allfiles Labs 10 az104-10-vms-edge-template.json e **\\Allfiles\\\\Labs 10 az104-10-vms-edge-parameters.json\\**\\**\\\\ no diretório home do Cloud Shell.

1. No painel Cloud Shell, execute o seguinte para criar o grupo de recursos que hospedará as máquinas virtuais (substitua o `[Azure_region]` espaço reservado pelo nome de uma região do Azure onde você pretende implantar máquinas virtuais do Azure). Digite cada linha de comando separadamente e execute-as separadamente:

   ```powershell
   $location = '[Azure_region]'
    ```
    
   ```powershell
   $rgName = 'az104-10-rg0'
    ```
    
   ```powershell
   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. No painel Cloud Shell, execute o seguinte para criar a primeira rede virtual e implantar uma máquina virtual nela usando os arquivos de modelo e parâmetro carregados:
    >Você será solicitado a fornecer uma senha.
    
   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-10-vms-edge-template.json `
      -TemplateParameterFile $HOME/az104-10-vms-edge-parameters.json `
      -AsJob
   ```

1. Minimize o Cloud Shell (mas não feche-o).

    >**Observação:** prossiga para a próxima etapa sem aguardar a conclusão da implantação. A implantação deve levar menos de cinco minutos.

## Tarefa 2: criar um cofre dos Serviços de Recuperação.

Neste guia de início rápido, você criou um cofre dos Serviços de Recuperação.

1. No **portal do Azure**, no painel de navegação esquerdo, clique em **Todos os serviços**, pesquise e selecione os **Cofres dos Serviços de Recuperação**.

1. Na folha Criar cofre** dos **Serviços de Recuperação, especifique as seguintes configurações:

    | Configurações | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | O nome de um novo grupo de recursos **aks-01-RG** |
    | Nome do cofre | **az104-10-rsv1** |
    | Region | O nome de uma região onde você implantou as duas máquinas virtuais na tarefa anterior |

    >**Nota**: Certifique-se de especificar a mesma região na qual você implantou máquinas virtuais na tarefa anterior.

1. Clique em **Examinar + criar** e, quando a verificação de validação for aprovada, clique em **Criar**.

    >Aguarde até que a implantação seja concluída. A implantação deve levar menos de um minuto.

1. Quando a implantação estiver concluída, selecione **Ir para o recurso**.

1. Na folha do cofre dos **Serviços de Recuperação az104-10-rsv1** , na **seção Configurações** , clique em **Propriedades**.

1. Na folha az104-10-rsv1 - Propriedades **, clique no **link Atualizar** em **Rótulo Configuração de Backup**.**

1. Na folha Configuração de Backup **, examine as opções para **Tipo **de replicação** de armazenamento. Deixe a configuração padrão de **Redundância** geográfica no lugar e feche a folha.

    >**Nota**: Esta configuração só pode ser definida se não houver itens de backup existentes.

1. De volta à **folha az104-10-rsv1 - Propriedades**, clique no link Atualizar** no ****rótulo Configurações** de segurança.

1. Na folha Configurações** de Segurança, observe que **Soft **Delete (para carga de trabalho em execução no Azure)** está **Habilitada**.

1. Feche a **folha Configurações** de Segurança e, de volta à folha do cofre dos **Serviços de Recuperação az104-10-rsv1** , clique em **Visão geral**.

## Tarefa 3: implementar o backup no nível da máquina virtual do Azure.

Tarefa 3: implementar o backup no nível da máquina virtual do Azure.

   >**Nota**: Antes de iniciar esta tarefa, certifique-se de que a implantação iniciada na primeira tarefa deste laboratório foi concluída com êxito.

1. Na folha do cofre dos **Serviços de Recuperação az104-10-rsv1**, clique em Visão geral** e em ****+ Backup**.

1. **Na folha Meta de Backup**, especifique as seguintes configurações:

    | Configurações | Valor |
    | --- | --- |
    | Onde a carga de trabalho está sendo executada? | **Azure** |
    | Do que você deseja fazer backup? | **Máquina virtual** |

1. Clique em Meta de Backup na folha Backup.

1. **Na política de Backup, revise as configurações de **DefaultPolicy** e selecione **Criar uma nova política****.

1. Defina uma nova política de backup com as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | ---- | ---- |
    | Nome da política | **az104-10-backup-policy** |
    | Frequência | **Diariamente** |
    | Hora | 12:00 |
    | Fuso horário | Digite o nome do fuso horário. |
    | Reter os instantâneos de recuperação instantânea por | **** 2 Dias(s) |

1. Clique em **OK para criar a política e, na **seção Máquinas Virtuais, selecione **Adicionar****.**

1. Na folha Selecionar máquinas** virtuais, selecione **az-104-10-vm0**, clique em OK** e, de volta à **folha Backup**, clique em ****Habilitar backup**.**

    >**Nota**: Aguarde até que o backup seja habilitado. Isso deverá levar cerca de dois minutos.

1. Navegue de volta para a folha do cofre dos **Serviços de Recuperação az104-10-rsv1**, na seção Itens protegidos, clique em Itens**** de backup e clique **na **entrada da máquina** virtual do **Azure.

1. **Na folha Itens de Backup (Máquina Virtual do Azure),** selecione o **link Exibir detalhes** para **az104-10-vm0** e examine os **valores das entradas Pré-verificação** de Backup e **Status** do Último Backup.

1. **Na folha Item de Backup az104-10-vm0**, clique em Backup agora **, aceite o **valor padrão na lista suspensa Reter Backup até** e clique em ****OK.**

    >**Nota**: Não aguarde a conclusão do backup, mas prossiga para a próxima tarefa.

## Tarefa 4: implantar o backup de arquivos e pastas.

Nesta tarefa, você implementará o backup de arquivos e pastas usando os Serviços de Recuperação do Azure.

1. No portal do Azure, procure e selecione **Máquinas virtuais e, na **folha Máquinas**** virtuais, clique em **az104-10-vm1**.

1. **Na folha az104-10-vm1**, clique em Conectar **, no menu suspenso, clique em RDP, na **folha Conectar com RDP**, clique **** em **Baixar Arquivo** RDP** e siga as instruções para iniciar a sessão da Área de Trabalho Remota.

    >**Nota**: Esta etapa refere-se à conexão via Área de Trabalho Remota a partir de um computador Windows. Em um Mac, você pode usar o Cliente de Área de Trabalho Remota na Mac App Store e, em computadores Linux, pode usar um software cliente RDP de código aberto.

    >**Nota**: Você pode ignorar quaisquer prompts de aviso ao se conectar às máquinas virtuais de destino.

1. Quando solicitado, entre usando o nome de usuário Estudante **** e a senha do arquivo de parâmetros.

    >**Observação:** como o portal do Azure não oferece mais suporte ao IE11, você precisará usar o Navegador Microsoft Edge para essa tarefa.

1. Na sessão de Área de Trabalho Remota para a **máquina virtual do Azure az104-10-vm1**, inicie um navegador da Web de Borda, navegue até o portal[ do ](https://portal.azure.com)Azure e entre usando suas credenciais.

1. No **portal do Azure**, no painel de navegação esquerdo, clique em **Todos os serviços**, pesquise e selecione os **Cofres dos Serviços de Recuperação**.

1. Na folha **Cofre dos serviços de recuperação**, selecione **Backup**.

1. **Na folha Meta de Backup**, especifique as seguintes configurações:

    | Configurações | Valor |
    | --- | --- |
    | Onde a carga de trabalho está sendo executada? | **No local** |
    | Do que você deseja fazer backup? | Arquivos e pastas |

    >**Observação**: mesmo que a máquina virtual que você está usando nesta tarefa esteja sendo executada no Azure, você pode aproveitá-la para avaliar os recursos de backup aplicáveis a qualquer computador local que esteja executando o sistema operacional Windows Server.

1. Na folha **Preparar infraestrutura**, clique em **Baixar**.

1. Na folha **Preparar infraestrutura**, clique em **Baixar agente do Windows Server ou Windows Client**.

1. Quando solicitado, clique em **Executar** para iniciar a instalação do **MARSAgentInstaller.exe** com as configurações padrão.

    >**Observação**: na **página Aceitação** do Microsoft Update do Assistente de Instalação do **Agente de Serviços de Recuperação** do Microsoft Azure, selecione a opção de instalação Não desejo usar o **Microsoft Update** .

1. **Na página Instalação do Assistente de Instalação** do Agente de Serviços de Recuperação** do **Microsoft Azure, clique em **Continuar para Registro**. Isso iniciará o **Assistente** para Registrar Servidor.

1. Alterne para a janela do navegador da Web que exibe o portal do Azure, na **folha Preparar infraestrutura** , marque a caixa **de seleção Já baixado ou usando o Agente do Servidor de Recuperação** mais recente e clique em **Baixar**.

1. Quando solicitado, seja para abrir ou salvar o arquivo de credenciais do vault, clique em **Salvar**. Isso salvará o arquivo de credenciais do vault na pasta Downloads local.

1. Volte para a janela Assistente para **Registrar Servidor** e, na página Identificação** do **Vault, clique em **Procurar**.

1. Na caixa de diálogo Selecionar Credenciais** do **Vault, navegue até a **pasta Downloads**, clique no arquivo de credenciais do vault que você baixou e clique em **Abrir**.

1. De volta à página Identificação do **cofre, clique em **Avançar****.

1. Verifique se **a opção Salvar senha com segurança no Cofre de Chaves** do Azure não está marcada. 

1. **Na página Configuração de Criptografia** do Assistente para **Registrar Servidor**, clique em **Gerar Senha**.

1. **Na página Configuração de Criptografia** do Assistente para Registrar Servidor **, clique no **botão Procurar** ao lado de Digite **um local para **salvar a senha**.

1. **Na caixa de diálogo Procurar pasta, selecione a **pasta** Documentos** e clique em **OK.**

1. Clique em Concluir **, revise o aviso do Backup do** **Microsoft Azure e clique em ****Sim** e aguarde a conclusão do registro.

    >**Nota**: Em um ambiente de produção, você deve armazenar o arquivo de senha em um local seguro diferente do servidor cujo backup está sendo feito.

1. **Na página Registro** do Servidor do Assistente para Registrar Servidor **, revise o aviso sobre o local do arquivo de senha, verifique se a **caixa de seleção Iniciar Agente de **Serviços de Recuperação** do Microsoft Azure está marcada e clique em **Fechar**. Isso abrirá automaticamente o console do Backup** do **Microsoft Azure.

1. No console do Backup do **Microsoft Azure, no **painel Ações**, clique em **Agendar Backup****.

1. Na página de Introdução do Assistente de Agendamento de Backup, clique em Avançar.

1. Na tela **Selecionar Itens para Backup**, clique em **Adicionar Itens**.

1. Na caixa de diálogo Selecionar Itens **, expanda **** C:\\Windows\\System32\\drivers\\etc**\\, selecione **hosts** e clique em **OK:**

1. Na página **Selecionar Itens para Backup**, clique em **Próximo**.

1. Na página Especificar Agendamento **de Backup** , verifique se a **opção Dia** está selecionada, na primeira caixa de listagem suspensa abaixo da **caixa Nos seguintes horários (O máximo permitido é três vezes ao dia),** selecione **4:30 AM** e clique em **Avançar**.

1. **Na página Selecionar Política** de Retenção, aceite os padrões e clique em **Avançar**.

1. **Na página Escolher Tipo** de Backup Inicial, aceite os padrões e clique em **Avançar**.

1. Na página **Confirmação** , clique em **Concluir**. Quando o agendamento de backup for criado, clique em **Fechar**.

1. No console do Backup do **Microsoft Azure, no painel Ações, clique em **Fazer Backup Agora**.**

    >**Nota**: A opção de executar backup sob demanda fica disponível assim que você cria um backup agendado.

1. No Assistente de Backup Agora, na **página Selecionar Item de Backup**, verifique se a **opção Arquivos e Pastas está selecionada e clique em **Avançar****.

1. **Na página Reter Backup Till**, aceite a configuração padrão e clique em **Avançar**.

1. Na página de confirmação, clique em Adicionar.

1. Quando o backup estiver concluído, clique em **Fechar** e feche o Backup do Microsoft Azure.

1. Alterne para a janela do navegador da Web que exibe o portal do Azure, navegue de volta para a folha do cofre dos **Serviços de Recuperação, na seção Itens protegidos, e clique em ****Itens** de backup**.**

1. **Na folha az104-10-rsv1 - Itens** de backup, clique em **Agente de Backup** do Azure.

1. **Na folha Itens de Backup (Agente de Backup do Azure),** verifique se há uma entrada fazendo referência à **unidade C:\\** de **az104-10-vm1..**

## Tarefa 5: executar a recuperação de arquivo usando o agente dos Serviços de Recuperação do Azure.

Tarefa 5: executar a recuperação de arquivo usando o agente dos Serviços de Recuperação do Azure.

1. Dentro da sessão de Área de Trabalho Remota para az104-10-vm1, abra o Explorador de Arquivos, navegue até a pasta C:\\Windows\\System32\\drivers\\etc**\\ e exclua o **** arquivo hosts**.****

1. Abra o Backup do Microsoft Azure e clique em **Recuperar dados** no **painel Ações** . Isso iniciará o **Assistente de Recuperação de Dados**.

1. Na página Introdução** do Assistente para Recuperação de **Dados**, verifique se **a **opção Este servidor (az104-10-vm1.)** estiver selecionada e clique em **Avançar**.

1. Na página **Selecionar Modo de Recuperação**, selecione **Arquivos e pastas individuais** e depois **Avançar**.

1. **Na página Selecionar Volume e Data**, na **lista suspensa Selecionar o volume**, selecione **C:\\**, aceite a seleção padrão do backup disponível e clique em **Montar**.

    >Aguarde a conclusão da operação. Isso pode levar cerca de 10 minutos.

1. **Na página Procurar e recuperar arquivos**, observe a letra da unidade do volume de recuperação e revise a dica sobre o uso da robocopy.

1. Clique em Iniciar **, expanda a pasta Sistema** do **Windows e clique em ****Prompt de Comando**.

1. No prompt de comando, execute o seguinte para copiar a restauração do arquivo hosts** para o local original (substitua `[recovery_volume]` pela letra da unidade do **volume de recuperação identificado anteriormente):

   ```sh
   robocopy [recovery_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. Volte para o **Assistente para Recuperação de Dados** e, em **Procurar e Recuperar Arquivos**, clique em Desmontar** e, quando solicitado a confirmar, clique em ****Sim**.

1. Navegar pela sessão de Área de Trabalho Remota

## Tarefa 6: executar a recuperação de arquivo usando instantâneos de máquina virtual do Azure (opcional).

Nesta tarefa, você restaurará um arquivo do backup baseado em instantâneo no nível da máquina virtual do Azure.

1. Em seu computador, alterne para a janela do navegador que está exibindo o portal do Azure.

1. No portal do Azure, procure e selecione **Máquinas virtuais e, na **folha Máquinas**** virtuais, clique em **az104-10-vm0**.

1. **Na folha az104-10-vm0**, clique em Conectar **, no menu suspenso, clique em RDP, na **folha Conectar com RDP**, clique **** em **Baixar Arquivo** RDP** e siga as instruções para iniciar a sessão de Área de Trabalho Remota.

    >**Nota**: Esta etapa refere-se à conexão via Área de Trabalho Remota a partir de um computador Windows. Em um Mac, você pode usar o Cliente de Área de Trabalho Remota na Mac App Store e, em computadores Linux, pode usar um software cliente RDP de código aberto.

    >**Nota**: Você pode ignorar quaisquer prompts de aviso ao se conectar às máquinas virtuais de destino.

1. Quando solicitado, entre usando o nome de usuário Estudante **** e a senha do arquivo de parâmetros.

   >**Observação:** como o portal do Azure não oferece mais suporte ao IE11, você precisará usar o Navegador Microsoft Edge para essa tarefa.

1. Na sessão de Área de Trabalho Remota para o az104-10-vm0 **, clique em Iniciar **, expanda a pasta Sistema** do **Windows e clique em ****Prompt de Comando**.**

1. No prompt de comando, execute o seguinte para excluir o **arquivo hosts** :

   ```sh
   del C:\Windows\system32\drivers\etc\hosts
   ```

   >**Observação**: você restaurará esse arquivo do backup baseado em instantâneo no nível da máquina virtual do Azure posteriormente nesta tarefa.

1. Na sessão de Área de Trabalho Remota para a **máquina virtual do Azure az104-10-vm0**, inicie um navegador da Web de Borda, navegue até o portal[ do ](https://portal.azure.com)Azure e entre usando suas credenciais.

1. No **portal do Azure**, no painel de navegação esquerdo, clique em **Todos os serviços**, pesquise e selecione os **Cofres dos Serviços de Recuperação**.

1. Na folha do cofre dos **Serviços de Recuperação az104-10-rsv1**, na **seção Itens protegidos, clique em **Itens** de backup**.

1. No bloco **Itens de Backup**, clique em **Máquinas Virtuais do Azure**.

1. Na folha Itens de **Backup (Máquina Virtual do Azure),** selecione **Exibir detalhes** de **az104-10-vm0**.

1. Na folha Item de Backup, clique em Recuperação de Arquivo

    >**Nota**: Você tem a opção de executar a recuperação logo após o início do backup com base no instantâneo consistente do aplicativo.

1. Na folha Recuperação** de Arquivos, aceite o ponto de **recuperação padrão e clique em **Baixar Executável**.

    >**Nota**: O script monta os discos do ponto de recuperação selecionado como unidades locais dentro do sistema operacional a partir do qual o script é executado.

1. Clique em Download** e, quando solicitado a executar ou salvar **o IaaSVMILRExeForWindows.exe**, clique em ****Salvar**.

1. De volta à janela do Explorador de Arquivos, clique duas vezes no arquivo recém-baixado.

1. Quando solicitado a fornecer a senha do portal, copie a **senha da caixa de texto Senha para executar o script** na **folha Recuperação** de Arquivos, cole-a no Prompt de Comando e pressione **Enter**.

    >**Observação**: isso abrirá uma janela do Windows PowerShell exibindo o progresso da montagem.

    >**Nota**: Se você receber uma mensagem de erro neste momento, atualize a janela do navegador da Web e repita as últimas três etapas.

1. Aguarde a conclusão do processo de montagem, revise as mensagens informativas na janela do Windows PowerShell, anote a letra da unidade atribuída ao volume que hospeda **o Windows** e inicie o Explorador de Arquivos.

1. No Explorador de Arquivos, navegue até a letra da unidade que hospeda o instantâneo do volume do sistema operacional identificado na etapa anterior e revise seu conteúdo.

1. Mude para a janela Comando.

1. No prompt de comando, execute o seguinte para copiar a restauração do arquivo hosts** para o local original (substitua `[os_volume]` pela letra da unidade do volume do **sistema operacional que você identificou anteriormente):

   ```sh
   robocopy [os_volume]:\Windows\System32\drivers\etc C:\Windows\system32\drivers\etc hosts /r:1 /w:1
   ```

1. No Portal do Azure, na folha **Recuperação de Arquivos (Versão Prévia)**, clique em **Desmontar Discos**.

1. Navegar pela sessão de Área de Trabalho Remota

## Tarefa 7: revise a funcionalidade de exclusão temporária dos Serviços de Recuperação do Azure (opcional).

1. No computador do laboratório, no portal do Azure, procure e selecione **cofres dos Serviços de Recuperação e, nos cofres** dos **Serviços de** Recuperação, clique em **az104-10-rsv1**.

1. Na folha do cofre dos **Serviços de Recuperação az104-10-rsv1**, na **seção Itens protegidos, clique em **Itens** de backup**.

1. **Na folha az104-10-rsv1 - Itens** de backup, clique em **Agente de Backup** do Azure.

1. **Na folha Itens de Backup (Agente de Backup do Azure),** clique na entrada que representa o backup de **az104-10-vm1**.

1. **No C:\\ em az104-10-vm1.** blade, selecione **Exibir detalhes** para **az104-10-vm1.** .

1. Na folha Detalhe, clique em **az104-10-vm1**.

1. **No az104-10-vm1.** folha Servidores Protegidos, clique em **Excluir**.

1. Nos detalhes da tarefa, especifique as seguintes configurações:

    | Configurações | Valor |
    | --- | --- |
    | DIGITE O NOME DO SERVIDOR | az104-04-vm1 |
    | Motivo | **Reciclando o servidor de desenvolvimento/teste** |
    | Comentários | **Laboratórios do AZ-500 ** |

    >**Nota**: Certifique-se de incluir o período final ao digitar o nome do servidor

1. Habilite a caixa de seleção ao lado do rótulo **Há dados de backup de 1 itens de backup associados a este servidor. Eu entendo que clicar em "Confirmar" excluirá permanentemente todos os dados de backup na nuvem. Essa ação não pode ser desfeita. Um alerta pode ser enviado aos administradores desta assinatura notificando-os dessa exclusão** e clique em **Excluir**.

    >**Nota**: Isso falhará porque o **recurso de exclusão** flexível deve ser desativado.

1. Navegue de volta para a **folha az104-10-rsv1 - Itens** de backup e clique em **Máquinas Virtuais** do Azure.

1. No bloco **Itens de Backup**, clique em **Máquinas Virtuais do Azure**.

1. Na folha Itens de **Backup (Máquina Virtual do Azure),** selecione **Exibir detalhes** de **az104-10-vm0**.

1. **Na folha Item de Backup az104-10-vm0**, clique em **Parar backup**.

1. **Na folha Parar backup, selecione **Excluir dados** de backup, especifique as seguintes configurações e clique em **Parar backup****:

    | Configurações | Valor |
    | --- | --- |
    | Digite o nome do item de Backup | az104-04-vm0 |
    | Motivo | **Others** |
    | Comentários | **Laboratórios do AZ-500 ** |

1. Navegue de volta para a **folha az104-10-rsv1 - Itens** de backup e clique em **Atualizar**.

    >**Observação**: a **entrada da Máquina** Virtual do Azure ainda lista **1** item de backup.

1. Clique na entrada Máquina Virtual do Azure e, na ****folha Itens de Backup (Máquina** Virtual do Azure),** clique na **entrada az104-10-vm0**.

1. Na folha Item de Backup az104-10-vm0****, observe que você tem a opção de **Cancelar a exclusão** do backup excluído.

    >**Observação**: essa funcionalidade é fornecida pelo recurso de exclusão flexível, que está, por padrão, habilitado para backups de máquina virtual do Azure.

1. Navegue de volta para a folha do cofre dos **Serviços de Recuperação az104-10-rsv1** e, na **seção Configurações** , clique em **Propriedades**.

1. **Na folha az104-10-rsv1 - Propriedades**, clique no **link Atualizar** em **Rótulo Configurações** de segurança.

1. **Na folha Configurações** de Segurança, desabilite a exclusão flexível (para cargas de trabalho em execução no Azure) e também desabilite ****Recursos de segurança (para cargas de trabalho em execução local)**** e clique em **Salvar**.

    >**Observação**: isso não afetará os itens que já estão no estado de exclusão flexível.

1. Feche a **folha Configurações** de Segurança e, de volta à folha do cofre dos **Serviços de Recuperação az104-10-rsv1** , clique em **Visão geral**.

1. Navegue de volta para a **folha Item de Backup az104-10-vm0** e clique em **Cancelar exclusão**.

1. **Na folha Undelete az104-10-vm0**, clique em **Undelete**.

1. Aguarde a conclusão da operação de undelete, atualize a página do navegador da Web, se necessário, navegue de volta para a **folha Item de Backup az104-10-vm0** e clique em **Excluir dados** de backup.

1. Na folha Excluir Dados** de **Backup, especifique as seguintes configurações e clique em **Excluir**:

    | Configurações | Valor |
    | --- | --- |
    | Digite o nome do item de Backup | az104-04-vm0 |
    | Motivo | **Others** |
    | Comentários | **Laboratórios do AZ-500 ** |

1. Repita as etapas no início desta tarefa para excluir os itens de backup para **az104-10-vm1**.

## Limpar os recursos

>Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-10*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-10*' | Remove-AzResourceGroup -Force -AsJob
   ```

   >**Nota**: Opcionalmente, você pode considerar excluir o grupo de recursos gerado automaticamente com o prefixo **AzureBackupRG_** (não há cobrança adicional associada à sua existência).

    >O comando é executado de modo assíncrono (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente depois na mesma sessão do PowerShell, levará alguns minutos antes de o grupo de recursos ser de fato removido.

## Revisão

Neste laboratório, você vai:

+ Tarefa 1: provisionar o ambiente de laboratório.
+ Para criar um cofre de Serviços de Recuperação
+ Tarefa 3: implementar o backup no nível da máquina virtual do Azure.
+ Tarefa 4: implantar o backup de arquivos e pastas.
+ Tarefa 5: executar a recuperação de arquivo usando o agente dos Serviços de Recuperação do Azure.
+ Tarefa 6: executar a recuperação de arquivo usando instantâneos de máquina virtual do Azure (opcional).
+ Tarefa 7: revise a funcionalidade de exclusão temporária dos Serviços de Recuperação do Azure (opcional).
