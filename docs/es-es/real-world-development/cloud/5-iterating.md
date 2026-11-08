---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo del taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, desarrollara una funcionalidad de juegos relacionados y añadiera funcionalidades de accesibilidad con controles para los modos de alto contraste y claro. Ahora revisarás los cambios y, si es necesario, pedirás a Copilot que los mejore.

### Escenario

La incorporación de IA generativa no cambia los fundamentos del diseño de software y DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual. Revisarás las sugerencias de GitHub Copilot para la documentación, los juegos relacionados y las funcionalidades de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama donde trabaja.
- El agente en la nube se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) del agente en la nube de GitHub Copilot, que añade documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que la PR muestre una advertencia de que el firewall bloquea a GitHub Copilot. Es normal: por defecto, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios exactos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo que proceden de las instrucciones personalizadas que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador de flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. La ejecución puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios específicos del código, igual que harías con otra persona del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay una indicación exacta que debas utilizar. Por ejemplo, podrías pedirle que:

- añada encabezados breves al principio de cada archivo de código.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**. Puedes elegir una de las ideas anteriores o pedir otra mejora para el código.
2. Selecciona **View Session** para observar cómo Copilot inicia una sesión nueva y aplica los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR en borrador como lista para revisión y combinarla.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca el indicador de flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. La ejecución puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la funcionalidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir iterando y pedirle más cambios, por ejemplo, modificar la cantidad de juegos mostrados o añadir encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Añade el comentario en la pestaña **Conversation** para iniciar otra sesión.

## Revisa las funcionalidades de accesibilidad

Por último, revisa las funcionalidades de accesibilidad implementadas con el agente personalizado. La PR debe incluir tanto el modo de alto contraste solicitado en el ejercicio 3 como el modo claro que pediste en el ejercicio 4 desde el panel de agentes.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los controles para cambiar entre modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios CSS o de estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - el código JavaScript/TypeScript que gestiona los modos.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca el indicador de flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. La ejecución puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en el Codespace para probar las funcionalidades de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama de Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicia la aplicación y prueba los controles para los modos de alto contraste y claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

El agente personalizado de accesibilidad ayuda a Copilot a implementar estas funcionalidades siguiendo prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que los resuelva.

## Ejercicio opcional: sigue delegando

El agente en la nube es útil para asignarle elementos reales del backlog mientras te ocupas de otras tareas. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para indicar interés en un juego.
- implementar la paginación en la página de juegos.
- añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En las lecciones:

- **Revisaste los archivos de instrucciones personalizadas incluidos en el repositorio** para observar después su efecto en el trabajo del agente.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo preparó el entorno, planificó y trabajó de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** para implementar modos de alto contraste y claro.
- **Utilizaste la página de agentes de Copilot como panel de control** para supervisar y orientar la sesión de accesibilidad.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y pasos siguientes

Has completado el itinerario del agente en la nube. Los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, MCP de Playwright, Agent Merge y una integración opcional de `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
