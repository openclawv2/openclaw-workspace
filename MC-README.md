# Mission Control - Dashboard de Gestión de Tareas 🎛️

## 📱 Acceso al Dashboard

**URL pública:** https://openclawv2.github.io/openclaw-workspace/

Este es el dashboard web donde puedes crear, editar y gestionar tareas en estilo Kanban.

---

## 🎯 Estados de Tareas

| Estado | Significado |
|--------|-------------|
| **Permanent** | Tareas recurrentes (checks diarios, mantenimiento) |
| **Backlog** | Esperando ser trabajadas |
| **In Progress** | El agente AI está trabajando en esta tarea |
| **Review** | Terminada, espera tu aprobación |
| **Done** | Completada y aprobada |

---

## 📝 Cómo Funciona

### Flujo de Trabajo

1. **Tú editas** `data/tasks.json` en el repo de GitHub (o directamente en el dashboard)
2. **Haces commit & push** al repo
3. **Cada 5 minutos** el servidor verifica cambios
4. **Si hay tareas nuevas en "In Progress"**, el agente empieza a trabajar automáticamente
5. **El agente actualiza** el status cuando termina

### Edición Manual del Repositorio

```bash
# Clone el repo (o clona el fork)
git clone https://github.com/openclawv2/openclaw-workspace.git
cd openclaw-workspace

# Edita tasks.json
nano data/tasks.json

# Commit y push
git add data/tasks.json
git commit -m "Add task: ..."
git push origin master
```

---

## 🤖 Uso del Agente

### Crear una Tarea

Edita `data/tasks.json` y agrega:

```json
{
  "tasks": [
    {
      "id": "task_001",
      "title": "Ejemplo de tarea",
      "description": "Qué necesita hacer el agente",
      "status": "backlog",
      "subtasks": [
        { "id": "sub_001", "title": "Investigar", "done": false },
        { "id": "sub_002", "title": "Implementar", "done": false }
      ],
      "priority": "high",
      "dod": "Qué significa completado",
      "comments": []
    }
  ]
}
```

### Prioridad del Agente

Para que el agente empiece a trabajar en una tarea:

1. Cambia el `status` a `"in_progress"`
2. Haz commit y push al repo
3. Espera 1-5 minutos (cron job se ejecuta cada 5 min)
4. El agente recibirá notificación y empezará a trabajar

### Comentarios en Tareas

Puedes agregar comentarios en `comments` array para dar feedback al agente:

```json
"comments": [
  {
    "author": "Usuario",
    "text": "Por favor agrega más detalles aquí",
    "timestamp": "2026-02-11T18:00:00Z"
  }
]
```

---

## 🔧 Scripts Disponibles

### `scripts/mc-check.sh`

Script de polling que verifica cambios en el repo cada 5 minutos (vía cron).

**Ejecución manual:**
```bash
bash /home/opc/.openclaw/workspace/scripts/mc-check.sh
```

**Logs:**
```bash
cat /tmp/mc-check.log
```

---

## 📊 Estructura de Tareas

### Tarea Simple

```json
{
  "id": "task_001",
  "title": "Título",
  "description": "Descripción detallada",
  "status": "backlog",
  "priority": "high|medium|low",
  "dod": "Definition of Done",
  "subtasks": [],
  "comments": []
}
```

### EPIC (Tarea Parent)

Un EPIC contiene múltiples child tickets que se ejecutan secuencialmente:

```json
{
  "id": "epic_001",
  "title": "EPIC: Construir feature completa",
  "tags": ["epic"],
  "status": "backlog",
  "subtasks": [
    { "title": "task_001: Diseño de UI", "done": false },
    { "title": "task_002: Backend API", "done": false },
    { "title": "task_003: Testing", "done": false }
  ]
}
```

Cuando un EPIC pasa a "In Progress", el agente:
1. Ejecuta los child tickets en orden (1 → 2 → 3)
2. Después de cada child: comenta resultado, lo mueve a "review"
3. Después del último: mueve el EPIC a "review"

---

## 🎛️ Configuración

Configuración guardada en: `~/.clawdbot/mission-control.json`

```json
{
  "gateway": {
    "url": "http://127.0.0.1:18789",
    "hookToken": "..."
  },
  "workspace": {
    "path": "/home/opc/.openclaw/workspace",
    "tasksFile": "data/tasks.json"
  },
  "agent": {
    "sessionPrefix": "hook:mission-control"
  }
}
```

---

## 🔒 Seguridad

- El dashboard es público en GitHub Pages (cualquiera puede ver)
- Los tokens de autenticación están en el servidor (no en el repo)
- El webhook usa token del Gateway para comunicación segura
- Recomiendo mantener el repo privado si quieres ocultar tareas

---

## 🚀 Próximos Pasos

Para un setup completo con webhooks automáticos:

1. Instalar Tailscale: `curl -fsSL https://tailscale.com/install.sh | sh`
2. Configurar Funnel: `tailscale funnel 18789`
3. Crear webhook en GitHub apuntando a la URL de Tailscale

Por ahora, el **cron job de polling** funciona bien.

---

## 📞 Soporte

- **Dashboard:** https://openclawv2.github.io/openclaw-workspace/
- **Repositorio:** https://github.com/openclawv2/openclaw-workspace
- **Logs del servidor:** `/tmp/mc-check.log`
