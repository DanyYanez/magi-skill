---
name: magi
description: Lanza 3 sub-agentes en paralelo (Melchior, Balthasar, Casper) inspirados en el sistema MAGI de Evangelion. Cada uno revisa código/PRs/features desde 3 enfoques distintos y devuelve hallazgos clasificados por severidad. Claude Code sintetiza los 3 reportes en uno solo y el usuario decide qué corregir. Úsalo cuando el usuario invoque /magi, pida un "code review profundo", quiera análisis multi-perspectiva, o diga "lanzar los magi" sobre un archivo, diff, PR o feature.
---

# MAGI — Sistema de revisión multi-perspectiva

Inspirado en los tres supercomputadoras MAGI de NERV en Neon Genesis Evangelion (Melchior, Balthasar, Casper). Cada agente representa una faceta de análisis diferente. Los tres trabajan **en paralelo, sin contexto previo de la conversación**, y entregan hallazgos clasificados por severidad. Tú (Claude Code) sintetizas los reportes y los presentas al usuario en un único informe consolidado.

---

## Cuándo usar

Invoca este skill cuando:
- El usuario escribe `/magi` o "lanza los magi" sobre código, un archivo, un diff o un PR
- Pide un "code review profundo", "análisis multi-perspectiva" o "revisión completa"
- Quiere validar arquitectura, seguridad, performance y UX a la vez
- Necesita una segunda (tercera, cuarta) opinión antes de mergear o desplegar

NO uses este skill para:
- Preguntas simples de código (responde directo)
- Bugs específicos con causa obvia (debug normal)
- Tareas de implementación (no es review)

---

## Idioma

**Detecta el idioma de la conversación** y úsalo en TODA la salida (prompts a los agentes, reporte final, encabezados). Si la conversación está en español, todo en español. Si está en inglés, todo en inglés. Si hay duda, pregunta una vez.

---

## Los 3 agentes

### 🧠 Melchior — el científico (análisis racional)
Enfoques:
- **Lógica & Correctness** — bugs, edge cases, condiciones mal manejadas, lógica rota
- **Arquitectura & Diseño de sistema** — capas, separación de responsabilidades, patrones
- **Acoplamiento & SOLID** — dependencias, cohesión, principios de diseño

Lee: `agents/melchior.md`

### 🛡️ Balthasar — la madre (protección)
Enfoques:
- **Seguridad & Vulnerabilidades** — inyecciones, autenticación, datos sensibles, OWASP
- **Testing & Edge cases** — cobertura, casos límite, escenarios de falla
- **Best practices & Convenciones** — estándares del lenguaje/framework, anti-patterns

Lee: `agents/balthasar.md`

### ✨ Casper — la mujer (intuición/experiencia)
Enfoques:
- **Performance & Escalabilidad** — complejidad, queries, recursos, cuellos de botella
- **UX & Diseño visual** — usabilidad, accesibilidad, jerarquía visual (si aplica)
- **Mantenibilidad & Legibilidad (DX)** — claridad, deuda técnica, experiencia del próximo dev

Lee: `agents/casper.md`

---

## Flujo de ejecución

### Paso 1 — Identificar el target
Pregunta al usuario qué revisar si no es obvio:
- Archivo(s) específico(s)
- Diff/PR (`git diff`, `git diff main`, número de PR)
- Feature completa (varios archivos relacionados)
- Carpeta o módulo

Si el target es un PR remoto, usa `gh pr diff <num>` para obtener el diff.

### Paso 2 — Lanzar los 3 agentes EN PARALELO
Usa la herramienta Task (subagent_type: general-purpose) **en una sola llamada** con 3 invocaciones simultáneas. Cada una:

1. Lee el contenido de su archivo de rol (`agents/melchior.md`, `agents/balthasar.md`, `agents/casper.md`)
2. Recibe el target (paths absolutos de archivos, diff completo en el prompt, o instrucción de leer)
3. Devuelve un reporte estructurado en el formato definido abajo

**Crítico:** los agentes son AGNÓSTICOS. No comparten contexto entre sí ni con la conversación principal. Pásales solo:
- Su archivo de rol
- El código/diff a revisar (paths o contenido)
- El idioma de salida

### Paso 3 — Sintetizar
Cuando los 3 reportes regresen, NO los muestres tal cual. Genera UN solo reporte consolidado con esta estructura:

```
# 🔮 Reporte MAGI

**Target:** <archivo/PR/feature>
**Idioma:** <es/en>

## Resumen ejecutivo
<2-3 líneas: qué tan crítico es el estado, cuántos hallazgos por severidad>

## Hallazgos por severidad

### 🔴 Críticos (N)
1. **[Melchior/Balthasar/Casper]** <título> — <archivo:línea>
   <descripción corta>
   _Sugerencia:_ <qué hacer>

### 🟡 Medios (N)
...

### 🟢 Menores (N)
...

## Vista por agente
- 🧠 **Melchior:** <1 línea con su veredicto general>
- 🛡️ **Balthasar:** <1 línea>
- ✨ **Casper:** <1 línea>

## Próximo paso
<Pregunta al usuario qué quiere abordar primero. NO empieces a corregir.>
```

### Paso 4 — Esperar decisión del usuario
Después del reporte, **PARA**. No hagas cambios automáticos. Pregunta cuál hallazgo abordar primero. El usuario decide.

---

## Reglas no negociables

1. **Siempre los 3 agentes en paralelo.** Nunca secuencial.
2. **Agentes sin contexto previo.** No les pases historia de la conversación.
3. **Severidad obligatoria** en cada hallazgo: 🔴 crítico / 🟡 medio / 🟢 menor.
4. **Un solo reporte final.** No vuelques los 3 reportes crudos.
5. **NO corrijas nada automáticamente.** El usuario decide qué tocar.
6. **Idioma consistente** en toda la salida según la conversación.
7. **Cita archivo:línea** en cada hallazgo cuando sea posible.
