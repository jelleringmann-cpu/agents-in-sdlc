---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas adicionales, como ejecutar pruebas. Normalmente, el flujo de trabajo de desarrollo consiste en localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto y volver a modificarlo. Y así sucesivamente.

El modo agente de Copilot puede ayudarte con ese proceso.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios y ejecuta tareas como las pruebas, y corrige automáticamente los problemas que encuentra. Veamos cómo usar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y cloud agent están pensados para experiencias distintas. El modo agente trabaja en el IDE y permite obtener comentarios e interactuar rápidamente. Cloud agent trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo el modo agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina los cambios.
- cómo el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- cómo revisar los cambios y las pruebas generados por el modo agente antes de combinarlos con el código base.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editor. En el ejercicio anterior añadiste un helper de editores y ahora completarás el resto del trabajo de datos, interfaz y pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y ver sus detalles. Es una única aplicación de Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM desde su frontmatter: no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con scripts `<script>` pequeños y específicos de Astro que usan las API estándar del DOM.

### Iniciar el sitio web

Para facilitar el inicio del sitio, se ha preparado la tarea `npm run dev`, que ejecuta el único servidor de desarrollo de Astro. Para iniciarlo en tu GitHub Codespace:

1. Vuelve a tu Codespace. Al terminar el ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar un enlace localhost desde el terminal del Codespace, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, donde se está ejecutando el servidor web.

### Explorar el sitio web

Con el sitio web en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accedes a una página con más información, como el título, la descripción, el editor y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los siguientes ejercicios, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo pueden variar en cada ejercicio; los pasos pertinentes lo indicarán.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generaron automáticamente varias incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no prefieren un modelo sobre otro. Al crear el taller, usamos Claude Sonnet 4.5 y por eso lo mencionamos en las instrucciones. Queremos que las sugerencias de código sean relativamente coherentes y que la experiencia sea buena. Sin embargo, los modelos de lenguaje grandes son probabilísticos y es posible que las sugerencias sean distintas de las que se describen aquí. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Envía este prompt a Copilot para que te muestre el backlog de incidencias:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según cuáles serán más útiles para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según lo útiles que considera que serán para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que usarás para proporcionar contexto. Utilizarás el archivo de [instrucciones de interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio web.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidades. Incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad para probarlos y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar y dividir las instrucciones complejas en partes más manejables e incluir ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, tendrás que realizar como mínimo tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- añadir la interfaz de filtrado a la página de lista de juegos.

Además, las pruebas deben ejecutarse y superarse antes de combinar el cambio con el código base. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editor y categoría según los requisitos de la incidencia correspondiente de GitHub en el backlog. El helper de editores ya existe gracias al ejercicio anterior, así que conserva y ajusta ese trabajo cuando sea necesario mientras completas el resto de la capa de datos, la interfaz y las pruebas. Comprueba que se superan todas las pruebas antes de terminar. El servidor ya está en ejecución; no hace falta que lo inicies.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería encontrar los helpers de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Durante el trabajo, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, verás uno de estos botones para cada cambio; no hace falta que los selecciones mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas o que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir funcionalidades al sitio web de Tailspin Toys. Has aprendido:

- cómo implementar funcionalidades en la interfaz y la capa de datos con el modo agente de GitHub Copilot.
- cómo el modo agente explora el proyecto, identifica los archivos pertinentes y coordina los cambios.
- cómo revisar los cambios y las pruebas generados por el modo agente antes de combinarlos con el código base.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que implemente la paginación en el sitio. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes e incluye los archivos de instrucciones pertinentes.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en el prompt las directrices necesarias para que Copilot implemente lo que necesitas.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Copilot en los modos ask, edit y agent: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
