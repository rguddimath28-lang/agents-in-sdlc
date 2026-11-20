---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] orientam como o código deve ser gerado.

Neste exercício, você:

- explorará como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas e arquivos específicos de caminhos.
- gerará o primeiro recorte da camada de dados para filtragem, um auxiliar de editoras, com as instruções atuais.
- adicionará um padrão geral a `.github/copilot-instructions.md`.
- enviará o mesmo prompt novamente e observará como o código gerado adota o novo padrão.
- confirmará as instruções e o código de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é probabilístico. O objetivo é observar a tendência de mudança após a atualização das instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: a camada de dados precisa de testes unitários, a interface deve usar modo escuro e ter uma aparência moderna, o código deve conter comentários TSDoc e cada arquivo deve começar com um bloco de comentários que explique sua finalidade. Os arquivos de instruções ajudam a orientar o Copilot de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas dão contexto e informam preferências ao Copilot. Elas podem especificar convenções de codificação, bibliotecas e tipos de comentários, no nível do repositório ou para tipos específicos de arquivo e tarefa.

Há dois tipos de arquivos:

- `.github/copilot-instructions.md`: um arquivo enviado ao Copilot em **todas** as solicitações do repositório, com contexto geral do projeto.
- `.github/instructions/*.instructions.md`: arquivos de diretrizes para linguagens, tipos de arquivo ou tarefas específicos.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para complementações de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo`. Também há suporte a padrões como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Mantenha `copilot-instructions.md` focado nas orientações gerais do projeto. Use arquivos `*.instructions.md` para tipos de arquivo ou tarefas específicos. Escreva instruções claras, em linguagem natural, e inclua exemplos.

> [!TIP]
> Explore os arquivos de instruções para [atualizações de interface][ui-instructions] e [Astro][astro-instructions]. O Copilot também pode gerar instruções, por exemplo, com **Configure Chat → Generate Agent Instructions** no VS Code. Para ver modelos, consulte o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos no repositório:

1. Abra `.github/copilot-instructions.md` e examine a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, incluindo a orientação sobre GitHub Actions Workflows.
2. Explore a pasta `.github/instructions`, que contém instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
3. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que corresponde a arquivos TypeScript de teste, por exemplo, `**/*.test.ts`.
4. Leia as instruções para testes unitários.
5. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e exemplos do projeto.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará comentários TSDoc e cabeçalhos de arquivo.

## Criar um branch para as alterações

1. Volte ao Codespace.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione um branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de continuar, abra o painel Copilot Chat no codespace e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa a seleção necessária.

1. Feche as guias do editor abertas em exercícios anteriores.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Selecione **Agent** no menu de agentes da visualização Chat.

   ![Captura de tela com o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e poderá alterar o auxiliar e os testes. Observe que a função tipada usa um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras, seguindo as instruções da camada de dados em `.github/instructions/drizzle.instructions.md`.
6. Observe se a proposta não inclui comentários TSDoc nem cabeçalho no nível do arquivo.

> [!CAUTION]
> O Copilot pode adicionar comentários mesmo sem a instrução explícita. Isso não é um problema; o objetivo é observar a consistência após a atualização das instruções.

## Adicionar um novo padrão ao repositório

1. Reabra `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements** e observe que ainda não há uma regra para documentação no código.
3. Adicione estas linhas de Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve o arquivo.

> [!TIP]
> As instruções do projeto ficam em `.github/copilot-instructions.md`; arquivos `*.instructions.md` atendem linguagens, tipos de arquivo e tarefas específicos. Regras gerais, como a documentação no código, pertencem ao arquivo do repositório.

## Repetir o prompt e observar a mudança

1. No Copilot Chat, selecione **New Chat** para limpar o contexto.
2. Volte a `src/lib/publishers.ts`.
3. Envie novamente o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe o bloco de comentários de cabeçalho e o comentário TSDoc adicionados à função. O texto será semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código que servirá de base para o próximo exercício.

## Confirmar as instruções e enviar o branch

1. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme se as instruções e o auxiliar foram alterados:

   ```bash
   git status
   ```

3. Prepare e confirme as instruções e o auxiliar; inclua outros arquivos de suporte, como testes, se aparecerem em `git status`:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie o branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Com o branch enviado, crie um pull request associado à issue de padrões de documentação do backlog. O Copilot pode localizar a issue, criar um PR para concluí-la e mesclar o PR.

1. Abra o Copilot Chat no codespace, pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) e confirme que **Agent** está selecionado.
2. Peça ao Copilot para localizar a issue e criar um PR a partir do branch atual:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

3. Quando o Copilot pedir para executar ações do GitHub, revise o comando e selecione **Allow** se for apropriado. Na primeira vez, talvez seja necessário entrar no GitHub.
4. Depois da criação do PR, peça ao Copilot que o mescle e atualize o branch local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

5. Revise as ações e os comandos e selecione **Allow** se for apropriado.

## Resumo e próximas etapas

Você explorou como o Copilot usa as instruções do projeto e, no VS Code, enviou um prompt de geração de código com as orientações existentes, adicionou um padrão ao `.github/copilot-instructions.md`, repetiu o prompt e observou as alterações. Por fim, confirmou as instruções e o auxiliar em `main`.

Em seguida, você usará o [modo de agente][next-lesson] para adicionar um recurso.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
