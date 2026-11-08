---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys, pondrás en marcha un [Codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del taller

Para crear una copia del repositorio donde desarrollarás el código, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el taller y la utilizarás durante los ejercicios.

1. En una nueva ventana del navegador, ve a la [plantilla Tailspin Toys][tailspin-template].
2. Crea tu copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de las personas mentoras. De lo contrario, crea el nuevo repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante.

> [!NOTE]
> **El backlog está listo**
>
> Al crear el repositorio a partir de la plantilla, también se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

## Crea un Codespace

A continuación, usarás un Codespace para completar los ejercicios del taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, a continuación, el botón **+** para crear un Codespace.

    ![Crear un Codespace](../../../_images/ex0-create-codespace.png)

La creación del Codespace tardará varios minutos, aunque será mucho más rápida que instalar todos los servicios manualmente. Mientras tanto, puedes explorar otras funcionalidades de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller se ha diseñado para ejecutarse en un Codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen todos los requisitos previos para que puedas empezar sin problemas. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza Codespaces.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Utiliza GitHub Copilot Chat y el modo de agente

Para utilizar el modo de agente de GitHub Copilot Chat, debes tener instalada la extensión GitHub Copilot Chat en el IDE. Debería estar instalada si utilizas GitHub Codespaces.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensiones de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, es posible que tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve a tu Codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** de la parte superior del Codespace.
3. Escribe algo como «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Así se iniciará Copilot Chat.
4. Si todavía no te has autenticado, se te pedirá que inicies sesión en GitHub. Sigue las instrucciones para autenticarte.

    ![Ejemplo de solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, verás la ventana de Copilot Chat.

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del taller y has iniciado la configuración del Codespace que utilizarás para programar.

## Siguiente paso

Empieza a trabajar con Copilot. Continúa con el [ejercicio 1: Instrucciones personalizadas][next-lesson], donde aprenderás a usar las convenciones del proyecto.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
