---
title: "Lección 10 - Recapitulación y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, los dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI, la interfaz de línea de comandos (CLI) de GitHub Copilot, en un flujo de trabajo continuo para Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado preparada.
- añadiste las valoraciones por estrellas, revisaste el resultado en el navegador mediante un puerto reenviado y combinaste manualmente tu primer pull request (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en el modo Plan, lo implementaste en el modo Autopilot y lo revisaste en el modo Interactive.
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Playwright del Model Context Protocol (MCP) y lo usaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisaste el cambio completo de filtrado y autorizaste Agent Merge para el PR de filtrado.
- exploraste comandos de barra para gestionar el contexto, los modelos, las sesiones compartidas y la delegación opcional a la nube.

## Lo que has entregado

El taller consta de dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones por estrellas:** muestra `starRating` existente y un estado explícito para los juegos sin valoración en las tarjetas.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de control de calidad e incluye las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos combinado ese trabajo en un solo PR para agilizar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionando el navegador y explorando el sitio con Copilot a través de MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y generó un informe con el nuevo formato. El agente de control de calidad combinó esos resultados con una revisión de los requisitos y la cobertura de pruebas antes de crear el PR.

Las pruebas añadidas deben cubrir carencias reales; también puede ser correcto que una ejecución de control de calidad no requiera pruebas nuevas. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las evidencias afectadas después de realizar cambios.

## Procedimientos recomendados

El contexto y las herramientas que proporcionas a Copilot determinan cómo trabaja. En este taller, actualizaste las instrucciones, personalizaste una habilidad, creaste un perfil de control de calidad y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir el enfoque antes de empezar, **Interactive** para seguir de cerca los cambios concretos y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para las modificaciones habituales y uno más capaz para el trabajo complejo.

El contexto también es tan importante como la infraestructura. Describir con claridad *qué* quieres desarrollar, *por qué* y *cómo* influye considerablemente en el resultado.

## Más opciones para explorar

Has completado el flujo de trabajo principal. Aquí tienes otras funciones de CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para hablar de un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea independiente.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, proyectos personales o esa aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprende de los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Incorporar Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
