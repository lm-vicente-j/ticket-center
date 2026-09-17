# SCOPE

## Problema
Una persona reporta una falla. Alguien atiende la falla, la resuelve y queda registro.

## Usuarios
- Solicitante: abre tickets y comenta en los propios
- Técnico: atiende y resuelve los tickets que tiene asignados.
- Supervisor: vigila la cola del equipo, asigna y reasigna técnicos, ve reportes, puede reabrir tickets.
- Administrador: gestiona usuarios y roles.

## v1 
- Login y roles 
- Abrir ticket (título, descripción, prioridad).
- Listar / filtrar por estado.
- Asignar y cambiar el estado: abierto, en proceso,  cerrado.
- Comentarios en el ticket, solo mientras está en proceso.
- Reporte simple: tickets por estado y por persona.
- Exportar el reporte a archivo y conservarlo en el servidor.


## Pantallas
1. Login
2. Lista de tickets
3. Detalle del ticket
4. Alta de usuario (solo administrador)
5. Reporte básico

## Flujos de trabajo
1. Usuario crea ticket, queda en estado "abierto"
2. Técnico lo acepta, el estado cambia a "en  proceso", comenta, y luego el estado pasa a "cerrado"
3. Supervisor ve cuántos cerró cada técnico y exporta el reporte a archivo

## Fuera del alcance
1. Sistema de notificaciones.