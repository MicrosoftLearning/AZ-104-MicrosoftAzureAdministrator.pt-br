---
lab:
  title: 'Laboratório: gerenciar recursos do Azure com o portal do Azure'
  module: Administer Azure Resources
---

# Laboratório: gerenciar recursos do Azure com o portal do Azure
# Manual do aluno

## Cenário do laboratório

Você precisa explorar os recursos básicos de administração do Azure associados ao provisionamento de recursos e organizá-los com base em grupos de recursos, incluindo a movimentação de recursos entre grupos de recursos. Você também deseja explorar opções para proteger os recursos de disco de serem excluídos acidentalmente, enquanto ainda permite modificar suas características de desempenho e tamanho.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%204)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: Criar grupos de recursos e implantar recursos em grupos de recursos
+ Mover recursos entre grupos de recursos
+ Tarefa 3: Implementar e testar bloqueios de recursos

## Tempo estimado: 20 minutos

## Diagrama de arquitetura

![imagem](../media/lab03a.png)

### Instruções

## Exercício 1

## Tarefa 1: Criar grupos de recursos e implantar recursos em grupos de recursos

Nesta tarefa, você usará o portal do Azure para criar grupos de recursos e criar um disco no grupo de recursos.

1. Entre no [**portal do Azure**](http://portal.azure.com).

1. No portal do Azure, procure e selecione **Discos**, clique em **+ Criar** e especifique as seguintes configurações:

    |Configuração|Valor|
    |---|---|
    |Subscription| Defina a assinatura, a região e o nome do grupo de recursos do Azure. |
    |Grupo de recursos| O nome de um novo grupo de recursos **aks-01-RG** |
    |Nome do disco| az104-03a-disk1 |
    |Região| **(EUA) Leste dos EUA** |
    |Zona de disponibilidade| **Sem necessidade de redundância de infraestrutura** |
    |Tipo de origem| **Nenhuma** |

    >Quando você cria uma conta de armazenamento, tem a opção de criar um grupo de recursos ou usar um existente.

1. Altere o tipo e o tamanho do disco para **HDD** padrão e **32 GiB**, respectivamente.

1. Clique em **Examinar + Criar** e em **Criar**.

    >**Nota**: Aguarde até que o disco seja criado. Isso deve levar menos de um minuto.

## Mover recursos entre grupos de recursos 

Nesta tarefa, moveremos o recurso de disco criado na tarefa anterior para um novo grupo de recursos. 

1. Pesquise e selecione **Grupos de recursos**. 

1. **Na folha Grupos de recursos**, clique na entrada que representa o **grupo de recursos az104-03a-rg1** criado na tarefa anterior.

1. **Na folha Visão geral** do grupo de recursos, na lista de recursos do grupo de recursos, selecione a entrada que representa o disco recém-criado, clique **em Mover na barra de ferramentas e, na lista suspensa, selecione **Mover** para outro grupo** de recursos.

    >**Nota**: Este método permite mover vários recursos ao mesmo tempo. 

1. Abaixo da **caixa de texto Grupo de recursos, clique em **Criar novo** e digite **az104-03a-rg2** na caixa de** texto. Selecione a opção **Eu entendo que ferramentas e scripts associados aos recursos movidos não funcionarão até que eu os atualize para usar as novas IDs do recurso** para confirmar e, em seguida, selecione **Mover**.

    >**Nota**: Não espere a conclusão da movimentação, mas prossiga para a próxima tarefa. A movimentação pode levar alguns minutos. Você pode determinar se a operação foi concluída monitorando as entradas do log de atividades do grupo de recursos de origem ou de destino. Revisite esta etapa depois de concluir a próxima tarefa.

## Tarefa 3: Implementar bloqueios de recursos

Nesta tarefa, você aplicará um bloqueio de recurso a um grupo de recursos do Azure que contém um recurso de disco.

1. No portal do Azure, procure e selecione **Discos**, clique em **+ Criar** e especifique as seguintes configurações:

    |Configuração|Valor|
    |---|---|
    |Subscription| Nome da assinatura que você está usando neste laboratório |
    |Grupo de recursos| Clique em **Criar Novo** Grupo de Recursos e nomeie-o AZ104-03A-RG3 **** |
    |Nome do disco| az104-03a-disk1 |
    |Region| o nome da região do Azure onde você criou os outros grupos de recursos neste laboratório |
    |Zona de disponibilidade| **Sem necessidade de redundância de infraestrutura** |
    |Tipo de origem| **Nenhuma** |

1. Defina o tipo e o tamanho do disco como **HDD** padrão e **32 GiB**, respectivamente.

1. Clique em **Examinar + Criar** e em **Criar**.

1. Clique em **Ir para o recurso**.

1. Na página Visão geral do disco, clique no nome do grupo de recursos, **az104-03a-rg3**.

1. Na folha do **grupo de recursos az104-03a-rg3**, clique em Bloqueios** e depois em **+ Adicionar** e **especifique as seguintes configurações:

    |Configuração|Valor|
    |---|---|
    |Nome do bloqueio| **az104-03a-delete-lock** |
    |Tipo de bloqueio| **Delete (excluir)** |
    
1. Clique em **OK**    

1. Na folha do **grupo de recursos az104-03a-rg3**, clique em Visão geral **, na lista de recursos do grupo de recursos, selecione a entrada que representa o disco criado anteriormente nesta tarefa e clique **em **Excluir** na barra de ferramentas. 

1. Quando solicitado **Deseja excluir todos os recursos selecionados?**, na caixa de **texto Confirmar exclusão** , digite **sim** e clique em **Excluir**.

1. Você verá uma mensagem de erro, notificando sobre a operação de exclusão com falha. 

    >**Nota**: Como a mensagem de erro indica, isso é esperado devido ao bloqueio de exclusão aplicado no nível do grupo de recursos.

1. Navegue de volta para a lista de recursos do **grupo de recursos az104-03a-rg3** e clique na entrada que representa o **recurso az104-03a-disk2** . 

1. **Na folha az104-03a-disk2**, na **seção Configurações**, clique em Tamanho + desempenho **, defina o tipo e o tamanho do disco como **SSD** Premium e 64 GiB **, respectivamente, e **clique **em **Salvar** para aplicar a alteração. Verifique se o pull ocorreu com êxito:

    >**Nota**: Isso é esperado, já que o bloqueio no nível do grupo de recursos se aplica apenas a operações de exclusão. 

## Limpar os recursos

   >**Observação**: não exclua os recursos implantados neste laboratório. Você os usará no próximo laboratório deste módulo. Remova apenas o bloqueio de recursos criado neste laboratório.

1. Navegue até a folha do **grupo de recursos az104-03a-rg3**, exiba sua **folha Bloqueios** e remova o bloqueio az104-03a-delete-lock** **clicando **no link Excluir no lado direito da entrada Excluir**** bloqueio**.

## Revisão

Neste laboratório, você vai:

- Criou grupos de recursos e implantou recursos em grupos de recursos
- Mover recursos entre grupos de recursos
- Bloqueios de recursos implementados e testados
