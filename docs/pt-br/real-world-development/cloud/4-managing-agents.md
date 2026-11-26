---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao Copilot cloud agent que realizasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o cloud agent trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao cloud agent, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas dos agentes que têm pull requests (PRs) abertos. Também pode examinar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do cloud agent.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois que a equipe pediu ao agente para criar um modo de alto contraste, percebeu que seria uma boa ideia adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles para alternar os modos, pareceu natural incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente tanto o modo claro quanto o alto contraste.

## Revisar as tarefas do Copilot cloud agent

Vamos conferir o estado atual das tarefas atribuídas ao Copilot cloud agent.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualização da documentação da base de código.
   - criação de APIs para modificar jogos.
   - adição de um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - preparar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - realizar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas específicas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel do pull request (PR), que aparece à direita. Nele, você pode acompanhar o PR e os arquivos alterados.

## Orientar o cloud agent

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua um controle para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando aparecerem mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Esse modo também deve ter um controle para que as pessoas possam selecionar a opção de exibição desejada.
    ```

    ![Captura de tela da tarefa do cloud agent na página de agentes com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa que estava executando quando você enviou a mensagem.

Como essa tarefa também levará alguns minutos, aproveite para pensar em tudo o que aprendeu e explorou até agora.

## Resumo e próximos passos

Você explorou a página de agentes do Copilot, o centro de controle para monitorar e orientar tarefas do GitHub Copilot cloud agent. Nela, você acompanha tarefas ativas e concluídas, revisa o trabalho em andamento e redireciona sessões para ajustar o escopo ou fornecer orientações adicionais.

Nesta lição, você:

- explorou o Mission Control e a página de agentes para monitorar tarefas do cloud agent.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados por ele][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
