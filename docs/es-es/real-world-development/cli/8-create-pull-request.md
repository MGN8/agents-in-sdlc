---
title: "Lección 8 - Crear y combinar la PR de la función"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, el cambio de la habilidad, el perfil de control de calidad y las pruebas están guardados en una rama. Ahora hay que revisarlos conjuntamente y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo normal sería separar la función, las actualizaciones de instrucciones, el cambio de la habilidad y el agente de control de calidad en varias PR. Para agilizar el taller, has mantenido todo el trabajo de filtrado y calidad en una sola conversación y rama, y lo incluirás en una única PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionarás el cambio completo de la función y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge únicamente después de revisar los cambios y confirmarás que se ha combinado la PR.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el resto del trabajo de la PR y mantener el control del desarrollador sobre la autorización de la combinación.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a establecer la base cuando hace falta. Después, habilita la combinación automática de GitHub si el repositorio la admite.

Hasta ahora has seleccionado tú mismo **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitarlo.

## Utilizar Agent Merge para gestionar la PR

Ya has creado todo el código. Vamos a revisarlo, crear la PR y permitir que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro de diálogo de agentes escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud de incorporación de cambios con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR. Cuando todas las comprobaciones se superen, realizará la combinación.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo y, si la configuración del repositorio lo permite, completa manualmente la combinación después de revisar los cambios.

## Resumen y siguientes pasos

Has automatizado varios aspectos del desarrollo: generar código, probarlo y validarlo, y gestionar las solicitudes de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- has inspeccionado el cambio completo de la función y las pruebas de verificación;
- has creado la PR de filtrado;
- has habilitado Agent Merge únicamente después de revisar los cambios y has confirmado la combinación de la PR.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
