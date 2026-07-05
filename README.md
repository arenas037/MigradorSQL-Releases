# RedesipSuite SQL Server (MigradorSQL)

![Redesip Logo](https://img.shields.io/badge/Redesip-SQL_Suite-blue?style=for-the-badge&logo=microsoft-sql-server)
![Version](https://img.shields.io/badge/Version-1.512-brightgreen?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Estable-success?style=for-the-badge)

La **RedesipSuite SQL Server** es una potente aplicación de escritorio escrita en Java (Swing) diseñada específicamente para interactuar, analizar, mantener y migrar bases de datos de Microsoft SQL Server de forma segura, rápida y controlada.

## Enlaces Rápidos
- ⬇️ **[Descargar Última Versión (v1.512)](https://github.com/arenas037/MigradorSQL-Releases/raw/main/RedesipSuiteSQLSERVER.exe)**

---

## Índice
- [Descripción General](#descripción-general)
- [Manual de Módulos](#manual-de-módulos)
  - [1. Buscador de Datos SQL](#1-buscador-de-datos-sql)
  - [2. Gestor de Paquetes (Data Packager)](#2-gestor-de-paquetes-data-packager)
  - [3. Migrador de Bases de Datos](#3-migrador-de-bases-de-datos)
  - [4. Reductor de Logs (Shrink)](#4-reductor-de-logs-shrink)
  - [5. Reparador DBCC](#5-reparador-dbcc)
  - [6. Reparador de Usuarios Huérfanos](#6-reparador-de-usuarios-huérfanos)
  - [7. Monitor de Bloqueos (Deadlocks)](#7-monitor-de-bloqueos-deadlocks)
  - [8. Gestor Masivo de Respaldos](#8-gestor-masivo-de-respaldos)
  - [9. Mantenimiento de Índices](#9-mantenimiento-de-índices)
  - [10. Reporteador SQL](#10-reporteador-sql)
  - [11. Limpiador de Tablas Temporales](#11-limpiador-de-tablas-temporales)
  - [12. Buscador de Dependencias](#12-buscador-de-dependencias)
  - [13. Gestor de Acceso (Usuarios DB)](#13-gestor-de-acceso-usuarios-db)
- [Control de Versiones (Changelog)](#control-de-versiones-changelog)

---

## Descripción General
**Redesip Suite SQL Server** es una potente herramienta todo-en-uno que abstrae la complejidad de la administración de MS SQL Server mediante una interfaz gráfica limpia y moderna. Desde realizar copias de seguridad de múltiples bases de datos con un clic, hasta empaquetar tablas completas y reparar corrupción, esta suite garantiza un control seguro y estructurado.

---

## Manual de Módulos

### 1. Buscador de Datos SQL
Este módulo te permite buscar un valor específico (números, textos, fechas) en *toda* una base de datos o en una tabla específica, sin tener que escribir sentencias `SELECT` interminables.

* **Caso de Uso:** Necesitas saber en qué tabla y columna se guardó el ID de transacción "TX1004" pero la base de datos tiene más de 300 tablas.
* **Características:**
  * Uso inteligente de `NOLOCK` para no bloquear tablas en producción.
  * Autodetección de tipos de datos.

### 2. Gestor de Paquetes (Data Packager)
Permite extraer registros de múltiples tablas y empaquetarlos en un archivo portátil (`.agy`), para luego importarlos en otra base de datos de manera sincronizada.

* **Caso de Uso:** Tienes un ambiente de Pruebas y un ambiente de Producción. Quieres pasar 50 nuevos artículos y 2 nuevas categorías desde Pruebas a Producción sin reescribir IDs.
* **Características:**
  * Validaciones de seguridad para bloquear operaciones destructivas accidentales.
  * Modos de importación: Diferencial (solo nuevos), Sincronización (Insert/Update), y Modo Normal.

### 3. Migrador de Bases de Datos
Transfiere esquemas, tablas, datos, procedimientos y vistas desde una base de datos de origen a una de destino, incluso operando entre servidores completamente diferentes (ej. SQL Server 2008 a SQL Server 2022).

* **Caso de Uso:** Migrar un cliente desde un servidor Legacy inestable hacia un servidor Cloud moderno, asegurando que todos los objetos se traspasen sin pérdida de datos.
* **Características Destacadas:**
  * **Modos de Migración Flexibles:** 
    * **Solo Datos:** Sincroniza información. Incluye la opción inteligente de *Crear Tablas Faltantes*.
    * **Solo Estructura (vía SMO):** Extrae la definición de tablas, vistas y SPs 100% fieles al origen usando scripts nativos.
    * **FULL:** Estructura completa + Inserción de Datos masiva.
  * **Migración Personalizada:** Interfaz quirúrgica con pestañas (Tablas, Vistas, Procedimientos) que te permite seleccionar exactamente qué objetos individuales deseas transferir.
  * **Resolución Inteligente de Dependencias:** Respeta y resuelve automáticamente llaves foráneas (Foreign Keys) para evitar errores de integridad referencial.
  * **Tolerancia a Fallos:** Ignora lotes erróneos (por dependencias externas irresolubles) permitiendo que la creación de la base de datos continúe sin abortar el proceso.

### 4. Reductor de Logs (Shrink)
Las bases de datos SQL Server pueden generar archivos de registro (`.ldf`) gigantescos. Este módulo los reduce de forma segura y aplica modelos de recuperación adaptados al caso (Simple/Full).

* **Caso de Uso:** El disco del servidor está al 99% de capacidad porque el archivo de transacciones creció a 100 GB.
* **Características:**
  * Reducción inteligente por tamaño objetivo en MB.

### 5. Reparador DBCC
Verifica la integridad física y lógica de todos los objetos en la base de datos especificada usando `DBCC CHECKDB`.

* **Caso de Uso:** El servidor sufrió un apagón y sospechas de corrupción de datos o páginas rotas.
* **Características:**
  * Forzado inteligente de modo `SINGLE_USER`. Si hay otras conexiones bloqueando el proceso, el sistema es capaz de hacer `KILL` automáticamente a todas las sesiones invasoras para garantizar la reparación.
  * Soporte para `REPAIR_ALLOW_DATA_LOSS` en casos críticos (bases de datos `SUSPECT`).

### 6. Reparador de Usuarios Huérfanos
Identifica y re-vincula usuarios de bases de datos (Database Users) que han perdido su conexión con el inicio de sesión del servidor (Server Login).

* **Caso de Uso:** Restauraste una base de datos de otro servidor y ahora la aplicación no conecta porque el SID del usuario no coincide con el del servidor.
* **Características:**
  * Opción "Auto-Fix" de un clic para usuarios sin contraseña conocida.

### 7. Monitor de Bloqueos (Deadlocks)
Visualiza en tiempo real los procesos bloqueados o en espera en la instancia de SQL Server.

* **Caso de Uso:** Una consulta pesada dejó "colgado" el sistema de facturación.
* **Características:**
  * Permite matar la sesión (`KILL`) directamente desde la interfaz.
  * Muestra el texto de la consulta infractora.

### 8. Gestor Masivo de Respaldos
Realiza backups completos, diferenciales o de registro de múltiples bases de datos al mismo tiempo, mostrando el progreso de forma visual.

* **Caso de Uso:** Respaldo preventivo antes de actualizar la aplicación de contabilidad (ERP).
* **Características:**
  * Respaldo por lotes.
  * Opción de añadir fecha/hora al nombre de archivo.

### 9. Mantenimiento de Índices
Analiza el grado de fragmentación de los índices de una base de datos y permite reorganizarlos o reconstruirlos.

* **Caso de Uso:** Las consultas de búsqueda están muy lentas tras meses de uso intenso del sistema.
* **Características:**
  * Sugiere REORGANIZE o REBUILD basado en el porcentaje de fragmentación.

### 10. Reporteador SQL y Ejecutor Nativo
Un editor SQL profesional y motor de ejecución integrado capaz de procesar consultas (`SELECT` o `EXEC SP`), además de interpretar y ejecutar reportes desarrollados originalmente en el sistema base de Delphi (guardados como T-Strings/TWriter serializados en BLOBs).

* **Caso de Uso:** El departamento de finanzas solicita un reporte complejo (ej. ventas cruzadas del año en curso) cuyo volumen de datos supera las 800 mil líneas, causando que el sistema legacy colapse o se quede sin memoria. El Reporteador extrae el código, genera la UI de parámetros y exporta el resultado directamente saltándose las limitaciones del cliente antiguo.
* **Características Destacadas:**
  * **Extracción Nativa de Código SQL:** Deserializa datos binarios (BLOBs) en tiempo real para recuperar el T-SQL original del reporte, permitiendo inspeccionarlo con doble clic.
  * **Parámetros Dinámicos (UI Generada al Vuelo):** Crea una interfaz de usuario a partir de metadatos (`INFORMEPARAMETROS`), con soporte para calendarios (fechas), validación estricta de números y buscadores genéricos interactivos para referencias cruzadas (opción `SELECCION`).
  * **Formato Condicional:** Aplica reglas de visibilidad de columnas (`INFORMECOLUMNAS`) automáticamente en los resultados.
  * **Exportación Masiva y Anti-Inyección:** Exporta a **Excel, CSV, JSON, XML, HTML y TXT**. Bloquea cualquier intento de `UPDATE`/`DELETE` accidental garantizando seguridad total en modo lectura.

### 11. Limpiador de Tablas Temporales
**Propósito:** Detectar y eliminar de manera segura todas aquellas tablas temporales "huérfanas" (`ATABLATEMPTIPOSDOC%`, `ATEMPARTICSLIN%`, `TEMP_INVENTARIO%`, o `GUIDs`) que el sistema crea pero no elimina, evitando acumulación de basura en la base de datos.
* **Casos de Uso:**
  * Limpieza rutinaria tras procesos intensivos en el sistema.
* **Cómo usarlo:**
  * Al ingresar, presiona **Analizar DB**. Esto no borrará nada, sólo generará una lista visual de qué tablas cumplen el criterio.
  * Revisa la lista, y si estás de acuerdo, presiona **Limpiar Tablas**. El sistema ejecutará el `DROP` tabla por tabla, capturando cualquier error si alguna llegara a estar bloqueada.

### 12. Buscador de Dependencias
Identifica si objetos en tu base de datos actual (Triggers, Vistas, Procedimientos Almacenados) dependen de otras bases de datos externas en el mismo servidor.
* **Caso de Uso:** Estás limpiando un servidor y necesitas saber si puedes eliminar la base de datos "FACTURACION_OLD" sin romper objetos en la base de datos de "PRODUCCION".
* **Características:**
  * Escanea el código fuente (definición) de todos los módulos SQL.
  * Muestra el nombre del objeto local y qué base de datos externa está referenciando.

### 13. Gestor de Acceso (Usuarios DB)
Permite activar (Habilitar) o desactivar (Denegar conexión) rápidamente a los usuarios de una base de datos específica (excepto cuentas críticas como `dbo` o `sa`).
* **Caso de Uso:** Vas a realizar un mantenimiento crítico en una base de datos y quieres evitar ingresos accidentales de usuarios de aplicación durante el proceso por seguridad.
* **Características:**
  * Interfaz de selección por casillas de verificación (Checkboxes).
  * Aplicación masiva de permisos de estado `CONNECT`.

---

## Control de Versiones (Changelog)

### v1.512 (Actual)
* **[NUEVO]** Modo Demo de la aplicación implementado. Permite la evaluación total del sistema por un tiempo limitado de manera segura, omitiendo la necesidad del autenticador de instalación inicial.

### v1.511
* **[NUEVO]** Módulo Reporteador: Capacidad para leer, interpretar y deserializar reportes nativos (T-Strings/TWriter) almacenados como BLOBs en la tabla `INFORME`.
* **[NUEVO]** Módulo Reporteador: Generación dinámica de UI para parámetros (`INFORMEPARAMETROS`), incluyendo validadores numéricos, máscaras de fecha y buscadores genéricos interactivos para campos relacionados (`SELECCION`).
* **[NUEVO]** Módulo Reporteador: Visor de código SQL original integrado (doble clic en lista de reportes) con opción de copiar al portapapeles.
* **[MEJORA]** Módulo Reporteador: Soporte para formato condicional de columnas (ocultar/mostrar basado en `INFORMECOLUMNAS`).
* **[MEJORA]** Interfaz renovada con `GridBagLayout` para una presentación mucho más limpia y profesional de los parámetros.

### v1.510
* **[NUEVO]** Migrador: Opción "Solo Datos (Crear Tablas Faltantes)". Detecta y crea automáticamente en destino únicamente las tablas que no existen, optimizando el tiempo y evitando sobrescribir estructura existente.
* **[NUEVO]** Migrador: Opción de Migración "Personalizada". Interfaz avanzada con pestañas (Tablas, Vistas, Procedimientos) para seleccionar objetos específicos a migrar de forma quirúrgica.
* **[MEJORA]** Motor de Migración (PowerShell SMO): Adaptado para inyectar dinámicamente objetos seleccionados y respetar configuraciones personalizadas en tiempo real.
* **[LOGRO]** Casos de éxito comprobados en producción: Migraciones exitosas en modo Solo Data y modo FULL (incluyendo procesamiento masivo de tablas con más de 1 billón de registros).

### v1.509
* **[NUEVO]** Módulo "Buscador de Dependencias" para encontrar objetos que dependan de otras bases de datos.
* **[NUEVO]** Módulo "Gestor de Acceso" para desactivar/activar conexiones de usuarios de base de datos masivamente por seguridad durante mantenimientos.
* **[MEJORA]** Reparador DBCC ahora auto-mata sesiones bloqueantes (`KILL`) para garantizar que el `SINGLE_USER` sea exitoso al reparar BD en estado sospechoso.
* **[MEJORA]** El script de SMO (Migrador) fue optimizado en PowerShell para escribir correctamente delimitadores `GO` entre lotes, evitando fallas de sintaxis en `CREATE VIEW`.
* **[MEJORA]** Migrador ahora tiene tolerancia a fallos: ignora lotes erróneos (por dependencias externas) y continúa con la creación del resto de la base de datos.

### v1.506

### v1.505
* **[NUEVO]** Comprobador manual de actualizaciones en pantalla principal.
* **[FIX]** Solución de problemas de caché agresiva en CDN al descargar versiones nuevas.

### v1.504
* **[NUEVO]** Creación del módulo "Limpiador de Tablas Temporales".
* **[MEJORA]** Implementación de análisis previo para prevención de borrado ciego en limpieza de temporales.

### v1.503
* **[NUEVO]** Incorporación del Botón de Ayuda con redirección a esta documentación de GitHub.
* **[MEJORA]** Establecimiento de guías y documentación oficial.

### v1.502
* **[MEJORA]** Modernización visual completa mediante la integración del Look & Feel `FlatLaf`.
* **[MEJORA]** Inclusión de la DLL de Autenticación de Windows pre-empaquetada para prevenir errores en cualquier equipo cliente.
* **[NUEVO]** Sistema avanzado de coloreado de sintaxis SQL en el Reporteador.
* **[SEGURIDAD]** Implementación de protección Anti-Inyección en Exportadores y Reporteador para evitar alteraciones accidentales.
* **[FIX]** Solución de NullPointerException en el auto-actualizador causado por el sistema de ocultación de Jar2Exe.

### v1.100 - v1.501
* **[NUEVO]** Soporte `NOLOCK` en el Buscador Global para no afectar rendimiento de bases transaccionales.
* **[NUEVO]** Sistema de Auto-Actualización integrado con liberación vía GitHub Releases.
* **[NUEVO]** Creación del módulo Gestor de Paquetes (Data Packager) para despliegues parciales.

### v1.000
* Lanzamiento inicial privado de la Suite. Módulos básicos (Migrador, Respaldos).

---
*Cualquier incidencia, comunicarse con el soporte corporativo de Redesip.*
