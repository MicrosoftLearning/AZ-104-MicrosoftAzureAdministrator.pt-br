---
lab:
  title: 'Laboratório 07: Gerenciar o Armazenamento do Microsoft Azure'
  module: Administer Azure Storage
---

# Laboratório 07: Gerenciar o Armazenamento do Microsoft Azure

## Introdução ao laboratório

Neste laboratório, você aprenderá a criar contas de armazenamento para blobs e arquivos do Azure. Você aprenderá a configurar e proteger contêineres de blobs. Você também aprenderá a usar o Navegador de Armazenamento para configurar e proteger compartilhamentos de arquivos do Azure. 

Este laboratório requer uma assinatura do Azure. Seu tipo de assinatura pode afetar a disponibilidade de recursos neste laboratório. Você pode alterar a região, mas as etapas são escritas usando o **Leste dos EUA**.

## Tempo estimado: 50 minutos

## Cenário do laboratório

No momento, sua organização está armazenando dados em armazenamentos de dados locais. A maioria desses arquivos não é acessada com frequência. Você gostaria de minimizar o custo do armazenamento colocando os arquivos menos acessados em camadas de armazenamento com preços mais baixos. Além disso, explore diferentes mecanismos de proteção que o armazenamento do Azure oferece, incluindo acesso à rede, autenticação, autorização e replicação. Por fim, você deseja determinar até que ponto os Arquivos do Azure são adequados para hospedar seus compartilhamentos de arquivos locais.

## Simulações interativas do laboratório

Há simulações interativas do laboratório que podem ser úteis para este tópico. A simulação permite que você clique em um cenário semelhante em seu próprio ritmo. Há diferenças entre a simulação interativa e este laboratório, mas muitos dos principais conceitos são os mesmos. Uma assinatura do Azure não é necessária. 

+ [Criação do armazenamento de blobs](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%205). Crie uma conta de armazenamento, gerencie o armazenamento de blobs e monitore as atividades de armazenamento. 
  
+ [Gerenciar o Armazenamento do Microsoft Azure](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011). Crie uma conta de armazenamento e examine a configuração. Gerencia os contêineres do armazenamento de blobs. Configure a rede de armazenamento. 

## Diagrama de arquitetura

![Diagrama das tarefas.](../media/az104-lab07-architecture.png)

## Habilidades de trabalho

+ Tarefa 1: Criar e configurar uma conta de armazenamento. 
+ Tarefa 2: Criar e configurar o armazenamento de blobs seguro.
+ Tarefa 3: Criar e configurar o armazenamento de arquivos seguro do Azure.

## Tarefa 1: Criar e configurar uma conta de armazenamento. 

Nesta tarefa, você criará e configurará uma conta de armazenamento. A conta de armazenamento usará o armazenamento com redundância geográfica e não terá acesso público. 

1. Entre no **portal do Azure** - `https://portal.azure.com`.

1. Pesquise e selecione `Storage accounts`, e clique em **+ Criar**.

1. Na guia **Noções básicas** da folha **Criar uma conta de armazenamento**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Assinatura          | o nome da sua assinatura do Azure  |
    | Grupo de recursos        | **az104-rg7** (criar novo) |
    | Nome da conta de armazenamento  | qualquer nome globalmente exclusivo de 3 a 24 caracteres composto por letras e dígitos |
    | Região                | **(EUA) Leste dos EUA**  |
    | Desempenho           | **Standard** (observe a opção Premium) |
    | Redundância            | **Armazenamento com redundância geográfica** (observe as outras opções)|
    | Permitir o acesso de leitura aos dados em caso de disponibilidade regional | Marque a caixa |

>**Você sabia?** Você deve usar o nível de desempenho Standard para a maioria dos aplicativos. Use o nível de desempenho Premium para aplicativos empresariais ou de alto desempenho. 

1. Na guia **Avançado**, use os ícones informativos para saber mais sobre as opções. Aceite os padrões. 

1. Na guia **Rede**, revise as opções disponíveis, selecione **Desabilitar acesso público e usar acesso privado**.

1. Examine a guia **Proteção de dados**. Observe que 7 dias é a política de retenção de exclusão temporária padrão. Observe que você pode habilitar o controle de versão dos blobs. Aceite os padrões.

1. Examine a guia **Criptografia**. Observe as opções de segurança adicionais. Aceite os padrões.

1. Selecione **Examinar**, aguarde a conclusão do processo de validação e clique em **Criar**.

1. Depois que a conta de armazenamento for implantada, selecione **Ir para o recurso**.

1. Examine a folha **Visão geral** e as configurações adicionais que podem ser alteradas. Essas são as configurações globais da conta de armazenamento. Observe que a conta de armazenamento pode ser usada para contêineres de blobs, compartilhamentos de arquivos, filas e tabelas.

1. Na seção **Segurança + rede**, selecione **Rede**. Observe que o acesso à rede pública está desabilitado.

    + Altere o **nível de acesso público** para **Habilitado de redes virtuais e endereços IP selecionados**.
    + Na seção **Firewall**, marque a caixa para **Adicionar o endereço IP do cliente.**
    + Não se esqueça de **Salvar** suas alterações. 
  
1. Na seção **Gerenciamento de dados**, exiba a folha **Redundância**. Observe as informações sobre seus locais de data center primário e secundário.

1. Na seção **Gerenciamento de dados**, selecione **Gerenciamento do ciclo de vida** e, em seguida, selecione **Adicionar uma regra**.

    + **Nomeie** a regra `Movetocool`. Observe suas opções para limitar o escopo da regra.
    
    + Na guia **Blobs de base**, *se* os blobs baseados tiverem sido modificados pela última vez há mais de `30 days`, *então*, **vá para o armazenamento esporádico**. Observe suas outras opções. 
    
    + Observe que você pode configurar outras condições. Selecione **Adicionar** quando terminar de explorar.

    ![Captura de tela da opção Vá para as condições de regra esporádica.](../media/az104-lab07-movetocool.png)

## Tarefa 2: Criar e configurar o armazenamento de blobs seguro

Nesta tarefa, você criará um contêiner de blob e carregará uma imagem. Os contêineres de blobs são estruturas semelhantes a diretórios que armazenam dados não estruturados.

### Criar um contêiner de blobs e uma política de retenção baseada em tempo

1. Continue no portal do Azure, trabalhando com sua conta de armazenamento.

1. Na seção **Armazenamento de dados**, clique em **Contêineres**. 

1. Clique em **+ Contêiner** e **Criar** um contêiner com as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Nome | `data`  |
    | Nível de acesso público | Observe que o nível de acesso está definido como privado |

    ![Captura de tela da criação de um contêiner.](../media/az104-lab07-create-container.png)

1. No contêiner, role até as reticências (...) na extrema direita, selecione **Política de Acesso**.

1. Na área **Armazenamento de blobs imutável**, selecione **Adicionar política**.

    | Configuração | Valor |
    | --- | --- |
    | Tipo de política | **Retenção baseada em tempo**  |
    | Definir o período de retenção para | `180` dias |

1. Selecione **Salvar**.

### Gerenciar uploads de blobs

1. Retorne à página de contêineres, selecione o contêiner de **dados** e clique em **Carregar**.

1. Na folha **Carregar blob**, expanda a seção **Avançado**.

    >**Observação**: Localize um arquivo para carregar. Pode ser qualquer tipo de arquivo, mas é melhor um arquivo pequeno. Um arquivo de exemplo pode ser baixado do diretório AllFiles. 

    | Configuração | Valor |
    | --- | --- |
    | Procurar arquivos | adicione o arquivo que você selecionou para carregar |
    | Selecione **Avançado** | |
    | Tipo de blob | **Blob de blocos** |
    | Tamanho do bloco | **4 MiB** |
    | Camada de acesso | **Camada de armazenamento frequente**  (observe as outras opções) |
    | Carregar na pasta | `securitytest` |
    | Escopo de criptografia | Usar o escopo de contêiner padrão existente |

1. Clique em **Carregar**.

1. Confirme se você tem uma nova pasta e se o arquivo foi carregado. 

1. Selecione o arquivo de upload e examine as opções, incluindo **Baixar**, **Excluir**, **Alterar nível** e **Adquirir concessão**.

1. Copie o **URL** do arquivo e cole em uma nova janela de navegação **Inprivate**.

1. Você deve receber uma mensagem formatada em XML informando **ResourceNotFound** ou **PublicAccessNotPermitted**.

    > **Observação**: Isso é esperado, já que o contêiner criado tem o nível de acesso público definido como **Privado (sem acesso anônimo)**.

### Configurar o acesso limitado ao armazenamento de blobs

1. Selecione o arquivo carregado e a guia **Gerar SAS**. Você também pode usar as reticências (...) da extrema direita. Especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configuração | Valor |
    | --- | --- |
    | Chave de assinatura | **Chave 1** |
    | Permissões | **Leitura** (observe suas outras opções) |
    | Data inicial | data de ontem |
    | Hora de início | hora atual |
    | Data de vencimento | data de amanhã |
    | Hora de expiração | hora atual |
    | Endereços IP permitidos | deixar em branco |

1. Clique em **Gerar token SAS e URL**.

1. Copie a entrada **URL de SAS do Blob** para a área de transferência.

1. Abra outra janela do navegador InPrivate e navegue até a entrada URL de SAS do Blob copiada na etapa anterior.

    >**Observação**: Você deve poder exibir o conteúdo do arquivo. 

## Tarefa 3: Criar e configurar um armazenamento de arquivos do Azure

Nesta tarefa, você criará e configurará compartilhamentos de arquivos do Azure. Você usará o Navegador de Armazenamento para gerenciar o compartilhamento de arquivo. 

### Criar o compartilhamento de arquivo e carregar um arquivo

1. No portal do Azure, navegue de volta para sua conta de armazenamento, na seção **Armazenamento de dados**, clique em **Compartilhamentos de arquivos**.

1. Clique em **+ Compartilhamento de arquivo** e, na guia **Noções básicas**, dê um nome ao compartilhamento de arquivo, `share1`. 

1. Observe as opções de **Camada de acesso**. Mantenha a **Transação otimizada** padrão.
   
1. Vá para a guia **Backup** e certifique-se de que **Habilitar backup** esteja **e não** marcado. Estamos desabilitando o backup para simplificar a configuração do laboratório.

1. Clique em **Revisar + criar** e, em seguida, **Criar**. Aguarde a implantação do compartilhamento de arquivos.

    ![Captura de tela da página Criar compartilhamento de arquivo.](../media/az104-lab07-create-share.png)

### Explorar o Navegador de Armazenamento e carregar um arquivo

1. Volte para sua conta de armazenamento e selecione **Navegador de armazenamento**. O Navegador de Armazenamento do Azure é uma ferramenta de portal que permite exibir rapidamente todos os serviços de armazenamento em sua conta.

1. Selecione **Compartilhamentos de arquivos** e verifique se o diretório **share1** está presente.

1. Selecione seu diretório **share1** e observe que você pode **+ Adicionar diretório**. Isso permite que você crie uma estrutura de pastas.

1. Escolha **Carregar**. Navegue até um arquivo de sua escolha e clique em **Carregar**.

    >**Observação**: Você pode exibir os compartilhamentos de arquivos e gerenciar esses compartilhamentos no Navegador de Armazenamento. No momento, não há restrições.

### Restrinja o acesso da rede à conta de armazenamento

1. No portal do Azure, pesquise e selecione **Redes virtuais**.

1. Selecione **+ Criar**. Selecione o grupo de recursos. e dê um **nome** à rede virtual, `vnet1`.

1. Assuma os padrões para outros parâmetros, selecione **Revisar + criar** e, em seguida, **Criar**.

1. Aguarde a implantação da rede virtual e selecione **Ir para o recurso**.

1. Na seção **Configurações**, selecione a folha **Pontos de extremidade de serviço**.
    + Selecione **Adicionar**. 
    + Na lista suspensa **Serviços**, selecione **Microsoft.Storage**.
    + Na lista suspensa **Sub-redes**, marque a sub-rede **Padrão**.
    + Clique em **Adicionar** para salvar suas alterações.  

1. Retorne à sua conta de armazenamento.

1. Na seção **Segurança + rede**, selecione a folha **Rede**.

1. Selecione **Adicionar rede virtual existente** e selecione **vnet1** e sub-rede **padrão** e, em seguida, selecione **Adicionar**.

1. Na seção **Firewall**, **Exclua** o endereço IP da sua máquina. O tráfego permitido só deve vir da rede virtual. 

1. Não se esqueça de **Salvar** suas alterações.

    >**Observação:** A conta de armazenamento agora só deve ser acessada da rede virtual que você acabou de criar. 

1. Selecione o **Navegador de armazenamento** e **Atualize** a página. Navegue até o compartilhamento de arquivo ou o conteúdo do blob.  

    >**Observação:** Você deve receber uma mensagem *não autorizado a executar esta operação*. Você não está se conectando da rede virtual. Pode levar alguns minutos para que isso entre em vigor.


![Captura de tela do acesso não autorizado.](../media/az104-lab07-notauthorized.png)

## Limpar os recursos

Se você estiver trabalhando com **sua própria assinatura**, reserve um minuto para excluir os recursos do laboratório. Isso garantirá que os recursos sejam liberados e que o custo seja minimizado. A maneira mais fácil de excluir os recursos do laboratório é excluir o grupo de recursos do laboratório. 

+ No portal do Azure, selecione o grupo de recursos, selecione **Excluir o grupo de recursos**, **Inserir o nome do grupo de recursos** e clique em **Excluir**.
+ Usar o Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usar a CLI, `az group delete --name resourceGroupName`.

## Estender seu aprendizado com o Copilot
O Copilot pode ajudar você a aprender a usar as ferramentas de script do Azure. O Copilot também pode ajudar em áreas não cobertas no laboratório ou onde você precisar de mais informações. Abra um navegador do Edge e escolha Copilot (canto superior direito) ou navegue até *copilot.microsoft.com*. Reserve alguns minutos para experimentar essas solicitações.

+ Forneça um script do Azure PowerShell para criar uma conta de armazenamento com um contêiner de blob. 
+ Forneça uma lista de verificação que eu possa usar para garantir que minha conta de armazenamento do Azure esteja segura.
+ Crie uma tabela para comparar modelos de redundância de armazenamento do Azure.

## Saiba mais com treinamento individual

+ [Otimize seus custos com o Armazenamento de Blobs do Azure](https://learn.microsoft.com/training/modules/optimize-your-cost-azure-blob-storage/). Saiba como otimizar seu custo com o Armazenamento de Blobs do Azure.
+ [Controlar o acesso ao Armazenamento do Microsoft Azure com assinaturas de acesso compartilhado](https://learn.microsoft.com/training/modules/control-access-to-azure-storage-with-sas/). Permita acesso a dados armazenados em suas contas de armazenamento do Azure com segurança usando assinaturas de acesso compartilhado.

## Principais aspectos a serem lembrados

Parabéns por concluir o laboratório. Aqui estão as principais lições deste laboratório. 

+ Uma conta de armazenamento do Azure contém todos os seus objetos de dados do Armazenamento do Azure: blobs, arquivos, filas e tabelas. Uma conta de armazenamento fornece um namespace exclusivo para os dados do Armazenamento do Microsoft Azure que podem ser acessados de qualquer lugar do mundo por HTTP ou HTTPS.
+ O armazenamento do Azure fornece vários modelos de redundância, incluindo LRS (Armazenamento com redundância local), ZRS (Armazenamento com redundância de zona) e GRS (Armazenamento com redundância geográfica). 
+ O armazenamento de blobs do Azure permite armazenar grandes quantidades de dados não estruturados na plataforma de armazenamento de dados da Microsoft. Blob significa objeto binário grande, que inclui objetos como arquivos de imagens e multimídia.
+ O armazenamento de arquivos do Azure fornece armazenamento compartilhado para dados estruturados. Os dados podem ser organizados em pastas.
+ O armazenamento imutável fornece a capacidade de armazenar dados em um estado WORM (gravação única, leitura múltipla). As políticas de armazenamento imutáveis podem ser baseadas em tempo ou em retenção legal.
