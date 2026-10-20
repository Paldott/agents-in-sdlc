---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo agente, o GitHub Copilot trabalha com autonomia: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina, em tempo real, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar cada diff em linha, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento, sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar um controle de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design solicitou também um modo claro para pessoas que consideram o tema padrão escuro demais. Em vez de iniciar uma nova conversa, oriente a sessão atual para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e alterações propostas. Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar as chamadas de ferramentas, leituras e edições de arquivos e comandos de terminal que ele solicitar.
3. Quando o agente propuser uma edição, o diff aparecerá no chat:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para revertê-la e permitir outra tentativa do agente.
4. Para ver todos os arquivos modificados na sessão, abra o indicador **Files changed** na parte superior do chat.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão, por exemplo, para executar um comando de terminal, o agente será pausado e mostrará a opção **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde até que o agente conclua o modo de alto contraste. Ele exibirá uma mensagem final resumindo as alterações.

## Oriente o agente para adicionar o modo claro

Depois da implementação do modo de alto contraste, amplie a mesma conversa para incluir um controle de modo claro. Como você continuará a mesma sessão, o agente manterá o contexto dos componentes, da abordagem de estilo e do padrão de armazenamento local usados anteriormente.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente examinará as alterações existentes e proporá as inclusões para o modo claro.
3. Revise cada nova edição quando aparecer, usando **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de uma maneira inadequada, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma orientação mais clara.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione atalhos de teclado para os dois controles").

## Revise o conjunto de alterações

Antes do commit, revise tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Ela deve incluir componentes Astro, estilos e testes relacionados, se houver.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se a implementação segue as orientações de acessibilidade do agente personalizado, como atributos de *Accessible Rich Internet Applications* (ARIA, aplicações de Internet ricas e acessíveis), navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você publicará o trabalho na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo agente do Copilot Chat para acompanhar e orientar uma sessão em andamento, revisando cada edição, aceitando ou rejeitando alterações e enviando prompts de acompanhamento sem perder o contexto. Você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade em um pull request e revise as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
