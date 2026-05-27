# 🎫 Sistema de Gestión de Turnos Digitales - Tunomatico

## ✅ Descripcion Genereral del sistema

Este proyecto corresponde al modelado arquitectónico completo de un "Sistema de Gestión de Turnos Digitales (Tunomático)", orientado a optimizar y digitalizar el proceso de atención al cliente en sucursales con alto flujo de público, eliminando las filas físicas y reemplazándolas por una cola virtual inteligente, accesible tanto de forma presencial como remota.

El sistema considera los aspectos operacionales críticos de un entorno de atención multicanal, incluyendo:

El sistema considera los aspectos operacionales críticos de un entorno de atención multicanal, incluyendo:

- Solicitud y emisión de turnos desde tótems físicos o dispositivos remotos vía web.
- Gestión centralizada de la cola de atención con llamado secuencial por operador.
- Visualización en tiempo real del estado de la cola en pantallas centrales de la sucursal.
- Envío de comprobantes digitales e impresión de tickets físicos según el canal del cliente.
- Configuración flexible de categorías de servicio y parámetros de atención por administrador.
- Integración con proveedores externos de notificaciones vía API.
- Generación de reportes y auditoría de métricas de rendimiento operacional.

---

## 🔍 Objetivos del Modelado

- Desarrollar una transición completa desde la visión funcional hasta el despliegue físico del sistema.
- Aplicar patrones de diseño reconocidos en el diseño lógico y reflejar su materialización en la arquitectura de implementación.
- Ejercitar el pensamiento arquitectónico en la separación de responsabilidades, modularidad y escalabilidad del sistema.

---

## 🔹 1. Diagrama de Casos de Uso UML

<img width="1246" height="811" alt="image" src="https://github.com/user-attachments/assets/d4507e0c-37b6-4f09-9a03-0c6b16c2e7cb" />

### Descripción General

El análisis funcional del sistema Tunomático permitió identificar con precisión los actores involucrados y las funcionalidades críticas del flujo de atención. Se aplicaron correctamente las relaciones `<<include>>` y `<<extend>>` para reflejar de manera fiel los flujos obligatorios y los comportamientos condicionales del proceso.

---

### Actores Identificados

| Actor | Tipo | Descripción |
|---|---|---|
| **Cliente General** | Primario (abstracto) | Actor base que generaliza hacia Cliente Presencial y Cliente Remoto. Evita duplicar casos de uso idénticos para ambos subtipos. |
| **Cliente Presencial** | Primario | Especialización del Cliente General que interactúa mediante el tótem físico de la sucursal. |
| **Cliente Remoto** | Primario | Especialización del Cliente General que accede al sistema mediante un navegador web. |
| **Operador** | Primario | Responsable de la atención en ventanilla. Gestiona el llamado de turnos y los eventos derivados de la atención. |
| **Administrador** | Primario | Gestiona la configuración del sistema, los operadores asignados y la supervisión mediante reportes y métricas. |
| **API de Notificaciones** | Externo (sistema) | Actor externo de tipo sistema que recibe eventos del sistema para el despacho de comprobantes digitales. |

La relación de **generalización** entre `Cliente General` y sus subtipos `Cliente Presencial` y `Cliente Remoto` está técnicamente justificada: ambos subtipos comparten el comportamiento de solicitar un turno, pero difieren en el canal de acceso y en los flujos opcionales disponibles (impresión física vs. comprobante digital). Este modelado evita la duplicación de lógica funcional y refleja correctamente la herencia de comportamiento.

---

### Casos de Uso Principales y Relaciones Aplicadas

#### Relaciones `<<include>>`

Las relaciones `<<include>>` se utilizaron en todos aquellos flujos donde el caso de uso base **no puede ejecutarse sin** el caso de uso incluido, es decir, se trata de comportamientos obligatorios e inseparables.

- **Solicitar Turno de Atención → Autenticar Identidad**
  Toda solicitud de turno requiere obligatoriamente verificar la identidad del cliente antes de continuar. No existe el flujo principal sin esta validación previa, lo que justifica técnicamente el uso de `<<include>>` y descarta el uso de `<<extend>>`.

- **Solicitar Turno de Atención → Seleccionar Categoría de Servicio**
  No es posible asignar un turno sin conocer el área de atención a la que va dirigido. Esta selección siempre ocurre como parte del flujo principal, siendo un comportamiento inherente e ineludible.

- **Atender Siguiente Turno → Actualizar Pantalla Central**
  Cada vez que un operador llama al siguiente turno, la pantalla central de la sucursal debe actualizarse de forma automática e inmediata. Se trata de un comportamiento obligatorio e inseparable del flujo principal de atención.

- **Auditar Métricas de Rendimiento → Generar Reporte de Atención**
  Para generar cualquier reporte de atención es necesario primero auditar las métricas disponibles del sistema. El reporte no existe sin esa consulta previa, lo que hace que la relación sea obligatoria.

#### Relaciones `<<extend>>`

Las relaciones `<<extend>>` se emplearon para representar comportamientos **opcionales o condicionados** que extienden el flujo principal solo bajo circunstancias específicas.

- **Imprimir Ticket Físico → Solicitar Turno de Atención**
  Solo aplica para clientes presenciales que requieren el comprobante en papel. Es un flujo opcional y condicional al canal de acceso del cliente, lo que justifica el uso de `<<extend>>`.

- **Enviar Comprobante Digital → Solicitar Turno de Atención**
  Ocurre únicamente si el cliente tiene habilitadas las notificaciones digitales en su perfil o configuración. Al no ser un comportamiento universal, se modela correctamente como extensión.

- **Registrar Inasistencia → Atender Siguiente Turno**
  Si el cliente no se presenta cuando es llamado, el operador puede registrar la inasistencia. Se trata de un flujo alternativo y excepcional al proceso estándar de atención.

- **Derivar Turno Interdepartamental → Atender Siguiente Turno**
  Cuando el operador determina que el cliente debe ser atendido por un departamento distinto, puede derivar el turno activo. Ocurre solo bajo una condición específica detectada durante la atención.

---
