---
demo:
  title: 'Demonstração 03: administrar recursos do Azure'
  module: Administer Administer Azure Resources
---
# 03 – Administrar Recursos do Azure

## Demonstração -- Portal do Azure

Nesta demonstração, exploraremos o portal do Azure.

**Referência**: [ Gerenciar configurações e preferências do portal do Azure](https://docs.microsoft.com/azure/azure-portal/set-preferences)

**Referência**: [Criar um painel no portal do Azure](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)

**Referência**: [Como criar uma solicitação de suporte do Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

1. Acessar o portal do Azure.

1. Selecione o ícone  **Suporte e Solução de problemas** no banner superior. Revise os links **Recursos de suporte**. 

1. Selecione o ícone **Configurações** no banner superior. Revise as configurações **Aparência + exibições de inicialização**. 

1. Use o menu do lado esquerdo e selecione **Dashboard**. **Edite** o painel usando a **Galeria de blocos**. Discuta as opções de personalização.

1. Mostrar como pesquisar e localizar recursos.

1. Use o menu superior esquerdo para localizar **Todos os serviços**. 

1. À medida que tiver tempo, analise outros recursos.
   
1. Pergunte se os alunos têm alguma dúvida.

## Demonstração -- Cloud Shell

Nesta demonstração, vamos experimentar o Cloud Shell.

**Referência**: [Início rápido para o Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?tabs=azurecli)

**Configurar o Cloud Shell**

1.  Acessar o  **Portal do Azure**.

1.  Clique no ícone do  **Cloud Shell** no banner superior.

1.  Na página Bem-vindo ao Shell, observe suas seleções para o Bash ou o PowerShell. Selecione **PowerShell**.

1.  Discute como o Azure Cloud Shell precisa de um compartilhamento de arquivos do Azure para persistir arquivos. Se necessário, configure o compartilhamento de armazenamento. 

**Experimente o Azure PowerShell e o Bash**

1. Verifique se o shell do **PowerShell** está selecionado e experimente executar alguns comandos. Por exemplo, **Get-AzSubscription** e **Get-AzResourceGroup**.

1. Mostre como funciona o preenchimento automático. Mostre como limpar a tela, **cls**. 

1. Verifique se o shell **Bash** está selecionado e experimente realizar alguns comandos. Por exemplo, **az account list** e **az resource list**.

1. Pergunte se os alunos têm alguma dúvida sobre como usar os comandos do PowerShell ou do Bash. 

**Experimente o editor do Cloud shell (opcional)**

1. Para usar o Editor de Nuvem, selecione o ícone de **chaves**.

1. Selecione um arquivo no painel de navegação esquerdo. Por exemplo,  **.profile**.

1. No banner superior do editor, observe as seleções em Configurações, (Tamanho do Texto e Fonte) e arquivos para Upload/Download.

1. Observe as reticências (**\...**) no canto direito para **Salvar**, **Fechar o Editor** e **Abrir Arquivo**.

1. Experimente à medida que tiver tempo e, em seguida **, feche** o Editor de Nuvem.

1. Feche o Cloud Shell.

## Demonstração -- Modelos de Início Rápido

Nesta demonstração, exploraremos os modelos de Início Rápido.

**Referência**: [Tutorial - Criar e implantar o modelo - Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

1. Para começar, navegue até a  [galeria de Modelos de Início Rápido do Azure](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager). Observe que há exemplos JSON e Bicep. 

1. Pergunte aos alunos se há algum modelo específico que seja de interesse. Caso contrário, selecione um modelo. Por exemplo, o modelo [Implantar uma VM simples do Windows com marcas](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/vm-tags/) .

1. O botão  **Implantar no Azure**  permite que você implante o modelo diretamente por meio do portal do Azure.

1. **Implante** o modelo JSON e discuta como você pode editar o modelo e o arquivo de parâmetros. Revise a finalidade dos arquivos. À medida que tiver tempo, revise a sintaxe. 

1. Retorne à galeria de exemplos de código e localize um modelo Bicep. Por exemplo, [Criar uma conta de armazenamento padrão](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/storage-account-create/). 

1. **Implante** o modelo Bicep e discuta como você pode editar o modelo e o arquivo de parâmetros. À medida que tiver tempo, revise a sintaxe. 
