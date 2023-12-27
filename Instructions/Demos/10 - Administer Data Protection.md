---
demo:
  title: 'Demonstração 10: Administrar a proteção de dados'
  module: Administer Data Protection
---

# 10 - Administrar a Proteção de Dados

## Backup de compartilhamentos de Arquivos do Azure

Nesta demonstração, exploraremos o backup de um compartilhamento de arquivos no portal do Azure.

> **Nota:** Esta demonstração requer uma conta de armazenamento com um compartilhamento de arquivos. 

Faça backup dos compartilhamentos de arquivos do Azure no portal do Azure.

**Criar um cofre dos Serviços de Recuperação**

1. Use o portal do Azure.

1. Pesquise e, em seguida, selecione **cofres dos Serviços de Recuperação**.

1. Crie um **Cofre de Serviços de Recuperação**. Revise o requisito de que o vault esteja na mesma região que o compartilhamento de arquivos. 

1. Aguarde até que o cofre de chaves seja criado. 

**Configurar o backup de arquivos do Azure**

1. Vá para **Central de backup e crie uma nova **instância de backup****.

1. Revise e discuta as opções na **lista suspensa Tipo** de fonte de dados. Selecionar Armazenamento do Microsoft Azure (Arquivos do Azure) 

1. Selecione seu cofre de chaves.

1. Continuar configurando a entidade. Selecione a conta de armazenamento específica e o compartilhamento de arquivos dos quais você deseja fazer backup.  

1. Nos detalhes** da política, **clique em **Editar esta política**. Discuta a finalidade das políticas de backup. Revise o agendamento** de backup e **o **intervalo** de retenção.  

1. **Habilite o backup** para salvar suas alterações. 

1. À medida que tiver tempo, analise **como Restaurar** uma **instância** de backup. Como **monitorar trabalhos de backup?** 

## Fazer backup de máquinas virtuais do Azure

Nesta demonstração, agendaremos um backup diário de uma máquina virtual em um cofre dos Serviços de Recuperação.

> **Nota:** Esta demonstração requer uma máquina virtual e um cofre de serviço de recuperação.

Tutorial – Fazer backup de várias máquinas virtuais do Azure

1. Use o portal do Azure.

1. Vá para **Central de backup e crie uma nova **instância de backup****.

1. Selecione **máquinas virtuais do Azure** como o **tipo de fonte de dados** e selecione o cofre que você criou.

1. Revise o **DefaultPolicy**. A política padrão faz o backup da VM uma vez por dia. Os backups diários são mantidos por 30 dias. Instantâneos de recuperação instantânea são mantidos por dois dias.

1. Use **Habilitar backup** para salvar sua configuração.

1. À medida que tiver tempo, analise como fazer **backup agora**. Além disso, como revisar seus **trabalhos** de backup.  

