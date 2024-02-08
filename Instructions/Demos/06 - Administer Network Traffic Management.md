---
demo:
  title: 'Demonstração 06: Administrar o gerenciamento de tráfego de rede'
  module: Administer Network Traffic Management
---


# 06 - Administrar o Gerenciamento de Tráfego de Rede

## Configurar o Azure Load Balancer

Nesta demonstração, aprenderemos como criar um balanceador de carga público. 

**Nota:** Esta demonstração requer uma rede virtual com pelo menos uma sub-rede. 

Guia de Início Rápido: Criar um balanceador de carga público para balancear cargas de VMs usando o portal do Azure

**Mostrar o recurso de ajuda para escolher do portal**

1. Acessar o portal do Azure

1. Procure e selecione **Balanceamento de carga - ajude-me a escolher**.

1. Use o assistente para percorrer diferentes cenários.
   
**Criar um balanceador de carga**

1. Continuar para o Portal do Azure

1. Procure e selecione **Balanceador de** carga. **Crie um balanceador de carga.** 

1. Na guia Noções básicas **, discuta **** SKU,**** Tipo** e **Camada**.

1. Na guia Configuração de** IP de front-end, discuta o uso de **um endereço IP público.

1. **Na guia Pools** de back-end, selecione a rede virtual com intervalo de endereços IP.

1. **Na guia Regras de** entrada, crie uma regra de balanceamento de carga. Discuta parâmetros como **Protocolo**, **Portas**, **Testes** de integridade e **Persistência de** sessão. 


## Configurar o Gateway de Aplicativo do Azure

Nesta demonstração, aprenderemos como criar um Gateway de Aplicativo do Azure. 

**Nota**: Para manter as coisas simples, crie novas redes virtuais e sub-redes à medida que avança na configuração. 

Início Rápido: Direcionar o tráfego da Web com o Gateway de Aplicativo do Azure – portal do Azure

Criar um Gateway de Aplicativo do Azure.

1. Acessar o portal do Azure

1. Pesquise e selecione **Gateways de Aplicativo**.

1. Crie um gateway da NAT.

1. Na guia Noções básicas, discuta Camadas, **dimensionamento automático e **contagens de**** instâncias****.****

1. Na guia Frontends **, discuta **os tipos de endereço IP.

1. **Na guia Back-ends, discuta quando usar um pool de back-end** vazio.

1. Na guia de Configuração, selecione **Adicionar uma regra de roteamento**. Compare com as regras do balanceador de carga.

1. Explique que, após a criação do gateway, você adicionaria destinos de back-end e testaria. 
