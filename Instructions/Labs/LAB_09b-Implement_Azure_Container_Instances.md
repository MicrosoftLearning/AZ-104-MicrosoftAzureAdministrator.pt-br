---
lab:
  title: 'Laboratório 09b: implementar Instâncias de Contêiner do Azure'
  module: Administer PaaS Compute Options
---

# Laboratório 09b – Implementar Instâncias de Contêiner do Azure

## Introdução ao laboratório

Neste laboratório, você aprenderá a implementar e implantar as Instâncias de Contêiner do Azure.

Este laboratório requer uma assinatura do Azure. Seu tipo de assinatura pode afetar a disponibilidade de recursos neste laboratório. Você pode alterar a região, mas as etapas são escritas usando o **Leste dos EUA**.

## Tempo estimado: 15 minutos

## Cenário do laboratório

Sua organização tem um aplicativo Web que é executado em uma máquina virtual em seu data center local. A organização deseja mover todos os aplicativos para a nuvem, mas não quer ter um grande número de servidores para gerenciar. Você decide avaliar as Instâncias de Contêiner do Azure e o Docker. 
## Simulações interativas do laboratório

Há simulações interativas do laboratório que podem ser úteis para este tópico. A simulação permite que você clique em um cenário semelhante em seu próprio ritmo. Há diferenças entre a simulação interativa e este laboratório, mas muitos dos principais conceitos são os mesmos. Não é necessária uma assinatura do Azure.

+ [Implante as Instâncias de Contêiner do Azure](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%203). Crie, configure e implante um contêiner do Docker com as Instâncias de Contêiner do Azure.
  
+ [Implemente as Instâncias de Contêiner do Azure](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014).  Implante uma imagem do Docker usando as Instâncias de Contêiner do Azure. 

## Diagrama de arquitetura

![Diagrama das tarefas.](../media/az104-lab09b-aci-architecture.png)

## Habilidades de trabalho

- Tarefa 1: Implante as Instâncias de Contêiner do Azure usando uma imagem do Docker.
- Tarefa 2: Teste e verifique a implantação de uma Instância de Contêiner do Azure.

## Tarefa 1: implantar Instâncias de Contêiner do Azure usando uma imagem do Docker.

Nesta tarefa, você criará um aplicativo Web simples usando uma imagem do Docker. O Docker é uma plataforma que oferece a capacidade de empacotar e executar aplicativos em ambientes isolados chamados contêineres. As Instâncias de Contêiner do Azure fornecem o ambiente de computação para a imagem de contêiner.

1. Entre no **portal do Azure** - `https://portal.azure.com`.

1. No portal do Azure, pesquise e selecione `Container instances` e, na folha **Instâncias de contêiner**, clique em **+ Criar**.

1. Na guia **Básico** do painel **Criar conta de armazenamento**, defina as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | ---- | ---- |
    | Subscription | Selecione sua Assinatura do Azure. |
    | Grupo de recursos | `az104-rg9` (Se necessário, selecione **Criar novo**) |
    | Nome do contêiner | `az104-c1` |
    | Region | **Leste dos EUA** (ou uma região disponível perto de você)|
    | Origem da imagem | **Imagens de início rápido** |
    | Imagem | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Clique em **Avançar: Rede >** e especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Rótulo do nome DNS | qualquer nome do host DNS global exclusivo e válido |

    >**Observação**: seu contêiner será acessível publicamente em dns-name-label.region.azurecontainer.io. Se você receber uma mensagem de erro **Rótulo de nome do DNS não disponível**, tente usar um rótulo de nome do DNS diferente.

1. Clique em **Avançar: Avançado >**, examine as configurações sem fazer nenhuma alteração.

 1. Clique em **Examinar + Criar**, verifique se a validação foi aprovada e selecione **Criar**.

    >**Observação**: aguarde até que a implantação seja concluída. Isso deve levar de 2 a 3 minutos.

    >**Observação**: enquanto espera, pode ser interessante visualizar o [código por trás do aplicativo de exemplo](https://github.com/Azure-Samples/aci-helloworld). Para exibir o código, navegue pela pasta do aplicativo \\.

## Tarefa 2: Testar e verificar a implantação de uma Instância de Contêiner do Azure 

Nesta tarefa, você examinará a implantação da instância de contêiner. Por padrão, a Instância de Contêiner do Azure é acessível pela porta 80. Depois que a instância tiver sido implantada, você poderá navegar até o contêiner usando o nome DNS fornecido na tarefa anterior.

1. Na folha de implantação, clique no link **Ir para o recurso**.

1. Na folha **Visão geral** da instância do contêiner, verifique se o **Status** é relatado como **Em execução**.

1. Copie o valor da instância do contêiner **FQDN**, abra uma nova guia do navegador e navegue até a URL correspondente.

     ![Captura de tela da página de visão geral das ACI no portal.](../media/az104-lab09b-aci-overview.png)

1. Verifique se a página **Bem-vindo(a) à Instância do Contêiner do Azure** é exibida. Atualize a página várias vezes para criar algumas entradas de log e feche a guia do navegador.  

1. Na seção **Configurações** da folha da instância de contêiner, clique em **Contêineres** e clique em **Logs**.

1. Verifique se você vê as entradas de log que representam a solicitação HTTP GET gerada exibindo o aplicativo no navegador.
   
## Limpar os recursos

Se você estiver trabalhando com **sua própria assinatura**, reserve um minuto para excluir os recursos do laboratório. Isso garantirá que os recursos sejam liberados e que o custo seja minimizado. A maneira mais fácil de excluir os recursos do laboratório é excluir o grupo de recursos do laboratório. 

+ No portal do Azure, selecione o grupo de recursos, selecione **Excluir o grupo de recursos**, **Inserir o nome do grupo de recursos** e clique em **Excluir**.
+ Usar o Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usar a CLI, `az group delete --name resourceGroupName`.


## Principais aspectos a serem lembrados

Parabéns por concluir o laboratório. Aqui estão as principais lições deste laboratório. 

+ As ACI (Instâncias de Contêiner do Azure) são um serviço que permite implantar contêineres na nuvem pública do Microsoft Azure.
+ As ACI não exigem que você provisione ou gerencie qualquer infraestrutura subjacente.
+ As ACI dão suporte a contêineres do Linux e do Windows.
+ As cargas de trabalho nas ACI geralmente são iniciadas e interrompidas por algum tipo de processo ou gatilho e costumam ter curta duração. 

## Saiba mais com treinamento individual

+ [Executar imagens de contêiner em Instâncias de Contêiner do Azure](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/). Saiba como as Instâncias de Contêiner do Azure podem ajudar você a implantar rapidamente contêineres, definir variáveis de ambiente e especificar políticas de reinicialização de contêiner.

    
