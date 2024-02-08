---
lab:
  title: 'Laboratório: gerenciar recursos do Azure com os modelos do ARM'
  module: Administer Azure Resources
---

# Laboratório: gerenciar recursos do Azure com os modelos do ARM
# Manual do aluno

## Cenário do laboratório
Agora que você explorou os recursos básicos de administração do Azure associados ao provisionamento de recursos e organizou-os com base em grupos de recursos usando o portal do Azure, você precisa executar a tarefa equivalente usando modelos do Gerenciador de Recursos do Azure.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: examinar um modelo do ARM para implantação de um disco gerenciado do Azure.
+ Tarefa 2: criar um disco gerenciado do Azure usando um modelo do ARM.
+ Tarefa 3: examinar a implantação baseada em modelo do ARM do disco gerenciado.

## Tempo estimado: 20 minutos

## Diagrama de arquitetura

![imagem](../media/lab03b.png)

### Instruções

## Exercício 1

## Tarefa 1: examinar um modelo do ARM para implantação de um disco gerenciado do Azure.

1. Entre no [**portal do Azure**](http://portal.azure.com).

1. No portal do Azure, procure por **Grupos de recursos** e selecione essa opção. 

1. Na lista de grupos de recursos, clique em **SalesTeamRG**.

1. Na folha do **grupo de recursos az104-03a-rg1** , na **seção Configurações** , clique em **Implantações**.

1. **Na folha az104-03a-rg1 - Implantações, clique na primeira entrada na lista de implantações**.

1. **Na folha Microsoft.ManagedDisk-XXXXXXXXX** \| Overview**, clique em **Modelo**.

    >**Observação**: revise o conteúdo do modelo e observe que você tem a opção de **baixá-lo** para o computador local, **Adicionar à biblioteca** ou **Implantá-lo** novamente.

1. Clique em **Baixar** e salve o arquivo compactado que contém os arquivos de modelo e parâmetros na pasta Downloads** no computador do **laboratório.

1. **Na folha Microsoft.ManagedDisk-XXXXXXXXX** \| Template**, clique em **Inputs**.

1. Observe o valor do parâmetro de saída  . Isso será necessário na próxima etapa.

1. Extraia o conteúdo do arquivo zip baixado para uma pasta local no computador.

    >**Nota**: Esses arquivos também estão disponíveis como **\\Allfiles Labs 03 az104-03b-md-template.json e **\\Allfiles\\\\Labs\\\\03\\\\az104-03b-md-parameters.json****
    
1. Feche todas as **janelas do Explorador** de Arquivos.

## Tarefa 2: criar um disco gerenciado do Azure usando um modelo do ARM.

1. No portal do Azure, pesquise e selecione **Implantar um modelo personalizado**.

1. Na **página Implantação personalizada**, clique em **Criar seu modelo no editor**.

1. **Na folha Editar modelo**, clique em **Carregar arquivo e carregue o **arquivo** template.json** que você baixou na tarefa anterior.

1. No painel do editor, remova as seguintes linhas:

   ```json
   "sourceResourceId": {
       "type": "String"
   },
   ```

   ```json
   "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

    >**Nota**: Esses parâmetros foram removidos, pois não são aplicáveis à implantação atual. Em particular, os parâmetros sourceResourceId, sourceUri, osType e hyperVGeneration são aplicáveis à criação de um disco do Azure a partir de um arquivo VHD existente.

1. **Salve** as alterações.

1. De volta à **folha Implantação** personalizada, clique em **Editar parâmetros**. 

1. **Na folha Editar parâmetros**, clique em **Carregar arquivo e carregue o **arquivo** parameters.json** que você baixou na tarefa anterior e **Salve** as alterações.

1. De volta à **folha Implantação** personalizada, especifique as seguintes configurações:

    | Configuração | Valor |
    | --- |--- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | O nome de um novo grupo de recursos aks-01-RG |
    | Region | o nome de qualquer região do Azure disponível na assinatura que você está usando neste laboratório |
    | Nome do disco | az104-03b-disk1 |
    | Location | O valor do parâmetro Location que você anotou na tarefa anterior |
    | Sku | Standard_LRS |
    | Tamanho do disco (GB) | **32** |
    | Criar opção | **empty** |
    | Tipo de criptografia de disco | **EncryptionAtRestWithPlatformKey** |
    | Modo de acesso a dados | Nenhum |
    | Política de acesso de rede pública | AllowAll |
    | Acesso à rede pública | Desabilitadas |

1. Selecione **Examinar + Criar** e **Criar**.

1. Verifique se a implantação foi concluída com êxito.

## Tarefa 3: examinar a implantação baseada em modelo do ARM do disco gerenciado.

1. No portal do Azure, procure por **Grupos de recursos** e selecione essa opção. 

1. Na lista de grupos de recursos, clique em **SalesTeamRG**.

1. Na folha do **grupo de recursos az104-03b-rg1** , na **seção Configurações** , clique em **Implantações**.

1. **Na folha az104-03b-rg1 - Implantações, clique na primeira entrada na lista de implantações** e revise o **conteúdo das folhas Entrada** e **Modelo**.

## Limpar os recursos

   >**Observação**: não exclua os recursos implantados neste laboratório. Você fará referência a eles no próximo laboratório deste módulo.

## Revisão

Neste laboratório, você vai:

- Tarefa 1: examinar um modelo do ARM para implantação de um disco gerenciado do Azure.
- Tarefa 2: criar um disco gerenciado do Azure usando um modelo do ARM.
- Tarefa 3: examinar a implantação baseada em modelo do ARM do disco gerenciado.
