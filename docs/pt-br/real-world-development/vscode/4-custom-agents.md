---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções, práticas recomendadas, padrões de codificação e conhecimentos específicos que ajudam o Copilot a realizar determinados tipos de trabalho. As equipes podem transformar seus conhecimentos em agentes reutilizáveis, como um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança ou um agente de testes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA na execução de tarefas. A distinção mais simples é: **o agente personalizado executa o trabalho; as skills são ferramentas**.

Os agentes personalizados têm uma janela de contexto própria e podem orquestrar skills e outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar skills para fluxos de pull request ou execução e gerenciamento de testes.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Teste e faça iterações para encontrar a abordagem adequada ao seu ambiente e aos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes indicaram que algumas pessoas têm dificuldade para ler o tema escuro devido ao baixo contraste entre texto e fundo. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado.

Neste exercício, você:

- revisará um agente personalizado de acessibilidade existente.
- usará esse agente no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente de acessibilidade já foi criado. Volte ao Codespace e crie um branch separado para o trabalho de acessibilidade, mantendo separado o PR de filtragem do Exercício 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam as responsabilidades principais, práticas recomendadas, exemplos de HTML, CSS e JavaScript e problemas comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados definidos em `.github/agents` no menu de agentes da visualização Copilot Chat. Você pode selecionar um agente para aplicar suas instruções e ferramentas à sessão.

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa a seleção necessária.

1. Se ainda não estiver selecionado, escolha **Agent** no menu de agentes.

   ![Captura de tela com o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abra o menu de agentes na parte inferior da visualização Chat; por padrão, o agente ativo é **default**.
3. Selecione **Accessibility agent**.
4. Envie este prompt:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat explorará o código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho e um diff para revisão.
6. Observe o indicador **Files changed** para acompanhar os arquivos modificados pelo agente.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot alterará arquivos Astro, CSS e possivelmente testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Você explorou [agentes personalizados][custom-agents], assistentes especializados em tarefas e domínios específicos. Eles permitem transformar os conhecimentos e padrões da equipe em orientações reutilizáveis para o Copilot.

Você aprendeu como os agentes personalizados são definidos e como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as alterações em andamento e adicionando um controle para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
