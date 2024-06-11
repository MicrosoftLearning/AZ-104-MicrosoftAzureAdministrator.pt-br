---
lab:
  title: "Laboratório\_06: Implementar o Gerenciamento de Tráfego"
  module: Administer Network Traffic Management
---

# Laboratório 06 – Implementar o Gerenciamento de Tráfego

## Introdução ao laboratório

Neste laboratório, você aprenderá a configurar e testar um Load Balancer público e um Gateway de Aplicativo.

Este laboratório requer uma assinatura do Azure. Seu tipo de assinatura pode afetar a disponibilidade de recursos neste laboratório. Você pode alterar a região, mas as etapas são escritas usando o **Leste dos EUA**.

## Tempo estimado: 50 minutos

## Cenário do laboratório

Sua organização tem um site público. Você precisa balancear a carga de solicitações públicas de entrada em diferentes máquinas virtuais. Você também precisa fornecer imagens e vídeos de diferentes máquinas virtuais. Você planeja implementar um Azure Load Balancer e um Gateway de Aplicativo do Azure. Todos os recursos estão na mesma região.

## Simulações interativas do laboratório

Há simulações interativas do laboratório que podem ser úteis para este tópico. A simulação permite que você clique em um cenário semelhante em seu próprio ritmo. Há diferenças entre a simulação interativa e este laboratório, mas muitos dos principais conceitos são os mesmos. Uma assinatura do Azure não é necessária.

+ [Criar e configurar um balanceador de carga do Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Create%20and%20configure%20an%20Azure%20load%20balancer). Crie uma rede virtual, servidores back-end, balanceador de carga e, em seguida, teste o balanceador de carga.
+ [Implantar o Gateway de Aplicativo do Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Deploy%20Azure%20Application%20Gateway). Crie um gateway de aplicativo, crie máquinas virtuais, crie o pool de back-end e teste o gateway.
+ [Implementar o gerenciamento de tráfego](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2010). Implemente a rede hub e spoke completa, incluindo máquinas virtuais, redes virtuais, emparelhamento, balanceador de carga e gateway de aplicativo.

## Habilidades de trabalho

+ Tarefa 1: Use um modelo para provisionar uma infraestrutura.
+ Tarefa 2: Configurar o Azure Load Balancer.
+ Tarefa 3: Configure o Gateway de Aplicativo do Azure.

## Tarefa 1: Usar um modelo para provisionar uma infraestrutura

Nesta tarefa, você usará um modelo para implantar uma rede virtual, um grupo de segurança de rede e duas máquinas virtuais.

1. Baixe os arquivos do laboratório **\\Allfiles\\Lab06** (modelo e parâmetros).

1. Entre no **portal do Azure** - `https://portal.azure.com`.

1. Pesquise e selecione `Deploy a custom template`.

1. Na página de implantação personalizada, selecione **Criar seu modelo no editor**.

1. Na página Editar modelo, selecione **Carregar arquivo**.

1. Localize e selecione o arquivo **\\Allfiles\\Lab06\\az104-06-vms-template.json** e selecione **Abrir**.

1. Selecione **Salvar**.

1. Selecione **Editar parâmetros** e carregue o arquivo **\\Allfiles\\Lab06\\az104-06-vms-parameters.json**.

1. Selecione **Salvar**.

1. Use as informações a seguir para preencher os campos na página de implantação personalizada, deixando todos os outros campos com o valor padrão.

    | Configuração       | Valor         |
    | ---           | ---           |
    | Subscription  | sua assinatura do Azure |
    | Resource group | `az104-rg6` (Se necessário, selecione **Criar novo**) |
    | Senha      | Forneça uma senha segura |

    >**Observação**: Se você receber um erro de que o tamanho da VM não está disponível, selecione um SKU disponível em sua assinatura e tenha pelo menos dois núcleos.

1. Selecione **Examinar + Criar** e **Criar**.

    >**Observação**: Aguarde a conclusão da implantação antes de passar para a próxima tarefa. A implantação deve levar aproximadamente 5 minutos.

    >**Observação**: Examine os recursos que estão sendo implantados. Haverá uma rede virtual com três sub-redes. Cada sub-rede terá uma máquina virtual.

## Tarefa 2: Configurar o Azure Load Balancer

Nesta tarefa, você implementa um Azure Load Balancer na frente das duas máquinas virtuais do Azure na rede virtual. Os Balanceadores de Carga no Azure fornecem conectividade de camada 4 nos recursos, como as máquinas virtuais. A configuração do Load Balancer inclui um endereço IP front-end para aceitar as conexões, um pool de back-end e as regras que definem como as conexões devem percorrer o balanceador de carga.

## Diagrama de arquitetura – Load Balancer

>**Observação**: Observe que o Load Balancer está distribuindo em duas máquinas virtuais na mesma rede virtual.

![Diagrama das tarefas do laboratório.](../media/az104-lab06-lb-architecture.png)

1. No portal do Azure, pesquise e selecione `Load balancers` e, na folha **Balanceadores de carga**, clique em **+ Criar**.

1. Crie um balanceador de carga com as seguintes configurações (deixe as outras com os seus valores padrão) e clique em **Avançar: Configuração de IP de front-end**:

    | Configuração | Valor |
    | --- | --- |
    | Subscription | sua assinatura do Azure |
    | Resource group | **az104-rg6** |
    | Nome | `az104-lb` |
    | Region | A **mesma** região que você implantou as VMs |
    | SKU  | **Standard** |
    | Tipo | **Pública** |
    | Camada | **Regional** |

     ![Captura de tela da página Criar balanceador de carga.](../media/az104-lab06-create-lb1.png)

1. Na guia **Configuração de IP de front-end**, clique em **Adicionar uma configuração de IP de front-end** e use as seguintes configurações:  

    | Configuração | Valor |
    | --- | --- |
    | Nome | `az104-fe` |
    | Tipo de IP | Endereço IP |
    | Balanceador de Carga para Gateway | Nenhum |
    | Endereço IP público | Selecione **Criar novo** (use as instruções na próxima etapa) |

1. No pop-up **Adicionar um endereço IP público**, use as seguintes configurações antes de clicar em **OK** e, em seguida, **Adicionar**. Quando concluído, clique em **Avançar: Pools de back-end**.

    | Configuração | Valor |
    | --- | --- |
    | Nome | `az104-lbpip` |
    | SKU | Standard |
    | Camada | Regional |
    | Atribuição | Estático |
    | Preferência de Roteamento | **Rede da Microsoft** |

    >**Observação:** O SKU Standard fornece um endereço IP estático. Os endereços IP estáticos são atribuídos com o recurso, são criados e liberados quando o recurso é excluído.  

1. Na guia **Pools de back-end**, clique em **Adicionar um pool de back-end** com as seguintes configurações (deixe as outras com seus valores padrão). Clique em **+ Adicionar** (duas vezes) e clique em **Avançar: Regras de entrada**.

    | Configuração | Valor |
    | --- | --- |
    | Nome | `az104-be` |
    | Rede virtual | **az104-06-vnet1** |
    | Configuração do pool de back-end | **NIC** |
    | Clique em **Adicionar** para adicionar uma máquina virtual |  |
    | az104-06-vm0 | **marque a caixa** |
    | az104-06-vm1 | **marque a caixa** |

1. Como você tem tempo, examine as outras guias e clique em **Examinar + criar**. Verifique se não há erros de validação e clique em **Criar**.

1. Aguarde até que o balanceador de carga seja implantado e clique em **Ir para o recurso**.

**Adicionar uma regra para determinar como o tráfego de entrada é distribuído**

1. Na folha **Configurações**, selecione **Regras de balanceamento de carga**.

1. Selecione **+ Adicionar**. Adicione uma regra de balanceamento de carga com as seguintes configurações (deixe as outras com seus valores padrão).  Ao configurar a regra, use os ícones informativos para saber mais sobre cada configuração. Quando terminar, clique em **Salvar**.

    | Configuração | Valor |
    | --- | --- |
    | Nome | `az104-lbrule` |
    | Versão IP | **IPv4** |
    | Endereço IP de front-end | **az104-fe** |
    | Pool de back-end | **az104-be** |
    | Protocolo | **TCP** |
    | Porta | `80` |
    | Porta de back-end | `80` |
    | Investigação de integridade | **Criar novo** |
    | Nome | `az104-hp` |
    | Protocolo | **TCP** |
    | Porta | `80` |
    | Intervalo | `5` |
    | Fechar a janela Criar investigação de integridade | **Salvar** |
    | Persistência de sessão | **Nenhuma** |
    | Tempo limite de ociosidade (minutos) | `4` |
    | Redefinição de TCP | **Desabilitado** |
    | IP flutuante | **Desabilitado** |
    | SNAT (conversão de endereços de rede de origem) de saída | **Recomendado** |

1. Selecione a **configuração de IP de front-end** na página Load Balancer. Copie o endereço IP público.

1. Abra outra guia do navegador e navegue até o endereço IP. Verifique se a janela do navegador exibe a mensagem **Olá, Mundo de az104-06-vm0** ou **Olá, Mundo de az104-06-vm1**.

1. Atualize a janela para verificar se a mensagem é alterada para a outra máquina virtual. Isso demonstra o balanceador de carga girando pelas máquinas virtuais.

    > **Observação**: Talvez seja necessário atualizar mais de uma vez ou abrir uma nova janela do navegador no modo InPrivate.

## Tarefa 3: Configurar um Gateway de Aplicativo do Azure

Nesta tarefa, você implementa um Gateway de Aplicativo do Azure na frente de duas máquinas virtuais do Azure. Um Gateway de Aplicativo fornece balanceamento de carga de camada 7, WAF (Firewall de Aplicativo Web), terminação SSL e criptografia de ponta a ponta para os recursos definidos no pool de back-end. O Gateway de Aplicativo roteia imagens para uma máquina virtual e vídeos para a outra máquina virtual.

## Diagrama de arquitetura – Gateway de Aplicativo

>**Observação**: Este Gateway de Aplicativo está funcionando na mesma rede virtual que o Load Balancer. Isso pode não ser típico em um ambiente de produção.

![Diagrama das tarefas do laboratório.](../media/az104-lab06-gw-architecture.png)

1. No portal do Azure, pesquise e selecione `Virtual networks`.

1. Na folha **Redes virtuais**, na lista de redes virtuais, clique em **az104-06-vnet1**.

1. Na folha de rede virtual **az104-06-vnet1**, na seção **Configurações**, clique em **Sub-redes** e clique em **+ Sub-rede**.

1. Adicione uma sub-rede com as seguintes configurações (deixe as outras com seus valores padrão).

    | Configuração | Valor |
    | --- | --- |
    | Nome | `subnet-appgw` |
    | Intervalo de endereços da sub-rede | `10.60.3.224/27` |

1. Clique em **Salvar**

    > **Observação**: Esta sub-rede será usada pelo Gateway de Aplicativo do Azure. O Gateway de Aplicativo requer uma sub-rede dedicada de /27 ou tamanho maior.

1. No portal do Azure, pesquise e selecione `Application gateways` e, na folha **Gateways de Aplicativo**, clique em **+ Criar**.

1. Na guia **Noções básicas**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Subscription | sua assinatura do Azure |
    | Resource group | `az104-rg6` |
    | Nome do gateway de aplicativo | `az104-appgw` |
    | Region | A **mesma** região do Azure que você usou na Tarefa 1 |
    | Camada | **Padrão V2** |
    | Habilitar o dimensionamento automático | **Não** |
    | Contagem mínima de instâncias | `2` |
    | Zona de disponibilidade | **Zona 1** |
    | HTTP2 | **Desabilitado** |
    | Rede virtual | **az104-06-vnet1** |
    | Sub-rede | **subnet-appgw (10.60.3.224/27)** |

    ![Captura de tela da página Criar gateway de aplicativo.](../media/az104-lab06-create-appgw.png)

1. Clique em **Avançar: Front-ends >** e especifique as seguintes configurações (deixe as outras com seus valores padrão). Ao concluir, clique em **OK**.

    | Configuração | Valor |
    | --- | --- |
    | Tipo de endereço IP de front-end | **Pública** |
    | Endereço IP público| **Adicionar nova** |
    | Nome | `az104-gwpip` |
    | Zona de disponibilidade | **Nenhuma** |

    >**Observação:** O Gateway de Aplicativo pode ter um endereço IP público e privado.
 
1. Clique em **Avançar: Back-ends >** e, em seguida, **Adicionar um pool de back-end**. Especifique as seguintes configurações (deixe as outras com seus valores padrão). Quando concluído, clique em **Adicionar**.

    | Configuração | Valor |
    | --- | --- |
    | Nome | `az104-appgwbe` |
    | Adicionar pool de back-end sem destinos | **Não** |
    | Máquina virtual | **az104-rg6-nic1 (10.60.1.4)** |
    | Máquina virtual | **az104-rg6-nic2 (10.60.2.4)** |

1. Clique em **Adicionar um pool de back-end**. Este é o pool de back-end para **imagens**. Especifique as seguintes configurações (deixe as outras com seus valores padrão). Quando concluído, clique em **Adicionar**.

    | Configuração | Valor |
    | --- | --- |
    | Nome | `az104-imagebe` |
    | Adicionar pool de back-end sem destinos | **Não** |
    | Máquina virtual | **az104-rg6-nic1 (10.60.1.4)** |

1. Clique em **Adicionar um pool de back-end**. Este é o pool de back-end para **vídeo**. Especifique as seguintes configurações (deixe as outras com seus valores padrão). Quando concluído, clique em **Adicionar**.

    | Configuração | Valor |
    | --- | --- |
    | Nome | `az104-videobe` |
    | Adicionar pool de back-end sem destinos | **Não** |
    | Máquina virtual | **az104-rg6-nic2 (10.60.2.4)** |

1. Selecione **Avançar: Configuração >** e, em seguida, **Adicionar regras de roteamento**. Conclua as informações.

    | Configuração | Valor |
    | --- | --- |
    | Nome da regra | `az104-gwrule` |
    | Prioridade | `10` |
    | Nome do ouvinte | `az104-listener` |
    | IP de front-end | **IPv4 público** |
    | Protocolo | **HTTP** |
    | Porta | `80` |
    | Tipo de ouvinte | **Basic** |

1. Mova para a guia **Destinos de back-end**. Selecione **Adicionar** depois de concluir as informações básicas.

   | Configuração | Valor |
    | --- | --- |
    | Destino de back-end | `az104-appgwbe` |
    | Configurações de back-end | `az104-http` (criar novo) |

   >**Observação:** Reserve um minuto para ler as informações sobre **Afinidade baseada em cookie** e **Esvaziamento de conexões**.

1. Na seção **Roteamento baseado em caminho**, selecione **Adicionar vários destinos para criar uma regra baseada em caminho**. Você criará duas regras. Clique em **Adicionar** após a primeira regra e, em seguida, **Adicionar** após a segunda regra. 

    **Regra – roteamento para o back-end de imagens**

    | Configuração | Valor |
    | --- | --- |
    | Caminho | `/image/*` |
    | Nome de destino | `images` |
    | Configurações de back-end | **az104-http** |
    | Destino de back-end | `az104-imagebe` |

    **Regra – roteamento para o back-end de vídeos**

    | Configuração | Valor |
    | --- | --- |
    | Caminho | `/video/*` |
    | Nome de destino | `videos` |
    | Configurações de back-end | **az104-http** |
    | Destino de back-end | `az104-videobe` |

1. Certifique-se de **Salvar** e verificar as alterações e selecione **Avançar: Marcas >**. Nenhuma alteração é necessária.

1. Selecione **Avançar: Examinar + criar >** e clique em **Criar**.

    > **Observação**: Aguarde a criação da instância do Gateway de Aplicativo. Isso levará aproximadamente 5-10 minutos. Enquanto você aguarda, considere revisar alguns dos links de treinamento individual no final desta página.

1. Depois que o gateway de aplicativo for implantado, pesquise e selecione **az104-appgw**.

1. No recurso **Gateway de Aplicativo**, na seção **Monitoramento**, selecione **Integridade do back-end**.

1. Certifique-se de que ambos os servidores no pool de back-end exibam **Íntegro**.

1. Na folha **Visão geral**, copie o valor do **IP de front-end**.

1. Inicie outra janela do navegador e teste este URL – `http://<frontend ip address>/image/`.

1. Verifique se você foi direcionado para o servidor de imagem (vm1).

1. Inicie outra janela do navegador e teste este URL – `http://<frontend ip address>/video/`.

1. Verifique se você foi direcionado para o servidor de vídeo (vm2).

> **Observação**: Talvez seja necessário atualizar mais de uma vez ou abrir uma nova janela do navegador no modo InPrivate.

## Limpar os recursos

Se você estiver trabalhando com **sua própria assinatura**, reserve um minuto para excluir os recursos do laboratório. Isso garantirá que os recursos sejam liberados e que o custo seja minimizado. A maneira mais fácil de excluir os recursos do laboratório é excluir o grupo de recursos do laboratório. 

+ No portal do Azure, selecione o grupo de recursos, selecione **Excluir o grupo de recursos**, **Inserir o nome do grupo de recursos** e clique em **Excluir**.
+ Usar o Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usar a CLI, `az group delete --name resourceGroupName`.

## Estender seu aprendizado com o Copilot

O Copilot pode ajudar você a aprender a usar as ferramentas de script do Azure. O Copilot também pode ajudar em áreas não cobertas no laboratório ou onde você precisar de mais informações. Abra um navegador do Edge e escolha Copilot (canto superior direito) ou navegue até *copilot.microsoft.com*. Reserve alguns minutos para experimentar essas solicitações.

+ Compare e contraste o Azure Load Balancer com o Gateway de Aplicativo do Azure.
+ Como posso solucionar problemas de conectividade de entrada com um Azure Load Balancer?
+ Quais são as etapas básicas para configurar o Gateway de Aplicativo do Azure?
+ Crie uma tabela realçando soluções de balanceamento de carga do Azure. Inclua estas colunas: Protocolos com suporte, balanceamento de carga privado, balanceamento de carga global, políticas de roteamento, ambientes com suporte, drenagem de conexão, afinidade de sessão, balanceamento de carga baseado em host e caminho, descarregamento de TLS, aceleração de site, segurança, armazenamento em cache e compactação.

## Saiba mais com treinamento individual

+ [Melhorar a escalabilidade e a resiliência do aplicativo usando o Azure Load Balancer](https://learn.microsoft.com/training/modules/improve-app-scalability-resiliency-with-load-balancer/). Discuta sobre os diferentes balanceadores de carga no Azure e como escolher a solução correta de balanceador de carga do Azure para atender aos seus requisitos.
+ [Balancear a carga do tráfego de seu serviço Web com o Gateway de Aplicativo](https://learn.microsoft.com/training/modules/load-balance-web-traffic-with-application-gateway/). Aumente a resiliência do aplicativo distribuindo a carga entre vários servidores e use o roteamento baseado em caminho para o direcionar o tráfego da Web.

## Principais aspectos a serem lembrados

Parabéns por concluir o laboratório. Aqui estão as principais lições deste laboratório.

+ O Azure Load Balancer é uma excelente opção para distribuir o tráfego de rede em várias máquinas virtuais na camada de transporte (camada OSI 4 – TCP e UDP).
+ Os balanceadores de carga públicos são usados para balancear a carga do tráfego de Internet para suas VMs. Um balanceador de carga interno (ou privado) é usado quando IPs privados são necessários apenas no front-end.
+ O balanceador de carga Básico é para aplicativos de pequena escala que não precisam de alta disponibilidade ou redundância. O balanceador de carga Standard é para alto desempenho e latência ultra baixa.
+ O Gateway de Aplicativo do Azure é um balanceador de carga de tráfego da Web (camada 7 OSI) que permite que você gerencie o tráfego para seus aplicativos Web.
+ A camada Standard do Gateway de Aplicativo oferece toda a funcionalidade L7, incluindo balanceamento de carga. A camada WAF adiciona um firewall para verificar se há tráfego mal-intencionado.
+ Um Gateway de Aplicativo pode tomar decisões de roteiros com base em outros atributos de uma solicitação HTTP, por exemplo, o caminho de URI ou os cabeçalhos de host.
