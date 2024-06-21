---
lab:
  title: 'Laboratório 01: Gerenciar identidades do Microsoft Entra ID'
  module: Administer Identity
---

# Laboratório 01 - Gerenciar identidades do Microsoft Entra ID

## Introdução ao laboratório

Este é o primeiro de uma série de laboratórios para administradores do Azure. Neste laboratório, você aprenderá sobre usuários e grupos. Usuários e grupos são os blocos de construção básicos para uma solução de identidade. 

Este laboratório requer uma assinatura do Azure. Seu tipo de assinatura pode afetar a disponibilidade de recursos neste laboratório. Você pode alterar a região, mas as etapas são escritas usando o **Leste dos EUA**. 


## Tempo estimado: 30 minutos

## Cenário do laboratório

Sua organização está criando um novo ambiente de laboratório para testes de pré-produção de aplicativos e serviços.  Alguns engenheiros estão sendo contratados para gerenciar o ambiente de laboratório, incluindo as máquinas virtuais. Para permitir que os engenheiros se autentiquem usando o Microsoft Entra ID, você foi encarregado de provisionar usuários e grupos. Para minimizar a sobrecarga administrativa, a associação dos grupos deve ser atualizada automaticamente com base nos cargos. 

## Simulação interativa de laboratório

Este laboratório usa uma simulação de laboratório interativa. A simulação permite que você clique em um cenário semelhante em seu próprio ritmo. Há diferenças entre a simulação interativa e este laboratório, mas muitos dos principais conceitos são os mesmos. Não é necessária uma assinatura do Azure.

>**Observação:** Esta simulação está sendo atualizada. O Microsoft Entra ID é o novo nome do Azure Active Directory (Azure AD). 

+ [Gerenciar as identidades do Entra ID](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201). Crie e configure usuários e atribua a grupos. Crie um locatário do Azure e gerencie as contas de convidado. 

## Diagrama de arquitetura
![Faça um diagrama da arquitetura do laboratório 01.](../media/az104-lab01-architecture.png)

## Habilidades de trabalho

+ Tarefa 1: Criar e configurar as contas de usuário.
+ Tarefa 2: Criar grupos e adicionar membros.

## Tarefa 1: Criar e configurar as contas de usuário

Nesta tarefa, você criará e configurará as contas de usuário. As contas de usuário armazenarão dados do usuário, como nome, departamento, localização e informações de contato.

1. Entre no **portal do Azure** - `https://portal.azure.com`.

    >**Observação:** O portal do Azure é usado em todos os laboratórios. Se você for novo no Azure, pesquise e selecione `Quickstart Center`. Reserve alguns minutos para assistir ao vídeo **Introdução ao portal do Azure**. Mesmo que você já tenha usado o portal antes, encontrará algumas dicas e truques para navegar e personalizar a interface.
    
1. Pesquise e selecione `Microsoft Entra ID`. O Microsoft Entra ID é a solução de gerenciamento de identidade e acesso baseada em nuvem do Azure. Reserve alguns minutos para se familiarizar com alguns dos recursos listados no painel esquerdo. 

1. Selecione a folha **Visão geral** e, em seguida, a guia **Gerenciar locatários**. 

    >**Você sabia?** Um locatário é uma instância específica do Microsoft Entra ID que contém contas e grupos. Dependendo da sua situação, você pode criar mais locatários e **Alternar** entre eles. 

1. Retorne à página **Entra ID** e selecione **Licenças**. A partir daqui, você poderá comprar uma licença, gerenciar as licenças que tem e atribuir licenças a usuários e grupos. Selecione **Recursos licenciados** para ver o que está disponível.
   
### Criar um novo usuário

1. Selecione **Usuários** e, na lista suspensa **Novo usuário**, selecione **Criar novo usuário**. 

1. Crie um novo usuário com as seguintes configurações (deixe as outras com seus padrões). Na guia **Propriedades**, observe todos os diferentes tipos de informações que podem ser incluídas na conta de usuário. 

    | Configuração | Valor |
    | --- | --- |
    | Nome UPN | `az104-user1` |
    | Nome de exibição | `az104-user1` |
    | Gerar senha automaticamente | **checked** |
    | Conta habilitada | **checked** |
    | Cargo (guia Propriedades) | `IT Lab Administrator` |
    | Departamento (guia Propriedades) | `IT` |
    | Local de uso (guia Propriedades) | **Estados Unidos** |

1. Depois de concluir a análise, selecione **Examinar + criar** e, em seguida, **Criar**.

1. Atualize a página e confirme se o novo usuário foi criado. 

### Convidar um usuário externo

1. Na lista suspensa **Novo usuário**, selecione **Convidar um usuário externo**. 

    | Configuração | Valor |
    | --- | --- |
    | Email | seu endereço de email |
    | Nome de exibição | seu nome |
    | Enviar mensagem de convite | **marque a caixa** |
    | Mensagem | `Welcome to Azure and our group project` |

1. Mover para a guia **Propriedades**. Conclua as informações básicas, incluindo estes campos. 

    | Configuração | Valor |
    | --- | --- |
    | Cargo  | `IT Lab Administrator` |
    | department  | `IT` |
    | Local de uso (guia Propriedades) | **Estados Unidos** |

1. Selecione **Examinar + convidar** e, em seguida, **Convidar**.

1. **Atualize** a página e confirme se o usuário convidado foi criado. Você deve receber o email de convite em breve. 

    >**Observação:** É improvável que você crie as contas de usuário individualmente. Você sabe como sua organização planeja criar e gerenciar as contas de usuário?
    
## Tarefa 2: Criar grupos e adicionar membros

Nesta tarefa, você cria uma conta de grupo. As contas de grupo podem incluir contas de usuário ou dispositivos. Estas são duas maneiras básicas de atribuir membros aos grupos: Estaticamente e dinamicamente. Os grupos estáticos exigem que os administradores adicionem e removam membros manualmente.  Os grupos dinâmicos são atualizados automaticamente com base nas propriedades de uma conta de usuário ou dispositivo. Por exemplo, cargo. 

1. No portal do Azure, pesquise e selecione `Groups`.

1. Reserve um minuto para se familiarizar com as configurações de grupo no painel esquerdo.

   + A **expiração** permite configurar o tempo de vida de um grupo em dias. Após esse tempo, o grupo deverá ser renovado pelo proprietário.
   + A **política de nomenclatura** permite configurar palavras bloqueadas e adicionar um prefixo ou sufixo a nomes de grupo.

1. Na folha **Todos os grupos**, selecione **+ Novo grupo** e crie um novo grupo.     

    | Configuração | Valor |
    | --- | --- |
    | Tipo de grupo | **Segurança** |
    | Nome do grupo | `IT Lab Administrators` |
    | Descrição do grupo | `Administrators that manage the IT lab` |
    | Tipo de afiliação | **Atribuído** |

    >**Observação**: Uma licença Entra ID Premium P1 ou P2 é necessária para a associação dinâmica. Se outros **Tipos de associação** estiverem disponíveis, as opções aparecerão na lista suspensa. 
    
    ![Captura de tela da criação do grupo atribuído.](../media/az104-lab01-create-assigned-group.png)

1. Selecione **Nenhum proprietário selecionado**.

1. Na página **Adicionar proprietários**, pesquise e **selecione** você mesmo (mostrado no canto superior direito) como proprietário. Observe que você pode ter mais de um proprietário. 

1. Escolha **Nenhum membro selecionado**.

1. No painel **Adicionar membros**, pesquise e **selecione** o **az104-user1** e o **usuário convidado** que você convidou. Adicione os dois usuários ao grupo. 

1. Selecione **Criar** para implantar o grupo.

1. **Atualize** a página e verifique se o grupo foi criado.

1. Selecione o novo grupo e examine as informações de **Membros** e **Proprietários**.

>**Observação:** Você pode estar gerenciando um grande número de grupos. Sua organização tem um plano para criar grupos e adicionar membros?
   
## Limpar os recursos

Se você estiver trabalhando com **sua própria assinatura**, reserve um minuto para excluir os recursos do laboratório. Isso garantirá que os recursos sejam liberados e que o custo seja minimizado. A maneira mais fácil de excluir os recursos do laboratório é excluir o grupo de recursos do laboratório. 

+ No portal do Azure, selecione o grupo de recursos, selecione **Excluir o grupo de recursos**, **Inserir o nome do grupo de recursos** e clique em **Excluir**.
+ Usar o Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usar a CLI, `az group delete --name resourceGroupName`.
  

## Estender seu aprendizado com o Copilot

O Copilot pode ajudar você a aprender a usar as ferramentas de script do Azure. O Copilot também pode ajudar em áreas não cobertas no laboratório ou onde você precisar de mais informações. Abra um navegador do Edge e escolha Copilot (canto superior direito) ou navegue até *copilot.microsoft.com*. Reserve alguns minutos para experimentar essas solicitações.
+ Quais são os comandos do Azure PowerShell e da CLI para criar um grupo de segurança chamado Administradores de TI? Forneça a página de referência de comando oficial.  
+ Forneça uma estratégia passo a passo para gerenciar usuários e grupos no Microsoft Entra ID.
+ Quais são as etapas no portal do Azure para criar usuários e grupos em massa?
+ Forneça uma tabela de comparação de contas de usuário internas e externas do Microsoft Entra ID. 


## Saiba mais com treinamento individual

+ [Entender o Microsoft Entra ID](https://learn.microsoft.com/training/modules/understand-azure-active-directory/). Compare o Microsoft Entra ID com o Active Directory DS. Saiba mais sobre o Microsoft Entra ID P1 e P2 e explore o Microsoft Entra Domain Services para gerenciar dispositivos e aplicativos conectados ao domínio na nuvem.
+ [Criar usuários e grupos do Azure no Microsoft Entra ID](https://learn.microsoft.com//training/modules/create-users-and-groups-in-azure-active-directory/). Crie usuários no Microsoft Entra ID. Entenda os diferentes tipos de grupos. Crie um grupo e adicione membros a ele. Gerencie contas de convidados entre empresas.
+ [Permita que os usuários redefinam a senha com a redefinição de senha self-service do Microsoft Entra](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/). Avalie a redefinição de senha self-service para permitir que os usuários em sua organização redefinam suas senhas ou desbloqueiem suas contas. Configure, ajuste e teste a redefinição de senha self-service.


## Principais aspectos a serem lembrados

Parabéns por concluir o laboratório. Aqui estão algumas das principais etapas deste laboratório:

+ Um locatário representa sua organização e ajuda você a gerenciar uma instância específica dos serviços de nuvem da Microsoft para seus usuários internos e externos.
+ O Microsoft Entra ID tem contas de usuário e de convidado. Cada conta tem um nível de acesso específico ao escopo do trabalho que deve ser feito.
+ Os grupos combinam usuários ou dispositivos relacionados. Há dois tipos de grupos, incluindo Segurança e Microsoft 365.
+ A associação de grupo pode ser atribuída estaticamente ou dinamicamente.
