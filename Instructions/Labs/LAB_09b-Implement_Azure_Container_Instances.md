---
lab:
  title: 'Laboratório 09b: implementar Instâncias de Contêiner do Azure'
  module: Administer PaaS Compute Options
---

# Laboratório 09b – Implementar Instâncias de Contêiner do Azure
# Manual do laboratório do aluno

## Cenário do laboratório

A Contoso deseja encontrar uma nova plataforma para suas cargas de trabalho virtualizadas. Você identificou várias imagens de contêiner que podem ser aproveitadas para atingir esse objetivo. Como você deseja minimizar o gerenciamento de contêiner, planeja avaliar o uso de Instâncias de Contêiner do Azure para implantação de imagens do Docker.

**Observação:** uma **[simulação de laboratório interativa](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014)** está disponível que permite que você navegue neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

- Tarefa 1: implantar Instâncias de Contêiner do Azure usando uma imagem do Docker.
- Tarefa 2: examinar a funcionalidade da Instância de Contêiner do Azure.

## Tempo estimado: 20 minutos

## Diagrama de arquitetura

![imagem](../media/lab09b.png)

### Instruções

## Exercício 1

## Tarefa 1: implantar Instâncias de Contêiner do Azure usando uma imagem do Docker.

Nesta tarefa, você criará uma nova instância de contêiner para o aplicativo Web.

1. Entre no [portal do Azure](https://portal.azure.com).

1. No portal do Azure, procure localizar **instâncias de contêiner** e, na folha **Instâncias de Contêiner**, clique em **+ Criar**.

1. Na guia **Básico** do painel **Criar conta de armazenamento**, defina as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | ---- | ---- |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | O nome de um novo grupo de recursos **aks-01-RG** |
    | Nome do contêiner | **az104-9b-c1** |
    | Region | o nome de uma região onde você pode provisionar instâncias de contêiner do Azure |
    | Origem da imagem | **Imagens de início rápido** |
    | Imagem | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Clique em **Avançar: Rede >** e, na guia **Rede** do painel **Criar instância de contêiner**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Rótulo do nome DNS | qualquer nome do host DNS global exclusivo e válido |

    >**Observação**: seu contêiner será acessível publicamente em dns-name-label.region.azurecontainer.io. Se você receber uma mensagem de erro **Rótulo de nome do DNS não disponível**, tente usar um rótulo de nome do DNS diferente.

1. Clique em **Avançar: Avançado >**, revise as configurações na guia **Avançado** do painel **Criar instância de contêiner** sem fazer alterações, clique em **Revisar + Criar**, verifique se a validação foi aprovada e clique em **Criar**.

    >**Observação**: aguarde até que a implantação seja concluída. Isso deverá levar cerca de 3 minutos.

    >**Observação**: enquanto espera, pode ser interessante visualizar o [código por trás do aplicativo de exemplo](https://github.com/Azure-Samples/aci-helloworld). Para exibi-lo, navegue pela pasta do \\aplicativo.

## Tarefa 2: examinar a funcionalidade da Instância de Contêiner do Azure.

Nesta tarefa, você analisará a implantação da instância de contêiner.

1. Na folha de implantação, clique no link **Ir para o recurso**.

1. Na folha **Visão geral** da instância do contêiner, verifique se o **Status** é relatado como **Em execução**.

1. Copie o valor da instância do contêiner **FQDN**, abra uma nova guia do navegador e navegue até a URL correspondente.

1. Verifique se a página **Bem-vindo(a) à Instância do Contêiner do Azure** é exibida.

1. Feche a nova guia do navegador, de volta no portal do Azure, na seção **Configurações** da folha da instância de contêiner, clique em **Contêineres** e, em seguida, clique em **Logs**.

1. Verifique se você vê as entradas de log que representam a solicitação HTTP GET gerada exibindo o aplicativo no navegador.

## Limpar os recursos

>**Observação**: lembre-se de remover todos os recursos recém-criados do Azure que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Observação**: não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam muito tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos. Portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

    >**Observação**: o armazenamento do Cloud Shell deve ser criado para que esses comandos funcionem. 

1. Liste todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Observação**: o comando é executado de maneira assíncrona (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente após na mesma sessão do PowerShell, levará alguns minutos antes dos grupos de recursos serem de fato removidos.

## Revisão

Neste laboratório, você vai:

- Implantamos uma imagem do Docker usando a Instância do Contêiner do Azure
- Examinamos a funcionalidade da Instância do Contêiner do Azure
