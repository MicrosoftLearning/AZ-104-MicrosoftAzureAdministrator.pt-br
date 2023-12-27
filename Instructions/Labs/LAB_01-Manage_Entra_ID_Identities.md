---
lab:
  title: 'Laboratório 01: Gerenciar identidades de ID do Microsoft Entra'
  module: Administer Identity
---

# Laboratório 01 - Gerenciar identidades de ID do Microsoft Entra

# Manual do aluno

## Cenário do laboratório

Para permitir que os usuários da Contoso se autentiquem usando a ID do Microsoft Entra, você foi encarregado de provisionar usuários e contas de grupo. A associação aos grupos deve ser atualizada automaticamente com base nos cargos dos usuários. Você também precisa criar um locatário de teste com uma conta de usuário de teste e conceder a essa conta permissões limitadas para recursos na assinatura do Contoso Azure.

**Nota:** Uma **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** de laboratório interativa está disponível que permite que você clique neste laboratório no seu próprio ritmo. Você pode encontrar pequenas diferenças entre a simulação interativa e o laboratório hospedado, mas os principais conceitos e ideias que estão sendo demonstrados são os mesmos.

## Objetivos

Neste laboratório, você vai:

+ Tarefa 1: criar e configurar usuários do Azure AD.
+ Tarefa 2: criar grupos do AD com associação atribuída e dinâmica.
+ Tarefa 3: Criar um locatário (opcional - problema de ambiente de laboratório)
+ Tarefa 4: Gerenciar usuários convidados (opcional - problema de ambiente de laboratório)

## Tempo estimado: 30 minutos

## Diagrama de arquitetura
![imagem](../media/lab01entra.png)

### Instruções

## Exercício 1

## Tarefa 1: criar e configurar usuários do Azure AD.

Nesta tarefa, você criará e configurará usuários.

>**Nota**: Se você tiver usado anteriormente a licença de avaliação para a ID do Microsoft Entra neste locatário, precisará de um novo locatário e executará a Tarefa 2 após a Tarefa 3 no novo locatário.

1. Entre no [portal do Azure](https://portal.azure.com).

1. No portal do Azure, pesquise pela **ID do Microsoft Entra** e selecione-a.

1. Na folha ID do Microsoft Entra, role para baixo até a **seção Gerenciar** , clique em **Configurações** do usuário e revise as opções de configuração disponíveis.

1. Na folha ID do Microsoft Entra, na **seção Gerenciar**, clique em Usuários** e em **sua conta de usuário para exibir suas **configurações de perfil**. 

1. Clique em Editar propriedades** e, na **guia Configurações**, defina **Local de uso como **Estados Unidos** e clique em **Salvar**** para aplicar a **alteração.

    >**Nota**: Isso é necessário para atribuir uma licença do Microsoft Entra ID P2 à sua conta de usuário posteriormente neste laboratório.

1. Navegue de volta para a **folha Usuários - Todos os usuários** e clique em **+ Novo usuário**.

1. Crie um novo usuário com as seguintes configurações (deixe outros com seus padrões):

    | Configuração | Valor |
    | --- | --- |
    | Nome UPN | az104-01a-aaduser1 |
    | Nome de exibição | az104-01a-aaduser1 |
    | Gerar senha automaticamente | Desmarcar |
    | Senha inicial | **Forneça uma senha segura** |
    | Título do trabalho (guia Propriedades) | Administradores de nuvem |
    | Departamento (guia Propriedades) | **IT** |
    | Local de uso (guia Propriedades) | **Estados Unidos** |

    >**Nota**: **Copie para a área de transferência** o Nome** Principal do Usuário completo **(nome de usuário mais domínio). Você precisará dela posteriormente nesta tarefa.

1. Na lista de usuários, clique na conta de usuário recém-criada para exibir sua folha.

1. Analise as opções disponíveis na **seção Gerenciar** e observe que você pode identificar as funções atribuídas à conta de usuário, bem como as permissões da conta de usuário para recursos do Azure.

1. Na seção Gerenciar, clique em Funções** atribuídas, clique **no **botão + Adicionar atribuição** e atribua a função de administrador** do usuário a **az104-01a-aaduser1****.** **

    >**Nota**: Você também tem a opção de atribuir funções ao provisionar um novo usuário.

1. Abra uma nova janela do navegador e faça logon no portal do Azure usando a conta ad-admin recém-criada. Quando solicitado a atualizar a senha, altere-a para uma senha segura de sua escolha. 

    >**Nota**: Em vez de digitar o nome de usuário (incluindo o nome de domínio), você pode colar o conteúdo da Área de Transferência.

1. No portal do Azure, pesquise e selecione o serviço Microsoft Entra ID.

    >**Observação**: embora essa conta de usuário possa acessar o locatário, ela não tem acesso aos recursos do Azure. Isso é esperado, já que esse acesso precisaria ser concedido explicitamente usando o Controle de Acesso Baseado em Função do Azure. 

1. Na janela do navegador InPrivate **, na folha ID do Microsoft Entra, role para baixo até a **seção Gerenciar**, clique em **Configurações** do **usuário e observe que você não tem permissões para modificar nenhuma opção de configuração.

1. Na janela do navegador InPrivate, na folha ID do **Microsoft Entra, na **seção Gerenciar**, clique em Usuários** e em ****+ Novo usuário**.**

1. Crie um novo usuário com as seguintes configurações (deixe outros com seus padrões):

    | Configuração | Valor |
    | --- | --- |
    | Nome UPN | az104-01a-aaduser2 |
    | Nome de exibição | az104-01a-aaduser2 |
    | Gerar senha automaticamente | Desmarcar  |
    | Senha inicial | **Forneça uma senha segura** |
    | Cargo | **Administrador do Sistema** |
    | Departamento | **IT** |
    | Local de uso | **Estados Unidos** |
    
1. Saia como o usuário az104-01a-aaduser1 no portal do Azure e feche a janela do navegador InPrivate.

## Tarefa 2: criar grupos do AD com associação atribuída e dinâmica.

Tarefa 2: criar grupos do AD com associação atribuída e dinâmica.

1. De volta ao portal do Azure onde você está conectado com sua **conta** de usuário, navegue de volta até a **folha Visão geral** do locatário e, na seção Gerenciar **, clique em ****Licenças**.

    >**Nota: As** licenças P1 ou P2 do Microsoft Entra ID Premium são necessárias para implementar grupos dinâmicos.

1. Na seção **Gerenciar**, clique em **Propriedades**.

1. Clique em **+ Try/Buy** e ative a avaliação gratuita do Microsoft Entra ID Premium P2.

1. Atualize a janela do navegador para verificar se a ativação foi bem-sucedida. 

    >**Nota**: Pode levar até 10 minutos para que as licenças sejam ativadas. Continue atualizando a página até que ela apareça. Não prossiga até que as licenças tenham sido ativadas.

1. Na folha Licenças - Todos os produtos **, selecione a ****entrada Microsoft Entra ID P2** e atribua todas as opções de licença à sua conta de usuário e às duas contas de usuário recém-criadas.

1. No portal do Azure, navegue de volta até a folha de locatário do Microsoft Entra ID e clique em **Grupos**.

1. Depois, clique no botão  para criar outro arquivo com as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Tipo de grupo | **Segurança** |
    | Nome do grupo | Administradores de Nuvem de TI |
    | Descrição do grupo | Administradores de Nuvem de TI |
    | Tipo de associação | **Usuário dinâmico** |

    >**Nota**: Se a lista suspensa Tipo de associação** estiver esmaecida, aguarde alguns minutos e atualize a **página do navegador.

1. Adicionar consulta dinâmica

1. **Na guia Configurar Regras** da folha Regras** de **associação dinâmica, crie uma nova regra com as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Propriedade | **JobTitle** |
    | Operador | **Igual a** |
    | Valor | Administradores de nuvem |

1. Salve a regra clicando em **+Adicionar expressão** e **Salvar**. Na folha **Novo**, clique em **Criar**. 

1. De volta à **folha Grupos - Todos os grupos** do locatário, clique no **botão + Novo grupo e crie um novo grupo** com as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Tipo de grupo | **Segurança** |
    | Nome do grupo | Administradores dos Sistemas de TI |
    | Descrição do grupo | Administradores dos Sistemas de TI |
    | Tipo de associação | **Usuário dinâmico** |

1. Adicionar consulta dinâmica

1. **Na guia Configurar Regras** da folha Regras** de **associação dinâmica, crie uma nova regra com as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Propriedade | **JobTitle** |
    | Operador | **Igual a** |
    | Valor | **Administrador do Sistema** |

1. Salve a regra clicando em **+Adicionar expressão** e **Salvar**. Na folha **Novo**, clique em **Criar**. 

1. De volta à **folha Grupos - Todos os grupos** do locatário, clique no **botão + Novo grupo e crie um novo grupo** com as seguintes configurações:

    | Configuração | Valor |
    | --- | --- |
    | Tipo de grupo | **Segurança** |
    | Nome do grupo | Administradores de laboratório de TI |
    | Descrição do grupo | Administradores de laboratório de TI |
    | Tipo de associação | **Atribuído** |
    
1. Em seguida, clique em **Nenhum membro selecionado**.

1. **Na folha Adicionar membros**, pesquise e selecione os grupos Administradores** de TI do Cloud e Administradores** do **Sistema de TI e**, de volta à **folha Novo Grupo**, clique em **Criar**.

1. De volta à **folha Grupos - Todos os grupos** , clique na entrada que representa o **grupo Administradores** do IT Cloud e, em seguida, exiba a **folha Membros** . Verifique se o **az104-01a-aaduser1** aparece na lista de membros do grupo.

    >**Observação**: você pode enfrentar atrasos com as atualizações dos grupos dinâmicos de associação. Para agilizar a atualização, navegue até a folha do grupo, exiba a folha Regras** de associação dinâmica, **Edite** a regra listada **na caixa de texto Sintaxe** da regra adicionando um espaço em branco no final e **Salve** a **alteração.

1. Navegue de volta para a **folha Grupos - Todos os grupos** , clique na entrada que representa o **grupo Administradores** de Sistema de TI e, em seguida, exiba sua **folha Membros** . Verifique se o **az104-01a-aaduser2** aparece na lista de membros do grupo.

## Tarefa 3: Criar um locatário (Opcional - Possíveis problemas de captcha, assinatura paga necessária)

Nesta tarefa, você criará uma nova função de usuário.
    
1. No portal do Azure, pesquise pela **ID do Microsoft Entra** e selecione-a.

    >**Nota**: Há um problema conhecido com a verificação do Captcha no ambiente de laboratório. Se você receber o erro **Falha na criação. Muitas solicitações, por favor, tente mais tarde**, faça o seguinte:
    - Experimente a criação algumas vezes.<br>
    - Verifique a **seção Gerenciar locatário para garantir que o locatário** não foi criado em segundo plano. <br>
    - Abra uma nova **janela InPrivate** e use o Portal do Azure e tente criar o locatário a partir daí.<br>
     Levante o problema com o treinador e, em seguida, use a **[simulação](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** de laboratório interativo para exibir as etapas. <br>
    - Você pode tentar essa tarefa mais tarde, mas a criação de um locatário não é necessária em outros laboratórios. 

1. Clique em **Gerenciar locatários** e, na tela seguinte, clique em **+ Criar** e especifique a seguinte configuração:

    | Configuração | Valor |
    | --- | --- |
    | TIPO DE DIRETÓRIO | **Microsoft Entra ID** |
    
1. Clique em **Avançar: configuração**.

    | Configuração | Valor |
    | --- | --- |
    | Nome da organização | Contoso Lab |
    | Nome de domínio inicial | Qualquer nome DNS válido que consista em letras minúsculas e dígitos e comece com uma letra | 
    | País/Região | **Estados Unidos** |

   > **Observação**: o **nome de domínio inicial não deve ser um nome** legítimo que potencialmente corresponda à sua organização ou a outra. A marca de seleção verde na **caixa de texto Nome de domínio inicial indicará que o nome de domínio** digitado é válido e exclusivo.

1. Clique em **Revisar + criar** e em **Criar**.

1. Exiba a folha do locatário recém-criado usando o link Clique aqui para navegar até seu novo locatário: Contoso Lab** ou o **** botão Diretório + Assinatura** (diretamente à direita do botão Cloud Shell) na barra de ferramentas do portal do Azure.

## Tarefa 4: Gerenciar usuários convidados.

Nesta tarefa, você criará usuários convidados e concederá a eles acesso a recursos em uma assinatura do Azure.

1. No portal do Azure que exibe o locatário do Contoso Lab, na **seção Gerenciar**, clique em Usuários** e em ****+ Novo usuário**.

1. Crie um novo usuário com as seguintes configurações (deixe outros com seus padrões):

    | Configuração | Valor |
    | --- | --- |
    | Nome UPN | az104-01b-aaduser1 |
    | Nome de exibição | az104-01b-aaduser1 |
    | Gerar senha automaticamente | Desmarcar  |
    | Senha inicial | **Forneça uma senha segura** |
    | Cargo | **Administrador do Sistema** |
    | Departamento | **IT** |

1. Clique no aplicativo recém-criado.

    >**Nota**: **Copie para a área de transferência** o Nome** Principal do Usuário completo **(nome de usuário mais domínio). Você precisará dela posteriormente nesta tarefa.

1. Retorne ao primeiro locatário que você criou anteriormente.
2. Selecione **Visão geral** no painel de navegação.
3. Clique em **Gerenciar locatários**.
4. Marque a caixa ao lado do primeiro locatário criado anteriormente e selecione **Alternar**.

1. Navegue de volta para a **folha Usuários - Todos os usuários** e clique em **+ Convidar usuário** externo.

1. Convide um novo usuário convidado com as seguintes configurações (deixe outras pessoas com seus padrões):

    | Configuração | Valor |
    | --- | --- |
    | Email | o Nome Principal do Usuário que você copiou anteriormente nesta tarefa |
    | Nome para exibição (guia Propriedades)  | az104-01b-aaduser1 |
    | Título do trabalho (guia Propriedades) | Administradores de laboratório de TI |
    | Departamento (guia Propriedades) | **IT** |
    | Local de uso (guia Propriedades) | **Estados Unidos** |

1. Clique em **Convidar**. 

1. De volta à **folha Usuários - Todos os usuários** , clique na entrada que representa a conta de usuário convidado recém-criada.

1. **Na folha az104-01b-aaduser1 - Perfil**, clique em **Grupos**.

1. Clique em **+ Adicionar associação** e adicione a conta de usuário convidado ao grupo Administradores** do **Laboratório de TI.


## Tarefa 5: limpar os recursos

> Lembre-se de remover todos os recursos do Azure que acabam de ser criados e que você não usa mais. Remover recursos não utilizados garante que você não veja encargos inesperados. Embora, nesse caso, não haja cobranças adicionais associadas aos locatários e seus objetos, convém considerar a remoção das contas de usuário, das contas de grupo e do locatário criado neste laboratório.

 > **Nota**: Não se preocupe se os recursos do laboratório não puderem ser removidos imediatamente. Às vezes, os recursos têm dependências e levam mais tempo para serem excluídos. É uma tarefa comum do Administrador monitorar o uso de recursos, portanto, basta revisar periodicamente seus recursos no Portal para ver como a limpeza está indo. 

1. No portal do Azure, pesquise pela ID do Microsoft Entra e selecione-a. Em **Gerenciar** , selecione **Licenças**. Uma vez em Licenças, em ****Gerenciar****, selecione Todos os Produtos** e, em seguida, selecione ****o item Microsoft Entra ID Premium P2** na lista. Prossiga selecionando **Usuários Licenciados**. Selecione as contas **de usuário az104-01a-aaduser1** e az104-01a-aaduser2** às quais você atribuiu licenças neste laboratório, clique em Remover licença** e**, quando solicitado a confirmar, clique em ****Sim**.

1. No portal do Azure, navegue até a folha Usuários - Todos os usuários **, clique na entrada que representa a conta de usuário convidado az104-01b-aaduser1, na folha az104-01b-aaduser1** - Perfil** clique em Excluir** e, quando solicitado a **** confirmar, clique em ****** OK.**

1. Repita a mesma sequência de etapas para excluir as contas de usuário restantes criadas neste laboratório.

1. Navegue até a folha Grupos - Todos os grupos, selecione os grupos** criados neste laboratório, clique em Excluir** e, quando solicitado a **confirmar, clique **em **OK.**

1. No portal do Azure, exiba a folha do locatário do Contoso Lab usando o **botão Diretório + Assinatura** (diretamente à direita do botão Cloud Shell) na barra de ferramentas do portal do Azure.

1. Navegue até a folha Usuários - Todos os usuários, clique na entrada que representa a conta de usuário az104-01b-aaduser1, na folha az104-01b-aaduser1** - Perfil** clique em Excluir e, quando solicitado a **** confirmar, clique em ******OK.******

1. Navegue até a folha Contoso Lab - Visão geral** do locatário do Contoso Lab, clique em Gerenciar locatários** e, na próxima tela, selecione a **caixa ao lado **de Contoso Lab**, clique em **** Excluir**, em Excluir locatário **'Contoso Labs'?** , clique no **link Obter permissão para excluir recursos do Azure, na folha Propriedades **, defina **Gerenciamento de acesso para recursos**** do **Azure como **Sim** e clique em **Salvar**.

1. Navegue de volta para a **folha Excluir locatário 'Contoso Lab'** e clique em Atualizar **, clique em ****Excluir**.

> **Observação**: se um locatário tiver uma licença de avaliação, você terá que aguardar a expiração da licença de avaliação antes de excluir o locatário. Isso não incorreria em nenhum custo adicional.

#### Revisão

Neste laboratório, você vai:

- Usuários criados e configurados
- Tarefa 2: criar grupos do AD com associação atribuída e dinâmica.
- Criar um locatário
- Gerenciar seus usuários convidados 
