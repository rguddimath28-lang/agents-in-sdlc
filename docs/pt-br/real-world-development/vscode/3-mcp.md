---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e permita que o Copilot controle um navegador real. Depois, publique o branch e abra o PR.

Neste exercício, você:

- aprenderá o que é o Model Context Protocol (MCP) e como servidores MCP ampliam as ferramentas do Copilot.
- adicionará o servidor MCP do Playwright ao espaço de trabalho.
- pedirá ao Copilot para testar manualmente o recurso de filtragem no navegador.
- publicará o branch e abrirá um pull request.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica mais potente quando pode acessar ferramentas fora do editor. O Model Context Protocol (MCP) é um padrão para conectar o Copilot a ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e como ele interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Eles podem acessar informações atualizadas e executar ações em seu nome.

Um servidor MCP faz a ponte entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais. Cada servidor oferece ao agente um conjunto próprio de ferramentas e recursos.

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com o Playwright para navegar em páginas, preencher formulários e selecionar botões.

O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta de servidores e contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada vem do modelo do projeto e permitiu que o Copilot acessasse o backlog na lição anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]. Não é preciso instalar nada localmente: o VS Code se conecta por HTTP e solicita autenticação do GitHub na primeira utilização.

## Adicionar o servidor MCP do Playwright

O [servidor MCP do Playwright][playwright-mcp-server] dá ao Copilot acesso a um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione a entrada `playwright` junto de `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é local: o VS Code o inicia com `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem janela visível, necessário em um codespace sem área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright nos testes de ponta a ponta. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP sob demanda. Na primeira vez que o Copilot precisar das ferramentas, o VS Code iniciará o servidor e solicitará que você confirme que confia nele.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code pedir confirmação de confiança, revise a configuração e autorize o servidor.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code o iniciará e solicitará confiança quando o Copilot precisar das ferramentas.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] permite que o Copilot abra páginas, navegue, aplique filtros e leia os resultados. Ele usa a [árvore de acessibilidade][playwright-mcp-server], não capturas de tela, para raciocinar sobre elementos rotulados, como botões, links e itens de lista.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador, percorrerá as etapas e informará o que observou. Compare o relatório com os critérios de aceitação da issue. Se algo não estiver correto, faça perguntas ou peça uma correção antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o novamente. Na primeira utilização do servidor MCP do Playwright, talvez seja necessário baixar um navegador; se isso acontecer, execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar o branch e criar um pull request

Depois de confirmar que o recurso funciona, publique o branch `filtering-vscode`:

1. Abra o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar o branch ao repositório.

## Criar o pull request

Há várias formas de criar um pull request, incluindo o github.com e a CLI do GitHub. Vamos pedir ao Copilot que encontre a issue correspondente e crie o PR:

1. No Copilot Chat, selecione **New Chat**.
2. Confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Envie este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Selecione **Continue** quando necessário para permitir que o Copilot obtenha informações e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar-se.
5. Confirme que o Copilot encontrou a issue correta e criou o PR.
6. Abra o link criado pelo Copilot para revisar o PR, mas **não o mescle ainda**.

## Resumo e próximas etapas

Neste exercício, você aprendeu o que é MCP, adicionou o servidor MCP do Playwright, usou-o para testar o recurso no navegador, publicou o branch e abriu um pull request.

O MCP é uma forma de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
