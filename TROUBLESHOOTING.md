# TROUBLESHOOTING — Humanio Agent Company

Referencia rápida para diagnosticar y resolver problemas conocidos del sistema.

---

## 1. Agente no despierta / no procesa tickets

**Síntoma:** Se creó un ticket y el agente asignado no lo procesó en > 1 hora.

**Diagnóstico:**
- Verificar que el ticket tiene el agente asignado correctamente (no solo en comentario)
- Verificar que el CEO envió un mensaje directo al agente después de crear el ticket
- Revisar si hay un heartbeat schedule activo para ese agente

**Solución:**
1. CEO reenviará un mensaje directo al agente: `"Tienes un ticket pendiente: {TICKET_ID}. Procésalo ahora."`
2. Si el agente sigue sin responder, el CEO crea un sub-ticket de escalación

---

## 2. Email no llega al prospecto (Chatwoot bug)

**Síntoma:** Se envió email vía Chatwoot, el sistema reportó éxito con `message_id`, pero el prospecto no recibió nada.

**Causa:** Bug conocido en Chatwoot v4.11 — `undefined method 'message_id' for nil` en el pipeline de email. El `message_id` en la respuesta de la API es un falso positivo.

**Solución:**
- SIEMPRE usar SMTP directo (nodemailer) para enviar emails — NUNCA la API de Chatwoot
- Config SMTP: `smtpout.secureserver.net`, puerto 465, SSL, auth `contacto@humanio.digital`
- Después del envío SMTP, registrar una nota privada en Chatwoot (solo como CRM)
- Si ya se "envió" via Chatwoot y no llegó: reenviar inmediatamente via SMTP

---

## 3. Surge.sh falla con error 409

**Síntoma:** El deploy de WebDesigner falla con `error 409: domain already exists`.

**Causa:** El slug `humanio-{nombre}.surge.sh` ya está registrado por un deploy anterior.

**Solución:**
```bash
# Agregar sufijo numérico al slug
DOMAIN="humanio-${SLUG}-2.surge.sh"
SURGE_TOKEN=$SURGE_TOKEN surge /tmp/proposal-$SLUG $DOMAIN
```
Si también falla con -2, usar -3, -4, etc.

---

## 4. Firecrawl rate limited o bloqueado

**Síntoma:** `firecrawl_scrape` devuelve error 429, 403, o respuesta vacía.

**Solución:**
1. Esperar 30 segundos y reintentar con `firecrawl_search` en lugar de `firecrawl_scrape`
2. Si persiste, usar el skill `lucasvibecoder/gtme-skills/web-scraping` como fallback:
   - Sigue el Pre-Scrape Analysis Gate del skill antes de extraer
   - Respeta robots.txt y rate limits (mínimo 1s entre requests)
3. Si el sitio está protegido por Cloudflare, documentar como "No analizable" en el reporte y estimar con datos de redes sociales y Google Business

---

## 5. Chatwoot no devuelve conversaciones

**Síntoma:** El Closer consulta la API de Chatwoot y recibe lista vacía o error.

**Diagnóstico:**
```bash
# Verificar conectividad
curl -s "https://n8n-humanio-chatwoot.yroec7.easypanel.host/api/v1/accounts/1/conversations?inbox_id=2&status=open" \
  -H "api_access_token: $CHATWOOT_API_TOKEN" | python3 -c "import json,sys; d=json.load(sys.stdin); print(d.get('error','OK'))"
```

**Posibles causas y soluciones:**
- Token inválido → verificar `$CHATWOOT_API_TOKEN` en variables de entorno del agente
- `inbox_id` incorrecto → el inbox de email es el 2; verificar en Chatwoot Settings > Inboxes
- `account_id` incorrecto → usar `accounts/1` (cuenta principal)
- Servidor caído → esperar 5 min y reintentar; escalar al CEO si persiste > 15 min

---

## 6. Referencia de skill no resuelta

**Síntoma:** El agente reporta que no puede cargar un skill o que el skill "no existe".

**Causa probable:** Inconsistencia entre el nombre del skill en `AGENTS.md` y el `name` en el `SKILL.md`.

**Diagnóstico:**
- Verificar que el slug en `AGENTS.md` coincide con el `slug:` en el frontmatter del `SKILL.md`
- Verificar que la ruta del skill es correcta:
  - Skills propios: `company/HUM/{skill-name}`
  - Skills de GTM Agents: `gtmagents/gtm-agents/{skill-name}`
  - Skills de lucasvibecoder: `lucasvibecoder/gtme-skills/{skill-name}`
  - Skills de anthropics: `anthropics/skills/{skill-name}`

**Solución:** Corregir la referencia en `AGENTS.md` del agente afectado.

---

## 7. Paquetes Hotmart no coinciden con los esperados

**Síntoma:** DataAnalyst reporta un revenue incorrecto o un paquete que no existe.

**Pricing correcto (no modificar):**
| Paquete | Precio USD | ID Hotmart |
|---------|-----------|------------|
| Starter | $27/mes | (verificar en panel Hotmart) |
| Pro | $47/mes | (verificar en panel Hotmart) |
| Business | $97/mes | (verificar en panel Hotmart) |

**Comisión Hotmart:** 9.9% + $0.50 por transacción

**Net revenue por paquete:**
- Starter neto: ~$23.77/mes
- Pro neto: ~$41.87/mes
- Business neto: ~$86.97/mes

---

## 8. Agente envía mensajes en inglés

**Síntoma:** Outreach o Closer envió un email o WhatsApp en inglés al prospecto.

**Causa:** El agente usó un skill externo (anthropics, microsoft) sin respetar la instrucción de idioma.

**Solución:**
- Todos los materiales al prospecto deben ser en español
- Los skills externos se usan solo como referencia técnica, no para generar texto final
- Si ya se envió en inglés: el agente envía un segundo mensaje en español disculpándose y repitiendo el contenido
