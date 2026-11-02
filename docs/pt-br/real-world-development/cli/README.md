---
slug: pt-br/real-world-development/cli
title: "CLI do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** coloca o GitHub Copilot no terminal como assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando.

Ao longo destas lições, você instalará e autenticará o Copilot CLI, fará uma pequena alteração no projeto e depois usará os modos Plan e Autopilot para desenvolver um recurso a partir de uma issue. Você orientará o agente com instruções personalizadas, personalizará skills de agente, adicionará validação com o Playwright MCP e criará um agente de QA. Em seguida, revisará e mesclará o PR do recurso com o Agent Merge e explorará comandos de barra para gerenciar contexto, modelos, compartilhamento e delegação opcional para a nuvem. Você também pode seguir uma [série opcional de três módulos com GitHub Copilot CLI e Foundry][foundry] para preparar um modelo, criar e implantar um agente hospedado e integrá-lo ao site.

## Lições

| Lição | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu Codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI |
| [2. Adicionar avaliações em estrelas][ex2] | Primeiro recurso | Faça uma pequena alteração e abra seu primeiro pull request |
| [3. Modos do agente][ex3] | Planejamento e implementação | Use Plan e Autopilot para desenvolver um recurso |
| [4. Instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções do projeto |
| [5. Skills de agente][ex5] | Skills | Personalize uma skill reutilizável de verificação de qualidade |
| [6. Validação com o Playwright MCP][ex6] | Ferramentas externas | Valide seu recurso em um navegador |
| [7. Agente de QA][ex7] | Agentes | Crie e use um agente especializado em QA |
| [8. Crie e mescle o PR][ex8] | Pull requests | Revise o recurso e use o Agent Merge |
| [9. Ferramentas avançadas da CLI][ex9] | Recursos da CLI | Explore comandos de contexto, modelos, compartilhamento e delegação opcional |
| [10. Revisão][ex10] | Resumo | Recapitule o fluxo e os próximos passos |
| [Opcional: Incorpore o Foundry][foundry] | Agentes hospedados | Prepare um modelo, crie e implante um Backer Concierge baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Familiaridade básica com operações de terminal e linha de comando
- [ ] O Git instalado e configurado

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop. Portanto, você pode concluir todas as atividades com esse plano.

> [!NOTE]
> Se você usa o Copilot Business ou Enterprise, confirme com o administrador que o Copilot CLI está habilitado.

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
