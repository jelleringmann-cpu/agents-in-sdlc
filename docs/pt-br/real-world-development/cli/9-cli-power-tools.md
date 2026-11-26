---
title: "Lição 9 - Comandos de barra do GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar contexto, selecionar modelos, compartilhar sessões e, opcionalmente, delegar tarefas."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra. Eles dão acesso a recursos avançados, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para conhecer os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho em paralelo, worktrees e delegação ao agente na nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal da CLI. Agora vamos conhecer alguns recursos adicionais: gerenciamento de contexto, troca de modelos, compartilhamento de sessões e delegação opcional ao [Copilot cloud agent][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Ao trabalhar em tarefas maiores ou mais complexas, você pode atingir o limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, e você pode inspecionar ou compactar o contexto com comandos de barra.

1. Volte ao Codespace e, se o Copilot CLI ainda não estiver aberto, inicie-o na raiz do repositório.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e como o contexto está dividido entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. Talvez não haja uma grande diferença se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela está ficando cheia. Use `/compact` para escolher quando fazer isso. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação com a atual e quiser iniciar uma conversa limpa.

## Escolher um modelo

Cada modelo tem pontos fortes diferentes, e cada pessoa desenvolvedora tem suas preferências. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todos a usar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e confira as opções de `/share` na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino mais adequado às suas necessidades, como `/share file` para exportá-la como um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretenda compartilhar.

## Opcional: executar tarefas em paralelo ou delegar

O fluxo principal do workshop foi concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir tarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar um worktree Git isolado para uma tarefa separada.
- `/delegate` pode enviar uma tarefa ao Copilot cloud agent, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Comece uma tarefa nova e bem definida antes de experimentá-los e revise os resultados pelo fluxo de trabalho habitual. Para saber mais sobre o trabalho assíncrono com agentes, continue com o [workshop do agente na nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra do Copilot CLI permitem configurar a ferramenta, compartilhar sessões e conferir o que está acontecendo nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usou `/model` para conhecer os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho em paralelo, worktrees e delegação ao agente na nuvem.

Há muitos outros comandos de barra e recursos para conhecer no Copilot CLI. Vamos encerrar nossa jornada [revisando o que aprendemos][next-lesson] e os próximos passos para continuar aprendendo.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
