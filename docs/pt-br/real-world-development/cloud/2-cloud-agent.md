---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações estão livres de dívida técnica: pode haver problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas e dedicar mais tempo ao trabalho de maior prioridade.

Neste exercício, você explorará:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues bem delimitadas.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver parte de sua dívida técnica. O código inicial do site está sem documentação. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas.

A equipe de design também quer facilitar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar outros títulos da mesma categoria. A implementação não precisa estar finalizada; o objetivo é permitir testes de aceitação da experiência. Esse trabalho é importante, mas tem prioridade menor que outras issues.

Esses são bons exemplos de tarefas que podem ser adiadas. O agente de nuvem pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras atividades e, depois, retorna para revisar o resultado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot — você ainda precisa pensar. Mesmo operações aparentemente simples podem ter complexidades. [Defina cuidadosamente o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software continuam os mesmos.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Antes de atribuir uma tarefa ao agente de nuvem, leia os arquivos de instruções do repositório para reconhecer o efeito deles mais tarde:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, especialmente as convenções de TypeScript e as expectativas para comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation`, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos nos arquivos do pull request. Essas orientações vêm dos arquivos de instruções e serão mencionadas novamente ao revisar PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um workflow de configuração especial, armazenado em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. O modelo já inclui essa configuração para instalar Node.js, dependências, navegadores para testes de ponta a ponta e configurar o banco de dados SQLite local da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Caso contrário, o Copilot fará isso automaticamente depois da conclusão das etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com outros workflows do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar o workflow sem aguardar uma tarefa do Copilot.

## Adicionar documentação

Muitos projetos têm documentação desatualizada ou não têm documentação alguma. Essa dívida técnica pode reduzir a produtividade e dificultar a manutenção do código e a integração de novas pessoas à equipe. O Copilot pode ajudar a criar a documentação, por isso esta é uma boa tarefa para delegar ao agente de nuvem.

1. Abra seu repositório no github.com.
2. Selecione **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. Defina o título como `Code lacks documentation`.
5. Use esta descrição:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

6. Selecione **Create** e, no painel à direita, **Assign to Copilot**.

    ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

7. Selecione **Assign** e abra a guia **Pull Requests**.

    ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

8. Abra o pull request recém-criado, com título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde e atualize a página.
9. Aguarde a lista de tarefas do Copilot e revise o que ele pretende fazer. A preparação do ambiente, a análise do projeto e a criação da lista podem levar alguns minutos.
10. Na linha do tempo do PR, selecione **View session** para acompanhar a sessão ao vivo e observar o Copilot explorar o código, planejar e criar alterações.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador de status atualizado.

O agente pode levar alguns minutos para concluir. Enquanto ele trabalha, você pode atribuir outra tarefa e continuar desenvolvendo o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é poder dividir o trabalho. Vamos atribuir a ele outra issue:

1. Volte ao repositório no github.com.
2. Selecione **Issues**, **New issue** e **Blank issue**.
3. Defina o título como `Show related games on the game details page`.
4. Use esta descrição:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

    - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
    - Show a "Related games" section on the game details page that uses the helper
    - Handle the case where a game has no related games
    - There should be unit tests created for the new helper
    - Before creating the PR, ensure all tests pass
    ```

5. Selecione **Create** e, à direita, **Assign to Copilot**.

    ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

6. Selecione **Assign**. Um emoji 👀 no primeiro comentário da issue indica que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O agente de nuvem trabalha de forma semelhante a uma pessoa desenvolvedora: não é necessário monitorar continuamente. Revise o resultado quando estiver pronto e, enquanto isso, comece a explorar agentes personalizados.

## Resumo e próximas etapas

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents], que pode trabalhar em issues de forma assíncrona para resolver dívida técnica, criar funcionalidades ou ajudar na migração de código entre frameworks. Você aprendeu a personalizar o ambiente, considerar as operações com segurança, delimitar issues e atribuí-las ao Copilot.

O agente pode usar [servidores MCP][cloud-agent-mcp] e as instruções personalizadas exploradas anteriormente. Agora você pode criar e usar agentes personalizados.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
