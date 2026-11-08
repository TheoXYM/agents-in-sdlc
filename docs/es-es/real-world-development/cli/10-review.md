---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo de trabajo completo en Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado.
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en modo Plan, lo desarrollaste en modo Autopilot y lo revisaste en modo Interactive.
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la utilizaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- revisaste el cambio completo de filtrado y autorizaste Agent Merge para esa PR.
- exploraste comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** mostrar el `starRating` existente y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Flujo de filtrado y calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la creación de su PR, has utilizado la misma conversación y rama. Hemos agrupado el trabajo en una sola PR para agilizar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, revisando tú mismo la aplicación en el navegador y dejando que Copilot la explorara con MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y comprobaciones de tipos, y generó un informe en el nuevo formato. El agente de QA agrupó esos resultados con la revisión de los requisitos y de la cobertura de pruebas antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si el agente de QA no necesita añadir ninguna, también puede ser el resultado correcto. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas de verificación afectadas tras realizar cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares; las habilidades describen tareas repetibles; los agentes personalizados definen roles especializados; y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para estudiar un enfoque antes de implementarlo, **Interactive** para mantener el control en los cambios específicos y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para los cambios rutinarios y otro más capaz para el trabajo complejo.

El contexto importa tanto como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye de forma importante en el resultado.

## Otras cosas que puedes explorar

Ya has recorrido el flujo principal. También puedes probar algunas funcionalidades de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar una tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que tienes en mente desde hace años. Comparte lo aprendido con el equipo y aprovecha lo que te enseñen las demás personas. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
