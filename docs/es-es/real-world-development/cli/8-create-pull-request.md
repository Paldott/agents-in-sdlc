---
title: "Lección 8 - Crear y combinar el pull request de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas; después, crea un PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlos juntos y abrir un pull request. Combinaste el pull request (PR) de la valoración por estrellas manualmente; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, dividiríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de control de calidad en varios PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sola conversación y rama, y todo ese trabajo formará parte de este PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás el PR de filtrado.
- habilitarás Agent Merge solo después de revisar el PR y confirmarás que se ha combinado.

## Situación

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante del PR y mantener el control de autorización de la combinación en manos del desarrollador. Para realizar el trabajo, usarás Copilot CLI (la interfaz de línea de comandos de Copilot).

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para incorporar un pull request. Cuando lo habilitas, Copilot resuelve lo que bloquea el PR —como corregir comprobaciones de integración continua (CI) fallidas, responder a comentarios de revisión y rebasar la rama cuando sea necesario— y, después, habilita la combinación automática de GitHub cuando el repositorio lo permite.

Hasta ahora, has seleccionado **Merge pull request** personalmente. Agent Merge puede asumir esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usar Agent Merge para gestionar el PR

Con todo el código preparado, revisémoslo, creemos el PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea un PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua del PR. Cuando todo se haya completado correctamente, realizará la combinación.
7. Comprueba que Copilot muestra un mensaje similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar el proceso del pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionaste el cambio completo de la funcionalidad y las pruebas de verificación.
- creaste el PR de filtrado.
- habilitaste Agent Merge solo después de revisar el PR y confirmaste que se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para trabajar con el contexto, los modelos, las sesiones compartidas y la delegación opcional a la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
