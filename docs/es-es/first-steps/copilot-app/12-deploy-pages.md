---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y verificar el Space Quiz en directo."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz con GitHub Pages para que todo el mundo pueda acceder a él. Copilot puede preparar el flujo de trabajo y guiarte en el proceso, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás el proceso de publicación y verificarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio, que consta de un único archivo, con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia de entorno de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este mensaje:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de GitHub Actions llamada Deploy website, con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz en directo en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este mensaje pide al agente que cree y combine una solicitud de incorporación de cambios y modifique la configuración de Pages del repositorio. Revisa las diferencias, los permisos de publicación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y verifica la publicación

1. Confirma que el README solo se ha añadido si el repositorio aún no tenía uno.
2. Revisa los cambios de la solicitud de incorporación, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Combina la solicitud solo cuando se hayan superado las comprobaciones obligatorias.
4. Sigue la ejecución de publicación de GitHub Actions hasta que se complete correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages o combinar la solicitud de incorporación de cambios, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que se complete la publicación y el sitio esté disponible.

## Resumen y pasos siguientes

Space Quiz ya está disponible como sitio web y has verificado la publicación en lugar de confiar únicamente en el informe del agente. Continúa con la [lección 13: Revisión y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/
