---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] son una de las formas principales de proporcionar orientación sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos aplicables a rutas concretas.
- generarás la primera parte de los datos para el filtrado (una función auxiliar de editoriales) con las instrucciones actuales.
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- volverás a enviar el mismo mensaje y observarás cómo el código generado sigue el estándar nuevo.
- confirmarás la actualización de instrucciones y la parte del filtrado en `main` para que Copilot utilice las nuevas pautas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no cumplir todos los estándares establecidos. Copilot no es determinista. El objetivo es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener siempre el mismo código exacto.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene pautas y requisitos para sus prácticas de programación. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su contenido.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para ayudarle a entender mejor tu estilo y tus requisitos de programación. Permiten orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan pautas para tipos de archivos, lenguajes o tareas concretos.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código con Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también admite otros formatos de archivos de instrucciones, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con las instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque representativo; el taller no pretende explicar en detalle cómo crear archivos de instrucciones. En términos generales:

- Limita `copilot-instructions.md` a pautas para todo el proyecto, como la descripción de lo que se desarrolla, la estructura del proyecto y los estándares generales de programación.
- Usa archivos `*.instructions.md` para pautas de tipos de archivos (pruebas unitarias, componentes de Astro, capa de datos) o tareas concretas.
- Usa un lenguaje natural y claro. Incluye ejemplos del código que se debe escribir y del que se debe evitar.

No hay una única manera correcta de crear archivos de instrucciones, como tampoco hay una sola manera de utilizar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que veas instrucciones para muchas tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y el trabajo con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma diferente (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente explica cómo hacerlo.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en el repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas diferentes. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las pautas anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot en el repositorio.
3. Abre la carpeta `.github/instructions` y explora su contenido. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con archivos de pruebas TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así se pueden dividir instrucciones extensas en archivos pequeños y reutilizables, y señalar ejemplos que Copilot debe seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige incluir documentación en el código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de archivo.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; se indica dónde corresponde.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un mensaje con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo mensaje para comparar los resultados.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot reciba únicamente el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. En el selector de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda editar archivos.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios de código, normalmente en el archivo auxiliar y en sus pruebas.
6. Observa que la función auxiliar propuesta recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo hayas pedido. No pasa nada; el objetivo es observar una mejora de la coherencia tras actualizar las instrucciones.

## Añade un nuevo estándar para el repositorio

Como ya se ha indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade un estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero aún no requiere documentación en el código; por eso la función generada no tenía comentarios de documentación.
3. Añade estas líneas de Markdown debajo de los estándares existentes para indicar que Copilot debe añadir comentarios TSDoc y encabezados de archivo:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones se pueden definir para todo el repositorio en `.github/copilot-instructions.md` o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Vuelve a enviar el mensaje y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo mensaje** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios similar a este:

   ```typescript
   /**
    * Funciones auxiliares para acceder a las editoriales de la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc similar a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código real sobre el que se basará el ejercicio siguiente.

## Confirma las instrucciones y sube la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan con el mismo sistema de control de versiones que los demás archivos. Confirma los cambios y sube la rama.

1. Abre otro terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba que la actualización de instrucciones y los cambios de la función auxiliar estén presentes:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de las instrucciones y todos los archivos modificados por Copilot para la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ahora que has subido la rama, crea una solicitud de incorporación de cambios (PR) vinculada a la incidencia del backlog sobre la actualización de las instrucciones. Copilot puede buscar la incidencia, crear la PR para cerrarla y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub; no hay nada que instalar. La primera vez que Copilot use una herramienta, VS Code puede pedirte que inicies sesión. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** esté seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con las instrucciones y cree una PR desde la rama actual que describa las instrucciones actualizadas y la función auxiliar de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se haya creado la PR, pídele que la combine y vuelva a `main` local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones en GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo utiliza Copilot los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar para todo el repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo mensaje y observar cómo el código propuesto aplica el estándar nuevo.
- confirmar la actualización de las instrucciones y la función auxiliar en `main` para que el ejercicio siguiente pueda basarse en ese trabajo.

A continuación, aplicarás las instrucciones en el [modo agente][next-lesson] para que Copilot añada una funcionalidad a varios archivos del proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
