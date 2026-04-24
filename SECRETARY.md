# SECRETARY — Calendario de Sincronización de Memoria

Guía de cuándo y cómo cada agente debe actualizar su memoria (sistema PARA) después de cada run.

El skill de referencia es `paperclipai/paperclip/para-memory-files`.

---

## Calendario semanal de sync

| Agente | Día | Hora | Qué extrae |
|--------|-----|------|-----------|
| CEO | Lunes | 9:00 | Prioridades de la semana, bloqueos críticos, decisiones pendientes |
| DataAnalyst | Lunes | 9:15 | MRR actual, churn, clientes en riesgo, giros con mejor conversión |
| Scout | Martes | 9:00 | Giros y ciudades prospectadas, ratio de calidad (score ≥ 6 / total) |
| Qualifier | Martes | 9:30 | Score promedio por giro, hallazgos SEO recurrentes, paquete más recomendado |
| WebDesigner | Miércoles | 9:00 | Estilos usados (para no repetir), slugs de Surge.sh publicados |
| Outreach | Miércoles | 9:30 | Tasa de apertura, subject lines que funcionaron, canales más efectivos |
| Closer | Jueves | 9:00 | Objeciones más frecuentes, argumentos que cerraron, prospectos en pipeline activo |

---

## Template de actualización de memoria

Cuando un agente hace su sync semanal, usa el skill `para-memory-files` para actualizar o crear una nota con este formato:

```
## [AGENTE] — Sync semana de {FECHA}

### Hechos nuevos esta semana
- [hecho específico y verificado]
- [hecho específico y verificado]

### Patrones detectados
- [patrón con evidencia: "3 de 5 dentistas en CDMX tienen score > 7"]

### Decisiones que debo recordar
- [decisión o regla que aprendí esta semana]

### Para la semana que viene
- [prioridad o tarea pendiente]
```

---

## Qué extractar por agente

### CEO
- Qué agentes estuvieron bloqueados esta semana y por qué
- Qué giros y países están generando más leads calificados
- Clientes en riesgo de churn que necesitan atención directa

### DataAnalyst
- MRR total y tendencia (subiendo / bajando / estable)
- Tasa de conversión por etapa del pipeline (benchmark: Scout→Qualifier 80%, Qualifier→WebDesigner 60%, Outreach→Respuesta 20%, Closer→Cierre 10%)
- Giro con mejor conversión esta semana
- País con más prospectos calificados

### Scout
- Giros donde el 70%+ de prospectos tiene score ≥ 6 (priorizar)
- Giros donde el 70%+ tiene score < 6 (evitar o ajustar criterios)
- Fuentes de datos más efectivas por país (Google Maps vs Yelp vs directorios locales)

### Qualifier
- Score promedio por giro (actualizar tabla de benchmarks)
- Hallazgo SEO más común esta semana (ej: "80% sin Google Business Profile")
- Paquete más recomendado y por qué

### WebDesigner
- Último estilo usado por giro (para no repetir en el siguiente)
- Slugs de Surge.sh activos (para detectar si un prospecto ya tiene sitio)
- Componentes de 21st.dev que funcionaron bien (para reutilizar)

### Outreach
- Subject lines con apertura > 40% (guardar para reusar)
- Subject lines con apertura < 20% (marcar como "no usar")
- Canal más efectivo esta semana (email vs WhatsApp)
- Mejor horario de envío detectado

### Closer
- Objeciones más frecuentes y respuestas que funcionaron (framework LACE)
- Prospectos en seguimiento activo: nombre, giro, ciudad, fecha de último contacto
- Prospectos que cerraron esta semana (para que DataAnalyst actualice MRR)
- Prospectos marcados como CERRADO_SIN_RESPUESTA (no volver a contactar)

---

## Reglas de memoria

- **No guardar datos efímeros** — solo hechos que serán útiles en 2+ semanas
- **Siempre fechar los hechos** — un dato sin fecha pierde valor rápido
- **Actualizar, no acumular** — si un hecho cambió, reemplazar el anterior
- **Hechos, no opiniones** — "el 60% de dentistas tiene score > 7" no "los dentistas son buenos prospectos"
