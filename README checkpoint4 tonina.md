# Checkpoint 4 — TONINA | Integraciones Avanzadas

Workflow de n8n que integra correo, CRM, IA y canal interno con controles preventivos antes de ejecutar acciones externas.

## Flujo

Gmail Trigger → Anti Auto-Reply → Limpieza/validación → Agente IA → HubSpot Lookup → Crear/actualizar contacto → Gmail Draft (HITL) → Payload mínimo → Slack #operaciones → Salida OK.

## Controles implementados

- **Anti Auto-Reply:** bloquea respuestas automáticas y deriva a `STOP - Correo Automatico Ignorado`.
- **Validación de remitente:** detiene el flujo si no existe un correo válido.
- **CRM controlado:** ejecuta `HubSpot - Look Up Contacto` antes de crear o actualizar.
- **Human-in-the-loop:** Gmail crea un borrador; el workflow no envía correos automáticamente.
- **Minimización de datos:** Slack recibe únicamente el payload mínimo necesario.
- **Salida explícita:** el workflow termina con `status = success` y declara los controles aplicados.

## Pruebas realizadas

- Correo normal: el control Anti Auto-Reply permitió continuar el flujo.
- Correo con asunto `Auto-reply prueba checkpoint`: fue bloqueado y enviado a `STOP - Correo Automatico Ignorado`.
- HubSpot sin contacto previo: creó el contacto.
- HubSpot con el mismo contacto: lo encontró y ejecutó la rama de actualización.
- Gmail: creó un borrador en el hilo, sin envío automático.
- Slack: publicó correctamente en `#operaciones`.
- Salida final: `status = success`.

## Credenciales necesarias después de importar

El archivo público no contiene secretos ni IDs internos de credenciales. Después de importar, seleccionar en n8n:

1. Gmail OAuth2
2. HubSpot OAuth2
3. Slack API (Bot Token)
4. OpenAI API

Para Slack, la app usada en las pruebas tuvo los scopes de bot:

- `channels:read`
- `chat:write`
- `chat:write.public`

## Seguridad

La versión pública fue saneada antes de compartir:

- sin access tokens;
- sin refresh tokens;
- sin Client Secrets;
- sin API keys;
- sin IDs internos de credenciales;
- sin `instanceId` de n8n;
- sin `webhookId` específicos de la instancia.

## Archivo de entrega

`checkpoint4_mauricio_sandoval.json`

Importar el archivo en n8n, seleccionar las cuatro credenciales y realizar una prueba antes de activarlo.
