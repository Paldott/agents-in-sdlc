---
title: "Lección 7 - Crear y usar un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright del Model Context Protocol (MCP) para observar la experiencia de filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás e inspeccionarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de control de calidad y revisarás sus conclusiones según la incidencia de filtrado.

## Situación

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir un pull request (PR). Un agente personalizado puede coordinar ese proceso de control de calidad y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito del agente, sus instrucciones y las herramientas disponibles. En este taller, definirás un rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen distintas funciones. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de control de calidad indica a Copilot cómo utilizar estas capacidades para evaluar los requisitos y comunicar los resultados. No sustituye a esas herramientas ni requiere otra conversación.

## Crear el perfil de control de calidad

Antes de abrir el PR de la funcionalidad, pedirás a Copilot que cree un perfil reutilizable de control de calidad. El perfil definirá las comprobaciones y los límites que deberá respetar.

1. Vuelve a tu Codespace y confirma que la conversación sobre el filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el nuevo agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas cuando falte cobertura.

   Pídele que indique para cada requisito si se ha superado, ha fallado o está bloqueado, y que aporte pruebas. Debe pedir permiso antes de modificar el código de implementación y no debe crear commits ni abrir pull requests.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Inspeccionar el perfil

Antes de usar el nuevo agente, revisa su perfil para confirmar que Copilot ha recogido el flujo de control de calidad y los límites previstos. Así evitarás que un perfil incompleto o demasiado amplio cambie la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo proporciona un nombre visible claro para el agente.
3. Lee las instrucciones del perfil y confirma que el agente empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que comunica pruebas que respaldan sus resultados, pide permiso antes de cambiar el código de implementación y no crea commits ni abre pull requests.
5. Si al perfil generado le falta alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecutar el control de calidad según la incidencia

Copilot CLI (la interfaz de línea de comandos de Copilot) carga los agentes del proyecto al iniciar una conversación. Reanuda la misma conversación sobre el filtrado después de crear el perfil y, a continuación, selecciona el agente de control de calidad para que pueda utilizar la incidencia y las decisiones de planificación que ya tiene como contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Está creado y el comando anterior lo activará.

2. Usa este mensaje para pedir al agente que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para un PR?
   ```

3. Espera a que el agente de control de calidad termine su trabajo.
4. Lee el informe que te proporcione.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y revisado su trabajo. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creaste e inspeccionaste un perfil reutilizable de control de calidad.
- seleccionaste el agente de control de calidad y revisaste sus conclusiones según la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de control de calidad, las pruebas y el informe de verificación listos para revisarlos. A continuación, [reunirás todo en un PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
