---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has pedido a Copilot cloud agent que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque cloud agent trabaja de forma autónoma y asíncrona, es importante supervisar estas tareas.

Hay varias herramientas para gestionar el trabajo asignado a cloud agent, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones realizadas y orientar las sesiones que aún están en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas de cloud agent.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se ha dado cuenta de que también sería un buen momento para añadir un modo claro. Como ya se están actualizando los estilos del sitio y añadiendo controles de modo, tiene sentido incluirlo. Orientarás el trabajo del agente para que añada el modo claro además del alto contraste.

## Revisar las tareas de Copilot cloud agent

Consulta el estado de todas las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de Copilot agents](https://github.com/copilot/agents).
2. Revisa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno para Copilot.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos concretos que aparezcan dependerán del estado de la tarea de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR), que aparece a la derecha y permite consultar la PR y los archivos modificados.

## Dirigir el trabajo de cloud agent

Ahora que has visto las tareas activas, pide a Copilot que añada el control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de prepararse y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Ya que estamos trabajando en un modo de alto contraste, añadamos también un modo claro. Debe incluir un control para que los usuarios puedan seleccionar el modo de visualización que prefieran.
   ```

   ![Tarea de cloud agent en la página de agentes con el cuadro Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando cuando enviaste el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar todo lo que has aprendido y explorado.

## Resumen y siguientes pasos

En esta lección has explorado la página de Copilot agents, el centro desde el que se supervisan y guían las tareas de GitHub Copilot cloud agent. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o dar instrucciones adicionales.

Has explorado estos conceptos:

- has utilizado mission control y la página de agentes para supervisar las tareas de cloud agent.
- has redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de implementar las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de Copilot agents][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
