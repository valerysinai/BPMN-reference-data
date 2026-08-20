# BPMN (Business Process Model and Notation)

## 1. ¿Qué es BPMN?

**BPMN** (Business Process Model and Notation) es una notación gráfica estándar para modelar procesos de negocio. Fue desarrollada originalmente por la **Business Process Management Initiative (BPMI)** y actualmente es mantenida por el **Object Management Group (OMG)**.

Su objetivo principal es proporcionar un lenguaje visual **comprensible tanto para personas técnicas (analistas, desarrolladores) como para personas del negocio** (gerentes, usuarios finales), de modo que todos puedan interpretar el mismo diagrama sin ambigüedades.

La versión más usada actualmente es **BPMN 2.0**, publicada en 2011, que además de la notación gráfica define un formato de intercambio en XML, permitiendo que los diagramas puedan ser ejecutados directamente por motores de procesos (BPMS).

---

## 2. ¿Para qué sirve?

- Documentar procesos de negocio de forma clara y estandarizada.
- Analizar y detectar cuellos de botella o ineficiencias en un proceso.
- Servir de puente entre el área de negocio y el área de desarrollo (TI).
- Automatizar procesos mediante motores BPM (Business Process Management Systems).
- Facilitar la mejora continua de procesos (rediseño, optimización).

---

## 3. ¿Cómo funciona? (Conceptos básicos)

Un diagrama BPMN se conoce como **BPD (Business Process Diagram)** y se construye combinando distintos tipos de elementos gráficos. El flujo se lee generalmente de **izquierda a derecha**, comenzando en un evento de inicio y terminando en uno o varios eventos de fin.

### 3.1 Categorías principales de elementos

| Categoría | Función |
|---|---|
| **Objetos de flujo** | Eventos, actividades y compuertas (gateways) — el corazón del diagrama |
| **Objetos de conexión** | Flechas que unen los objetos de flujo |
| **Swimlanes (carriles)** | Organizan y asignan responsabilidades (pools y lanes) |
| **Artefactos** | Información adicional: datos, anotaciones, grupos |

---

## 4. Elementos gráficos principales

### 4.1 Eventos (círculos)
Representan algo que **sucede** durante el proceso.

- **Evento de inicio** (círculo delgado, verde/blanco): dispara el proceso.
- **Evento intermedio** (círculo de doble línea): ocurre durante el proceso (espera, mensaje, temporizador, etc.).
- **Evento de fin** (círculo de borde grueso): termina el proceso.

Los eventos pueden tener un **disparador (trigger)** representado por un ícono dentro del círculo:
- ✉️ Mensaje
- ⏱️ Temporizador
- ⚠️ Error
- 🔗 Señal
- ⛔ Cancelación
- ➕ Múltiple

### 4.2 Actividades (rectángulos redondeados)
Representan el **trabajo que se realiza**.

- **Tarea (Task)**: unidad de trabajo atómica, no se descompone más.
  - Tarea de usuario, tarea de servicio, tarea manual, tarea de script, tarea de envío/recepción de mensaje, etc.
- **Subproceso (Sub-Process)**: agrupa varias tareas relacionadas; puede colapsarse o expandirse.

### 4.3 Compuertas / Gateways (rombos)
Controlan **cómo se bifurca o converge** el flujo.

| Tipo | Símbolo | Significado |
|---|---|---|
| **Exclusiva (XOR)** | ✕ o vacío | Solo un camino posible según una condición |
| **Paralela (AND)** | + | Todos los caminos se ejecutan simultáneamente |
| **Inclusiva (OR)** | ○ | Uno o varios caminos, según condiciones |
| **Basada en eventos** | ⬠ | El camino depende de qué evento ocurra primero |

### 4.4 Objetos de conexión (flechas)
- **Flujo de secuencia** (línea sólida): orden en que se ejecutan las actividades.
- **Flujo de mensaje** (línea discontinua): comunicación entre participantes distintos (pools).
- **Asociación** (línea punteada): conecta artefactos (datos, anotaciones) con objetos de flujo.

### 4.5 Swimlanes (carriles)
- **Pool (piscina)**: representa un participante completo del proceso (una empresa, un sistema, un departamento).
- **Lane (carril)**: subdivisión dentro de un pool que representa un rol, área o persona responsable.

Sirven para dejar claro **quién hace qué** dentro del proceso.

### 4.6 Artefactos
- **Objeto de datos**: información que entra o sale de una actividad.
- **Grupo**: agrupa visualmente elementos relacionados sin afectar el flujo.
- **Anotación de texto**: comentarios aclaratorios.

---

## 5. Ejemplo conceptual de lectura de un diagrama

```
(Inicio) → [Tarea: Recibir solicitud] → <Compuerta: ¿Aprobada?>
                                             ├── Sí → [Tarea: Procesar pedido] → (Fin)
                                             └── No → [Tarea: Notificar rechazo] → (Fin)
```

1. El proceso inicia con un **evento de inicio**.
2. Se ejecuta una **tarea**.
3. Una **compuerta exclusiva** evalúa una condición y decide el camino.
4. Cada camino termina en un **evento de fin**.

---

## 6. Buenas prácticas al modelar en BPMN

- Un proceso debe tener **al menos un evento de inicio y uno de fin**.
- Usar **nombres claros y en modo verbo-sustantivo** para las tareas (ej. "Validar documento", no solo "Documento").
- Evitar cruces innecesarios de flechas; mantener el diagrama de **izquierda a derecha**.
- Usar **pools** cuando haya más de una organización/sistema involucrado, y **lanes** para roles dentro de la misma organización.
- No abusar de las compuertas: usar solo las necesarias para representar la lógica real del proceso.
- Mantener un **nivel de detalle uniforme** (no mezclar procesos muy generales con pasos extremadamente específicos en el mismo diagrama).

---

## 7. Herramientas comunes para modelar BPMN

- **Bizagi Modeler** (gratuito, muy usado en Latinoamérica)
- **Camunda Modeler** (orientado a automatización/ejecución)
- **Draw.io / diagrams.net** (gratuito, con plantillas BPMN)
- **Lucidchart**
- **Signavio**

---

## 8. BPMN vs otras notaciones

| Notación | Enfoque |
|---|---|
| **BPMN** | Procesos de negocio, flujo de actividades, ejecutable |
| **Flowchart tradicional** | Más simple, menos estandarizado, no siempre ejecutable |
| **UML (Diagrama de actividades)** | Orientado a diseño de software, no exclusivo de negocio |
| **EPC (Event-driven Process Chain)** | Similar a BPMN pero usado principalmente en SAP/ARIS |

---

## 9. Resumen rápido

- **BPMN** = notación estándar para modelar procesos de negocio.
- Se compone de **eventos, actividades, compuertas, flujos, pools/lanes y artefactos**.
- Su fortaleza está en ser **entendible por negocio y TI a la vez**.
- Puede usarse solo para **documentar** o también para **ejecutar** procesos en un motor BPM.
