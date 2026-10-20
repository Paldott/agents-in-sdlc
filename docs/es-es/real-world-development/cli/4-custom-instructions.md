---
title: "Lección 4 - Orientar Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, conviene proporcionar esas indicaciones a Copilot. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código. A lo largo de esta lección, usarás Copilot CLI (la interfaz de línea de comandos de Copilot).

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones aplicables a rutas específicas.
- actualizarás el archivo de instrucciones para que se sigan los estándares de código.
- verás el efecto de los archivos de instrucciones en el código.

## Situación

Como cualquier empresa de desarrollo profesional, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, en lugar de repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, sus parámetros y sus valores de retorno con TSDoc/JSDoc, incluido un argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices existentes de formato y lint.

Con los archivos de instrucciones te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo y tus requisitos. Con ellas puedes orientar las sugerencias y los fragmentos de código, por ejemplo, indicando las convenciones de código, las bibliotecas y el tipo de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivos y tareas concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información relevante para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`, archivos con directrices para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones de este proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Antes de modificarlos, explora su contenido y comprueba qué efecto tienen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y observa la breve descripción del proyecto y las directrices de código. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Encontrarás instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones amplios en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

## Actualizar las instrucciones para reflejar las directrices del equipo

Los archivos existentes son un buen comienzo, pero aún falta una indicación. Modificaremos el archivo principal `copilot-instructions.md` para que el nuevo código TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio cuando empieza una conversación. Después de guardar las instrucciones actualizadas, sal de la conversación de filtrado y reanúdala para que Copilot cargue las instrucciones nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si aparece un mensaje, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar nuestras instrucciones y directrices de código. Actualiza el código que has generado para que las siga.
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo obtiene contexto Copilot CLI de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En esta lección:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones aplicables a rutas específicas.
- actualizaste el archivo de instrucciones para que se siguieran los estándares de código.
- viste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para que las pruebas y el lint se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
