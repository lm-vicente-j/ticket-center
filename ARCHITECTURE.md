# ARCHITECTURE

## 1. Vision general.

El sistema sigue una arquitectura tipo cliente servidor en un monolito.
Cada cliente consume la API proporcionada por el servidor,
el servidor contiene la logica del negocio y el acceso a la base de datos.

## 2. Sistema

Se listan los roles que interactuan con los componentes
de la aplicacion.

Roles:
 - Solicitante.
 - Tecnico
 - Supervisor
 - Administrador

Componentes del servidor:
 - Base datos.
 - Almacenamiento de archivos (reportes)

## 3. Modulos del sistema

El servidor es un solo despliegue, pero internamente se divide por dominio.
Cada módulo tiene sus datos y de las reglas que los protegen.

| Módulo | Responsabilidad | Reglas que protege | Dueño |
|---|---|---|---|
| Identidad y acceso | Autenticación, usuarios, roles | Solo el administrador crea usuarios y asigna roles | Pendiente |
| Tickets | Alta, listado, filtros, asignación, ciclo de vida | Transiciones de estado válidas y que roles pueden ejecutarlas | Pendiente |
| Comentarios | Hilo de comentarios e historial de actividad | Un comentario siempre pertenece a un ticket y a un autor, y solo se admite mientras el ticket está en proceso | Pendiente |
| Reportes | Agregaciones y exportación a archivo | Solo lectura sobre los demás módulos; nunca modifica tickets | Pendiente |

Dependencias permitidas:

 - Tickets depende de Identidad y acceso.
 - Comentarios depende de Tickets e Identidad y acceso.
 - Reportes lee de Tickets e Identidad y acceso.

Dependencias prohibidas:

 - Identidad y acceso no depende de ningún otro módulo.
 - Ningún módulo escribe directamente en las tablas de otro.

## 4. Restricciones

 - Un solo artefacto desplegable para el servidor (monolito) y una sola base
   de datos relacional.
 - Toda regla de negocio y de autorización se valida en el servidor. El cliente
   no es una fuente de verdad.
 - Sin notificaciones de estado en v1, según `SCOPE.md`.
 - Lenguaje, framework y tipo de cliente (escritorio o web): pendiente de
   decisión.

## 5. Modelo de datos

Entidades principales. El esquema exacto vive en las migraciones.

| Entidad | Campos relevantes | Relaciones | Invariantes |
|---|---|---|---|
| Usuario | nombre, credencial, rol, activo | Uno a muchos con Ticket y Comentario | Rol único por usuario; credencial nunca se almacena en claro |
| Ticket | título, descripción, prioridad, estado, fecha de creación, fecha de cierre | Pertenece a un solicitante; opcionalmente asignado a un técnico | Un ticket cerrado no se edita; solo puede tener un técnico asignado a la vez |
| Comentario | texto, fecha, autor | Pertenece a un Ticket y a un Usuario | Solo se puede crear si el ticket está en proceso; no se elimina ni se edita |
| HistorialEstado | estado anterior, estado nuevo, autor, fecha | Pertenece a un Ticket | Se escribe en cada transición, nunca se modifica |
| Reporte | tipo, parámetros, fecha de generación, ruta del archivo | Generado por un Usuario | El archivo es inmutable una vez generado |

Ciclo de vida del ticket:

```
abierto ──► en proceso ──► cerrado
```

| Transición | Quién puede ejecutarla |
|---|---|
| (alta) a abierto | Solicitante |
| abierto a en proceso | Técnico que lo acepta, o supervisor al asignarlo |
| en proceso a cerrado | Técnico asignado o supervisor |

La asignación de un ticket debe ser una operación atómica, para que dos
técnicos no puedan tomar el mismo ticket a la vez.

Los comentarios solo se pueden escribir mientras el ticket está en proceso.
La discusión ocurre durante la atención: un ticket abierto todavía no tiene
técnico con quien discutir, y uno cerrado queda como registro inmutable.
Los comentarios existentes siguen siendo visibles en cualquier estado.

## 6. Flujos de trabajo

1. Alta: el solicitante crea un ticket con título, descripción y prioridad.
   Queda en estado abierto y sin técnico asignado.
2. Atención: un técnico acepta el ticket o el supervisor lo asigna. El estado
   pasa a en proceso y se registra la transición en el historial. El técnico
   comenta el avance y al terminar cierra el ticket.
3. Reasignación: el supervisor puede cambiar el técnico asignado mientras el
   ticket no esté cerrado.
4. Reporte: el supervisor consulta los tickets por estado y por técnico, y
   exporta el resultado a un archivo que queda almacenado en el servidor.
5. Gestión de usuarios: el administrador da de alta usuarios y les asigna rol.

### Contenido del reporte

Filtros que recibe: rango de fechas, técnico y estado. Los tres son
opcionales; sin filtros el reporte cubre todos los tickets.

Contenido mínimo de v1:

 - Totales por estado: cuántos tickets hay abiertos, en proceso y cerrados.
 - Por técnico: tickets asignados actualmente y tickets cerrados en el
   periodo consultado. El cierre se cuenta por quien ejecutó la transición
   según `HistorialEstado`, no por quien figura asignado al momento de la
   consulta, porque un ticket puede haber sido reasignado.
 - Totales generales: tickets creados y cerrados en el periodo.

Cabecera del archivo exportado, para que el reporte sea reproducible y
auditable: fecha y hora de generación, usuario que lo generó y filtros
aplicados. Sin esos datos, un archivo suelto no dice a qué periodo
corresponde.

El reporte es solo lectura y no modifica tickets. El formato del archivo
queda pendiente de decisión; el mínimo aceptable es un formato tabular que
se pueda abrir en una hoja de cálculo.

## 7. Desarrollo local

Pendiente hasta definir el stack. Esta sección debe documentar:

 - Requisitos previos y versiones.
 - Comandos para levantar base de datos, servidor y cliente.
 - Carga de datos de prueba con un usuario por cada rol.

## 8. Despliegue

Pendiente hasta definir el stack. Criterios ya acordados:

 - Servidor y base de datos centralizados; los clientes no acceden a la base
   de datos directamente.
 - Las migraciones de base de datos se aplican como parte de la publicación.
 - Cada versión publicada queda registrada en `CHANGELOG.md`.

## 9. Deuda tecnica

| Deuda o riesgo | Impacto | Cuándo atacarlo |
|---|---|---|
| Stack y tipo de cliente sin decidir | Bloquea despliegue y desarrollo local | Antes de escribir código |
| Decisiones sin ADR que las respalde | No se puede reconstruir el porqué | Al cerrar cada decisión |
| Autorización solo por rol, sin considerar pertenencia del ticket | Un técnico podría actuar sobre tickets de otro | Antes de v1 |
| Archivos de reporte en disco local sin respaldo | Pérdida de reportes generados | Cuando haya más de un entorno |
| Reportes calculados sobre las tablas operativas | Consultas lentas al crecer el volumen | Si el reporte tarda de forma perceptible |

## 10. Glosario

| Término | Significado en este sistema |
|---|---|
| Ticket | Registro de una falla reportada, con su ciclo de vida completo |
| Estado | Punto del ciclo de vida del ticket: abierto, en proceso o cerrado |
| Asignación | Relación entre un ticket y el técnico responsable de atenderlo |
| Solicitante | Usuario que reporta la falla |
| Técnico | Usuario que atiende y resuelve tickets asignados |
| Supervisor | Usuario que distribuye la carga del equipo y consulta reportes |
| Administrador | Usuario que gestiona cuentas y roles |
| Reporte | Agregación de tickets por estado y por técnico, exportable a archivo |
