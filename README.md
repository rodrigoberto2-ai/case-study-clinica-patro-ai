# Case Study — Automatización con IA para la Clínica Patro

**Problema de negocio → solución técnica → impacto esperado.** Un sistema de
automatización de atención vía WhatsApp (texto y voz) para una clínica de
podología en Madrid, combinando BI/análisis de procesos con ingeniería de IA
aplicada.

## El problema

Sin automatización, la clínica perdía tiempo e ingresos en tres puntos:

- **Mensajes repetitivos** (horarios, servicios, cómo reservar) consumían horas
  a la semana que podían dedicarse a la atención de pacientes.
- **Respuesta lenta** — un paciente sin respuesta rápida busca otra clínica;
  la primera respuesta decide si la cita se cierra o se pierde.
- **Faltas sin aviso** — entre el 15% y el 25% de los pacientes olvidan la cita
  sin un recordatorio automático, y cada hueco vacío es ingreso no recuperado.

## La solución — 3 módulos interconectados

### 1. Bot de WhatsApp (`clinica-patro-bot`)
Atiende al paciente 24h, recoge los datos de la reserva (servicio, día
preferido, contacto), propone horarios libres de la agenda en tiempo real y
confirma la cita, sin intervención manual.

- **Stack:** Next.js 16, TypeScript, OpenAI API (conversación), Supabase
  (base de datos de pacientes/reservas), `pdfjs-dist`/`pdf-parse` (el bot lee
  documentos internos de la clínica —precios, servicios— para responder con
  información siempre actualizada sin hardcode).
- **Decisión técnica:** mantener el bot y el sitio institucional en el mismo
  stack (Next.js) simplifica el despliegue y la reutilización de componentes/estilos.

### 2. Recordatorios automáticos
Envía un recordatorio 24h y 2h antes de cada cita, con opción de confirmar o
cancelar. Una cancelación activa automáticamente el módulo siguiente.

### 3. Lista de espera inteligente (`clinica-patro-voz`)
Cuando se libera un hueco (cancelación), el sistema puede contactar por **voz**
al siguiente paciente de la lista de espera, no solo por texto.

- **Stack:** Fastify + WebSocket (streaming de audio en tiempo real), Twilio
  (telefonía), OpenAI Realtime API (conversación por voz), despliegue en
  Docker/Fly.io.
- **Por qué voz y no solo texto:** parte de los pacientes (sobre todo de más
  edad) responde mejor a una llamada que a un mensaje, cubriendo un segmento
  al que el bot de texto por sí solo no llegaba.

## Impacto (estimado a partir del análisis del proceso de la clínica)

| Métrica | Antes | Con el sistema |
|---|---|---|
| Tiempo de respuesta al paciente | Horas (dependía de disponibilidad manual) | Segundos, 24h/día |
| Faltas sin aviso | 15–25% de las citas | ~20% menos faltas (con recordatorio automático) |
| Huecos por cancelación | Perdidos | Cubiertos automáticamente vía lista de espera |
| Coste de infraestructura | — | €0–5/mes (WhatsApp Cloud API + Supabase, planes gratuitos hasta el volumen de la clínica) |

## Qué demuestra esto (más allá del bot en sí)

- Capacidad de **traducir un proceso de negocio real** (pérdida de ingresos
  por faltas y respuesta lenta) en una solución técnica con métricas de
  impacto claras — la misma competencia de BI aplicada a un producto, no solo
  a un dashboard.
- **Integración de sistemas** (WhatsApp Cloud API, Supabase, OpenAI, Twilio)
  en producción, no solo en un entorno de tutorial.
- **Análisis coste-beneficio** — diseño de la solución para un coste operativo
  mínimo (€0–5/mes), relevante para pymes, el segmento más común del mercado
  español.

## Nota sobre este case study

El código fuente de estos proyectos no es público (es una aplicación en
producción con datos reales de pacientes). Este documento describe
arquitectura, decisiones técnicas e impacto de negocio, tal como se
presentaría el proyecto en una entrevista técnica.
