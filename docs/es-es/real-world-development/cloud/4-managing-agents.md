---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has asignado al agente en la nube de Copilot tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma asíncrona y autónoma, sigue siendo importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen pull requests (PR) abiertos, consultar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Situación

Tras asignar al agente la creación del modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Ya se está actualizando el estilo del sitio y añadiendo controles para alternar entre modos, por lo que parece lógico incluir esta función. Orientarás el trabajo del agente para que añada también el modo claro.

## Revisar las tareas del agente en la nube de Copilot

Consulta el estado actual de todas las tareas que se han asignado al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías encontrar las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código base.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones que ha realizado Copilot. Por ejemplo:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar los distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel del pull request (PR), que aparece a la derecha. Desde ahí puedes consultar el PR y los archivos modificados para supervisar el trabajo.

## Orientar al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la creación del modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado actual del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

   ```plaintext
   Ya que estamos trabajando en un modo de alto contraste, añadamos también un modo claro. Este modo debe tener su propio control para que los usuarios puedan elegir la presentación que prefieran.
   ```

   ![Tarea del agente en la nube en la página de agentes, con el cuadro de diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot confirma que ha recibido el mensaje y lo incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso concreto que estaba realizando al recibir el mensaje.

Este proceso también tardará varios minutos. Aprovecha el tiempo para repasar y reflexionar sobre lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde Mission Control puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir tareas en curso para ajustar su alcance o proporcionar indicaciones adicionales.

Has explorado estos conceptos:

- exploraste Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las mejoras de accesibilidad, puedes [iterar sobre los pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
