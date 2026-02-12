---
name: dashboard-fix
description: Fix Mission Control dashboard to work without GitHub token (uses repo data directly)
---

# Mission Control - Dashboard Corregido

## 🎯 Corrección Implementada:

El dashboard ahora funciona **sin requerir token de GitHub**. Usa directamente los datos del archivo `data/tasks.json` que está en el repo.

---

## 📋 Cambios:

1. **Eliminado requerimiento de token** - Ya no pide GitHub token
2. **Usa datos del repo** - Lee directamente de `data/tasks.json`
3. **Caché funcional** - Los datos se actualizan cuando subes cambios
4. **Error eliminado** - Ya no muestra "Could not load from GitHub, using cached data"

---

## 🚀 Cómo Usar:

### Visualizar Tareas:
1. Abre el dashboard: https://workspace-liart-theta.vercel.app
2. Verás todas las tareas en el Kanban board
3. Arrastra tareas entre columnas para cambiar estado

### Crear/Editar Tareas:
**Opción A: Desde el Dashboard**
1. Click en botón "+" en cualquier columna
2. Llena el formulario
3. **IMPORTANTE**: Los cambios NO se guardan automáticamente
4. Debes hacer commit manualmente en GitHub

**Opción B: Desde GitHub (Recomendado)**
1. Ve a: https://github.com/openclavv2/openclav-workspace
2. Abre `data/tasks.json`
3. Edita directamente
4. Haz commit y push

### Progreso del Agente:
El agente actualiza el progreso automáticamente:
- Subtasks marcadas como completadas
- Comentarios agregados
- Status cambiado cuando termina

---

## 🤖 Ver Progreso:

**Dashboard**: Ver las tareas en el Kanban board
**Script**: Ejecutar para ver progreso detallado
```bash
bash /home/opc/.openclaw/workspace/scripts/agent-progress.sh
```

**Archivo de Estado**: Ver estado JSON
```bash
cat /home/opc/.openclaw/workspace/data/agent-status.json
```

---

## 📊 Filtrado:

El dashboard tiene botones de proyecto en la parte superior:
- Click para filtrar por proyecto
- Verás solo las tareas de ese proyecto

---

**El error está corregido. Ya no necesitas GitHub token para usar el dashboard.** ✅
