---
lab:
  title: 'Laboratório 04: Implementar Redes Virtuais'
  module: Administer Virtual Networking
---

# Laboratório 04 - Implementar Redes Virtuais

# Manual do laboratório do aluno

## Cenário do laboratório

Você precisa explorar os recursos de rede virtual do Azure. Para começar, você planeja criar uma rede virtual no Azure que hospedará algumas máquinas virtuais do Azure. Como você pretende implementar a segmentação baseada em rede, você os implantará em diferentes sub-redes da rede virtual. Você também quer ter certeza de que seus endereços IP privados e públicos não serão alterados ao longo do tempo. Para estar em conformidade com os requisitos de segurança da Contoso, você precisa proteger pontos de extremidade públicos de máquinas virtuais do Azure acessíveis pela Internet. Finalmente, você precisa implementar a resolução de nomes DNS para máquinas virtuais do Azure dentro da rede virtual e da Internet.

**Observação:** uma **[simulação de laboratório interativo](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208)** está disponível e permite que você clique neste laboratório em seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: Criar e configurar uma rede virtual
+ Tarefa 2: implantar máquinas virtuais na rede virtual
+ Tarefa 3: configurar endereços IP públicos e privados de VMs do Azure
+ Tarefa 4: configurar grupos de segurança de rede
+ Tarefa 5: configurar o DNS do Azure para resolução de nomes interna
+ Tarefa 6: configurar o DNS do Azure para resolução de nomes externos

## Tempo estimado: 40 minutos

## Diagrama de arquitetura

![imagem](../media/lab04.png)

### Instruções

## Exercício 1

## Tarefa 1: Criar e configurar uma rede virtual

Nesta tarefa, você criará uma rede virtual com várias sub-redes usando o portal do Azure

1. Entre no [portal do Azure](https://portal.azure.com).

1. No portal do Azure, procure e selecione **Redes virtuais** e, na folha **Redes virtuais**, clique em **+ Criar**.

1. Crie uma rede virtual com as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | o nome da assinatura do Azure que você usará neste laboratório |
    | Grupo de recursos | O nome de um **novo** grupo de recursos **az104-04-rg1** |
    | Nome | **az104-04-vnet1** |
    | Region | o nome de qualquer região do Azure disponível na assinatura que você usará neste laboratório |

1. Clique em **Avançar: Endereços IP**. O **Endereço inicial** é **10.40.0.0**. O **Tamanho do espaço do endereço** é **/20**. Certifique-se de clicar em **Adicionar**. 

1. Clique em **+ Adicionar sub-rede**, insira os valores a seguir e, em seguida, clique em **Adicionar**.

    | Configuração | Valor |
    | --- | --- |
    | Nome da sub-rede | **subnet0** |
    | Endereço inicial | **10.40.0.0** |
    | Tamanho da sub-rede | **/24 (256 endereços)** |

1. Aceite os padrões e clique em **Revisar e Criar**. Deixe a validação ocorrer e clique em **Criar** novamente para enviar sua implantação.

    >**Observação:** aguarde até que a rede virtual seja provisionada. Isso deve levar menos de um minuto.

1. Clique em **Ir para o recurso**

1. Na folha de rede virtual **az104-04-vnet1**, clique em **Sub-redes** e, em seguida, clique em **+ Sub-rede**.

1. Crie uma sub-rede com as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Nome | **subnet1** |
    | Intervalo de endereços (bloco CIDR) | **10.40.1.0/24** |
    | Grupo de segurança de rede | **Nenhuma** |
    | Tabela de rotas | **Nenhuma** |

1. Clique em **Salvar**

## Tarefa 2: implantar máquinas virtuais na rede virtual

Nesta tarefa, você implantará máquinas virtuais do Azure em diferentes sub-redes da rede virtual usando um modelo ARM

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >**Observação**: se esta for a primeira vez que você está iniciando o **Cloud Shell** e você receber a mensagem **Você não tem nenhum armazenamento montado**, selecione a assinatura que você está usando no laboratório e selecione **Criar armazenamento**.

1. Na barra de ferramentas do painel Cloud Shell, clique no ícone **Carregar/Baixar arquivos**, e no menu suspenso, clique em **Carregar**. Faça upload de **\\Allfiles\\Labs\\04\\az104-04-vms-loop-template.json** e **\\Allfiles\\Labs\\04\\az104-04-vms-loop-parameters.json** no diretório base do Cloud Shell.

    >**Observação**: você deve fazer upload de cada arquivo separadamente. Após o upload, use **dir** para garantir que ambos os arquivos foram carregados com êxito.

1. No painel Cloud Shell, execute o seguinte para implantar duas máquinas virtuais usando os arquivos de modelo e parâmetro:
    >**Observação**: você será solicitado a fornecer uma senha.
    
   ```powershell
   $rgName = 'az104-04-rg1'

   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-04-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-04-vms-loop-parameters.json
   ```
   
    >**Observação**: esse método de implantação de modelos ARM usa o Azure PowerShell. Você pode executar a mesma tarefa executando o comando equivalente da CLI do Azure **az deployment create** (para obter mais informações, consulte [Implantar recursos com modelos do Gerenciador de Recursos e CLI do Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-cli).

    >**Observação**: aguarde a conclusão da implantação antes de prosseguir para a próxima tarefa. Isso deverá levar cerca de dois minutos.

    >**Observação**: se você recebeu um erro informando que o tamanho da VM não está disponível, peça ajuda ao seu instrutor e tente realizar estas etapas:
    > 1. Clique no botão `{}` no seu CloudShell, selecione o** az104-04-vms-loop-parameters.json** na barra lateral esquerda e anote o valor do parâmetro `vmSize`.
    > 1. Verifique o local no qual o grupo de recursos 'az104-04-rg1' está implantado. Você pode executar `az group show -n az104-04-rg1 --query location` em seu CloudShell para obter o caminho.
    > 1. Execute `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"` em seu CloudShell. Se não houver SKUs listados (ou seja, não houver resultados), você não poderá implantar nenhuma máquina virtual D2S nessa região. Você precisará encontrar uma região que permita implantar máquinas virtuais D2S. Depois de escolher um local adequado, exclua o grupo de recursos AZ104-04-rg1 e reinicie o laboratório.
    > 1. Substitua o valor do parâmetro `vmSize` por um dos valores retornados pelo comando que você acabou de executar.
    > 1. Agora reimplante seus modelos executando o comando `New-AzResourceGroupDeployment` novamente. Você pode pressionar o botão de seta para cima algumas vezes, o que deve exibir o último comando executado.

1. Feche o painel do Cloud Shell.

## Tarefa 3: configurar endereços IP públicos e privados de VMs do Azure

Nesta tarefa, você configurará a atribuição estática de endereços IP públicos e privados atribuídos às interfaces de rede de máquinas virtuais do Azure.

   >**Observação**: os endereços IP privados e públicos são realmente atribuídos às interfaces de rede, que, por sua vez, são anexadas às máquinas virtuais do Azure, no entanto, é bastante comum fazer referência a endereços IP atribuídos a VMs do Azure.

   >**Observação**: Você precisará de **dois** endereços IP públicos para concluir este laboratório. 

1. No portal do Azure, pesquise e selecione **Endereços IP públicos** e, em seguida, selecione **+ Criar**.

1. Verifique se o **grupo de recursos** é **az104-04-rg1**,

1. Nos **Detalhes da Configuração**, verifique se o **nome** é **az104-04-pip0**.

1. Selecione **Examinar e criar** e, em seguida, **Criar**.

1. No portal do Azure, pesquise e selecione **Endereços IP públicos** e, em seguida, selecione **+ Criar**.

1. Verifique se o **grupo de recursos** é **az104-04-rg1**,

1. Nos **Detalhes da Configuração**, verifique se o **nome** é **az104-04-pip1**.

1. Selecione **Examinar e criar** e, em seguida, **Criar**.

1. No portal do Azure, procure e selecione **Grupos de recursos** e, na folha **Grupos de recursos**, clique em **az104-04-rg1**.

1. Na folha do grupo de recursos **az104-04-rg1**, na lista de seus recursos, clique em **az104-04-vnet1**.

1. Na folha da rede virtual **az104-04-vnet1**, revise a seção **Dispositivos conectados** e verifique se há dois adaptadores de rede **az104-04-nic0** e **az104-04-nic1** anexados à rede virtual.

1. Clique em **az104-04-nic0**e, na folha **az104-04-nic0**, clique em **Configurações de IP**.

    >**Observação**: Verifique se **ipconfig1** está atualmente configurado com um endereço IP privado dinâmico.

1. Na lista de configurações de IP, clique em **ipconfig1**.

1. Verifique se a **Alocação** é **Estática**.

1. Selecione **Associar endereço IP público** e, na lista suspensa **Endereço IP público**, selecione **az104-04-pip0**.

1. Selecione **Salvar**.

1. Navegue de volta para a folha **folha az104-04-vnet1**.

1. Clique em **az104-04-nic1**e , na folha **az104-04-nic1**, clique em **configurações de IP**.

    >**Observação**: Verifique se **ipconfig1** está atualmente configurado com um endereço IP privado dinâmico.

1. Na lista de configurações de IP, clique em **ipconfig1**.

1. Verifique se a **Alocação** é **Estática**.

1. Selecione **Associar endereço IP público** e, na lista suspensa **Endereço IP público**, selecione **az104-04-pip1**.

1. Selecione **Salvar**.
   
1. Navegue de volta para a folha do grupo de recursos **az104-04-rg1**, na lista de seus recursos, clique em **az104-04-vm0** e, na folha da máquina virtual **az104-04-vm0**, anote a entrada do endereço IP público.

1. Navegue de volta para a folha do grupo de recursos **az104-04-rg1**, na lista de seus recursos, clique em **az104-04-vm1** e, na folha da máquina virtual **az104-04-vm1**, anote a entrada do endereço IP público.

    >**Observação**: Você precisará de ambos os endereços IP na última tarefa deste laboratório.

## Tarefa 4: configurar grupos de segurança de rede

Nesta tarefa, você configurará grupos de segurança de rede para permitir conectividade restrita com máquinas virtuais do Azure.

1. No portal do Azure, navegue de volta para a folha do grupo de recursos **az104-04-rg1** e, na lista de seus recursos, clique em **az104-04-vm0**.

1. Na folha de visão geral **az104-04-vm0**, clique em **Conectar**, clique em **RDP** no menu suspenso, na folha **Conectar com RDP**, clique em **Fazer Download de Arquivo RDP**.

1. Verifique se há falha na tentativa de conexão.

    >**Observação**: Isso é esperado, porque os endereços IP públicos da SKU padrão, por padrão, exigem que as interfaces de rede às quais eles são atribuídos sejam protegidas por um grupo de segurança de rede. Para permitir conexões de Área de Trabalho Remota, você criará um grupo de segurança de rede permitindo explicitamente o tráfego RDP de entrada da Internet e o atribuirá às interfaces de rede de ambas as máquinas virtuais.

1. Pare as máquinas virtuais **az104-04-vm0** e **az104-04-vm1**.

    >**Observação**: Isso é feito por conveniência do laboratório. Se as máquinas virtuais estiverem em execução quando um grupo de segurança de rede estiver conectado à interface de rede, poderá levar mais de 30 minutos para que o anexo entre em vigor. Depois que o grupo de segurança de rede tiver sido criado e anexado, as máquinas virtuais serão reiniciadas e o anexo entrará em vigor imediatamente.

1. No portal do Azure, procure e selecione **Grupos de segurança de rede** e, na folha **Grupos de segurança de rede**, clique em **+ Criar**.

1. Crie um grupo de segurança de rede com as seguintes configurações (deixe outros com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | **az104-04-rg1** |
    | Nome | **az104-04-nsg01** |
    | Region | o nome da região do Azure onde você implantou todos os outros recursos neste laboratório |

1. Clique em **Revisar e criar**. Deixe a validação ocorrer e pressione **Criar** para enviar sua implantação.

    >**Observação**: aguarde até que a implantação seja concluída. Isso deverá levar cerca de dois minutos.

1. Na folha de implantação, clique em **Ir para recurso** para abrir a folha do grupo de segurança de rede **az104-04-nsg01**.

1. Na folha do grupo de segurança de rede **az104-04-nsg01**, clique na seção **Configurações** e clique em **Regras de segurança de entrada**.

1. Adicione uma regra de entrada com as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Fonte | **Qualquer** |
    | Intervalos de portas de origem | * |
    | Destino | **Qualquer** |
    | Serviço | **RDP** |
    | Ação | **Permitir** |
    | Prioridade | **300** |
    | Nome | **AllowRDPInBound** |

1. Na folha **az104-04-nsg01** do grupo de segurança de rede, na seção **Configurações**, clique em **Interfaces de rede** e, em seguida, clique em  **+ Associar**.

1. Associe o grupo de segurança de rede **az104-04-nsg01** às interfaces de rede **az104-04-nic0** e **az104-04-nic1**.

    >**Observação**: pode levar até 5 minutos para que as regras do Grupo de Segurança de Rede recém-criado sejam aplicadas à Placa de Interface de Rede.

1. Inicie as máquinas virtuais **az104-04-vm0** e **az104-04-vm1**.

1. Navegue de volta para a folha da máquina virtual **az104-04-vm0**.

    >**Observação**: nas etapas subsequentes, você verificará se pode se conectar com êxito à máquina virtual de destino.

1. Na folha **az104-04-vm0**, clique em **Conectar** e clique em **RDP**. Na folha **Conectar com RDP**, clique em **Baixar arquivo RDP** usando o endereço IP público e siga as instruções para iniciar a sessão de área de trabalho remota.

    >**Observação**: esta etapa refere-se à conexão via Área de Trabalho Remota a partir de um computador Windows. Em um Mac, você pode usar o Cliente de Área de Trabalho Remota na Mac App Store e, em computadores Linux, pode usar um software cliente RDP de código aberto.

    >**Observação**: você pode ignorar quaisquer prompts de aviso ao se conectar às máquinas virtuais de destino.

1. Quando solicitado, entre com um nome de usuário e senha.

    >**Observação**: deixe a sessão de ambiente de trabalho remoto aberta. Isso será necessário na próxima tarefa.

## Tarefa 5: configurar o DNS do Azure para resolução de nomes interna

Nesta tarefa, você configurará a resolução de nomes DNS em uma rede virtual usando zonas DNS privadas do Azure.

1. No portal do Azure, procure e selecione **Zonas de DNS Privadas** e, na folha **Zonas de DNS Privadas**, clique em **+ Criar**.

1. Crie uma zona de DNS privada com as seguintes configurações (deixe os outros parâmetros com seus valores padrões):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | **az104-04-rg1** |
    | Nome | **contoso.org** |

1. Clique em **Revisar e criar**. Deixe a validação ocorrer e clique em **Criar** novamente para enviar sua implantação.

    >**Observação**: aguarde até que a zona de DNS privada seja criada. Isso deverá levar cerca de dois minutos.

1. Clique em **Ir para o recurso** para abrir a folha de zona DNS privada**contoso.org**.

1. Na folha de zona DNS privada **contoso.org**, na seção **Configurações**, clique em **Links de rede virtual**

1. Clique em **+ Adicionar** para criar um link de rede virtual com as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Nome do link | **az104-04-vnet1-link** |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Rede virtual | **az104-04-vnet1** |
    | Habilitar o registro automático | Habilitado |

1. Clique em **OK.**

    >**Observação:** aguarde até que o link de rede virtual seja criado. Isso deverá levar menos de 1 minuto.

1. Na folha de zona DNS privada **contoso.org**, na barra lateral, clique em **Visão geral**

1. Verifique se os registros DNS para **az104-04-vm0** e **az104-04-vm1** aparecem na lista de conjuntos de registros como **Registrado automaticamente**.

    >**Observação:** talvez seja necessário aguardar alguns minutos e atualizar a página se os conjuntos de registros não estiverem listados.

1. Alterne para a sessão da Área de Trabalho Remota para **az104-04-vm0**, clique com o botão direito do mouse no botão **Iniciar** e, no menu do botão direito do mouse, clique em **Windows PowerShell (Administrador)**.

1. Na janela do console do Windows PowerShell, execute o seguinte para testar a resolução de nomes internos na zona DNS privada recém-criada:

   ```powershell
   nslookup az104-04-vm0.contoso.org
   nslookup az104-04-vm1.contoso.org
   ```

1. Verifique se a saída do comando inclui o endereço IP privado de **az104-04-vm1** (**10.40.1.4**).

## Tarefa 6: configurar o DNS do Azure para resolução de nomes externos

Nesta tarefa, você configurará a resolução de nomes DNS externos usando zonas DNS públicas do Azure.

1. Em um navegador da Web, abra uma nova guia e navegue até <https://www.godaddy.com/domains/domain-name-search>.

1. Use a pesquisa de nome de domínio para identificar um nome de domínio que não está em uso.

1. No portal do Azure, procure e selecione **zonas DNS** e, na folha **Zonas DNS**, clique em **+ Criar**.

1. Crie uma zona DNS com as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | **az104-04-rg1** |
    | Nome | o nome de domínio DNS que você identificou anteriormente nesta tarefa |

1. Clique em **Revisar e criar**. Deixe a validação ocorrer e clique em **Criar** novamente para enviar sua implantação.

    >**Observação**: aguarde até que a zona DNS seja criada. Isso deverá levar cerca de dois minutos.

1. Clique em **Ir para recurso** para abrir a folha da zona DNS recém-criada.

1. Na folha zona DNS, clique em **+ Conjunto de registros**.

1. Adicione um conjunto de registros com as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az104-04-vm0** |
    | Tipo | **A** |
    | Conjunto de registros de alias | **Não** |
    | TTL | **1** |
    | Unidade de TTL | **Horas** |
    | Endereço IP | o endereço IP público de **az104-04-vm0** que você identificou no terceiro exercício deste laboratório |

1. Clique em **OK**

1. Na folha zona DNS, clique em **+ Conjunto de registros**.

1. Adicione um conjunto de registros com as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Nome | **az104-04-vm1** |
    | Tipo | **A** |
    | Conjunto de registros de alias | **Não** |
    | TTL | **1** |
    | Unidade de TTL | **Horas** |
    | Endereço IP | o endereço IP público de **az104-04-vm1** que você identificou no terceiro exercício deste laboratório |

1. Clique em **OK**

1. Na folha zona DNS, anote o nome da entrada **Servidor de nomes 1**.

1. No portal do Azure, abra a sessão do **PowerShell** no ** Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. No painel do Cloud Shell, execute o seguinte para testar a resolução de nomes externa do conjunto de registros DNS **az104-04-vm0** na zona DNS recém-criada (substitua o espaço reservado `[Name server 1]` pelo nome do **Servidor de nomes 1** que você anotou anteriormente nesta tarefa e o espaço reservado `[domain name]` pelo nome do domínio DNS criado anteriormente nesta tarefa):

   ```powershell
   nslookup az104-04-vm0.[domain name] [Name server 1]
   ```

1. Verifique se a saída do comando inclui o endereço IP público de **az104-04-vm0**.

1. No painel do Cloud Shell, execute o seguinte para testar a resolução de nomes externa do conjunto de registros DNS **az104-04-vm1** na zona DNS recém-criada (substitua o espaço reservado `[Name server 1]` pelo nome do **Servidor de nomes 1** que você anotou anteriormente nesta tarefa e o espaço reservado `[domain name]` pelo nome do domínio DNS criado anteriormente nesta tarefa):

   ```powershell
   nslookup az104-04-vm1.[domain name] [Name server 1]
   ```

1. Verifique se a saída do comando inclui o endereço IP público de **az104-04-vm1**.

## Limpar os recursos

 > **Observação**: lembre-se de remover todos os recursos recém-criados do Azure que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

 > **Observação**: não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos. Portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Liste todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Observação**: o comando é executado de maneira assíncrona (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente após na mesma sessão do PowerShell, levará alguns minutos antes dos grupos de recursos serem de fato removidos.

## Revisão

Neste laboratório, você vai:

+ Criou e configurou uma rede virtual.
+ Implantar máquinas virtuais na rede virtual
+ Configurou endereços IP públicos e privados de VMs do Azure
+ Configurou grupos de segurança de rede
+ Configurou o DNS do Azure para resolução de nomes interna
+ Configurou o DNS do Azure para resolução de nomes externa.
