---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu três tarefas ao agente de nuvem do Copilot para melhorar a experiência e adicionar funcionalidades. Embora ele trabalhe de forma autônoma e assíncrona, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente, entre elas a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você vê as tarefas com pull requests (PRs) abertos, explora as operações realizadas e orienta sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe percebeu que também seria útil oferecer um modo claro. Como o agente já está atualizando o estilo do site e adicionando controles, peça que ele inclua o modo claro junto com o modo de alto contraste.

## Revise as tarefas do agente de nuvem

Confira o estado das tarefas atribuídas ao agente de nuvem do Copilot.

1. Abra a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas nas áreas principal e lateral. A lista deve incluir:
   - atualização da documentação do código.
   - geração de APIs para modificar jogos.
   - adição do modo de alto contraste ao site.
3. Selecione uma tarefa em execução e revise as operações do Copilot. Elas podem incluir:
   - clonar o código do repositório.
   - configurar o ambiente de trabalho.
   - preparar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho e a abordagem adotada pelo Copilot.

4. Observe também o painel de pull request à direita, que permite acompanhar o PR e os arquivos alterados.

## Oriente o agente de nuvem

Peça ao Copilot que adicione o controle do modo claro enquanto implementa o modo de alto contraste.

1. Selecione a sessão relacionada à implementação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos até que a configuração esteja concluída e o agente comece o trabalho. Você verá mensagens indicando esse progresso.
3. Na caixa de diálogo **Steer active session while Copilot is working**, digite:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem com a caixa de diálogo de orientação da sessão ativa](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot reconhece o pedido e o incorpora ao fluxo de trabalho.

## Deixe o Copilot trabalhar

O Copilot incorporará a solicitação atualizada ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Como o processo pode levar alguns minutos, aproveite a pausa para refletir sobre o que aprendeu e explorou.

## Resumo e próximos passos

Você explorou a página de agentes do Copilot, o espaço central para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. O painel permite monitorar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento. Você:

- explorou o painel central e a página de agentes para acompanhar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui as funcionalidades de acessibilidade, [itere sobre os pull requests criados pelo Copilot][next-lesson].

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
