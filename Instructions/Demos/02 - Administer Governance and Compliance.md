---
demo:
  title: 'Demonstração 02: Administrar Governança e Compliance'
  module: Administer Governance and Compliance
---

# 02 - Administrar Governança e Compliance

## Configurar assinaturas

Esta área não tem uma manifestação formal. 

Criar uma assinatura adicional do Azure

## Configurar o Azure Policy

Nesta demonstração, trabalharemos com políticas do Azure.

Tutorial: Criar políticas para impor conformidade

**Atribuir uma política**

1.  Acessar o portal do Azure

2.  Pesquise por  **Política** e selecione essa opção.

3.  Selecione **Atribuições** e, em seguida, **Atribuir política**.

5.  O escopo determina em quais recursos ou grupo de recursos a atribuição de política é imposta.

6.  Selecione a reticências  **Definição de política** para abrir a lista de definições. Reserve algum tempo para revisar as definições de política internas.

7.  Pesquise e selecione a **política Locais permitidos** . Esta política permite que você restrinja os locais que sua organização pode especificar ao implantar recursos.

8.  Mova a guia Parâmetros ** e, usando a lista suspensa,** selecione um ou mais locais permitidos.

9.  Clique em Revisar + criar e, em seguida, em ** **Criar** para criar** a política.

Criar e atribuir uma definição de iniciativa

1.  Retorne à página Política do Azure e selecione **Definições** em Criação.

2.  Selecione **Definição** de iniciativa na parte superior da página.

3.  Forneça um nome ** e **uma**descrição**.

4.  **Crie uma nova** categoria.

5.  No painel **direito, adicione** a **política Locais permitidos** .

6.  Adicione uma política adicional de sua escolha.

7.  **Salve** suas alterações e, em seguida, **Atribua** sua definição de iniciativa à sua assinatura.

Verificar conformidade

1.  Volte para a página Policy no portal do Azure.

2.  Selecione **Conformidade**.

3.  Revise o status de sua política e sua definição.

Acompanhar tarefas de correção

1.  Volte para a página Policy no portal do Azure.

2.  Selecione **Remediação**.

3.  Revise todas as tarefas de correção listadas.

4. Como você tem tempo, remova a política e a iniciativa. 

## Configurar o controle de acesso baseado em função

Nesta tarefa, vamos aprender sobre as atribuições de função.

Tutorial: Conceder acesso aos recursos do Azure para um usuário usando o portal do Azure – RBAC do Azure

Início rápido – Verificar o acesso que um usuário tem aos recursos do Azure – RBAC do Azure

**Localizar o painel de Controle de Acesso**

1.  Acesse o portal do Azure e selecione um grupo de recursos. Anote o grupo de recursos que você usa.

2.  Selecione o painel  **IAM (Controle de Acesso)** .

3.  Esse painel estará disponível para muitos recursos diferentes, para que você possa controlar o acesso.

**Examinar permissões de função**

1.  Selecione a guia  **Funções**  (superior).

1.  Examine o grande número de funções internas que estão disponíveis.

1.  Clique duas vezes em uma função e selecione  **Permissões**  (superior).

1.  Continue detalhando a função até que você possa exibir as ações de  **Leitura, Gravação e Exclusão**  para essa função.

1.  Retorne para o painel  **IAM (Controle de Acesso)** .

**Adicionar uma atribuição de função**

1.  Crie um usuário ou encontre um usuário existente.

1.  Selecione Adicionar e escolha Adicionar atribuição de função. Por exemplo, *proprietário*.

1.  Selecione **Verificar acesso**.

1.  Analise as permissões do usuário.

1.  Observe que você pode **Negar atribuições**.
