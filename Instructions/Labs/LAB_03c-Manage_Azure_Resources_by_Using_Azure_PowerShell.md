---
lab:
  title: 'Laboratório: gerenciar recursos do Azure com o Azure PowerShell (opcional)'
  module: Administer Azure Resources
---

# Gerenciar recursos do Azure usando o Azure PowerShell
# Manual do aluno

## Cenário do laboratório

Agora que você explorou os recursos básicos de administração do Azure associados ao provisionamento de recursos e organizou-os com base em grupos de recursos usando o portal do Azure e os modelos do Gerenciador de Recursos do Azure, você precisa executar a tarefa equivalente usando o Azure PowerShell. Para evitar a instalação de módulos do PowerShell do Azure, você aproveitará o ambiente do PowerShell disponível no Azure Cloud Shell.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%206)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: Iniciar uma sessão do PowerShell no Azure Cloud Shell
+ Tarefa 2: Criar um grupo de recursos e um disco gerenciado usando o PowerShell.
+ Tarefa 3: Configurar o disco gerenciado usando o Azure PowerShell.

## Tempo estimado: 20 minutos

## Diagrama de arquitetura

![imagem](../media/lab03c.png)

### Instruções

> **Nota**: Sempre crie sua própria senha segura para qualquer máquina virtual ou conta de usuário que você criar. Se a máquina virtual for criada para você, use **Redefinir senha no Portal para atualizar a senha** . 

## Exercício 1

## Tarefa 1: Iniciar uma sessão do PowerShell no Azure Cloud Shell

Nesta tarefa, você abrirá uma sessão do PowerShell no Cloud Shell. 

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**. 

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento. 

1. Se solicitado, selecione **Criar armazenamento** e aguarde até que o painel do Azure Cloud Shell seja exibido. 

1. Verifique se o **Bash** aparece no menu suspenso no canto superior esquerdo do painel do Cloud Shell.

## Tarefa 2: Criar um grupo de recursos e um disco gerenciado usando o PowerShell.

Nesta tarefa, você criará um grupo de recursos e um disco gerenciado do Azure usando a sessão do Azure PowerShell no Cloud Shell

1. Para criar um grupo de recursos na mesma região do Azure que o **grupo de recursos az104-03b-rg1** criado no laboratório anterior, a partir da sessão do PowerShell no Cloud Shell, execute o seguinte:

   ```powershell
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. Para recuperar propriedades do grupo de recursos recém-criado, execute o seguinte:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```
1. Para criar um novo disco gerenciado com as mesmas características que você criou nos laboratórios anteriores deste módulo, execute o seguinte:

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -SkuName Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. Para recuperar propriedades do disco recém-criado, execute o seguinte:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

## Tarefa 3: Configurar o disco gerenciado usando o Azure PowerShell.

Nesta tarefa, você gerenciará a configuração do disco gerenciado do Azure usando a sessão do Azure PowerShell no Cloud Shell. 

1. Para aumentar o tamanho do disco gerenciado do Azure para **64 GB,** na sessão do PowerShell no Cloud Shell, execute o seguinte:

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Para verificar se as alterações entraram em vigor, execute o seguinte comando:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. Para verificar a SKU atual como **Standard_LRS**, execute o seguinte:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. Para alterar a SKU de desempenho do disco para **Premium_LRS**, na sessão do PowerShell no Cloud Shell, execute o seguinte:

   ```powershell
   New-AzDiskUpdateConfig -SkuName Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Para verificar se as alterações entraram em vigor, execute o seguinte comando:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

## Limpar os recursos

   >**Observação**: não exclua os recursos implantados neste laboratório. Você fará referência a eles no próximo laboratório deste módulo.

## Revisão

Neste laboratório, você vai:

- Tarefa 1: Iniciar uma sessão do PowerShell no Azure Cloud Shell
- Tarefa 2: Criar um grupo de recursos e um disco gerenciado usando o PowerShell.
- Tarefa 3: Configurar o disco gerenciado usando o Azure PowerShell.
