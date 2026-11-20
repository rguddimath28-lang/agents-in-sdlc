---
title: "Exercício 5: Fazer iterações no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu que o Copilot documentasse o código, criasse o recurso de jogos relacionados e implementasse melhorias de acessibilidade com modos de alto contraste e claro. Agora, você revisará as alterações e, se necessário, pedirá que o Copilot as aperfeiçoe.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a inclusão da IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Você revisará as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e as funcionalidades de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** ao branch usado para o código.
- O agente de nuvem é executado no GitHub Actions em um ambiente separado e temporário.
- Workflows do GitHub Actions precisam da aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem: o PR que adiciona documentação ao código.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**, pois o acesso a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Abra seu repositório no github.com.
2. Selecione **Pull Requests** e abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

3. Quando o PR estiver pronto, selecione **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

4. Examine o código atualizado e procure comentários TSDoc, convenções de TypeScript e cabeçalhos de arquivo, seguindo as instruções revisadas no Exercício 2.
5. Volte à guia **Conversation** e role até o final. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

6. Confirme que os workflows foram iniciados e que as verificações do projeto para o aplicativo Astro foram aprovadas.

## Solicitar alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou comentários em linha no código. Ele verá os comentários e iniciará outra sessão para tratá-los. Como os resultados não são determinísticos, não há um prompt único. Você pode pedir que o Copilot:

- adicione cabeçalhos de comentário aos arquivos de código.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário sobre a documentação gerada e mencione **@copilot**.
2. Selecione **View Session** para acompanhar a nova sessão.
3. Use **Back to pull request** para voltar ao PR.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar, revise o novo commit na guia **Files changed**.

Continue fazendo iterações até que o resultado esteja adequado. Depois, marque o PR como pronto para revisão e mescle-o.

![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, revise o PR criado pelo Copilot para mostrar jogos relacionados na página de detalhes.

1. No GitHub.com, abra a guia **Pull Requests** e selecione o PR com título semelhante a **Show related games on the game details page**.
2. Selecione **Files changed** e revise o código.
3. Se workflows aguardarem aprovação na guia **Conversation**, selecione **Approve and run workflows** e confirme que as verificações foram aprovadas.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

4. **Opcional:** Para testar o recurso manualmente, abra um Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo branch criado pelo Copilot, por exemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Você pode continuar fazendo iterações com o agente de nuvem, pedindo, por exemplo, alterações na quantidade de jogos relacionados ou cabeçalhos de comentário e documentação TSDoc. Adicione comentários na guia **Conversation** para iniciar outra sessão.

## Revisar as funcionalidades de acessibilidade

Por fim, revise as funcionalidades de acessibilidade implementadas pelo agente personalizado. O PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. No GitHub.com, abra **Pull Requests** e selecione o PR com título semelhante a **Add high contrast mode to website**.
2. Se o PR ainda tiver a indicação **[WIP]**, aguarde a conclusão do Copilot.
3. Selecione **Files changed** e revise:
   - os controles de interface para alternar os modos.
   - o uso do armazenamento local para manter as preferências.
   - as alterações de CSS e estilo para os modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - o código JavaScript/TypeScript que controla a alternância.
4. Na guia **Conversation**, selecione **Approve and run workflows** se houver workflows aguardando aprovação e confirme que as verificações foram aprovadas.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

5. **Opcional:** Para testar manualmente, obtenha o branch no Codespace com `git fetch origin` e `git checkout <branch-name>`. Inicie o aplicativo e teste os controles para confirmar que as preferências persistem após recarregar a página.

Se identificar problemas de acessibilidade, mencione **@copilot** em um comentário e peça as alterações necessárias.

## Exercício opcional — continuar delegando

O agente de nuvem é útil para trabalhar em itens reais do backlog. Crie mais algumas issues e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de listagem de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Você:

- **inspecionou os arquivos de instruções personalizadas** do repositório e observou o efeito deles nos resultados.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** para implementar recursos de acessibilidade.
- **usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão.
- **revisou e fez iterações nos pull requests**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximas etapas

Você concluiu o percurso do agente de nuvem. Para continuar explorando, veja os outros percursos:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas no terminal com os modos Plan e Autopilot, skills de agente, agente de QA personalizado, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
