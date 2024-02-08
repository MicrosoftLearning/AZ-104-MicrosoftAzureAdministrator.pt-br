---
demo:
  title: 'Demonstração 05: Administrar conectividade entre sites'
  module: Administer Intersite Connectivity
---

# 05 - Administrar a conectividade entre sites

## Configurar o emparelhamento de VNet

**Nota:**  Para esta demonstração você precisará de duas redes virtuais.

Tutorial: conectar redes virtuais com o Emparelhamento VNET – portal do Azure

Configurar o emparelhamento de rede virtual hub

1. No portal do Azure, selecione a rede virtual. Analise o valor do emparelhamento. 

1. Em **Configurações, selecione **Emparelhamentos e **+ Adicionar** um novo emparelhamento****.

1. Configurar o emparelhamento de rede virtual hub Use os ícones de informações para revisar as diferentes configurações. 

1. Quando o emparelhamento estiver concluído, revise o **status** Emparelhamento. 

Confirmar a segunda rede virtual

1. No portal do Azure, selecione a rede virtual.

1. Em **Configurações, selecione **Emparelhamentos****.

1. Observe que um emparelhamento foi criado automaticamente. Observe que o Status ** de**emparelhamento está **conectado**.

1. No laboratório, os alunos criarão emparelhamento e testarão a conexão entre máquinas virtuais. 

## Configurar pontos de extremidade e roteamento de rede

Nesta demonstração, aprenderemos como criar uma tabela de rotas, definir uma rota personalizada e associar a rota a uma sub-rede.

**Nota:** Esta demonstração requer uma rede virtual com pelo menos uma sub-rede.

Rotear o tráfego de rede – tutorial – portal do Azure

Criar uma tabela de roteamento

1. À medida que tiver tempo, reveja o diagrama tutorial. Explique por que você precisa criar uma rota definida pelo usuário. 

1. Acessar o portal do Azure

1. Pesquise por **Tabela de rotas** e selecione essa opção. Discuta quando **as rotas** de gateway de propagação devem ser usadas. 

1. Crie uma tabela de roteamento, explique as configurações incomuns. 

1. Aguarde até que a nova tabela de roteamento seja implantada.

Adicionar uma rota

1.  Selecione sua nova tabela de roteamento e, em seguida, selecione **Rotas**.

1.  Criar um novo roteiro Discuta os diferentes **tipos** de salto disponíveis. 

1.  Selecione Criar e aguarde até que o recurso seja implantado.
 
Associar uma tabela de rotas a uma sub-rede

1.  Selecione a sub-rede à qual você deseja associar a tabela de rotas.

1.  Selecione **Tabela de rotas e escolha sua nova tabela de** roteamento. 

1.  **Salve** suas alterações.

