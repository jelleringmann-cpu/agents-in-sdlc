---
title: "Exercício 2 - Adicionar funcionalidades com o Copilot Agent Mode"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma alteração simples em um aplicativo costuma exigir atualizações em vários arquivos e a execução de operações, como testes. O fluxo de trabalho de uma pessoa desenvolvedora geralmente envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra alteração... E assim por diante.

É aí que entra o Copilot Agent Mode.

O Copilot Agent Mode foi criado para atuar com mais autonomia no IDE. Ele funciona de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o Agent Mode para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o cloud agent foram criados para experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O cloud agent trabalha de forma assíncrona como uma pessoa da equipe e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o Copilot Agent Mode pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o GitHub Copilot Agent Mode pode implementar funcionalidades na interface e na camada de dados.
- revisar alterações e testes gerados pelo Copilot Agent Mode antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, vai concluir o trabalho restante na camada de dados, na interface e nos testes com o Copilot Agent Mode.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM no frontmatter; não há uma API de backend separada nem um framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com elementos `<script>` Astro pequenos e específicos que usam as APIs DOM padrão.

### Iniciar o site

Para facilitar a execução, há uma tarefa `npm run dev` que inicia o servidor de desenvolvimento único do Astro. Siga estas etapas para executá-lo no GitHub Codespace:

1. Volte ao Codespace. Você deve estar na branch `main` desde o exercício anterior.
2. Pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) para abrir um novo terminal no Codespace.
3. Crie uma nova branch para o trabalho de filtragem e mude para ela:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute este comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se for aberta uma caixa de diálogo perguntando se você quer abrir `http://localhost:4321` em um navegador, selecione **x** para fechá-la.

5. Para abrir o site, mantenha <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecione o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explorar o site

Quando o site estiver em execução, explore suas funcionalidades. Entre os principais recursos estão:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indica as opções relevantes.

A implementação inicial do site funciona, mas queremos melhorá-la com novos recursos. Vamos começar pelo backlog. Ao criar o projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. Na visualização Chat, selecione **Agent** no menu suspenso de agentes. O Agent planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e chama ferramentas.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos o Claude Sonnet 4.5 ao criar o workshop e, por isso, o incluímos nas instruções. A intenção é manter as sugestões relativamente consistentes e oferecer uma boa experiência. No entanto, como os modelos de linguagem são probabilísticos, as sugestões podem ser diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem, o Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie o prompt a seguir para pedir ao Copilot que mostre o backlog de issues:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los com base na utilidade para quem usa o produto.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, aproveite para revisar o arquivo que dará contexto ao Copilot. Usaremos o arquivo de [instruções da interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Examine as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de tipos de arquivo, como Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem apontar para outros arquivos de instruções e para arquivos do projeto. Os caminhos são relativos à localização do arquivo de instruções. Assim, é possível reutilizar orientações, dividir arquivos complexos em partes menores e fornecer exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de integrar as alterações à base de código. O Copilot Agent Mode pode realizar essas tarefas.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Captura de tela mostrando como selecionar o arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Na visualização Chat, confirme que **Agent** continua selecionado no menu suspenso de agentes.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria de acordo com os requisitos da issue relacionada do GitHub no backlog. O auxiliar de editoras já existe desde o exercício anterior, então preserve e refine esse trabalho conforme necessário para concluir o restante da camada de dados, da interface e dos testes. Confirme que todos os testes passaram. O servidor já está em execução, então não é preciso iniciá-lo novamente.
   ```

6. Observe como o Copilot começa explorando o projeto e localizando os arquivos relacionados à funcionalidade. Você deverá vê-lo identificar os auxiliares da camada de dados, a interface e os testes. Depois, ele começa a modificar os arquivos e executar testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot realiza várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Dependendo da complexidade da tarefa e da base de código, isso pode levar alguns minutos. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, cada alteração terá uma opção **Keep** ou **Undo**; não é necessário selecioná-las enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela mostrando uma caixa de diálogo no painel do Copilot Chat solicitando autorização para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e alguns testes podem falhar durante o processo. Não há problema: o Copilot alterna entre geração de código e testes até concluir a tarefa e não detectar erros.

   ![Captura de tela mostrando uma sessão completa do Copilot Agent Mode no Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você explorou como usar o GitHub Copilot Agent Mode para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu como:

- o GitHub Copilot Agent Mode implementa funcionalidades na interface e na camada de dados.
- o Copilot Agent Mode explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar alterações e testes gerados pelo Copilot Agent Mode antes de integrá-los à base de código.

Agora vamos [testar a funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — implementar paginação

À medida que a lista de jogos crescer, será necessário oferecer paginação. Use o que aprendeu neste exercício para pedir ao Copilot que implemente esse recurso no site. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções.
- decidir se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- incluir no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, solicitar alterações e fornecer contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
