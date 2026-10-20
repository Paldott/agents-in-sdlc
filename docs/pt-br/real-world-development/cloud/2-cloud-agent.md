---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É difícil encontrar organizações que não enfrentem dívida técnica: problemas de segurança ainda não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao terminar, ele cria um pull request (PR) de rascunho para que uma pessoa desenvolvedora revise o trabalho. Assim, tarefas tediosas podem ser delegadas e a equipe pode se concentrar em atividades de maior impacto.

Neste exercício, você vai explorar:

- a personalização do ambiente de geração de código.
- a execução segura das operações.
- a importância de delimitar bem as issues.
- a atribuição de issues ao Copilot.

## Cenários

O Tailspin Toys quer resolver uma dívida técnica: o código do site está sem documentação. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

A equipe de design também quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar outros títulos da mesma categoria, para que as pessoas continuem explorando o catálogo. Ainda não é necessário polir a implementação; a equipe quer algo para testar a experiência de usuário (UX) e a aceitação. Esse trabalho está bloqueado, mas há issues de maior prioridade.

São tarefas que podem ser adiadas e que são adequadas para o agente de nuvem. Ele pode trabalhar de forma assíncrona enquanto você se dedica a outras atividades; depois, você revisa o resultado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) trabalha em segundo plano de forma semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um PR de rascunho para acompanhar o progresso, configura um ambiente e começa o trabalho. Você pode examinar a sessão durante a execução ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no PR.

## A importância de instruções bem delimitadas

O GitHub Copilot não faz o trabalho de pensar por você. Até operações aparentemente simples podem ter complexidade. Por isso, [delimite com cuidado as tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: divida o trabalho em etapas, aprenda, experimente e adapte o processo. A IA generativa não muda os fundamentos do desenvolvimento de software.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu um exercício prático sobre esse assunto, conhece `.github/copilot-instructions.md`; caso contrário, aproveite para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções incluídos no repositório, sem alterá-los, para identificar seus efeitos mais adiante.

Abra estes arquivos na interface Web do GitHub ou em um Codespace que já esteja em execução:

- `.github/copilot-instructions.md`: revise a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Na próxima seção, ao atribuir ao agente de nuvem a issue `Code lacks documentation`, observe se o PR resultante contém comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções e serão destacadas novamente ao revisar PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

A criação de código geralmente exige um ambiente específico e a execução de scripts de configuração. O mesmo vale quando você atribui uma tarefa ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa [GitHub Actions][github-actions] para configurar o ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml` e executado antes de o agente começar. O workflow pré-configurado para este laboratório instala o Node.js e as dependências do projeto, os binários do navegador para testes de ponta a ponta e prepara o banco de dados SQLite migrado e preenchido da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Caso contrário, o Copilot fará isso automaticamente depois que as etapas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com outros workflows do GitHub e tem dois pontos importantes:

- Ele contém um único trabalho, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no PR.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar que o Copilot a execute.

## Adicione documentação ao código

Muitos projetos têm informações desatualizadas ou carecem de documentação, o que aumenta a dívida técnica, reduz a produtividade e dificulta a manutenção e a integração de novas pessoas à equipe. O Copilot pode ajudar a gerar documentação, tornando este trabalho adequado para o agente de nuvem. Você voltará para revisar o resultado em um exercício futuro.

1. Em uma nova guia do navegador, abra o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o PR recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente por meio do workflow descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o agente pretende realizar.
14. Role a linha do tempo do PR até encontrar uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Percorra a sessão ao vivo para acompanhar o Copilot explorando o código e o estado do projeto, planejando a abordagem e criando código.

Esse processo pode levar alguns minutos. Um dos objetivos do agente de nuvem é realizar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Aproveite esse recurso: atribua outra tarefa ao agente e, enquanto ele trabalha, implemente uma funcionalidade no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é dividir o trabalho: você pode se concentrar em uma tarefa enquanto ele executa outra. A seção de jogos relacionados pode liberar tempo para outras atividades, então atribua essa tarefa ao agente.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em pouco tempo, o primeiro comentário da issue deverá mostrar o emoji de olhos 👀, indicando que o Copilot está trabalhando.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O agente está trabalhando na segunda solicitação. Como ele opera de forma semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente; revise o resultado quando terminar. Agora, crie e use agentes personalizados.

## Resumo e próximos passos

Você explorou o [agente de nuvem do GitHub Copilot][copilot-agents], que permite atribuir issues ao Copilot para execução assíncrona. Ele pode resolver dívida técnica, criar funcionalidades ou ajudar a migrar código entre frameworks. Você:

- personalizou o ambiente de geração de código.
- entendeu como executar operações com segurança.
- aprendeu a importância de delimitar bem as issues.
- atribuiu issues ao Copilot.

Enquanto o agente trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e recebe instruções personalizadas, que foram apresentadas em exercícios anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
