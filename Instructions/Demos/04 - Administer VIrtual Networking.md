---
demo:
  title: 'Demonstração 04: Administrar redes virtuais'
  module: Administer Virtual Networking
---

# 04 - Administrar Redes Virtuais

## Configurar Redes Virtuais

Nesta demonstração, você criará redes virtuais.

Guia de início rápido: Criar uma rede virtual – portal do Azure

## Crie uma rede virtual no portal do Azure.

1.  Entre no portal do Azure e pesquise por  **Cofres de chaves**.

1.  Crie uma rede virtual, explicando as configurações básicas à medida que avança. Verifique se pelo menos uma sub-rede foi criada. 

1.  Verifique se sua rede virtual foi criada.

## Configurar Grupos de Segurança de Rede

Nesta demonstração, você explorará NSGs e pontos de extremidade de serviço.

Restringir o acesso de recursos de PaaS – tutorial – portal do Azure

Criar um grupo de segurança de rede

1. Acessar o portal do Azure.

1. Procure e selecione  **Grupos de segurança de rede**.

1. Crie um NSG explicando as configurações à medida que avança. 
 
1. Aguarde até que o ASG seja implantado.

**Explore as regras de entrada e saída**

1. Selecione seu novo NSG.

1. Discuta como o NSG pode ser associado a sub-redes ou interfaces de rede.

1. Discuta as regras de entrada e saída padrão.  

1. Discuta as regras de entrada e saída padrão. 

1. Crie uma nova regra, explicando as configurações à medida que avança. Discuta especificamente a seleção de serviço (como HTTPS) e as configurações de prioridade. 

## Configurar o DNS do Azure

Nesta demonstração, exploraremos o Azure Key Vault.

Tutorial: hospedar seu domínio e subdomínio no DNS do Azure


**Criar uma zona DNS**

1. Acessar o portal do Azure.

1. Procure o **serviço de zonas** DNS.

1. Crie uma **zona DNS e explique a finalidade da zona** . Para um nome, você pode usar contoso.internal.com.

1.  Aguarde a VM a ser criada. Talvez seja necessário  **Atualizar**  a página.

Adicionar conjuntos de registros DNS

Tutorial: Criar um registro de alias para se referir a um registro de recurso de zona

1. Depois que sua zona for criada, selecione **+Conjunto** de registros.

1. Use a **lista suspensa Tipo** para exibir os diferentes tipos de registros. Revise como os diferentes tipos de registro são usados. Observe como as informações do registro mudam à medida que você seleciona diferentes tipos de registro.

1. Crie um **registro A** como exemplo. 

