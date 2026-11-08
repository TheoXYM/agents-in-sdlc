---
title: "Lição 8 - Criar e integrar o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você integrou o PR de avaliações por estrelas por conta própria; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e uma branch, e incluiremos todo esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar o PR do recurso completo e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar a integração do PR.

## Cenário

Durante o fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle de quem desenvolve.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que estiver impedindo o merge — corrigindo verificações de integração contínua (CI) com falha, respondendo a comentários de revisão e fazendo rebase quando necessário — e faz o merge assim que o GitHub permite. Ele trabalha em segundo plano, continua funcionando após reinicializações do aplicativo e é desativado quando o pull request é integrado.

Até agora, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de autorização explícita para editar o código e fazer o merge. Revise as ações permitidas e o trabalho antes de conceder a permissão de merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge começará. Ele vai:

- criar o pull request com título e descrição.
- incluir no corpo da descrição uma referência à issue relacionada, se a sessão tiver começado a partir de uma issue.
- fazer rebase ou lidar com possíveis conflitos de merge com a branch de destino.
- monitorar o processo de CI para garantir que todas as verificações sejam aprovadas.
- monitorar o PR em busca de comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer atualizações para responder a esses comentários.
- opcionalmente, integrar o PR automaticamente quando tudo estiver aprovado.

Vamos permitir que o Agent Merge também integre o PR depois que todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão de merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça o merge:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios relatados. Antes de continuar, confirme se o status do PR é **Merged**. Ter habilitado a permissão de merge ou um agente em execução não confirma que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração, o teste e a validação de código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou o PR do recurso completo e as evidências de verificação.
- autorizou o Agent Merge somente após a revisão e confirmou que o PR foi integrado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
