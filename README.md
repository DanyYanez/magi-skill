<h1 align="center">🔮 MAGI Skill</h1>

<p align="center">
  <strong>Three parallel AI agents inspired by NERV's MAGI system from Neon Genesis Evangelion.</strong><br/>
  Code review from three independent perspectives — synthesized into a single report.
</p>

<p align="center">
  <a href="#english">🇬🇧 English</a> · <a href="#español">🇪🇸 Español</a>
</p>

---

## English

### What is this?

A Claude Code skill that launches **three parallel sub-agents** — Melchior, Balthasar, and Casper — each reviewing your code from a distinct perspective. Just like NERV's MAGI in Evangelion, the three deliberate independently and the main system synthesizes the verdict.

### The three MAGI

| MAGI | Persona | Focus areas |
|------|---------|-------------|
| 🧠 **Melchior** | The Scientist (rational mind) | Logic & correctness · Architecture & system design · Coupling & SOLID |
| 🛡️ **Balthasar** | The Mother (protector) | Security & vulnerabilities · Testing & edge cases · Best practices |
| ✨ **Casper** | The Woman (intuition) | Performance & scalability · UX & visual design · Maintainability (DX) |

Each agent runs **agnostic** — no prior context from your conversation. They report findings classified by severity (🔴 critical / 🟡 medium / 🟢 minor), and Claude Code synthesizes everything into a single consolidated report.

### Installation

```bash
git clone https://github.com/DanyYanez/magi-skill.git ~/.claude/skills/magi
```

That's it. Claude Code auto-loads skills from `~/.claude/skills/`.

### Usage

Inside Claude Code, just say:

```
/magi src/auth.ts
```

Or in plain language:

```
Launch the MAGI on this PR
```

```
Run a deep review on the checkout flow
```

### What you get

```
🔮 MAGI Report
Target: src/auth.ts

Executive summary
2 critical findings, 4 medium, 3 minor. Main risk: security.

🔴 Critical (2)
1. [Balthasar] Missing auth on /admin endpoint — src/auth.ts:42
2. [Melchior] Race condition in token refresh — src/auth.ts:118

🟡 Medium (4) ...
🟢 Minor (3) ...

By agent
🧠 Melchior: changes required (logic risk)
🛡️ Balthasar: changes required (security)
✨ Casper: minor changes (perf OK, slight DX issues)

Next step: which finding do you want to tackle first?
```

The MAGI **never** modify your code. They only report. You decide what to fix.

### Why three agents?

Because a single review pass misses things. A security expert won't notice slow queries. A performance engineer won't notice missing tests. By splitting the cognitive load across three specialized roles in parallel, you get coverage no single review can match.

> *"For a major decision, the three MAGI vote. Majority wins."*
> — NERV operations protocol, sort of.

### Credits

- Inspired by **Neon Genesis Evangelion** (Hideaki Anno / Gainax / Khara) — all rights to the MAGI concept, names and aesthetic belong to them.
- Built by [@DanyYanez](https://github.com/DanyYanez) for [Claude Code](https://claude.com/claude-code).

If this skill helps you, drop a ⭐ and fork it. Don't re-upload it as your own.

### License

MIT — see [LICENSE](LICENSE). You can use, modify and redistribute freely; just keep the copyright notice.

---

## Español

### ¿Qué es esto?

Un skill de Claude Code que lanza **tres sub-agentes en paralelo** — Melchior, Balthasar y Casper — cada uno revisando tu código desde una perspectiva distinta. Igual que el sistema MAGI de NERV en Evangelion, los tres deliberan por separado y el sistema principal sintetiza el veredicto.

### Los tres MAGI

| MAGI | Personalidad | Enfoques |
|------|--------------|----------|
| 🧠 **Melchior** | El científico (mente racional) | Lógica & correctness · Arquitectura & diseño · Acoplamiento & SOLID |
| 🛡️ **Balthasar** | La madre (protectora) | Seguridad & vulnerabilidades · Testing & edge cases · Best practices |
| ✨ **Casper** | La mujer (intuición) | Performance & escalabilidad · UX & diseño visual · Mantenibilidad (DX) |

Cada agente corre **agnóstico** — sin contexto previo de la conversación. Reportan hallazgos clasificados por severidad (🔴 crítico / 🟡 medio / 🟢 menor), y Claude Code sintetiza todo en un único reporte consolidado.

### Instalación

```bash
git clone https://github.com/DanyYanez/magi-skill.git ~/.claude/skills/magi
```

Listo. Claude Code carga automáticamente los skills de `~/.claude/skills/`.

### Uso

Dentro de Claude Code:

```
/magi src/auth.ts
```

O en lenguaje natural:

```
Lanza los MAGI sobre este PR
```

```
Hazme un review profundo del flujo de checkout
```

### Qué recibes

```
🔮 Reporte MAGI
Target: src/auth.ts

Resumen ejecutivo
2 hallazgos críticos, 4 medios, 3 menores. Riesgo principal: seguridad.

🔴 Críticos (2)
1. [Balthasar] Endpoint /admin sin auth — src/auth.ts:42
2. [Melchior] Race condition en refresh de token — src/auth.ts:118

🟡 Medios (4) ...
🟢 Menores (3) ...

Por agente
🧠 Melchior: cambios requeridos (riesgo lógico)
🛡️ Balthasar: cambios requeridos (seguridad)
✨ Casper: cambios menores (perf OK, ligeros temas de DX)

Próximo paso: ¿qué hallazgo quieres atacar primero?
```

Los MAGI **nunca** modifican tu código. Solo reportan. Tú decides qué corregir.

### ¿Por qué tres agentes?

Porque una sola pasada de review se pierde cosas. Un experto en seguridad no nota queries lentos. Un ingeniero de performance no nota la falta de tests. Al partir la carga cognitiva en tres roles especializados en paralelo, obtienes una cobertura que ningún review individual puede igualar.

> *"Para una decisión mayor, los tres MAGI votan. Gana la mayoría."*
> — Protocolo de operaciones de NERV, más o menos.

### Créditos

- Inspirado en **Neon Genesis Evangelion** (Hideaki Anno / Gainax / Khara) — todos los derechos del concepto MAGI, nombres y estética les pertenecen.
- Construido por [@DanyYanez](https://github.com/DanyYanez) para [Claude Code](https://claude.com/claude-code).

Si este skill te sirve, déjale una ⭐ y forkéalo. No lo subas como tuyo.

### Licencia

MIT — ver [LICENSE](LICENSE). Puedes usar, modificar y redistribuir libremente; solo mantén el aviso de copyright.

---

<p align="center">
  <sub>🔮 <em>"All systems online. MAGI deliberation complete."</em></sub>
</p>
