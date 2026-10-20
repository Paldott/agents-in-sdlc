---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta, hacer otra actualización... y así sucesivamente.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura existente del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que detecta. Veamos cómo utilizar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube están diseñados para experiencias distintas. El modo agente trabaja en el IDE y permite iterar con rapidez e interactuar. El agente en la nube está pensado para trabajar de forma asíncrona, como un compañero del equipo, e interactuar contigo mediante incidencias y pull requests.

En este ejercicio, aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, localizar los archivos pertinentes y coordinar cambios.
- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinar el código.

## Situación

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya añadiste un helper para los editores. Ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de realizar cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo de software. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera sus páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz de cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`. La interactividad se añade con un `<script>` pequeño y acotado de Astro que utiliza las API estándar del DOM.

### Iniciar el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro para que puedas ejecutar el sitio fácilmente en GitHub Codespaces.

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre una ventana de terminal nueva en el Codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web con el siguiente comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la dirección URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir `http://localhost:4321` en una ventana del navegador, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor de Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar un enlace a la dirección local desde el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Este túnel privado conecta con el servidor web que ahora se ejecuta en tu Codespace.

### Explorar el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de realizar las tareas siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. Las opciones de modo y modelo varían según el ejercicio; cada paso las indicará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos añadir nuevas capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te las muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y utiliza herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo sobre otro. Durante su creación utilizamos Claude Sonnet 4.5 y lo hemos incluido en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia resulte satisfactoria. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, puede que las sugerencias que recibas sean distintas de las que se describen en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog mediante este mensaje:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las tareas según lo útiles que cree que serán para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidades al sitio web.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidades. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivo, entre ellos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar contenido, dividir instrucciones complejas en fragmentos más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios en la aplicación:

- Añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento del filtrado.
- Actualizar la página de listado de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código. El modo agente de Copilot puede encargarse de estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Actualiza el sitio para permitir filtrar por editor y categoría según los requisitos de la incidencia correspondiente del backlog de GitHub. Ya existe un helper para editores del ejercicio anterior; consérvalo y perfecciónalo si es necesario mientras completas el trabajo restante de la capa de datos, la interfaz y las pruebas. Antes de terminar, asegúrate de que se superan todas las pruebas. El servidor ya está en ejecución, así que no hace falta que lo inicies.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería encontrar los helpers de la capa de datos, la interfaz y las pruebas. A continuación, empezará a modificar archivos y a ejecutar las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, aparecerá una opción **Keep** o **Undo** para cada cambio; no hace falta que selecciones nada mientras el trabajo sigue en curso.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión de Copilot Chat con el modo agente completada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genere Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has explorado cómo usar el modo agente de GitHub Copilot para añadir capacidades al sitio web de Tailspin Toys. Has aprendido que:

- El modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz y la capa de datos.
- El modo agente de Copilot puede explorar el proyecto, localizar los archivos pertinentes y coordinar cambios.
- Debes revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás un pull request.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Usa lo que has aprendido en este ejercicio y pide a Copilot que actualice el sitio. Ten en cuenta estas cuestiones:

- Sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes.
- Decide cómo quieres implementar la paginación: por ejemplo, si los usuarios podrán elegir el tamaño de página o si será fijo.
- Al redactar el mensaje, proporciona a Copilot las indicaciones necesarias para que la implementación cumpla tus expectativas.
- Es posible que tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. Es normal trabajar así con Copilot.

## Recursos

- [Modos de Copilot para preguntar, editar y trabajar como agente: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
