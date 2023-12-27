---
lab:
  title: 'Laboratório 11: implementar o monitoramento'
  module: Administer Monitoring
---

# Laboratório 11 – Implementar o monitoramento
# Manual do laboratório do aluno

## Cenário do laboratório

Você precisa avaliar a funcionalidade do Azure que fornece insights sobre o desempenho e a configuração dos recursos do Azure, concentrando-se em particular nas máquinas virtuais do Azure. Para fazer isso, você deve examinar os recursos do Azure Monitor, incluindo o Log Analytics.

**Observação:** uma **[simulação de laboratório interativa](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017)** está disponível que permite que você navegue neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos. 

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: provisionar o ambiente de laboratório
+ Tarefa 2: registrar os provedores de recursos Microsoft.Insights e Microsoft.AlertsManagement
+ Tarefa 3: criar e configurar um workspace do Azure Log Analytics e soluções baseadas em Automação do Azure
+ Tarefa 4: examinar as configurações de monitoramento padrão das máquinas virtuais do Azure
+ Tarefa 5: definir as configurações de diagnóstico da máquina virtual do Azure
+ Tarefa 6: examinar a funcionalidade do Azure Monitor.
+ Tarefa 7: examinar a funcionalidade do Azure Log Analytics

## Tempo estimado: 45 minutos

## Diagrama de arquitetura

![imagem](../media/lab11.png)

### Instruções

## Exercício 1

## Tarefa 1: provisionar o ambiente de laboratório

Nesta tarefa, você implantará uma máquina virtual que será usada para testar cenários de monitoramento.

1. Entre no [portal do Azure](https://portal.azure.com).

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **Bash**.

    >**Observação**: se esta for a primeira vez que você está iniciando o **Cloud Shell** e você receber a mensagem **Você não tem nenhum armazenamento montado**, selecione a assinatura que você está usando no laboratório e selecione **Criar armazenamento**.

1. Na barra de ferramentas do painel do Cloud Shell, clique no ícone **Carregar/Baixar arquivos**, no menu suspenso, clique em **Carregar** e carregue os arquivos **\\Allfiles\\Labs\\11\\az104-11-vm-template.json** e **\\Allfiles\\Labs\\11\\az104-11-vm-parameters.json** no diretório base do Cloud Shell.

1. No painel do Cloud Shell, execute o seguinte para criar o grupo de recursos que hospedará as máquinas virtuais (substitua o espaço reservado `[Azure_region]` pelo nome de uma região do Azure onde você pretende implantar máquinas virtuais do Azure):

    >**Observação**: certifique-se de escolher uma das regiões listadas como **Região do Workspace do Log Analytics** nas referências da [Documentação de mapeamentos do workspace](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)

   ```powershell
   $location = '[Azure_region]'

   $rgName = 'az104-11-rg0'

   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. No painel Cloud Shell, execute o seguinte para criar a primeira rede virtual e implantar uma máquina virtual nela usando os arquivos de modelo e parâmetro carregados:

    >**Observação**: você será solicitado a fornecer uma senha.
    
   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-11-vm-template.json `
      -TemplateParameterFile $HOME/az104-11-vm-parameters.json `
      -AsJob
   ```

    >**Observação:** prossiga para a próxima etapa sem aguardar a conclusão da implantação. A implantação deve levar cerca de 3 minutos.

## Tarefa 2: registrar os provedores de recursos Microsoft.Insights e Microsoft.AlertsManagement.

1. No painel do Cloud Shell, execute o seguinte para registrar os provedores de recursos Microsoft.Insights e Microsoft.AlertsManagement.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

1. Minimize o painel Cloud Shell (mas não o feche).

## Tarefa 3: criar e configurar um workspace do Azure Log Analytics e soluções baseadas em Automação do Azure

Nesta tarefa, você criará e configurará um workspace do Azure Log Analytics e soluções baseadas em Automação do Azure

1. No portal do Azure, pesquise e selecione **Workspaces do Log Analytics** e, na folha **Workspaces do Log Analytics**, clique em **+ Criar**.

1. Na guia **Básico** da folha **Criar Workspace do Log Analytics**, insira as configurações a seguir e clique em **Revisar + Criar** e em **Criar**:

    | Configurações | Valor |
    | --- | --- |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | o nome de um novo grupo de recursos **az104-11-rg1** |
    | Workspace do Log Analytics | qualquer nome exclusivo |
    | Region | o nome da região do Azure na qual você implantou a máquina virtual na tarefa anterior |

    >**Observação**: certifique-se de especificar a mesma região na qual você implantou máquinas virtuais na tarefa anterior.

    >**Observação**: aguarde até que a implantação seja concluída. A implantação deve levar menos de um minuto.

1. No portal do Azure, procure e selecione **Contas de automação** e, no painel **Contas de automação**, clique em **+ Criar**.

1. No painel **Criar uma conta de automação** especifique as seguintes configurações e clique em **Revisar + Criar** após validação, clique em **Criar**:

    | Configurações | Valor |
    | --- | --- |
    | Nome da conta da Automação | qualquer nome exclusivo |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | **az104-11-rg1** |
    | Region | o nome da região do Azure determinada com base na [documentação de mapeamentos de espaço de trabalho](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings) |

    >**Observação**: certifique-se de especificar a região do Azure com base na [documentação de mapeamentos do Espaço de Trabalho](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)

    >**Observação**: aguarde até que a implantação seja concluída. A implantação pode levar até 3 minutos.

1. Clique em **Ir para o recurso**.

1. No painel Conta de automação, na seção **Gerenciamento de configuração** clique em **Inventário**.

1. No painel **Inventário** na lista suspensa do **Workspace do Log Analytics** selecione o workspace do Log Analytics que você criou mais cedo nessa tarefa e clique em **Habilitar**.

    >**Observação**: aguarde a conclusão da instalação da solução Log Analytics correspondente. Isso pode levar cerca de 3 minutos.

    >**Observação**: isso instala automaticamente a solução de **controle de alterações** também.

1. No painel Conta de automação, na seção **Gerenciamento de Atualizações** clique em **Gerenciamento de atualizações** e clique em **Habilitar**.

    >**Observação**: aguarde a conclusão da instalação. Isso pode levar cerca de cinco minutos.

## Tarefa 4: examinar as configurações de monitoramento padrão das máquinas virtuais do Azure

Tarefa 4: revisar as configurações de monitoramento padrão das máquinas virtuais do Azure

1. No portal do Azure, procure e selecione **Máquinas virtuais** e, no painel **Máquinas virtuais**, clique em **az104-11-vm0**.

1. No painel **az104-11-vm0**, na seção **Monitoramento**, clique em **Métricas**.

1. No painel **Métricas do az104-11-vm0\|**, no gráfico padrão, observe que o único **Namespace de métricas** disponível é **Host de Máquina Virtual**.

    >**Observação**: isso é esperado, já que nenhuma configuração de diagnóstico em nível de convidado foi definida ainda. No entanto, você tem a opção de habilitar métricas de memória de convidado diretamente da lista suspensa **Namespace de métricas**. Você precisará dela posteriormente no exercício.

1. Na lista suspensa **Métrica**, revise a lista de métricas disponíveis.

    >**Observação**: a lista inclui uma variedade de métricas relacionadas à CPU, disco e rede que podem ser coletadas do host da máquina virtual, sem ter acesso a métricas de nível de convidado.

1. Na lista suspensa **Métrica**, selecione **Porcentagem de CPU** na lista suspensa **Agregação**, selecione **Média** e revise o gráfico resultante.

## Tarefa 5: definir as configurações de diagnóstico da máquina virtual do Azure

Tarefa 5: definir as configurações de diagnóstico da máquina virtual do Azure.

1. No painel **az104-11-vm0** na seção **Monitoramento**, clique em **Configurações de diagnóstico**.

1. Na guia **Visão geral** do painel **Configurações de diagnóstico** az104-11-vm0\|, selecione uma **conta de armazenamento de diagnóstico** e clique em **Habilitar monitoramento** no nível de convidado.

    >**Observação**: aguarde até que a extensão de configurações de diagnóstico seja instalada. Isso pode levar cerca de 3 minutos.

1. Alterne para a guia **Contadores de desempenho** do painel **Configurações de diagnóstico az104-11-vm0 \| **e examine os contadores disponíveis.

    >**Observação**: por padrão, CPU, memória, disco e contadores de rede estão ativados. Você pode alternar para o modo de exibição **Personalizado** para obter uma listagem mais detalhada.

1. Alterne para a guia **Logs** do painel **Configurações de diagnóstico az104-11-vm0 \| **e examine as opções de coleta de log de eventos disponíveis.

    >**Observação**: por padrão, a coleta de logs inclui entradas críticas, de erro e de aviso do log do aplicativo e do sistema, bem como entradas de falha de auditoria do log de segurança. Aqui também você pode alternar para o modo de exibição **Personalizado** para definições de configuração mais detalhadas.

1. No painel **az104-11-vm0**, na seção **Monitoramento**, clique em **Logs** e em **Habilitar**.

1. No painel **az104-11-vm0 - Logs**, observe que o **agente do Azure Monitor** será instalado e clique em **Configurar**.  

    >**Observação**: não espere que a operação seja concluída, mas prossiga para a próxima etapa. A operação pode levar cerca de 5 minutos.

1. No painel **az104-11-vm0 \| Logs**, na seção **Monitoramento** clique em **Métricas**.

1. No painel **Métricas az104-11-vm0\|** no gráfico padrão, observe que, neste ponto, a lista suspensa **Namespace de métricas**, além da entrada **Host da Máquina Virtual**, inclui também a entrada **Convidado (clássico)**.

    >**Observação**: isso é esperado, já que você habilitou as configurações de diagnóstico no nível de convidado. Você também tem a opção de **Habilitar novas métricas de memória de convidado**.

1. Na lista suspensa **Namespace de métricas**, selecione a entrada **Convidado (clássico)**.

1. Na lista suspensa **Métrica**, revise a lista de métricas disponíveis.

    >**Observação**: a lista inclui métricas adicionais no nível do convidado que não estão disponíveis quando se depende apenas do monitoramento no nível do host.

1. Na lista suspensa **Métrica**, selecione **Bytes Disponíveis de Memória\\**, na lista suspensa **Agregação**, selecione **Max** e revise o gráfico resultante.

## Tarefa 6: examinar a funcionalidade do Azure Monitor.

1. No portal do Azure, procure e selecione **Monitor** e, no painel **Visão Geral do Monitor\|**, clique em **Métricas**.

1. No painel **Selecionar um escopo**, na guia **Procurar**, navegue até o grupo de recursos **az104-11-rg0**, expanda-o, marque a caixa de seleção ao lado da entrada da máquina virtual **az104-11-vm0** dentro desse grupo de recursos e clique em **Aplicar**.

    >**Observação**: isso oferece a mesma exibição e opções disponíveis no painel **az104-11-vm0 - Métricas**.

1. Na lista suspensa **Métrica**, selecione **Porcentagem de CPU** na lista suspensa **Agregação**, selecione **Média** e revise o gráfico resultante.

1. No painel **Monitorar \| Métricas**, no painel **Porcentagem média de CPU para az104-11-vm0**, clique em **Nova regra de alerta**.

    >**Observação**: a criação de uma regra de alerta a partir das Métricas não é suportada para métricas do namespace de métrica Convidado (clássico). Isso pode ser feito usando modelos do Gerenciador de Recursos do Azure, conforme descrito no documento [Enviar métricas do sistema operacional convidado para o repositório de métricas do Monitor do Azure usando um modelo do Gerenciador de Recursos para uma máquina virtual do Windows](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm)

1. No painel **Criar regra de alerta**, na seção **Condição**, clique na entrada de condição existente.

1. No painel **Configurar lógica de sinal**, na lista de sinais, na seção **Lógica de alerta**, especifique as seguintes configurações (deixe outras com seus valores padrão) e clique em **Concluído**:

    | Configurações | Valor |
    | --- | --- |
    | Limite | **Estático** |
    | Tipo de agregação | **Média** |
    | Operador | **Maior que** |
    | Valor limite | **2** |
    | Verificar a cada | **1 minuto** |
    | Período de retrospectiva| **1 minuto** |

1. Clique em **Avançar: Ações >**, no painel **Criar uma regra de alerta**. Na seção **Grupo de ações**, clique no botão **+ Criar grupo de ações**.

1. Na guia **Noções básicas** da folha **Criar grupo de ações**, especifique as seguintes configurações (deixe outras pessoas com seus valores padrão) e selecione **Avançar: Notificações >**:

    | Configurações | Valor |
    | --- | --- |
    | Subscription | nome da assinatura que você está usando neste laboratório |
    | Grupo de recursos | **az104-11-rg1** |
    | Nome do grupo de ações | **az104-11-ag1** |
    | Nome de exibição | **az104-11-ag1** |

1. Na guia **Notificações** do painel **Criar um grupo de ações**, na lista suspensa **Tipo de notificação**, selecione **Email/mensagem SMS/Push/Voz**. Na caixa de texto **Nome**, digite **email do administrador**. Clique no ícone de lápis, **Editar detalhes**.

1. No painel **E-mail/Mensagem SMS/Push/Voz**, marque a caixa de seleção **E-mail**, digite seu endereço de e-mail na caixa de texto **E-mail**, deixe as outras com seus valores padrão, clique em **OK**, volte para a guia **Notificações** da folha **Criar um grupo de ações**, selecione **Avançar: Ações >**.

1. Na guia **Ações** do painel **Criar grupo de ações**, revise os itens disponíveis na lista suspensa **Tipo de ação** sem fazer alterações e selecione **Revisar + criar**.

1. Na guia **Examinar + criar** do painel **Criar grupo de ações**, selecione **Criar**.

1. De volta ao painel **Criar regra de alerta**, clique em **Avançar: Detalhes>** e, na seção **Detalhes da regra de alerta**, especifique as seguintes configurações (deixe as outras com seus valores padrão):

    | Configurações | Valor |
    | --- | --- |
    | Nome da regra de alerta | **Porcentagem da CPU acima do limite de teste** |
    | Descrição da regra de alerta | **Porcentagem da CPU acima do limite de teste** |
    | Severidade | **Sev 3** |
    | Habilitar a regra depois de ser criada | **Sim** |

1. Clique em **Revisar + criar** e na guia **Revisar + criar** clique em **Criar**.

    >**Observação**: pode demorar até 10 minutos para que uma regra de alerta de métrica se torne ativa.

1. No portal do Azure, procure e selecione **Máquinas virtuais** e, no painel **Máquinas virtuais**, clique em **az104-11-vm0**.

1. No painel **az104-11-vm0**, clique em **Conectar**. No menu suspenso, clique em **RDP** e no painel **Conectar com RDP**, clique em **Baixar arquivo RDP** e siga as instruções para iniciar a sessão de Área de Trabalho Remota.

    >**Observação**: esta etapa refere-se à conexão via Área de Trabalho Remota a partir de um computador Windows. Em um Mac, você pode usar o Cliente de Área de Trabalho Remota na Mac App Store e, em computadores Linux, pode usar um software cliente RDP de código aberto.

    >**Observação**: você pode ignorar quaisquer prompts de aviso ao se conectar às máquinas virtuais de destino.

1. Quando solicitado, entre usando o nome de usuário **Estudante** e a senha do arquivo de parâmetros.

1. Na sessão Área de Trabalho Remota, clique em **Iniciar**, expanda a pasta **Sistema do Windows** e clique em **Prompt de Comando**.

1. No prompt de comando, execute o seguinte para disparar o aumento da utilização da CPU na VM do Azure **az104-11-vm0**:

   ```sh
   for /l %a in (0,0,1) do echo a
   ```

    >**Observação**: isso iniciará o loop infinito que deve aumentar a utilização da CPU acima do limite da regra de alerta recém-criada.

1. Deixe a sessão da Área de Trabalho Remota aberta e volte para a janela do navegador que exibe o portal do Azure no computador do laboratório.

1. No portal do Azure, navegue até o painel **Monitor** e clique em **Alertas**.

1. Observe o número de alertas **Sev 3** e, em seguida, clique na linha **Sev 3**.

    >**Observação**: talvez seja necessário aguardar alguns minutos e clicar em **Atualizar**.

1. Na folha **Todos os Alertas**, revise os alertas gerados.

## Tarefa 7: examinar a funcionalidade do Azure Log Analytics

1. No portal do Azure, navegue de volta para a folha **Monitor** e clique em **Logs**.

    >**Observação**: talvez seja necessário clicar em **Introdução** se esta for a primeira vez que você acessa o Log Analytics.

1. Se necessário, clique em **Selecionar escopo**, na folha **Selecione um escopo**, selecione a guia **Recente**, selecione **az104-11-vm0** e clique em **Aplicar**.

1. Na janela de consulta, cole a seguinte consulta, clique em **Executar** e examine o gráfico resultante:

   ```sh
   // Virtual Machine available memory
   // Chart the VM's available memory over the last hour.
   InsightsMetrics
   | where TimeGenerated > ago(1h)
   | where Name == "AvailableMB"
   | project TimeGenerated, Name, Val
   | render timechart
   ```

    > **Observação**: a consulta não deve ter erros (indicados por blocos vermelhos na barra de rolagem direita). Se a consulta não for colada sem erros diretamente das instruções, cole o código de consulta em um editor de texto, como o Bloco de Notas, e copie-o e cole-o na janela de consulta a partir daí.


1. Clique em **Consultas** na barra de ferramentas, no painel **Consultas**, localize o bloco **Acompanhar a disponibilidade da VM** e clique duas vezes nele para preencher a janela de consulta, clique no botão **Executar** no bloco e confira os resultados.

1. Na guia **Nova Consulta 1**, selecione o cabeçalho **Tabelas** e revise a lista de tabelas na seção **Máquinas virtuais**.

    >**Observação**: os nomes de várias tabelas correspondem às soluções instaladas anteriormente neste laboratório.

1. Passe o mouse sobre a entrada **VMComputer** e clique no ícone **Ver dados de visualização**.

1. Se houver dados disponíveis, no painel **Atualizar**, clique em **Usar no editor**.

    >**Observação**: talvez seja necessário aguardar alguns minutos antes que os dados de atualização fiquem disponíveis.

## Limpar os recursos

>**Observação**: lembre-se de remover todos os recursos recém-criados do Azure que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados.

>**Observação**: não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos. Portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, abra a sessão **PowerShell** no painel **Cloud Shell**.

1. Liste todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*'
   ```

1. Exclua todos os grupos de recursos criados em todos os laboratórios deste módulo executando o seguinte comando:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Observação**: o comando é executado de maneira assíncrona (conforme determinado pelo parâmetro -AsJob), portanto, embora você possa executar outro comando do PowerShell imediatamente após na mesma sessão do PowerShell, levará alguns minutos antes dos grupos de recursos serem de fato removidos.

## Revisão

Neste laboratório, você vai:

+ Provisionou o ambiente de laboratório
+ Criamos e configuramos um workspace do Azure Log Analytics e soluções baseadas em Automação do Azure
+ Revisamos as configurações de monitoramento padrão das máquinas virtuais do Azure
+ Definimos as configurações de diagnóstico da máquina virtual do Azure
+ Revisamos a funcionalidade do Azure Monitor
+ Revisamos a funcionalidade do Azure Log Analytics
