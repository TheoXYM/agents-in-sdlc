---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga acceso a esa orientación. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas para determinadas rutas.
- actualizarás el archivo de instrucciones para garantizar el cumplimiento de los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene pautas y requisitos para sus prácticas de programación. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, sus parámetros y los valores que devuelven mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben seguir reflejando el código cuando este cambie.
- Se deben conservar las pautas existentes de formato y lint.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para ayudarle a entender mejor tu estilo y tus requisitos de programación. Permiten orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas globales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan pautas para tipos de archivos, lenguajes o tareas concretos.

> [!NOTE]
> Los formatos de instrucciones adicionales y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad con las instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explóralos antes de hacer cambios para observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las pautas de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en el repositorio.
4. Abre la carpeta `.github/instructions` y explora su contenido. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y otras tareas.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define el patrón que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así se pueden dividir instrucciones extensas en archivos pequeños y reutilizables, y señalar ejemplos que Copilot debe seguir al generar código.

## Actualiza los archivos de instrucciones para reflejar las pautas del equipo

Aunque los archivos existentes son un buen punto de partida, falta algo. Añadirás un estándar sobre comentarios TSDoc para el código TypeScript nuevo.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio cuando se inicia una conversación. Después de guardar las instrucciones, sal de la conversación y reanuda la sesión de filtrado para que Copilot cargue las pautas nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las pautas actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc que se han añadido y confirma que describen el código correctamente.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI carga el contexto de los archivos de instrucciones del proyecto y has aplicado un estándar nuevo a la funcionalidad. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas para determinadas rutas.
- actualizaste el archivo de instrucciones para garantizar el cumplimiento de los estándares de programación.
- observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para garantizar que se ejecuten de forma uniforme el lint y las pruebas.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
