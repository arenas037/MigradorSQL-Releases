<p align="center">
  <a href="https://redesip.org">
    <!-- Si tienes un logo público, se reemplaza la URL aquí. Por ahora, usamos el texto con la imagen de marca -->
    <h1 align="center">Redesip Suite SQL Server</h1>
  </a>
</p>

<p align="center">
  <em>Desarrollado por Redesip.org para simplificar, optimizar y asegurar el mantenimiento de bases de datos SQL Server.</em>
</p>

---

## Índice
- [Descripción General](#descripción-general)
- [Manual de Módulos](#manual-de-módulos)
  - [1. Buscador de Datos SQL](#1-buscador-de-datos-sql)
  - [2. Gestor de Paquetes (Data Packager)](#2-gestor-de-paquetes-data-packager)
  - [3. Migrador de Bases de Datos](#3-migrador-de-bases-de-datos)
  - [4. Reductor de Logs (Shrink)](#4-reductor-de-logs-shrink)
  - [5. Reparador (CheckDB)](#5-reparador-checkdb)
  - [6. Usuarios Huérfanos](#6-usuarios-huérfanos)
  - [7. Gestor de Respaldos](#7-gestor-de-respaldos)
  - [8. Reporteador SQL](#8-reporteador-sql)
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
Transfiere esquemas, tablas, datos, procedimientos y vistas desde una base de datos de origen a una de destino, incluso entre diferentes servidores.

* **Caso de Uso:** Migrar un cliente desde un servidor Legacy a un servidor Cloud.
* **Características:**
  * Resuelve automáticamente dependencias y llaves foráneas para evitar errores de orden de inserción.

### 4. Reductor de Logs (Shrink)
Las bases de datos SQL Server pueden generar archivos de registro (`.ldf`) gigantescos. Este módulo los reduce de forma segura y aplica modelos de recuperación adaptados al caso (Simple/Full).

* **Caso de Uso:** El disco del servidor está al 99% de capacidad porque el archivo de transacciones creció a 100 GB.
* **Características:**
  * Reducción inteligente por tamaño objetivo en MB.

### 5. Reparador (CheckDB)
Verifica la integridad física y lógica de todos los objetos en la base de datos especificada usando `DBCC CHECKDB`.

* **Caso de Uso:** El servidor sufrió un apagón y sospechas de corrupción de datos o páginas rotas.

### 6. Usuarios Huérfanos
Identifica y re-vincula usuarios de bases de datos (Database Users) que han perdido su conexión con el inicio de sesión del servidor (Server Login).

* **Caso de Uso:** Restauraste una base de datos de otro servidor y ahora la aplicación no conecta porque el SID del usuario no coincide con el del servidor.
* **Características:**
  * Opción "Auto-Fix" de un clic para usuarios sin contraseña conocida.

### 7. Gestor de Respaldos
Realiza backups completos, diferenciales o de registro de múltiples bases de datos al mismo tiempo, mostrando el progreso de forma visual.

* **Caso de Uso:** Respaldo preventivo antes de actualizar la aplicación de contabilidad (ERP).
* **Características:**
  * Respaldo por lotes.
  * Opción de añadir fecha/hora al nombre de archivo.

### 8. Reporteador SQL
Un editor SQL profesional integrado para ejecutar consultas (`SELECT` o `EXEC SP`), previsualizar los resultados y exportarlos masivamente.

* **Caso de Uso:** El departamento de finanzas solicita un reporte complejo en Excel de las ventas cruzadas del año en curso.
* **Características:**
  * Exportación nativa a **Excel, CSV, JSON, XML, HTML y TXT** respetando los tipos de datos estrictos (fechas, decimales).
  * Editor `RSyntaxTextArea` con coloreado de sintaxis SQL.
  * Protección Anti-Inyección bloqueando modificaciones de data (No Update/Delete).
  * Autenticación integrada de Windows Auth pre-configurada.

---

## Control de Versiones (Changelog)

### v1.503 (Actual)
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
