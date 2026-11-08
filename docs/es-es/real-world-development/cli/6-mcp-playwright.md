---
title: "Lección 6 - Validar la funcionalidad con MCP de Playwright"
description: "Inspecciona o configura MCP de Playwright en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos comentado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot utilice otras automatizaciones. Para eso sirven los servidores MCP: permiten que Copilot vaya más allá de las funcionalidades integradas en la CLI y acceda a herramientas y servicios adicionales.

En esta lección:

- aprenderás qué es el Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor MCP de Playwright si aún no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz también hay que interactuar con ella. Quieres que Copilot pueda utilizar el sitio web como lo haría una persona y automatizar la comprobación de los cambios para tener más confianza en que funcionan según lo previsto.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Gracias a MCP, pueden acceder a información actualizada y realizar acciones en tu nombre.

El acceso a estas herramientas y recursos se realiza a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos disponibles para el agente.

Algunos servidores MCP populares son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece funcionalidades de automatización del navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y fomentar las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizarlos, revisa su código fuente, comprueba quién los publica y valora las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si ya has configurado MCP de Playwright en Copilot CLI, omite estos pasos y continúa en la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP con este comando:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y, a continuación, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes comprobaste manualmente el funcionamiento. Ahora, pide a Copilot que haga lo mismo con el servidor MCP de Playwright.

1. Usa este mensaje para pedir a Copilot que valide la funcionalidad con el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente no hace falta pedir explícitamente a Copilot que utilice el servidor MCP, porque suele detectarlo por sí solo. Sin embargo, indicárselo puede ayudarle a elegir la herramienta adecuada, mejorar la coherencia de los resultados y ahorrar tokens.

2. Observa los pasos que sigue Copilot en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio web, detendrá el servidor y te entregará un informe.

## Resumen y pasos siguientes

¡Enhorabuena! Has utilizado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- aprendiste qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadiste el servidor MCP de Playwright si aún no estaba disponible.
- pediste al agente que controlara un navegador y explorara la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combina la habilidad y las herramientas del navegador en un perfil especializado.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
