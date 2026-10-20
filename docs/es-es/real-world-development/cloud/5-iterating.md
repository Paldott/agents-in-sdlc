---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio, has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, desarrollara una función de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera mejoras de accesibilidad, incluidos los controles del modo de alto contraste y del modo claro. Vamos a explorar los cambios de código y, si es necesario, proporcionar comentarios a Copilot para que mejore el resultado.

### Situación

Como ya hemos señalado varias veces, los principios fundamentales del diseño de software y DevOps no cambian con la llegada de la IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisaremos las propuestas de GitHub Copilot para añadir documentación, juegos relacionados y mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y el agente en la nube de GitHub Copilot

El agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, por lo que se han establecido algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para su código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno efímero independiente para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por el primer pull request (PR) que ha generado el agente en la nube de GitHub Copilot: añadir documentación al código. Para revisar el trabajo, usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar el PR, puede que veas una advertencia que indica que un firewall ha bloqueado a GitHub Copilot. **Es lo esperado**: Copilot tiene acceso limitado a los recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar el firewall del agente en la nube de Copilot o deshabilitarlo][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre el PR titulado, por ejemplo, **Add missing documentation** o con un nombre más descriptivo.

> [!NOTE]
> Si Copilot todavía está trabajando en la tarea, el pull request tendrá la etiqueta **[WIP]**. Si es así, espera a que termine. Puede tardar unos minutos; puedes tomarte un descanso o repasar lo que has aprendido hasta ahora.

4. Cuando el pull request esté listo, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otros elementos. Los cambios exactos pueden variar.

   Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones personalizadas que examinaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la cola de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto de la única aplicación Astro se completarán correctamente. El proceso puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en un pull request no es un proceso unidireccional. Puedes mencionar a Copilot en comentarios del pull request o en comentarios insertados en el código, igual que harías con otros miembros del equipo. Copilot los leerá e iniciará otra sesión para resolverlos. Como el resultado es impredecible, no podemos indicar un texto exacto para solicitar los cambios. Estas son algunas ideas:

- Añadir al principio de cada archivo de código comentarios de cabecera con una breve descripción de su función.
- Añadir comentarios TSDoc a los archivos de TypeScript y Astro.
- Crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Usa una de las ideas anteriores u otra sugerencia sobre la documentación que te gustaría ver en el código base.
2. Selecciona **View Session** para ver cómo trabaja Copilot. Observa que inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver al pull request.

   ![Volver al pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado los cambios, debería aparecer un commit nuevo en el pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando hayas terminado, puedes marcar el PR en borrador como listo para revisión y combinarlo en la rama principal.

![Marcar el PR como listo para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Vuelve al PR que ha generado Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona el PR titulado, por ejemplo, **Show related games on the game details page** o con un nombre más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la cola de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto de la única aplicación Astro se completarán correctamente. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve a tu Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube de Copilot para pedir cambios. Por ejemplo, puedes solicitar que ajuste el número de juegos relacionados o que añada comentarios de cabecera y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Como antes, puedes pedir estos cambios mediante un comentario nuevo en la pestaña **Conversation**; Copilot lo leerá e iniciará una sesión nueva.

## Revisar las mejoras de accesibilidad

Por último, revisaremos las mejoras de accesibilidad implementadas con el agente personalizado de accesibilidad. Este PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde Mission Control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona el PR titulado, por ejemplo, **Add high contrast mode to website** o con un nombre más descriptivo.

> [!NOTE]
> Si Copilot todavía está trabajando en la tarea, el pull request tendrá la etiqueta **[WIP]**. Si es así, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación, prestando especial atención a:
   - Los componentes de la interfaz de usuario que permiten alternar entre los modos.
   - El uso del almacenamiento local para conservar las preferencias del usuario.
   - Los cambios de CSS o de estilo para los modos de alto contraste y claro.
   - Los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - El código JavaScript/TypeScript que controla el cambio de modo.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la cola de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto de la única aplicación Astro se completarán correctamente. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las mejoras de accesibilidad. Ve a tu Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan correctamente y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar estas mejoras siguiendo prácticas recomendadas de accesibilidad. Si detectas problemas o posibles mejoras de accesibilidad, puedes mencionar a **@copilot** en un comentario para solicitar actualizaciones, igual que hiciste con los PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube funciona mejor cuando puedes asignarle tareas reales del backlog y ocuparte de otros asuntos. Para acostumbrarte a este flujo, registra algunas incidencias más en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario de interés para patrocinadores en la página de detalles del juego.
- Implementar la paginación en la página de listado de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizadas incluidos en este repositorio** para ver su efecto posterior en el resultado del agente en la nube.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo configuraba su entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** de accesibilidad para añadir controles de modo de alto contraste y modo claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad en curso.
- **Revisaste e iteraste sobre los pull requests del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado aquí:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de control de calidad, Playwright MCP, Agent Merge y un puente opcional con `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
