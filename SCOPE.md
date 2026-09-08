# SCOPE

## Problema
Una persona reporta una falla. Alguien atiende la falla, la resuelve y queda registro.

## Usuarios
- Solicitante: abre tickets
- Técnico: los atiende
- Admin: ve reportes y usuarios

## v1 
- Login y roles 
- Abrir ticket (título, descripción, prioridad)
- Listar / filtrar por estado
- Asignar y cambiar el estado: abierto, en proceso,  cerrado.
- Comentarios en el ticket
- Reporte simple: tickets por estado y por persona


## Pantallas
1. Login
2. Lista de tickets
3. Detalle del ticket
4. Alta de usuario (solo admin)
5. Reporte básico

## Flujos
1. Usuario crea ticket, queda en estado "abierto"
2. Técnico lo acepta, el estado cambia a "en  proceso", comenta, y luego el estado pasa a "cerrado"
3. Admin ve cuántos cerró cada técnico
