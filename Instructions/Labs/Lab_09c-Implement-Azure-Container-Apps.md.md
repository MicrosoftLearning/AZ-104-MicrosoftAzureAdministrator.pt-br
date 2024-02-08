---
lab:
  title: Implementar os Aplicativos de Contêiner do Azure
  module: Administer PaaS Compute Options
---

# Implementar os Aplicativos de Contêiner do Azure
# Manual do aluno

## Cenário do laboratório
Os Aplicativos de Contêiner do Azure permitem que você execute microsserviços e aplicativos conteinerizados em uma plataforma sem servidor. Com os aplicativos de contêiner, você aproveita os benefícios da execução de contêineres, deixando para trás as preocupações de configurar manualmente a infraestrutura de nuvem e orquestradores de contêineres complexos.

## Objetivos

Neste laboratório, você vai:
- Criar um aplicativo de contêiner e um ambiente de aplicativo de contêiner
- Implantar o aplicativo de contêiner
- Tarefa 3: Testar e verificar a implantação do aplicativo de contêiner

Para começar, entre no [portal do Azure](https://portal.azure.com).

## Tempo estimado: 20 minutos

## Criar um aplicativo de contêiner e um ambiente de aplicativo de contêiner

Para criar o aplicativo de contêiner, comece na home page do portal do Azure.

1. Pesquise  na barra de pesquisa .
1. Selecione **Aplicativos de Contêiner** nos resultados da pesquisa.
1. Selecione o botão **Criar**.

### Guia Básico

Na guia *Básico*, execute as ações a seguir.

1. Insira os valores a seguir na seção *Detalhes do projeto*.

    | Configuração | Ação |
    |---|---|
    | Subscription | Selecione sua assinatura do Azure. |
    | Resource group | Selecione **Criar novo** e insira `az104-09c-rg1`. |
    | Nome do aplicativo de contêiner |  Digite `my-container-app`. |

#### Criar um ambiente

Em seguida, crie um ambiente para seu aplicativo de contêiner.

1. Selecione a região apropriada.

    | Configuração | Valor |
    |--|--|
    | Region | **Sua escolha** |

1. No campo *Criar ambiente de Aplicativos de Contêiner*, selecione o link **Criar novo**.
1. Na página *Criar Ambiente de Aplicativos de Contêiner*, na guia *Básico*, insira os seguintes valores:

    | Configuração | Valor |
    |--|--|
    | Nome do ambiente | Digite `my-environment`. |
    | Redundância de zona | Selecione **Desabilitado** |

1. Selecione a guia **Monitoramento** para criar um workspace do Log Analytics.
1. Selecione o link **Criar novo** no campo *Workspace do Log Analytics* e insira os valores a seguir.

    | Configuração | Valor |
    |--|--|
    | Nome | Inserir `my-container-apps-logs` |
  
    O campo Local é preenchido previamente com EUA Central para você.

1. Selecione **OK** e, em seguida, **Criar**. 

1. Clique em **Avançar: Contêiner**.

1. Marque a caixa ao lado de Usar imagem** de **início rápido.

1. Selecione o botão **Revisar e criar** na parte inferior da página. Isso poderá levar alguns minutos para ser executado. 

    Em seguida, as configurações no Aplicativo de Contêiner são verificadas. Se nenhum erro for encontrado, o botão *Criar* será habilitado.  

    Se houver erros, qualquer guia que contiver erros será marcada com um ponto vermelho.  Navegue até a guia apropriada. Os campos que contiverem erros serão realçados em vermelho.  Depois que todos os erros forem corrigidos, selecione **Revisar e criar** novamente.

1. Selecione **Criar**.

    Uma página com a mensagem *A implantação está em andamento* é exibida.  Depois que a implantação for concluída com êxito, você verá a mensagem *Sua implantação foi concluída*.
   
## Tarefa 2: Testar e verificar a implantação do aplicativo de contêiner

1. Selecione **Ir para o recurso** para exibir o novo aplicativo de contêiner.

1. Selecione o link ao lado de *URL do Aplicativo* para exibir o aplicativo.

1. Verifique se você recebe a **mensagem Your Azure Container Apps app is live (Seu aplicativo de contêiner do Azure) está em tempo real** .

## Limpar os recursos

Se caso não pretende usar esse aplicativo, você pode excluir a instância de Aplicativos de Contêiner do Azure e todos os serviços associados removendo o grupo de recursos.

1. Selecione o grupo de recursos **my-container-apps** na seção *Visão geral*.
1. Selecione o botão **Excluir grupo de recursos** na parte superior da *Visão geral* do grupo de recursos.
1. Insira o nome do grupo de recursos e confirme que deseja excluí-lo. 
1. Selecione **Excluir**.
1. O processo para excluir o grupo de recursos pode levar alguns minutos para ser concluído.
