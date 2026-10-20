---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya muy pocas organizaciones que no tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de pull request (PR) para que lo revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en aspectos más generales.

Con el agente en la nube de Copilot explorarás cómo:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- crear incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Situaciones

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: verás que no hay ninguna. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados —otros títulos de la misma categoría— para que los usuarios puedan seguir explorando. Todavía no necesitan una implementación pulida; solo algo que puedan usar para probar la experiencia de usuario (UX). Aunque esto bloquea el trabajo, hay otras incidencias con más prioridad en este momento.

Estas tareas suelen perder prioridad con facilidad y son ideales para asignarlas al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar el trabajo y comprobar que cumple las expectativas.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que trabajarías con otro desarrollador, puedes interactuar de distintas formas, como [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Tras recibir la asignación, Copilot crea un borrador de pull request para hacer seguimiento del progreso, configura un entorno y empieza a trabajar. Puedes explorar la sesión mientras sigue activa o cuando haya terminado. Cuando la solución esté lista para revisarla, Copilot te mencionará en el pull request.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes saltarte la fase de reflexión. Incluso las tareas que parecen sencillas se vuelven complejas al examinarlas en detalle, así que [define cuidadosamente el alcance de las tareas que asignes al agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el proceso. La llegada de la IA generativa no cambia los principios fundamentales del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores, has descubierto las instrucciones personalizadas y cómo guían a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, examina en modo de solo lectura los archivos de instrucciones incluidos en este repositorio para reconocer su efecto más adelante.

Abre los siguientes archivos en la interfaz web de GitHub de tu repositorio o en un Codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la siguiente sección, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los comentarios de cabecera que aparezcan en el pull request. Proceden de estos archivos de instrucciones. Volveremos a señalarlo al revisar los PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube de Copilot

La creación de código suele requerir un entorno específico y la ejecución de ciertos scripts para dejarlo todo listo, independientemente de quién realice el trabajo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo creando un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecutará antes de que empiece el trabajo. Así, el agente tendrá acceso a las herramientas de desarrollo y dependencias necesarias. Este archivo ya está configurado para facilitar el desarrollo del laboratorio y ofrecer esta oportunidad de aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y poblada para la única aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna los permisos mínimos que necesiten tus pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración deben clonar el repositorio, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (la aplicación completa ahora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga los datos de la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo parece cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunas características importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Este se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en el pull request.
- Observa el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que se ejecuta correctamente sin esperar a que Copilot lo inicie.

## Añadir documentación

Todo el mundo reconoce la importancia de la documentación, pero la mayoría de los proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica a menudo se queda sin resolver, lo que reduce la productividad y dificulta el mantenimiento del código base o la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, así que esta es una incidencia perfecta para asignársela al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior, volverás para revisar el resultado.

1. En una pestaña nueva del navegador, ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. En **Description**, escribe lo siguiente:

   ```plaintext
   Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando resulten útiles. Lamentablemente, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios de documentación donde ayuden a aclarar el comportamiento.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre el pull request (PR) recién creado. Su título será parecido a `[WIP]: Code lacks documentation`. Si no aparece en la lista, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas pendientes.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en el PR. Copilot está creando su entorno (ejecuta el flujo de trabajo descrito anteriormente), analizando el proyecto y determinando el mejor enfoque para resolver la tarea.

13. Revisa la lista y las tareas que va a realizar.
14. Desplázate hacia abajo por la cronología del pull request y busca una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema. Explora el código y su estado, observa cómo se detiene para reflexionar y decidir un plan adecuado, y cómo genera código.

Es probable que el proceso tarde varios minutos. Una de las principales ventajas del agente en la nube de Copilot es que puede realizar tareas de forma asíncrona, mientras tú te centras en otras. Aprovecharemos esa función para asignarle otra tarea y, después, nos ocuparemos de escribir código para añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: puedes centrarte en unas tareas mientras el agente se ocupa de otras. Puede que añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, pero podrías emplearlo en otras tareas. Asígnasela al agente en la nube de Copilot.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. En **Description**, escribe lo siguiente:

   ```markdown
   Queremos ayudar a los usuarios a descubrir más juegos mostrando títulos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego determinado, sin incluir ese juego
   - Mostrar en la página de detalles una sección «Juegos relacionados» que utilice el helper
   - Gestionar el caso en el que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo helper
   - Asegurarse de que se superan todas las pruebas antes de crear el PR
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, debería aparecer un emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot indica que está trabajando en la incidencia con el emoji de los ojos](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube de Copilot trabaja de forma parecida a un ingeniero de software, así que no hace falta supervisarlo constantemente; revisa el resultado cuando haya terminado. Ahora vamos a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias a Copilot para que trabaje de forma asíncrona. Así, el agente puede resolver deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- crear incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes centrarte en crear y utilizar agentes personalizados. El [agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] y admite instrucciones personalizadas, que hemos explorado en módulos anteriores.

## Recursos

- [Información sobre el agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
