---
lab:
  title: 'Laboratório: gerenciar a governança por meio do Azure Policy'
  module: Administer Governance and Compliance
---

# Laboratório: gerenciar a governança por meio do Azure Policy
# Manual do aluno

## Cenário do laboratório

Para melhorar o gerenciamento de recursos do Azure na Contoso, você foi encarregado de implementar a seguinte funcionalidade:

- marcação de grupos de recursos que incluem apenas recursos de infraestrutura (como contas de armazenamento do Cloud Shell)

- garantir que apenas recursos de infraestrutura devidamente marcados possam ser adicionados a grupos de recursos de infraestrutura

- Corrigir recursos sem conformidade

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%203)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: criar e atribuir marcas por meio do portal do Azure.
+ Tarefa 2: impor a marcação usando uma política do Azure.
+ Tarefa 3: Aplicar marcação por meio de uma política do Azure

## Tempo estimado: 30 minutos

## Diagrama de arquitetura

![imagem](../media/lab02b.png)

### Instruções

## Exercício 1

## Tarefa 1: criar e atribuir marcas por meio do portal do Azure.

Nesta tarefa, você criará e atribuirá uma marca a um grupo de recursos do Azure por meio do portal do Azure.

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

    >Se esta for a primeira vez que você está iniciando o Cloud Shell e você receber a mensagem Você não tem nenhum armazenamento montado, selecione a assinatura que você está usando no laboratório e selecione Criar armazenamento. 

1. No painel Cloud Shell, execute o seguinte para identificar o nome da conta de armazenamento usada pelo Cloud Shell:

   ```powershell
   df
   ```

1. Na saída do comando, observe a primeira parte do caminho totalmente qualificado que designa a montagem da unidade inicial do Cloud Shell (marcada aqui como `xxxxxxxxxxxxxx`:

   ```
   //xxxxxxxxxxxxxx.file.core.windows.net/cloudshell   (..)  /usr/csuser/clouddrive
   ```

1. No portal do Azure, pesquise e selecione **Contas** de armazenamento e, na lista das contas de armazenamento, clique na entrada que representa a conta de armazenamento identificada na etapa anterior.

1. Na folha Conta de armazenamento, clique no link que representa o nome do grupo de recursos que contém a conta de armazenamento.

    **Observação**: observe em qual grupo de recursos a conta de armazenamento está, você precisará dela mais tarde no laboratório.

1. Na folha do grupo de recursos, clique **em Marcas** no menu esquerdo e crie uma nova marca.

1. Crie uma tag com as seguintes configurações e Aplique sua alteração:

    | Configuração | Valor |
    | --- | --- |
    | Nome | **Função** |
    | Valor | Infra |

1. Clique em **Aplicar** e feche a janela de edição de tags para navegar de volta para a folha da conta de armazenamento. clique nas reticências na conta de armazenamento e selecione **Editar tags** para observar que a nova tag não foi atribuída automaticamente à conta de armazenamento. 

## Tarefa 2: impor a marcação usando uma política do Azure.

Nesta tarefa, você atribuirá a política interna Exigir uma marca e seu valor na política de recursos ao grupo de *recursos* e avaliará o resultado. 

1. No portal do Azure, pesquise e selecione **Política**. 

1. Na seção **Definições da compilação**, clique em **+ Novo**. Reserve um momento para navegar pela lista de definições de política internas que estão disponíveis para uso. Liste todas as políticas internas que envolvem o uso de tags selecionando a **entrada Tags** (e desmarcando todas as outras entradas) na **lista suspensa Categoria** . 

1. Localize a política interna **Exigir uma marca e o valor dela em recursos** e examine a definição.

1. **Na folha Exigir uma marca e seu valor em recursos** internos de definição de política, clique em **Atribuir**.

1. Especifique o **Escopo** clicando no botão de reticências e selecionando os seguintes valores:

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | o nome do grupo de recursos que contém a conta do Cloud Shell que você identificou na tarefa anterior |

    >Um escopo determina em quais recursos ou agrupamento de recursos a atribuição de política é imposta. Você pode atribuir essa política no nível de grupos de recursos, assinaturas ou grupos de gerenciamento. Você também tem a opção de especificar exclusões, como assinaturas individuais, grupos de recursos ou recursos (dependendo do escopo da atribuição). 

1. Configure as propriedades Basics** da atribuição especificando as **seguintes configurações (deixe outras pessoas com seus padrões):

    | Configuração | Valor |
    | --- | --- |
    | Nome da atribuição | **Exigir marca Role com valor Infra**|
    | Descrição | **Exigir marca de função com valor de infra para todos os recursos no grupo de recursos do Cloud Shell**|
    | Imposição de política | Habilitado |

    >O Nome da atribuição é automaticamente preenchido com o nome da política selecionada, mas você pode alterá-lo. Você também pode adicionar uma **Descrição**opcional. **Atribuído por** é preenchido automaticamente com base no nome de usuário que cria a atribuição. 

1. Clique em **Avançar** duas vezes e defina **Parâmetros** para os seguintes valores:

    | Configuração | Valor |
    | --- | --- |
    | Nome da Marca | **Função** |
    | Valor da marca | Infra |

1. Clique em **Avançar** e revise a guia Remediação **. Deixe a ****caixa de seleção Criar uma identidade** gerenciada desmarcada. 

    >**Nota**: Essa configuração pode ser usada quando a política ou iniciativa inclui o **efeito deployIfNotExists** ou **Modify** .

1. Clique em **Examinar + Criar** e em **Criar**.

    >**Observação**: agora você verificará se a nova atribuição de política está em vigor tentando criar outra conta de Armazenamento do Azure no grupo de recursos sem adicionar explicitamente a marca necessária. 
    
    >**Observação**: pode levar entre 5 e 15 minutos para que a política entre em vigor.

1. Navegue de volta para a folha do grupo de recursos que hospeda a conta de armazenamento usada para a unidade inicial do Cloud Shell, que você identificou na tarefa anterior.

1. Na folha do grupo de recursos, clique em + Criar e, em seguida, procure Conta de Armazenamento e clique em ****+ Criar****.**** 

1. **Na guia Noções Básicas** da folha Criar conta** de armazenamento, verifique se você está usando o Grupo de **Recursos ao qual a Política foi aplicada e especifique as seguintes configurações (deixe outras pessoas com seus padrões), clique em Revisar** e em ****Criar**:

    | Configuração | Valor |
    | --- | --- |
    | Nome da conta de armazenamento | Qualquer nome global exclusivo com 3 a 24 caracteres composto por dígitos e letras em minúsculas e começando com uma letra |

    >**Nota**: Você pode receber uma falha na **validação. Clique aqui para detalhes** do erro; Em caso afirmativo, clique na mensagem de erro para identificar o motivo da falha e ignore a próxima etapa. 

1. Depois de criar a implantação, você verá a **mensagem Falha** na implantação na **lista Notificações** do portal. Na **lista Notificações** , navegue até a visão geral da implantação e clique em Falha na **implantação. Clique aqui para obter a mensagem de detalhes** para identificar o motivo da falha. 

    >**Nota**: Verifique se a mensagem de erro indica que a implantação de recursos não foi permitida pela diretiva. 

    >**Nota**: Ao clicar na **guia Erro** bruto, você pode encontrar mais detalhes sobre o erro, incluindo o nome da marca de definição **de função Exigir função com valor** Infra. A implantação falhou porque a conta de armazenamento que você tentou criar não tinha uma marca chamada **Role** com seu valor definido como **Infra**.

## Tarefa 3: Aplicar marcação por meio de uma política do Azure

Nesta tarefa, usaremos uma definição de política diferente para corrigir quaisquer recursos não compatíveis. 

1. No portal do Azure, pesquise e selecione **Política**. 

1. Na seção **Criação**, selecione **Atribuições**. 

1. Na lista de atribuições, clique no ícone de reticências na linha que representa a **marca Exigir Função com atribuição de política de valor** Infra e use o **item de menu Excluir atribuição** para excluir a atribuição.

1. Clique em **Atribuir política** e especifique o **Escopo** clicando no botão de reticências e selecionando os seguintes valores:

    | Configuração | Valor |
    | --- | --- |
    | Subscription | Nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | o nome do grupo de recursos que contém a conta do Cloud Shell que você identificou na primeira tarefa |

1. Para especificar a **definição** de política, clique no botão de reticências e, em seguida, procure e selecione **Herdar uma marca do grupo de recursos, se estiver ausente**.

1. Configure as propriedades Básicas** restantes **da atribuição especificando as seguintes configurações (deixe outras pessoas com seus padrões):

    | Configuração | Valor |
    | --- | --- |
    | Nome da atribuição | **Herdar a marca Role e seu valor Infra do grupo de recursos do Cloud Shell se estiver ausente**|
    | Descrição | **Herdar a marca Role e seu valor Infra do grupo de recursos do Cloud Shell se estiver ausente**|
    | Imposição de política | Habilitado |

1. Clique em **Avançar** duas vezes e defina **Parâmetros** para os seguintes valores:

    | Configuração | Valor |
    | --- | --- |
    | Nome da Marca | **Função** |

1. Clique em **Avançar** e, na **guia Remediação** , defina as seguintes configurações (deixe outras pessoas com seus padrões):

    | Configuração | Valor |
    | --- | --- |
    | Criar uma tarefa de correção | Habilitado |
    | Política a ser corrigida | **Herdar uma marca do grupo de recursos, se ela estiver ausente** |

    >**Nota**: Esta definição de política inclui o **efeito Modificar** .

1. Clique em **Examinar + Criar** e em **Criar**.

    >**Observação**: para verificar se a nova atribuição de política está em vigor, você criará outra conta de Armazenamento do Azure no mesmo grupo de recursos sem adicionar explicitamente a marca necessária. 
    
    >**Observação**: pode levar entre 5 e 15 minutos para que a política entre em vigor.

1. Navegue de volta para a folha do grupo de recursos que hospeda a conta de armazenamento usada para a unidade inicial do Cloud Shell, que você identificou na primeira tarefa.

1. Na folha do grupo de recursos, clique em + Criar e, em seguida, procure Conta de Armazenamento e clique em ****+ Criar****.**** 

1. **Na guia Noções básicas** da folha Criar conta** de **armazenamento, verifique se você está usando o Grupo de Recursos ao qual a Política foi aplicada e especifique as seguintes configurações (deixe outras pessoas com seus padrões) e clique em **Revisar**:

    | Configuração | Valor |
    | --- | --- |
    | Nome da conta de armazenamento | Qualquer nome global exclusivo com 3 a 24 caracteres composto por dígitos e letras em minúsculas e começando com uma letra |

1. Verifique se desta vez a validação passou e clique em **Criar**.

1. Depois que a nova conta de armazenamento for provisionada, clique no **botão Ir para** recurso e, na **folha Visão geral** da conta de armazenamento recém-criada, observe que a marca **Role** com o valor **Infra** foi atribuída automaticamente ao recurso.

## Tarefa 4: limpar recursos

   >Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. A remoção de recursos não utilizados garante que você não verá cobranças inesperadas, embora tenha em mente que as políticas do Azure não incorrem em custo extra.
   
   >**Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, pesquise e selecione **Política**.

1. **Na seção Criação**, clique em Atribuições **, clique no ícone de reticências à direita da atribuição criada na tarefa anterior e clique **em **Excluir atribuição**. 

1. No portal do Azure, procure e selecione **Contas de armazenamento**.

1. Na lista de contas de armazenamento, selecione o grupo de recursos correspondente à conta de armazenamento criada na última tarefa deste laboratório. Selecione **Tags** e clique em **Excluir** (Lixeira à direita) para a **tag Role:Infra** e pressione **Aplicar**. 

1. Clique em Visão geral** e clique em ****Excluir** na parte superior da folha da conta de armazenamento. Quando for solicitada a confirmação, na **folha Excluir conta de armazenamento, digite o nome da conta** de armazenamento a ser confirmada e clique em **Excluir**. 

## Revisão

Neste laboratório, você vai:

- Tarefa 1: criar e atribuir marcas por meio do portal do Azure.
- Marcação imposta por meio de uma política do Azure
- Marcação aplicada por meio de uma política do Azure
