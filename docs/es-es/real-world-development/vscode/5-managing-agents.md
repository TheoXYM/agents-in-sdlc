---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat de VS Code muestra cada llamada a herramientas y cada edición de archivos. Puedes revisar las diferencias en el chat, aceptar o rechazar cambios concretos y orientar la conversación con mensajes de seguimiento para corregir o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- pedirás al agente que añada un control para el modo claro, además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita otra mejora: un control para el modo claro, dirigido a las personas que encuentran demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva, ampliarás la tarea desde la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra todos los pasos del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, las diferencias aparecerán en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al final, mostrará un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora que está implementado el modo de alto contraste, ampliarás la conversación para añadir un control para el modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el uso de almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, revisará los cambios y propondrá las mejoras para el modo claro.
3. Revisa cada cambio nuevo, igual que con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si reorganiza los controles de una manera que no te convence—, deténlo con el botón **Stop** junto al cuadro de mensaje y envía una aclaración.

> [!TIP]
> Es más rápido orientar una sesión en curso que empezar de nuevo. Usa mensajes de seguimiento para pedir ajustes (por ejemplo, «simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los cambios que ha realizado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes de Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones de archivos individuales.
- orientaste la sesión con mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
