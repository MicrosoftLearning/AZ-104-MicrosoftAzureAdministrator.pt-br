---
lab:
  title: 'Laboratório 11: implementar o monitoramento'
  module: Administer Monitoring
---

# Laboratório 11 – Implementar o monitoramento

## Introdução ao laboratório

Neste laboratório, você aprenderá sobre o Azure Monitor. Você aprenderá a criar um alerta e enviá-lo para um grupo de ações. Você irá disparar e testar o alerta e verificar o log de atividades.  

Este laboratório requer uma assinatura do Azure. Seu tipo de assinatura pode afetar a disponibilidade de recursos neste laboratório. Você pode alterar a região, mas as etapas são escritas usando o **Leste dos EUA**.

## Tempo estimado: 40 minutos

## Cenário do laboratório

Sua organização migrou sua infraestrutura para o Azure. É importante que os administradores sejam notificados sobre quaisquer alterações significativas na infraestrutura. Você planeja examinar os recursos do Azure Monitor, incluindo o Log Analytics.

## Simulação interativa de laboratório

Há uma simulação interativa do laboratório que pode ser útil para este tópico. A simulação permite que você clique em um cenário semelhante em seu próprio ritmo. Há diferenças entre a simulação interativa e este laboratório, mas muitos dos principais conceitos são os mesmos. Não é necessária uma assinatura do Azure.

+ [Implementar o monitoramento.](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017) Crie um workspace do Log Analytics e soluções de automação do Azure. Examine as configurações de monitoramento e diagnóstico das máquinas virtuais. Examine a funcionalidade do Azure Monitor e do Log Analytics. 

## Diagrama de arquitetura

![Diagrama das tarefas de arquitetura](../media/az104-lab11-architecture.png)

## Habilidades de trabalho

+ Tarefa 1: Use um modelo para provisionar uma infraestrutura.
+ Tarefa 2: Criar um alerta.
+ Tarefa 3: Configure as notificações do grupo de ações.
+ Tarefa 4: Dispare um alerta e confirme se ele está funcionando.
+ Tarefa 5: Crie uma regra de processamento de alerta.
+ Tarefa 6: Use consultas de log do Azure Monitor.

## Tarefa 1: Usar um modelo para provisionar uma infraestrutura

Nesta tarefa, você implantará uma máquina virtual que será usada para testar cenários de monitoramento.

1. Baixe os arquivos de laboratório **\\Allfiles\\Lab11\\az104-11-vm-template.json** em seu computador.

1. Entre no **portal do Azure** - `https://portal.azure.com`.

1. No portal do Azure, pesquise e selecione `Deploy a custom template`.

1. Na página de implantação personalizada, selecione **Criar seu próprio modelo no editor**.

1. Na página Editar modelo, selecione **Carregar arquivo**.

1. Localize e selecione o arquivo **\\Allfiles\\Labs11\\az104-11-vm-template.json** e selecione **Abrir**.

1. Selecione **Salvar**.

1. Use as informações a seguir para preencher os campos de implantação customizados, deixando todos os outros campos com seus valores padrão:

    | Configuração       | Valor         | 
    | ---           | ---           |
    | Subscription  | Sua assinatura do Azure |
    | Resource group| `az104-rg11` (Se necessário, selecione **Criar novo**)
    | Region        | **Leste dos EUA**   |
    | Nome de usuário      | `localadmin`   |
    | Senha      | Forneça uma senha complexa |
    
1. Selecione **Examinar + Criar** e **Criar**.

1. Aguarde a conclusão da implantação e clique em **Ir para o grupo de recursos**.

1. Examine quais recursos foram implantados. Deve haver uma rede virtual com uma máquina virtual.

**Configurar o Azure Monitor para máquinas virtuais (isso será usado na última tarefa)**

1. No portal, pesquise e selecione **Monitor**.

1. Reserve um minuto para examinar todas as ferramentas de insights, detecção, triagem e diagnóstico disponíveis.

1. Selecione **Exibir** na caixa **Insights da VM** e selecione **Configurar Insights**.

1. Selecione sua máquina virtual e, em seguida, **Habilitar** (duas vezes).

1. Use os padrões para regras de assinatura e coleta de dados e selecione **Configurar**. 

1. Levará alguns minutos para que o agente de máquina virtual instale e configure. Prossiga para a próxima etapa. 
   
## Tarefa 2: Criar um alerta

Nesta tarefa, você criará um alerta para quando uma máquina virtual for excluída. 

1. Continue na página **Monitor** e selecione **Alertas**. 

1. Selecione **Criar +** e selecione **Regra de alerta**. 

1. Marque a caixa da assinatura e selecione **Aplicar**. Esse alerta será aplicado a todas as máquinas virtuais da assinatura. Como alternativa, você pode simplesmente especificar um computador específico. 

1. Selecione a guia **Condição** e, em seguida, selecione o link **Ver todos os sinais**.

1. Pesquise e selecione **Excluir Máquina Virtual (Máquinas Virtuais)**. Observe os outros sinais internos. Selecione **Aplicar**

1. Na área **Lógica de alerta** (role para baixo), examine as seleções de **Nível de evento**. Mantenha o padrão de **Todos selecionados**.

1. Examine as seleções de **Status**. Mantenha o padrão de **Todos selecionados**.

1. Deixe o painel **Criar uma regra de alerta** aberto para a próxima tarefa.

## Tarefa 3: Configurar notificações do grupo de ações

Nesta tarefa, se o alerta for disparado, envie uma notificação por email para a equipe de operações. 

1. Continue trabalhando em seu alerta. Selecione **Avançar: Ações** e, em seguida, selecione **Criar grupo de ações**.

    >**Você sabia?** Você pode adicionar até cinco grupos de ações a uma regra de alerta. Os grupos de ações são executados simultaneamente, sem ordem específica. Várias regras de alerta podem usar o mesmo grupo de ações. 

1. Na guia **Básico**, insira os valores a seguir para cada configuração.

    | Configuração | Valor |
    |---------|---------|
    | **Detalhes do projeto** |
    | Subscription | sua assinatura |
    | Grupo de recursos | **az104-rg11** |
    | Region | **Global** (padrão) |
    | **Detalhes da instância** |
    | Nome do grupo de ações | `Alert the operations team` (deve ser exclusivo no grupo de recursos) |
    | Nome de exibição | `AlertOpsTeam` |

1. Selecione **Avançar: Notificações** e insira os valores a seguir para cada configuração.

    | Configuração | Valor |
    |---------|---------|
    | Tipo de notificação | Selecione **Email/Mensagem SMS/Push/Voz** |
    | Nome | `VM was deleted` |

1. Selecione **Email** e, na caixa de **Email**, insira o endereço de email e selecione **OK**. 

    >**Observação:** Você deve receber uma notificação por email informando que foi adicionado a um grupo de ações. Pode haver alguns minutos de atraso, mas isso é um sinal certo de que a regra foi implantada.

1. Selecione **Examinar + Criar** e **Criar**.
   
1. Depois que o grupo de ações for criado, vá para a guia **Próximo: Detalhes** e insira os valores a seguir para cada configuração.

    | Configuração | Valor |
    |---------|---------|
    | Nome da regra de alerta | `VM was deleted` |
    | Descrição da regra de alerta | `A VM in your resource group was deleted` |

1. Selecione **Examinar + criar** para validar a entrada e escolha **Criar**.

## Tarefa 4: Disparar um alerta e confirmar se ele está funcionando

Nesta tarefa, você dispara o alerta e confirma se uma notificação é enviada. 

>**Observação:** Se você excluir a máquina virtual antes que a regra seja implantada, a regra de alerta poderá não ser disparada. 

1. No portal, pesquise e selecione **Máquinas virtuais**.

1. Marque a caixa da máquina virtual **az104-vm0**.

1. Selecione **Excluir** na barra de menus.

1. Marque a caixa para **Aplicar exclusão forçada**. Marque a caixa na parte inferior confirmando que quer que os recursos sejam excluídos e selecione **Excluir**. 

1. Na barra de título, selecione o ícone **Notificações** e aguarde até que a **vm0** seja excluída com êxito.

1. Você deve receber um email de notificação que diz, **Aviso importante: o alerta do Azure Monitor “a VM foi excluída” foi ativado...** Caso contrário, abra seu programa de email e procure por um email de azure-noreply@microsoft.com.

    ![Captura de tela do email de alerta.](../media/az104-lab11-alert-email.png)
   
1. No menu de recursos do portal do Microsoft Azure, selecione **Monitor** e, em seguida, **Alertas** no menu à esquerda.

1. Você verá três alertas detalhados que foram gerados pela exclusão da **vm0**.

   >**Observação:** Pode levar alguns minutos para que o email de alerta seja enviado e que os alertas sejam atualizados no portal. Se você não quiser esperar, continue para a próxima tarefa e retorne depois. 

1. Selecione o nome de um dos alertas (por exemplo, **VM excluída**). Um painel **Detalhes do alerta** é exibido, mostrando mais detalhes sobre o evento.

## Tarefa 5: Criar uma regra de processamento de alerta

Nesta tarefa, você criará uma regra de alerta para suprimir notificações durante um período de manutenção. 

1. Continue na folha **Alertas**, selecione **Regras de processamento de alerta** e, em seguida, **+ Criar**. 
   
1. Selecione a sua **Subscription** e, em seguida, **Aplicar**.
   
1. Selecione **Avançar: configurações de regras** e, em seguida, selecione **Suprimir notificações**.
   
1. Selecione **Avançar: Agendamento**.
   
1. Por padrão, a regra funciona o tempo todo, a menos que você a desabilite ou configure um agendamento. Você definirá uma regra para suprimir as notificações durante a manutenção noturna.
Insira estas configurações para o agendamento da regra de processamento de alerta:

    | Configuração | Valor |
    |---------|---------|
    | Aplicar a regra | Em um tempo específico |
    | Iniciar | Insira a data de hoje às 22h. |
    | Término | Insira a data de amanhã às 7h. |
    | Fuso horário | Selecione o fuso horário local. |

    ![Captura de tela da seção de agendamento de uma regra de processamento de alerta](../media/az104-lab11-alert-processing-rule-schedule.png)

1. Selecione **Avançar: Detalhes** e insira essas configurações:

    | Configuração | Valor |
    |---------|---------|
    | Grupo de recursos | **az104-rg11** |
    | Nome da regra | `Planned Maintenance` |
    | Descrição | `Suppress notifications during planned maintenance.` |

1. Selecione **Examinar + criar** para validar a entrada e escolha **Criar**.

## Tarefa 6: Consultas de log do Azure Monitor

Nesta tarefa, você usará o Azure Monitor para consultar os dados capturados da máquina virtual.

1. No portal do Azure, pesquise e selecione `Monitor` e clique em **Logs**.

1. Se necessário, feche a tela inicial. 

1. Se necessário, selecione um escopo, a sua **Assinatura**. Escolha **Aplicar**. 

1. Na guia **Consultas**, selecione **Máquinas virtuais** (painel esquerdo). Pode ser necessário reabrir a folha.

    ![Captura de tela da guia de consultas.](../media/az104-lab11-queries.png)

1. Examine as consultas disponíveis. **Execute** (passe o mouse sobre a consulta) a consulta **Contagem de pulsações**.

1. Você deve receber uma contagem de pulsação de quando a máquina virtual estava em execução.

1. Examine a consulta. Essa consulta usa a tabela de *pulsação*. 

1. Substitua a consulta por esta e clique em **Executar**. Examine o gráfico resultante. 

   ```
    InsightsMetrics
    | where TimeGenerated > ago(1h)
    | where Name == "UtilizationPercentage"
    | summarize avg(Val) by bin(TimeGenerated, 5m), Computer //split up by computer
    | render timechart
   ```

1. Como você tem tempo, examine e execute outras consultas. 

    >**Você sabia?**: Se você quiser praticar com outras consultas, existe um [Ambiente de Demonstração do Log Analytics](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-tutorial#open-log-analytics).
    
    >**Você sabia?**: Assim que encontrar uma consulta de que goste, você pode criar um alerta a partir dela. 

## Limpar os recursos

Se você estiver trabalhando com **sua própria assinatura**, reserve um minuto para excluir os recursos do laboratório. Isso garantirá que os recursos sejam liberados e que o custo seja minimizado. A maneira mais fácil de excluir os recursos do laboratório é excluir o grupo de recursos do laboratório. 

+ No portal do Azure, selecione o grupo de recursos, selecione **Excluir o grupo de recursos**, **Inserir o nome do grupo de recursos** e clique em **Excluir**.
+ Usar o Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Usar a CLI, `az group delete --name resourceGroupName`.

## Estender seu aprendizado com o Copilot
O Copilot pode ajudar você a aprender a usar as ferramentas de script do Azure. O Copilot também pode ajudar em áreas não cobertas no laboratório ou onde você precisar de mais informações. Abra um navegador do Edge e escolha Copilot (canto superior direito) ou navegue até *copilot.microsoft.com*. Reserve alguns minutos para experimentar essas solicitações.

+ Quais são as etapas básicas de configuração para ser alertado no Azure quando uma máquina virtual estiver inoperante?
+ Como posso ser notificado quando um alerta do Azure for disparado?
+ Construa uma consulta do Azure Monitor para fornecer informações de desempenho da CPU da máquina virtual.

## Saiba mais com treinamento individual

+ [Melhore a resposta a incidentes com alertas no Azure](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/). Responda a incidentes e atividades em sua infraestrutura por meio de funcionalidades de alerta no Azure Monitor.
+ [Monitore suas máquinas virtuais do Azure com o Azure Monitor](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/). Monitore suas VMs do Azure usando o Azure Monitor para coletar e analisar os logs e as métricas de cliente e do host da VM.

## Principais aspectos a serem lembrados

Parabéns por concluir o laboratório. Aqui estão as principais lições deste laboratório. 

+ Os alertas ajudam você a detectar e resolver problemas antes que os usuários percebam que pode haver um problema com sua infraestrutura ou aplicativo.
+ Você pode receber alertas sobre qualquer fonte de dados de log ou métrica na plataforma de dados do Azure Monitor.
+ Uma regra de alerta monitora seus dados e captura um sinal que indica que algo está acontecendo no recurso especificado.
+ Um alerta será disparado se as condições da regra de alerta forem atendidas. Várias ações (email, SMS, push, voz) podem ser disparadas.
+ Os grupos de ações incluem indivíduos que devem ser notificados de um alerta.
