---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações que o Copilot deve conhecer — disponibilize esse contexto. Os [arquivos de instruções][instruction-files] fornecem orientação para que o Copilot entenda não apenas *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você:

- explorará como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções específicos de caminhos.
- adicionará um novo padrão geral ao repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não executará um prompt *antes/depois* aqui. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, por isso é mais difícil demonstrar o efeito lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve conter documentação no formato de comentários TSDoc.
- Cada arquivo deve começar com um bloco de comentários que descreva sua finalidade.

Com os arquivos de instruções, você dará ao Copilot as informações necessárias para trabalhar de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas dão contexto e informam preferências ao Copilot para que ele entenda melhor o estilo e os requisitos de codificação. Você pode indicar convenções, bibliotecas e tipos de comentários preferidos. As instruções podem abranger o repositório inteiro ou tipos específicos de arquivo e tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações gerais do projeto, como a pilha de tecnologias, uma visão geral do que está sendo criado e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para complementações de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também aceita outros padrões de arquivos de instruções, como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está fora do escopo deste workshop, mas o projeto de exemplo demonstra uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em informações gerais do projeto e padrões de codificação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo ou tarefas, como testes unitários, componentes Astro e a camada de dados.
- Use linguagem natural e orientações claras, com exemplos de código correto e incorreto.

Não há uma única maneira certa de criar arquivos de instruções ou usar IA. A experimentação ajuda a descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste projeto há instruções para várias tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de uma forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos no repositório: há um `copilot-instructions.md` principal e diversos arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre workflows do GitHub Actions. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define um glob relativo à raiz do repositório. Ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções e arquivos do projeto, que permitem dividir conjuntos maiores em partes reutilizáveis. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Adicionar um novo padrão ao repositório

Agora você adicionará uma regra geral para que a documentação no código use comentários TSDoc e cabeçalhos no nível do arquivo.

1. No terminal do Codespace, crie e selecione um branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Reabra `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27.
4. Adicione estas linhas de Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> As instruções podem ficar no nível do repositório, em `.github/copilot-instructions.md`, para orientações gerais, ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicos. Regras gerais, como a exigência de comentários de documentação, pertencem ao arquivo do repositório.

## Confirmar, enviar e mesclar a atualização das instruções

O agente de nuvem lê os arquivos de instruções do branch de destino da issue. No próximo exercício, o agente será iniciado a partir de `main`, portanto as alterações de instruções precisam estar em `main`.

1. Prepare e confirme as alterações:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie o branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request do branch `custom-instructions-cloud` para `main` e faça a mesclagem. O agente de nuvem usará essas instruções nas issues atribuídas a ele no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main`, pule a etapa do branch e confirme as alterações diretamente em `main`. O uso de um branch demonstra o fluxo de trabalho habitual em um projeto real.

## Resumo e próximas etapas

Você explorou como o Copilot usa os arquivos de instruções do projeto e adicionou um padrão geral a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem gerar nos próximos exercícios.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
