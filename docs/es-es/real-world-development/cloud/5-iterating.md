---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este taller, has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Has pedido a Copilot que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda probarla e implemente funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Vamos a revisar los cambios de código y, si hace falta, daremos indicaciones a Copilot para mejorarlos.

### Escenario

Como ya se ha destacado varias veces, incorporar IA generativa no cambia los fundamentos del diseño de software y DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisaremos las propuestas de GitHub Copilot para documentar el código, añadir juegos relacionados y mejorar la accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad de GitHub Copilot cloud agent

Como Copilot cloud agent trabaja de forma asíncrona y sin supervisión, se han establecido medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama en la que trabaja.
- Cloud agent se ejecuta en GitHub Actions, donde crea un entorno de trabajo temporal e independiente.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- El [acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empezaremos por revisar la primera solicitud de incorporación de cambios (PR) que ha generado GitHub Copilot cloud agent: la que añade documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que al revisar la PR veas una advertencia que indica que el cortafuegos ha bloqueado a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado de forma predeterminada a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos de Copilot cloud agent][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** u otro título más descriptivo.

> [!NOTE]
> Si Copilot aún está trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar un momento o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que ahora incluye comentarios de documentación TSDoc y otros documentos. Los cambios concretos pueden variar.

   Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones personalizadas que has revisado al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot en una solicitud no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR, igual que harías con otra persona del equipo, o añadir comentarios en línea en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte una frase exacta que debas utilizar. Puedes pedirle, por ejemplo, que:

- añada al principio de cada archivo de código un comentario de cabecera que describa brevemente su contenido.
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores o pide otro cambio de documentación que quieras ver en el código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, aparecerá un commit nuevo en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que ha generado Copilot para resolver la incidencia de los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** u otro título más descriptivo.
4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu Codespace y probar manualmente la función de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama que ha creado Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes seguir trabajando con Copilot cloud agent e iterar para pedir cambios. Por ejemplo, puedes solicitar que ajuste el número de juegos relacionados o que añada comentarios de cabecera y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que solicitaste desde mission control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** u otro título más descriptivo.

> [!NOTE]
> Si Copilot aún está trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Revisa la implementación y presta especial atención a:
   - Los componentes de interfaz para alternar entre los modos.
   - El uso del almacenamiento local para conservar las preferencias del usuario.
   - Los cambios de CSS o de estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con el teclado.
   - El código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta estos comandos, sustituyendo `<branch-name>` por el nombre de la rama que ha creado Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan como se espera y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha guiado a Copilot para implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas algún problema o mejora, puedes mencionar a **@copilot** en un comentario y pedirle que haga cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

Cloud agent resulta especialmente útil cuando puedes asignarle tareas reales del backlog y ocuparte de otra cosa. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para que los usuarios muestren interés por un juego en su página de detalles.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el itinerario de cloud agent. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas del repositorio** para reconocer su efecto en los resultados de cloud agent.
- **Has asignado incidencias a Copilot cloud agent** y observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad, con controles de alto contraste y modo claro.
- **Has utilizado la página de Copilot agents como centro de control** para supervisar y orientar durante el trabajo la sesión de accesibilidad.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios de cloud agent**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el itinerario de cloud agent. Si quieres seguir aprendiendo, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el comando opcional `/delegate` para volver a delegar en cloud agent.

En tu propio repositorio, prueba también estas ideas:

- Asigna a cloud agent una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el cortafuegos de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
