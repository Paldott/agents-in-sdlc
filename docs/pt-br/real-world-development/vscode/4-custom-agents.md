---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Defina agentes em arquivos Markdown na pasta `.github/agents` do repositório para fornecer instruções específicas, práticas recomendadas, padrões de código e conhecimento do domínio. Assim, a equipe pode transformar sua experiência em agentes reutilizáveis: por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA a realizar tarefas. A distinção mais simples é: o **agente personalizado** executa o trabalho; as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes. Neste laboratório, o agente de acessibilidade revisa e atualiza o site segundo as diretrizes de acessibilidade; como parte do trabalho, pode chamar skills de fluxo de pull request ou de execução e gerenciamento de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Teste e itere para encontrar o que funciona no seu ambiente e nos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no modo agente do Copilot Chat para implementar um modo de alto contraste.

## Cenário

O Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao baixo contraste entre texto e fundo. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é essencial, use um agente personalizado para gerar essa funcionalidade.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Revise o conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace e, no terminal, crie uma branch nova a partir de `main` para manter o trabalho de acessibilidade separado do PR de filtragem do Exercício 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que apresentam:
   - responsabilidades essenciais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra na lista suspensa de agentes do painel Copilot Chat todos os agentes personalizados definidos em `.github/agents`. Ao selecionar um deles, a sessão de chat usa as instruções e ferramentas desse agente.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes dos próximos exercícios, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indica as opções necessárias.

1. Na lista suspensa de agentes da visualização Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra a lista suspensa de agentes na parte inferior do painel Chat. Ela mostra o agente ativo, que por padrão é **default**.
3. Selecione **Accessibility agent** na lista.
4. Envie este prompt ao agente de acessibilidade:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada alteração aparecerá no chat com o caminho do arquivo e um diff para revisão.
6. A indicação **Files changed** será atualizada à medida que o agente modificar arquivos.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot fará alterações reais no repositório, como editar componentes Astro, CSS e testes relacionados.

No próximo exercício, você revisará e orientará esse trabalho em andamento.

## Resumo e próximos passos

Você explorou os [agentes personalizados][custom-agents], assistentes especializados em tarefas e domínios específicos. Eles permitem transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o trabalho do Copilot. Você:

- revisou um agente personalizado de acessibilidade existente.
- usou um agente personalizado no modo agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações durante a execução e adicione um controle de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar a funcionalidade com o servidor Playwright MCP][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
