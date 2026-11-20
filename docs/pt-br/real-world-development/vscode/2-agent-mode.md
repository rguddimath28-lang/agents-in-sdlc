---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples em um aplicativo pode exigir alterações em vários arquivos e a execução de testes. Uma pessoa desenvolvedora normalmente localiza os arquivos necessários, faz alterações, executa testes, depura problemas e corrige o que ficou faltando. O modo de agente do Copilot pode explorar a estrutura do projeto, fazer alterações coordenadas, executar tarefas como testes e corrigir erros encontrados.

> [!NOTE]
> Embora tenham nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE e permite interação e feedback rápidos. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, usando issues e pull requests.

Neste exercício, você aprenderá como o modo de agente pode explorar o projeto, implementar recursos na interface e na camada de dados e criar testes. Você também revisará as alterações antes de mesclá-las.

## Cenário

À medida que a lista de jogos cresce, você quer permitir a filtragem por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site para entender as funcionalidades existentes. O site de financiamento coletivo para jogos de tabuleiro é um único aplicativo Astro que gera páginas HTML estáticas. As páginas consultam um banco de dados SQLite local com Drizzle ORM; não há API de back-end separada nem framework de interface no cliente. Os auxiliares de acesso a dados ficam em `src/lib/`, e a interatividade usa um elemento `<script>` do Astro com APIs padrão do DOM.

### Iniciar o site

O comando `npm run dev` inicia o servidor de desenvolvimento Astro.

1. Volte ao Codespace. Você deve estar no branch `main` depois do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Atualize `main` e crie um branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um endereço semelhante a `http://localhost:4321`.

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura do navegador em `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o endereço do servidor no terminal mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando `http://localhost:4321`.

> [!NOTE]
> Em um codespace, o link localhost do terminal é redirecionado para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor do codespace.

### Explorar o site

O site lista jogos de tabuleiro com títulos, imagens e descrições. Ao selecionar um jogo, você verá uma página de detalhes com informações como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa a seleção necessária.

O site já funciona, mas queremos ampliar seus recursos. O modelo do projeto cria automaticamente um backlog de issues. Peça ao GitHub Copilot para exibi-lo:

1. Selecione **Agent** no menu de agentes da visualização Chat. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e invoca ferramentas.

   ![Captura de tela com o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Este modelo aparece porque foi usado durante a criação do workshop, não como recomendação. As respostas podem variar, pois os modelos de linguagem são probabilísticos.

3. Envie este prompt para consultar o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para listar as issues e revise a lista gerada.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, revise `.github/instructions/ui.instructions.md`, que fornece contexto sobre como adicionar funcionalidades ao site:

1. Abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações sobre arquitetura, design de componentes, testabilidade e acessibilidade, além dos links para instruções de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, o que permite reutilizar orientações e exemplos.

## Implementar a filtragem

Para concluir a filtragem, é necessário implementar ou refinar a lógica de dados em `src/lib/`, criar ou atualizar testes e adicionar a interface de filtragem à página de listagem de jogos. Os testes devem ser aprovados antes da mesclagem.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui**.

   ![Captura de tela mostrando um exemplo de seleção do arquivo de instruções UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** e **Claude Sonnet 4.5** estão selecionados.

   ![Captura de tela com o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Acompanhe o Copilot enquanto ele explora o projeto, localiza os auxiliares da camada de dados, a interface e os testes, altera arquivos e executa verificações.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> A tarefa pode levar alguns minutos. O Copilot pode alternar entre gerar código e executar testes até concluir o trabalho. Durante o processo, os controles **Keep** e **Undo** aparecem no editor; aguarde a conclusão antes de escolher o destino das alterações.

6. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat que pergunta se você quer executar testes](../../../_images/ex3-agent-mode-run-tests.png)

7. Revise o código e qualquer resultado de teste.

   ![Captura de tela de uma sessão completa do Copilot Chat com o modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

8. Volte ao navegador, explore a nova funcionalidade e, quando a revisão estiver concluída, selecione **Keep** na janela do Copilot Chat.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

## Resumo e próximas etapas

Você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu a explorar o projeto, fazer alterações coordenadas na interface e na camada de dados e revisar alterações e testes antes da mesclagem.

Agora, você [testará o recurso com o servidor MCP do Playwright][next-lesson] e abrirá um pull request.

### Exercício extra — implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use o que aprendeu para pedir ao Copilot que adicione esse recurso. Considere as práticas existentes e os arquivos de instruções, decida se as pessoas poderão selecionar o tamanho da página e itere sobre o resultado conforme necessário.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
