---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que se encargue de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube está diseñado para trabajar de forma autónoma y asíncrona, es importante supervisar las tareas.

Hay varias herramientas para gestionar las tareas que asignas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este panel puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas y orientar una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir otra funcionalidad.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se ha dado cuenta de que también convendría añadir un modo claro. Como ya se estaba trabajando en actualizar los estilos del sitio y añadir controles para alternar entre modos, tiene sentido incluirlo en la misma tarea. Orientarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Comprueba el estado actual de las tareas que has asignado al agente en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel de la izquierda. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Por ejemplo:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado de la tarea de Copilot y el enfoque utilizado.

4. Observa también el panel de PR, que aparece a la derecha y permite consultar la solicitud y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que le haya asignado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que se ha completado la configuración y ha empezado a trabajar. Verás mensajes que muestran que ha comenzado la tarea.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo añade al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba ejecutando al recibir el mensaje. La tarea tardará varios minutos; puedes aprovechar ese tiempo para repasar lo que has aprendido hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de supervisión y orientación de las tareas del agente en la nube de GitHub Copilot. Desde allí puedes consultar las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso.

Has explorado estos conceptos:

- utilizaste el panel de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para solicitar otra funcionalidad.

Mientras Copilot termina de trabajar en las funcionalidades de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
