---
lab:
  title: 'Laboratório: gerenciar assinaturas e RBAC'
  module: Administer Governance and Compliance
---

# Laboratório: gerenciar assinaturas e RBAC
# Manual do aluno

## Requisitos do laboratório

Este laboratório requer permissões para criar usuários, criar funções RBAC (Controle de Acesso Baseado em Função) personalizadas do Azure e atribuir essas funções aos usuários. Nem todos os hosters de laboratório podem fornecer esse recurso. Pergunte ao seu instrutor sobre a disponibilidade deste laboratório.

## Cenário do laboratório

Para melhorar o gerenciamento de recursos do Azure na Contoso, você foi encarregado de implementar a seguinte funcionalidade:

- criar um grupo de gerenciamento que incluiria todas as assinaturas do Azure da Contoso

- Conceder permissões para enviar solicitações de suporte para todas as assinaturas no grupo de gerenciamento a um usuário designado. As permissões desse usuário devem ser limitadas apenas a: 

    - Criando tickets de solicitação de suporte
    - exibindo grupos de recursos

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%202)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos.

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: implementar grupos de gerenciamento.
+ Tarefa 2: Criar funções RBAC personalizadas 
+ Tarefa 3: atribuir funções do RBAC.


## Tempo estimado: 30 minutos

## Diagrama de arquitetura

![imagem](../media/lab02aentra.png)


### Instruções

## Exercício 1

## Tarefa 1: implementar grupos de gerenciamento.

Nesta tarefa, você criará e configurará grupos de gerenciamento. 

1. Entre no [**portal do Azure**](http://portal.azure.com).

1. Pesquise e selecione **Grupos de gerenciamento para navegar até a **folha Grupos** de gerenciamento**.

1. Revise as mensagens na parte superior da **folha Grupos** de gerenciamento. Se você estiver vendo a mensagem informando **que você está registrado como administrador de diretório, mas não tem as permissões necessárias para acessar o grupo** de gerenciamento raiz, execute a seguinte sequência de etapas:

    1. No portal do Azure, pesquise pela **ID do Microsoft Entra** e selecione-a.
    
    1.  No painel que exibe as propriedades do locatário do Azure AD, no menu vertical, nas seções **Gerenciar**, selecione **Usuários**.
    
    1.  **Na folha Propriedades** do seu locatário, na **seção Gerenciamento de acesso para recursos** do Azure, selecione **Sim** e Salvar****.
    
    1.  Navegue de volta para a **folha Grupos** de gerenciamento e selecione **Atualizar**.

1. **Na folha Grupos de gerenciamento**, clique em **+ Criar**.

    >**Observação**: se você não tiver criado Grupos de Gerenciamento anteriormente, selecione **Começar a usar grupos de gerenciamento**

1. Crie um novo grupo de dispositivos com as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | ID do grupo de gerenciamento | az104-02-mg1 |
    | Nome de exibição do grupo de gerenciamento | az104-02-mg1 |

1. Na lista de grupos de gerenciamento, clique na entrada que representa o grupo de gerenciamento recém-criado.

1. **Na folha az104-02-mg1**, clique em **Assinaturas**. 

1. **Na folha Assinaturas** az104-02-mg1\|, clique em + Adicionar, na folha Adicionar** assinatura, na **lista suspensa Assinatura**, selecione a assinatura** que você está usando neste laboratório e clique em ****** Salvar.**

    >**Observação**: na folha Assinaturas** az104-02-mg1\|, copie a ID da sua assinatura do Azure para a **Área de Transferência. Isso será necessário na próxima etapa.

## Tarefa 2: Criar funções RBAC personalizadas

Nesta tarefa, você criará uma definição de uma função RBAC personalizada.

1. No computador do laboratório, abra o arquivo **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** no bloco de notas e revise seu conteúdo:

   ```json
   {
      "Name": "Support Request Contributor (Custom)",
      "IsCustom": true,
      "Description": "Allows to create support requests",
      "Actions": [
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
          "/providers/Microsoft.Management/managementGroups/az104-02-mg1",
          "/subscriptions/SUBSCRIPTION_ID"
      ]
   }
   ```
    > **Nota**: Se você não tiver certeza de onde os arquivos estão armazenados localmente em seu ambiente de laboratório, pergunte ao seu instrutor.

1. Substitua o `SUBSCRIPTION_ID` espaço reservado no arquivo JSON pela ID de assinatura copiada para a Área de Transferência e salve a alteração.

1. No portal do Azure, abra o painel **Cloud Shell** selecionando o ícone da barra de ferramentas ao lado da caixa de texto de pesquisa.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**. 

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento. 

1. Na barra de ferramentas do painel Cloud Shell, clique no **ícone Carregar/Baixar arquivos** , no menu suspenso clique em **Carregar** e carregue o arquivo **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** no diretório inicial do Cloud Shell.

1. Execute o seguinte comando no Cloud Shell para criar uma função personalizada:

   ```powershell
   New-AzRoleDefinition -InputFile $HOME/az104-02a-customRoleDefinition.json
   ```

1. Feche o painel do Cloud Shell.

## Tarefa 3: atribuir funções do RBAC.

Nesta tarefa, você criará um usuário, atribuirá a função RBAC criada na tarefa anterior a esse usuário e verificará se o usuário pode executar a tarefa especificada na definição de função RBAC.

1. No portal do Azure, procure e selecione **Microsoft Entra ID**, clique em Usuários** e, em seguida, clique em ****+ Novo usuário**.

1. Crie um novo usuário com as seguintes configurações (deixe outros com seus padrões):

    | Configuração | Valor |
    | --- | --- |
    | Nome de usuário | az104-02-aaduser1|
    | Nome | az104-02-aaduser1|
    | Permitir que eu crie a senha | Habilitado |
    | Senha inicial | **Forneça uma senha segura** |

    >**Nota**: **Copie para a área de transferência** o nome** de usuário completo**. Você precisará disso em uma etapa posterior.

1. No portal do Azure, navegue de volta para o grupo de gerenciamento az104-02-mg1** e exiba **seus **detalhes**.

1. Clique em **Controle de Acesso (IAM)** e, em seguida, clique em **Adicionar** e selecione **Adicionar atribuição de função**. **Na guia Função**, procure **Colaborador de Solicitação de Suporte (Personalizado).** 

    >**Observação**: se sua função personalizada não estiver visível, pode levar até 10 minutos para que a função personalizada apareça após a criação.

1. Para Função, selecione  e clique em Avançar. Na guia Membros **, clique em **+ Selecionar membros** e **selecione** a **conta de usuário az104-******.****.onmicrosoft.com. Clique em **Selecionar** e em **Examinar+atribuir**.

1. Abra uma nova janela do navegador e faça logon no portal do Azure usando a conta ad-admin recém-criada. Quando solicitado a atualizar a senha, altere a senha do usuário.

    >**Nota**: Em vez de digitar o nome de usuário, você pode colar o conteúdo da Área de Transferência.

1. Na janela do **navegador InPrivate** , no portal do Azure, pesquise e selecione **Grupos** de recursos para verificar se o usuário az104-02-aaduser1 pode ver todos os grupos de recursos.

1. Na janela do **navegador InPrivate** , no portal do Azure, pesquise e selecione **Todos os recursos** para verificar se o usuário az104-02-aaduser1 não pode ver nenhum recurso.

1. Na janela do **navegador InPrivate**, no portal do Azure, pesquise e selecione **Ajuda + suporte e clique em **+ Criar uma solicitação de** suporte**. 

1. Na janela do navegador InPrivate, na **guia Descrição do **Problema/Resumo da folha Ajuda + suporte - Nova solicitação de suporte, digite **Limites de serviço e assinatura no campo Resumo** e selecione o **tipo de** problema Limites** de **serviço e assinatura (cotas).**** Observe que a assinatura que você está usando neste laboratório está listada **na lista suspensa Assinatura** .

    >**Observação**: a **presença da assinatura que você está usando neste laboratório na lista suspensa Assinatura** indica que a conta que você está usando tem as permissões necessárias para criar a solicitação de suporte específica da assinatura.

    >**Observação**: se você não vir a **opção Limites de serviço e assinatura (cotas),** saia do portal do Azure e entre novamente.

1. Não continue com a criação da solicitação de suporte. Em vez disso, saia como o usuário az104-02-aaduser1 no portal do Azure e feche a janela do navegador InPrivate.

## Tarefa 4: limpar recursos

   >Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. A remoção de recursos não utilizados garante que você não verá cobranças inesperadas, embora os recursos criados neste laboratório não incorram em custo extra.

   >**Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo.

1. No portal do Azure, pesquise pela ID do Microsoft Entra e selecione-a.

1. Na folha Usuários - Todos os **usuários** , clique em **az104-02-aaduser1**.

1. **Na folha az104-02-aaduser1 - Perfil**, copie o valor do atributo ID** do **objeto.

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. No painel Cloud Shell, execute o seguinte para remover a atribuição da definição de função personalizada (substitua o `[object_ID]` espaço reservado pelo valor do atributo ID** do **objeto da conta de **usuário az104-02-aaduser1** que você copiou anteriormente nesta tarefa):

   ```powershell
   
    $scope = (Get-AzRoleDefinition -Name 'Support Request Contributor (Custom)').AssignableScopes | Where-Object {$_ -like '*managementgroup*'}
    
    Remove-AzRoleAssignment -ObjectId '[object_ID]' -RoleDefinitionName 'Support Request Contributor (Custom)' -Scope $scope
   ```

1. No painel Cloud Shell, execute o seguinte para remover a definição de função personalizada:

   ```powershell
   Remove-AzRoleDefinition -Name 'Support Request Contributor (Custom)' -Force
   ```

1. No portal do Azure, navegue de volta para a folha Usuários - Todos os usuários da ID do Microsoft Entra e exclua a ****conta de **usuário az104-02-aaduser1**.****

1. No portal do Azure, navegue até a folha **Gerenciamento de Custos e Cobrança**. 

1. **Na folha Grupos** de gerenciamento, selecione o ícone de reticências ao lado de sua assinatura no **grupo de gerenciamento az104-02-mg1** e selecione **Mover** para mover a assinatura para o **grupo** de** gerenciamento Raiz do **locatário.

   >**Nota**: É provável que o grupo de gerenciamento de destino seja o grupo de gerenciamento Raiz do **Locatário, a menos que você tenha criado uma hierarquia de grupo** de gerenciamento personalizada antes de executar este laboratório.
   
1. Selecione **Atualizar** para verificar se a assinatura foi movida com êxito para o grupo** de gerenciamento Raiz do **Locatário.

1. Navegue de volta para a **folha Grupos** de gerenciamento, clique no **ícone de reticências** à direita do grupo de **gerenciamento az104-02-mg1** e clique em **Excluir**.
  >**Observação**: se você não conseguir excluir o grupo de gerenciamento Raiz do Locatário, é provável que a Assinatura** do **Azure esteja sob o **grupo** de gerenciamento. Você precisa mover **a Assinatura** do Azure para fora do grupo de gerenciamento Raiz do **Locatário e excluir o grupo** .

## Revisão

Neste laboratório, você vai:

- Tarefa 1: implementar grupos de gerenciamento.
- Crie uma função RBAC personalizada. 
- Atribuir funções do RBAC
