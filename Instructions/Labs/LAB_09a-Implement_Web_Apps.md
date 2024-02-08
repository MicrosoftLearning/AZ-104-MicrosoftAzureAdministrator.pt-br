---
lab:
  title: 'Laboratório 09a: Implementar Aplicativos Web'
  module: Administer PaaS Compute Options
---

# Laboratório 09a - Implementar Aplicativos Web
# Manual do laboratório do aluno

## Cenário do laboratório

Você precisa avaliar o uso dos aplicativos Web do Azure para hospedar os sites da Contoso, hospedados atualmente nos data centers locais da empresa. Os sites estão em execução em servidores Windows usando a pilha de runtime do PHP. Você também precisa determinar como implementar práticas de DevOps aproveitando os slots de implantação de aplicativos Web do Azure.

**Observação:** Uma **[simulação interativa de laboratório](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013)** está disponível para você clicar neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: criar um aplicativo Web do Azure
+ Tarefa 2: criar um slot de implantação de preparo
+ Tarefa 3: definir configurações de implantação do aplicativo Web
+ Tarefa 4: implantar código no slot de implantação de preparo
+ Tarefa 5: trocar os slots de preparo
+ Tarefa 6: configurar e testar o dimensionamento automático do aplicativo Web do Azure

## Tempo estimado: 30 minutos

## Diagrama de arquitetura

![imagem](../media/lab09a.png)

### Instruções

## Exercício 1

## Tarefa 1: criar um aplicativo Web do Azure

Nesta tarefa, você criará um aplicativo Web do Azure.

1. Entre no [**portal do Azure**](http://portal.azure.com).

1. No portal do Azure, pesquise e selecione **Serviços de aplicativo** e, no painel **Serviços de Aplicativo**, clique em **+ Criar**.

1. Na guia **Básico** da folha **Criar Aplicativo Web**, defina as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | ---|
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | o nome de um novo grupo de recursos **az104-09a-rg1** |
    | Nome do aplicativo Web | qualquer nome globalmente exclusivo |
    | Publicação | **Código** |
    | Pilha de runtime | **PHP 8.2** |
    | Sistema operacional | **Linux** |
    | Region | o nome de uma região do Azure onde você pode provisionar aplicativos web do Azure |
    | Planos de preços | aceite a configuração padrão |

1. Clique em **Revisar + Criar**. Na guia **Revisar + criar** da folha **Criar Aplicativo Web**, verifique se a validação foi aprovada e clique em **Criar**.

    >**Observação**: aguarde até que o aplicativo web seja criado para prosseguir para a próxima tarefa. Isso deverá levar cerca de um minuto.

1. Na folha de implantação, clique em **Ir para o recurso**.

## Tarefa 2: criar um slot de implantação de preparo

Nesta tarefa, você criará um slot de implantação de preparo.

1. No painel do aplicativo web recém-implantado, clique no link **Domínio padrão** para exibir a página da web padrão em uma nova guia do navegador.

1. Feche a nova guia do navegador e, no portal do Azure, na seção **Implantação** da folha do aplicativo Web, clique em **Slots de implantação**.

    >**Observação**: o aplicativo Web, neste momento, tem um único slot de implantação com o rótulo **PRODUÇÃO**.

1. Clique em **+ Adicionar slot** e adicione um novo slot com as seguintes configurações:

    | Configuração | Valor |
    | --- | ---|
    | Nome | **staging** |
    | Clone as configurações de | **Não clonar as configurações**|

1. De volta à folha **Slots de implantação** do aplicativo Web, clique na entrada que representa o slot de preparo recém-criado.

    >**Observação**: isso abrirá o painel exibindo as propriedades do slot de preparo.

1. Examine a folha do slot de preparo e observe que sua URL é diferente daquela atribuída ao slot de produção.

## Tarefa 3: definir configurações de implantação do aplicativo Web

Nesta tarefa, você definirá as configurações de implantação do aplicativo Web.

1. Na folha do slot de implantação de preparo, na seção **Implantação**, clique no **Centro de Implantação** e selecione a guia **Configurações**.

    >**Observação:** confirme se você está na folha do slot de preparo (em vez do slot de produção).
    
1. Na guia **Configurações**, na lista suspensa **Origem**, selecione **Git Local** e clique no botão **Salvar**

1. No painel **Centro de implantação**, copie a entrada **Uri do Git Clone** para o Bloco de notas.

    >**Observação:** você precisará do valor do URI do Git Clone na próxima tarefa deste laboratório.

1. Na folha **Centro de Implantação**, selecione a guia **Credenciais locais do Git/FTPS**, na seção **Escopo do Usuário**, especifique as configurações a seguir e clique em **Salvar**.

    | Configuração | Valor |
    | --- | ---|
    | Nome de usuário | qualquer nome globalmente exclusivo (veja a observação)  |
    | Senha | qualquer senha que atenda aos requisitos de complexidade (veja a observação) |

    >**Observação:** copie essas credenciais para o Bloco de notas. Você precisará delas mais tarde.
    
    >**Observação:** essas credenciais serão passadas pelo URI. Não inclua caracteres especiais que afetem a interpretação do URI. Por exemplo, @, $, ou #. Um sinal de asterisco ou de mais (no meio da cadeia de caracteres) daria certo.
    
## Tarefa 4: implantar código no slot de implantação de preparo

Nesta tarefa, você implantará o código no slot de implantação de preparo.

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >**Observação**: se esta for a primeira vez que você está iniciando o **Cloud Shell** e você receber a mensagem **Você não tem nenhum armazenamento montado**, selecione a assinatura que você está usando no laboratório e selecione **Criar armazenamento**.

1. No painel do Cloud Shell, execute o seguinte para clonar o repositório remoto que contém o código do aplicativo Web.

   ```powershell
   git clone https://github.com/Azure-Samples/php-docs-hello-world
   ```

1. No painel do Cloud Shell, execute o comando a seguir para definir o local atual para o clone recém-criado do repositório local que contém o código do aplicativo Web de exemplo.

   ```powershell
   Set-Location -Path $HOME/php-docs-hello-world/
   ```

1. No painel do Cloud Shell, execute o seguinte para adicionar o git remoto (substitua os espaços reservados `[deployment_user_name]` e `[git_clone_uri]` pelo valor do nome de usuário das **Credenciais de Implantação** e do **Uri do Git Clone**, respectivamente, que você identificou na tarefa anterior):

   ```powershell
   git remote add [deployment_user_name] [git_clone_uri]
   ```

    >**Observação**: o valor após `git remote add` não precisa corresponder ao nome de usuário das **Credenciais de Implantação**, mas precisa ser exclusivo

1. No painel do Cloud Shell, execute o seguinte para efetuar push do código do aplicativo Web de exemplo do repositório local para o slot de implantação de preparo do aplicativo Web do Azure (substitua o espaço reservado pelo valor do nome de usuário das **Credenciais de Implantação**, além da senha e do nome do aplicativo que você identificou na tarefa anterior):

   ```powershell
    git push https://<deployment-username>:<deployment-password>@<app-name>-staging.scm.azurewebsites.net/<app-name>.git master
   ```

1. Feche o painel do Cloud Shell.

1. No painel do slot de preparo, clique em **Visão geral** e clique no link da **Domínio padrão** para exibir a página da web padrão em uma nova guia do navegador.

1. Verifique se a página do navegador exibe a mensagem **Olá, Mundo!** e feche a nova guia.

## Tarefa 5: trocar os slots de preparo

Nesta tarefa, você trocará o slot de preparo pelo slot de produção

1. Navegue de volta para a folha que está exibindo o slot de produção do aplicativo Web.

1. Na seção **Implantação**, clique em **Slots de implantação** e, em seguida, clique no ícone **Trocar** da barra de ferramentas.

1. Na folha **Trocar**, examine as configurações padrão e clique em **Trocar**.

1. Clique em **Visão geral** no painel do slot de produção do aplicativo Web e clique no link **Domínio padrão** para exibir a home page do site em uma nova guia do navegador.

1. Verifique se a página da web padrão foi substituída pela página **Hello World!** .

## Tarefa 6: configurar e testar o dimensionamento automático do aplicativo Web do Azure

Nesta tarefa, você configurará e testará o dimensionamento automático do aplicativo Web do Azure.

1. No painel que exibe o slot de produção do aplicativo Web, na seção **Configurações**, clique em **Dimensionamento horizontal (plano do Serviço de Aplicativo)**.

1. Na **Seção de dimensionamento**, selecione a opção **Baseado em regras** e clique no link **Gerenciar dimensionamento baseado em regras**.

1. Clique em **Dimensionamento automático personalizado**.

    >**Observação**: você também tem a opção de dimensionar o aplicativo Web manualmente.

1. Selecione **Dimensionamento com base em uma métrica** e clique em **+ Adicionar uma regra**

1. No painel **Regra de dimensionamento**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- |--- |
    | Origem da métrica | **Recurso atual** |
    | Namespace da métrica | **métricas padrão** |
    | Nome da métrica | **Porcentagem de CPU** |
    | Operador | **Maior que** |
    | Limite de métrica para disparar ação de escala | **10** |
    | Duração (em minutos) | **1** |
    | Estatística de intervalo de agregação | **Máximo** |
    | Agregação de tempo | **Máximo** |
    | Operação | **Aumentar a contagem em** |
    | Contagem de instâncias | **1** |
    | Tempo de resfriamento (minutos) | **5** |

    >**Observação**: esses valores não representam uma configuração realista, uma vez que o objetivo é acionar o dimensionamento automático o mais rápido possível, sem período de espera prolongado.

1. Clique em **Adicionar** e, novamente no painel de dimensionamento do plano do Serviço de Aplicativo, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- |--- |
    | Limites mínimos de instância | **1** |
    | Limites máximos de instância | **2** |
    | Limites padrão de instância | **1** |

1. Clique em **Save** (Salvar).

    >**Observação**: se você recebeu um erro informando que o provedor de recursos "microsoft.insights" não está registrado, execute `az provider register --namespace 'Microsoft.Insights'` no cloudshell e tente salvar novamente suas regras de dimensionamento automático.

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

1. No painel do Cloud Shell, execute o seguinte para identificar a URL do aplicativo Web do Azure.

   ```powershell
   $rgName = 'az104-09a-rg1'

   $webapp = Get-AzWebApp -ResourceGroupName $rgName
   ```

1. No painel do Cloud Shell, execute o seguinte para iniciar o loop infinito que envia as solicitações HTTP para o aplicativo Web:

   ```powershell
   while ($true) { Invoke-WebRequest -Uri $webapp.DefaultHostName }
   ```

1. Minimize o painel do Cloud Shell (mas não o feche) e, no painel do aplicativo Web, na seção Configurações, clique em **Dimensionamento horizontal (plano do Serviço de Aplicativo)**.

1. Monitore a utilização e o número de instâncias por alguns minutos. 

    >**Observação**: talvez seja necessário **Atualizar** a página.

1. Depois de perceber que o número de instâncias aumentou para 2, reabra o painel do Cloud Shell e encerre o script pressionando **Ctrl+C**.

1. Feche o painel do Cloud Shell.

## Limpar os recursos

>**Observação**: lembre-se de remover todos os recursos recém-criados do Azure que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Observação**: não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam muito tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos. Portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Liste todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Observação**: o comando é executado de maneira assíncrona (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente após na mesma sessão do PowerShell, levará alguns minutos antes dos grupos de recursos serem de fato removidos.

## Revisão

Neste laboratório, você vai:

+ Criou um aplicativo Web do Azure
+ Criou um slot de implantação de preparo
+ Definiu as configurações de implantação do aplicativo Web
+ Implantou um código no slot de implantação de preparo
+ Trocou os slots de preparo
+ Configurou e testou o dimensionamento automático do aplicativo Web do Azure
