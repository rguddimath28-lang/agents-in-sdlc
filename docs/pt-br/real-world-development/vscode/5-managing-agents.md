---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre na sua máquina e em tempo real, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar diffs, aceitar ou rejeitar alterações e orientar a conversa sem sair do editor.

Neste exercício, você:

- monitorará em tempo real o trabalho do agente de acessibilidade.
- revisará e aceitará as edições propostas.
- orientará o agente durante a sessão para adicionar um controle do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design solicitou também um modo claro para quem considera o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente.

## Monitorar o progresso do Copilot

A visualização Copilot Chat mostra as etapas executadas pelo agente: arquivos explorados, comandos executados e edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramentas, leituras e edições de arquivos e os comandos de terminal solicitados.
3. Quando o agente propuser uma edição:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para desfazê-la e permitir outra tentativa.
4. Abra **Files changed** na parte superior do chat para revisar todos os arquivos alterados.

> [!NOTE]
> Se uma chamada pedir permissão, por exemplo, para executar um comando de terminal, o agente pausará e mostrará **Allow**. Aprove apenas os comandos em que você confia.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará um resumo das alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você adicionará um controle de modo claro à mesma conversa. Assim, o agente mantém o contexto dos componentes, da abordagem de estilo e do padrão de armazenamento local usados anteriormente.

1. No Copilot Chat, envie este prompt:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente examinará as alterações anteriores e proporá mudanças para o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de forma inadequada, interrompa-o com **Stop** e envie uma mensagem para esclarecer a solicitação.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Peça ajustes específicos, corrija a abordagem ou amplie o escopo em prompts de acompanhamento.

## Revisar o conjunto de alterações

Antes de confirmar o trabalho, revise tudo que o agente alterou.

1. Abra **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Ela deve incluir componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme se as práticas de acessibilidade, como atributos ARIA, navegação por teclado, HTML semântico e persistência no armazenamento local, foram aplicadas.
4. Quando estiver satisfeito, prepare e confirme as alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento, revisar cada edição, aceitar ou rejeitar alterações e ampliar o trabalho com prompts de acompanhamento.

Você aprendeu a monitorar o trabalho do agente em tempo real, aceitar ou rejeitar edições individuais e orientar uma sessão com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Fazer iterações no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
