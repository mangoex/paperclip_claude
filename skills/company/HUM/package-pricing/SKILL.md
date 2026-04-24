---
name: "package-pricing"
description: "Asignador de paquetes de suscripción Humanio. Analiza el perfil del prospecto y recomienda el paquete óptimo (Starter $27, Pro $47 o Business $97 USD/mes) según las necesidades detectadas."
slug: "package-pricing"
metadata:
  paperclip:
    slug: "package-pricing"
    skillKey: "company/HUM/package-pricing"
    paperclipSkillKey: "company/HUM/package-pricing"
  skillKey: "company/HUM/package-pricing"
  key: "company/HUM/package-pricing"
---

# Package Pricing — Asignador de Paquetes | Humanio

## Paquetes de suscripción

| Paquete | Precio USD | Incluye | Perfil ideal |
|---------|-----------|---------|--------------| 
| **Starter** | $27/mes | Web profesional + enlace WhatsApp + formulario contacto | Negocio sin presencia digital |
| **Pro** | $47/mes | Todo Starter + Chatbot WhatsApp con info del negocio | Negocio con web básica, necesita atención automatizada |
| **Business** | $97/mes | Todo Pro + Chatbot IA con agendamiento de citas | Negocio basado en citas y consultas |

## Reglas de asignación

### Starter ($27 USD/mes)
Asignar cuando:
- El negocio NO tiene página web
- Tiene web pero está caída, abandonada o es solo un perfil de Facebook
- No tiene presencia digital profesional
- Negocio pequeño que necesita empezar desde cero

### Pro ($47 USD/mes)
Asignar cuando:
- El negocio YA tiene web (básica o funcional)
- Recibe muchas preguntas repetitivas (horarios, precios, ubicación)
- No tiene WhatsApp Business activo o lo usa manualmente
- Vende productos o servicios que no requieren cita
- Restaurantes, tiendas, comercios en general

### Business ($97 USD/mes)
Asignar cuando:
- El negocio se basa en **citas y consultas**
- Giros típicos: dentistas, doctores, abogados, psicólogos, coaches, salones de belleza, veterinarias, consultores
- Necesita agenda automática
- El tiempo del profesional es el recurso más valioso

### No prioritario
Marcar cuando:
- El negocio ya tiene web profesional + chatbot activo + agenda online
- Score < 6 en la evaluación del Qualifier
- No se detecta necesidad clara de los servicios de Humanio

## Equivalencia en moneda local

Al presentar precios, siempre incluir equivalencia aproximada:

| Paquete | USD | MXN (~) | COP (~) | PEN (~) | ARS (~) |
|---------|-----|---------|---------|---------|---------| 
| Starter | $27 | $460 | $108,000 | S/100 | $24,300 |
| Pro | $47 | $800 | $188,000 | S/175 | $42,300 |
| Business | $97 | $1,650 | $388,000 | S/360 | $87,300 |

*Nota: Equivalencias aproximadas. Hotmart cobra en moneda local al tipo de cambio del día.*

## Pasarela de pago

- **Hotmart** — suscripción recurrente mensual
- Multi-país, multi-moneda nativo
- Comisión: 9.9% + $0.50 USD por transacción
- El prospecto recibe link de pago personalizado

## Formato de recomendación

Al incluir la recomendación en un ticket, usar este formato:

```
## Recomendación de paquete

**Paquete:** {Starter/Pro/Business}
**Precio:** ${precio} USD/mes (~{equivalencia} {moneda local})
**Razón:** {justificación en 1-2 líneas basada en hallazgos reales}
**Link Hotmart:** {link correspondiente al paquete}
```

## Upselling path

Siempre mencionar la ruta de crecimiento:
- Starter → Pro: "Cuando quiera automatizar la atención por WhatsApp"
- Pro → Business: "Cuando necesite agendar citas automáticamente"
- Business → Servicios premium: "Automatizaciones avanzadas, tiendas en línea, agentes de IA personalizados"
