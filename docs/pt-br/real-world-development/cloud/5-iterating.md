---
title: "Exercício 5: Itere no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade com modos de alto contraste e claro. Vamos explorar as alterações e, se necessário, orientar o Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão pelo restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, há restrições para manter a segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de escrita **somente** à branch que usará para o trabalho.
- O agente de nuvem é executado no GitHub Actions em um ambiente separado e temporário.
- Os fluxos de trabalho do GitHub Actions precisam de aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão][agent-firewall], inclusive a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: a adição de documentação ao código. Você fará essa revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do agente de nuvem a recursos externos, inclusive servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a marca **[WIP]**. Aguarde até que o trabalho termine; isso pode levar alguns minutos.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc e outras documentações. As alterações exatas podem variar. Procure os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções personalizados que você examinou no início do Exercício 2.
6. Quando terminar a revisão, volte à guia **Conversation** e role a página.
7. Procure um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos deverão ser colocados na fila da seção de verificações do pull request. As verificações do projeto para o aplicativo Astro devem ser aprovadas; isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários do PR ou comentários em linha no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um texto único a recomendar. Algumas ideias de alterações que você pode solicitar:

- adicionar cabeçalhos de comentário no início de cada arquivo de código, descrevendo sua finalidade.
- adicionar comentários TSDoc aos arquivos TypeScript e Astro.
- criar um README com uma descrição da estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com outra pessoa da equipe. Use uma das ideias acima ou proponha outra alteração na documentação do código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit deverá aparecer no PR.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até que o resultado esteja satisfatório. Depois, você pode marcar o PR de rascunho como pronto para revisão e mesclá-lo à branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora vamos voltar ao PR que o Copilot criou para resolver a issue de exibir jogos relacionados na página de detalhes.

1. Acesse novamente o repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Quando terminar a revisão, volte à guia **Conversation** e role a página.
6. Procure um indicador de que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Confirme que as verificações do projeto foram colocadas na fila e aguarde a aprovação.
9. **Opcional:** faça o teste manual no Codespace. Abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode iterar no trabalho do agente de nuvem. Por exemplo, peça que ele altere o número de jogos relacionados ou adicione cabeçalhos de comentário e comentários TSDoc — lembre-se de que a issue foi atribuída **antes** da atualização das instruções. Para isso, adicione um comentário na guia **Conversation**; o Copilot iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Este PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no Exercício 4.

1. Acesse novamente seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a marca **[WIP]**. Aguarde até que o trabalho termine; isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código.
5. Revise a implementação, principalmente:
   - os componentes de alternância entre modos.
   - o uso de armazenamento local para manter as preferências.
   - as alterações de CSS ou estilo dos modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - o código JavaScript/TypeScript que controla a mudança de modo.
6. Quando terminar a revisão, volte à guia **Conversation** e role a página.
7. Procure um indicador de que alguns fluxos aguardam aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Aguarde as verificações do projeto na seção de verificações do PR.
10. **Opcional:** faça o teste manual no Codespace. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos claro e de alto contraste no navegador. Confirme que as preferências continuam ativas depois de atualizar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot na implementação dessas funcionalidades conforme boas práticas de acessibilidade. Se notar algum problema ou oportunidade de melhoria, mencione **@copilot** em um comentário e solicite uma atualização.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie algumas issues no seu repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes de um jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizados do repositório** para ver como influenciam o trabalho do agente.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, com modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e ajustou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, veja os outros percursos:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas no terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma opção `/delegate` para voltar ao agente de nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
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
