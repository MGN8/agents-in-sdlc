---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y utilízalo para explorar en un navegador la función de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos indicado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos y permitir que Copilot utilice automatizaciones adicionales. Para ello sirven los servidores MCP: amplían las funciones integradas de la CLI y proporcionan herramientas y servicios adicionales.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- añadirás el servidor MCP de Playwright si todavía no está disponible;
- pedirás al agente que maneje un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot pueda utilizar el sitio web como lo haría una persona y automatizar así más partes del proceso, lo que te dará más confianza en que los cambios funcionan como esperas.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Las herramientas y los recursos se utilizan a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios y herramientas externos. Cada servidor ofrece al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[Servidor MCP de GitHub][github-mcp]:** proporciona acceso a API para gestionar repositorios de GitHub, incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright][playwright-mcp-server]:** proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub mantiene un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si todavía no está disponible, añade el servidor MCP de Playwright para que Copilot pueda interactuar con el sitio web como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve al codespace.
2. Abre el cuadro de diálogo para añadir un servidor MCP escribiendo este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Comprueba que el tipo de servidor es **STDIO**. Pulsa <kbd>Enter</kbd> y, después, <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pedir a Copilot que explore la función con Playwright

Antes has comprobado manualmente que la funcionalidad funciona como esperas. Ahora utilizarás el servidor MCP de Playwright disponible en Copilot para repetir la comprobación.

1. Envía este prompt para pedir a Copilot que use el servidor MCP de Playwright y valide la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No hace falta pedir explícitamente a Copilot que utilice el servidor MCP: normalmente lo descubrirá por su cuenta. Aun así, como sabes cuál debería utilizar, puedes indicárselo. Así aumentas la coherencia de los resultados y ahorras algunos tokens.

2. Observa los pasos que sigue Copilot en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y comprueba que todo se comporta como esperas.

Copilot iniciará el servidor, interactuará con el sitio web mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el servidor MCP de Playwright desde Copilot CLI para explorar tu funcionalidad en un navegador real. En esta lección:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- has añadido el servidor MCP de Playwright si todavía no estaba disponible;
- has pedido al agente que maneje un navegador y explore la función de filtrado.

A continuación, crearás un [agente personalizado de control de calidad][next-lesson] que combine la habilidad y las herramientas del navegador en un perfil especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
