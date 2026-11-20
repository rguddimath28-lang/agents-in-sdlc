---
title: "Lección 12: Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y verificar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dale al Space Quiz un hogar público y publícalo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar la publicación, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una implementación con GitHub Pages;
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarlos;
- seguirás el proceso de implementación y verificarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero la gente necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él mediante una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pedir a Copilot que prepare la implementación

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y enviar código. Revisa la configuración de aprobaciones y el repositorio de destino antes de enviar este prompt. Si está seleccionado **Approve all**, no esperes otro aviso de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisar el flujo de trabajo y la solicitud de incorporación de cambios

1. Confirma que el archivo README solo se ha añadido si el repositorio todavía no tenía uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación e implementación del flujo de trabajo, sus desencadenadores y sus permisos.
3. Confirma que el flujo de trabajo implementa desde la rama predeterminada, no desde una solicitud de incorporación de cambios que todavía no se ha combinado.
4. Pide las correcciones necesarias y revisa las diferencias actualizadas. Espera a que se superen las comprobaciones obligatorias de la solicitud de incorporación de cambios antes de autorizar su combinación.

## Autorizar la combinación y la implementación

> [!WARNING]
> El siguiente prompt autoriza al agente a combinar la solicitud de incorporación de cambios revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de completar la revisión y confirmar el repositorio y la solicitud de incorporación de cambios. Según la configuración de aprobaciones, estas acciones podrían realizarse sin otro aviso.

Sustituye `<PR_NUMBER>` por el número de la solicitud de incorporación de cambios revisada y envía este prompt por separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verificar la implementación

1. Sigue la ejecución de implementación de GitHub Actions hasta que se complete correctamente.
2. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages ni combinar la solicitud de incorporación de cambios, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que la implementación se complete y el sitio publicado se cargue.

## Resumen y próximos pasos

Tu Space Quiz ya está disponible como sitio web y has verificado la implementación, en lugar de confiar solo en el informe del agente. Continúa con el [repaso y los próximos pasos de la lección 13][next-lesson].

[next-lesson]: ../13-review/
