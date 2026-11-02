---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem dívidas técnicas. Elas podem incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request de rascunho para que uma pessoa desenvolvedora o revise. Isso permite delegar tarefas repetitivas e concentrar-se em atividades de maior impacto.

Neste exercício, você explorará o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar o site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc às funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre outros jogos da mesma categoria, para que as pessoas continuem explorando o catálogo. Ainda não é necessário ter uma implementação refinada; basta algo que permita testar a experiência. Embora isso esteja bloqueando o trabalho, há outras issues de prioridade maior no momento.

Esses são exemplos de tarefas que costumam ser adiadas e que são ideais para delegar ao agente de nuvem. Ele pode trabalhar de forma assíncrona enquanto você se concentra em outras tarefas; depois, você revisa o trabalho e confirma que o resultado está correto.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] executa tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Uma possibilidade é [atribuir uma issue do GitHub ao Copilot][assign-issue]. Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto ela está em andamento ou depois que terminar. Quando a solução estiver pronta para revisão, o agente marcará você no pull request.

## A importância de instruções com escopo adequado

O GitHub Copilot não é mágico: ainda é preciso pensar. Até tarefas aparentemente simples podem ter complexidades escondidas, por isso é importante [definir bem o escopo do trabalho do agente de nuvem][cloud-agent-best-practices]. Trate-o como um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu a atividade prática de instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções do repositório para reconhecer o efeito deles mais adiante.

Abra os arquivos a seguir na interface web do GitHub ou em um Codespace:

- `.github/copilot-instructions.md`: revise a seção **Code standards**, principalmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando atribuir ao agente a issue `Code lacks documentation`, observe nos pull requests os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar os PRs.

## Configure o ambiente de desenvolvimento do agente de nuvem

Para desenvolver código, é comum ser necessário configurar um ambiente e executar scripts para preparar ferramentas e dependências. O mesmo vale para o Copilot, que trabalha de maneira semelhante a uma pessoa desenvolvedora.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente. Você pode personalizá-lo com um [fluxo de configuração especial][setup-workflow], armazenado em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. Isso disponibiliza ferramentas e dependências para o agente. O modelo já inclui essa configuração para este laboratório: Node.js, dependências do projeto, navegadores para testes de ponta a ponta e o banco SQLite migrado e populado do aplicativo Astro.

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório como parte da configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migra e popula o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer fluxo do GitHub Actions, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar o agente iniciá-la.

## Adicione documentação

Embora todo mundo reconheça a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzir a produtividade e dificultar a manutenção do código ou a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação, por isso esta é uma issue ideal para atribuir ao agente de nuvem. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Assign**.

   ![Atribuição do Copilot à issue](../../../_images/shared-assign-copilot.png)

   ![Detalhes da atribuição do Copilot à issue](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-criado, com título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um momento e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente com o fluxo de trabalho descrito, analisando o projeto e definindo uma abordagem.

13. Revise as tarefas que o agente pretende executar.
14. Percorra a linha do tempo do pull request e procure uma atualização indicando que o Copilot começou a trabalhar.
15. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

16. Acompanhe a sessão, incluindo a exploração do código, a análise do estado do projeto, o planejamento e a criação do código. Talvez seja necessário atualizar a página para ver o indicador mais recente.

Esse trabalho pode levar vários minutos. Um dos objetivos principais do agente de nuvem é executar tarefas de forma assíncrona, liberando você para cuidar de outras atividades. Vamos aproveitar esse recurso e atribuir outra tarefa ao agente; enquanto ele trabalha, você criará uma funcionalidade no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é dividir o trabalho: você pode se concentrar em uma tarefa enquanto o agente cuida de outra. Vamos atribuir ao Copilot a adição de uma seção de jogos relacionados.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues** e depois **New issue**.
3. Selecione **Blank issue**.
4. Defina **Title** como `Show related games on the game details page`.
5. Defina **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecione **Create**.
7. À direita, selecione **Assign to Copilot** e depois **Assign**.

   ![Atribuição do Copilot à issue](../../../_images/shared-assign-copilot.png)

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando nessa segunda solicitação. Como o agente de nuvem atua de forma parecida com uma pessoa desenvolvedora, não é necessário acompanhá-lo continuamente; você poderá revisar o resultado quando estiver concluído. Enquanto isso, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues para que o Copilot as execute de forma assíncrona. Ele pode resolver dívidas técnicas, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, vamos criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que já exploramos.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

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
