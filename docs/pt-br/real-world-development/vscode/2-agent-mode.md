---
title: "Exercício 2 - Adicione funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até atualizações simples de um aplicativo costumam exigir alterações em vários arquivos e operações como executar testes. Em geral, uma pessoa desenvolvedora precisa localizar os arquivos, fazer as alterações, testar, depurar, descobrir o que faltou e repetir o processo. O modo agente do Copilot pode ajudar nessa tarefa.

O modo agente trabalha com mais autonomia no IDE. Ele explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige problemas encontrados. Use-o para adicionar funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo agente e o agente de nuvem oferecem experiências diferentes. O modo agente trabalha no IDE, permitindo interação e ciclos rápidos de feedback. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o modo agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo agente do GitHub Copilot implementa funcionalidades na interface do usuário (UI) e na camada de dados.
- revisar as alterações e os testes gerados pelo modo agente antes de integrá-los ao código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você criou um auxiliar para editoras. Agora conclua o trabalho restante na camada de dados, na interface e nos testes usando o modo agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site atual do Tailspin Toys.

O site é uma plataforma de financiamento coletivo de jogos de tabuleiro com tema de programação. Ele permite listar jogos e exibir detalhes. É um único aplicativo Astro que renderiza páginas como HTML (Hypertext Markup Language) estático durante o build. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do Drizzle ORM; não há uma API de back-end separada nem um framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`; a interatividade usa um pequeno elemento `<script>` do Astro com APIs do Document Object Model (DOM).

### Inicie o site

O script `npm run dev` inicia o servidor de desenvolvimento Astro. Execute-o no Codespace:

1. Volte ao Codespace. Após o exercício anterior, você deve estar em `main`.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, será exibida uma mensagem com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo solicitando que você abra `http://localhost:4321` em uma janela do navegador, feche-a selecionando **x**.

5. Mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione `http://localhost:4321` no terminal para abrir o site.

> [!NOTE]
> No Codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site em execução, explore os recursos principais:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: mostra mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indicará as opções necessárias.

A primeira versão do site funciona, mas queremos adicionar recursos. Comece explorando o backlog de issues criado automaticamente quando o projeto foi criado a partir do modelo.

1. Na lista suspensa de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos do terminal e chama ferramentas.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> A inclusão desse modelo não indica preferência dos autores do workshop. Ele foi usado na criação do workshop para tentar tornar as sugestões de código mais consistentes. Como os modelos de linguagem grandes (LLMs, do inglês large language models) são probabilísticos, as sugestões podem diferir dos exemplos; isso é esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente e ainda assim concluir a tarefa.

3. Envie este prompt ao Copilot:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou as issues com base na utilidade que estima para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto. O arquivo [da interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) descreve como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades:
   - visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, permitindo reutilizar conteúdo, dividir orientações complexas em partes menores e fornecer exemplos e modelos.

## Implemente a filtragem

A filtragem exige pelo menos três alterações no aplicativo:

- adicionar ou refinar a lógica restante na camada de dados (`src/lib/`).
- adicionar ou atualizar os testes do comportamento dos filtros.
- atualizar a página que lista os jogos para incluir a interface de filtragem.

Execute os testes e confirme que foram aprovados antes de integrar as alterações. O modo agente do Copilot pode realizar essas tarefas.

1. Continue na conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista suspensa de agentes.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade conforme a issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot enquanto ele explora o projeto e localiza os arquivos relevantes da camada de dados, da interface e dos testes, modifica os arquivos e executa os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, editar arquivos e executar testes. Isso pode levar alguns minutos. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Aguarde a conclusão; então, haverá uma opção **Keep** ou **Undo** para cada alteração.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat que solicita confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Durante o processo, pode haver pausas e alguns testes podem falhar. O Copilot itera entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Sessão completa do modo agente do Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure problemas.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Você usou o modo agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o modo agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados.
- o modo agente explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar as alterações e os testes gerados antes de integrá-los ao código.

Agora, [teste a funcionalidade com o servidor Playwright do Model Context Protocol (MCP)][next-lesson] e abra um pull request.

### Exercício extra — implemente paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Use o que aprendeu para pedir ao Copilot que implemente esse recurso. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções.
- decidir se a pessoa usuária poderá selecionar o tamanho da página ou se ele será fixo.
- incluir no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Copilot nos modos ask, edit e agent: diferenças e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
