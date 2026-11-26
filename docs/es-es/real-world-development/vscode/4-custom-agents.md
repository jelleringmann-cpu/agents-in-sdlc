---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas de tu flujo de desarrollo. Al definirlos en archivos Markdown dentro de `.github/agents`, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio para ayudarle a realizar ciertos tipos de trabajo de manera más eficaz. Los equipos pueden plasmar su experiencia en agentes reutilizables: un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo contiene frontmatter YAML con, como mínimo, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la especialización y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Hay cierto solapamiento entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar ciertas tareas. La forma más sencilla de distinguirlos es esta: **el agente personalizado es quien trabaja y las habilidades son las herramientas**.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes como parte de su trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Para ello, podría recurrir a habilidades como una que gestione el flujo de trabajo de las solicitudes de incorporación de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar lo que mejor funcione en tus entornos y situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de su capacidad visual o sus preferencias. Los comentarios recientes de los usuarios indican que algunas personas tienen dificultades para leer el tema oscuro porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es fundamental, así que quieres implementar esta funcionalidad cuanto antes. Para ello, utilizarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para las tareas de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y abre un terminal. Crea una rama nueva a partir de `main` para el trabajo de accesibilidad, de modo que la PR de filtrado del ejercicio 3 permanezca separada:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - una lista de problemas y errores habituales.

## Usar el agente personalizado en Copilot Chat

La lista desplegable de agentes situada en la parte inferior de la vista Copilot Chat de VS Code muestra todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para que la sesión de chat use sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los siguientes ejercicios, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo pueden variar en cada ejercicio; los pasos pertinentes lo indicarán.

1. Si aún no está seleccionado, elige **Agent** en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes en la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

   ```plaintext
   Añade al sitio un modo de alto contraste. Debe haber un control que permita al usuario activarlo o desactivarlo y la configuración debe conservarse al volver a cargar las páginas, utilizando el almacenamiento local del navegador.
   ```

5. Copilot Chat explorará el código base, propondrá cambios y los aplicará a los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que puedes revisar.
6. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** para mostrar el conjunto de archivos que el agente ha modificado.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot realizará cambios reales en el repositorio y editará archivos existentes, como componentes de Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás el trabajo en curso y orientarás al agente.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en determinadas tareas y áreas. Con ellos, puedes plasmar la experiencia y los estándares del equipo en agentes reutilizables para que Copilot trabaje de forma más eficaz.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

Después, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios a medida que se aplican y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
