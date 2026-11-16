# Changelog

Todos los cambios notables de este proyecto se documentarán en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/),
y este proyecto se adhiere al [Versionado Semántico](https://semver.org/lang/es/).

## [No publicado]

### Añadido

- Definición de C# con .NET 10 LTS para v1. Servidor y cliente separados.
  Todos los clientes apuntarán al servidor. La base de datos se ejecutará
  en el equipo del servidor.
- Documento de alcance inicial (`SCOPE.md`) con usuarios, alcance de v1,
  pantallas y flujos principales.
- Documento de arquitectura (`ARCHITECTURE.md`) con visión general
  cliente-servidor, módulos por dominio, modelo de datos, flujos de trabajo,
  deuda técnica conocida y glosario.
- Plantillas de issue en `.github/ISSUE_TEMPLATE/` para tareas de
  implementación y para decisiones de diseño.

### Cambiado

- `README.md`: se agregan descripción, estado, funciones previstas y usuarios.
  Se documentan las dependencias, la instalación pendiente y el uso previsto.
  Se enlazan los documentos del proyecto.
- `SCOPE.md`: se separan los roles de Supervisor y Administrador, y se agrega la exportación del reporte a archivo dentro del alcance de v1.
- Los comentarios solo se pueden escribir mientras el ticket está en proceso.

<!--
Secciones disponibles, usar solo las que apliquen en cada versión:

### Añadido       para funcionalidades nuevas.
### Cambiado      para cambios en funcionalidades existentes.
### Obsoleto      para funcionalidades que se eliminarán próximamente.
### Eliminado     para funcionalidades eliminadas.
### Corregido     para corrección de errores.
### Seguridad     en caso de vulnerabilidades.

Al liberar una versión, renombrar [No publicado] a [X.Y.Z] - YYYY-MM-DD,
crear una nueva sección [No publicado] vacía arriba y agregar los enlaces
de comparación al final del archivo:

[No publicado]: https://github.com/<org>/ticket-center/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/<org>/ticket-center/releases/tag/v0.1.0
-->
