---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o app GitHub Copilot para configurar um workflow do Pages, revisar o pull request e verificar se o Space Quiz está no ar."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publique o Space Quiz com o GitHub Pages para que qualquer pessoa possa acessá-lo. O Copilot pode preparar o workflow e orientar a publicação, mas você continua responsável por revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot que configure a publicação no GitHub Pages.
- revisar o workflow e o pull request antes de fazer o merge.
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de testá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar um servidor ou uma dependência de runtime.

## Peça ao Copilot para publicar o site

Na sessão do app Copilot para `space-quiz`, envie este prompt:

```plaintext
Habilite o GitHub Pages para este repositório. Crie um workflow do GitHub Actions que compile o site se necessário e faça a implantação no GitHub Pages. Adicione um README com a descrição do app e as instruções para executá-lo localmente, caso ainda não exista. Crie um pull request com essas alterações e faça o merge depois que as verificações obrigatórias forem aprovadas. Acompanhe a implantação até que ela seja concluída e, em seguida, informe a URL final do site. Se não tiver permissão para habilitar o Pages ou fazer o merge, informe exatamente o que preciso aprovar ou fazer; não declare sucesso até que o site esteja no ar.
```

![Diagrama em três etapas: pull request do space-quiz mesclado; execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e Space Quiz disponível em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede que o agente crie e mescle um pull request e altere as configurações do Pages no repositório. Antes de continuar, revise o diff, as permissões de implantação e todas as solicitações de aprovação do GitHub.

## Revise o workflow e verifique a publicação

Revise as alterações do pull request e confirme que a implantação foi concluída antes de considerar o site publicado.

1. Confirme que o README só foi adicionado se o repositório ainda não tinha um.
2. Revise as alterações do pull request, incluindo as etapas de build e implantação do workflow.
3. Faça o merge somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com sucesso.
5. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou mesclar o pull request, siga as instruções específicas fornecidas. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site esteja no ar.

## Resumo e próximas etapas

Agora o Space Quiz está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relato do agente. Continue para a [Lição 13: revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
