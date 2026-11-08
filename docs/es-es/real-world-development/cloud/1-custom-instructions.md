---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones aplicables a rutas concretas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás una instrucción antes y después. El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás cómo influyen las instrucciones más adelante, cuando revises las solicitudes de incorporación de cambios que genere el agente.

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
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivos concretos. Permiten proporcionar pautas para lenguajes específicos (como TypeScript o Astro) o tareas (como crear un componente de interfaz de usuario o pruebas unitarias).

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
> Todos los proyectos que utilizan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que veas instrucciones para muchas tareas, como las [actualizaciones de la interfaz][ui-instructions] y el trabajo con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma diferente (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente explica cómo hacerlo.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye el repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas instrucciones se aplican a cualquier interacción con Copilot en el repositorio.
3. Abre la carpeta `.github/instructions` y explora su contenido. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón que determina a qué archivos se aplican las instrucciones. En este caso, coincide con archivos de pruebas TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así se pueden dividir instrucciones extensas en archivos pequeños y reutilizables, y señalar ejemplos que Copilot debe seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige incluir documentación en el código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de archivo.

## Añade un nuevo estándar para el repositorio

Este es el único cambio que realizarás en el ejercicio: añadir una regla global para documentar el código con comentarios TSDoc y un encabezado de archivo. El agente en la nube tendrá en cuenta esta regla al trabajar en las incidencias que le asignes más adelante.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama de destino de la incidencia, pero tú confirmarás los cambios de forma ordenada:

1. Desde el terminal del Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como ya se ha indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade un estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero aún no requiere documentación en el código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade estas líneas de Markdown debajo de los estándares existentes para indicar que Copilot debe añadir comentarios TSDoc y encabezados de archivo:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones se pueden definir para todo el repositorio en `.github/copilot-instructions.md` o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, sube y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama de destino de la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot, que partirá de `main`. Por eso, los cambios en las instrucciones deben llegar a `main`.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así el agente en la nube podrá leer las instrucciones al trabajar en incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. La rama permite practicar un flujo de trabajo habitual en un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo utiliza Copilot los archivos de instrucciones del proyecto y has añadido un estándar para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
