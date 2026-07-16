# Case Study — Automatização com IA para a Clínica Patro

**Problema de negócio → solução técnica → impacto esperado.** Um sistema de
automatização de atendimento via WhatsApp (texto e voz) para uma clínica de
podologia em Madrid, combinando BI/análise de processos com engenharia de IA
aplicada.

## O problema

Sem automatização, a clínica perdia tempo e receita em três pontos:

- **Mensagens repetitivas** (horários, serviços, como marcar) consumiam horas
  por semana que podiam ir para atendimento a pacientes.
- **Resposta lenta** — um paciente sem resposta rápida procura outra clínica;
  a primeira resposta decide se a marcação se fecha ou se perde.
- **Faltas sem aviso** — entre 15% e 25% dos pacientes esquecem a consulta
  sem lembrete automático, e cada vaga vazia é receita não recuperada.

## A solução — 3 módulos interligados

### 1. Bot de WhatsApp (`clinica-patro-bot`)
Atende o paciente 24h, recolhe os dados da marcação (serviço, dia preferido,
contacto), propõe horários livres da agenda em tempo real e confirma a
consulta — sem intervenção manual.

- **Stack:** Next.js 16, TypeScript, OpenAI API (conversação), Supabase
  (base de dados de pacientes/marcações), `pdfjs-dist`/`pdf-parse` (o bot lê
  documentos internos da clínica — preços, serviços — para responder com
  informação sempre atualizada sem hardcode).
- **Decisão técnica:** manter o bot e o site institucional no mesmo stack
  (Next.js) simplifica deployment e partilha de componentes/estilos.

### 2. Recordatórios automáticos
Envia lembrete 24h e 2h antes de cada consulta, com opção de confirmar ou
cancelar. Uma cancelação aciona automaticamente o módulo seguinte.

### 3. Lista de espera inteligente (`clinica-patro-voz`)
Quando há uma vaga (cancelamento), o sistema pode contactar por **voz** o
próximo paciente da lista de espera — não só texto.

- **Stack:** Fastify + WebSocket (streaming de áudio em tempo real), Twilio
  (telefonia), OpenAI Realtime API (conversação por voz), deploy em
  Docker/Fly.io.
- **Porquê voz e não só texto:** parte dos pacientes (sobretudo idade mais
  avançada) responde melhor a uma chamada do que a uma mensagem — cobre um
  segmento que o bot de texto sozinho não alcançava.

## Impacto (estimado com base na análise de processo da clínica)

| Métrica | Antes | Com o sistema |
|---|---|---|
| Tempo de resposta ao paciente | Horas (dependia de disponibilidade manual) | Segundos, 24h/dia |
| Faltas sem aviso | 15–25% das consultas | ~20% menos faltas (com lembrete automático) |
| Vagas por cancelamento | Perdidas | Preenchidas automaticamente via lista de espera |
| Custo de infraestrutura | — | €0–5/mês (WhatsApp Cloud API + Supabase, planos gratuitos até ao volume da clínica) |

## O que isto demonstra (para além do bot em si)

- Capacidade de **traduzir um processo de negócio real** (perda de receita
  por faltas e resposta lenta) numa solução técnica com métricas de impacto
  claras — a mesma competência de BI aplicada a um produto, não só a um
  dashboard.
- **Integração de sistemas** (WhatsApp Cloud API, Supabase, OpenAI, Twilio)
  em produção, não só em ambiente de tutorial.
- **Análise de custo-benefício** — desenho da solução para custo operacional
  mínimo (€0–5/mês), relevante para PMEs, o segmento mais comum no mercado
  espanhol.

## Nota sobre este case study

O código-fonte destes projetos não está público (é uma aplicação em produção
com dados reais de pacientes). Este documento descreve arquitetura, decisões
técnicas e impacto de negócio — a mesma forma como se apresentaria o projeto
numa entrevista técnica.
