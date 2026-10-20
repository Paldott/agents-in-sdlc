---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica ou há informações que o Copilot precisa conhecer, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não apenas *o que* fazer, mas também *como* fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos aplicados a caminhos específicos.
- adicionar um padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes VS Code e CLI, aqui você não comparará prompts antes e depois da alteração. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, o que dificulta uma demonstração lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests criados pelo agente.

## Arquivos de instruções

### Cenário

O Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter aparência moderna.
- O código deve incluir documentação em comentários TSDoc.
- Cada arquivo deve começar com um bloco de comentários que explique sua finalidade.

Use arquivos de instruções para informar esses padrões ao Copilot.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a gerar sugestões mais relevantes. Elas podem especificar convenções de código, bibliotecas e estilos de comentários. Você pode criar instruções gerais para o repositório ou instruções específicas para certos tipos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Inclua nele informações gerais pertinentes à maioria das solicitações de chat ou CLI, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e orientações globais.
- `.github/instructions/*.instructions.md`: arquivos para tarefas ou tipos de arquivo específicos. Eles podem orientar linguagens como TypeScript ou Astro, ou tarefas como criar um componente de interface ou testes unitários.

> [!NOTE]
> Em um IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` durante a geração de código.
>
> O Copilot também [aceita arquivos de instruções em outros formatos][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

O projeto de exemplo ilustra uma abordagem possível:

- Mantenha `copilot-instructions.md` focado em orientações gerais do projeto, como a descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para orientar tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou tarefas específicas.
- Use linguagem natural, mantenha as orientações claras e inclua exemplos de como o código deve ou não deve ser escrito.

Não existe uma única maneira de criar arquivos de instruções ou de usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Projetos que usam o GitHub Copilot devem ter um conjunto consistente de arquivos de instruções. Neste projeto, há arquivos para várias tarefas, como [alterações na interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de maneira diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição da interface em uso apresenta essa opção quando pertinente.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções incluídos no repositório pelo GitHub ou no Codespace, se ele já estiver em execução.

1. Abra `.github/copilot-instructions.md`.
2. Explore a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para **GitHub Actions Workflows**. Elas se aplicam às interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos, incluindo as instruções para Astro, a camada de dados Drizzle e testes.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início do arquivo. Ele define um padrão glob relativo à raiz do repositório. Por exemplo, arquivos de teste TypeScript que correspondem a `**/*.test.ts` recebem essas instruções.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Isso ajuda a dividir instruções maiores em arquivos reutilizáveis menores e a apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções, e não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicione um padrão para o repositório

Adicione uma regra geral para que a documentação fique no código, em comentários TSDoc e em um cabeçalho de comentário no nível do arquivo. O agente de nuvem usará essa orientação ao trabalhar em issues atribuídas nas próximas etapas.

Antes de editar o arquivo, crie uma branch. O agente de nuvem lê as instruções da branch de destino da issue; mesmo assim, faça commit das alterações de forma organizada.

1. No terminal do Codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra novamente `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, perto da linha 27. Ela documenta os padrões de código, mas ainda não exige documentação no código.
4. Logo abaixo dos padrões existentes, adicione estas linhas para orientar o Copilot a criar cabeçalhos nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Os arquivos de instruções podem ficar na raiz do repositório, em `.github/copilot-instructions.md`, para fornecer orientações globais, ou em `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é adequado para padrões gerais, como a regra de comentários de documentação adicionada aqui.

## Faça commit, push e merge da atualização de instruções

O agente de nuvem lê as instruções da branch de destino da issue. No próximo exercício, ele criará uma branch com base em `main`; portanto, as instruções precisam ser integradas a `main` para que o agente as use.

1. Prepare e faça commit da alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. O agente de nuvem usará as instruções quando trabalhar nas issues do próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, ignore as etapas de branch e faça commit diretamente em `main`. A criação da branch demonstra o fluxo usado em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções e adicionou um padrão geral a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests gerados pelo agente de nuvem ao longo deste ambiente.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: coleção de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
