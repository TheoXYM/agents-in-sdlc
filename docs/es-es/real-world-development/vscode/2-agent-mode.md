---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo habitual de una persona desarrolladora incluye localizar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, encontrar el archivo que falta y volver a modificarlo. Y así sucesivamente.

El modo agente de Copilot te ayuda con este proceso.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a una persona desarrolladora: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. En este ejercicio, lo utilizarás para añadir una funcionalidad al sitio web.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube están diseñados para experiencias diferentes. El modo agente trabaja en tu IDE y permite recibir comentarios e interactuar rápidamente. El agente en la nube funciona de manera asíncrona como una persona colaboradora del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo explora el proyecto el modo agente de Copilot, identifica archivos pertinentes y realiza cambios coordinados.
- cómo implementa funcionalidades en la interfaz de usuario y la capa de datos.
- cómo revisar los cambios y las pruebas generados por Copilot antes de combinarlos en el repositorio.

## Escenario

A medida que crece la lista de juegos, quieres permitir que las visitas los filtren por categoría y editorial. En el ejercicio anterior ya añadiste un asistente para consultar las editoriales. Ahora completarás los cambios restantes en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de cambiar nada, explora el sitio web de Tailspin Toys para conocer su funcionalidad actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar los juegos y sus detalles. Es una única aplicación Astro que genera las páginas en HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en los metadatos de Astro; no hay una API de backend independiente ni un framework de interfaz del lado cliente. Las funciones auxiliares reutilizables para acceder a los datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con las API estándar del DOM.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre otro terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace `localhost` del terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en marcha, explora sus funcionalidades principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; se indica dónde corresponde.

La primera versión del sitio funciona, pero queremos añadir capacidades. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generaron incidencias automáticamente. Pide a GitHub Copilot que las muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal e invoca herramientas de forma autónoma.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo frente a otro. Durante su creación se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes y ofrecieran una experiencia adecuada. Como los modelos de lenguaje no son deterministas, es normal que los resultados difieran de los ejemplos del taller.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot puede utilizar un comando MCP diferente y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog con este mensaje:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según los que considere más útiles para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas generales para añadir funcionalidades, incluidas:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para tipos de archivos como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a los archivos del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizar instrucciones, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- añadir o ajustar la lógica restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de listado de juegos con la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas.

1. Continúa la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** continúa seleccionado en el selector de agentes de la vista Chat.
4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes en la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explora el proyecto, modifica archivos y ejecuta pruebas. Según la complejidad de la tarea, puede tardar varios minutos. Mientras trabaja, puede que aparezcan los controles **Keep** y **Undo** en el editor. No tienes que seleccionarlos mientras el agente trabaja; cuando termine, podrás decidir qué hacer con todos los cambios.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del panel Copilot Chat con una solicitud para confirmar que se ejecuten las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot haga pausas y que algunas pruebas fallen durante el proceso. Es normal: alternará entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completada de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot y por cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! Has utilizado el modo agente de GitHub Copilot para añadir funcionalidades al sitio web de Tailspin Toys. En este ejercicio has aprendido:

- cómo implementar funcionalidades en la interfaz de usuario y la capa de datos.
- cómo explora el proyecto el modo agente, identifica archivos pertinentes y realiza cambios coordinados.
- cómo revisar los cambios y las pruebas generados por Copilot antes de combinarlos en el repositorio.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que implemente la paginación. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación y si la persona usuaria puede seleccionar el tamaño de página o si este será fijo.
- incluye en el mensaje toda la orientación necesaria para obtener la implementación que quieres.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto; es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
