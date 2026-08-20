# BPMN (Business Process Model and Notation)

## ¿Qué es?

Notación gráfica estándar (mantenida por el **OMG**) para modelar procesos de negocio, entendible tanto por negocio como por TI. La versión más usada es **BPMN 2.0**, que además permite ejecutar los diagramas en motores BPM.

## ¿Para qué sirve?

- Documentar procesos de forma clara y estandarizada
- Detectar cuellos de botella
- Servir de puente entre negocio y desarrollo
- Automatizar procesos (motores BPM)

## ¿Cómo funciona?

Un diagrama BPMN (**BPD**) se lee de **izquierda a derecha**, desde un evento de inicio hasta uno de fin, combinando estos elementos:

| Elemento | Símbolo | Qué representa |
|---|---|---|
| **Evento** | Círculo | Algo que sucede (inicio, intermedio, fin) |
| **Actividad** | Rectángulo redondeado | Trabajo realizado (tarea o subproceso) |
| **Gateway** | Rombo | Decisión: bifurca o une el flujo (exclusivo, paralelo, inclusivo) |
| **Flujo de secuencia** | Flecha sólida | Orden de ejecución |
| **Flujo de mensaje** | Flecha punteada | Comunicación entre participantes |
| **Pool / Lane** | Carril | Quién es responsable (organización / rol) |

## Ejemplo de lectura

```
(Inicio) → [Tarea: Recibir solicitud] → <¿Aprobada?>
                                            ├── Sí → [Procesar pedido] → (Fin)
                                            └── No → [Notificar rechazo] → (Fin)
```

## Buenas prácticas

- Siempre un evento de inicio y uno de fin
- Nombrar tareas con verbo + sustantivo ("Validar documento")
- Usar pools para distintas organizaciones y lanes para roles internos
- No abusar de gateways; mantener detalle uniforme

## Herramientas comunes

Bizagi Modeler, Camunda Modeler, draw.io, Lucidchart, Signavio

## BPMN vs otras notaciones

| Notación | Enfoque |
|---|---|
| BPMN | Procesos de negocio, ejecutable |
| Flowchart | Simple, no estandarizado |
| UML (actividades) | Diseño de software |
| EPC | Similar a BPMN, usado en SAP/ARIS |
