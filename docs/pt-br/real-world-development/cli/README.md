---
slug: pt-br/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** leva o GitHub Copilot ao terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você mantenha o foco sem alternar para um editor gráfico.

Ao longo destas lições, você instalará e autenticará o Copilot CLI e fornecerá contexto ao projeto com instruções personalizadas. Em seguida, usará o modo Plan para planejar um recurso, pedirá ao Copilot para implementá-lo e validará o resultado com testes e no navegador. Você conectará o servidor MCP do Playwright, criará um agente de QA e abrirá um pull request. Por fim, explorará comandos de barra para gerenciar contexto, modelos e compartilhamento. Também é possível seguir a [série opcional de três módulos sobre o GitHub Copilot CLI e o Foundry][foundry] para preparar um modelo, criar e implantar um agente hospedado e integrá-lo ao site.

## Lições

| Lição | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI |
| [2. Adicionar avaliações por estrelas][ex2] | Primeira alteração | Faça uma pequena alteração nos cartões de jogos, revise-a e mescle seu primeiro pull request |
| [3. Modos de agente][ex3] | Implementação | Planeje e implemente a filtragem com os modos Plan e Autopilot |
| [4. Instruções personalizadas][ex4] | Contexto | Explore as instruções do repositório e adicione um padrão de documentação |
| [5. Skills de agente][ex5] | Skills | Personalize uma skill reutilizável para verificações de qualidade |
| [6. Validação com o MCP do Playwright][ex6] | Ferramentas externas | Teste o recurso de filtragem em um navegador |
| [7. Criar e usar um agente de QA][ex7] | Agentes personalizados | Combine os requisitos da issue, a skill de verificações de qualidade e o MCP do Playwright |
| [8. Criar e mesclar o pull request][ex8] | Pull requests | Revise o recurso e use o Agent Merge |
| [9. Ferramentas úteis da CLI][ex9] | Recursos da CLI | Explore comandos de barra para contexto, modelos, compartilhamento e delegação opcional |
| [10. Revisão][ex10] | Recapitulação | Recapitule o fluxo de trabalho e os próximos passos |
| [Opcional: Incorporar o Foundry][foundry] | Agentes hospedados | Prepare um modelo, crie e implante um Backer Concierge baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Familiaridade básica com operações de terminal e linha de comando
- [ ] O Git instalado e configurado

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop, permitindo concluir todos os percursos.

> [!NOTE]
> Se você usa o Copilot Business ou Enterprise, confirme com o administrador se o Copilot CLI está habilitado para uso.

## Começar

[**Comece pela Lição 0: Pré-requisitos →**][ex0]

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
[callout-student-plan-education]: https://github.com/education/students
