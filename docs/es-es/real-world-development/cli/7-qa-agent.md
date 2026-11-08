---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combina los requisitos de una incidencia, la habilidad quality-checks y MCP de Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y MCP de Playwright para observar la experiencia de filtrado en un navegador. Ahora reunirás estas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo se combinan en un agente personalizado las instrucciones, las habilidades y las herramientas MCP.
- crearás y revisarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de QA y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar el proceso de QA y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones diferentes. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. MCP de Playwright proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos e informar de los resultados. No reemplaza las otras personalizaciones ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar el agente.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el agente, revisa su perfil para confirmar que Copilot ha incluido el flujo de QA y los límites previstos. Así evitarás que un agente incompleto o demasiado permisivo modifique la funcionalidad cuando solo querías verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos. `description` es obligatorio; `name` es opcional, aunque incluirlo permite mostrar un nombre identificable para el agente.
3. Lee las instrucciones del perfil y confirma que el agente de QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza MCP de Playwright.
4. Confirma que el agente presenta pruebas que respaldan sus conclusiones, pide permiso antes de modificar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si faltan responsabilidades o límites, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta el agente de QA sobre la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la conversación de filtrado y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que no aparezca en la lista de agentes disponibles. El comando anterior lo activará de todas formas.

2. Pide al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Deja que el agente de QA trabaje.
4. Cuando termine, lee su informe.

## Resumen y pasos siguientes

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado sus resultados. En esta lección:

- exploraste cómo se combinan en un agente personalizado las instrucciones, las habilidades y las herramientas MCP.
- creaste y revisaste un perfil reutilizable de control de calidad.
- seleccionaste el agente de QA y revisaste sus conclusiones respecto a la incidencia de filtrado.

Ya tienes la implementación, la habilidad actualizada, el perfil de QA, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
