

# Openclaw-config

> por [ara.so](https://ara.so) — 1.5K+ instalaciones en [skills.sh](https://skills.sh/adisinghstudent/ara.so/openclaw-config)

Habilidad para agentes de IA para administrar, depurar y operar [OpenClaw](https://github.com/Aradotso/zeroclaw): el entorno de ejecución de agente de IA de código abierto con más de 30 proveedores de LLM y 14 canales de mensajería.

Instálalo y tu agente de programación sabrá instantáneamente cómo diagnosticar problemas, buscar sesiones, editar la configuración y resolver fallos en cada canal.

## Instalación

```bash
npx skills add adisinghstudent/ara.so
```

Compatible con Claude Code, Cursor, Codex, Windsurf, Cline, GitHub Copilot y [más de 30 otros agentes](https://skills.sh/adisinghstudent/ara.so/openclaw-config).

## Estructura del Repositorio

```
.
├── README.md
└── skills/
    └── openclaw-config/
        └── SKILL.md          # 850 líneas de conocimiento operativo
```

## Contenido de la Habilidad

### Diagnósticos

- **Verificación rápida de estado** — comando instantáneo que verifica la puerta de enlace, el JSON de configuración, canales, plugins, credenciales, cron, errores recientes y la base de datos de memoria
- **Patrones de errores conocidos** — 12 errores documentados con significado exacto y solución

### Mapa de Archivos

Referencia completa para todo en `~/.openclaw/`:

```
~/.openclaw/
├── openclaw.json                    # Configuración principal: canales, autenticación, puerta de enlace, plugins
├── agents/main/
│   ├── agent/auth-profiles.json     # Tokens de autenticación LLM
│   └── sessions/
│       ├── sessions.json            # Índice de sesiones
│       └── *.jsonl                  # Transcripciones de sesiones
├── workspace/                       # Espacio de trabajo del agente
│   ├── SOUL.md                      # Personalidad y tono
│   ├── IDENTITY.md                  # Nombre y marca
│   ├── USER.md                      # Contexto del propietario
│   ├── AGENTS.md                    # Reglas de operación
│   ├── BOOT.md                      # Instrucciones de inicio
│   ├── HEARTBEAT.md                 # Lista de verificación de tareas periódicas
│   ├── MEMORY.md                    # Memoria curada a largo plazo
│   ├── TOOLS.md                     # Contactos, hosts SSH
│   ├── memory/                      # Registros diarios
│   └── skills/                      # Habilidades a nivel de espacio de trabajo
├── memory/main.sqlite               # DB de memoria vectorial (incrustaciones de Gemini)
├── logs/
│   ├── gateway.log                  # Eventos de ejecución
│   └── gateway.err.log              # Errores
├── cron/
│   ├── jobs.json                    # Definiciones de trabajos
│   └── runs/                        # Registros de ejecución por trabajo
├── credentials/
│   ├── whatsapp/default/            # Sesión de Baileys (~1400 archivos)
│   ├── telegram/{bot}/token.txt     # Tokens de bot
│   └── bird/cookies.json            # Autenticación de X/Twitter
├── extensions/{name}/               # Plugins personalizados (TypeScript)
├── browser/openclaw/user-data/      # Perfil de Chromium
└── tools/signal-cli/                # Binario de Signal CLI
```

### Resolución de problemas de canales

Guías de resolución detalladas para cada canal:

| Canal | Problemas cubiertos |
|---------|---------------|
| **WhatsApp** | Sin respuesta a mensajes, tiempos de espera 408, bloqueos entre contextos, búsqueda de sesión, política de lista permitida/grupo, congestión de carril, desconexión total, eliminación de credenciales |
| **Telegram** | Errores de validación de configuración (`botToken` vs `token`), tiempos de espera de sondeo, desplazamiento atascado, bot "olvidando" (compacción), plantilla de configuración correcta |
| **Signal** | Fallos RPC, estado del proceso signal-cli, limitación de tasa, formato de destino incorrecto, spam de nombre de perfil, reinicio de daemon |
| **Cron** | Vista general del estado del trabajo, detalles de fallos, análisis de registro de ejecución, causas comunes de fallo, próximos horarios programados, deshabilitación de trabajos rotos |

### Sistema de Memoria

Arquitectura de memoria de tres capas:

1. **Ventana de contexto** — en sesión, poda por compacción
2. **Archivos del espacio de trabajo** — `MEMORY.md` + registros diarios en `memory/`
3. **Base de datos vectorial** — SQLite + incrustaciones de Gemini con búsqueda FTS5

Incluye comandos para inspeccionar cada capa, verificar límites de tasa de incrustaciones y reconstruir el índice.

### Búsqueda de Sesiones

- Buscar conversaciones por persona, canal o fecha
- Buscar contenido de mensajes en todas las sesiones
- Leer transcripciones de sesiones específicas con salida formateada
- Entender el formato JSONL (eventos de sesión, mensaje, compacción y cambio de modelo)

### Edición de Configuración

Patrones de edición seguros usando `jq`:
- Cambiar políticas de canal (open, allowlist, pairing, disabled)
- Habilitar modo piloto automático
- Cambiar modelo LLM
- Establecer límites de concurrencia
- Habilitar/deshabilitar plugins
- Copia de seguridad y restauración

### Modos de Seguridad

| Modo | Comportamiento | Riesgo |
|------|----------|------|
| `open` + `allowFrom: ["*"]` | Cualquiera puede enviar mensajes, el bot responde a todos | ALTO |
| `allowlist` + `allowFrom: ["+1..."]` | Solo números en la lista | BAJO |
| `pairing` | Los remitentes desconocidos reciben un código de aprobación | BAJO |
| `disabled` | Canal desactivado | NINGUNO |

### Extender OpenClaw

- **Habilidades** — paquetes de conocimiento en markdown vía ClawdHub o `npx skills add`
- **Extensiones** — plugins de canal personalizados en TypeScript
- **Trabajos Cron** — tareas autónomas programadas
- **Multi-agente** — generar Codex/Claude Code/Pi como trabajadores en segundo plano
- **Multicanal** — recibir en WhatsApp, notificar en Signal
- **Canvas** — enviar HTML/tableros a dispositivos conectados
- **Llamadas de voz** — integración con Twilio/Telnyx/Plivo

## Ejemplos de Uso

Pregúntale a tu agente cualquiera de estas:

```
Why is my WhatsApp channel not connecting?
Show me the last 10 sessions on Telegram
Search all sessions for "deployment"
Switch Signal to allowlist mode for just my number
Which cron jobs are failing and why?
How do I add a new Telegram bot?
```

La habilidad le proporciona al agente los comandos, archivos y soluciones exactos, sin adivinaciones.

## Enlaces

- [skills.sh](https://skills.sh/adisinghstudent/ara.so/openclaw-config)
- [OpenClaw (ZeroClaw)](https://github.com/Aradotso/zeroclaw)
- [ara.so](https://ara.so) — entornos de agentes de IA instantáneos en la nube
- [Habilidades Tendencia](https://github.com/Aradotso/trending-skills) — habilidades generadas automáticamente desde tendencias de GitHub

## Licencia

MIT
