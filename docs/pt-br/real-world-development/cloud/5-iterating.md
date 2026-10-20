---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma seção de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo controles de alto contraste e modo claro. Agora revise as alterações e, se necessário, envie comentários para aprimorar o trabalho.

### Cenário

Os fundamentos do projeto de software e do DevOps não mudam com a IA generativa. Sempre revise o código gerado e siga o processo DevOps habitual. Revise as propostas do GitHub Copilot para a documentação, a seção de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão contínua, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O agente é executado no GitHub Actions, em um ambiente separado e temporário.
- Um ser humano precisa aprovar qualquer workflow do GitHub Actions antes de sua execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive a servidores MCP.

## Revise a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem: a documentação do código. Use a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Talvez o PR exiba um aviso de que o Copilot está bloqueado por um firewall. Isso **é esperado**: por padrão, o Copilot tem acesso limitado a recursos externos, incluindo servidores MCP. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o título do PR incluirá a marca **[WIP]**. Aguarde o término; isso pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários TSDoc e outras documentações. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções revisados no início do Exercício 2.
6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão entrar na fila da seção de verificações do PR. Se tudo correr bem, as verificações do projeto da aplicação Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request pode ser iterativo. Você pode mencioná-lo em comentários do PR ou em comentários em linha no código, como faria com outra pessoa da equipe. O Copilot verá os comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um prompt único prescrito. Você pode pedir, por exemplo, que o Copilot:

- adicione ao início de cada arquivo de código um cabeçalho que explique brevemente sua finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**. Use uma das sugestões anteriores ou outra melhoria que queira ver no código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Ele iniciará outra sessão para fazer as alterações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar, haverá um novo commit no PR.
5. Selecione **Files changed** e revise as alterações.

Continue iterando até ficar satisfeito. Em seguida, você pode marcar o PR de rascunho como pronto para revisão e fazer o merge para a branch principal.

![Marcar PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Volte ao PR criado pelo Copilot para resolver a issue sobre exibir jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** e revise o código gerado.
5. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
6. Procure um indicador de workflows aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão entrar na fila da seção de verificações do PR. Se tudo correr bem, as verificações do projeto da aplicação Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** para testar manualmente a funcionalidade, mude para a branch do agente no Codespace. Abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou a seção de jogos relacionados. Você pode continuar iterando com o agente de nuvem e pedir alterações, como ajustar a quantidade de jogos exibidos ou adicionar cabeçalhos de comentários e comentários TSDoc. Lembre-se de que essa issue foi atribuída antes da atualização das instruções personalizadas. Para solicitar mudanças, adicione um comentário na guia **Conversation**; o Copilot o verá e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. O PR deve conter o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado pelo painel de agentes no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o título do PR conterá **[WIP]**. Aguarde alguns minutos até a conclusão.

4. Selecione **Files changed** para revisar o código gerado.
5. Examine a implementação, prestando atenção a:
   - componentes de interface dos controles para alternar entre modos.
   - uso do armazenamento local para preservar as preferências.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância de modos.
6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão entrar na fila da seção de verificações do PR. Se tudo correr bem, as verificações do projeto da aplicação Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** para testar manualmente os recursos de acessibilidade, mude para a branch no Codespace, abra o terminal e execute estes comandos, substituindo `<branch-name>` pela branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Depois, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que funcionam e que as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade orientou o Copilot na implementação de acordo com práticas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem é útil para tarefas reais do backlog enquanto você se dedica a outras atividades. Para praticar, crie algumas issues no seu repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário para demonstrar interesse como apoiador na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entradas e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o ambiente do agente de nuvem. Ao longo das lições, você:

- **inspecionou os arquivos de instruções personalizados** do repositório para observar sua influência no agente de nuvem.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** de acessibilidade para adicionar controles de alto contraste e modo claro.
- **usou a página de agentes do Copilot como painel central** para acompanhar e orientar a sessão de acessibilidade.
- **revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o ambiente do agente de nuvem. Se quiser continuar, os outros ambientes complementam o que praticou:

- 🖥️ **[Ambiente VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Ambiente CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
