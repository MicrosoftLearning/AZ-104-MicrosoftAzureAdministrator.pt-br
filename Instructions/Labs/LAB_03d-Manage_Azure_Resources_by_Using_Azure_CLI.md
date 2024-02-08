---
lab:
  title: 'Laboratório: gerenciar recursos do Azure com a CLI do Azure (opcional)'
  module: Administer Azure Resources
---

# Gerenciar recursos do Azure usando a CLI do Azure
# Manual do aluno

## Cenário do laboratório

Agora que você explorou os recursos básicos de administração do Azure associados ao provisionamento de recursos e organizou-os com base em grupos de recursos usando o portal do Azure, os modelos do Azure Resource Manager e o Azure PowerShell, você precisa executar a tarefa equivalente usando a CLI do Azure. Para evitar a instalação da CLI do Azure, você aproveitará o ambiente Bash disponível no Azure Cloud Shell.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%207)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: Iniciar uma sessão do Bash no Azure Cloud Shell.
+ Tarefa 2: Criar um grupo de recursos e um disco gerenciado usando a CLI do Azure.
+ Tarefa 3: Configurar o disco gerenciado usando a CLI do Azure.

## Tempo estimado: 20 minutos

## Diagrama de arquitetura

![imagem](../media/lab03d.png)

### Instruções

## Exercício 1

## Tarefa 1: Iniciar uma sessão do Bash no Azure Cloud Shell.

Nesta tarefa, você abrirá uma sessão Bash no Cloud Shell. 

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**. 

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento. 

1. Se solicitado, selecione **Criar armazenamento** e aguarde até que o painel do Azure Cloud Shell seja exibido. 

1. Verifique se o **Bash** aparece no menu suspenso no canto superior esquerdo do painel do Cloud Shell.

## Tarefa 2: Criar um grupo de recursos e um disco gerenciado usando a CLI do Azure.

Nesta tarefa, você criará um grupo de recursos e um disco gerenciado do Azure usando a sessão da CLI do Azure no Cloud Shell.

1. Para criar um grupo de recursos na mesma região do Azure que o **grupo de recursos az104-03c-rg1** criado no laboratório anterior, a partir da sessão Bash no Cloud Shell, execute o seguinte:

   ```sh
   LOCATION=$(az group show --name 'az104-03c-rg1' --query location --out tsv)

   RGNAME='az104-03d-rg1'

   az group create --name $RGNAME --location $LOCATION
   ```
1. Para recuperar propriedades do grupo de recursos recém-criado, execute o seguinte:

   ```sh
   az group show --name $RGNAME
   ```
1. Para criar um novo disco gerenciado com as mesmas características que você criou nos laboratórios anteriores deste módulo, a partir da sessão Bash no Cloud Shell, execute o seguinte:

   ```sh
   DISKNAME='az104-03d-disk1'

   az disk create \
   --resource-group $RGNAME \
   --name $DISKNAME \
   --sku 'Standard_LRS' \
   --size-gb 32
   ```
    >**Nota**: Ao usar a sintaxe de várias linhas, verifique se cada linha termina com barra invertida (`\`) sem espaços à direita e se não há espaços à esquerda no início de cada linha.

1. Para recuperar propriedades do disco recém-criado, execute o seguinte:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME
   ```

## Tarefa 3: Configurar o disco gerenciado usando a CLI do Azure.

Nesta tarefa, você gerenciará a configuração do disco gerenciado do Azure usando a sessão da CLI do Azure no Cloud Shell. 

1. Para aumentar o tamanho do disco gerenciado do Azure para **64 GB,** na sessão Bash no Cloud Shell, execute o seguinte:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --size-gb 64
   ```

1. Para verificar se as alterações entraram em vigor, execute o seguinte comando:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query diskSizeGB
   ```

1. Para alterar a SKU de desempenho do disco para **Premium_LRS**, na sessão Bash no Cloud Shell, execute o seguinte:

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --sku 'Premium_LRS'
   ```

1. Para verificar se as alterações entraram em vigor, execute o seguinte comando:

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query sku
   ```

## Limpar os recursos

 > Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

 > **Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam muito tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].name" --output tsv
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >O comando é executado de modo assíncrono (conforme determinado pelo parâmetro --nowait), portanto, embora você possa executar outro comando da CLI do Azure imediatamente depois na mesma sessão Bash, levará alguns minutos antes de o grupo de recursos ser removido.

## Revisão

Neste laboratório, você vai:

- Tarefa 1: Iniciar uma sessão do Bash no Azure Cloud Shell.
- Tarefa 2: Criar um grupo de recursos e um disco gerenciado usando a CLI do Azure.
- Tarefa 3: Configurar o disco gerenciado usando a CLI do Azure.
