---
demo:
  title: 'Demonstração 07: administrar o Armazenamento do Azure'
  module: Administer Azure Storage
---


# 07 - Administrar o Armazenamento do Azure

## Configurar contas de armazenamento

Nesta demonstração, criaremos uma conta de armazenamento.

**Referência**: [criar uma conta de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. Use o portal do Azure.

1. Revisar a finalidade das contas de armazenamento. 
   
1. Pesquisar e selecionar **Contas de armazenamento**. 
 
1. Criar uma conta de armazenamento básica. 

    - Discutir os requisitos relacionados à nomeação de uma conta de armazenamento e a necessidade de o nome ser exclusivo no Azure. 

    - Analisar os diferentes tipos de armazenamento. Por exemplo, v2 de uso geral. 

    - Examinar as seleções da camada de acesso. Por exemplo, as camadas frias e quentes. 

    - Outras guias e configurações serão abordadas em outras demonstrações. 

1. Criar a conta de armazenamento e esperar que o recurso seja implantado. 


## Configurar Armazenamento de Blobs

Nesta demonstração, exploraremos o armazenamento de blobs.

**Observação:** estas etapas exigem uma conta de armazenamento.

**Referência**: [Início rápido: carregar, baixar e listar blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. Navegar até uma conta de armazenamento no portal do Azure.

1. Revisar a finalidade do armazenamento de blobs. 

1. Criar um contêiner de blob. Definir o nível de acesso do contêiner. Por exemplo, privado (sem acesso anônimo). 

1. Carregue um blob no contêiner. À medida que tiver tempo, reveja as configurações avançadas. Por exemplo, tipo de blob e tamanho de blob. 

## Configurar a segurança de armazenamento

Nesta demonstração, criaremos uma assinatura de acesso compartilhado.

**Observação:** esta demonstração requer uma conta de armazenamento com um contêiner de blob e um arquivo carregado.

**Referência**: [criar tokens SAS para contêineres de armazenamento](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. Selecionar um blob ou arquivo que você deseja proteger. 

1. Gerar uma SAS (Assinatura de Acesso Compartilhado). Revisar as permissões, os horários de início e expiração e os protocolos permitidos.

1. Usar a URL SAS para garantir que o recurso seja exibido. 


## Configurar Arquivos do Azure 

Nesta demonstração, trabalharemos com compartilhamentos de arquivos e instantâneos.

**Observação:** estas etapas exigem uma conta de armazenamento.

**Referência**: [início rápido para gerenciar compartilhamentos de arquivo do Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. Revisar a finalidade dos compartilhamentos de arquivos. 

1. Acesse uma conta de armazenamento e clique em  **Arquivos**.

1. Crie um compartilhamento de arquivos. Revisar cotas, upload de arquivos e adição de diretórios para organizar as informações. 

1. Criar instantâneos de compartilhamento de arquivo. Revisar quando usar snapshots e como eles são diferentes dos backups. À medida que tiver tempo, carregue um arquivo, tire um instantâneo, exclua o arquivo e restaure-o.


## Ferramentas de armazenamento (opcional)

Nesta demonstração, analisaremos várias ferramentas comuns de armazenamento do Azure. 

**Referência**: [Introdução ao Gerenciador de Armazenamento](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. Instale o Gerenciador de Armazenamento ou use o Navegador de Armazenamento.

1. Analise como navegar e criar recursos de armazenamento. Por exemplo, adicione um contêiner de blob. 

**Referência**: [Copiar ou mover dados para o Armazenamento do Azure com o AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. Discuta quando usar o AzCopy. Veja a ajuda, **azcopy /?**.

1. Role para baixo a seção **Exemplos** . Como você tem tempo, tente qualquer um dos exemplos. 
    



