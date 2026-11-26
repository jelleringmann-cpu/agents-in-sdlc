---
title: "Exercício 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não tenham dificuldades com dívida técnica. Isso pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram no backlog por falta de tempo para implementá-las. O cloud agent do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para uma pessoa desenvolvedora revisar. Assim, é possível delegar tarefas tediosas, acelerar o desenvolvimento e se concentrar em questões mais abrangentes.

Neste exercício, você explorará como usar o Copilot cloud agent para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que deseja resolver. As pessoas contratadas inicialmente para criar o site não deixaram a documentação em um estado adequado: você verá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A ideia é mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo, para que as pessoas continuem explorando. Ainda não é necessário criar uma implementação refinada; basta algo que permita testar a experiência do usuário. Essa tarefa está bloqueada no momento porque há outras issues de prioridade mais alta.

Esses são exemplos de tarefas cuja prioridade pode ser reduzida e que são ótimas para atribuir ao Copilot cloud agent. Ele pode trabalhar nelas de forma assíncrona enquanto você se concentra em outras atividades. Depois, você pode voltar para revisar o trabalho e confirmar que está de acordo com o esperado.

## Conhecer o GitHub Copilot cloud agent

O [GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa da equipe, há várias maneiras de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a tarefa, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não faz mágica: você ainda precisa pensar. Mesmo operações aparentemente simples podem se tornar complexas quando examinadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas atribuídas ao Copilot cloud agent][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a adição de IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu a atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo rapidamente. Antes de atribuir uma tarefa ao Copilot cloud agent, leia os arquivos de instruções já incluídos no repositório. Isso ajudará a identificar os efeitos deles mais adiante.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — examine a seção **Code standards**, especialmente as expectativas para convenções de TypeScript e comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Código sem documentação` ao cloud agent, na próxima seção, observe se o pull request resultante contém comentários TSDoc, segue as convenções de TypeScript e inclui comentários de cabeçalho nos arquivos. Essas características vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do Copilot cloud agent

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em boas condições. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O cloud agent usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [fluxo de trabalho de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes de o agente começar. Assim, ele tem acesso às ferramentas de desenvolvimento e dependências necessárias. A configuração já foi preparada para agilizar o laboratório e oferecer esta experiência de aprendizado. Ela fornece ao Copilot Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina apenas as permissões mínimas necessárias para estas etapas. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se precisar clonar o repositório durante as etapas de configuração — por exemplo, para instalar dependências —, use a permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend e do aplicativo - Node.js (o aplicativo agora é todo baseado em Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que os builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Assim, você pode testar se ele funciona sem esperar o Copilot iniciá-lo.

## Adicionar documentação

Todo mundo entende a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica costuma não ser resolvido, reduzindo a produtividade e dificultando a manutenção da base de código e a integração de novas pessoas à equipe. O Copilot é muito útil para criar documentação, então esta é uma issue ideal para atribuir ao Copilot cloud agent. Ele trabalhará em segundo plano para gerar a documentação necessária. Você voltará para revisar o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar a issue.
5. No campo **Title**, digite `Código sem documentação`.
6. No campo **Description**, digite:

    ```plaintext
    Nossa organização exige que funções e métodos tenham comentários de documentação TSDoc quando isso for útil. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação quando eles ajudarem a esclarecer o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, cujo título será parecido com `[WIP]: Código sem documentação`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente (executando o fluxo de trabalho descrito anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Examine a lista e as tarefas que o Copilot vai realizar.
14. Role a linha do tempo do pull request para baixo e procure uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: ele explora o código e analisa o estado, pausa para planejar a abordagem e cria o código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do Copilot cloud agent é permitir que tarefas sejam realizadas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao cloud agent e, enquanto ele trabalha, adicionaremos funcionalidades ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do GitHub Copilot cloud agent é poder dividir o trabalho: você se concentra em um conjunto de tarefas enquanto o agente cuida de outro. Mesmo que a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo, você pode usar esse tempo em outras tarefas. Vamos atribuí-la ao Copilot cloud agent.

1. Volte ao seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. No campo **Title**, digite `Mostrar jogos relacionados na página de detalhes do jogo`.
6. No campo **Description**, digite:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos, mostrando jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer alguns testes de aceitação. Estes são os requisitos:

    - Adicione um auxiliar de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de um jogo informado, excluindo o próprio jogo.
    - Mostre na página de detalhes do jogo uma seção "Related games" que use esse auxiliar.
    - Trate o caso em que um jogo não tenha jogos relacionados.
    - Crie testes de unidade para o novo auxiliar.
    - Antes de criar o PR, confirme que todos os testes passaram.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver um 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando no segundo pedido. O Copilot cloud agent atua de maneira semelhante a uma pessoa engenheira de software, então você não precisa monitorá-lo continuamente; basta revisar o resultado quando estiver pronto. Vamos passar à criação e ao uso de agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [GitHub Copilot cloud agent][copilot-agents]. Você pode atribuir issues ao cloud agent para que ele trabalhe de forma assíncrona. Use o Copilot para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o cloud agent trabalha em segundo plano, você pode criar e usar agentes personalizados. O [Copilot cloud agent também pode usar servidores MCP][cloud-agent-mcp] e as instruções personalizadas que vimos em módulos anteriores.

## Recursos

- [Sobre o Copilot cloud agent][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do Copilot cloud agent][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
