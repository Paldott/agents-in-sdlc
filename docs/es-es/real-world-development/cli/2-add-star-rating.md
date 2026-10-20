---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para realizar un pequeño cambio en las tarjetas de juegos, revisarlo en el navegador mediante un puerto reenviado y combinarlo como tu primer pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado GitHub Copilot CLI, la interfaz de línea de comandos (CLI) de GitHub Copilot, y has probado una conversación. Ahora realizarás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero las tarjetas de la página principal todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primer pull request.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad.
- pedirás al agente que realice un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para confirmar el resultado en el navegador mediante un puerto reenviado.
- abrirás y combinarás tu primer pull request.

## Situación

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de juegos de la página principal solo muestran el título, la categoría, el editor y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño e independiente, perfecto para tu primera sesión.

## Estructura de una conversación

Una **conversación** es el espacio en el que trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que tengas actualmente en el terminal, en lugar de crear un worktree dedicado. Las conversaciones guardadas te permiten volver a la misma discusión; los archivos y la rama siguen siendo estado normal de Git en el disco.

En una conversación verás tres elementos: tus mensajes y las respuestas del agente; la actividad de las herramientas que utiliza para explorar y editar archivos; y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y solicitar el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si aún no está abierto, inicia Copilot con el siguiente comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva escribiendo el comando `/new` y pulsando <kbd>Enter</kbd>.
5. Usa el siguiente mensaje para solicitar el cambio:

   ```plaintext
   Muestra el `starRating` de cada juego sobre 5 en las tarjetas de la página de listado. Si la valoración es `null`, muestra "No rating yet". Mantén el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisar los cambios

Todos los cambios generados por inteligencia artificial (IA) deben revisarse antes de combinarlos, aunque sean pequeños. Revisemos los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los fallos.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la pantalla de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, funciona de forma probabilística, no determinista. El código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprobar los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para examinar la interfaz de usuario (UI) actualizada en el navegador a través del puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de la valoración por estrellas. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abrir y combinar tu primer pull request

Ya has creado la funcionalidad. Ahora crearás un pull request (PR) para incorporar el código al proyecto.

1. Pide al agente predeterminado que cree un commit para el cambio:

   ```plaintext
   Crea un commit con los cambios revisados de la valoración por estrellas y un mensaje de commit adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI enviará el commit existente al crear el PR y mostrará la dirección URL del PR.
3. Abre el PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu copia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has entregado tu primer cambio con GitHub Copilot CLI. En esta lección:

- iniciaste una conversación de Copilot centrada en una rama de funcionalidad.
- pediste al agente que realizara un pequeño cambio en las tarjetas de juegos.
- revisaste el cambio con `/diff`.
- ejecutaste la aplicación y confirmaste la valoración por estrellas en el navegador a través del puerto reenviado.
- abriste y combinaste tu primer pull request.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para desarrollar una funcionalidad más amplia.

## Recursos

- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
