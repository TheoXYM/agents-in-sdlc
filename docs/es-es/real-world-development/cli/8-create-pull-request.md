---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste manualmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la PR del filtrado.
- habilitarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo del filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar las tareas restantes de la PR sin quitar al desarrollador el control de autorización de la combinación.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y reorganiza la rama cuando hace falta. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede hacerse cargo de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar la PR

Con el código terminado, revisa los cambios, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agente escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y validación y, ahora, gestión de la PR. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionaste todos los cambios de la funcionalidad y las pruebas de verificación.
- creaste la PR del filtrado.
- habilitaste Agent Merge solo después de revisar el trabajo y confirmaste que la PR se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a un agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
