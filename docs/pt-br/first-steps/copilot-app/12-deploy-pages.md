---
title: "Lição 12 - Publicar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um workflow do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o workflow e orientar a publicação, mas você continua responsável por revisar as mudanças e confirmar que o site está disponível.

Nesta lição, você vai:

- pedir ao Copilot para configurar a publicação com o GitHub Pages.
- revisar o workflow e o pull request antes de fazer o merge.
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma forma simples de experimentá-lo. Publicar o site de um único arquivo com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar um servidor ou dependência de tempo de execução.

## Peça ao Copilot para publicar o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama em três etapas: um pull request do space-quiz integrado; uma execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede que o agente crie e integre um pull request e altere as configurações de Pages do repositório. Revise o diff, as permissões de publicação e todos os pedidos de aprovação do GitHub antes de continuar.

## Revise o workflow e verifique a publicação

1. Confirme se o README foi adicionado somente quando o repositório ainda não tinha um.
2. Revise as mudanças do pull request, incluindo as etapas de compilação e publicação do workflow.
3. Faça o merge somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de publicação do GitHub Actions até que ela seja concluída com êxito.
5. Abra a URL informada e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou integrar o pull request, siga as instruções específicas fornecidas por ele. Não considere a tarefa concluída até que a publicação seja bem-sucedida e o site esteja acessível.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site publicado, e você verificou a publicação em vez de confiar apenas no relato do agente. Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
