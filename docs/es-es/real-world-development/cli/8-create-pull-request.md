---
title: "Lección 8: Crear y combinar la solicitud de incorporación de cambios de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas; después crea una solicitud de incorporación de cambios y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están en una sola rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios (PR). Tú combinaste la solicitud de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente QA en varias solicitudes de incorporación de cambios. Para simplificar el taller, todo el trabajo de filtrado y calidad se ha mantenido en una conversación y una rama, y se incluirá en esta solicitud.

En esta lección, harás lo siguiente:

- Aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- Crearás la solicitud de incorporación de cambios del filtrado.
- Activarás Agent Merge solo después de revisar los cambios y confirmarás que la solicitud se ha combinado.

## Escenario

Durante el trabajo de filtrado has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar las tareas restantes de la solicitud, sin quitar al desarrollador el control sobre la autorización de la combinación.

## Presentación de Agent Merge

**Agent Merge** automatiza las tareas pendientes para integrar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la solicitud —por ejemplo, corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y ejecuta un rebase cuando es necesario— y, después, activa la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la solicitud

Ya has creado todo el código. Ahora revisarás los cambios, crearás la solicitud y dejarás que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Introduce `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR. Cuando se superen todas las comprobaciones, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando la configuración del repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora la gestión de la solicitud de incorporación de cambios. En esta lección, has:

- Aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- Creado la solicitud de incorporación de cambios del filtrado.
- Activado Agent Merge solo después de revisar los cambios y confirmado que la solicitud se ha combinado.

A continuación, [explorarás otros comandos de barra de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
