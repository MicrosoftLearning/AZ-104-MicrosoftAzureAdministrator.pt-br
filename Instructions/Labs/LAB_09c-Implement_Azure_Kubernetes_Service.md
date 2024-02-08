---
lab:
  title: 'Laboratório: implementar o Serviço de Kubernetes do Azure'
  module: Administer PaaS Compute Options
---

# Laboratório: implementar o Serviço de Kubernetes do Azure
# Manual do aluno

## Cenário do laboratório

A Contoso tem vários aplicativos de várias camadas que não são adequados para serem executados usando Instâncias de Contêiner do Azure. Para determinar se eles podem ser executados como cargas de trabalho em contêineres, você deseja avaliar usando o Kubernetes como o orquestrador de contêineres. Para minimizar ainda mais a sobrecarga de gerenciamento, você deseja testar o Serviço de Kubernetes do Azure, incluindo sua experiência de implantação simplificada e recursos de dimensionamento.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2015)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: registrar o Microsoft Kubernetes e os provedores de recursos de Configuração do Microsoft Kubernetes.
+ Tarefa 2: implantar um cluster do Serviço de Kubernetes do Azure.
+ Tarefa 3: implantar pods no cluster do Serviço de Kubernetes do Azure.
+ Tarefa 4: dimensionar cargas de trabalho em contêineres no cluster do serviço do Kubernetes do Azure.

## Tempo estimado: 40 minutos

## Diagrama de arquitetura

![imagem](../media/lab09c.png)

### Instruções

## Exercício 1

## Tarefa 1: registrar o Microsoft Kubernetes e os provedores de recursos de Configuração do Microsoft Kubernetes.

Nesta tarefa, você registrará os provedores de recursos necessários para implantar um cluster dos Serviços de Kubernetes do Azure.

1. Entre no [portal do Azure](https://portal.azure.com).

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento.

1. No painel Cloud Shell, execute o seguinte para registrar os provedores de recursos Microsoft.Kubernetes e Microsoft.KubernetesConfiguration.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Kubernetes

   Register-AzResourceProvider -ProviderNamespace Microsoft.KubernetesConfiguration
   ```

1. Feche o painel do Cloud Shell.

## Tarefa 2: implantar um cluster do Serviço de Kubernetes do Azure.

Neste início rápido, você implanta um cluster do AKS (Serviço de Kubernetes do Azure) usando a CLI do Azure.

1. No portal do Azure, procure localizar **serviços** do Kubernetes e, em seguida, na folha Serviços** do Kubernetes, clique em + Criar e, em seguida, clique **em **+ Criar** um cluster** do **Kubernetes.

1. Na guia **Noções básicas** da folha **Criar conta de armazenamento**, defina as seguintes configurações (deixe outras com seus valores padrão):

    | Configuração | Valor |
    | ---- | ---- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | O nome de um novo grupo de recursos **aks-01-RG** |
    | Configuração predefinida do cluster | Desenvolvimento/Teste ($) |
    | Nome do cluster do Kubernetes | az104-09c-aks1 |
    | Region | o nome de uma região onde você pode provisionar um cluster do Kubernetes |
    | Zonas de disponibilidade | **Nenhum** (desmarque todas as caixas) |
    | Versão do Kubernetes | Aceite o padrão |
    | Disponibilidade do servidor de API | Aceite o padrão |
    | Tamanho do nó | Aceite o padrão |
    | Método de dimensionamento | **Manual** |
    | Contagem de nós | **1** |

1. Clique em **Avançar: >** Pools de Nós e, na **guia Pools de Nós** da folha Criar cluster** do **Kubernetes, especifique as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | ---- | ---- |
    | Habilitar nós virtuais | **Desabilitado** (padrão). |

1. Clique em **Avançar: >** de acesso e, na **guia Acesso** da **folha Criar cluster** do Kubernetes, deixe as configurações com seus valores padrão:

    | Configuração | Valor |
    | ---- | ---- |
    | identity-resource-ID | **Identidade gerenciada atribuída pelo sistema** |
    | Método de autenticação | **Contas locais com o Kubernetes RBAC** |

1. Clique em **Avançar: >** de rede e, na **guia Rede** da folha Criar cluster** do **Kubernetes, especifique as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | ---- | ---- |
    | Configuração de rede | **kubenet** |
    | Prefixo do nome DNS | **qualquer prefixo DNS válido e globalmente exclusivo** |

1. Clique em **Avançar: Integrações >**, na **guia Integrações** da folha Criar cluster** do **Kubernetes, especifique as seguintes configurações (deixe outras pessoas com seus valores padrão):

    | Configuração | Valor |
    | ---- | ---- |
    | Monitoramento de contêineres | **Desabilitar** |
    | Para habilitar as regras de alerta recomendadas: | **Desmarcar** |
    
1.  Clique em **Examinar + criar** e, quando a verificação de validação for aprovada, clique em **Criar**.

    >**Nota**: Em cenários de produção, convém habilitar o monitoramento. O monitoramento é desativado neste caso, uma vez que não é coberto no laboratório.

    >Aguarde até que a implantação seja concluída. Isso deverá levar cerca de cinco minutos.

## Tarefa 3: implantar pods no cluster do Serviço de Kubernetes do Azure.

Nesta tarefa, você implantará um pod no cluster do Serviço Kubernetes do Azure.

1. Na folha de implantação, clique no link Ir para** recurso**.

1. Na folha do serviço Kubernetes az104-9c-aks1****, na **seção Configurações**, clique em **Pools** de nós.

1. **Na folha az104-9c-aks1 - Pools de nós**, verifique se o cluster consiste em um único pool com um nó.

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Mude o **Azure Cloud Shell** para **Bash** (fundo preto).

1. No painel Cloud Shell, execute o seguinte para recuperar as credenciais para acessar o cluster AKS:

    ```sh
    RESOURCE_GROUP='az104-09c-rg1'

    AKS_CLUSTER='az104-9c-aks1'

    az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER
    ```

1. No **painel Cloud Shell** , execute o seguinte para verificar a conectividade com o cluster AKS:

    ```sh
    kubectl get nodes
    ```

1. No painel Cloud **Shell** , revise a saída e verifique se o nó no qual o cluster consiste neste ponto está relatando o **status Pronto** .

1. No painel Cloud **Shell** , execute o seguinte para implantar a **imagem nginx** do Docker Hub:

    ```sh
    kubectl create deployment nginx-deployment --image=nginx
    ```

    > **Nota**: Certifique-se de usar letras minúsculas ao digitar o nome da implantação (nginx-deployment)

1. **No painel Cloud Shell**, execute o seguinte para verificar se um pod do Kubernetes foi criado:

    ```sh
    kubectl get pods
    ```

1. No **painel Cloud Shell** , execute o seguinte para identificar o estado da implantação:

    ```sh
    kubectl get deployment
    ```

1. No painel Cloud Shell **, execute o **seguinte para disponibilizar o pod na Internet:

    ```sh
    kubectl expose deployment nginx-deployment --port=80 --type=LoadBalancer
    ```

1. No painel Cloud Shell **, execute o **seguinte para identificar se um endereço IP público foi provisionado:

    ```sh
    kubectl get service
    ```

1. Execute novamente o comando até que o **valor na coluna EXTERNAL-IP para a **entrada nginx-deployment** mude de **\<pending\>** um endereço IP** público. Observe o **endereço IP público na coluna EXTERNAL-IP** para **nginx-deployment**.

1. Abra uma janela do navegador e navegue até o endereço IP obtido na etapa anterior. Verifique se o navegador da Web exibe a página **Sucesso!**. .

## Tarefa 4: dimensionar cargas de trabalho em contêineres no cluster do serviço do Kubernetes do Azure.

Nesta tarefa, você dimensionará horizontalmente o número de pods e, em seguida, o número de nós de cluster.

1. No **painel Cloud Shell** e execute o seguinte para dimensionar a implantação aumentando o número de pods para 2:

    ```sh
    kubectl scale --replicas=2 deployment/nginx-deployment
    ```

1. No **painel Cloud Shell** , execute o seguinte para verificar o resultado do dimensionamento da implantação:

    ```sh
    kubectl get pods
    ```

    > **Nota**: Revise a saída do comando e verifique se o número de pods aumentou para 2.

1. **No painel Cloud Shell**, execute o seguinte para expandir o cluster aumentando o número de nós para 2:

    ```sh
    RESOURCE_GROUP='az104-09c-rg1'

    AKS_CLUSTER='az104-9c-aks1'

    az aks scale --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER --node-count 2
    ```

    > **Nota**: Aguarde a conclusão do provisionamento do nó adicional. Isso pode levar cerca de 10 minutos. Se isso falhar, execute `az aks scale` e execute o comando novamente.

1. No painel Cloud Shell **, execute o **seguinte para verificar o resultado do dimensionamento do cluster:

    ```sh
    kubectl get nodes
    ```

    > **Nota**: Revise a saída do comando e verifique se o número de nós aumentou para 2.

1. No painel Cloud Shell **, execute o **seguinte para dimensionar a implantação:

    ```sh
    kubectl scale --replicas=10 deployment/nginx-deployment
    ```

1. No **painel Cloud Shell** , execute o seguinte para verificar o resultado do dimensionamento da implantação:

    ```sh
    kubectl get pods
    ```

    > **Nota**: Revise a saída do comando e verifique se o número de pods aumentou para 10.

1. No **painel Cloud Shell** , execute o seguinte para revisar a distribuição de pods entre nós de cluster:

    ```sh
    kubectl get pod -o=custom-columns=NODE:.spec.nodeName,POD:.metadata.name
    ```

    > **Nota**: Revise a saída do comando e verifique se os pods estão distribuídos em ambos os nós.

1. **No painel Cloud Shell**, execute o seguinte para excluir a implantação:

    ```sh
    kubectl delete deployment nginx-deployment
    ```

1. Feche o painel do **Cloud Shell**.

## Limpar os recursos

>Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam muito tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```sh
   az group list --query "[?starts_with(name,'az104-09c')].name" --output tsv
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```sh
   az group list --query "[?starts_with(name,'az104-09c')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >O comando é executado de modo assíncrono (conforme determinado pelo parâmetro --nowait), portanto, embora você possa executar outro comando da CLI do Azure imediatamente depois na mesma sessão Bash, levará alguns minutos antes de o grupo de recursos ser removido.

## Revisão

Neste laboratório, você vai:

+ Implantar um cluster do Serviço de Kubernetes do Azure
+ Tarefa 3: implantar pods no cluster do Serviço de Kubernetes do Azure.
+ Tarefa 4: dimensionar cargas de trabalho em contêineres no cluster do serviço do Kubernetes do Azure.
