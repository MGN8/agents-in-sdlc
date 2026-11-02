---
title: "Exercício 2 - Adicione funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo podem exigir alterações em vários arquivos e tarefas como executar testes. Em geral, uma pessoa desenvolvedora precisa localizar os arquivos relevantes, alterar o código, executar testes, depurar, identificar o que ficou faltando e fazer novas alterações. A lista continua.

É para isso que serve o modo agente do Copilot.

O modo agente foi criado para trabalhar com mais autonomia no IDE. Ele explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos usar o modo agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo agente e o agente de nuvem oferecem experiências diferentes. O modo agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem atua de forma assíncrona, como parte da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- implementar recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, vai concluir o trabalho restante na camada de dados, na interface e nos testes usando o modo agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site para entender os recursos atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele lista jogos e exibe seus detalhes. O site é um único aplicativo Astro, que renderiza páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local com Drizzle ORM; não há uma API de backend separada nem um framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa um `<script>` Astro pequeno e isolado com APIs padrão do DOM.

### Inicie o site

Para facilitar a execução, o projeto inclui a tarefa `npm run dev`, que inicia o servidor de desenvolvimento do Astro.

1. Volte ao Codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> No Codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor.

### Explore o site

Os principais recursos do site incluem:

- **Página inicial**: mostra uma lista de jogos de tabuleiro, com títulos, imagens e descrições.
- **Página de detalhes do jogo**: mostra mais informações sobre um jogo, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício; as etapas indicarão quando for relevante.

O site já funciona, mas queremos ampliar os recursos. Vamos começar revisando o backlog, criado automaticamente quando você configurou o projeto a partir do modelo.

1. Selecione **Agent** no menu de agentes da visualização Chat. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela do seletor de agente no Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao desenvolver o workshop para que as sugestões fossem relativamente consistentes. Como os modelos de linguagem são probabilísticos, as sugestões podem ser diferentes dos exemplos, o que é normal e esperado.

> [!NOTE]
> Por causa do caráter probabilístico dos modelos de linguagem, o Copilot pode usar outro comando MCP, mas ainda deverá concluir a tarefa.

3. Peça ao Copilot que mostre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que ele liste as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto. Vamos usar o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades:
   - a arquitetura.
   - os princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, o que permite reutilizar conteúdo, dividir orientações complexas e fornecer exemplos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações:

- adicionar ou atualizar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento dos filtros.
- atualizar a página de listagem para incluir os controles de filtragem.

Os testes também devem ser aprovados antes da mesclagem. O modo agente pode cuidar dessas tarefas.

1. Continue a conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** está selecionado no menu de agentes.

   ![Captura de tela do seletor de agente no Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente a funcionalidade com base na issue:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot explorando o projeto, localizando os arquivos relevantes, alterando o código e executando testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos. Durante o processo, os controles **Keep** e **Undo** podem aparecer no editor. Aguarde o fim do trabalho para decidir se aceita ou desfaz as alterações.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela do painel Copilot Chat pedindo confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas temporárias nos testes. O Copilot alterna entre gerar código e executar testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão concluída do modo agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore o novo recurso.
11. Quando confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, usamos o modo agente do GitHub Copilot para adicionar recursos ao site da Tailspin Toys. Você aprendeu como:

- o modo agente implementa recursos na interface e na camada de dados.
- o Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar alterações e testes antes de mesclá-los à base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus — implemente paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use o que aprendeu neste exercício e peça ao Copilot que atualize o site. Considere:

- seguir as boas práticas existentes e os arquivos de instruções.
- decidir se as pessoas podem selecionar o tamanho da página ou se ele será fixo.
- fornecer ao Copilot as orientações necessárias no prompt.
- iterar com o GitHub Copilot, solicitando alterações e adicionando contexto.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
