---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta o hay información de fondo que Copilot debe conocer, asegúrate de que pueda acceder a ese contexto. Hay varias formas de proporcionar contexto específico a Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que ofrecen directrices para generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de código y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones aplicables a rutas específicas.
- generarás la primera parte de los datos para el filtrado (un helper para los editores) con las instrucciones actuales.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volverás a enviar el mismo mensaje y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás los cambios de instrucciones y la parte del filtrado en `main` para que Copilot pueda aplicar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares definidos. Copilot es probabilístico. El objetivo de este ejercicio es observar la *tendencia* de los cambios en el comportamiento después de actualizar las instrucciones, no obtener un resultado idéntico carácter por carácter.

## Archivos de instrucciones

### Situación

Como cualquier empresa de desarrollo profesional, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

Con los archivos de instrucciones te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo y tus requisitos. Con ellas puedes orientar las sugerencias y los fragmentos de código, por ejemplo, indicando las convenciones de código, las bibliotecas y el tipo de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivos y tareas concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes de Copilot, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices generales.
- Puedes crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivo concretos. Por ejemplo, puedes proporcionar directrices para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> Cuando trabajes en tu entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las sugerencias de finalización de código o de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto el archivo de instrucciones del repositorio como los archivos `*.instructions.md` (con el frontmatter `applyTo`) al generar código.
>
> Copilot también [admite archivos de instrucciones en otros formatos][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Crear archivos de instrucciones queda fuera del alcance de este taller, pero los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices para todo el proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares globales de código.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas de tipos de archivo (pruebas unitarias, componentes de Astro o la capa de datos) o de tareas concretas.
- Utiliza lenguaje natural y directrices claras. Incluye ejemplos de cómo debe ser el código y de cómo no debe serlo.

No hay una única forma correcta de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. La experimentación te ayudará a descubrir qué funciona mejor para cada proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener un conjunto completo de archivos de instrucciones. Al explorar los de este proyecto, puede que encuentres archivos para muchos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de una forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente al entorno que estés usando te indicará cuándo resulte pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Estas se aplican a todas las interacciones con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora su contenido. Encontrarás instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio: define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincidirá con cualquier archivo de pruebas de TypeScript, por ejemplo, uno que coincida con `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones amplios en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero todavía no exige documentar el código. En los pasos siguientes, añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Crear una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para tus cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

Antes de realizar las siguientes tareas, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. Las opciones de modo y modelo varían según el ejercicio; cada paso las indicará cuando sea necesario.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de realizar las siguientes tareas, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. Las opciones de modo y modelo varían según el ejercicio; cada paso las indicará cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, envía primero un mensaje con el archivo de instrucciones actual. Más adelante, actualizarás el archivo y volverás a enviar el mismo mensaje para ver la diferencia.

1. Cierra las pestañas del editor abiertas de ejercicios anteriores para que Copilot solo reciba el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe estar el helper.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todos los editores, con el nombre y el identificador de cada uno. Aplica los cambios a los archivos.
   ```

5. Copilot explorará el proyecto y aplicará cambios al código, a menudo tanto al archivo del helper como a sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el comentario de cabecera de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo pidas. Si ocurre, no pasa nada: el objetivo es observar la mejora en la *coherencia* después de actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Documenta los estándares de código del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de código del proyecto, pero todavía no incluye ninguna regla para documentar el código; por eso el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas Markdown justo después de los estándares existentes para indicar a Copilot que incluya comentarios de cabecera de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para incluir directrices generales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para incluir estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Volver a enviar el mensaje y observar los cambios

Envía el mismo mensaje que antes para observar cómo las nuevas instrucciones influyen en el código que propone Copilot.

1. Vuelve a Copilot Chat y selecciona **New Chat** para vaciar el contexto de la conversación.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo mensaje** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todos los editores, con el nombre y el identificador de cada uno. Aplica los cambios a los archivos.
   ```

4. Observa que el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a los datos de los editores de la plataforma de financiación
    * colectiva de Tailspin Toys. Proporciona funciones para recuperar información
    * de los editores de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editor.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real sobre el que trabajarás en el siguiente ejercicio.

## Confirmar las instrucciones y publicar la rama

Los archivos de instrucciones, como cualquier otro recurso del repositorio, se gestionan con el control de código fuente. Confirma los cambios y publica la rama en el repositorio.

1. Abre una ventana de terminal nueva pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que estén presentes las actualizaciones de instrucciones y del helper mediante:

   ```bash
   git status
   ```

3. Desde el terminal, prepara y confirma las instrucciones actualizadas y todos los archivos que haya modificado Copilot para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios de apoyo (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar un pull request

Una vez publicada la rama, crea un pull request y vincúlalo a la incidencia del backlog sobre los estándares de documentación. Copilot puede hacerlo con las herramientas de GitHub ya conectadas al proyecto: pídele que localice la incidencia, cree un PR para cerrarla y, después, lo combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realizará acciones en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub al espacio de trabajo, así que no tienes que configurar nada. Puede que VS Code te pida iniciar sesión en GitHub la primera vez que Copilot use una de esas herramientas. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia sobre la actualización de los archivos de instrucciones y cree un PR desde la rama actual que describa tanto las instrucciones como la nueva base del helper de editores:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea un pull request desde la rama actual, indica que cierra esa incidencia y añade que también hemos creado la base del helper de editores para el próximo trabajo de filtrado.
   ```

4. Copilot buscará la incidencia y creará el PR.
5. Cuando Copilot te pida **Allow** para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree el PR, pide a Copilot que lo combine y que vuelva a `main` en la copia local con este mensaje:

   ```plaintext
   Combina el PR en main. Después, vuelve localmente a main y descarga el código más reciente para que esté actualizado.
   ```

7. Cuando Copilot te pida **Allow** para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado un pull request con la ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo obtiene Copilot el contexto de los archivos de instrucciones de este proyecto y has usado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar qué produce Copilot con las instrucciones *existentes*.
- añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volver a enviar el mismo mensaje y observar cómo el código propuesto adopta el estándar.
- confirmar los cambios de instrucciones y la base del helper en `main` para que se utilicen en el siguiente ejercicio.

A continuación, pondrás en práctica esas instrucciones en el [modo agente][next-lesson], mientras Copilot añade una funcionalidad en distintas partes del código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
