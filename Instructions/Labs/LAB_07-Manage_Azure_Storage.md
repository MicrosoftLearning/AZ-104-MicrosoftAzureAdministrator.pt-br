---
lab:
  title: 'Laboratório: gerenciar o armazenamento do Azure'
  module: Administer Azure Storage
---

# Laboratório: gerenciar o armazenamento do Azure
# Manual do aluno

## Cenário do laboratório

Você precisa avaliar o uso do armazenamento do Azure para armazenar arquivos que residem atualmente em armazenamentos de dados locais. Embora a maioria desses arquivos não seja acessada com frequência, há algumas exceções. Minimizar o custo do armazenamento colocando os arquivos menos acessados em camadas de armazenamento com preços mais baixos. Explorar diferentes mecanismos de proteção que o Armazenamento do Azure oferece, incluindo acesso à rede, autenticação, autorização e replicação. Determinar até que ponto o serviço Arquivos do Azure é adequado para hospedar os compartilhamentos de arquivo locais.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: provisionar o ambiente de laboratório.
+ Tarefa 2: Criar e configurar contas de armazenamento do Azure.
+ Tarefa 3: Gerenciar o armazenamento de blobs.
+ Tarefa 4: Gerenciar a autenticação e a autorização no Armazenamento do Azure.
+ Tarefa 5: Criar e configurar um compartilhamento dos Arquivos do Azure.
+ Tarefa 6: Gerenciar o acesso de rede ao Armazenamento do Azure.

## Tempo estimado: 40 minutos

## Diagrama de arquitetura

![imagem](../media/lab07.png)


### Instruções

## Exercício 1

## Tarefa 1: provisionar o ambiente de laboratório.

Nesta tarefa, você implantará uma máquina virtual do Azure que usará posteriormente neste laboratório.

1. Entre no **[portal do Azure](https://portal.azure.com)**.

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento.

1. Na barra de ferramentas do painel do Cloud Shell, clique no ícone Carregar/Baixar arquivos. No menu suspenso, clique em Carregar e carregue os arquivos template.json e parameters.json a seguir no diretório base do Cloud Shell, um por um.

1. No painel Cloud Shell, execute o seguinte para criar o grupo de recursos que hospedará a máquina virtual (substitua o espaço reservado '[Azure_region]' pelo nome de uma região do Azure onde você pretende implantar a máquina virtual do Azure)

    >**Observação**: para listar os nomes das regiões do Azure, execute**`(Get-AzLocation).Location`
    > Observação**: cada comando abaixo deve ser digitado separadamente

    ```powershell
    $location = '[Azure_region]'
    ```
  
    ```powershell
     $rgName = 'az104-07-rg0'
    ```

    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```
    
1. No painel Cloud Shell, execute o seguinte para implantar a máquina virtual usando os arquivos de modelo e parâmetro carregados:

    >Você será solicitado a fornecer uma senha.

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-07-vm-template.json `
      -TemplateParameterFile $HOME/az104-07-vm-parameters.json `
      -AsJob
   ```

    >**Observação:** prossiga para a próxima etapa sem aguardar a conclusão da implantação.

    >**Nota**: Se você recebeu um erro informando que o tamanho da VM não está disponível, peça ajuda ao seu instrutor e tente estas etapas.
    > 1. Clique no `{}` botão no seu CloudShell, selecione o az104-07-vm-parameters.json** na barra lateral esquerda e anote o **valor do `vmSize` parâmetro.
    > 1. Verifique o local no qual o grupo de recursos 'az104-04-rg1' está implantado. Você pode executar `az group show -n az104-04-rg1 --query location` em seu CloudShell para obtê-lo.
    > 1. Execute `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` em seu CloudShell.
    > 1. Substitua o valor do `vmSize` parâmetro por um dos valores retornados pelo comando que você acabou de executar.
    > 1. Agora reimplante seus modelos executando o `New-AzResourceGroupDeployment` comando novamente. Você pode pressionar o botão para cima algumas vezes, o que traria o último comando executado.

1. Feche o painel do Cloud Shell.

## Tarefa 2: Criar e configurar contas de armazenamento do Azure.

Nesta lição, você criará uma conta de Armazenamento do Windows Azure e um contêiner.

1. No portal do Azure, selecione **+ Criar um recurso** e, em seguida, pesquise e selecione **Conta de armazenamento.**

1. Na guia **Noções básicas** da folha **Criar conta de armazenamento**, defina as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | O nome de um novo grupo de recursos aks-01-RG |
    | Nome da conta de armazenamento | Insira qualquer nome global exclusivo de 3 a 24 caracteres composto por letras e dígitos e começando com uma letra. |
    | Region | o nome de uma região do Azure onde você pode criar uma conta de Armazenamento do Azure  |
    | Desempenho | **Standard** |
    | Redundância | **Armazenamento com redundância geográfica (GRS)** |

1. Clique em **Avançar: >** Avançado, na **guia Avançado** da folha Criar conta** de armazenamento, revise as opções disponíveis, aceite os padrões e clique em **Avançar: >** de **rede.

1. **Na guia Rede** da folha Criar conta** de armazenamento, revise as opções disponíveis, aceite a opção **padrão Habilitar acesso público de todas as redes** e clique em **Avançar: Proteção de **dados >**.

1. **Na guia Proteção** de dados da folha Criar conta** de armazenamento, revise as opções disponíveis, aceite os padrões, clique em Revisar + Criar, aguarde a conclusão do processo de **validação e clique em ****Criar****.

    >Aguarde até a conta de armazenamento ser criado. Isso deverá levar cerca de dois minutos.

1. Na folha de implantação, clique em **Ir para o recurso** para exibir a folha da conta do Armazenamento do Azure.

1. Na folha Conta de armazenamento, na **seção Gerenciamento de dados** , clique em **Redundância** e observe o local secundário. 

1. Selecione **LRS (armazenamento com redundância local)** na lista suspensa **Redundância**. Observe que, neste ponto, a conta de armazenamento tem apenas o local principal.

1. Na folha Conta de armazenamento, na **seção Configurações** , selecione **Configuração**. Defina **a camada de acesso Blob (padrão)** como **Cool** e salve a alteração.

    > **Nota**: A camada de acesso legal é ideal para dados que não são acessados com frequência.

## Tarefa 3: Gerenciar o armazenamento de blobs.

Nesta tarefa, você criará um contêiner de blob e carregará um blob nele.

1. Na folha Conta de armazenamento, na **seção Armazenamento de** dados, clique em **Contêineres**.

1. Na caixa de diálogo **Novo contêiner**, crie um novo contêiner com as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az104-07-contêiner**  |
    | Nível de acesso público | Privado (sem acesso anônimo) |

1. Na lista de contêineres, clique em az104-07-container** e, em seguida, clique em ****Carregar**.

1. Navegue até **\\Allfiles\\Labs 07\\LICENSE** no computador do\\seu laboratório e clique em **Abrir**.

1. **Na folha Carregar blob**, expanda a **seção Avançado** e especifique as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Tipo de blob | **Blob de blocos** |
    | Tamanho do bloco | **4 MB** |
    | Camada de acesso | **Frequente** |
    | Carregar na pasta | **licenças** |

    > **Nota**: A camada de acesso pode ser definida para blobs individuais.

1. Clique em **Carregar**.

    > **Nota**: Observe que o upload criou automaticamente uma subpasta chamada **licenças**.

1. De volta à **folha az104-07-container**, clique em licenças** e, em seguida, clique em ****LICENSE.**

1. **Na folha licenças/LICENÇA**, revise as opções disponíveis.

    > **Nota**: Você tem a opção de baixar o blob, alterar sua camada de acesso (atualmente está definida como Quente **), adquirir uma concessão, o que alteraria seu status de concessão para **Bloqueado** (atualmente está definido como **** Desbloqueado) e proteger o blob de ser modificado ou excluído**, bem como atribuir metadados personalizados (especificando uma chave arbitrária e pares de valores). Você também tem a capacidade de **editar** o arquivo diretamente na interface do portal do Azure, sem baixá-lo primeiro. Você também pode criar instantâneos, bem como gerar um token SAS (você explorará essa opção na próxima tarefa).

## Tarefa 4: Gerenciar a autenticação e a autorização no Armazenamento do Azure.

Nesta tarefa, você configurará a autenticação e a autorização para o Armazenamento do Azure.

1. **Na folha licenças/LICENÇA, na **guia Visão** geral**, clique no **botão Copiar para a área** de transferência ao lado da entrada de **URL**.

1. Abra outra janela do navegador usando o modo InPrivate e navegue até a URL copiada na etapa anterior.

1. Você deve receber uma mensagem formatada em XML informando **ResourceNotFound** ou **PublicAccessNotPermitted**.

    > **Nota**: Isso é esperado, já que o contêiner criado tem o nível de acesso público definido como **Privado (sem acesso anônimo).**

1. Feche a janela do navegador do modo InPrivate, retorne à janela do navegador mostrando a **folha licenças/LICENSE** do contêiner de Armazenamento do Azure e alterne para a **guia Gerar SAS** .

1. **Na guia Gerar SAS** da **folha licenças/LICENÇA**, especifique as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Chave de assinatura | **Chave 1**. |
    | Permissões | **Ler** |
    | Data de início | data de ontem |
    | Hora de início | CURRENT TIME |
    | Data de vencimento | data de amanhã |
    | Hora de expiração | CURRENT TIME |
    | Endereços IP permitidos | deixar em branco |
    

1. Clique em **Gerar token SAS e URL**.

1. Clique no **botão Copiar para a área** de transferência ao lado da entrada URL **SAS** do Blob.

1. Abra outra janela do navegador usando o modo InPrivate e navegue até a URL copiada na etapa anterior.

    > **Nota**: Você deve ser capaz de visualizar o conteúdo do arquivo baixando-o e abrindo-o com o bloco de notas.

    > **Nota**: Isso é esperado, já que agora seu acesso é autorizado com base no token SAS recém-gerado.

    > **Nota**: Salve a URL SAS do blob. Você precisará disso em uma etapa posterior.

1. Feche a janela do navegador do modo InPrivate, retorne à janela do navegador mostrando a folha licenças/LICENSE do contêiner de Armazenamento do Azure e, de lá, navegue de volta para a ****folha az104-07-container**.**

1. Clique no **link Alternar para a Conta** de Usuário do Azure AD ao lado do rótulo do **método** de autenticação.

    > **Observação**: você pode ver um erro ao alterar o método de autenticação (o erro é *"Você não tem permissões para listar os dados usando sua conta de usuário com o Azure AD").* Isso é esperado?  

    > **Nota**: Neste ponto, você não tem permissões para alterar o método de autenticação.

1. **Na folha az104-07-container**, clique em **Controle de acesso (IAM).**

1. **Na guia Verificar acesso**, clique em **Adicionar atribuição de** função.

1. **Na folha Adicionar atribuição** de função, especifique as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Função | **Proprietário de Dados do Blob de Armazenamento** |
    | Atribuir acesso a | **Usuário, grupo ou entidade de serviço** |
    | Membros | O nome da conta de usuário. |

1. Clique em Revisar + Atribuir** e, em seguida, em ****Revisar + atribuir** e retorne à **folha Visão geral** do contêiner az104-07-container** e verifique se você pode alterar o método de autenticação para (Alternar para Conta de Usuário do **Azure AD).

    > Pode levar cerca de 15 minutos para que as alterações entrem em vigor.

## Tarefa 5: Criar e configurar um compartilhamento dos Arquivos do Azure.

Nesta tarefa, você criará e configurará compartilhamentos de Arquivos do Azure.

> **Nota**: Antes de iniciar esta tarefa, verifique se a máquina virtual provisionada na primeira tarefa deste laboratório está em execução.

1. No portal do Azure, navegue de volta para a folha da conta de armazenamento que você criou na primeira tarefa deste laboratório e, na **seção Armazenamento** de dados, clique em **Compartilhamentos de arquivos**.

1. Clique em **+ Compartilhamento de arquivos e, na **guia Noções** básicas, dê um nome ao compartilhamento de** arquivos, **az104-07-share**. Revise as outras configurações nesta guia. 

1. Vá para a guia Backup e verifique se a **opção Habilitar Backup**** não** está **marcada.**

1. Clique em **Revisar e criar** e em **Criar**. Aguarde até que a implantação seja concluída. 

1. Clique no compartilhamento de arquivos recém-criado e observe as informações disponíveis na **folha az104-07-share** .

1. Clique em **Procurar** e observe que não há arquivos ou pastas no novo compartilhamento de arquivos. Clique em **Conectar**.

1. Na folha Conectar****, verifique se a **guia Windows** está selecionada. Abaixo você encontrará um botão com o rótulo **Mostrar Script**. Clique no botão e você encontrará uma caixa de texto cinza com um script, no canto inferior direito dessa caixa passe o mouse sobre o ícone de páginas e clique em **Copiar para a área de transferência**.

1. No portal do Azure, procure e selecione **Máquinas** virtuais e, na lista de máquinas virtuais, clique em **az104-07-vm0**.

1. Na folha az104-07-vm0 **, na **seção Operações**, clique em **Executar comando**.**

1. Na folha de **comando az104-07-vm0 - Executar, clique em **RunPowerShellScript****.

1. **Na folha Executar Script de Comando**, cole o script copiado anteriormente nesta tarefa no painel Script** do **PowerShell e clique em **Executar**.

1. Verifique se todas as tarefas foram concluídas com êxito.

1. Substitua o conteúdo do painel Script** do **PowerShell pelo seguinte script e clique em **Executar**:

   ```powershell
   New-Item -Type Directory -Path 'Z:\az104-07-folder'

   New-Item -Type File -Path 'Z:\az104-07-folder\az-104-07-file.txt'
   ```

1. Verifique se todas as tarefas foram concluídas com êxito.

1. Navegue de volta para a folha az104-07-share \| Procurar** compartilhamento de arquivos, clique em **Atualizar** e verifique se a ****pasta** az104-07 aparece na lista de pastas.

1. Clique em **az104-07-folder** e verifique se **az104-07-file.txt** aparece na lista de arquivos.

## Tarefa 6: Gerenciar o acesso de rede ao Armazenamento do Azure.

Nesta tarefa, você configurará o acesso à rede para o Armazenamento do Azure.

1. No portal do Azure, navegue de volta para a folha da conta de armazenamento criada na primeira tarefa deste laboratório e, na **seção Segurança + Rede, clique em Rede**** e em ****Firewalls e redes** virtuais.

1. Clique na opção Habilitado a **partir de redes virtuais e endereços** IP selecionados e revise as definições de configuração que ficam disponíveis quando essa opção é habilitada.

    > **Observação**: você pode usar essas configurações para configurar a conectividade direta entre máquinas virtuais do Azure em sub-redes designadas de redes virtuais e a conta de armazenamento usando pontos de extremidade de serviço.

1. Clique na caixa **de seleção Adicionar o endereço** IP do cliente e salve a alteração.

1. Abra outra janela do navegador usando o modo InPrivate e navegue até a URL SAS de blob gerada na tarefa anterior.

    > **Nota**: Se você não gravou a URL SAS da tarefa 4, você deve gerar uma nova com a mesma configuração. Use a Tarefa 4, etapas 4 a 6, como um guia para gerar uma nova URL SAS de blob. 

1. Você deve ser apresentado com o conteúdo da **página The MIT License (MIT).**

    > **Nota**: Isso é esperado, já que você está se conectando a partir do endereço IP do cliente.

1. Feche a janela do navegador do modo InPrivate, retorne à janela do navegador mostrando a **folha Rede** da conta de Armazenamento do Azure.

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

1. No painel Cloud Shell, execute o seguinte para tentar baixar o blob LICENSE do **contêiner az104-07-container** da conta de armazenamento (substitua o `[blob SAS URL]` espaço reservado pela URL SAS de blob gerada na tarefa anterior):

   ```powershell
   Invoke-WebRequest -URI '[blob SAS URL]'
   ```
1. Verifique se a tentativa de download falhou.

    > **Nota**: Você deve receber a mensagem informando **AuthorizationFailure: Esta solicitação não está autorizada a executar esta operação**. Isso é esperado, já que você está se conectando a partir do endereço IP atribuído a uma VM do Azure que hospeda a instância do Cloud Shell.

1. Feche o painel do Cloud Shell.

## Limpar os recursos

>Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam muito tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. Você também pode tentar excluir o Grupo de Recursos onde os recursos residem. Esse é um atalho rápido do administrador. Se você tiver dúvidas, fale com seu instrutor.

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >O comando é executado de modo assíncrono (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente depois na mesma sessão do PowerShell, levará alguns minutos antes de o grupo de recursos ser de fato removido.

## Revisão

Neste laboratório, você vai:

- Tarefa 1: provisionar o ambiente de laboratório.
- Tarefa 2: Criar e configurar contas de armazenamento do Azure.
- Gerenciar o armazenamento de blobs
- Tarefa 4: Gerenciar a autenticação e a autorização no Armazenamento do Azure.
- Tarefa 5: Criar e configurar um compartilhamento dos Arquivos do Azure.
- Tarefa 6: Gerenciar o acesso de rede ao Armazenamento do Azure.
