---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publique o Space Quiz com o GitHub Pages para que ele tenha um endereço acessível ao público. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas você continua responsável por revisar as mudanças e confirmar que o site está disponível.

Nesta lição, você vai:

- pedir ao Copilot para configurar uma implantação no GitHub Pages.
- revisar o fluxo de trabalho e o pull request antes de integrá-lo.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma forma simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em um endereço público na web, identificado por um localizador uniforme de recursos (URL), sem adicionar um servidor ou uma dependência de tempo de execução (runtime).

## Peça ao Copilot para implantar o site

Para pedir ao Copilot que configure a implantação, use um prompt que solicite a criação do fluxo de trabalho, a revisão das alterações e a confirmação de que o site está no ar.

1. Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

   ```plaintext
   Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![Diagrama em três etapas: um pull request do space-quiz integrado; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz disponível em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e integre um pull request e altere as configurações do Pages do repositório. Revise o diff, as permissões de implantação e todas as solicitações de aprovação do GitHub antes de continuar.

## Revise o fluxo de trabalho e verifique a implantação

Antes de considerar a implantação concluída, revise as alterações e confirme que o site publicado está acessível.

1. Confirme que o arquivo `README` foi adicionado somente se o repositório ainda não tiver um.
2. Revise as mudanças do pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho.
3. Integre o pull request somente depois que as verificações obrigatórias forem aprovadas.
4. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com êxito.
5. Abra a URL informada e confirme que o Space Quiz carrega.
6. Se o agente não puder habilitar o Pages ou integrar o pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site publicado carregue.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relato do agente. Continue para a [revisão e os próximos passos][next-lesson].

[next-lesson]: ../13-review/
