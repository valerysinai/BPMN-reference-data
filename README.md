# Resumen General — Proyecto Horarios SENA

---

## 1. BPMN (Business Process Model and Notation)

**¿Qué es?**
Notación gráfica estándar (mantenida por el OMG) para modelar procesos de negocio, entendible tanto por negocio como por TI. La versión más usada es BPMN 2.0, que además permite ejecutar los diagramas en motores BPM.

**¿Para qué sirve?**
- Documentar procesos de forma clara y estandarizada
- Detectar cuellos de botella
- Servir de puente entre negocio y desarrollo
- Automatizar procesos (motores BPM)

**¿Cómo funciona?**
Un diagrama BPMN (BPD) se lee de izquierda a derecha, desde un evento de inicio hasta uno de fin, combinando estos elementos:

| Elemento | Símbolo | Qué representa |
|---|---|---|
| Evento | Círculo | Algo que sucede (inicio, intermedio, fin) |
| Actividad | Rectángulo redondeado | Trabajo realizado (tarea o subproceso) |
| Gateway | Rombo | Decisión: bifurca o une el flujo (exclusivo, paralelo, inclusivo) |
| Flujo de secuencia | Flecha sólida | Orden de ejecución |
| Flujo de mensaje | Flecha punteada | Comunicación entre participantes |
| Pool / Lane | Carril | Quién es responsable (organización / rol) |

**Ejemplo de lectura**
```
(Inicio) → [Tarea: Recibir solicitud] → <¿Aprobada?>
                                            ├── Sí → [Procesar pedido] → (Fin)
                                            └── No → [Notificar rechazo] → (Fin)
```

**Buenas prácticas**
- Siempre un evento de inicio y uno de fin
- Nombrar tareas con verbo + sustantivo ("Validar documento")
- Usar pools para distintas organizaciones y lanes para roles internos
- No abusar de gateways; mantener detalle uniforme

**Herramientas comunes:** Bizagi Modeler, Camunda Modeler, draw.io, Lucidchart, Signavio

**BPMN vs otras notaciones**

| Notación | Enfoque |
|---|---|
| BPMN | Procesos de negocio, ejecutable |
| Flowchart | Simple, no estandarizado |
| UML (actividades) | Diseño de software |
| EPC | Similar a BPMN, usado en SAP/ARIS |

> **¿Necesita BPMN el microservicio `reference-data`?** No. Sus dos dominios (`institutional_structure` y `parameterization`) son datos maestros/catálogos: CRUD sin decisiones de negocio, actores múltiples ni eventos complejos. BPMN aplica donde sí hay ese tipo de lógica, como en `scheduling-service` (motor de horarios) o `monitoring-service`, identificados como subdominios **CORE** del sistema.

---

## 2. Microservicio de Datos de Referencia (`reference-data-service`)

| Dominio | Tablas | Responsabilidad |
|---|---|---|
| **`institutional_structure`** | `macroregion`, `microregion`, `department`, `municipality`, `training_center`, `institutional_unit` | Jerarquía geográfica/organizacional (macrorregión → microrregión → departamento → municipio → centro de formación → unidad institucional). |
| **`parameterization`** | `catalog`, `catalog_detail`, `parameter` | Catálogos genéricos, valores parametrizables y configuración del sistema. |

---

## 3. Diferencia entre Dominio y Microservicio

| | **Dominio** | **Microservicio** |
|---|---|---|
| Naturaleza | Concepto de negocio (DDD) | Decisión de arquitectura/infraestructura |
| Qué es | Área de responsabilidad con sus propias entidades y reglas | Proceso independiente, desplegable por separado |
| Implica | Lenguaje, entidades y lógica cohesivas | API propia, ciclo de vida propio, BD propia (idealmente) |
| Cardinalidad | Puede vivir dentro de un monolito o de varios servicios | Puede contener uno o varios dominios |

**En corto:** el dominio responde *"¿qué hace esto para el negocio?"*; el microservicio responde *"¿cómo se despliega y ejecuta esto?"*.

**Relación entre ambos:**
```
Dominio (negocio)  →  puede mapearse a  →  Microservicio (despliegue)
```

En este proyecto, los dominios `institutional_structure` y `parameterization` conviven hoy dentro de **un solo microservicio** (`reference-data-service`). No es obligatorio separarlos: solo se dividirían en microservicios distintos si el acoplamiento entre ellos es bajo y la complejidad/escala lo justifica.

---

## 4. `design-software-docs` — Resumen corto

**Qué es:** repositorio de documentación oficial del proyecto **Horarios SENA** (PRJ-EDU-HORARIOS). Fuente única de verdad documental; no contiene código.

**Problema que resuelve:** hoy los coordinadores académicos del SENA arman horarios de forma manual, sin detectar conflictos de instructores/ambientes/franjas. Meta: crear y publicar un horario válido en **< 1 hora**, con detección automática de conflictos y visibilidad en tiempo real.

**Arquitectura**
- Microservicios con DDD + Hexagonal, 1 base de datos PostgreSQL por servicio
- REST síncrono + eventos asíncronos (broker de mensajes)
- Auth con JWT (`iam-service`), object storage para documentos, caché Redis

**Servicios (9 + 1 en camino)**

| Servicio | Función |
|---|---|
| `iam-service` | Autenticación/autorización |
| `reference-data-service` | Catálogos institucionales |
| `academic-management-service` | Programas y fichas |
| `training-environment-service` | Ambientes físicos |
| `scheduling-service` | Motor de horarios (**núcleo**) |
| `actors-service` | Instructores, aprendices, empresas |
| `document-service` | PDFs y almacenamiento |
| `monitoring-service` | KPIs y alertas (**núcleo**) |
| `audit-service` | Auditoría append-only |
| `notification-service` | Notificaciones (en desarrollo) |

> Hoy solo existen las capas de datos (`*-db`); las APIs aún no están construidas.

**Estructura del repo**
17 carpetas numeradas (`00-governance` a `99-archive`): gobierno, contexto, dominio, producto, requisitos, arquitectura/ADRs, datos, API, UML, microservicios, DevOps, calidad, UX/UI, operaciones, training, control de proyecto y archivo.

**Reglas clave**
- Nada se sube directo a `dev`/`qa`/`main`: rama + PR + revisión
- Archivos en `kebab-case.md`; carpetas con prefijo numérico
- ADRs en `05-architecture/decisions/records/`
- Prohibido publicar credenciales o datos sensibles

**Estado actual**
236 documentos: 50 estables 🟢, 102 en progreso 🟡, 36 pendientes 🔴 — proyecto en fase de diseño avanzada pero activa.
