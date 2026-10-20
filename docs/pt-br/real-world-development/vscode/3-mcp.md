---
title: "Exercício 3 - Teste a funcionalidade com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor Playwright MCP** e peça ao Copilot que controle um navegador real; depois, publique a branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor Playwright MCP ao workspace.
- pedir ao Copilot que o use para testar manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo agente fica mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é um padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento do modo agente e de sua interação com contexto, modelos de linguagem grandes (LLMs) e ferramentas, incluindo as fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Com recursos, eles acessam informações atualizadas; com ferramentas, executam ações em seu nome.

Um servidor MCP faz a ponte entre o agente e as ferramentas e serviços externos e gerencia a comunicação, inclusive com interfaces de programação de aplicativos (APIs) existentes ou ferramentas locais, como pacotes NPM. Cada servidor oferece um conjunto próprio de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub, criar repositórios, atualizá-los e gerenciar issues e pull requests.
- **[Servidor Playwright MCP](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright, como navegar em páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição para esse ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique o publicador e avalie as implicações de segurança. Use apenas servidores confiáveis e tenha cautela ao conceder acesso a recursos ou operações sensíveis.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Confirme que já existe um servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

O servidor `github` vem com o modelo do projeto. Por isso, o Copilot pôde ler o backlog de issues no exercício anterior. Ele usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]; não é necessário instalá-lo localmente. O VS Code se conecta usando o protocolo de transferência de hipertexto (HTTP), e você autentica na primeira utilização de uma ferramenta.

## Adicione o servidor Playwright MCP

Adicione um segundo servidor. O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` junto à entrada `github`, de modo que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code inicia-o na máquina executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem janela visível, o que é necessário em um Codespace sem área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright em testes de ponta a ponta; por isso, o navegador geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**. Na primeira vez que o Copilot precisar de suas ferramentas, o VS Code iniciará o servidor e solicitará confirmação de confiança.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme que confia nele.

Iniciar o servidor manualmente é opcional. Caso pule essas etapas, o VS Code iniciará o servidor e pedirá sua confirmação na primeira utilização das ferramentas pelo Copilot.

## Teste a filtragem

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a funcionalidade no navegador. Ele pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou.

O servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página, e não capturas de tela. Assim, o agente interpreta elementos estruturados e rotulados — botões, links e itens de lista — de forma semelhante a uma tecnologia assistiva. Uma validação funcional rápida também oferece uma verificação básica de acessibilidade.

Envie este prompt para exercitar a filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador usando o Playwright MCP, executará as etapas e relatará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo não estiver correto, faça perguntas de acompanhamento ou peça que o Copilot corrija o código antes de abrir um pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor de desenvolvimento, inicie-o antes de enviar o prompt. Na primeira utilização do Playwright MCP, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Depois de confirmar que a funcionalidade funciona, publique a branch `filtering-vscode` para que a equipe possa revisar o trabalho.

1. Abra o painel **Source Control** no Codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Peça ao Copilot, que já está conectado às ferramentas do GitHub, que localize a issue correspondente e crie o PR associado a ela.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. Na lista suspensa de agentes, confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot colete informações e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar sua conta; siga as instruções.
5. Observe o Copilot procurar a issue correta e criar o PR.
6. Selecione o link fornecido pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor Playwright MCP ao workspace.
- usou o servidor para testar manualmente a filtragem no navegador antes da entrega.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma maneira de ampliar os recursos do Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
