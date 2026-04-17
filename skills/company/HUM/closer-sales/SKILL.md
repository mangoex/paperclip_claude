---
name: closer-sales
slug: closer-sales
---

# Closer Sales — Ejecución de Seguimiento Comercial | Humanio

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
chatwoot_conversation_id: {CHATWOOT_CONV_ID}       # ID de conversación en Chatwoot — para responder en mismo hilo
```

### 2. Verificar si el prospecto respondió (DECISIÓN DE CAMINO)

Antes de calcular fechas, consulta Chatwoot para saber si el prospecto ya respondió:

```javascript
const CONV_ID = '{CHATWOOT_CONVERSATION_ID}';
const CW_URL  = process.env.CHATWOOT_API_URL;
const CW_TOKEN = process.env.CHATWOOT_API_TOKEN;
const ACCOUNT_ID = process.env.CHATWOOT_ACCOUNT_ID;

let prospectReplied = false;
let prospectMessages = [];

if (CONV_ID && CONV_ID !== 'null') {
  const r = await fetch(
    `${CW_URL}/api/v1/accounts/${ACCOUNT_ID}/conversations/${CONV_ID}/messages`,
    { headers: { 'api_access_token': CW_TOKEN } }
  );
  const d = await r.json();
  // message_type 0 = incoming (del prospecto), excluyendo mensajes privados
  prospectMessages = (d.payload || []).filter(m => m.message_type === 0 && !m.private);
  prospectReplied = prospectMessages.length > 0;
}

console.log(`Prospecto respondió: ${prospectReplied} (${prospectMessages.length} mensajes entrantes)`);
```

**→ Si `prospectReplied === true`: ir al CAMINO B (Cierre Conversacional)**
**→ Si `prospectReplied === false`: ir al CAMINO A (Seguimientos Programados)**

---

## 🔵 CAMINO B — El prospecto respondió: Cierre Conversacional

> Ejecuta este camino cuando el prospecto haya enviado al menos un mensaje en la conversación.
> **No envíes follow-ups 2 ni 3.** En su lugar, responde con enfoque de cierre de ventas.

### B1. Leer y clasificar la respuesta

Lee el último mensaje del prospecto y clasifícalo:

| Clasificación | Señales | Acción |
|--------------|---------|--------|
| `INTERES_POSITIVO` | "me interesa", "cuánto cuesta", "cuándo podemos hablar", "me llamó la atención" | Proponer llamada de 15 min |
| `PREGUNTA_TECNICA` | Pregunta sobre cómo funciona SEO, IA, el servicio | Responder con dato del Qualifier + proponer llamada |
| `OBJECION_PRECIO` | "es caro", "no tengo presupuesto", "cuánto cuesta exactamente" | Manejar objeción + proponer llamada |
| `OBJECION_TIEMPO` | "estoy muy ocupado", "ahorita no", "en unos meses" | Manejar objeción + proponer llamada |
| `OBJECION_PROVEEDOR` | "ya tenemos alguien", "ya trabajamos con una agencia" | Posicionar como complementario + proponer llamada |
| `RECHAZO` | "no gracias", "no me interesa", "no en este momento" | Cerrar con dignidad, no insistir |

### B2. Generar respuesta de cierre

Redacta la respuesta según la clasificación. **Siempre en primera persona como Miguel González.** **Siempre con objetivo de agendar llamada de 15 minutos.**

**Template INTERES_POSITIVO:**
```
Hola {NOMBRE_CONTACTO},

Me da gusto que te haya llamado la atención — precisamente por eso quería platicarte los puntos clave del diagnóstico de forma directa.

Tengo listo el análisis completo con oportunidades concretas para {NOMBRE_NEGOCIO}. Son cosas muy específicas que podemos revisar en una llamada corta de 15 minutos.

¿Te funciona alguno de estos horarios esta semana?
• Martes o miércoles por la mañana
• Jueves por la tarde

Me ajusto a tu agenda. También podemos hacerlo por videollamada si te resulta más cómodo.

Quedo atento.

Miguel González | Humanio
```

**Template PREGUNTA_TECNICA:**
```
Hola {NOMBRE_CONTACTO},

Buena pregunta — {RESPUESTA_ESPECIFICA_CON_DATO_DEL_QUALIFIER}.

Para que tenga más contexto y pueda mostrarte exactamente cómo aplicaría esto a {NOMBRE_NEGOCIO}, lo más práctico sería una llamada de 15 minutos donde puedo compartir pantalla y mostrarte el diagnóstico completo.

¿Tienes disponibilidad esta semana?

Miguel González | Humanio
```

**Template OBJECION_PRECIO:**
```
Hola {NOMBRE_CONTACTO},

Entiendo la pregunta sobre la inversión — es lo más razonable antes de tomar una decisión.

Lo que quiero mostrarte en el diagnóstico es precisamente eso: qué impacto concreto tendría en {NOMBRE_NEGOCIO} y si los números tienen sentido para tu negocio. Hay opciones para empezar pequeño y escalar.

¿Tienes 15 minutos esta semana para que te explique los puntos clave? Así puedes decidir con información real.

Miguel González | Humanio
```

**Template OBJECION_TIEMPO:**
```
Hola {NOMBRE_CONTACTO},

Lo entiendo perfectamente — el día a día de {NOMBRE_NEGOCIO} siempre es lo primero.

Precisamente por eso trabajamos con un modelo donde nosotros hacemos todo — tú no necesitas aprender ni gestionar nada técnico. La llamada de diagnóstico son solo 15 minutos y la hago yo en el horario que mejor te funcione.

¿Hay algún momento esta semana, aunque sea breve?

Miguel González | Humanio
```

**Template OBJECION_PROVEEDOR:**
```
Hola {NOMBRE_CONTACTO},

Con gusto. Lo que hacemos en Humanio es diferente a una agencia tradicional — integramos IA para automatizar procesos del negocio que generan ventas directas.

El diagnóstico que preparé para {NOMBRE_NEGOCIO} va más allá del SEO — incluye oportunidades de automatización que complementan lo que ya tienen.

¿Le damos 15 minutos para que lo veas? No cuesta nada evaluar.

Miguel González | Humanio
```

**Template RECHAZO:**
```
Hola {NOMBRE_CONTACTO},

Con todo gusto, lo entiendo perfectamente. Si en algún momento el contexto cambia o quieren explorar algo relacionado con IA o presencia digital, aquí estamos.

Le deseo mucho éxito a {NOMBRE_NEGOCIO}.

Miguel González | Humanio
```

### B3. Enviar respuesta vía SMTP

```javascript
const nodemailer = require('nodemailer');
const fs = require('fs');

// Obtener source_id del último mensaje entrante para In-Reply-To
const lastIncoming = prospectMessages[prospectMessages.length - 1];
const inReplyTo = lastIncoming?.source_id || null;

const responseHTML = `<!DOCTYPE html>
<html><head><meta charset="UTF-8">
<style>
  body{font-family:'Inter',Arial,sans-serif;margin:0;padding:20px;background:#f4f4f4}
  .container{max-width:600px;margin:0 auto;background:#fff;border-radius:8px;padding:32px 36px}
  p{font-size:15px;color:#1a1a2e;line-height:1.7;margin:0 0 16px}
  .signature{margin-top:24px;padding-top:16px;border-top:1px solid #f0f0f0}
  .signature p{font-size:13px;color:#94a3b8;margin:0;line-height:1.6}
  .signature strong{color:#374151}
</style></head><body>
<div class="container">
  {CONTENIDO_RESPUESTA_HTML}
  <div class="signature">
    <p><strong>Miguel González</strong><br>
    Humanio — Inteligencia Artificial para negocios<br>
    contacto@humanio.digital · humanio.digital<br>
    {TELEFONO_MIGUEL_DISPLAY}</p>
  </div>
</div></body></html>`;

const transporter = nodemailer.createTransport({
  host: 'smtpout.secureserver.net', port: 465, secure: true,
  auth: { user: process.env.SMTP_USER || 'contacto@humanio.digital', pass: process.env.SMTP_PASS }
});

const mailOpts = {
  from: '"Miguel González | Humanio" <contacto@humanio.digital>',
  to: '{EMAIL_PROSPECTO}',
  subject: 'Re: Análisis digital de {NOMBRE_NEGOCIO}',
  html: responseHTML
};
if (inReplyTo) { mailOpts.inReplyTo = inReplyTo; mailOpts.references = inReplyTo; }

const info = await transporter.sendMail(mailOpts);
console.log(`✅ Respuesta de cierre enviada vía SMTP — ${info.messageId}`);

// Nota privada en Chatwoot
await fetch(`${CW_URL}/api/v1/accounts/${ACCOUNT_ID}/conversations/${CONV_ID}/messages`, {
  method: 'POST',
  headers: { 'api_access_token': CW_TOKEN, 'Content-Type': 'application/json' },
  body: JSON.stringify({
    content: `📧 Respuesta de cierre enviada vía SMTP (${'{CLASIFICACION}'})\nPara: {EMAIL_PROSPECTO}\nMessageId: ${info.messageId}`,
    private: true
  })
});
```

### B4. Acciones post-respuesta según clasificación

- **INTERES_POSITIVO / PREGUNTA_TECNICA / OBJECION (cualquiera):**
  - Crear ticket para CEO: *"Prospecto {NOMBRE_NEGOCIO} respondió con {CLASIFICACION} — en espera de confirmación de llamada"*
  - Dejar ticket del Closer en status `in_progress`

- **RECHAZO:**
  - Actualizar ticket del Closer a `done` con nota: `CERRADO — prospecto rechazó`
  - No crear ticket para CEO

> **Una vez en Camino B, NO ejecutes nada del Camino A (pasos 3–9).**

---

## 🟠 CAMINO A — Sin respuesta: Seguimientos Programados

> Ejecuta este camino SOLO cuando `prospectReplied === false`.

### 3. Calcular timing

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

### 4. Generar todos los archivos (día 3 — generación anticipada completa)

> **Principio clave:** Genera TODOS los archivos el día que te activas — msg 2, msg 3, log.
> Súbelos a Drive ANTES de enviar cualquier cosa.
> Así Miguel puede revisar el plan completo antes de que salga ningún mensaje.
> El msg 2 se envía hoy. El msg 3 se guarda para envío en día 7 (solo si no hay respuesta).

```bash
mkdir -p /tmp/closer-{slug}
```

#### 4a. Mensaje 2 — WhatsApp

Sigue el framework VALOR del skill `sales-copywriting` → "WHATSAPP — Mensaje 2":
- Abre con nombre del contacto (no con "Hola 👋" ni "Soy Miguel")
- Aporta UN dato nuevo que no estaba en el mensaje 1
- Semilla de IA opcional solo si fluye naturalmente con el dato
- Pregunta abierta de micro-compromiso
- Máximo 8 líneas

```bash
WA_MSG2="{NOMBRE_CONTACTO}, soy Miguel de Humanio.

Te escribí hace unos días sobre {NOMBRE_NEGOCIO}.
{DATO_NUEVO_DE_VALOR}

¿Tuviste chance de ver la propuesta?
{PROPUESTA_URL}

Si tienes alguna duda, aquí estoy.
— Miguel"

echo "$WA_MSG2" > /tmp/closer-{slug}/seguimiento-2-whatsapp.txt
echo "✅ seguimiento-2-whatsapp.txt generado"
```

#### 4b. Mensaje 2 — Email (vía Chatwoot, mismo hilo del mensaje 1)

> El email de seguimiento se envía como reply en la conversación de Chatwoot creada por Outreach.
> Usa el `chatwoot_conversation_id` del ticket. Si no existe, guarda el HTML para envío manual.

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
  <p>Si tienes alguna pregunta, me puedes escribir por aquí o por WhatsApp.</p>
  <p>Saludos,<br>Miguel</p>
  <div class="signature">
    <p><strong>Miguel González</strong><br>
    Humanio &mdash; Inteligencia Artificial para negocios<br>
    contacto@humanio.digital &middot; humanio.digital<br>
    {TELEFONO_MIGUEL_DISPLAY}</p>
  </div>
</div>
</body>
</html>`;

fs.mkdirSync('/tmp/closer-{slug}', {recursive: true});
fs.writeFileSync('/tmp/closer-{slug}/seguimiento-2-email.html', email2HTML);

// ⚠️ REGLA CRÍTICA: Enviar SIEMPRE vía SMTP directo — NO usar Chatwoot API para envío de emails.
// Chatwoot v4.11 tiene un bug 'undefined method message_id for nil' al enviar emails salientes
// cuando la conversación fue iniciada vía SMTP externo (no vía Chatwoot).
// Chatwoot se usa SOLO como CRM (notas privadas de registro).

const CHATWOOT_URL   = process.env.CHATWOOT_API_URL;
const CHATWOOT_TOKEN = process.env.CHATWOOT_API_TOKEN;
const ACCOUNT_ID     = process.env.CHATWOOT_ACCOUNT_ID;
const CONV_ID        = '{CHATWOOT_CONVERSATION_ID}';

// Obtener source_id del último mensaje entrante para In-Reply-To
let inReplyTo = null;
if (CONV_ID && CONV_ID !== 'null' && CONV_ID !== '') {
  try {
    const msgsResp = await fetch(
      `${CHATWOOT_URL}/api/v1/accounts/${ACCOUNT_ID}/conversations/${CONV_ID}/messages`,
      { headers: { 'api_access_token': CHATWOOT_TOKEN } }
    );
    const msgsData = await msgsResp.json();
    const incoming = (msgsData.payload || []).filter(m => m.message_type === 0 && m.source_id);
    if (incoming.length > 0) inReplyTo = incoming[incoming.length - 1].source_id;
  } catch(e) { console.log('No se pudo obtener source_id:', e.message); }
}

// Enviar vía SMTP directo (igual que Outreach)
const nodemailer = require('nodemailer');
const transporter = nodemailer.createTransport({
  host: 'smtpout.secureserver.net', port: 465, secure: true,
  auth: { user: process.env.SMTP_USER || 'contacto@humanio.digital', pass: process.env.SMTP_PASS }
});

let email2Status = 'ERROR_SMTP';
let email2MsgId  = null;

try {
  const mailOpts = {
    from: '"Miguel González | Humanio" <contacto@humanio.digital>',
    to: '{EMAIL_PROSPECTO}',
    subject: 'Re: Análisis digital de {NOMBRE_NEGOCIO}',
    html: email2HTML
  };
  if (inReplyTo) { mailOpts.inReplyTo = inReplyTo; mailOpts.references = inReplyTo; }
  const info = await transporter.sendMail(mailOpts);
  email2MsgId  = info.messageId;
  email2Status = 'ENVIADO_VIA_SMTP';
  console.log(`✅ Email msg 2 enviado vía SMTP — messageId: ${email2MsgId}`);
} catch(err) {
  console.error('⚠️ Error SMTP msg 2:', err.message);
  email2Status = 'ERROR_SMTP — ' + err.message;
}

// Registrar en Chatwoot como nota privada (CRM)
if (CONV_ID && CONV_ID !== 'null' && CONV_ID !== '') {
  try {
    await fetch(`${CHATWOOT_URL}/api/v1/accounts/${ACCOUNT_ID}/conversations/${CONV_ID}/messages`, {
      method: 'POST',
      headers: { 'api_access_token': CHATWOOT_TOKEN, 'Content-Type': 'application/json' },
      body: JSON.stringify({ content: `📧 Follow-up email enviado vía SMTP (${email2Status})\nMessageId: ${email2MsgId}`, private: true })
    });
  } catch(e) { console.log('⚠️ No se pudo registrar nota en Chatwoot:', e.message); }
}

fs.writeFileSync('/tmp/closer-{slug}/seguimiento-2-meta.json', JSON.stringify({
  chatwoot_conversation_id: CONV_ID,
  chatwoot_message_id: email2MsgId,
  subject: 'Re: Análisis digital de {NOMBRE_NEGOCIO}',
  status: email2Status,
  scheduledFor: '{FECHA_MSG2}',
  createdAt: new Date().toISOString()
}, null, 2));
console.log('✅ seguimiento-2-email.html generado');
```

#### 4c. Mensaje 3 — WhatsApp (generado ahora, enviado en día 7)

Sigue el template "WHATSAPP — Mensaje 3" del skill `sales-copywriting`:
- Declarar que es el último mensaje (elimina presión)
- No introducir temas nuevos
- Dejar puerta abierta con calidez
- Máximo 5 líneas

```bash
WA_MSG3="{NOMBRE_CONTACTO}, este es mi último mensaje sobre el tema.

Entiendo que el día a día de {NOMBRE_NEGOCIO} es lo primero.

El análisis sigue disponible cuando quieras:
{PROPUESTA_URL}

Éxito con todo.
— Miguel"

echo "$WA_MSG3" > /tmp/closer-{slug}/seguimiento-3-whatsapp.txt
echo "✅ seguimiento-3-whatsapp.txt generado (programado para día 7 si no hay respuesta)"
```

#### 4d. Mensaje 3 — Email (generado ahora, enviado en día 7 vía Chatwoot)

> El HTML se genera ahora. El envío ocurre en el Paso 8 (día 7) solo si no hay respuesta.
> Usa el mismo `chatwoot_conversation_id` para mantener el hilo completo en Chatwoot.

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
  <p>No quiero ser insistente &mdash; sé que el día a día de {NOMBRE_NEGOCIO} es lo primero.</p>
  <p>Tu análisis y propuesta siguen disponibles aquí: <a href="{PROPUESTA_URL}">{PROPUESTA_URL}</a></p>
  <p>Si en algún momento te interesa explorarlo, me encuentras en este correo o por WhatsApp ({TELEFONO_MIGUEL_DISPLAY}).</p>
  <p>Te deseo mucho éxito.</p>
  <p>Miguel González<br>Humanio &mdash; Inteligencia Artificial para negocios</p>
</div>
</body>
</html>`;

fs.writeFileSync('/tmp/closer-{slug}/seguimiento-3-email.html', email3HTML);
fs.writeFileSync('/tmp/closer-{slug}/seguimiento-3-meta.json', JSON.stringify({
  chatwoot_conversation_id: '{CHATWOOT_CONVERSATION_ID}',
  subject: 'Re: Análisis digital de {NOMBRE_NEGOCIO}',
  status: 'PENDIENTE_DIA_7',
  scheduledFor: '{FECHA_MSG3}',
  note: 'Enviar solo si no hubo respuesta al mensaje 2 — ver Paso 8',
  createdAt: new Date().toISOString()
}, null, 2));
console.log('✅ seguimiento-3-email.html generado (se enviará en día 7 si no hay respuesta)');
```

### 5. Generar closer-log.txt (estado inicial)

```bash
cat > /tmp/closer-{slug}/closer-log.txt << 'EOF'
═══════════════════════════════════════════════════════════
REGISTRO DE SEGUIMIENTO — {NOMBRE_NEGOCIO}
═══════════════════════════════════════════════════════════

Prospecto : {NOMBRE_NEGOCIO}
Contacto  : {NOMBRE_CONTACTO}
Giro      : {GIRO} | {CIUDAD}
Score     : {SCORE}/10
Propuesta : {PROPUESTA_URL}

SECUENCIA PLANIFICADA:
──────────────────────
[{FECHA_MSG1}] MSG 1 — Outreach
  Canal    : {CANAL_MSG1}
  Hallazgo : {HALLAZGO_PRINCIPAL_MSG1}
  Status   : Enviado

[{FECHA_MSG2}] MSG 2 — Closer (hoy)
  Canal    : Email (draft) + WhatsApp
  Dato nuevo: {DATO_NUEVO_DE_VALOR}
  Status   : PENDIENTE ENVÍO

[{FECHA_MSG3}] MSG 3 — Closer (día 7)
  Canal    : Email (draft) + WhatsApp
  Status   : PROGRAMADO — solo si no hay respuesta al msg 2

RESPUESTAS DEL PROSPECTO:
──────────────────────────
(sin respuestas aún)

ESTADO: EN_SEGUIMIENTO
═══════════════════════════════════════════════════════════
EOF
echo "✅ closer-log.txt generado"
```

### 6. Subir TODOS los archivos a Drive (antes de enviar)

```bash
# IMPORTANTE: pasar NOMBRE_NEGOCIO como env var evita inyección de shell.
export NOMBRE_NEGOCIO="{NOMBRE_NEGOCIO}"

node /paperclip/upload-to-drive.js "$NOMBRE_NEGOCIO" \
  "/tmp/closer-{slug}/seguimiento-2-whatsapp.txt" \
  "/tmp/closer-{slug}/seguimiento-2-email.html" \
  "/tmp/closer-{slug}/seguimiento-3-whatsapp.txt" \
  "/tmp/closer-{slug}/seguimiento-3-email.html" \
  "/tmp/closer-{slug}/closer-log.txt"

if [ $? -ne 0 ]; then
  node /app/upload-to-drive.js "$NOMBRE_NEGOCIO" \
    "/tmp/closer-{slug}/seguimiento-2-whatsapp.txt" \
    "/tmp/closer-{slug}/seguimiento-2-email.html" \
    "/tmp/closer-{slug}/seguimiento-3-whatsapp.txt" \
    "/tmp/closer-{slug}/seguimiento-3-email.html" \
    "/tmp/closer-{slug}/closer-log.txt"
fi
echo "✅ Todos los archivos subidos a Drive"
```

### 7. Enviar mensaje 2 por WhatsApp

```bash
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
      \"body\": $(cat /tmp/closer-{slug}/seguimiento-2-whatsapp.txt | python3 -c 'import json,sys; print(json.dumps(sys.stdin.read()))')
    }
  }")

WA_MSG_ID=$(echo "$WA_RESPONSE" | python3 -c "import json,sys; d=json.load(sys.stdin); print(d['messages'][0]['id'])" 2>/dev/null)

if [ -n "$WA_MSG_ID" ]; then
  echo "✅ WhatsApp msg 2 enviado — ID: $WA_MSG_ID"
  WA2_STATUS="ENVIADO — ID: $WA_MSG_ID"
else
  echo "⚠️ WhatsApp msg 2 no enviado — PENDIENTE ENVÍO MANUAL"
  WA2_STATUS="PENDIENTE ENVÍO MANUAL"
fi
```

### 8. Envío de mensaje 3 (día 7) — SOLO si sin respuesta

> ⚠️ CONDICIÓN OBLIGATORIA: Solo ejecutar si `prospectReplied === false`.
> Si el prospecto respondió en cualquier momento (aunque sea con un "no gracias"),
> NO envíes el mensaje 3. Ve al CAMINO B para manejar esa respuesta.

**Envío de mensaje 3 (día 7):**
```bash
HOY=$(date +%Y-%m-%d)
if [ "$HOY" >= "$FECHA_MSG3" ] && [ "$STATUS_RESPUESTA" = "sin_respuesta" ]; then

  # ── Mensaje 3 WhatsApp (mismo curl del paso 7) ──────────────────────────
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
        \"body\": $(cat /tmp/closer-{slug}/seguimiento-3-whatsapp.txt | python3 -c 'import json,sys; print(json.dumps(sys.stdin.read()))')
      }
    }")

  WA3_MSG_ID=$(echo "$WA_RESPONSE" | python3 -c "import json,sys; d=json.load(sys.stdin); print(d['messages'][0]['id'])" 2>/dev/null)
  if [ -n "$WA3_MSG_ID" ]; then
    echo "✅ WhatsApp msg 3 enviado — ID: $WA3_MSG_ID"
    WA3_STATUS="ENVIADO — ID: $WA3_MSG_ID"
  else
    echo "⚠️ WhatsApp msg 3 no enviado — PENDIENTE ENVÍO MANUAL"
    WA3_STATUS="PENDIENTE ENVÍO MANUAL"
  fi

  # ── Mensaje 3 Email (vía SMTP directo — NO Chatwoot API) ───────────────
  node -e "
const fs = require('fs');
const nodemailer = require('nodemailer');
const email3HTML = fs.readFileSync('/tmp/closer-{slug}/seguimiento-3-email.html', 'utf8');
const CONV_ID = '{CHATWOOT_CONVERSATION_ID}';
const CW_URL = process.env.CHATWOOT_API_URL;
const CW_TOKEN = process.env.CHATWOOT_API_TOKEN;
const ACCOUNT_ID = process.env.CHATWOOT_ACCOUNT_ID;

(async () => {
  // Obtener source_id del último mensaje entrante para In-Reply-To
  let inReplyTo = null;
  if (CONV_ID && CONV_ID !== 'null') {
    try {
      const r = await fetch(\`\${CW_URL}/api/v1/accounts/\${ACCOUNT_ID}/conversations/\${CONV_ID}/messages\`,
        { headers: { 'api_access_token': CW_TOKEN } });
      const d = await r.json();
      const incoming = (d.payload || []).filter(m => m.message_type === 0 && m.source_id);
      if (incoming.length > 0) inReplyTo = incoming[incoming.length - 1].source_id;
    } catch(e) {}
  }

  const transporter = nodemailer.createTransport({
    host: 'smtpout.secureserver.net', port: 465, secure: true,
    auth: { user: process.env.SMTP_USER || 'contacto@humanio.digital', pass: process.env.SMTP_PASS }
  });
  const mailOpts = {
    from: '\"Miguel González | Humanio\" <contacto@humanio.digital>',
    to: '{EMAIL_PROSPECTO}',
    subject: 'Re: Análisis digital de {NOMBRE_NEGOCIO}',
    html: email3HTML
  };
  if (inReplyTo) { mailOpts.inReplyTo = inReplyTo; mailOpts.references = inReplyTo; }

  try {
    const info = await transporter.sendMail(mailOpts);
    console.log('✅ Email msg 3 enviado vía SMTP — messageId: ' + info.messageId);
    // Nota privada en Chatwoot
    if (CONV_ID && CONV_ID !== 'null') {
      await fetch(\`\${CW_URL}/api/v1/accounts/\${ACCOUNT_ID}/conversations/\${CONV_ID}/messages\`,
        { method: 'POST', headers: { 'api_access_token': CW_TOKEN, 'Content-Type': 'application/json' },
          body: JSON.stringify({ content: '📧 Email msg 3 enviado vía SMTP — ' + info.messageId, private: true }) });
    }
  } catch(err) {
    console.error('⚠️ Error SMTP msg 3:', err.message);
  }
})();
"

  echo "✅ Mensaje 3 completo (WA + Email)"
else
  WA3_STATUS="NO APLICA — prospecto respondió o aún no es día 7"
fi
```

### 9. Notificar al CEO

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
