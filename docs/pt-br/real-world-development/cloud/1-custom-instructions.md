---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou há informações de contexto que o Copilot deve conhecer —, você precisa garantir que elas estejam acessíveis. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não apenas *o que* você quer, mas também *como* quer que o trabalho seja feito.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos de instruções associados a caminhos específicos.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos do VS Code e da CLI, aqui você não executará um prompt *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests criados pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Eles incluem:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo e os requisitos do seu código. Esse recurso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de programação, bibliotecas e os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas ao Copilot pelo chat ou pela CLI, como a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou um conjunto de testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções de outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado em orientações para o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e padrões gerais de programação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve (e não deve) ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar IA. Experimente para descobrir o que funciona melhor para o seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de um jeito diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição do ambiente em uso indicará quando esse recurso for relevante.
>
> Está procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório: há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Examine a descrição breve do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, serão incluídos.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicionar um padrão ao repositório

Esta será a única edição deste exercício: adicionar uma regra para todo o projeto que exija documentação no código por meio de comentários TSDoc e um cabeçalho de comentário por arquivo. O agente de nuvem usará essa regra ao trabalhar nas issues que você atribuir a ele mais adiante.

Antes de editar o arquivo, crie uma branch (o agente de nuvem lerá as instruções da branch de destino da issue, mas você fará um commit organizado):

1. No terminal do codespace, crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como vimos, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de programação do projeto, mas ainda não exige documentação no código; por isso, o helper gerado não tinha comentários de documentação.
3. Logo abaixo dos padrões existentes, adicione estas linhas Markdown para orientar o Copilot a incluir cabeçalhos de comentário e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como vimos na lição anterior, as instruções podem ser criadas na raiz do repositório (`.github/copilot-instructions.md`) para fornecer orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicos. O arquivo do repositório é o lugar certo para padrões gerais como a regra dos comentários de documentação que você acabou de adicionar.

## Fazer commit, enviar e integrar a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. No próximo exercício, ao atribuir uma issue ao Copilot, ele criará uma branch a partir de `main`. Portanto, as atualizações das instruções precisam estar em `main` para que o agente de nuvem possa usá-las.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem poderá ler essas instruções quando trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a etapa de branch e fazer commit diretamente em `main`. A branch foi incluída para que o workshop represente o fluxo usado em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem criar no restante deste percurso.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
