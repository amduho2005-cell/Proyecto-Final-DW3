# PRD · Bodega COCOA USFQ: sistema de reservas de equipos

- **Versión:** borrador 1 (generado por IA y pendiente de revisión del equipo)
- **Fecha:** 23-11-2026
- **Fuente única:** `docs/fuentes/entrevista.md`, con las respuestas del cliente del 11-11-2026.

**Cómo citar:** `R1`…`R22` son las respuestas numeradas de la entrevista. `R·10:42` («NADA AUTOMATIZADO») y `R·10:45` (PDF) son las dos anotaciones sin número, identificadas por su hora. `S#` es un supuesto de la sección 7. Lo que no cita ninguna de estas referencias es un hueco.

> En las fuentes no hay dos documentos que se contradigan, porque solo hay uno. Las contradicciones están dentro de esa misma entrevista y se resuelven en la sección 7.

---

## 1. Problema y objetivo

**Problema.** La Bodega COCOA USFQ presta equipos (cámaras, computadoras y otros) a estudiantes y profesores (R3, R7, R22). Hace falta un sistema en el que cada usuario pida ítems concretos por fechas y horas y en el que un administrador revise cada solicitud a mano (R2, R5, R17). *La entrevista no describe cómo funciona hoy el proceso ni qué problema concreto causa (S1).*

**Objetivo.** Tener una aplicación web responsive llamada «Bodega COCOA USFQ» (R20, R22) con estas funciones:
- Los usuarios reservan uno o varios ítems específicos sin límite de duración (R1, R2, R3).
- Los administradores aprueban, editan y controlan cada reserva manualmente (R5, R16).
- El sistema avisa por correo de las solicitudes, las confirmaciones y las devoluciones (R4, R8).

## 2. Usuarios

| Perfil | Fuente | Lo que necesita hacer |
|---|---|---|
| **Usuario general**: estudiante o profesor | R3 | Ver los equipos disponibles y los que están en mantenimiento (R6). Elegir un ítem concreto por su ID (R2, R9). Juntar varios ítems en una sola reserva (R1). Elegir fechas y horas fácilmente (R17). Escribir un comentario corto sobre el uso (R3). Recibir correos de envío, confirmación y devolución (R4, R8). Cancelar con al menos 1 h de anticipación (R14). Obtener el PDF de su reserva (R·10:45). Usar la aplicación bien desde el celular (R19). |
| **Administrador** | R3 | Recibir y revisar a mano cada solicitud (R5, R8). Confirmar reservas (R8). Editar la reserva de cualquier usuario (R16). Bloquear usuarios con una razón (R11). Ver los ítems dañados o extraviados que los usuarios no ven (R12). Ver en el panel principal un resumen del total de reservas (R18). Trabajar sobre todo desde computadora (R19). |

*No se sabe si los profesores tienen reglas distintas a las de los estudiantes (S13).*

## 3. Alcance del MVP

1. **Catálogo por ítem:** cada equipo aparece como un ítem individual con su propio ID, aunque sea del mismo modelo que otro, por ejemplo «Cámara modelo 1» y «Cámara modelo 2» (R9).
2. **Elegir un ítem específico:** el usuario elige exactamente qué ítem quiere, aunque haya varios iguales (R2).
3. **Reserva conjunta (carrito):** el usuario junta varios ítems en una misma reserva (R1, R9).
4. **Vencimiento del carrito:** si la reserva no se envía en 1 h, el carrito se vacía (R15).
5. **Selector de fecha y hora:** una forma fácil de elegir fechas y horas, en formato de 24 h (R17, R21).
6. **Comentario de uso:** la reserva lleva un comentario corto obligatorio sobre el uso, sin pedir justificación formal (R3).
7. **Aprobación manual:** cada solicitud queda pendiente hasta que un administrador la revisa y la confirma (R5, R·10:42).
8. **Correos de solicitud y confirmación:** al admin le llega un correo cuando entra una solicitud y otro de confirmación. Al usuario le llega uno cuando envía la reserva y otro cuando el admin la aprueba (R8).
9. **Recordatorios de devolución:** se manda un correo 10 min antes de la hora de devolución y otro 30 min después si el equipo no se devolvió (R4).
10. **Cancelar:** el usuario cancela su reserva con al menos 1 h de anticipación (R14, S8).
11. **Edición por el admin:** el administrador edita la reserva de cualquier usuario (R16).
12. **Estado de mantenimiento:** un ítem en mantenimiento sigue en el catálogo, con una nota visible (R6).
13. **Ítems dañados o extraviados:** desaparecen del catálogo de los usuarios sin ninguna indicación y solo el admin los ve (R12).
14. **Restricción del día de devolución:** algunos tipos de ítem no se pueden devolver en ciertos días. Por ejemplo, una cámara no se devuelve el sábado y una computadora sí (R7).
15. **Bloqueo de usuarios:** el admin bloquea a un usuario con una razón, y el bloqueo termina solo cuando pasa el tiempo (R11).
16. **PDF de la reserva:** cada reserva genera un PDF con su información (R·10:45).
17. **Panel del administrador:** en la pantalla principal del admin hay un resumen del total de reservas (R18).
18. **Responsive:** la parte de usuarios funciona bien en el celular, y la del admin también es responsive aunque está pensada sobre todo para computadora (R19, R20, S3).
19. **Identidad:** el nombre de la página incluye «Bodega COCOA USFQ» (R22).

## 4. Reglas de negocio

| # | Regla (comprobable) | Fuente |
|---|---|---|
| RN1 | Cada ítem tiene un ID único. Dos ítems del mismo modelo son registros distintos. | R9 |
| RN2 | Una reserva contiene 1 o más ítems. | R1 |
| RN3 | La reserva siempre apunta a un ítem por su ID, nunca a «cualquiera de este modelo». | R2 |
| RN4 | No hay duración máxima para una reserva. Se aceptan reservas que duren todo el semestre. | R3 |
| RN5 | El comentario de uso es obligatorio y no se pide ninguna justificación adicional. | R3 |
| RN6 | No hay una anticipación mínima ni un horario restringido para pedir un equipo. | R7 |
| RN7 | Ninguna reserva pasa a «confirmada» sin la acción de un administrador. | R5, R·10:42 |
| RN8 | Un carrito que no se envía en 60 min se vacía. | R15, S9 |
| RN9 | El usuario puede cancelar solo si faltan 60 min o más para el inicio de la reserva. | R14, S8 |
| RN10 | Una reserva no se puede renovar. | R10 |
| RN11 | Se manda un correo de recordatorio a T−10 min y otro a T+30 min, si no hubo devolución (T es la hora de devolución acordada). | R4 |
| RN12 | El atraso máximo que se tolera en la devolución es de 30 min. | R13, S4 |
| RN13 | Cada tipo de ítem tiene días en los que se puede devolver. Una cámara no se puede devolver el sábado y una computadora sí. | R7, S6 |
| RN14 | Un ítem en mantenimiento se muestra en el catálogo con una nota visible de mantenimiento. | R6, S10 |
| RN15 | Un ítem dañado o extraviado no aparece para los usuarios y sí aparece para los administradores. | R12 |
| RN16 | Para bloquear a un usuario, el admin tiene que escribir una razón. El desbloqueo ocurre solo cuando se cumple el plazo. | R11, S7 |
| RN17 | El administrador puede editar cualquier reserva, sea de quien sea. | R16 |
| RN18 | Todas las horas se muestran y se piden en formato de 24 h. | R21 |
| RN19 | Cada reserva tiene un PDF descargable con su información. | R·10:45, S14 |
| RN20 | El título de la página contiene «Bodega COCOA USFQ». | R22 |
| RN21 | Hay solo dos roles: usuario general (estudiante o profesor) y administrador. | R3 |

## 5. Fuera de alcance

| Qué queda fuera | Motivo | Fuente |
|---|---|---|
| Renovar o extender una reserva | El cliente dijo que no hay forma de renovar. | R10 |
| Aprobar reservas automáticamente | Cada reserva se revisa a mano. «NADA AUTOMATIZADO». | R5, R·10:42 |
| Límite de duración de las reservas | El cliente dijo que no hay límite. | R3 |
| Pedir una justificación formal del préstamo | Basta con un comentario corto. | R3 |
| Mostrar a los usuarios que un ítem está dañado o extraviado | Solo el admin lo puede ver. | R12 |
| Que el admin tenga que quitar el bloqueo a mano | El desbloqueo es automático por tiempo. | R11 |

## 6. Decisiones del cliente

Todas son del **11-11-2026**. La hora es la de la anotación.

| Hora | Decisión | Ref. |
|---|---|---|
| 10:11 | Se permite una reserva conjunta de varios equipos. | R1 |
| 10:13 | El usuario elige el ítem específico. | R2 |
| 10:14 | No hay límite de tiempo. Se pide un comentario corto. Los roles son usuario general y admin. | R3 |
| 10:17 | Hay avisos por correo 10 min antes y 30 min después de la devolución. | R4 |
| — | El admin revisa manualmente cada reserva. | R5 |
| 10:19 | Los ítems en mantenimiento se ven, con una nota. | R6 |
| 10:21 | No hay horario para pedir. Hay restricciones de día para devolver según el tipo. | R7 |
| 10:23 | Se definen los correos para el admin y para el usuario. | R8 |
| 10:25 | Se reserva por ítem con ID único. | R9 |
| 10:27 | No hay renovación. | R10 |
| 10:28 | Hay bloqueo con razón y desbloqueo automático por tiempo. | R11 |
| 10:30 | Los ítems dañados o extraviados solo los ven los admins. | R12 |
| 10:31 | Se tolera un atraso de 15 a 30 min como máximo. | R13 |
| 10:38 | Se cancela con 1 h como mínimo. | R14 |
| 10:40 | El carrito dura 1 h. | R15 |
| 10:41 | El admin edita cualquier reserva. | R16 |
| 10:42 | «NADA AUTOMATIZADO». | R·10:42 |
| 10:45 | Cada reserva genera un PDF. | R·10:45 |
| 10:47 | Se pide un selector de fecha y hora fácil. | R17 |
| 11:00 | El panel del admin muestra un resumen del total de reservas. | R18 |
| 11:05 | Usuarios responsive; admin, mejor en computadora. | R19 |
| 11:06 | El responsive es prioridad en las dos partes. | R20 |
| 11:09 | Se usa el formato de 24 h. | R21 |
| 11:12 | El nombre es «Bodega COCOA USFQ». | R22 |

## 7. Supuestos y preguntas abiertas

Cada pregunta tiene un **supuesto provisional** que se usa hasta que el cliente responda.

| # | Pregunta abierta | Por qué está abierta | Supuesto provisional |
|---|---|---|---|
| S1 | ¿Cómo se gestionan hoy los préstamos y qué problema concreto causa eso? | La entrevista no lo describe. | Hoy no hay un sistema digital. El objetivo es pasar a digital la solicitud y la aprobación. |
| S2 | «NADA AUTOMATIZADO» (R5, R·10:42) frente a los correos automáticos (R4, R8), el desbloqueo por tiempo (R11) y el vencimiento del carrito (R15). ¿Qué cubre exactamente? | La entrevista se contradice. | La frase se refiere a las decisiones sobre reservas (aprobar, rechazar, cambiar estados). Los correos, el vencimiento del carrito y el desbloqueo sí son automáticos, porque el cliente los pidió expresamente. |
| S3 | R19 dice «admin mejor en compu» y R20 dice «prioridad de responsive en ambos». | La entrevista se contradice. | Las dos partes son responsive. El diseño del admin se optimiza primero para computadora y el de usuarios primero para celular. |
| S4 | ¿La tolerancia de atraso es de 15 o de 30 min (R13)? | R13 da un rango. | 30 min, para que coincida con el aviso de T+30 de R4. |
| S5 | ¿Qué pasa cuando se supera la tolerancia de atraso? | No se dice. | No pasa nada automático. El admin decide a mano si bloquea al usuario (R11). |
| S6 | ¿Qué tipos de ítem se pueden devolver en qué días y en qué horario? | R7 solo da un ejemplo (cámara frente a computadora). | El admin configura, para cada categoría de ítem, los días de la semana en que se acepta la devolución. Por defecto se aceptan todos los días, y las cámaras no aceptan el sábado. |
| S7 | ¿Cuánto dura un bloqueo y quién decide la duración? | R11 no lo dice. | El admin pone la fecha de fin cada vez que bloquea a alguien. |
| S8 | «Mínimo 1 h para cancelar» (R14): ¿se mide antes del inicio de la reserva o después de crearla? | La frase es ambigua. | Se puede cancelar hasta 1 h antes del inicio de la reserva. |
| S9 | ¿Desde cuándo cuenta la hora del carrito (R15)? | No se dice. | Desde que se agrega el primer ítem. |
| S10 | ¿Se puede reservar un ítem que está en mantenimiento? | R6 solo dice que se ve con una nota. | Se ve, pero no se puede reservar mientras esté en mantenimiento. |
| S11 | ¿Quién da de alta los ítems y cambia su estado (disponible, mantenimiento, dañado, extraviado)? | No se dice. | El admin crea, edita y cambia el estado de los ítems desde su panel. |
| S12 | ¿Cómo se identifican los usuarios? ¿Con cuenta institucional? ¿Con registro libre? | No se dice. | Inician sesión con su correo institucional de la USFQ, y los admins se crean a mano. |
| S13 | ¿Los profesores tienen reglas distintas a las de los estudiantes? | R3 los agrupa como «usuario general». | Tienen las mismas reglas que los estudiantes. |
| S14 | ¿Qué datos lleva el PDF y quién lo recibe? | R·10:45 no lo dice. | Lleva el ID de la reserva, el usuario, los ID de los ítems, las fechas y horas, el comentario y el estado. Lo pueden descargar el usuario y el admin. |
| S15 | ¿Dos solicitudes pendientes pueden pedir el mismo ítem en fechas que se cruzan? | No se dice. | Solo una reserva confirmada bloquea el ítem en esas fechas. Si hay varias pendientes que se cruzan, el admin decide a mano. |
| S16 | ¿Al usuario le llega un correo si su reserva se rechaza o la edita el admin? | R8 solo menciona el envío y la confirmación. | Sí, se le notifica el rechazo (con un motivo) y las ediciones del admin. |
| S17 | ¿A qué se refiere el correo de «confirmación de reserva» para el admin (R8)? | La frase es ambigua. | Es una copia de la confirmación que se le manda al usuario. |
| S18 | ¿Qué datos tiene el resumen del panel del admin (R18)? | Solo se dice «total de reservas». | Muestra cuántas reservas hay en cada estado: pendientes, confirmadas, activas, atrasadas y cerradas. |
| S19 | ¿La bodega tiene un horario de atención para retirar o devolver? | No se dice (R7 solo habla de días por tipo). | No hay una restricción de horario aparte de RN13. |
| S20 | ¿Cómo va a medir el éxito el cliente? ¿Hay fecha de entrega o restricciones técnicas? | No se dice. | Se usan los criterios provisionales de la sección 9. |

## 8. Épicas del MVP

1. **E1 · Catálogo e inventario.** El admin gestiona los ítems con ID único y sus estados, y los usuarios ven el catálogo filtrado según su rol (funcionalidades 1, 12, 13 y 14).
2. **E2 · Reserva del usuario.** Incluye el carrito con varios ítems, el selector de fecha y hora, el comentario, el envío, la cancelación y el PDF (funcionalidades 2 a 6, 10 y 16).
3. **E3 · Gestión del administrador.** Incluye la aprobación manual, la edición de reservas, los bloqueos y el panel con el resumen (funcionalidades 7, 11, 15 y 17).
4. **E4 · Notificaciones por correo.** Incluye los correos de solicitud y confirmación y los recordatorios de devolución (funcionalidades 8 y 9).

## 9. Criterios de éxito y restricciones

### Restricciones (vienen de la fuente)

- El nombre «Bodega COCOA USFQ» aparece en el título de la página (R22).
- Las horas van en formato de 24 h (R21).
- Las dos partes son responsive (R19, R20).
- Ninguna decisión sobre una reserva se toma sin un administrador (R5, R·10:42).

### Criterios de éxito (provisionales, ver S20)

- El 100 % de las reservas confirmadas tienen registrada la aprobación de un admin (RN7).
- Un usuario puede crear una reserva de 2 ítems del mismo modelo, cada uno elegido por su ID, desde el celular y sin ayuda (RN1 a RN3, R17, R19).
- Los correos de RN11 se mandan a la hora correcta (T−10 y T+30) en todas las pruebas.
- Ningún ítem dañado o extraviado aparece en la vista de usuario (RN15).
- Cada reserva confirmada tiene su PDF descargable (RN19).
