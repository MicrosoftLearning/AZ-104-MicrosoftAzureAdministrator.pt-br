---
lab:
  title: 'Laboratório: implementar conectividade entre sites'
  module: Administer Intersite Connectivity
---

# Laboratório: implementar conectividade entre sites
# Manual do aluno

## Cenário do laboratório

A Contoso tem seus datacenters em Boston, Nova York e escritórios de Seattle conectados por meio de links de rede de área ampla de malha, com conectividade total entre eles. Você precisa implementar um ambiente de laboratório que reflita a topologia das redes locais da Contoso e verificar sua funcionalidade.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: provisionar o ambiente de laboratório.
+ Tarefa 2: configurar o emparelhamento de rede virtual local e global.
+ Tarefa 3: Testar a conectividade entre sites

## Tempo estimado: 30 minutos

## Diagrama de arquitetura

![imagem](../media/lab05.png)

### Instruções

## Exercício 1

## Tarefa 1: provisionar o ambiente de laboratório.

Nesta tarefa, você implantará três máquinas virtuais, cada uma em uma rede virtual separada, com duas delas na mesma região do Azure e a terceira em outra região do Azure.

1. Entre no [portal do Azure](https://portal.azure.com).

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento.

1. Na barra de ferramentas do painel Cloud Shell, clique no ícone Upload/Download files, no menu suspenso, clique **em **Upload** and upload the\\** files Allfiles Labs 05 az104-05-vnetvm-loop-template.json e\\** Allfiles\\\\Labs\\\\05 az104-05-vnetvm-loop-parameters.json**\\\\** no diretório home do Cloud Shell.** 

1. No painel Cloud Shell, execute o seguinte para criar o grupo de recursos que hospedará o ambiente de laboratório. As duas primeiras redes virtuais e um par de máquinas virtuais serão implantadas em [Azure_region_1]. A terceira rede virtual e a terceira máquina virtual serão implantadas no mesmo grupo de recursos, mas em outro [Azure_region_2]. (substitua o espaço reservado [Azure_region_1] e [Azure_region_2], incluindo os colchetes, pelos nomes de duas regiões diferentes do Azure onde você pretende implantar essas máquinas virtuais do Azure. Um exemplo é $location 1 = 'eastus'. Você pode usar Get-AzLocation para listar todos os locais.):

   ```powershell
   $location1 = 'eastus'

   $location2 = 'westus'

   $rgName = 'az104-05-rg1'

   New-AzResourceGroup -Name $rgName -Location $location1
   ```

   >**Nota**: As regiões usadas acima foram testadas e conhecidas por funcionarem quando este laboratório foi revisado oficialmente pela última vez. Se você preferir usar locais diferentes ou eles não funcionarem mais, será necessário identificar duas regiões diferentes nas quais as máquinas virtuais Standard D2Sv3 podem ser implantadas.
   >
   >Para identificar regiões do Azure, a partir de uma sessão do PowerShell no Cloud Shell, execute **(Get-AzLocation). Localização**
   >
   >Depois de identificar duas regiões que você gostaria de usar, execute o comando abaixo no Cloud Shell para cada região para confirmar que você pode implantar máquinas virtuais D2Sv3 padrão
   >
   >```az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name" ```
   >
   >Se o comando não retornar nenhum resultado, você precisará escolher outra região. Depois de identificar duas regiões adequadas, você pode ajustar as regiões no bloco de código acima.

1. No painel Cloud Shell, execute o seguinte para criar as três redes virtuais e implantar máquinas virtuais nelas usando os arquivos de modelo e parâmetro carregados:
    
    >Você será solicitado a fornecer uma senha.

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-05-vnetvm-loop-template.json `
      -TemplateParameterFile $HOME/az104-05-vnetvm-loop-parameters.json `
      -location1 $location1 `
      -location2 $location2
   ```

    >Aguarde a conclusão da implantação antes de prosseguir para a próxima etapa. Isso deverá levar cerca de dois minutos.

1. Feche o painel do Cloud Shell.

## Tarefa 2: configurar o emparelhamento de rede virtual local e global.

Nesta tarefa, você configurará o emparelhamento local entre as redes virtuais implantadas nas tarefas anteriores para criar uma topologia de rede hub e spoke.

1. No portal do Azure, pesquise e selecione **Redes virtuais**.

1. Examine as redes virtuais criadas na tarefa anterior e verifique se as duas primeiras estão localizadas na mesma região do Azure e a terceira em uma região diferente do Azure.

    >**Nota**: O modelo usado para a implantação das três redes virtuais garante que os intervalos de endereços IP das três redes virtuais não se sobreponham.

1. Na lista de redes virtuais, clique em **az104-05-vnet0**.

1. Na folha da **rede virtual az104-05-vnet0** , na **seção Configurações** , clique em **Emparelhamentos** e em **+ Adicionar**.

1. Adicione um emparelhamento com as seguintes configurações (deixe outras pessoas com seus valores padrão) e clique em **Adicionar**:

    | Configuração | Valor|
    | --- | --- |
    | Esta rede virtual: nome do link do Emparelhamento | **az104-05-vnet0_to_az104-05-vnet1** |
    | Configurações para permitir acesso, tráfego encaminhado e gateway | **Verifique se apenas as três primeiras caixas estão marcadas** |
    | Rede virtual remota: nome do link do Emparelhamento | **az104-05-vnet1_to_az104-05-vnet0** |
    | Modelo de implantação de rede virtual | **Gerenciador de recursos** |
    | Conheço minha ID do recurso | não selecionado |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Rede virtual | **az104-05-VNET1** |
    | Permitir o acesso à rede virtual atual |  Verifique se a caixa Pesquisável está marcada (padrão). |
    | Configurações para permitir acesso, tráfego encaminhado e gateway | **Verifique se apenas as três primeiras caixas estão marcadas** |

    >**Nota**: Esta etapa estabelece dois emparelhamentos locais - um de az104-05-vnet0 a az104-05-vnet1 e o outro de az104-05-vnet1 a az104-05-vnet0.

    >**Observação**: caso você tenha um problema com a interface do portal do Azure que não exibe as redes virtuais criadas na tarefa anterior, você pode configurar o emparelhamento executando os seguintes comandos do PowerShell no Cloud Shell:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet1' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet1.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet0' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Na folha da **rede virtual az104-05-vnet0** , na **seção Configurações** , clique em **Emparelhamentos** e em **+ Adicionar**.

1. Adicione um emparelhamento com as seguintes configurações (deixe outras pessoas com seus valores padrão) e clique em **Adicionar**:

    | Configuração | Valor|
    | --- | --- |
    | Esta rede virtual: nome do link do Emparelhamento | **az104-05-vnet0_to_az104-05-vnet2** |
    | Permitir acesso à rede virtual remota |Verifique se a caixa Pesquisável está marcada (padrão). |
    | Rede virtual remota: nome do link do Emparelhamento | **az104-05-vnet2_to_az104-05-vnet0** |
    | Modelo de implantação de rede virtual | **Gerenciador de recursos** |
    | Conheço minha ID do recurso | não selecionado |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Rede virtual | **az104-05-VNET2** |
    | Permitir o acesso à rede virtual atual |Verifique se a caixa Pesquisável está marcada (padrão). |

    >**Nota**: Esta etapa estabelece dois emparelhamentos globais - um de az104-05-vnet0 a az104-05-vnet2 e o outro de az104-05-vnet2 a az104-05-vnet0.

    >**Observação**: caso você tenha um problema com a interface do portal do Azure que não exibe as redes virtuais criadas na tarefa anterior, você pode configurar o emparelhamento executando os seguintes comandos do PowerShell no Cloud Shell:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet2' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet0' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Navegue de volta para a **folha Redes** virtuais e, na lista de redes virtuais, clique em **az104-05-vnet1**.

1. Na folha da **rede virtual az104-05-vnet1** , na **seção Configurações** , clique em **Emparelhamentos** e em **+ Adicionar**.

1. Adicione um emparelhamento com as seguintes configurações (deixe outras pessoas com seus valores padrão) e clique em **Adicionar**:

    | Configuração | Valor|
    | --- | --- |
    | Esta rede virtual: nome do link do Emparelhamento | **az104-05-vnet1_to_az104-05-VNET2** |
    | Permitir acesso à rede virtual remota | Verifique se a caixa Pesquisável está marcada (padrão). |
    | Rede virtual remota: nome do link do Emparelhamento | **az104-05-vnet2_to_az104-05-vnet1** |
    | Modelo de implantação de rede virtual | **Gerenciador de recursos** |
    | Conheço minha ID do recurso | não selecionado |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Rede virtual | **az104-05-VNET2** |
    | Permitir o acesso à rede virtual atual | Verifique se a caixa Pesquisável está marcada (padrão). |

    >**Nota**: Esta etapa estabelece dois emparelhamentos globais - um de az104-05-vnet1 para az104-05-vnet2 e o outro de az104-05-vnet2 para az104-05-vnet1.

    >**Observação**: caso você tenha um problema com a interface do portal do Azure que não exibe as redes virtuais criadas na tarefa anterior, você pode configurar o emparelhamento executando os seguintes comandos do PowerShell no Cloud Shell:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   ``` 

## Tarefa 3: Testar a conectividade entre sites

Nesta tarefa, você testará a conectividade entre máquinas virtuais nas três redes virtuais conectadas por meio de emparelhamento local e global na tarefa anterior.

1. No portal do Azure, procure e selecione **Máquinas virtuais**.

1. Na lista de máquinas virtuais, clique em **az104-05-vm0**.

1. **Na folha az104-05-vm0,** clique em Conectar **, no menu suspenso, clique em RDP, na **folha Conectar com RDP**, clique ****em **Baixar Arquivo** RDP** e siga as instruções para iniciar a sessão de Área de Trabalho Remota.

    >**Nota**: Esta etapa refere-se à conexão via Área de Trabalho Remota a partir de um computador Windows. Em um Mac, você pode usar o Cliente de Área de Trabalho Remota na Mac App Store e, em computadores Linux, pode usar um software cliente RDP de código aberto.

    >**Nota**: Você pode ignorar quaisquer prompts de aviso ao se conectar às máquinas virtuais de destino.

1. Quando solicitado, entre usando o nome de usuário Estudante **** e a senha que você configurou ao implantar suas máquinas virtuais por meio do CloudShell. 

1. Na sessão de Área de Trabalho Remota para **az104-05-vm0**, clique com o botão direito do mouse no botão Iniciar** e, no menu do botão direito do **mouse, clique em **Windows PowerShell (Admin).**

1. Na janela do console do Windows PowerShell, execute o seguinte para testar a conectividade com **az104-05-vm1 (que tem o endereço IP privado 10.51.0.4******) pela porta TCP 3389:

   ```powershell
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Nota**: O teste usa TCP 3389, uma vez que esta porta é permitida por padrão pelo firewall do sistema operacional.

1. Examine a saída do comando e verifique se a conexão foi bem-sucedida.

1. Na janela do console do Windows PowerShell, execute o seguinte para testar a conectividade com **az104-05-vm2 (que tem o endereço IP privado 10.52.0.4******):

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

1. Alterne de volta para o portal do Azure no computador de laboratório e navegue de volta para a folha Máquinas **** virtuais.

1. Na lista de máquinas virtuais, clique em **az104-05-vm1**.

1. **Na folha az104-05-vm1**, clique em Conectar **, no menu suspenso, clique em RDP, na **folha Conectar com RDP**, clique **** em **Baixar Arquivo** RDP** e siga as instruções para iniciar a sessão de Área de Trabalho Remota.

    >**Nota**: Esta etapa refere-se à conexão via Área de Trabalho Remota a partir de um computador Windows. Em um Mac, você pode usar o Cliente de Área de Trabalho Remota na Mac App Store e, em computadores Linux, pode usar um software cliente RDP de código aberto.

    >**Nota**: Você pode ignorar quaisquer prompts de aviso ao se conectar às máquinas virtuais de destino.

1. Quando solicitado, entre usando o nome de usuário Estudante **** e a senha do arquivo de parâmetros. 

1. Na sessão de Área de Trabalho Remota para **az104-05-vm1**, clique com o botão direito do mouse no botão Iniciar** e, no menu do botão direito do **mouse, clique em **Windows PowerShell (Admin).**

1. Na janela do console do Windows PowerShell, execute o seguinte para testar a conectividade com **az104-05-vm2 (que tem o endereço IP privado 10.52.0.4******) pela porta TCP 3389:

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Nota**: O teste usa TCP 3389, uma vez que esta porta é permitida por padrão pelo firewall do sistema operacional.

1. Examine a saída do comando e verifique se a conexão foi bem-sucedida.

## Limpar os recursos

>Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >O comando é executado de modo assíncrono (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente depois na mesma sessão do PowerShell, levará alguns minutos antes de o grupo de recursos ser de fato removido.

## Revisão

Neste laboratório, você vai:

+ Tarefa 1: provisionar o ambiente de laboratório.
+ Tarefa 2: configurar o emparelhamento de rede virtual local e global.
+ Conectividade entre sites testada
