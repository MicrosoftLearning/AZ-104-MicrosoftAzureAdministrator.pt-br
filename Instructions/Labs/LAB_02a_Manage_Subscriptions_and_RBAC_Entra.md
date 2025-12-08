---
lab:
  title: 'Laboratório 02a: Gerenciar Assinaturas e RBAC'
  module: Administer Governance and Compliance
---

# Laboratório 02a - Gerenciar Assinaturas e RBAC

## Introdução ao laboratório

Neste laboratório, você aprenderá sobre o controle de acesso baseado em função. Você aprenderá a usar permissões e escopos para controlar quais ações as identidades podem ou não executar. Você também aprenderá a facilitar o gerenciamento de assinaturas usando grupos de gerenciamento. 

Este laboratório requer uma assinatura do Azure. Seu tipo de assinatura pode afetar a disponibilidade de recursos neste laboratório. Você pode alterar a região, mas as etapas são escritas usando o **Leste dos EUA**. 

## Tempo estimado: 20 minutos

## Cenário do laboratório

Para simplificar o gerenciamento de recursos do Azure em sua organização, você foi encarregado de implementar a seguinte funcionalidade:

- Criar um grupo de gerenciamento que inclui todas as suas assinaturas do Azure.

- Conceder permissões para enviar solicitações de suporte para todas as assinaturas no grupo de gerenciamento. As permissões devem ser limitadas apenas a: 

    - Criar e gerenciar máquinas virtuais
    - Criar tíquetes de solicitação de suporte (não inclua a adição de provedores do Azure)

## Diagrama de arquitetura

![Diagrama de tarefas do laboratório.](../media/az104-lab02a-architecture.png)

## Habilidades de trabalho

+ Tarefa 1: Implementar grupos de gerenciamento.
+ Tarefa 2: revisar e atribuir uma função integrada do Azure.
+ Tarefa 3: criar uma função RBAC personalizada.
+ Tarefa 4: Monitorar as atribuições de função com o Log de Atividades.

## Tarefa 1: Implementar Grupos de Gerenciamento

Nesta tarefa, você criará e configurará grupos de gerenciamento. Os grupos de gerenciamento são usados ​​para organizar e segmentar assinaturas de forma lógica. Permitem que o RBAC e o Azure Policy sejam atribuídos e herdados a outros grupos de gestão e subscrições. Por exemplo, se sua organização tiver uma equipe de suporte dedicada para a Europa, você poderá organizar assinaturas europeias em um grupo de gerenciamento para fornecer à equipe de suporte acesso a essas assinaturas (sem fornecer acesso individual a todas as assinaturas). Em nosso cenário, todos no suporte técnico precisarão criar uma solicitação de suporte em todas as assinaturas. 

1. Entre no **portal do Azure** - `https://portal.azure.com`.

1. Pesquise e selecione `Microsoft Entra ID`.

1. Na folha **Gerenciar**, selecione **Propriedades**.

1. Examine a área **Gerenciamento de acesso de recursos do Azure**. Observe que você pode gerenciar o acesso a todas as assinaturas do Azure e aos grupos de gerenciamento no locatário.
   
1. Pesquise e selecione **Grupos de gerenciamento**.

1. Na folha **Grupos de gerenciamento**, clique em **+ Criar**.

1. Crie um grupo de gerenciamento com as seguintes configurações. Selecione **Enviar** quando concluir. 

    | Configuração | Valor |
    | --- | --- |
    | ID do grupo de gerenciamento | `az104-mg1` (deve ser exclusivo no diretório) |
    | Nome de exibição do grupo de gerenciamento | `az104-mg1` |

1. **Atualize** a página do grupo de gerenciamento para garantir que seu novo grupo de gerenciamento seja exibido. Isso pode levar alguns minutos. 

   >**Observação:** Você notou o grupo de gerenciamento raiz? O grupo de gerenciamento raiz é compilado na hierarquia para que todos os grupos de gerenciamento e assinaturas sejam dobrados nele. Esse grupo de gerenciamento raiz permite que políticas globais e atribuições de função do Azure sejam aplicadas no nível de diretório. Depois de criar um grupo de gerenciamento, você adicionará todas as assinaturas que devem ser incluídas no grupo. 

## Tarefa 2: Examinar e atribuir uma função interna do Azure

Nesta tarefa, você examinará as funções internas e atribuirá a função Colaborador de VM a um membro do Suporte Técnico. O Azure fornece um grande número de [funções internas](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles). 

>**Observação:** Nas etapas a seguir, você atribuirá a função ao **grupo de suporte técnico**. Se você não tiver um grupo do Suporte Técnico, reserve um minuto para criá-lo.

1. Selecione o grupo de gerenciamento **az104-mg1**.

1. Selecione a folha **Controle de acesso (IAM)** e, em seguida, a guia **Funções**.

1. Role as definições de função interna que estão disponíveis. **Exiba** uma função para obter informações detalhadas sobre as **Permissões**, **JSON** e **Atribuições**. Muitas vezes, você usará *proprietário*, *colaborador* e *leitor*. 

1. No menu suspenso, selecione **+ Adicionar** e, em seguida, selecione **Adicionar atribuição de função**. 

1. Na folha **Adicionar atribuição de função**, pesquise e selecione o **Colaborador da Máquina Virtual**. A função de colaborador da máquina virtual permite que você gerencie máquinas virtuais, mas não acesse seu sistema operacional ou gerencie a rede virtual e a conta de armazenamento às quais estão conectadas. Essa é uma boa função para o Suporte Técnico. Selecione **Avançar**.

    >**Você sabia?** O Azure originalmente forneceu apenas o modelo de implantação **Clássico**. Isso foi substituído pelo modelo de implantação do **Azure Resource Manager**. Como melhor prática, não use os recursos clássicos.

1. Na guia **Membros**, **Selecionar Membros**.

1. Pesquise e selecione o grupo `helpdesk`. Clique em **Selecionar**. 

1. Clique em **Examinar + atribuir** duas vezes para criar a atribuição de função.

1. Continue na folha **Controle de acesso (IAM)**. Na guia **Atribuições de função**, confirme se o grupo de **suporte técnico** tem a função **Colaborador da Máquina Virtual**. 

    >**Observação:** Como melhor prática, sempre atribua funções a grupos e não a indivíduos. 

    >**Você sabia?** Esta atribuição pode não conceder privilégios adicionais. Se você já tiver a função Proprietário, essa função inclui todas as permissões associadas à função Colaborador da VM.
    
## Tarefa 3: Criar uma função RBAC personalizada

Nesta tarefa, você criará uma função RBAC personalizada. As funções personalizadas são uma parte central da implementação do princípio de privilégio mínimo de um ambiente. As funções internas podem ter muitas permissões para seu cenário. Também criaremos uma nova função e removeremos as permissões que não são necessárias. Você tem um plano para gerenciar as permissões sobrepostas?

1. Continue trabalhando em seu grupo de gerenciamento. Navague até a folha **Controle de Acesso (IAM)**.

1. No menu suspenso, selecione **+ Adicionar** e, em seguida, selecione **Adicionar função personalizada**.

1. Na guia Noções básicas, conclua a configuração.

    | Configuração | Valor |
    | --- | --- |
    | Nome da função personalizada | `Custom Support Request` |
    | Descrição | `A custom contributor role for support requests.` |

1. Para **Permissões de linha de base**, selecione **Clonar uma função**. No menu suspenso **Função para clonar**, selecione **Colaborador da solicitação de suporte**.

    ![Captura de tela clonar uma função.](../media/az104-lab02a-clone-role.png)

1. Selecione **Avançar** para ir para a guia **Permissões** e selecione **+ Excluir permissões**.

1. No campo de pesquisa do provedor de recursos, insira `.Support` e selecione **Microsoft.Support**.

1. Na lista de permissões, coloque uma caixa de seleção ao lado de **Outros: Registra o Provedor de Recursos de Suporte** e, em seguida, selecione **Adicionar**. A função deve ser atualizada para incluir essa permissão como um *NotAction*.

    >**Observação:** Um provedor de recursos do Azure é um conjunto de operações REST que habilitam a funcionalidade para um serviço específico do Azure. Não queremos que o Suporte Técnico tenha essa funcionalidade, portanto, ele está sendo removido da função clonada. 

1. Na guia **Escopos atribuíveis**, certifique-se de que o grupo de gerenciamento está listado e clique em **Avançar**.

1. Examine o JSON para as *Ações*, *NotActions* e *AssignableScopes* que são personalizados na função. 

1. Clique em **Examinar + Criar**, depois em **Criar**.

    >**Observação:** Neste ponto, você criou uma função personalizada e a atribuiu ao grupo de gerenciamento.  

## Tarefa 4: Monitorar as atribuições de função com o Log de Atividades

Nesta tarefa, você exibirá o log de atividades para determinar se alguém criou uma nova função. 

1. No portal, localize o recurso **az104-mg1** e selecione **Log de atividades**. O log de atividades fornece insights sobre eventos no nível da assinatura. 

1. Examine as atividades das atribuições de função. O log de atividades pode ser filtrado para operações específicas. 

    ![Captura de tela da página Log de atividades com filtro configurado.](../media/az104-lab02a-searchactivitylog.png)

## Limpar os recursos

Se você estiver trabalhando com **sua própria assinatura**, reserve um minuto para excluir os recursos do laboratório. Isso garantirá que os recursos sejam liberados e que o custo seja minimizado. A maneira mais fácil de excluir os recursos do laboratório é excluir o grupo de recursos do laboratório. 

+ No portal do Azure, selecione o grupo de recursos, selecione **Excluir o grupo de recursos**, **Inserir o nome do grupo de recursos** e clique em **Excluir**.
+ Usar o Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usar a CLI, `az group delete --name resourceGroupName`.
  
## Estender seu aprendizado com o Copilot

O Copilot pode ajudar você a aprender a usar as ferramentas de script do Azure. O Copilot também pode ajudar em áreas não cobertas no laboratório ou onde você precisar de mais informações. Abra um navegador do Edge e escolha Copilot (canto superior direito) ou navegue até *copilot.microsoft.com*. Reserve alguns minutos para experimentar essas solicitações.
+ Crie duas tabelas realçando comandos importantes do PowerShell e da CLI para obter informações sobre assinaturas da organização no Azure e explique cada comando na coluna “Explicação”. 
+ Qual é o formato do arquivo JSON do RBAC do Azure?
+ Quais são as etapas básicas para criar uma função RBAC personalizada do Azure?
+ Qual é a diferença entre as funções RBAC do Azure e as funções do Microsoft Entra ID?

## Saiba mais com treinamento individual

+ [Proteger seus recursos do Azure com o controle de acesso baseado em função do Azure (Azure RBAC)](https://learn.microsoft.com/training/modules/secure-azure-resources-with-rbac/). Use o Azure RBAC para gerenciar o acesso aos recursos no Azure.

## Principais aspectos a serem lembrados

Parabéns por concluir o laboratório. Aqui estão as principais lições deste laboratório. 

+ Os grupos de gerenciamento são usados para organizar logicamente as assinaturas.
+ O grupo de gerenciamento raiz interno inclui todos os grupos de gerenciamento e assinaturas.
+ O Azure tem muitas funções internas. Você pode atribuir essas funções para controlar o acesso aos recursos.
+ Você pode criar novas funções ou personalizar as funções existentes.
+ As funções são definidas em um arquivo formatado JSON e incluem *Actions*, *NotActions* e *AssignableScopes*.
+ Você pode usar o Log de Atividades para monitorar as atribuições de função.











