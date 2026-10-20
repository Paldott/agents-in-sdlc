---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Defina agentes em arquivos Markdown na pasta `.github/agents` do repositório para fornecer instruções específicas, práticas recomendadas, padrões de código e conhecimento do domínio. Assim, a equipe pode transformar sua experiência em agentes reutilizáveis: por exemplo, um agente de acessibilidade que aplica as diretrizes de acessibilidade para conteúdo da Web (WCAG), um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA a realizar tarefas. A distinção mais simples é: o **agente personalizado** executa o trabalho; as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes. Neste exercício, o agente de acessibilidade revisa e atualiza o site com base nas diretrizes de acessibilidade; como parte do trabalho, pode chamar skills de fluxo de pull request ou de execução e gerenciamento de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona no seu ambiente e nos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai:

- explorar como os agentes personalizados são definidos.
- atribuir uma tarefa a um agente personalizado.

## Cenário

O Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao baixo contraste entre texto e fundo. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é essencial, use um agente personalizado para gerar essa funcionalidade com rapidez.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Revise seu conteúdo para entender como ele orientará o Copilot.

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que apresentam:
   - responsabilidades essenciais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Crie e atribua uma issue

O painel de agentes é o espaço central para trabalhar com agentes: nele, você pode atribuir tarefas ao agente de nuvem do Copilot, acompanhar tarefas e redirecionar ou orientar o trabalho. Atribua ao Copilot uma tarefa para criar o modo de alto contraste.

1. Abra seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

   ![Atribuição do agente de nuvem com o agente de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Você explorou os [agentes personalizados][custom-agents], assistentes de IA especializados em tarefas e domínios específicos. Eles permitem transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o trabalho do Copilot. Você:

- explorou como os agentes personalizados são definidos.
- atribuiu uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, [acompanhe e oriente a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
