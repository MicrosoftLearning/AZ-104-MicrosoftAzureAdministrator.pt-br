---
demo:
  title: 'Demonstração 09: Administrar opções de computação PaaS'
  module: Administer PaaS Compute Options
---

# 09 - Administrar opções de computação PaaS

## Configurar planos do Serviço de Aplicativo do Azure

Nesta demonstração, criaremos e trabalharemos com planos do Serviço de Aplicativo do Azure.

**Referência**: [Gerenciar plano do Serviço de Aplicativo - Serviço de Aplicativo do Azure](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

Escalar verticalmente um aplicativo no Serviço de Aplicativo do Azure

Dimensionamento automático no Serviço de Aplicativo do Azure

1. Use o portal do Azure. 

1. Pesquise e selecione  **Planos de Serviços de Aplicativos**.

1. Criar um serviço de aplicativo simples Discuta a necessidade de selecionar Windows ou Linux. Discuta os planos de preços agora ou nas próximas etapas. 

1. Implante seu novo plano de serviço de aplicativo. 

1. Escalar verticalmente o Plano do Serviço de Aplicativo. Discuta a diferença entre **os planos de Desenvolvimento/Teste** e **Produção** . Revise a lista de recursos. 

1. Escalar horizontalmente o Plano do Serviço de Aplicativo. Analise a diferença entre **Manual** e **Baseado em** Regras. 

## Configurar os Serviços de Aplicativos do Azure

Nesta demonstração, criaremos um novo aplicativo Web que executa um contêiner do Docker. O contêiner exibe uma mensagem de boas-vindas.

**Referência**: [Criar um aplicativo Web](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

Nesta tarefa, criaremos um Aplicativo Web do Serviço de Aplicativo do Azure.

1. Use o portal do Azure. 

1. Pesquise pelos Serviços de Aplicativos e selecione essa opção.

1. Crie um aplicativo Web.

    - Publicar: **código** Analise outras opções.
    - Pilha de tempo de execução: **.Net**. Analise outras opções.
    - Sistema Operacional: Linux

1. Selecione o **plano de serviço F1** gratuito.

1. **Revisar + criar** o aplicativo Web. Aguarde até o recurso ser implantado.

1. **Na página Visão geral**, verifique se o **Status** está **em execução**.

1. Selecione a URL** e verifique se a **página de espaço reservado padrão é carregada.

1. À medida que tiver tempo, explore as opções de slots** de **implantação.
   
## Configurar Instâncias de Contêiner do Azure

Nesta demonstração, criamos, configuramos e implantamos um contêiner usando as Instâncias de Contêiner do Azure (ACI) do Portal do Azure. O aplicativo ACI exibe uma página HTML estática com a imagem pública do Microsoft Hello World. 

Início Rápido – Implantar um contêiner do Docker na instância de contêiner – Portal

1. Use o portal do Azure.

1. Pesquise e selecione **Instâncias** de contêiner.

1. Criar uma instância de contêiner 

1. Preencha o grupo** de recursos e **o nome** do **contêiner. 

1. Discuta as opções de origem** da **imagem. Usar imagem de início rápido

1. Para **imagem** de contêiner use **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)**. Esta imagem de exemplo do Linux empacota um pequeno aplicativo Web escrito no Node.js que veicula a uma página HTML estática.

1. Na página **Rede**, especifique um **Rótulo de nome DNS** para seu contêiner. 

1. Deixe todas as outras configurações com seus padrões e selecione **Examinar + criar**.

1. Aguarde até o recurso ser implantado.

1. **Na página Visão geral** do recurso, verifique se o **Status** está **em execução**.

1. Navegue até o **FQDN** da instância do contêiner e verifique se a página de boas-vindas é exibida. 

**Nota**: Para evitar custos adicionais, exclua o recurso. 

## Criar e configurar Aplicativos de Contêiner do Azure

Nesta demonstração, criaremos e trabalharemos com os Aplicativos de Contêiner do Azure. 

Guia de Início Rápido: Implantar seu primeiro aplicativo de contêiner usando o portal do Azure

1. Pesquise e selecione **Condição**.

1. Conclua os detalhes** do **projeto e crie o ambiente** de aplicativos **de contêiner.

1. **Revise e crie** o aplicativo de contêiner.

1. Selecione o link ao lado de **URL do Aplicativo** para exibir o aplicativo.

1. Verifique se o navegador exibe a **mensagem Bem-vindo aos Aplicativos** de Contêiner do Azure. 






