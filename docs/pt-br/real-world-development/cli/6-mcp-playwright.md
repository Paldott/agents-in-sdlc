---
title: "Lição 6 - Valide a funcionalidade com o Playwright MCP"
description: "Confira ou configure o Playwright MCP no Copilot CLI e use-o para explorar a filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Escrever código envolve mais do que criar código: também é preciso trabalhar com dados, serviços externos e automações disponíveis para o Copilot. Os servidores MCP ampliam os recursos integrados ao CLI, fornecendo acesso a ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor Playwright MCP se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador para explorar a filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações de interface também exige interação com ela. Com o Playwright MCP, o Copilot pode usar o site como uma pessoa usuária e ajudar a verificar o comportamento.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Um servidor MCP atua como uma ponte entre o agente e essas ferramentas e serviços; cada servidor disponibiliza um conjunto de ferramentas e recursos.

Alguns servidores MCP conhecidos são:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta e a contribuição para esse ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, pule estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o comando na caixa **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot que explore a funcionalidade com Playwright

Você já confirmou manualmente que a funcionalidade funciona. Agora use o servidor Playwright MCP disponível no Copilot para repetir a validação.

1. Use este prompt para pedir que o Copilot valide a funcionalidade com o Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente, o Copilot identifica sozinho quando deve usar o servidor MCP. Ainda assim, mencionar a ferramenta pode ajudar a orientar a tarefa.

2. Acompanhe as etapas que o Copilot executa no navegador.
3. Leia o relatório e confirme se o comportamento está correto.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximos passos

Você usou o servidor Playwright MCP para explorar a funcionalidade em um navegador real pelo Copilot CLI. Você:

- entendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor Playwright MCP se ele ainda não estava disponível.
- pediu ao agente que controlasse um navegador para explorar a filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] que reúna a skill e as ferramentas do navegador.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
