---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem no navegador. Agora vai reunir esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e examinar um perfil reutilizável de garantia de qualidade (QA).
- selecionar o agente de QA e revisar as conclusões dele em relação à issue de filtragem.

## Cenário

A Tailspin Toys quer revisar de forma consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações que você usou têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O MCP do Playwright fornece ferramentas de navegador. O perfil de QA informa ao Copilot como usar esses recursos para avaliar os requisitos e relatar as conclusões. Ele não substitui esses recursos nem exige outra conversa.

## Criar o perfil de QA

Antes de abrir o PR da funcionalidade, peça ao Copilot que crie um perfil de QA reutilizável. O perfil definirá tanto as verificações quanto os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar as funcionalidades de acordo com as issues e os requisitos definidos, seguir as instruções do repositório, executar a skill quality-checks, usar o MCP do Playwright para validar o comportamento e adicionar testes quando a cobertura estiver incompleta.

   Peça que ele classifique cada requisito como aprovado, reprovado ou bloqueado e apresente evidências. Ele deve pedir autorização antes de alterar o código da implementação e não pode fazer commit de alterações nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Examinar o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot definiu o fluxo de QA e os limites de autoridade esperados. Assim, um agente incompleto ou com permissões amplas demais não alterará a funcionalidade quando você só quiser verificá-la.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme que o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme que o QA apresenta evidências, pergunta antes de alterar o código da implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou limites, peça ao agente padrão que o revise antes de continuar.

## Executar o QA em relação à issue

O Copilot CLI carrega os agentes do projeto quando uma conversa é iniciada. Retome a mesma conversa de filtragem depois de criar o perfil e, em seguida, selecione QA para que o agente tenha acesso à issue e às decisões de planejamento já presentes no contexto.

1. Na conversa atual, abra o seletor de agentes na caixa do prompt.
2. Selecione **QA** e confirme que o aplicativo mostra **QA** como o agente ativo antes de enviar o prompt.
3. Envie este prompt para solicitar a revisão da funcionalidade:

   ```plaintext
   Revise a funcionalidade de filtragem de acordo com a issue e as decisões do nosso plano. Ela está pronta para um PR?
   ```

4. Confirme que o QA está usando a issue e as decisões corretas do planejamento. Se o agente solicitar, forneça a URL da issue ou qualquer contexto ausente.
5. Quando o trabalho terminar, leia o relatório.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o resultado. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e examinou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as conclusões dele em relação à issue de filtragem.

Agora, a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação estão prontos para revisão. Em seguida, você [reunirá esses itens em um PR da funcionalidade e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
