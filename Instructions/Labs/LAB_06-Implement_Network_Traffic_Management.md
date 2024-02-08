---
lab:
  title: 'Laboratório: implementação de gerenciamento de tráfego'
  module: Administer Network Traffic Management
---

# Laboratório: implementação de gerenciamento de tráfego
# Manual do aluno

## Cenário do laboratório

Você foi encarregado de testar o gerenciamento do tráfego de rede direcionado a máquinas virtuais do Azure na topologia de rede hub e spoke, que a Contoso considera implementar em seu ambiente do Azure (em vez de criar a topologia de malha, que você testou no laboratório anterior). Esse teste precisa incluir a implementação de conectividade entre raios, confiando em rotas definidas pelo usuário que forçam o tráfego a fluir pelo hub, bem como a distribuição de tráfego entre máquinas virtuais usando balanceadores de carga de camada 4 e camada 7. Para isso, você pretende usar o Azure Load Balancer (camada 4) e o Gateway de Aplicativo do Azure (camada 7).

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2010)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

>**Observação**: esse laboratório, por padrão, requer um total de 8 vCPUs disponíveis na série Standard_Dsv3 na região escolhida para implantação, pois envolve a implantação de quatro VMs do Azure de Standard_D2s_v3 SKU. Se seus alunos estiverem usando contas de avaliação, com o limite de 4 vCPUs, você poderá usar um tamanho de VM que exija apenas uma vCPU (como Standard_B1s).

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: provisionar o ambiente de laboratório.
+ Tarefa 2: configurar a topologia de rede hub e spoke.
+ Tarefa 3: testar a transitividade do emparelhamento de rede virtual.
+ Tarefa 4: configurar o roteamento na topologia hub e spoke.
+ Tarefa 5: implementar o Azure Load Balancer.
+ Tarefa 6: implementar o Gateway de Aplicativo do Azure.

## Tempo estimado: 60 minutos

## Diagrama de arquitetura

![imagem](../media/lab06.png)


### Instruções

## Exercício 1

## Tarefa 1: provisionar o ambiente de laboratório.

Nesta tarefa, você implantará quatro máquinas virtuais na mesma região do Azure. Os dois primeiros residirão em uma rede virtual de hub, enquanto cada um dos dois restantes residirá em uma rede virtual spoke separada.

1. Entre no [portal do Azure](https://portal.azure.com).

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento.

1. Na barra de ferramentas do painel Cloud Shell, clique no **ícone Upload/Download files**, no menu suspenso, clique em **Upload** and upload the **\\files Allfiles Labs 06 az104-06-vms-loop-template.json e **\\Allfiles\\\\Labs 06 az104-06-vms-loop-parameters.json\\**\\**\\\\ no diretório inicial do Cloud Shell.

1. No painel Cloud Shell, execute o seguinte para criar o primeiro grupo de recursos que hospedará o ambiente de laboratório (substitua o espaço reservado '[Azure_region]' pelo nome de uma região do Azure onde você pretende implantar máquinas virtuais do Azure)(você pode usar o "(Get-AzLocation). Local" cmdlet para obter a lista de regiões):

    ```powershell 
    $location = '[Azure_region]'
    ```
    
    Defina o nome do grupo de recursos.
    ```powershell
    $rgName = 'az104-06-rg1'
    ```
    
    E, finalmente, crie o grupo de recursos no local desejado:
    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```


1. No painel Cloud Shell, execute o seguinte para criar as três redes virtuais e quatro VMs do Azure nelas usando os arquivos de modelo e parâmetro que você carregou:

    >Você será solicitado a fornecer uma senha.

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-06-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-06-vms-loop-parameters.json
   ```

    >Aguarde a conclusão da implantação antes de prosseguir para a próxima etapa. Isso deverá levar cerca de cinco minutos.

    >**Nota**: Se você recebeu um erro informando que o tamanho da VM não está disponível, peça ajuda ao seu instrutor e tente estas etapas.
    > 1. Clique no `{}` botão no seu CloudShell, selecione o az104-06-vms-loop-parameters.json** na barra lateral esquerda e anote o **valor do `vmSize` parâmetro.
    > 1. Verifique o local no qual o grupo de recursos 'az104-06-rg1' está implantado. Você pode executar `az group show -n az104-06-rg1 --query location` em seu CloudShell para obtê-lo.
    > 1. Execute `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` em seu CloudShell.
    > 1. Substitua o valor do `vmSize` parâmetro por um dos valores retornados pelo comando que você acabou de executar. Se não houver valores retornados, talvez seja necessário escolher uma região diferente para implantar. Você também pode escolher um nome de família diferente, como "Standard_B1s".
    > 1. Agora reimplante seus modelos executando o `New-AzResourceGroupDeployment` comando novamente. Você pode pressionar o botão para cima algumas vezes, o que traria o último comando executado.

1. No painel Cloud Shell, execute o seguinte para instalar a extensão Network Watcher nas VMs do Azure implantadas na etapa anterior:

   ```powershell
   $rgName = 'az104-06-rg1'
   $location = (Get-AzResourceGroup -ResourceGroupName $rgName).location
   $vmNames = (Get-AzVM -ResourceGroupName $rgName).Name

   foreach ($vmName in $vmNames) {
     Set-AzVMExtension `
     -ResourceGroupName $rgName `
     -Location $location `
     -VMName $vmName `
     -Name 'networkWatcherAgent' `
     -Publisher 'Microsoft.Azure.NetworkWatcher' `
     -Type 'NetworkWatcherAgentWindows' `
     -TypeHandlerVersion '1.4'
   }
   ```

    >Aguarde a conclusão da implantação antes de prosseguir para a próxima etapa. Isso deverá levar cerca de cinco minutos.



1. Feche o painel do Cloud Shell.

## Tarefa 2: configurar a topologia de rede hub e spoke.

Nesta tarefa, você configurará o emparelhamento local entre as redes virtuais implantadas nas tarefas anteriores para criar uma topologia de rede hub e spoke.

1. No portal do Azure, pesquise e selecione **Redes virtuais**.

1. a rede virtual que você criou na etapa anterior.

    >**Nota**: O modelo usado para a implantação das três redes virtuais garante que os intervalos de endereços IP das três redes virtuais não se sobreponham.

1. Na lista de redes virtuais, selecione **az104-06-vnet2**.

1. **Na folha az104-06-vnet2**, selecione **Propriedades**. 

1. Na folha Propriedades** az104-06-vnet2 \| **, registre o **valor da propriedade ID do Recurso**.

1. Navegue de volta para a lista de redes virtuais e selecione **az104-06-vnet3**.

1. **Na folha az104-06-vnet3**, selecione **Propriedades**. 

1. Na folha Propriedades** az104-06-vnet3 \| **, registre o **valor da propriedade ID do Recurso**.

    >**Nota**: Você precisará dos valores da propriedade ResourceID para ambas as redes virtuais posteriormente nesta tarefa.

    >**Observação**: essa é uma solução alternativa que resolve o problema com o portal do Azure ocasionalmente não exibindo a rede virtual recém-provisionada ao criar emparelhamentos de rede virtual.

1. Na lista de redes virtuais, clique em **az104-06-vnet01**.

1. Na folha de **rede virtual az104-06-vnet01** , na **seção Configurações** , clique em **Emparelhamentos** e em **+ Adicionar**.

1. Adicione um emparelhamento com as seguintes configurações (deixe outras pessoas com seus valores padrão) e clique em **Adicionar**:

    | Configuração | Valor |
    | --- | --- |
    | Esta rede virtual: nome do link do Emparelhamento | **az104-06-vnet01_to_az104-06-vnet2** |
    | Permitir que 'az104-06-vnet01' acesse a rede virtual emparelhada | Verifique se a caixa Pesquisável está marcada (padrão). |
    | Permitir que o gateway em 'az104-06-vnet01' encaminhe o tráfego para a rede virtual emparelhada | Marque esta caixa: |
    | Rede virtual remota: nome do link do Emparelhamento | **az104-06-vnet2_to_az104-06-vnet01** |
    | Modelo de implantação de rede virtual | **Gerenciador de recursos** |
    | Conheço minha ID do recurso | Habilitado |
    | ID de Recurso | O valor do parâmetro resourceID de **az104-06-vnet2** que você registrou anteriormente nesta tarefa. |
    | Permitir que az104-06-vnet2 acesse az104-06-vnet01 | Verifique se a caixa Pesquisável está marcada (padrão). |
    | Permitir que az104-06-vnet2 receba tráfego encaminhado de az104-06-vnet01 | Marque esta caixa: |

    >Aguarde a conclusão da operação.

    >**Nota**: Esta etapa estabelece dois emparelhamentos locais - um de az104-06-vnet01 a az104-06-vnet2 e o outro de az104-06-vnet2 a az104-06-vnet01.

1. Na folha de **rede virtual az104-06-vnet01** , na **seção Configurações** , clique em **Emparelhamentos** e em **+ Adicionar**.

1. Adicione um emparelhamento com as seguintes configurações (deixe outras pessoas com seus valores padrão) e clique em **Adicionar**:

    | Configuração | Valor |
    | --- | --- |
    | Esta rede virtual: nome do link do Emparelhamento | **az104-06-vnet01_to_az104-06-vnet3** |
    | Permitir que 'az104-06-vnet01' acesse a rede virtual emparelhada | Verifique se a caixa Pesquisável está marcada (padrão). |
    | Permitir que o gateway em 'az104-06-vnet01' encaminhe o tráfego para a rede virtual emparelhada | Marque esta caixa: |
    | Rede virtual remota: nome do link do Emparelhamento | **az104-06-vnet3_to_az104-06-vnet01** |
    | Modelo de implantação de rede virtual | **Gerenciador de recursos** |
    | Conheço minha ID do recurso | Habilitado |
    | ID de Recurso | O valor do parâmetro resourceID de **az104-06-vnet3** que você registrou anteriormente nesta tarefa. |
    | Permitir que az104-06-vnet3 acesse az104-06-vnet01 | Verifique se a caixa Pesquisável está marcada (padrão). |
    | Permitir que az104-06-vnet3 receba tráfego encaminhado de az104-06-vnet01 | Marque esta caixa: |


    >Aguarde a conclusão da operação.
    
    >**Nota**: Esta etapa estabelece dois emparelhamentos locais - um de az104-06-vnet01 a az104-06-vnet3 e o outro de az104-06-vnet3 a az104-06-vnet01. Isso conclui a configuração da topologia hub e spoke (com duas redes virtuais de spoke).

## Tarefa 3: testar a transitividade do emparelhamento de rede virtual.

Nesta tarefa, você testará a transitividade do emparelhamento de rede virtual usando Observador de Rede.

1. No portal do Azure, pesquise e selecione **Redes virtuais**.

1. **Na folha Inspetor de Rede**, expanda a listagem de regiões do Azure e verifique se o serviço está habilitado na região que você está usando. 

1. Na folha Inspetor de Rede **, navegue até a Solução** de **problemas de **conexão.

1. **Na folha Inspetor de Rede - Solução de problemas** de conexão, inicie uma verificação com as seguintes configurações (deixe outras pessoas com seus valores padrão):

    > Levará poucos minutos para criar o grupo de recursos. Se você não quiser esperar, tente o seguinte: exclua o Inspetor de Rede, crie um novo Inspetor de Rede e tente novamente a Solução de Problemas de Conexão. 

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | az104-06-rg1 |
    | Tipo de origem | **Máquina virtual** |
    | Máquina virtual | az104-06-vm0 |
    | Destino | Especificar manualmente |
    | URI, FQDN ou IPv4 | 10.62.0.4 |
    | Protocolo | **TCP** |
    | Porta de destino | **3389** |

    > **Nota**: **10.62.0.4** representa o endereço IP privado de **az104-06-vm2**

1. Clique em Executar **testes** de diagnóstico e aguarde até que os resultados da verificação de conectividade sejam retornados. Verifique se o teste foi bem-sucedido. Revise o caminho de rede e observe que a conexão foi direta, sem saltos intermediários entre as VMs.

    > **Nota**: Isso é esperado, uma vez que a rede virtual do hub é emparelhada diretamente com a primeira rede virtual falada.

1. **Na folha Inspetor de Rede - Solução de problemas** de conexão, inicie uma verificação com as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | az104-06-rg1 |
    | Tipo de origem | **Máquina virtual** |
    | Máquina virtual | az104-06-vm0 |
    | Destino | Especificar manualmente |
    | URI, FQDN ou IPv4 | 10.63.0.4 |
    | Protocolo | **TCP** |
    | Porta de destino | **3389** |

    > **Nota**: **10.63.0.4** representa o endereço IP privado de **az104-06-vm3**

1. Clique em Executar **testes** de diagnóstico e aguarde até que os resultados da verificação de conectividade sejam retornados. Verifique se o teste foi bem-sucedido. Revise o caminho de rede e observe que a conexão foi direta, sem saltos intermediários entre as VMs.

    > **Nota**: Isso é esperado, uma vez que a rede virtual do hub é emparelhada diretamente com a segunda rede virtual falada.

1. **Na folha Inspetor de Rede - Solução de problemas** de conexão, inicie uma verificação com as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | az104-06-rg1 |
    | Tipo de origem | **Máquina virtual** |
    | Máquina virtual | az104-06-vm2 |
    | Destino | Especificar manualmente |
    | URI, FQDN ou IPv4 | 10.63.0.4 |
    | Protocolo | **TCP** |
    | Porta de destino | **3389** |

1. Clique em Executar **testes** de diagnóstico e aguarde até que os resultados da verificação de conectividade sejam retornados. Observe que o status é **Fail**.

    > **Nota**: Isso é esperado, uma vez que as duas redes virtuais faladas não são emparelhadas uma com a outra (o emparelhamento de rede virtual não é transitivo).

## Tarefa 4: configurar o roteamento na topologia hub e spoke.

Nesta tarefa, você configurará e testará o roteamento entre as duas redes virtuais spoke habilitando o encaminhamento IP na interface de rede da máquina virtual az104-06-vm0 **, habilitando o **roteamento em seu sistema operacional e configurando rotas definidas pelo usuário na rede virtual spoke.

1. No portal do Azure, procure e selecione **Máquinas virtuais**.

1. **Na folha Máquinas** virtuais, na lista de máquinas virtuais, clique em **az104-06-vm0**.

1. Na folha da **máquina virtual az104-06-vm0** , na **seção Configurações** , clique em **Rede**.

1. Clique no **link az104-06-nic0 ao lado do rótulo da interface de rede e, na folha da **interface** de **rede az104-06-nic0****, na **seção Configurações, clique em **Configurações** de** IP.

1. Defina **Habilitar para os usuários entrarem?** como **Sim** e salve a alteração.

   > **Nota**: Essa configuração é necessária para **que o az104-06-vm0** funcione como um roteador, que roteará o tráfego entre duas redes virtuais de spoke.

   > **Nota**: Agora você precisa configurar o sistema operacional da **máquina virtual az104-06-vm0** para oferecer suporte ao roteamento.

1. No portal do Azure, navegue de volta para a folha da **máquina virtual do Azure az104-06-vm0** e clique em **Visão geral**.

1. Na folha az104-06-vm0, na **seção Operações**, clique em Executar comando** e, na lista de comandos,** clique em ****RunPowerShellScript**.**

1. **Na folha Executar Script de Comando**, digite o seguinte e clique em **Executar** para instalar a função Windows Server de Acesso Remoto.

   ```powershell
   Install-WindowsFeature RemoteAccess -IncludeManagementTools
   ```

   > **Nota**: Aguarde a confirmação de que o comando foi concluído com êxito.

1. **Na folha Executar Script de Comando**, digite o seguinte e clique em **Executar** para instalar o serviço de função Roteamento.

   ```powershell
   Install-WindowsFeature -Name Routing -IncludeManagementTools -IncludeAllSubFeature

   Install-WindowsFeature -Name "RSAT-RemoteAccess-Powershell"

   Install-RemoteAccess -VpnType RoutingOnly

   Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled
   ```

   > **Nota**: Aguarde a confirmação de que o comando foi concluído com êxito.

   > **Nota**: Agora você precisa criar e configurar rotas definidas pelo usuário nas redes virtuais de spoke.

1. No portal do Azure, pesquise e selecione **Tabelas de rotas e, na folha Tabelas** de **rotas** , clique em **+ Criar**.

1. Crie uma tabela de rotas com as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | az104-06-rg1 |
    | Location | o nome da região do Azure na qual você criou as redes virtuais |
    | Nome | az104-06-rt23 |
    | Propagar rotas de gateway | **Não** |

1. Clique em **Revisar e criar**. Deixe a validação ocorrer e clique em **Criar** para enviar sua implantação.

   > **Nota**: Aguarde até que a tabela de rotas seja criada. Isso deverá levar cerca de cinco minutos.

1. Clique em **Ir para o recurso**.

1. Na folha da **tabela de rotas az104-06-rt23**, na **seção Configurações**, clique em Rotas** e em ****+ Adicionar**.

1. Adicione um novo provedor com as seguintes configurações: 

    | Configuração | Valor |
    | --- | --- |
    | Nome da rota | **az104-06-route-vnet2-to-vnet3** |
    | Prefixo de endereço de destino | **Endereços IP** |
    | Intervalos de CIDR /endereço IP de destino | 10.63.0.0/16 |
    | Tipo do próximo salto | **Solução de virtualização** |
    | Endereço do próximo salto | 10.60.0.4 |

1. Clique em **Adicionar**

1. De volta à folha da **tabela de rotas az104-06-rt23** , na **seção Configurações** , clique em **Sub-redes** e em **+ Associar**.

1. Associar a tabela de rotas à sub-rede

    | Configuração | Valor |
    | --- | --- |
    | Rede virtual | **az104-06-VNet2** |
    | Sub-rede | subnet0 |

1. Clique em **Adicionar**

1. Navegue de volta para a **folha Tabelas** de rotas e clique em **+ Criar**.

1. Crie uma tabela de rotas com as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | az104-06-rg1 |
    | Region | o nome da região do Azure na qual você criou as redes virtuais |
    | Nome | az104-06-rt32 |
    | Propagar rotas de gateway | **Não** |

1. Clique em Revisar e criar. Deixe a validação ocorrer e pressione Criar para enviar sua implantação.

   > **Nota**: Aguarde até que a tabela de rotas seja criada. Isso deverá levar cerca de cinco minutos.

1. Clique em **Ir para o recurso**.

1. Na folha da **tabela de rotas az104-06-rt32**, na **seção Configurações**, clique em Rotas** e em ****+ Adicionar**.

1. Adicione um novo provedor com as seguintes configurações: 

    | Configuração | Valor |
    | --- | --- |
    | Nome da rota | **az104-06-route-vnet3-to-vnet2** |
    | Prefixo de endereço de destino | **Endereços IP** |
    | Intervalos de CIDR /endereço IP de destino | 10.62.0.0/16 |
    | Tipo do próximo salto | **Solução de virtualização** |
    | Endereço do próximo salto | 10.60.0.4 |

1. Clique em **OK**

1. De volta à folha da **tabela de rotas az104-06-rt32** , na **seção Configurações** , clique em **Sub-redes** e em **+ Associar**.

1. Associar a tabela de rotas à sub-rede

    | Configuração | Valor |
    | --- | --- |
    | Rede virtual | **az104-06-VNet3** |
    | Sub-rede | subnet0 |

1. Clique em **OK**

1. No portal do Azure, navegue de volta para a folha Inspetor de Rede - Solução de problemas de **conexão** .

1. **Na folha Inspetor de Rede - Solução de problemas de** conexão, use as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | az104-06-rg1 |
    | Tipo de origem | **Máquina virtual** |
    | Máquina virtual | az104-06-vm2 |
    | Destino | Especificar manualmente |
    | URI, FQDN ou IPv4 | 10.63.0.4 |
    | Protocolo | **TCP** |
    | Porta de destino | **3389** |

1. Clique em Executar **testes** de diagnóstico e aguarde até que os resultados da verificação de conectividade sejam retornados. Verifique se o teste foi bem-sucedido. Examine o caminho de rede e observe que o tráfego foi roteado via **10.60.0.4**, atribuído ao **adaptador de rede az104-06-nic0** . Se o status for **Fail**, você deve parar e iniciar az104-06-vm0.

    > **Nota**: Isso é esperado, uma vez que o tráfego entre redes virtuais spoke agora é roteado através da máquina virtual localizada na rede virtual hub, que funciona como um roteador.

    > **Nota**: Você pode usar **o Inspetor** de Rede para exibir a topologia da rede.

## Tarefa 5: implementar o Azure Load Balancer.

Nesta tarefa, você implementará um Azure Load Balancer antes das duas máquinas virtuais do Azure na rede virtual do hub.

1. No portal do Azure, procure e selecione **Balanceadores de carga e, na **folha Balanceadores**** de carga, clique em **+ Criar**.

1. Crie um balanceador de carga com as seguintes configurações (deixe outras pessoas com seus valores padrão) e clique em **Avançar: Configuração** de IP de front-end:

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | **az104-06-rg4** (se necessário criar) |
    | Nome | az104-06-rg4 |
    | Region | nome da região do Azure na qual você implantou todos os outros recursos neste laboratório |
    | SKU  | **Standard** |
    | Tipo | **Público** |
    | Camada | **Regional** |
    
1. Na guia Configuração de** IP de front-end, clique em **Adicionar uma configuração** de IP de **front-end e use as seguintes configurações:  
     
    | Configuração | Valor |
    | --- | --- |
    | Nome | az104-06-rg4 |
    | Tipo de IP | Endereço IP |
    | Endereço IP público | Selecione **Criar** |
    | Balanceador de Carga para Gateway | Nenhum |
    
1. **No pop-up Adicionar um endereço** IP público, use as seguintes configurações antes de clicar em OK** e em ****Adicionar**. Quando terminar, clique em **Avançar: pools de** back-end. 
     
    | Configuração | Valor |
    | --- | --- |
    | Nome | az104-06-pip4 |
    | SKU | Standard |
    | Camada | Regional |
    | Atribuição | Estático |
    | Preferência de Roteamento | Roteamento de rede |

1. **Na guia Pools** de back-end, clique em **Adicionar um pool** de back-end com as seguintes configurações (deixe outras pessoas com seus valores padrão). Clique em + Adicionar** (duas vezes) e, em seguida, clique em ****Avançar:Regras de** entrada. 

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az104-06-lb4-be1** |
    | Rede virtual | **az104-06-VNet01** |
    | Configuração do pool de back-end | NIC | 
    | Versão IP | **IPv4** |
    | Clique em **Adicionar uma máquina virtual**. |  |
    | az104-06-vm0 | Marque a caixa |
    | az104-06-vm1 | Marque a caixa |


1. **Na guia Regras de** entrada, clique em **Adicionar uma regra** de balanceamento de carga. Adicione uma regra de balanceamento de carga com as seguintes configurações (deixe outras pessoas com seus valores padrão). Quando concluído, clique em **OK**.

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az104-06-lb4-lbrule1** |
    | Versão IP | **IPv4** |
    | Endereço IP de front-end | az104-06-rg4 |
    | Pool de back-end | **az104-06-lb4-be1** |    
    | Protocolo | **TCP** |
    | Porta | **80** |
    | Porta de back-end | **80** |
    | Investigação de integridade | **Criar novo** |
    | Nome | **az104-06-lb4-hp1** |
    | Protocolo | **TCP** |
    | Porta | **80** |
    | Intervalo | **5** |
    | Feche a janela de investigação de integridade de criação | **OK** | 
    | Persistência de sessão | **Nenhuma** |
    | Tempo limite de ociosidade (minutos) | **4** |
    | Redefinição de TCP | **Desabilitado** |
    | IP flutuante | **Desabilitada** |
    | SNAT (conversão de endereços de rede de origem) de saída | **Recomendado** |

1. À medida que tiver tempo, reveja os outros separadores e, em seguida, clique em **Rever e criar**. Verifique se não há erros de validação e clique em **Criar**. 

1. Aguarde até o contêiner ser implantado e clique em **Ir para o recurso**.  

1. Na página de balanceador de carga, selecione Configuração de IP de front-end em Configurações. Copie o endereço IP.

1. Abra um navegador da Web e navegue até o endereço . Verifique se a janela do navegador exibe a mensagem **Hello World de az104-06-vm0** ou **Hello World de az104-06-vm1**.

1. Atualize a janela para verificar se a mensagem é alterada para a outra máquina virtual. Isso demonstra o balanceador de carga girando pelas máquinas virtuais.

    > **Nota**: Talvez seja necessário atualizar mais de uma vez ou abrir uma nova janela do navegador no modo InPrivate.

## Tarefa 6: implementar o Gateway de Aplicativo do Azure.

Nesta tarefa, você implementará um Gateway de Aplicativo do Azure antes das duas máquinas virtuais do Azure nas redes virtuais spoke.

1. No portal do Azure, pesquise e selecione **Redes virtuais**.

1. **Na folha Redes** virtuais, na lista de redes virtuais, clique em **az104-06-vnet01**.

1. Na folha de  **rede virtual az104-06-vnet01** , na **seção Configurações** , clique em **Sub-redes** e em **+ Sub-rede**.

1. Adicione uma sub-rede com as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Nome | Subnet-appgw |
    | Intervalo de endereços da sub-rede | 10.0.30.224/27 |

1. Clique em **Salvar**

    > **Observação**: essa sub-rede será usada pelas instâncias do Gateway de Aplicativo do Azure, que você implantará posteriormente nesta tarefa. O Application Gateway requer uma sub-rede dedicada de tamanho /27 ou maior.

1. No portal do Azure, procure Gateway de Aplicativo e selecione Criar gateway de aplicativo.

1. Na folha Novo Contêiner, especifique as seguintes configurações (deixe as demais com seus valores padrão) e selecione OK:

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | **az104-06-rg5** (criar novo) |
    | Nome do gateway de aplicativo | az104-06-appgw5 |
    | Region | nome da região do Azure na qual você implantou todos os outros recursos neste laboratório |
    | Camada | **Padrão V2** |
    | Habilitar o dimensionamento automático | **Não** |
    | Contagem de instâncias | **2** |
    | Zona de disponibilidade | **Nenhuma** |
    | HTTP2 | **Desabilitado** |
    | Rede virtual | **az104-06-VNet01** |
    | Sub-rede | Subnet-appgw 10.60.3.224/27 |

1. Clique em **Avançar: Front-ends >** e especifique as seguintes configurações (deixe outras pessoas com seus valores padrão). Ao concluir, clique em **OK**. 

    | Configuração | Valor |
    | --- | --- |
    | Tipo de endereço IP de front-end | **Público** |
    | Endereço IP público| **Adicionar nova** | 
    | Nome | az104-06-pip5 |
    | Zona de disponibilidade | **Nenhuma** |

1. Clique em **Avançar: >** de back-ends e em **Adicionar um pool** de back-ends. Especifique as configurações a seguir (deixe outras pessoas com seus valores padrão). Quando concluído, clique em **OK**.

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az104-06-appgw5-be1** |
    | Adicionar pool de back-end sem destinos | **Não** |
    | Endereço IP ou FQDN | 10.62.0.4 | 
    | Endereço IP ou FQDN | 10.63.0.4 |

    > **Nota**: Os destinos representam os endereços IP privados de máquinas virtuais nas redes **virtuais spoke az104-06-vm2** e **az104-06-vm3**.

1. Clique em Avançar: >** de configuração e, em seguida, em ****+ Adicionar uma regra** de roteamento. Especifique as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Nome da regra | **az104-06-appgw5-rl1** |
    | Prioridade | **10** |
    | Nome do ouvinte | **az104-06-appgw5-rl1l1** |
    | IP de front-end | **Público** |
    | Protocolo | **HTTP** |
    | Porta | **80** |
    | Tipo de ouvinte | **Basic** |
    | URL da página de erro | **Não** |

1. Alterne para a **guia Destinos de back-end** e especifique as seguintes configurações (deixe outras pessoas com seus valores padrão). Quando terminar, clique em **Adicionar** (duas vezes).  

    | Configuração | Valor |
    | --- | --- |
    | Tipo de destino | **Pool de back-end** |
    | Destino de back-end | **az104-06-appgw5-be1** |
    | Configurações de back-end | **Adicionar nova** |
    | Nome das configurações de back-end | **az104-06-appgw5-http1** |
    | Protocolo de back-end | **HTTP** |
    | Porta de back-end | **80** |
    | Configurações adicionais | Basta aceitar os padrões. |
    | Nome do host | Basta aceitar os padrões. |

1. Clique em Avançar: Marcas >, seguido por **Avançar: Revisar + criar >**** e, em seguida, clique em ****Criar**.

    > **Nota**: Aguarde até que a instância do Application Gateway seja criada. Isso pode levar cerca de 10 minutos.

1. No portal do Azure, pesquise e selecione **Gateways de Aplicativo e, na **folha Gateways de**** Aplicativo, clique em **az104-06-appgw5**.

1. **Na folha az104-06-appgw5** Application Gateway, copie o **valor do endereço IP público do Front-end**.

1. Inicie outra janela do navegador e navegue até o endereço IP que você identificou na etapa anterior.

1. Verifique se a janela do navegador exibe a mensagem **Hello World de az104-06-vm2** ou **Hello World de az104-06-vm3**.

1. Atualize a janela para verificar se a mensagem é alterada para a outra máquina virtual. 

    > **Nota**: Talvez seja necessário atualizar mais de uma vez ou abrir uma nova janela do navegador no modo InPrivate.

    > **Observação**: direcionar máquinas virtuais em várias redes virtuais não é uma configuração comum, mas destina-se a ilustrar o ponto em que o Gateway de Aplicativo é capaz de direcionar máquinas virtuais em várias redes virtuais (bem como pontos de extremidade em outras regiões do Azure ou mesmo fora do Azure), ao contrário do Balanceador de Carga do Azure, que equilibra a carga entre máquinas virtuais na mesma rede virtual.

## Limpar os recursos

>Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >O comando é executado de modo assíncrono (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente depois na mesma sessão do PowerShell, levará alguns minutos antes de o grupo de recursos ser de fato removido.

## Revisão

Neste laboratório, você vai:

+ Tarefa 1: provisionar o ambiente de laboratório.
+ Tarefa 2: configurar a topologia de rede hub e spoke.
+ Tarefa 3: testar a transitividade do emparelhamento de rede virtual.
+ Tarefa 4: configurar o roteamento na topologia hub e spoke.
+ Implementar o Azure Load Balancer
+ Implementar o Gateway de Aplicativo do Azure
