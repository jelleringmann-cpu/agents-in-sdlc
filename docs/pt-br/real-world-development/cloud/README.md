---
slug: pt-br/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O [**GitHub Copilot cloud agent**](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui uma tarefa no GitHub, e o cloud agent a executa em segundo plano — explora o repositório, faz alterações e abre um pull request — enquanto você se dedica a outras atividades.

Neste conjunto de exercícios, você adicionará instruções personalizadas para o cloud agent e, em seguida, atribuirá uma issue do GitHub para ele implementar o trabalho. Você revisará e usará agentes personalizados para orientar a abordagem, monitorará e direcionará sessões pelo painel de agentes e, por fim, revisará os pull requests e iterará sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e Codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione instruções personalizadas para o cloud agent seguir |
| [2. Cloud agent][ex2] | Agente assíncrono | Atribua issues ao Copilot cloud agent |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Monitore e oriente sessões de agentes |
| [5. Iteração][ex5] | Revisão | Revise PRs, itere sobre o trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, confirme que você tem:

- [ ] Uma conta do GitHub com um plano **Copilot Student, Pro, Pro+, Business ou Enterprise** ativo
- [ ] O Copilot cloud agent ativado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem usar o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e os recursos do Copilot CLI usados neste workshop, permitindo concluir todos os percursos.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O cloud agent está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — todos, exceto o Copilot Free. No Copilot Business ou Enterprise, um administrador precisa ativá-lo para a organização.

> [!NOTE]
> Este percurso não aborda MCP. Para aprender a usar servidores MCP com o Copilot, consulte o [percurso do CLI][cli] ou o [percurso do VS Code][vscode].

## Começar

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
