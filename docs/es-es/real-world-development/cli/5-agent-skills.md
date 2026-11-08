---
title: "Lección 5 - Personalizar y utilizar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica mucho más que limitarse a escribirlo. Has comprobado manualmente que funciona y has utilizado archivos de instrucciones para garantizar que cumple los estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de las tareas de integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo realizar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- ejecutarás la habilidad y revisarás su salida.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos. El equipo quiere mejorar el informe para facilitar la lectura de los resultados.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El archivo contiene metadatos con un nombre y una descripción que definen la habilidad, una introducción sobre su propósito e indicaciones sobre cuándo debe utilizarse. La carpeta también puede incluir scripts y otros recursos que la habilidad necesite.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. En este ejemplo, `quality-checks` ejecuta comandos `npm` para iniciar las pruebas y el lint, por lo que no necesita archivos auxiliares.

Las habilidades pueden residir en la carpeta `.github/skills` del proyecto para que todo el equipo pueda compartirlas y reutilizarlas, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

1. Si aún no tienes abierto un lienzo **Files**, selecciona **+** en el panel de revisión y, después, **File**.
2. Busca `.github/skills/quality-checks/SKILL.md`.
3. Lee `name` y `description` al principio. Observa que la descripción ayuda a Copilot a decidir cuándo debe utilizar la habilidad.
4. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.

## Ejecuta la habilidad antes de realizar un cambio

Puedes invocar las habilidades directamente con un comando de barra o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Selecciona el modo **Interactive** en el menú desplegable para confirmar que Copilot lo utiliza.
2. Utiliza este mensaje para invocar la habilidad:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personaliza el informe

Mejora el informe para que indique qué se ha ejecutado, si ha tenido éxito y qué han comunicado realmente las herramientas. Actualiza la habilidad para generar ese informe.

1. Vuelve al lienzo **Files**.
2. Si aún no está abierto, abre `.github/skills/quality-checks/SKILL.md`.
3. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

El archivo se guardará automáticamente.

## Ejecuta la habilidad actualizada

Ahora que has actualizado la habilidad, comprueba el resultado. Utilizarás exactamente el mismo mensaje que antes.

1. Selecciona el modo **Interactive** en el menú desplegable para confirmar que Copilot lo utiliza.
2. Utiliza el mismo mensaje para invocar la habilidad:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Resumen y pasos siguientes

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- personalizaste el formato de sus resultados.
- ejecutaste la habilidad y revisaste su salida.

El cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
