---
name: "Onboarding"
---

# Proyecto: Onboarding de Nuevos Clientes

Responsable de activar a cada nuevo cliente de Humanio después de que Hotmart confirma el pago. El objetivo es que el cliente reciba su web publicada, su chatbot configurado y su primer mensaje de bienvenida — todo en menos de 72 horas hábiles.

## Criterios de aceptación

Un cliente está correctamente onboarded cuando:

- [ ] La propuesta web ya publicada en Surge.sh sigue activa y funcional
- [ ] El cliente recibió el email de bienvenida con sus credenciales y URLs
- [ ] El chatbot de WhatsApp está configurado y responde al número del cliente
- [ ] El cliente respondió o confirmó recepción (por email o WhatsApp)
- [ ] El ticket de onboarding está cerrado en Paperclip con nota de entrega

## Pipeline por paquete

### Starter ($27/mes)
1. CEO recibe notificación de pago de Hotmart
2. CEO crea ticket de Onboarding con datos del cliente (nombre, giro, ciudad, email, teléfono)
3. WebDesigner verifica que la propuesta web ya exista (`humanio-{slug}.surge.sh`) — si no existe, la crea con prioridad alta
4. CEO envía email de bienvenida al cliente:
   - URL de su sitio web
   - Instrucciones para actualizar contenido (si aplica)
   - Contacto de soporte: contacto@humanio.digital
5. CEO marca ticket como cerrado

### Pro ($47/mes)
Igual que Starter, más:
4b. Closer configura el chatbot básico de WhatsApp (respuestas a preguntas frecuentes del negocio)
4c. CEO envía mensaje de WhatsApp al cliente para confirmar que el chatbot responde

### Business ($97/mes)
Igual que Pro, más:
4d. Closer configura el flujo de agendamiento automático (integración con agenda del negocio)
4e. CEO hace llamada de 15 min con el cliente para confirmar que el sistema de citas funciona

## Datos requeridos para cada onboarding

```
nombre_negocio: ""
giro: ""
ciudad: ""
pais: ""
paquete: "Starter | Pro | Business"
email_cliente: ""
whatsapp_cliente: ""
hotmart_order_id: ""
surge_slug: ""          # humanio-{slug}
surge_url: ""           # https://humanio-{slug}.surge.sh
fecha_pago: ""
fecha_limite_entrega: "" # fecha_pago + 72h hábiles
```

## Reglas

- El CEO es el dueño del proyecto de Onboarding — coordina a WebDesigner y Closer cuando se necesitan
- Si el slug de Surge.sh ya existe desde la prospección, se reutiliza — no se crea un sitio nuevo
- Si el cliente pagó pero no existe propuesta web previa (cliente inbound directo), crear sitio de emergencia en 24h
- Notificar al CEO inmediatamente si no hay respuesta del cliente en 48h post-entrega
- DataAnalyst monitorea el estado de onboarding en el reporte semanal: clientes activados vs pendientes
