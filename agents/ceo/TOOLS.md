# Tools del CEO

## Paperclip API (coordinación de agentes)

Tu herramienta principal. Toda la delegación, seguimiento y comunicación pasa por aquí.

- **Crear tickets**: `POST /api/issues` con `parentId`, `goalId`, y asignación al agente correcto
- **Checkout**: `POST /api/issues/{id}/checkout` — SIEMPRE antes de trabajar un ticket. Nunca reintentar 409.
- **Comentar**: `POST /api/issues/{id}/comments` — Actualiza status, comparte URLs, escala decisiones
- **Mensajes directos**: Para despertar agentes (Scout→Qualifier, Qualifier→WebDesigner, etc.)
- **Header obligatorio**: Incluir `X-Paperclip-Run-Id` en toda llamada mutante

## Chatwoot (CRM y comunicación inbound)

Lectura de conversaciones con prospectos que nos contactan directamente.

- **URL**: `$CHATWOOT_API_URL` (definido en secrets de Paperclip, sin slash final)
- **Inbox de email**: ID 2 (`contacto@humanio.digital`)
- **Listar conversaciones**: `GET /api/v1/accounts/{ACCOUNT_ID}/conversations?inbox_id=2&status=open`
- **Leer mensajes**: `GET /api/v1/accounts/{ACCOUNT_ID}/conversations/{CONV_ID}/messages`
- **Uso**: Solo lectura para triage de inbound. NO enviar emails vía Chatwoot (bug v4.11). Delegar a Closer/Outreach.

## Sistema de Memoria PARA (`para-memory-files`)

Gestión de contexto persistente entre sesiones.

- **Memoria diaria**: `$AGENT_HOME/memory/YYYY-MM-DD.md` — Plan del día, tareas completadas, bloqueos
- **Hechos durables**: `$AGENT_HOME/life/` — Estructura PARA (Projects, Areas, Resources, Archive)
- **Extracción de hechos**: En cada heartbeat, revisa conversaciones nuevas y extrae hechos durables
- **Regla**: Siempre lee el plan del día al inicio. Siempre actualiza antes de salir.

## Google Drive (verificación de entregables)

No subes archivos directamente — eso lo hacen Outreach y Closer. Pero puedes verificar que los archivos estén donde deben estar.

- **Carpeta raíz**: `$GOOGLE_DRIVE_FOLDER_ID`
- **Estructura**: Una subcarpeta por prospecto con todos los materiales (propuesta, reporte, emails, WhatsApp)

## Hotmart (verificación de pagos)

Cuando Closer escala un cierre exitoso, verifica el pago en Hotmart antes de activar onboarding.

- **Dashboard**: Verificar suscripciones activas, pagos procesados, cancelaciones
- **Nota**: Integración directa pendiente — hoy se verifica manualmente

## Surge.sh (verificación de deploys)

Cuando WebDesigner notifica una URL publicada:
- Verificar que `https://humanio-{slug}.surge.sh` carga correctamente
- Verificar que `/propuesta` y `/reporte` son accesibles
- Compartir la URL al Board como comentario en el ticket original
