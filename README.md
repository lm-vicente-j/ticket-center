# ticket-center

Sistema interno de soporte para registrar fallas y dar seguimiento a su atención.
El cliente será una aplicación de escritorio para Windows.

## Estado del proyecto

El proyecto está en definición. El repositorio contiene la documentación inicial.
Todavía no hay una versión ejecutable.

## Funcionalidades previstas para v1

- Login y roles.
- Abrir tickets con título, descripción y prioridad.
- Listar y filtrar tickets por estado.
- Asignar técnicos y cambiar el estado de los tickets.
- Comentar mientras el ticket está en proceso.
- Consultar reportes por estado y por persona.
- Exportar reportes a archivo y conservarlos en el servidor.

Las notificaciones quedan fuera del alcance de v1.

## Usuarios

- Solicitante: abre tickets y comenta en los propios.
- Técnico: atiende y resuelve los tickets asignados.
- Supervisor: asigna y reasigna técnicos. Consulta la cola y los reportes.
- Administrador: gestiona usuarios y roles.

## Dependencias

- C# con .NET 10 LTS.
- Entity Framework Core 10 para el acceso a datos.
- WinForms para el cliente de escritorio.
- PostgreSQL como base de datos relacional.

## Instalación

Pendiente hasta implementar el servidor y el cliente.
Los comandos de instalación y configuración se documentarán cuando estén disponibles.

## Uso previsto

1. El solicitante abre un ticket. Queda en estado abierto.
2. El técnico lo acepta. El estado cambia a en proceso y permite comentarios.
3. El técnico termina la atención y cierra el ticket.
4. El supervisor consulta los reportes y exporta el resultado a un archivo.

## Documentación

- [Alcance](SCOPE.md): usuarios, pantallas y funciones de v1.
- [Arquitectura](ARCHITECTURE.md): flujo general, módulos y modelo de datos.
- [Historial de cambios](CHANGELOG.md): cambios registrados en el proyecto.

## Licencia

Pendiente de definir.
