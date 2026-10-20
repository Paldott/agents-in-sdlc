---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica ou há informações que o Copilot precisa conhecer, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] são uma das principais formas de fornecer orientações para a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos aplicados a caminhos específicos.
- gerar a primeira parte dos dados da filtragem (um auxiliar para editoras) com as instruções atuais.
- adicionar um padrão geral a `.github/copilot-instructions.md`.
- repetir o mesmo prompt e observar o código gerado seguir o novo padrão.
- fazer commit das instruções e da parte de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é não determinístico. O objetivo é observar a tendência das mudanças de comportamento após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

O Tailspin Toys tem diretrizes para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter aparência moderna.
- O código deve incluir documentação em comentários TSDoc.
- Cada arquivo deve começar com um bloco de comentários que explique sua finalidade.

Use arquivos de instruções para informar esses padrões ao Copilot.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a entender o estilo e os requisitos do código e a gerar sugestões mais relevantes. Elas podem especificar convenções, bibliotecas e estilos de comentários. Você pode criar instruções gerais para todo o repositório ou específicas para determinados tipos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em todas as solicitações do repositório. Inclua nele informações gerais do projeto, como a pilha de tecnologias, uma visão geral, práticas recomendadas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados para geração de código no Copilot Chat, mas não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` durante a geração de código.
>
> O Copilot também [aceita arquivos de instruções em outros formatos][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto mostram uma abordagem possível:

- Mantenha `copilot-instructions.md` focado em orientações gerais, como a descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou de tarefas específicas.
- Use linguagem natural, mantenha as orientações claras e inclua exemplos de como o código deve ou não deve ser escrito.

Não existe uma única maneira de criar arquivos de instruções ou de usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Projetos que usam o GitHub Copilot devem ter um conjunto consistente de arquivos de instruções. Neste projeto, há arquivos para várias tarefas, como [alterações na interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de maneira diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição da interface em uso apresenta a opção quando pertinente.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções incluídos no projeto pelo editor ou pela interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para **GitHub Actions Workflows**. Elas se aplicam às interações com o Copilot.
3. Explore a pasta `.github/instructions`. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início do arquivo. Ele define um padrão glob relativo à raiz do repositório; arquivos de teste TypeScript que correspondem a `**/*.test.ts` recebem essas instruções.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Isso permite dividir instruções maiores em arquivos reutilizáveis menores e apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Crie uma branch para as alterações

Crie uma branch para manter as alterações deste exercício organizadas.

1. Volte ao Codespace do exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes dos próximos exercícios, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indica as opções relevantes.

Para observar o efeito das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, atualize o arquivo e envie o mesmo prompt novamente.

1. Feche as guias de editor abertas nos exercícios anteriores para que o Copilot receba apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Na lista suspensa de agentes da visualização Chat, selecione **Agent** para permitir que o Copilot altere arquivos.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, geralmente ao arquivo auxiliar e aos testes.
6. Observe que a função tipada proposta recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essa convenção vem de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Isso é aceitável; o objetivo é observar a consistência após atualizar as instruções.

## Adicione um padrão ao repositório

O arquivo `.github/copilot-instructions.md` fornece informações gerais ao Copilot. Atualize os padrões do repositório para orientar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões, mas ainda não exige documentação no código.
3. Logo abaixo dos padrões existentes, adicione estas linhas para orientar a inclusão de cabeçalhos de arquivo e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como no exercício anterior, instruções gerais ficam em `.github/copilot-instructions.md`; arquivos `*.instructions.md` são adequados para linguagens, tipos de arquivo ou tarefas específicas. O arquivo no nível do repositório é apropriado para padrões gerais, como a regra de comentários de documentação.

## Repita o prompt e observe a diferença

Envie o mesmo prompt após atualizar as instruções para comparar a sugestão do Copilot.

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o buffer.
2. Selecione `src/lib/publishers.ts` para que o Copilot use o arquivo como contexto.
3. Envie o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código que servirá de base para o próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são controlados pelo mesmo sistema de controle de versão que os outros arquivos do repositório. Faça commit e envie a branch.

1. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme que as instruções e as alterações do auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e faça o merge de um pull request

Crie um pull request associado à issue do backlog sobre o padrão de documentação. O Copilot pode localizar a issue, criar o PR e fechá-la após o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub no workspace. Na primeira utilização, o VS Code talvez peça que você entre na conta do GitHub; siga as instruções para autorizar. Você aprenderá como essa conexão funciona, por meio do Model Context Protocol, em um exercício posterior.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que localize a issue sobre a atualização do arquivo de instruções e crie um PR a partir da branch atual, descrevendo as instruções e a base do auxiliar para editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot localizará a issue e criará o PR.
5. Quando solicitado a permitir ações do GitHub, revise o comando e selecione **Allow** conforme apropriado.
6. Depois da criação do PR, peça ao Copilot que faça o merge em `main`, volte à branch local `main` e baixe o código atualizado:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir ações do GitHub e comandos do shell, revise o comando e selecione **Allow** conforme apropriado.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um padrão geral a `.github/copilot-instructions.md`.
- repetir o prompt e observar o código proposto adotar o novo padrão.
- fazer commit das instruções e da base do auxiliar em `main` para que o próximo exercício possa usá-la.

Em seguida, aplique as instruções no [modo agente][next-lesson] enquanto o Copilot adiciona uma funcionalidade ao projeto.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot: coleção de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
