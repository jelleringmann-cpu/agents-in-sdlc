---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando pones GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición a medida que se producen. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha hecho una petición adicional: quiere un control de modo claro para las personas a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva, ampliarás el trabajo de la sesión actual.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat, con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante esta sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que hayas revisado y estés dispuesto a ejecutar.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías recibir un mensaje final que resuma los cambios.

## Pedir al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un control de modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el método de almacenamiento local.

1. Envía este prompt de seguimiento desde el cuadro de texto de Copilot Chat:

   ```plaintext
   ¡Buen trabajo! Ahora añade un control de modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón, conservar la configuración en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el de alto contraste de forma independiente.
   ```

2. Observa cómo el agente vuelve a examinar los cambios anteriores y propone las modificaciones necesarias para añadir el modo claro.
3. Revisa cada edición nueva como hiciste con el modo de alto contraste. Selecciona **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta refactorizar la interfaz de los controles de una manera que no te convence—, detenlo con el botón **Stop** junto al cuadro de chat y envíale una indicación aclaratoria.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes de Astro, estilos y pruebas relacionadas.
3. Abre algunos de los archivos modificados y examina los diff. Confirma que se han aplicado las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y conservación de las preferencias en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar en tiempo real el trabajo de un agente.
- aceptar o rechazar ediciones individuales de archivos.
- orientar una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás lo que ha generado el agente personalizado local durante el taller.

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
