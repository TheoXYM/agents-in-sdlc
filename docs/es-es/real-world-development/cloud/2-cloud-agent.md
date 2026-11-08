---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: problemas de seguridad sin resolver, código heredado que hay que actualizar o funcionalidades que llevan mucho tiempo pendientes porque no había tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones más importantes.

Con el agente en la nube explorarás:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Las personas contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. El primer paso será añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere mostrar juegos relacionados (otros títulos de la misma categoría) en la página de detalles de cada juego para que la gente pueda seguir explorando. Por ahora no necesita una implementación refinada; quiere algo que sirva para probar la experiencia de usuario. Aunque esta tarea es un impedimento, hay otras incidencias más prioritarias.

Son dos ejemplos de tareas que pueden quedar relegadas y que se pueden asignar al agente en la nube de Copilot. Este trabajará en segundo plano, lo que te permitirá ocuparte de otras cosas y volver a revisar su trabajo cuando esté listo.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a una persona desarrolladora. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después, Copilot creará una PR en borrador para registrar el progreso, preparará un entorno y empezará a trabajar. Puedes consultar la sesión de Copilot mientras trabaja o después. Cuando la solución esté lista para revisión, te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas si se examinan de cerca. Ten en cuenta las [prácticas recomendadas para definir el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como un agente de programación en pareja: trabaja por etapas, aprende, experimenta y adapta el enfoque. Incorporar IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has aprendido cómo las instrucciones personalizadas guían a Copilot. Si ya has completado el ejercicio sobre instrucciones personalizadas, habrás visto cómo se utiliza `.github/copilot-instructions.md`; si no, este es un buen momento para revisar rápidamente el archivo. Antes de asignar tareas al agente en la nube, examina sin modificar los archivos de instrucciones que incluye el repositorio para que después puedas observar su efecto.

Abre estos archivos en la interfaz web de GitHub del repositorio o en un Codespace que ya esté en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los requisitos de comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que limitan la aplicación de estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de archivo según estos archivos de instrucciones. Volverás a revisarlo al examinar las PR en una lección posterior.

## Configura el entorno de desarrollo del agente en la nube

El desarrollo de código suele requerir un entorno específico y la ejecución de scripts para instalar herramientas y dependencias. También sucede cuando asignas tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que Copilot empiece. Así puedes instalar las herramientas y dependencias necesarias. El taller ya incluye un flujo de configuración que proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña «Actions» del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si estos pasos clonan el repositorio (por ejemplo, para instalar dependencias), necesitan el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente al terminar los pasos de configuración.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL).
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

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene dos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica puede ralentizar el trabajo y dificultar el mantenimiento del código o la incorporación de nuevas personas al equipo. Copilot puede ayudar a crear documentación, por lo que este es un buen ejemplo de tarea para el agente en la nube. En una lección posterior revisarás los cambios que genere.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios que acaba de generar Copilot. Su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está creando su entorno (ejecutando el flujo de trabajo mencionado), analizando el proyecto y determinando cómo resolver la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR hasta ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, analiza su estado, elabora un plan y crea código.

La tarea puede tardar varios minutos. Una de las ventajas del agente en la nube de Copilot es que trabaja de forma asíncrona, de modo que puedes ocuparte de otras cosas. Aprovecharás esa capacidad para asignarle otra tarea y, mientras tanto, añadirás funcionalidades a la aplicación.

## Añade una sección de juegos relacionados

Una de las ventajas del agente en la nube es la posibilidad de distribuir el trabajo: puedes ocuparte de una tarea mientras él trabaja en otra. Añadir una sección de juegos relacionados puede llevar poco tiempo, pero ese tiempo se puede dedicar a otras tareas. Asigna esta tarea al agente en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot**.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás un emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de manera parecida a una persona ingeniera de software; no hace falta que lo supervises continuamente, sino que puedes revisar su trabajo cuando termine. Ahora aprenderás a crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y pedirle que resuelva deuda técnica, añada funcionalidades o ayude a migrar código a otro framework.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

Mientras trabaja el agente en la nube, puedes aprender a crear y utilizar agentes personalizados. El agente en la nube de Copilot también puede [utilizar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

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
