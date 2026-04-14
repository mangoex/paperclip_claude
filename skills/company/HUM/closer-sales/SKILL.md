---
name: closer-sales
slug: closer-sales
---

# Closer Sales — Ejecución de Seguimiento Comercial | Humanio Marketing

## Identidad

Eres el motor operativo del agente Closer. Este skill define CÓMO ejecutas cada paso del seguimiento: composición de mensajes, envío por API, registro de interacciones, y lógica de decisión.

Para el TONO y CONTENIDO de los mensajes, consulta siempre el skill `sales-copywriting`.

---

## Proceso completo

### 1. Leer el ticket de seguimiento

Del ticket extrae:

```yaml
nombre_negocio: "{NOMBRE_NEGOCIO}"
nombre_contacto: "{NOMBRE_CONTACTO}"
giro: "{GIRO}"
ciudad: "{CIUDAD}"
score: {SCORE}
telefono: "{TELEFONO_PROSPECTO}"        # formato E.164: 5216671234567
email: "{EMAIL_PROSPECTO}"
propuesta_url: "https://humanio-{slug}.surge.sh"
reporte_url: "https://humanio-{slug}.surge.sh/reporte"
fecha_mensaje_1: "{FECHA_MSG1}"          # YYYY-MM-DD
canal_mensaje_1: "email|whatsapp|ambos"
status_respuesta: "sin_respuesta|leido|respondio_positivo|respondio_negativo|respondio_pregunta|respondio_objecion"
diagnostico_resumen: "{RESUMEN_QUALIFIER}"
keyword_principal: "{KEYWORD}"
busquedas_mes: "{BUSQUEDAS_MES}"
precio_propuesto: "{PRECIO_TOTAL}"
hallazgos_originales:
  - "{HALLAZGO_1}"
  - "{HALLAZGO_2}"
  - "{HALLAZGO_3}"
hallazgo_usado_msg1: "{HALLAZGO_PRINCIPAL_MSG1}"  # para no repetir
```

### 2. Calcular timing

```bash
FECHA_MSG1="{FECHA_MSG1}"
FECHA_MSG2=$(date -d "$FECHA_MSG1 + 3 days" +%Y-%m-%d)
FECHA_MSG3=$(date -d "$FECHA_MSG1 + 7 days" +%Y-%m-%d)
HOY=$(date +%Y-%m-%d)

if [ "$HOY" \< "$FECHA_MSG2" ]; then
  echo "⏳ Aún no es momento del mensaje 2. Esperar hasta $FECHA_MSG2"
  exit 0
fi
```

### 3. Investigar dato nuevo

Antes de redactar el mensaje 2, busca información fresca. Usa firecrawl o búsqueda web:

```bash
# Opción 1: Buscar competidores actualizados
SEARCH_QUERY="{KEYWORD} {CIUDAD}"

# Opción 2: Verificar posición actual del prospecto
SEARCH_QUERY="{NOMBRE_NEGOCIO} {CIUDAD}"

# Opción 3: Dato del sector
SEARCH_QUERY="{GIRO} tendencias México 2026"
```

Componer el `{DATO_NUEVO_DE_VALOR}` con lo que encuentres. Ejemplos reales:

- "Vi que {COMPETIDOR} en {CIUDAD} ya tiene sitio web optimizado y está apareciendo en las primeras posiciones para '{KEYWORD}'"
- "Las búsquedas de '{KEYWORD}' en {CIUDAD} crecieron este mes — cada vez más personas buscan tu servicio por Google"
- "Noté que uno de tus competidores cercanos ya activó WhatsApp Business y está capturando consultas directas"

Si no encuentras nada nuevo, usa un hallazgo del diagnóstico original que NO se haya usado en el mensaje 1:
```
DATO_NUEVO = hallazgos_originales[i] donde i != hallazgo_usado_msg1
```

### 4. Componer mensaje 2 — WhatsApp

Aplica el template del skill `sales-copywriting` → sección "WHATSAPP — Mensaje 2".

```bash
# Composición
WA_MSG2="Hola {NOMBRE_CONTACTO}, soy Miguel de Humanio 👋

Te escribí hace unos días sobre {NOMBRE_NEGOCIO}.

{DATO_NUEVO_DE_VALOR}

¿Tuviste chance de ver la propuesta? Si tienes alguna duda, con gusto te la resuelvo por aquí.

— Miguel"

# Guardar
mkdir -p /tmp/closer-{slug}
echo "$WA_MSG2" > /tmp/closer-{slug}/seguimiento-2-whatsapp.txt

# Enviar vía WhatsApp Business Cloud API
WA_RESPONSE=$(curl -s -X POST \
  "https://graph.facebook.com/v18.0/$WHATSAPP_PHONE_NUMBER_ID/messages" \
  -H "Authorization: Bearer $WHATSAPP_CLOUD_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"messaging_product\": \"whatsapp\",
    \"to\": \"{TELEFONO_PROSPECTO_E164}\",
    \"type\": \"text\",
    \"text\": {
      \"preview_url\": true,
      \"body\": $(echo "$WA_MSG2" | python3 -c 'import json,sys; print(json.dumps(sys.stdin.read()))')
    }
  }")

WA_MSG_ID=$(echo "$WA_RESPONSE" | python3 -c "import json,sys; d=json.load(sys.stdin); print(d['messages'][0]['id'])" 2>/dev/null)

if [ -n "$WA_MSG_ID" ]; then
  echo "✅ WhatsApp msg 2 enviado — ID: $WA_MSG_ID"
  WA2_STATUS="ENVIADO — ID: $WA_MSG_ID"
else
  echo "⚠️ WhatsApp msg 2 falló"
  WA2_STATUS="PENDIENTE ENVÍO MANUAL"
fi
```

### 5. Componer mensaje 2 — Email (draft)

```javascript
const fs = require('fs');

const email2HTML = `<!DOCTYPE html>
<html>
<head><meta charset="UTF-8">
<style>
  body{font-family:'Inter',Arial,sans-serif;margin:0;padding:20px;background:#f4f4f4}
  .container{max-width:600px;margin:0 auto;background:#fff;border-radius:8px;padding:32px 36px}
  p{font-size:15px;color:#1a1a2e;line-height:1.7;margin:0 0 16px}
  a{color:#2dd4bf;text-decoration:none;font-weight:500}
  .signature{margin-top:24px;padding-top:16px;border-top:1px solid #f0f0f0}
  .signature p{font-size:13px;color:#94a3b8;margin:0;line-height:1.6}
  .signature strong{color:#374151}
</style>
</head>
<body>
<div class="container">
  <p>Hola {NOMBRE_CONTACTO},</p>
  <p>Quería saber si tuviste chance de revisar el análisis que te envié.</p>
  <p>{DATO_NUEVO_DE_VALOR}</p>
  <p>La propuesta sigue disponible aquí: <a href="{PROPUESTA_URL}">{PROPUESTA_URL}</a></p>
  <p>Si tienes alguna pregunta o quieres que te explique algo en particular, me puedes escribir por aquí o por WhatsApp.</p>
  <p>Saludos,<br>Miguel</p>
  <div class="signature">
    <p><strong>Miguel González</strong> · Humanio Marketing<br>
    contacto@humanio.digital · humanio.digital<br>
    WhatsApp: {TELEFONO_MIGUEL_DISPLAY}</p>
  </div>
</div>
</body>
</html>`;

fs.mkdirSync('/tmp/closer-{slug}', {recursive: true});
fs.writeFileSync('/tmp/closer-{slug}/seguimiento-2-email.html', email2HTML);
fs.writeFileSync('/tmp/closer-{slug}/seguimiento-2-meta.json', JSON.stringify({
  to: '{EMAIL_PROSPECTO}',
  subject: 'Re: Análisis digital de {NOMBRE_NEGOCIO}',
  from: 'contacto@humanio.digital',
  fromName: 'Miguel González | Humanio Marketing',
  inReplyTo: '{MESSAGE_ID_MSG1}',  // ID del email original para threading
  status: 'DRAFT_PENDING_REVIEW',
  createdAt: new Date().toISOString()
}, null, 2));
```

### 6. Monitorear respuestas (día 3-7)

Si el prospecto responde por WhatsApp entre el mensaje 2 y el 3:

1. Leer el mensaje entrante (via webhook de WhatsApp Business API o revisión manual)
2. Clasificar la intención:
   - `PRECIO` → responder con referencia a propuesta web
   - `INTERES` → escalar a CEO para agendar llamada
   - `RECHAZO` → cerrar con gracia, no enviar mensaje 3
   - `PREGUNTA_TECNICA` → responder con datos del diagnóstico del Qualifier
   - `OBJECION` → manejar según guía del skill sales-copywriting
3. Registrar en `closer-log.txt`

### 7. Componer mensaje 3 — WhatsApp (día 7, solo si no hay respuesta)

```bash
WA_MSG3="Hola {NOMBRE_CONTACTO} 👋

Este es mi último mensaje sobre el tema. Entiendo que hay mil cosas en el día a día de un negocio.

Solo quería que supieras que el análisis de {NOMBRE_NEGOCIO} sigue disponible: {PROPUESTA_URL}

Si en algún momento quieres retomarlo, me encuentras aquí. Éxito con todo 🙌

— Miguel"

echo "$WA_MSG3" > /tmp/closer-{slug}/seguimiento-3-whatsapp.txt

# Enviar (mismo proceso de API que mensaje 2)
```

### 8. Componer mensaje 3 — Email (día 7)

```javascript
const email3HTML = `<!DOCTYPE html>
<html>
<head><meta charset="UTF-8">
<style>
  body{font-family:'Inter',Arial,sans-serif;margin:0;padding:20px;background:#f4f4f4}
  .container{max-width:600px;margin:0 auto;background:#fff;border-radius:8px;padding:32px 36px}
  p{font-size:15px;color:#1a1a2e;line-height:1.7;margin:0 0 16px}
  a{color:#2dd4bf;text-decoration:none;font-weight:500}
  .signature{margin-top:24px;padding-top:16px;border-top:1px solid #f0f0f0}
  .signature p{font-size:13px;color:#94a3b8;margin:0;line-height:1.6}
  .signature strong{color:#374151}
</style>
</head>
<body>
<div class="container">
  <p>Hola {NOMBRE_CONTACTO},</p>
  <p>No quiero ser insistente — sé que el día a día de {NOMBRE_NEGOCIO} es lo primero.</p>
  <p>Tu análisis y propuesta siguen disponibles aquí: <a href="{PROPUESTA_URL}">{PROPUESTA_URL}</a></p>
  <p>Si en algún momento te interesa explorarlo, me encuentras en este correo o en WhatsApp ({TELEFONO_MIGUEL_DISPLAY}).</p>
  <p>Te deseo mucho éxito.</p>
  <p>Miguel González<br>Humanio Marketing</p>
</div>
</body>
</html>`;

fs.writeFileSync('/tmp/closer-{slug}/seguimiento-3-email.html', email3HTML);
```

### 9. Generar closer-log.txt

```
═══════════════════════════════════════════════════════════
REGISTRO DE SEGUIMIENTO — {NOMBRE_NEGOCIO}
═══════════════════════════════════════════════════════════

Prospecto: {NOMBRE_NEGOCIO}
Contacto: {NOMBRE_CONTACTO}
Giro: {GIRO} | Ciudad: {CIUDAD}
Score: {SCORE}/10

SECUENCIA DE MENSAJES:
─────────────────────
[{FECHA_MSG1}] MSG 1 (Outreach)
  Canal: {CANAL_MSG1}
  Status: Enviado
  Hallazgo usado: {HALLAZGO_PRINCIPAL_MSG1}

[{FECHA_MSG2}] MSG 2 (Closer)
  Canal: Email (draft) + WhatsApp ({WA2_STATUS})
  Dato nuevo: {DATO_NUEVO_DE_VALOR}
  Status: {STATUS_MSG2}

[{FECHA_MSG3}] MSG 3 (Closer)
  Canal: Email (draft) + WhatsApp ({WA3_STATUS})
  Status: {STATUS_MSG3}

RESPUESTAS DEL PROSPECTO:
─────────────────────────
{REGISTRO_RESPUESTAS}

ESTADO FINAL: {ESTADO_FINAL}
ACCIÓN RECOMENDADA: {ACCION}

═══════════════════════════════════════════════════════════
```

### 10. Subir archivos a Drive

Sube los archivos nuevos a la MISMA carpeta del prospecto que creó Outreach:

```bash
# Los archivos de Outreach ya existen en la carpeta
# Agregar los del Closer:
node /paperclip/upload-to-drive.js "{NOMBRE_NEGOCIO}" \
  /tmp/closer-{slug}/seguimiento-2-email.html \
  /tmp/closer-{slug}/seguimiento-2-whatsapp.txt \
  /tmp/closer-{slug}/seguimiento-3-email.html \
  /tmp/closer-{slug}/seguimiento-3-whatsapp.txt \
  /tmp/closer-{slug}/closer-log.txt
```

### 11. Notificar al CEO

Reporta con el template definido en AGENTS.md del Closer.

---

## Reglas de calidad

- SIEMPRE consulta `sales-copywriting` antes de redactar cualquier mensaje
- NUNCA envíes mensaje 3 si el prospecto respondió (positiva o negativamente) al mensaje 2
- NUNCA envíes mensaje 2 si el prospecto ya respondió al mensaje 1
- NUNCA inventes datos para el "dato nuevo de valor" — debe ser real o del diagnóstico
- Los emails de seguimiento SIEMPRE van como draft (nunca auto-envío)
- Los WhatsApp se envían automáticamente solo si hay número disponible
- Si el API de WhatsApp falla, guardar como .txt y marcar como PENDIENTE ENVÍO MANUAL
- El `closer-log.txt` debe registrar TODA interacción — es el historial completo del prospecto
- Máximo 3 mensajes totales por prospecto (1 Outreach + 2 Closer) — NUNCA más
