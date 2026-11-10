---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando GitHub Copilot trabaja en modo de agente, actúa de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat de VS Code muestra las llamadas a herramientas y las ediciones de archivos. Puedes revisar las diferencias de cada cambio, aceptarlo o rechazarlo y orientar la conversación con prompts de seguimiento, sin salir del editor.

En este ejercicio, harás lo siguiente:

- Supervisarás en tiempo real el trabajo del agente de accesibilidad.
- Revisarás y aceptarás los cambios propuestos en los archivos.
- Orientarás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño pide también un control para el modo claro, pensado para quienes consideran demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva, ampliarás el trabajo de la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Las ediciones aparecen en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver sus llamadas a herramientas, lecturas y ediciones de archivos, y los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición, aparecerá con las diferencias en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para deshacerlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, situado en la parte superior del chat, para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Una vez implementado el modo de alto contraste, ampliarás la misma conversación con un control para el modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo previo, incluidos los componentes y los estilos modificados y el patrón de almacenamiento local utilizado.

1. Envía este prompt desde el campo de texto de Copilot Chat:

   ```plaintext
   ¡Buen trabajo! Ahora añade un control del modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: conservar la preferencia en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo de alto contraste y el modo claro de forma independiente.
   ```

2. El agente retomará el trabajo, explorará los cambios realizados y propondrá los necesarios para el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca. Utiliza **Keep** o **Undo** en cada diferencia, como hiciste con el modo de alto contraste.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar los controles de una forma que no te convence—, detenlo con el botón **Stop** junto al campo de texto y envía un prompt para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Envía prompts de seguimiento para pedir ajustes, como «simplifica las hojas de estilo en cascada (CSS) y utiliza variables CSS en lugar de duplicar selectores»; corregir el rumbo, por ejemplo, «no añadas un tercer control; integra el modo claro en el panel de configuración actual»; o ampliar el alcance, como «añade también atajos de teclado para ambos controles».

## Revisa los archivos modificados

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes de Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos de aplicaciones de Internet enriquecidas y accesibles (ARIA), navegación con teclado, estructura semántica en el lenguaje de marcado de hipertexto (HTML) y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara los cambios y crea el commit desde el panel Source Control. Los publicarás en [el siguiente ejercicio][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio has:

- Supervisado en tiempo real el trabajo del agente.
- Aceptado y rechazado ediciones de archivos.
- Orientado la sesión con prompts de seguimiento.

En el [siguiente ejercicio][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que haya generado el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
