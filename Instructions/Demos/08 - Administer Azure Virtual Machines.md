---
demo:
  title: 'Demonstração 08: Administrar máquinas virtuais do Azure'
  module: Administer Azure Virtual Machines
---


# 08 - Administrar Máquinas Virtuais do Azure

## Demonstração: criar uma máquina virtual no portal

Nesta demonstração, criaremos e acessaremos uma máquina virtual do Azure no portal.

**Referências**

Início Rápido – Criar uma VM Windows no portal do Azure

Início Rápido – Criar uma VM Linux no portal do Azure

[Azure Bastion para se conectar a máquinas virtuais](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

Criar a máquina virtual

**Nota:** Estas etapas abrangem apenas alguns parâmetros de máquina virtual. Sinta-se livre para explorar e cobrir outras áreas. Você pode criar uma máquina virtual Windows ou Linux, dependendo do seu público.

1. Use o portal do Azure.

1. Procure **máquinas virtuais**. 

1. Criar uma máquina virtual – Guia Noções básicas Revise as opções de disponibilidade, imagens e regras de entrada.

1. Discuta a importância de criar uma conta de administrador segura.

1. Crie o recurso e aguarde até que ele seja implantado.  

**Conecte-se à máquina virtual**

1. Há várias maneiras de se conectar** à **máquina virtual. 

1. Para um servidor Windows, você pode usar **o RDP**, conforme mostrado no Guia de Início Rápido. 

1. Para um servidor Linux, você pode **SSH**, como mostrado no Guia de início rápido. 

1. Para qualquer servidor, você pode se conectar com o **serviço Bastion (Guia de** início rápido). Analise por que Bastion é preferido a RDP ou SSH. 

## Configurar disponibilidade da máquina virtual

Nesta demonstração, exploraremos as opções de dimensionamento de máquina virtual.

**Referências**

Criar máquinas virtuais em um conjunto de dimensionamento usando o portal do Azure

1. Use o portal do Azure.

1. Pesquise e selecione **Conjuntos de dimensionamento de máquinas virtuais**. 

1. Crie um conjunto de dimensionamento de máquinas virtuais Revise a finalidade dos conjuntos de dimensionamento de máquina virtual. Compare as diferenças de API entre os modos de orquestração uniforme e flexível. Explicar sua seleção pode afetar suas opções de dimensionamento. 

1. Mover para a **guia Dimensionamento** . 

1. Analise como a escala ** manual e **a**política** de dimensionamento são usadas. 

1. Altere para uma **política de dimensionamento personalizado** . 

1. Analise como **o limite de CPU (%)** é usado para dimensionar e dimensionar nas instâncias de máquina virtual. 

