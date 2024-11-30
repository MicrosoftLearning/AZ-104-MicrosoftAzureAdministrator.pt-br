---
lab:
  title: 'Laboratório 04: Implementar Redes Virtuais'
  module: Implement Virtual Networking
---

# Laboratório 04 - Implementar Redes Virtuais

## Introdução ao laboratório

Este laboratório é o primeiro de três laboratórios que se concentra na rede virtual. Nesse laboratório, você aprenderá os conceitos básicos de rede virtual e sub-rede. Você aprenderá a proteger sua rede com grupos de segurança de rede e grupos de segurança de aplicativos. Você também aprenderá sobre zonas e registros DNS. 

Esse laboratório requer uma assinatura do Azure. Seu tipo de assinatura pode afetar a disponibilidade de recursos neste laboratório. Você pode alterar a região, mas as etapas são escritas usando o **Leste dos EUA**.

## Tempo estimado: 50 minutos

## Cenário do laboratório 

Sua organização global planeja implementar redes virtuais. A meta imediata é acomodar todos os recursos existentes. No entanto, a organização está em uma fase de crescimento e quer garantir que haja capacidade adicional para o crescimento.

A rede virtual **CoreServicesVnet** tem o maior número de recursos. Uma grande quantidade de crescimento está prevista, portanto, um espaço de endereços grande é necessário para essa rede virtual.

A rede virtual **ManufacturingVnet** contém sistemas para as operações das instalações de fabricação. A organização está antecipando um grande número de dispositivos conectados internos para que seus sistemas recuperem dados. 

## Simulações interativas do laboratório

Há várias simulações interativas do laboratório que podem ser úteis para este tópico. A simulação permite que você clique em um cenário semelhante em seu próprio ritmo. Há diferenças entre a simulação interativa e este laboratório, mas muitos dos principais conceitos são os mesmos. Não é necessária uma assinatura do Azure. 

+ [Tráfego de rede seguro](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013). Crie uma máquina virtual, uma rede virtual e um grupo de segurança de rede. Adicione regras de grupo de segurança de rede para permitir e proibir tráfego.
  
+ [Criar uma rede virtual simples](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204). Crie uma rede virtual com duas máquinas virtuais. Demonstre que as máquinas virtuais podem se comunicar. 

+ [Projetar e implementar uma rede virtual no Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure). Crie um grupo de recursos e crie redes virtuais com sub-redes.  

+ [Implementar redes virtuais](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208). Crie e configure uma rede virtual, implante máquinas virtuais, configure grupos de segurança de rede e configure o DNS do Azure.

## Diagrama de arquitetura

![Layout de rede](../media/az104-lab04-architecture.png)

Essas redes virtuais e sub-redes são estruturadas de forma a acomodar os recursos existentes e ainda permitir o crescimento projetado. Vamos criar essas redes virtuais e sub-redes para estabelecer a base para nossa infraestrutura de rede.

>**Você sabia?**: É uma boa prática evitar a sobreposição de intervalos de endereços IP para reduzir problemas e simplificar a solução de problemas. A sobreposição é uma preocupação em toda a rede, seja na nuvem ou no local. Muitas organizações projetam um esquema de endereçamento IP em toda a empresa para evitar a sobreposição e planejar o crescimento futuro.

## Habilidades de trabalho

+ Tarefa 1: Criar uma rede virtual com sub-redes usando o portal.
+ Tarefa 2: Criar uma rede virtual e sub-redes usando um modelo.
+ Tarefa 3: Criar e configurar a comunicação entre um Grupo de Segurança de Aplicativos e um Grupo de Segurança de Rede.
+ Tarefa 4: Configurar zonas DNS públicas e privadas do Azure.
  
## Tarefa 1: Criar uma rede virtual com sub-redes usando o portal

A organização planeja uma grande quantidade de crescimento para os principais serviços. Nesta tarefa, você cria a rede virtual e as sub-redes associadas para acomodar os recursos existentes e o crescimento planejado. Nesta tarefa, você usará o portal do Azure. 

1. Entre no **portal do Azure** - `https://portal.azure.com`.
   
1. Pesquise e selecione `Virtual Networks`.

1. Na página Redes virtuais, selecione **Criar**.

1. Conclua a guia **Noções básicas** do CoreServicesVnet.  

    |  **Opção**         | **Valor**            |
    | ------------------ | -------------------- |
    | Grupo de recursos     | `az104-rg4` (se necessário, crie um novo) |
    | Nome               | `CoreServicesVnet`     |
    | Região             | (EUA) **Leste dos EUA**         |

1. Mova a guia **Endereços IP**.

    |  **Opção**         | **Valor**            |
    | ------------------ | -------------------- |
    | Espaço de endereço IPv4 | Substitua o espaço `10.20.0.0/16` de endereço IPv4 pré-preenchido por (separe as entradas)  |

1. Selecione **+Adicionar uma sub-rede**. Conclua as informações de nome e endereço de cada sub-rede. Certifique-se de selecionar **Adicionar** para cada nova sub-rede. Exclua a sub-rede padrão antes ou depois de criar as outras sub-redes.

    | **Sub-rede**             | **Opção**           | **Valor**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Nome da sub-rede          | `SharedServicesSubnet`   |
    |                        | Endereço inicial     | `10.20.10.0`          |
    |                        | Tamanho                 | `/24` |
    | DatabaseSubnet         | Nome da sub-rede          | `DatabaseSubnet`         |
    |                        | Endereço inicial     | `10.20.20.0`        |
    |                        | Tamanho                 | `/24` |

    >**Observação:** Cada rede virtual deve ter pelo menos uma sub-rede. Lembre-se de que cinco endereços IP sempre serão reservados, portanto, considere isso em seu planejamento. 

1. Para concluir a criação da CoreServicesVnet e das sub-redes associadas, escolha **Revisar + criar**.

1. Verifique se a configuração foi aprovada na validação e, depois, escolha **Criar**.

1. Aguarde a implantação da rede virtual e selecione **Ir para o recurso**.

1. Reserve um minuto para verificar o **Espaço de endereço** e as **Sub-redes**. Observe suas outras opções na folha **Configurações**. 

1. Na seção **Automação**, selecione **Exportar modelo** e aguarde até que o modelo seja gerado.

1. **Baixe** o modelo.

1. Navegue no computador local até a pasta **Downloads** e **Extrair todos** os arquivos no arquivo zip baixado. 

1. Antes de continuar, verifique se você tem o arquivo **template.json**. Você usará esse modelo para criar o ManufacturingVnet na próxima tarefa. 
 
## Tarefa 2: Criar uma rede virtual e sub-redes usando um modelo

Nesta tarefa, você cria a rede virtual ManufacturingVnet e as sub-redes associadas. A organização prevê o crescimento dos escritórios de produção para que as sub-redes sejam dimensionadas para o crescimento esperado. Para essa tarefa, você usa um modelo para criar os recursos. 

1. Localize o arquivo **template.json** exportado na tarefa anterior. Deve estar na sua pasta **Downloads**.

1. Edite o arquivo usando o editor de sua preferência. Muitos editores têm um recurso *alterar todas as ocorrências*. Se você estiver usando o Visual Studio Code, certifique-se de estar trabalhando em uma **janela confiável** e não no **modo restrito**. Consulte o diagrama de arquitetura para verificar os detalhes. 

### Faça alterações na rede virtual ManufacturingVnet

1. Substitua todas as ocorrências de **CoreServicesVnet** por `ManufacturingVnet`. 

1. Substituir todas as ocorrências de **10.20.0.0** por `10.30.0.0`. 

### Faça alterações nas sub-redes ManufacturingVnet

1. Altere todas as ocorrências de **SharedServicesSubnet** para `SensorSubnet1`.

1. Altere todas as ocorrências de **10.20.10.0/24** para `10.30.20.0/24`.

1. Altere todas as ocorrências de **DatabaseSubnet** para `SensorSubnet2`.

1. Altere todas as ocorrências de **10.20.20.0/24** para `10.30.21.0/24`.

1. Leia novamente o arquivo e certifique-se de que tudo esteja correto.

1. Não se esqueça de **Salvar** suas alterações.

>**Observação:** Há arquivos de modelo concluídos no diretório de arquivos do laboratório. 

### Faça alterações no arquivo de parâmetros

1. Localize o arquivo **parameters.json** exportado na tarefa anterior. Deve estar na sua pasta **Downloads**.

1. Edite o arquivo usando o editor de sua preferência.

1. Substitua a única ocorrência de **CoreServicesVnet** por `ManufacturingVnet`.

1. **Salve** suas alterações.
   
### Implantar o modelo personalizado

1. No portal, pesquise e selecione **Implantar um modelo personalizado**.

1. Selecione **Criar seu próprio modelo no editor** e, em seguida, **Carregar arquivo**.

1. Selecione o arquivo **templates.json** com suas alterações de fabricação e selecione **Salvar**.

1. Selecione **Examinar + criar** e depois **Criar**.

1. Aguarde a implantação do modelo e confirme (no portal) se a rede virtual e as sub-redes de fabricação foram criadas.

>**Observação:** Se você precisar implantar mais de uma vez, poderá encontrar alguns recursos concluídos com sucesso e que a implantação está falhando. Você pode remover manualmente esses recursos e tentar novamente. 
   
## Tarefa 3: Criar e configurar a comunicação entre um Grupo de Segurança de Aplicativos e um Grupo de Segurança de Rede

Nesta tarefa, criamos um Grupo de Segurança de Aplicativos e um Grupo de Segurança de Rede. O NSG terá uma regra de segurança de entrada que permite o tráfego do ASG. O NSG também terá uma regra de saída que nega acesso à internet. 

### Crie o Grupo de Segurança de Aplicativos (ASG)

1. No portal do Azure, pesquise e selecione `Application security groups`.

1. Clique em **Criar** e forneça as informações básicas.

    | Configuração | Valor |
    | -- | -- |
    | Subscription | *sua assinatura* |
    | Grupo de recursos | **az104-rg4** |
    | Nome | `asg-web` |
    | Region | **Leste dos EUA**  |

1. Clique em **Revisar + criar** e após a validação, clique em **Criar**.

### Crie o Grupo de Segurança de Rede e associe-o à sub-rede ASG

1. No portal do Azure, pesquise e selecione `Network security groups`.

1. Selecione **+ Criar** e forneça informações na guia **Noções básicas**. 

    | Configuração | Valor |
    | -- | -- |
    | Subscription | *sua assinatura* |
    | Grupo de recursos | **az104-rg4** |
    | Nome | `myNSGSecure` |
    | Region | **Leste dos EUA**  |

1. Clique em **Revisar + criar** e após a validação, clique em **Criar**.

1. Depois que o NSG for implantado, clique em **Ir para o recurso**.

1. Em **Configurações**, clique em **Sub-redes** e em **Associar**.

    | Configuração | Valor |
    | -- | -- |
    | Rede virtual | **CoreServicesVnet (az104-rg4)** |
    | Sub-rede | **SharedServicesSubnet** |

1. Clique em **OK** para salvar a associação.

### Configure uma regra de segurança de entrada para permitir o tráfego ASG

1. Continue trabalhando com o seu NSG. Na área **Configurações**, selecione **Regras de segurança de entrada**.

1. Revise as regras de entrada padrão. Observe que somente outras redes virtuais e balanceadores de carga têm acesso permitido.

1. Selecione **+ Adicionar**.

1. Na folha **Adicionar regra de segurança de entrada**, use as informações a seguir para adicionar uma regra de porta de entrada. Essa regra permite o tráfego ASG. Quando terminar, selecione **Adicionar**.

    | Configuração | Valor |
    | -- | -- |
    | Fonte | **Grupo de segurança do aplicativo** |
    | Grupos de segurança de aplicativos de origem | **asg-web** |
    | Intervalos de portas de origem |  * |
    | Destino | **Qualquer** |
    | Serviço | **Personalizado** (observe suas outras opções) |
    | Intervalos de portas de destino | **80,443** |
    | Protocolo | **TCP** |
    | Ação | **Permitir** |
    | Prioridade | **100** |
    | Nome | `AllowASG` |

### Configure uma regra do NSG de saída que negue o acesso à Internet

1. Depois de criar a sua regra do NSG de entrada, selecione **Regras de segurança de saída**. 

1. Observe a regra **AllowInternetOutboundRule**. Observe também que a regra não pode ser excluída e a prioridade é 65001.

1. Selecione **+ Adicionar** e configure uma regra de saída que negue acesso à Internet. Quando terminar, selecione **Adicionar**.

    | Configuração | Valor |
    | -- | -- |
    | Fonte | **Qualquer** |
    | Intervalos de portas de origem |  * |
    | Destino | **Marca do serviço** |
    | Marca de serviço de destino | **Internet** |
    | Serviço | **Personalizado** |
    | Intervalos de portas de destino | **8080** |
    | Protocolo | **Qualquer** |
    | Ação | **Deny** |
    | Prioridade | **4096** |
    | Nome | **DenyAnyCustom8080Outbound** |


## Tarefa 4: Configurar zonas DNS públicas e privadas do Azure

Nesta tarefa, você criará e configurará zonas DNS públicas e privadas. 

### Configurar uma zona DNS pública

Você pode configurar o DNS do Azure para resolver nomes de host em seu domínio público. Por exemplo, se você comprou o nome de domínio contoso.xyz de um registrador de nomes de domínio, poderá configurar o DNS do Azure para hospedar o domínio `contoso.com` e resolver www.contoso.xyz para o endereço IP do seu servidor Web ou aplicativo Web.

1. No portal, pesquise e selecione `DNS zones`.

1. Selecione **+ Criar**.

1. Configure a guia **Noções básicas**.

    | Propriedade | Valor    |
    |:---------|:---------|
    | Subscription | **Selecione sua assinatura** |
    | Grupo de recursos | **az-104-rg4** |
    | Nome | `contoso.com` (se reservado ajuste o nome) |
    | Region |**Leste dos EUA** (revise o ícone informativo) |

1. Selecione **Revisar + criar** e, em seguida, **Criar**.
   
1. Aguarde a implantação da zona DNS e selecione **Ir para o recurso**.

1. Na folha **Visão geral** observe os nomes dos quatro servidores de nomes DNS do Azure atribuídos à zona. **Copie** um dos endereços do servidor de nomes. Você precisará dela em uma etapa posterior. 
  
1. Selecione **+ Conjunto de registros**. Você adiciona um registro de link de rede virtual para cada rede virtual que precisa de suporte à resolução de nomes privados.

    | Propriedade | Valor    |
    |:---------|:---------|
    | Nome | **www** |
    | Tipo | **A** |
    | TTL | **1** |
    | Endereço IP | **10.1.1.4** |

>**Observação:**  Em um cenário real, você inseriria o endereço IP público do seu servidor Web.

1. Selecione **OK** e verifique se **contoso.com** tem um conjunto de registros A chamado **www**.

1. Abra um prompt de comando e execute o seguinte:

   ```sh
   nslookup www.contoso.com <name server name>
   ```
1. Verifique se o nome do host www.contoso.com é resolvido para o endereço IP fornecido. Isso confirma que a resolução de nomes está funcionando corretamente.

### Configurar uma zona DNS privada

Uma zona DNS privada fornece serviços de resolução de nomes em redes virtuais. Uma zona DNS privada só pode ser acessada pelas redes virtuais às quais está vinculada e não pode ser acessada pela Internet. 

1. No portal, pesquise e selecione `Private dns zones`.

1. Selecione **+ Criar**.

1. Na guia **Noções básicas** em Criar zona de DNS privada, insira as informações conforme listadas na tabela abaixo:

    | Propriedade | Valor    |
    |:---------|:---------|
    | Subscription | **Selecione sua assinatura** |
    | Grupo de recursos | **az-104-rg4** |
    | Nome | `private.contoso.com` (ajuste se você tiver que renomear) |
    | Region |**Leste dos EUA** |

1. Selecione **Revisar + criar** e, em seguida, **Criar**.
   
1. Aguarde a implantação da zona DNS e selecione **Ir para o recurso**.

1. Observe na folha **Visão geral** que não há registros de servidor de nomes. 

1. Selecione **Gerenciamento de DNS** e, em seguida, selecione **Links de rede virtual**. Configure o link. 

    | Propriedade | Valor    |
    |:---------|:---------|
    | Nome do link | `manufacturing-link` |
    | Rede virtual | `ManufacturingVnet` |

1. Selecione **Criar** e aguarde a criação do link. 

1. Na folha **Gerenciamento de DNS**, selecione **+ Conjuntos de registros**. Agora você adicionaria um registro para cada máquina virtual que precisa de suporte para resolução de nomes privados.

    | Propriedade | Valor    |
    |:---------|:---------|
    | Nome | **sensorvm** |
    | Tipo | **A** |
    | TTL | **1** |
    | Endereço IP | **10.1.1.4** |

 >**Observação:**  Em um cenário real, você inseriria o endereço IP de uma máquina virtual de fabricação específica.

## Limpar os recursos

Se você estiver trabalhando com **sua própria assinatura**, reserve um minuto para excluir os recursos do laboratório. Isso garantirá que os recursos sejam liberados e que o custo seja minimizado. A maneira mais fácil de excluir os recursos do laboratório é excluir o grupo de recursos do laboratório. 

+ No portal do Azure, selecione o grupo de recursos, selecione **Excluir o grupo de recursos**, **Inserir o nome do grupo de recursos** e clique em **Excluir**.
+ Usar o Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usar a CLI, `az group delete --name resourceGroupName`.

## Estender seu aprendizado com o Copilot

O Copilot pode ajudar você a aprender a usar as ferramentas de script do Azure. O Copilot também pode ajudar em áreas não cobertas no laboratório ou onde você precisar de mais informações. Abra um navegador do Edge e escolha Copilot (canto superior direito) ou navegue até *copilot.microsoft.com*. Reserve alguns minutos para experimentar essas solicitações.
+ Compartilhe as 10 melhores práticas ao implantar e configurar uma rede virtual no Azure.
+ Como usar comandos do Azure PowerShell e da CLI do Azure para criar uma rede virtual com um endereço IP público e uma sub-rede. 
+ Explique as regras de entrada e de saída do Grupo de Segurança de Rede do Azure e como elas são usadas.
+ Qual é a diferença entre os Grupos de Segurança de Rede do Azure e os Grupos de Segurança de Aplicativo Azure? Compartilhe exemplos de quando usar cada um desses grupos. 
+ Forneça um guia passo a passo sobre como solucionar problemas de rede que enfrentamos ao implantar uma rede no Azure. Compartilhe também o processo de raciocínio usado em cada etapa para solucionar problemas.

## Saiba mais com treinamento individual

+ [Introdução às Redes Virtuais do Azure](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/). Projete e implemente a infraestrutura principal da Rede do Azure, como redes virtuais, IPs públicos e privados, DNS, emparelhamento de rede virtual, roteamento e NAT Virtual do Azure.
+ [Criar um esquema de endereçamento IP](https://learn.microsoft.com/training/modules/design-ip-addressing-for-azure/). Identifique os recursos de endereçamento IP público e privado das redes virtuais locais e do Azure.
+ [Proteger e isolar o acesso aos recursos do Azure usando grupos de segurança de rede e pontos de extremidade de serviço](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/). Os Grupos de Segurança de Rede e os pontos de extremidade de serviço ajudam você a assegurar que as máquinas virtuais e os serviços do Azure sejam protegidos contra o acesso não autorizado à rede.
+ [Hospede seu domínio no DNS do Azure](https://learn.microsoft.com/training/modules/host-domain-azure-dns/). Crie uma zona DNS para seu nome de domínio. Crie registros DNS para mapear o domínio para um endereço IP. Teste se o nome de domínio é resolvido para o seu servidor Web.
  
## Principais aspectos a serem lembrados

Parabéns por concluir o laboratório. Aqui estão as principais lições deste laboratório. 

+ Uma rede virtual é uma representação da sua própria rede na nuvem. 
+ Ao projetar redes virtuais, é uma boa prática evitar a sobreposição de intervalos de endereços IP. Isso reduzirá os problemas e simplificará a solução de problemas.
+ Uma sub-rede é um intervalo de endereços IP na rede virtual. Você pode dividir a rede virtual em várias sub-redes para fins de organização e segurança.
+ Um grupo de segurança de rede contém regras de segurança que permitem ou negam o tráfego de rede. Há regras de entrada e saída padrão que você pode personalizar de acordo com suas necessidades.
+ Os grupos de segurança de aplicativos são usados para proteger grupos de servidores com uma função comum, como servidores Web ou servidores de banco de dados.
+ O DNS do Azure é um serviço de hospedagem para domínios DNS que fornece a resolução de nomes. Você pode configurar o DNS do Azure para resolver nomes de host em seu domínio público.  Você também pode usar zonas DNS privadas para atribuir nomes DNS a máquinas virtuais (VMs) em suas redes virtuais do Azure.
