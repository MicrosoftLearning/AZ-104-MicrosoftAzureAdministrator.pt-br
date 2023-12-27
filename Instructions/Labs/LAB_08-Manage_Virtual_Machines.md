---
lab:
  title: 'Laboratório 08: gerenciar máquinas virtuais'
  module: Administer Virtual Machines
---

# Laboratório 08 – Gerenciar máquinas virtuais
# Manual do laboratório do aluno

## Cenário do laboratório

Você foi encarregado de identificar diferentes opções para implantar e configurar máquinas virtuais do Azure. Primeiro, você precisa determinar diferentes opções de resiliência e escalabilidade de computação e armazenamento que você pode implementar ao usar máquinas virtuais do Azure. Em seguida, você precisa investigar as opções de resiliência e escalabilidade de computação e armazenamento disponíveis ao usar conjuntos de dimensionamento de máquinas virtuais do Azure. Você também deve explorar a capacidade de configurar automaticamente máquinas virtuais e conjuntos de dimensionamento de máquinas virtuais usando a extensão de Script Personalizada de Máquina Virtual do Azure.

**Observação:** está disponível uma **[simulação de laboratório interativa](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2012)**, permitindo navegar neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: implantar máquinas virtuais do Azure resilientes à zona usando o portal do Azure e um modelo do Azure Resource Manager.
+ Tarefa 2: configurar máquinas virtuais do Azure usando extensões de máquina virtual
+ Tarefa 3: dimensionar a computação e o armazenamento das máquinas virtuais do Azure
+ Tarefa 4: registrar os provedores de recursos Microsoft.Insights e Microsoft.AlertsManagement.
+ Tarefa 5: implantar conjuntos de dimensionamento de máquinas virtuais do Azure resilientes à zona usando o portal do Azure
+ Tarefa 6: configurar conjuntos de dimensionamento de máquinas virtuais do Azure usando extensões de máquina virtual
+ Tarefa 7: dimensionar a computação e o armazenamento dos conjuntos de dimensionamento de máquinas virtuais do Azure (opcional)

## Tempo estimado: 50 minutos

## Diagrama de arquitetura

![imagem](../media/lab08.png)


### Instruções

## Exercício 1

## Tarefa 1: implantar máquinas virtuais do Azure resilientes à zona usando o portal do Azure e um modelo do Azure Resource Manager.

Nesta tarefa, você vai implantar máquinas virtuais do Azure em diferentes zonas usando o portal do Azure e um modelo do Azure Resource Manager.

1. Entre no [portal do Azure](http://portal.azure.com).

1. No portal do Azure, procure e selecione **Máquinas virtuais** e, no painel **Máquinas virtuais**, clique em **+ Criar**e depois, clique em **+ Máquina virtual do Azure**.

1. Na guia **Básico** do painel **Criar uma máquina virtual**, defina as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | o nome da assinatura do Azure que você usará neste laboratório |
    | Grupo de recursos | o nome de um novo grupo de recursos **az104-08-rg01** |
    | Nome da máquina virtual | **az104-08-vm0** |
    | Region | selecione uma das regiões que dão suporte às zonas de disponibilidade e onde você pode provisionar máquinas virtuais do Azure |
    | Opções de disponibilidade | **Zona de disponibilidade** |
    | Zona de disponibilidade | **Zona 1** |
    | Imagem | **Windows Server 2019 Datacenter – Gen2** |
    | Instância do Azure Spot | **Não** |
    | Tamanho | **Standard D2s v3** |
    | Nome de Usuário | **Aluno** |
    | Senha | **Forneça uma senha segura, com um mínimo de 12 caracteres** |
    | Porta de entrada públicas | **Nenhuma** |
    | Deseja usar uma licença existente do Windows Server? | **Desmarcado** |

1. Clique em **Avançar: Discos >** e, na guia **Discos** da folha **Criar uma máquina virtual**, defina as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Tipo de disco de SO | **SSD Premium** |
    | Habilitar a compatibilidade com o Disco Ultra | **Desmarcado** |

1. Clique em **Avançar: Rede >** e, na guia **Rede** da folha **Criar uma máquina virtual**, clique em **Criar novo** abaixo da caixa de texto **Rede virtual**.

1. Na folha **Criar rede virtual**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az104-08-vnet01** |
    | Intervalo de endereços | **10.80.0.0/20** |
    | Nome da sub-rede | **subnet0** |
    | Intervalo de sub-rede | **10.80.0.0/24** |

1. Clique em **OK** e, de volta à guia **Rede** da folha **Criar uma máquina virtual**, defina as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Sub-rede | **subnet0** |
    | IP público | **default** |
    | Grupo de segurança de rede da NIC | **básico** |
    | Portas de entradas Públicas | **Nenhuma** |
    | Redes aceleradas | **Desativado**
    | Opções de balanceamento de carga | **Nenhuma** |

1. Selecione **Avançar: Gerenciamento >** e, na guia **Gereciamento** da folha **Criar uma máquina virtual** defina as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Opções de orquestração de patch | **Atualizações manuais** |  

1. Clique em **Avançar: Monitoramento >** e, na guia **Monitoramento** da folha **Criar uma máquina virtual**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Diagnóstico de inicialização | **Habilitar com a conta de armazenamento personalizada** |
    | Conta de armazenamento de diagnóstico | **aceitar o valor padrão** |

    >**Observação**: se necessário, selecione uma conta de armazenamento existente na lista suspensa ou crie uma nova conta de armazenamento. Registre o nome da conta de armazenamento. Você a usará na próxima etapa.

1. Clique em **Avançar: Avançado >** na guia **Avançado** da folha **Criar uma máquina virtual**, examine as configurações disponíveis sem modificar nenhuma delas e clique em **Examinar + Criar**.

1. Na folha **Examinar + Criar**, clique em **Criar**.

1. Na folha implantação. clique em **Modelo**.

1. Examine o modelo que representa a implantação em andamento e clique em **Implantar**.

    >**Observação**: você usará esta opção para implantar a segunda máquina virtual com configuração correspondente, exceto para a zona de disponibilidade.

1. Na folha **Implantação personalizada**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Grupo de recursos | **az104-08-rg01** |
    | Nome da Interface de Rede | **az104-08-vm1-nic1** |
    | Nome do endereço IP público | **az104-08-vm1-ip** |
    | Nome da Máquina Virtual, Nome da Máquina Virtual1, Nome do Computador da Máquina Virtual   | **az104-08-vm1** |
    | RG da Máquina Virtual | **az104-08-rg01** |    
    | Nome de Usuário do Administrador | **Aluno** |
    | Senha do Administrador | **Forneça uma senha segura**  |
    | Habilitar Aplicação de patch dinâmica | **false** |
    | Zona | **2** |

    >**Observação**: você precisa modificar os parâmetros correspondentes às propriedades dos diferentes recursos que você está implantando usando o modelo, incluindo a máquina virtual e sua interface de rede.

1. Clique em **Examinar + Criar** e na folha **Examinar + Criar**, clique em **Criar**.

    >**Observação**: aguarde a conclusão de ambas as implantações antes de prosseguir para a próxima tarefa. Isso pode levar cerca de cinco minutos.

## Tarefa 2: configurar máquinas virtuais do Azure usando extensões de máquina virtual

Nesta tarefa, você instalará a função de servidor Web do Windows Server nas duas máquinas virtuais do Azure implantadas na tarefa anterior usando a extensão de máquina virtual de Script Personalizado.

1. No portal do Azure, pesquise e selecione **Contas de armazenamento** e na folha **Contas de armazenamento**, clique na entrada que representa a conta de armazenamento de diagnóstico que você criou na tarefa anterior.

1. Na folha da conta de armazenamento, na seção **Armazenamento de Dados**, clique em **Contêineres** e em **+ Contêiner**.

1. Na folha **Novo contêiner**, especifique as seguintes configurações (deixe as outras com seus valores padrão) e clique em **Criar**:

    | Configuração | Valor |
    | --- | --- |
    | Nome | **scripts** |
    | Nível de acesso público | **Privado (sem acesso anônimo**) |

1. De volta à folha da conta de armazenamento que exibe a lista de contêineres, clique em **scripts**.

1. Na folha **scripts**, clique em **Carregar**.

1. Na folha **Carregar blob**, clique no ícone de pasta na caixa de diálogo **Abrir**, navegue até a pasta **\\Allfiles\\Labs\\08**, selecione **az104-08-install_IIS.ps1**, clique em **Abrir** e de volta à folha **Carregar blob**, clique em **Carregar**.

1. No portal do Azure, pesquise e selecione **Máquinas virtuais** e na folha **Máquinas virtuais**, clique em **az104-08-vm0**.

1. Na folha da máquina virtual **az104-08-vm0**, na seção ** Configurações**, clique em **Extensões + aplicativos** e em **+ Adicionar**.

1. No painel **Instalar uma extensão**, clique em **Extensão de Script Personalizada** e, em seguida, clique em **Avançar**.

1. No painel **Configurar extensão de extensão de script personalizada**, clique em **Procurar**.

1. No painel **Contas de armazenamento**, clique no nome da conta de armazenamento na qual você carregou o script **az104-08-install_IIS.ps1**. No painel **Contêineres**, clique em **scripts**. No painel **scripts**, clique em **az104-08-install_IIS.ps1** e, em seguida, clique em **Selecionar**.

1. De volta à folha **Instalar extensão**, clique em **Revisar + criar** e, na folha **Revisar+ criar**, clique em **Criar**.

1. No portal do Azure, procure e selecione **Máquinas virtuais** e, no painel ** Máquinas virtuais **, clique em **az104-08-vm1**.

1. No painel **az104-08-vm1**, na seção **Automação**, clique em **Exportar modelo**.

1. No painel **az104-08-vm1 - Exportar modelo**, clique em **Implantar**.

1. No painel **Implantação personalizada**, clique em **Editar modelo**.

    >**Observação**: desconsidere a mensagem que diz **O grupo do recurso está em um local que não é suportado por um ou mais recursos no modelo. Selecione um grupo de recurso diferente**. Isso é esperado e pode ser ignorado neste caso.

1. Na folha **Editar modelo**, na seção que exibe o conteúdo do modelo, insira o seguinte código começando com a linha **20** (diretamente abaixo da linha `"resources": [`):

   >**Observação**: se você estiver usando uma ferramenta que cola o código linha por linha, o IntelliSense pode adicionar colchetes extras causando erros de validação. Talvez seja interessante colar o código no bloco de notas primeiro e, em seguida, colá-lo na linha 20.

   ```json
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "az104-08-vm1/customScriptExtension",
            "apiVersion": "2018-06-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "az104-08-vm1"
            ],
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "CustomScriptExtension",
                "typeHandlerVersion": "1.7",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "commandToExecute": "powershell.exe Install-WindowsFeature -name Web-Server -IncludeManagementTools && powershell.exe remove-item 'C:\\inetpub\\wwwroot\\iisstart.htm' && powershell.exe Add-Content -Path 'C:\\inetpub\\wwwroot\\iisstart.htm' -Value $('Hello World from ' + $env:computername)"
              }
            }
        },

   ```

   >**Observação**: esta seção do modelo define a mesma extensão de script personalizado de máquina virtual do Azure que você implantou anteriormente na primeira máquina virtual por meio do Azure PowerShell.

1. Clique em **Salvar** e, de volta à folha **Modelo personalizado**, clique em **Revisar + Criar** e, na folha **Revisar + Criar**, clique em **Criar**

    >**Observação**: aguarde até que a implantação do modelo seja concluída. Você pode monitorar seu progresso no painel **Extensões** das máquinas virtuais **az104-08-vm0** e **az104-08-vm1**. Isso deve levar menos de 3 minutos.

1. Para verificar se a configuração baseada na extensão Script Personalizado foi bem-sucedida, navegue novamente para a folha **az104-08-vm1**, na seção **Operações**, clique em **Executar comando** e, na lista de comandos, clique em **RunPowerShellScript**.

1. Na folha **Executar Script de Comando**, digite as informações a seguir e clique em **Executar** para acessar o site hospedado em **az104-08-vm1**:

   ```powershell
   Invoke-WebRequest -URI http://10.80.0.4 -UseBasicParsing
   ```

    >**Observação**: o parâmetro **-UseBasicParsing** é necessário para eliminar a dependência do Internet Explorer para concluir a execução do cmdlet

    >**Observação**: o parâmetro **-URI** é o **endereço de IP privado** da VM. Navegue até a folha **az104-08-vm1**, na seção **Rede**, e clique em **Configurações de rede**

    >**Observação**: você também pode se conectar à **az104-08-vm0** e executar `Invoke-WebRequest -URI http://10.80.0.5 -UseBasicParsing` para acessar o site hospedado na **az104-08-vm1**.

## Tarefa 3: dimensionar a computação e o armazenamento das máquinas virtuais do Azure

Nesta tarefa, você vai dimensionar a computação das máquinas virtuais do Azure alterando o tamanho e dimensionar o armazenamento anexando e configurando os discos de dados.

1. No portal do Azure, pesquise e selecione **Máquinas virtuais** e na folha **Máquinas virtuais**, clique em **az104-08-vm0**.

1. Na folha da máquina virtual **az104-08-vm0**, clique em **Tamanho** e defina o tamanho da máquina virtual como **Standard DS1_v2** e clique em **Redimensionar**

    >**Observação**: escolha outro tamanho se o **DS1_v2 Standard** não estiver disponível.

1. Na folha da máquina virtual **az104-08-vm0**, clique em **Discos** e, em **Discos de dados**, clique em **+ Criar e anexar um novo disco**.

1. Crie um disco gerenciado com as seguintes configurações (deixe as outras com seus valores padrão) e clique em **Aplicar**:

    | Configuração | Valor |
    | --- | --- |
    | Nome do disco | **az104-08-vm0-datadisk-0** |
    | Tipo de armazenamento | **SSD Premium** |
    | Tamanho (GiB| **1024** |

1. De volta à folha **az104-08-vm0 - Discos**, em **Discos de dados**, clique em **+ Criar e anexar um novo disco**.

1. Crie um disco gerenciado com as seguintes configurações (deixe as outras com seus valores padrão) e clique em **Aplicar**:

    | Configuração | Valor |
    | --- | --- |
    | Nome do disco | **az104-08-vm0-datadisk-1** |
    | Tipo de armazenamento | **SSD Premium** |
    | Tamanho (GiB)| **1024 GiB** |


1. No painel **az104-08-vm0**, na seção **Operações**, clique em **Executar comando** e, na lista de comandos, clique em **RunPowerShellScript**.

1. No painel **Executar Script de Comando**, digite o seguinte e clique em **Executar** para criar uma unidade Z: consistindo nos dois discos recém-conectados com o layout simples e provisionamento fixo:

   ```powershell
   New-StoragePool -FriendlyName storagepool1 -StorageSubsystemFriendlyName "Windows Storage*" -PhysicalDisks (Get-PhysicalDisk -CanPool $true)

   New-VirtualDisk -StoragePoolFriendlyName storagepool1 -FriendlyName virtualdisk1 -Size 64GB -ResiliencySettingName Simple -ProvisioningType Fixed

   Initialize-Disk -VirtualDisk (Get-VirtualDisk -FriendlyName virtualdisk1)

   New-Partition -DiskNumber 4 -UseMaximumSize -DriveLetter Z
   ```

    > **Observação**: aguarde a confirmação de que os comandos foram concluídos com êxito.

1. No portal do Azure, procure e selecione **Máquinas virtuais** e, no painel ** Máquinas virtuais **, clique em **az104-08-vm1**.

1. No painel **az104-08-vm1**, na seção **Automação**, clique em **Exportar modelo**.

1. No painel **az104-08-vm1 - Exportar modelo**, clique em **Implantar**.

1. No painel **Implantação personalizada**, clique em **Editar modelo**.

    >**Observação**: desconsidere a mensagem que diz **O grupo do recurso está em um local que não é suportado por um ou mais recursos no modelo. Selecione um grupo de recurso diferente**. Isso é esperado e pode ser ignorado neste caso.

1. No painel **Editar modelo**, na seção que exibe o conteúdo do modelo, substitua a linha **30**`"vmSize": "Standard_D2s_v3"` pela seguinte linha):

   ```json
                    "vmSize": "Standard_DS1_v2"

   ```

    >**Observação**: esta seção do modelo define o mesmo tamanho de máquina virtual do Azure que você especificou para a primeira máquina virtual por meio do portal do Azure.

1. No painel **Editar modelo**, na seção que exibe o conteúdo do modelo, substitua a linha **54** (`"dataDisks": [ ],`) pelo seguinte código:

   ```json
                    "dataDisks": [
                      {
                        "lun": 0,
                        "name": "az104-08-vm1-datadisk0",
                        "diskSizeGB": "1024",
                        "caching": "ReadOnly",
                        "createOption": "Empty"
                      },
                      {
                        "lun": 1,
                        "name": "az104-08-vm1-datadisk1",
                        "diskSizeGB": "1024",
                        "caching": "ReadOnly",
                        "createOption": "Empty"
                      }
                    ],
   ```

    >**Observação**: se você estiver usando uma ferramenta que cola o código linha por linha, o IntelliSense pode adicionar colchetes extras causando erros de validação. Talvez você queira colar o código no bloco de notas primeiro e, em seguida, colá-lo na linha 49.

    >**Observação**: esta seção do modelo cria dois discos gerenciados e os anexa a **az104-08-vm1**, de forma semelhante à configuração de armazenamento da primeira máquina virtual por meio do portal do Azure.


1. Clique em **Salvar** e, novamente no painel **Implantação personalizada**, clique em **Revisar + Criar** e, no painel **Revisar + Criar**, clique em **Criar**.

    >**Observação**: aguarde até que a implantação do modelo seja concluída. Você pode monitorar seu progresso a partir do painel **Discos** da máquina virtual **az104-08-vm1**. Isso deve levar menos de 3 minutos.

1. De volta ao painel **az104-08-vm1**, na seção **Operações**, clique em ** Executar comando** e, na lista de comandos, clique em **RunPowerShellScript**.

1. No painel **Executar Script de Comando**, digite o seguinte e clique em **Executar** para criar uma unidade Z: consistindo nos dois discos recém-conectados com o layout simples e provisionamento fixo:

   ```powershell
   New-StoragePool -FriendlyName storagepool1 -StorageSubsystemFriendlyName "Windows Storage*" -PhysicalDisks (Get-PhysicalDisk -CanPool $true)

   New-VirtualDisk -StoragePoolFriendlyName storagepool1 -FriendlyName virtualdisk1 -Size 2046GB -ResiliencySettingName Simple -ProvisioningType Fixed

   Initialize-Disk -VirtualDisk (Get-VirtualDisk -FriendlyName virtualdisk1)

   New-Partition -DiskNumber 4 -UseMaximumSize -DriveLetter Z
   ```

    > **Observação**: aguarde a confirmação de que os comandos foram concluídos com êxito.

## Tarefa 4: registrar os provedores de recursos Microsoft.Insights e Microsoft.AlertsManagement.

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >**Observação**: se esta for a primeira vez que você está iniciando o **Cloud Shell** e você receber a mensagem **Você não tem nenhum armazenamento montado**, selecione a assinatura que você está usando no laboratório e selecione **Criar armazenamento**.

1. No painel do Cloud Shell, execute o seguinte para registrar os provedores de recursos Microsoft.Insights e Microsoft.AlertsManagement.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

## Tarefa 5: implantar conjuntos de dimensionamento de máquinas virtuais do Azure resilientes à zona usando o portal do Azure

Nesta tarefa, você implantará o conjunto de dimensionamento de máquina virtual do Azure em zonas de disponibilidade usando o portal do Azure.

1. No portal do Azure, pesquise e selecione **Conjuntos de dimensionamento de máquina virtual** e, no painel **Conjuntos de dimensionamento de máquina virtual**, clique em **+ Adicionar** (ou **+ Criar**).

1. Na guia **Básico** do painel **Criar conjunto de dimensionamento de máquina virtual**, especifique as seguintes configurações (deixe as outras com seus valores padrão) e clique em **Avançar: Discos >**:

    | Configuração | Valor |
    | --- | --- |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | o nome de um novo grupo de recursos **az104-08-rg02** |
    | Nome do conjunto de dimensionamento de máquinas virtuais | **az10408vmss0** |
    | Region | selecione uma das regiões que dão suporte às zonas de disponibilidade e onde você pode provisionar máquinas virtuais do Azure diferentes daquela usada para implantar máquinas virtuais anteriormente neste laboratório |
    | Zona de disponibilidade | **Zonas 1, 2, 3** |
    | Modo de orquestração | **Uniforme** |
    | Imagem | **Windows Server 2019 Datacenter – Gen2** |
    | Executar com desconto do Spot do Azure | **Não** |
    | Tamanho | **Standard D2s_v3** |
    | Nome de Usuário | **Aluno** |
    | Senha | **Forneça uma senha segura**  |
    | Já tem uma licença do Windows Server? | **Desmarcado** |

    >**Observação**: para obter a lista de regiões do Azure que oferecem suporte à implantação de máquinas virtuais do Windows em zonas de disponibilidade, consulte [O que são zonas de disponibilidade no Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

1. Na guia **Discos** do painel **Criar um conjunto de dimensionamento de máquina virtual**, aceite os valores padrão e clique em **Avançar: Redes >**.

1. Na guia **Rede** do painel **Criar um conjunto de dimensionamento de máquina virtual**, clique no link **Criar rede virtual** abaixo da caixa de texto **Rede virtual** e crie uma nova rede virtual com as seguintes configurações (deixe as outras com seus valores padrão). 

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az104-08-rg02-vnet** |
    | Intervalo de endereços | **10.82.0.0/20** |
    | Nome da sub-rede | **subnet0** |
    | Intervalo de sub-rede | **10.82.0.0/24** |

    >**Observação**: após criar uma nova rede virtual e retornar à guia **Rede** do painel **Criar um conjunto de dimensionamento de máquina virtual**, o valor da **Rede virtual** será definido automaticamente como **az104-08-rg02-vnet**.

1. De volta à guia **Rede** do painel **Criar um conjunto de dimensionamento de máquina virtual**, clique no ícone **Editar interface de rede** à direita da entrada da interface de rede.

1. No painel **Editar interface de rede**, na seção **Grupo de segurança de rede NIC**, clique em **Avançado** e clique em **Criar novo** na lista suspensa **Configurar grupo de segurança de rede**.

1. No painel **Criar grupo de segurança de rede**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az10408vmss0-nsg** |

1. Clique em **Adicionar uma regra de entrada**e adicione uma regra de segurança de entrada com as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Fonte | **Qualquer** |
    | Intervalos de portas de origem | **\*** |
    | Destino | **Qualquer** |
    | Intervalos de portas de destino | **80** |
    | Protocolo | **TCP** |
    | Ação | **Permitir** |
    | Prioridade | **1010** |
    | Nome | **custom-allow-http** |

1. Clique em **Adicionar** e, novamente no painel **Criar grupo de segurança de rede**, clique em **OK**.

1. De volta à folha **Editar adaptador de rede**, na seção **Endereço IP público**, clique em **Ativado** e em **OK**.

1. De volta à guia **Rede** da folha **Criar um conjunto de dimensionamento de máquinas virtuais**, na seção **Balanceamento de carga**, especifique o seguinte (deixe as outras com seus valores padrão).

    | Configuração | Valor |
    | --- | --- |
    | Opções de balanceamento de carga | **Balanceador de carga do Azure** |
    | Selecionar um balanceador de carga | **Criar um balanceador de carga** |
    
1.  Na página **Criar um balanceador de carga**, especifique o nome do balanceador de carga e use os padrões. Clique em **Criar** quando terminar e, em seguida, em **Avançar : Dimensionamento >**.
    
    | Configuração | Valor |
    | --- | --- |
    | Nome do balanceador de carga | **az10408vmss0-lb** |

1. Na guia **Dimensionamento** da folha **Criar um conjunto de dimensionamento de máquinas virtuais**, especifique as configurações a seguir (deixe as outras com os valores padrão) e clique em **Avançar : Gerenciamento >**:

    | Configuração | Valor |
    | --- | --- |
    | Contagem de instâncias inicial | **2** |
    | Política de dimensionamento | **Manual** |

1. Na guia **Gerenciamento** da folha **Criar um conjunto de dimensionamento de máquinas virtuais**, defina as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Diagnóstico de inicialização | **Habilitar com a conta de armazenamento personalizada** |
    | Conta de armazenamento de diagnóstico | aceitar o valor padrão |

    >**Observação**: você precisará do nome dessa conta de armazenamento na próxima tarefa.

   Clique em **Avançar : Integridade >**:

1. Na guia **Integridade** da folha **Criar um conjunto de dimensionamento de máquinas virtuais**, analise as configurações padrão sem fazer alterações e clique em **Avançar : Avançado >**.

1. Na guia **Avançado** da folha **Criar um conjunto de dimensionamento de máquinas virtuais**, defina as configurações a seguir (deixe as outras com seus valores padrão) e clique em **Revisar + criar**.

    | Configuração | Valor |
    | --- | --- |
    | Difusão do algoritmo | **Difusão fixa (não recomendada com zonas)** |

    >**Observação**: A configuração de **Propagação máxima** não está funcional no momento.

1. Na guia **Revisar + criar** da folha **Criar um conjunto de dimensionamento de máquinas virtuais**, verifique se a validação passou e clique em **Criar**.

    >**Observação**: aguarde a conclusão da implantação do conjunto de dimensionamento de máquinas virtuais. Isso deverá levar cerca de cinco minutos.

## Tarefa 6: configurar conjuntos de dimensionamento de máquinas virtuais do Azure usando extensões de máquina virtual

Nesta tarefa, você instalará a função de Servidor Web do Windows Server nas instâncias do conjunto de dimensionamento de máquinas virtuais do Azure implantado na tarefa anterior usando a extensão de máquina virtual de Script Personalizado.

1. No portal do Azure, pesquise e selecione **Contas de armazenamento** e na folha **Contas de armazenamento**, clique na entrada que representa a conta de armazenamento de diagnóstico que você criou na tarefa anterior.

1. Na folha da conta de armazenamento, na seção **Armazenamento de Dados**, clique em **Contêineres** e, em seguida, clique em **+ Contêiner**.

1. Na folha **Novo Contêiner**, especifique as seguintes configurações (deixe as outras com seus valores padrão) e clique em **Criar**:

    | Configuração | Valor |
    | --- | --- |
    | Nome | **scripts** |
    | Nível de acesso público | **Privado (sem acesso anônimo)**) |

1. De volta à folha da conta de armazenamento que exibe a lista de contêineres, clique em **scripts**.

1. Na folha **scripts**, clique em **Carregar**.

1. Na folha **Carregar blob**, clique no ícone da pasta, na caixa de diálogo **Abrir**, navegue para a pasta **\\Allfiles\\Laboratórios\\08**, selecione **az104-08-install_IIS.ps1**, clique em **Abrir** e volte para a folha **Carregar blob** e clique em **Carregar**.

1. No portal do Azure, navegue de volta para a folha **Conjuntos de dimensionamento de máquinas virtuais** e clique em **az10408vmss0**.

1. Na folha **az10408vmss0**, na seção **Configurações**, clique em **Extensões e aplicativos** e, em seguida, clique em **+ Adicionar**.

1. Na folha **Novo recurso**, clique em **Extensão de Script Personalizado** e, em seguida, clique em **Avançar**.

1. Na folha **Instalar extensão**, **Navegue** para e **Selecione** o script **az104-08-install_IIS.ps1** que foi carregado para o contêiner **scripts** na conta de armazenamento anteriormente nesta tarefa e, em seguida, clique em **Criar**.

    >**Observação**: Aguarde a conclusão da instalação da extensão antes de prosseguir para a próxima etapa.

1. Na seção **Configurações** da folha **az10408vmss0**, clique em **Instâncias**, selecione as caixas de seleção ao lado das duas instâncias do conjunto de dimensionamento de máquinas virtuais, clique em **Atualizar** e, em seguida, quando solicitar a confirmar, clique em **Sim**.

    >**Observação:** aguarde a conclusão da atualização antes de prosseguir para a próxima etapa.

1. No portal do Azure, procure e selecione **Balanceadores de carga** e, na lista de balanceadores de carga, clique em **az10408vmss0-lb**.

1. Na folha **az10408vmss0-lb**, observe o valor do **Endereço IP público** atribuído ao front-end do balanceador de carga, abra uma nova guia do navegador e navegue até esse endereço IP.

    >**Observação**: verifique se a página do navegador exibe o nome de uma das instâncias do conjunto de dimensionamento de máquinas virtuais do Azure **az10408vmss0**.

## Tarefa 7: dimensionar a computação e o armazenamento dos conjuntos de dimensionamento de máquinas virtuais do Azure

Nesta tarefa, você alterará o tamanho das instâncias do conjunto de dimensionamento de máquina virtual, definirá suas configurações de dimensionamento automático e anexará discos a elas.

1. No portal do Azure, pesquise e selecione **Conjuntos de dimensionamento de máquinas virtuais** e selecione o conjunto de dimensionamento **az10408vmss0**

1. Na folha **az10408vmss0**, na seção **Configurações**, clique em **Tamanho**.

1. Na lista de tamanhos disponíveis, selecione **Standard DS1_v2** e clique em **Redimensionar**.

1. Na seção **Configurações**, clique em **Instâncias**, marque as caixas de seleção ao lado das duas instâncias do conjunto de dimensionamento de máquinas virtuais, clique em **Atualizar** e, em seguida, quando precisar confirmar, clique em **Sim**.

1. Na lista de instâncias, clique na entrada que representa a primeira instância e, na folha de instância do conjunto de escala, observe seu **Local** (deve ser uma das zonas na região do Azure de destino na qual você implantou o conjunto de dimensionamento de máquinas virtuais do Azure).

1. Retorne à folha **folha az10408vmss0 - Instâncias**, clique na entrada que representa a segunda instância e, na folha de instância do conjunto de escala, anote seu **Local** (deve ser uma das outras duas zonas na região do Azure de destino na qual você implantou o conjunto de dimensionamento de máquinas virtuais do Azure).

1. Retorne à folha **az10408vmss0 - Instâncias** e, na seção **Configurações**, clique em **Dimensionamento**.

1. Na folha **az10408vmss0 - Dimensionamento**, selecione a opção **Dimensionamento personalizado automático** e configure o dimensionamento automático com as seguintes configurações (deixe outros com seus valores padrão):

    | Configuração | Valor |
    | --- |--- |
    | Modo de escala | **Escala baseada em uma métrica** |

1. Clique no link **+ Adicionar uma regra** e, na folha **Regra de escala**, especifique as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | --- |--- |
    | Origem da métrica | **Recurso atual (az10480vmss0)** |
    | Namespace da métrica | **Host de Máquina Virtual** |
    | Nome da métrica | **Rede no Total** |
    | Operador | **Maior que** |
    | Limite de métrica para disparar ação de escala | **10** |
    | Duração (em minutos) | **1** |
    | Estatística de intervalo de agregação | **Média** |
    | Agregação de tempo | **Média** |
    | Operação | **Aumentar a contagem em** |
    | Contagem de instâncias | **1** |
    | Tempo de resfriamento (minutos) | **5** |

    >**Observação**: obviamente esses valores não representam uma configuração realista, uma vez que seu objetivo é acionar o dimensionamento automático o mais rápido possível, sem período de espera prolongado.

1. Clique em **Adicionar** e de volta à folha **az10408vmss0 - Dimensionamento**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- |--- |
    | Limites mínimos de instância | **1** |
    | Limites máximos de instância | **3** |
    | Limites padrão de instância | **1** |

1. Clique em **Save** (Salvar).

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

1. No painel do Cloud Shell, execute o seguinte para identificar o endereço IP público do balanceador de carga na frente do conjunto de dimensionamento de máquinas virtuais do Azure **az10408vmss0**.

   ```powershell
   $rgName = 'az104-08-rg02'

   $lbpipName = 'az10408vmss0-lb-publicip'

   $pip = (Get-AzPublicIpAddress -ResourceGroupName $rgName -Name $lbpipName).IpAddress
   ```

1. No painel do Cloud Shell, execute o seguinte para iniciar um loop infinito que envia as solicitações HTTP aos sites hospedados nas instâncias do conjunto de dimensionamento de máquinas virtuais do Azure **az10408vmss0**.

   ```powershell
   while ($true) { Invoke-WebRequest -Uri "http://$pip" }
   ```

1. Minimize o painel do Cloud Shell, mas não o feche, volte para a folha ** az10408vmss0 - Instâncias** e monitore o número de instâncias.

    >**Observação**: talvez seja necessário aguardar alguns minutos e clicar em **Atualizar**.

1. Depois que a terceira instância for provisionada, navegue até sua folha para determinar seu **Local** (deve ser diferente das duas primeiras zonas identificadas anteriormente nesta tarefa.

1. Feche o painel do Cloud Shell.

1. Na folha **az10408vmss0**, na seção **Configurações**, clique em **Discos**, selecione **+ Criar e anexar um novo disco** e anexe um novo disco gerenciado com as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | LUN | **0** |
    | Tipo de armazenamento | **HDD Standard** |
    | Tamanho (GiB) | **32** |

1. Aplique a alteração

1. Na seção **Configurações** da folha **az10408vmss0**, clique em **Instâncias**, selecione as caixas de seleção próximas às instâncias do conjunto de dimensionamento de máquinas virtuais, clique em **Atualizar** e, quando for solicitada a confirmação, selecione **Sim**.

    >**Observação**: o disco anexado na etapa anterior é um disco bruto. Antes que o disco possa ser usado, é necessário criar uma partição, um sistema de arquivos e montá-lo. Para fazer isso, você usará a extensão de Script Personalizado da máquina virtual do Azure. Primeiro, você precisará remover a Extensão de Script Personalizado existente.

1. Na seção **Configurações** da folha **az10408vmss0**, clique em **Extensões e aplicativos**, em **CustomScriptExtension** e selecione **Desinstalar**.

    >**Observação**: aguarde a conclusão da desinstalação.

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

1. Na barra de ferramentas do painel do Cloud Shell, clique no ícone **Carregar/Baixar arquivos**, no menu suspenso, clique em **Carregar** e carregue o arquivo **\\Allfiles\\Labs\\08\\az104-08-configure_VMSS_disks.ps1** no diretório base do Cloud Shell.

1. No painel do Cloud Shell, execute o seguinte para exibir o conteúdo do script:

   ```powershell
   Set-Location -Path $HOME

   Get-Content -Path ./az104-08-configure_VMSS_disks.ps1
   ```

    >**Observação**: o script instala uma extensão de script personalizada que configura o disco anexado.

1. No painel do Cloud Shell, execute o seguinte para executar o script e configurar discos do conjunto de dimensionamento de máquinas virtuais do Azure:

   ```powershell
   ./az104-08-configure_VMSS_disks.ps1
   ```

1. Feche o painel do Cloud Shell.

1. Na seção **Configurações** da folha **az10408vmss0**, clique em **Instâncias**, selecione as caixas de seleção próximas às instâncias do conjunto de dimensionamento de máquinas virtuais, clique em **Atualizar** e, quando for solicitada a confirmação, selecione **Sim**.

## Limpar os recursos

>**Observação**: lembre-se de remover todos os recursos recém-criados do Azure que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Observação**: não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos. Portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 
1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Remova az104-08-configure_VMSS_disks.ps1 executando o seguinte comando:

   ```powershell
   rm ~\az104-08*
   ```

1. Liste todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-08*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-08*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Observação**: o comando é executado de maneira assíncrona (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente após na mesma sessão do PowerShell, levará alguns minutos antes dos grupos de recursos serem de fato removidos.

## Revisão

Neste laboratório, você vai:

+ Implantou máquinas virtuais do Azure resilientes à zona usando o portal do Azure e um modelo do Azure Resource Manager
+ Configurou máquinas virtuais do Azure usando extensões de máquina virtual
+ Dimensionou a computação e o armazenamento para as máquinas virtuais do Azure
+ Implantou conjuntos de dimensionamento de máquinas virtuais do Azure resilientes à zona usando o portal do Azure.
+ Configurou conjuntos de dimensionamento de máquinas virtuais do Azure usando extensões de máquina virtual
+ Dimensionou a computação e o armazenamento para conjuntos de dimensionamento de máquinas virtuais do Azure
