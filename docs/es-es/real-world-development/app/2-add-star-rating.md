---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, realiza un pequeño cambio en las tarjetas de los juegos y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Después de explorar el espacio de trabajo y utilizar un chat rápido, iniciarás una **sesión de agente** y harás tu primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración por estrellas en sus datos, pero las tarjetas de la página de inicio todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una sesión de agente y aprenderás cómo se estructura;
- pedirás al agente que realice un cambio pequeño y específico en el proyecto;
- revisarás el cambio en la vista de diferencias del espacio de trabajo;
- ejecutarás la aplicación en local para comprobar el cambio en el navegador;
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta. Es un cambio pequeño y autónomo, perfecto para tu primera sesión.

## Anatomía de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión recibe un **árbol de trabajo y una rama de Git dedicados**, lo que permite ejecutar varias sesiones a la vez —por ejemplo, una para añadir una función y otra para corregir un error— sin que sus cambios entren en conflicto. Las sesiones aparecen en la barra lateral, agrupadas por repositorio. Selecciona una para cambiar a ella.

Dentro de una sesión verás tres elementos: la **conversación** con el agente, la **actividad de las herramientas** mientras explora y edita archivos, y la lista de **archivos modificados** con sus diferencias.

## Iniciar una sesión y pedir el cambio

Empecemos una sesión nueva para explorar el proyecto e implementar la funcionalidad. Durante la [configuración de la aplicación][prior-lesson], añadiste el proyecto desde su repositorio de GitHub. Crearemos una sesión para ese repositorio y solicitaremos el cambio.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona `tailspin-toys` como repositorio.
4. Elige un **new working tree** y el modo **Interactive** debajo del campo de prompt. Pide el cambio con este prompt:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pulsa <kbd>Enter</kbd> para enviar el prompt a Copilot.

La aplicación Copilot empieza por crear un árbol de trabajo nuevo, una copia aislada del proyecto. Después, explora el proyecto, localiza los archivos que debe actualizar para añadir la función y crea el código necesario. Ya has añadido una funcionalidad con la aplicación Copilot.

## Revisar las diferencias

Todos los cambios generados por IA deben revisarse antes de combinarlos, incluso los más pequeños. Vamos a explorarlos directamente en la aplicación Copilot.

1. En la esquina superior derecha de la aplicación, selecciona **Toggle review panel**. En el panel que se abre, selecciona **Changes** para ver los archivos modificados y sus diferencias.

    ![Barra de herramientas superior de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel, a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Deberías observar código añadido a `GameCard.astro`, el archivo principal que se utiliza para mostrar los detalles de los juegos. Debería ser similar al siguiente: un bloque pequeño que muestra la valoración cuando existe y utiliza `No rating yet` cuando `starRating` es `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como Copilot y todas las herramientas de IA generativa son probabilísticos y no deterministas, el código concreto puede variar respecto al ejemplo. No obstante, debería ser relativamente parecido.

## Comprobar los cambios

Antes de abrir el navegador, revisa los resultados de las comprobaciones automatizadas del agente. Confirma que las pruebas cubren tanto un valor numérico de `starRating` como la alternativa para `null`. Un requisito previo ausente o una comprobación omitida no cuentan como superados; revisa cualquier solicitud de instalación antes de aprobarla.

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que abra el sitio web para examinar la interfaz actualizada en un lienzo del navegador.

> [!TIP]
> Un lienzo es un widget interactivo integrado en la aplicación Copilot. Más adelante explorarás lienzos personalizados e incluso crearás uno, pero por ahora utilizaremos el lienzo de navegador integrado.

1. Utiliza este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de juegos con valoración muestran su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión y cierre el lienzo:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

Ya has creado la función. Ahora crearás una solicitud de incorporación de cambios (PR) para añadir el código al proyecto.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las indicaciones para autenticarte.
3. Si Copilot explica que la directiva del repositorio exige una incidencia vinculada antes de crear la PR, confirma que debe seguir ese proceso.
4. Copilot empezará a crear la PR.
5. Selecciona la burbuja **PR** que aparece justo encima del chat para abrir la solicitud en el panel de revisión. Puedes revisar la PR desde allí.
6. Cuando estés conforme, selecciona **Ready to merge**.
7. En el nuevo cuadro de diálogo, selecciona **Merge pull request** para combinar la solicitud.

## Resumen y pasos siguientes

¡Enhorabuena! Has publicado tu primer cambio con la aplicación GitHub Copilot. En concreto:

- has iniciado una sesión de agente y aprendido cómo se estructuran las sesiones;
- has pedido al agente que haga un cambio pequeño y específico en las tarjetas de juegos;
- has revisado el cambio en la vista de diferencias del espacio de trabajo;
- has iniciado la aplicación en local y confirmado la valoración por estrellas en el navegador;
- has abierto la PR 1, revisado sus comprobaciones y la has combinado explícitamente.

A continuación, [empezarás con la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para desarrollar una función más completa.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instalar-y-configurar-la-aplicacion-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
