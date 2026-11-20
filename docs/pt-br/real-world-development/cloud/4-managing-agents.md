---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel, você pode ver todas as tarefas com pull requests abertos, explorar as operações realizadas e orientar sessões em andamento.

Nesta lição, você:

- explorará a página de agentes para monitorar as tarefas do agente de nuvem.
- orientará uma sessão em andamento e pedirá outra funcionalidade.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe decidiu que também seria um bom momento para adicionar um modo claro. Como o agente já está atualizando o estilo do site e adicionando funcionalidades de alternância, você pedirá que ele inclua o modo claro junto ao alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos verificar o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas, incluindo:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exatas variam conforme o estado do trabalho do Copilot e a abordagem utilizada.

4. Observe também o painel do pull request (PR), à direita, que permite acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas em andamento, vamos pedir ao Copilot que inclua um botão para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que o agente conclua a configuração e comece o trabalho.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot reconhece a solicitação e a incorpora ao trabalho.

## Deixar o Copilot trabalhar

O Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso pode levar alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que aprendeu até aqui.

## Resumo e próximas etapas

Você explorou a página de agentes do Copilot, o painel central para monitorar e orientar as tarefas do agente de nuvem. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para ajustar o escopo ou fornecer mais orientações.

Você aprendeu a monitorar as tarefas no painel e a redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui as alterações de acessibilidade, você pode [fazer iterações nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Fazer iterações no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
