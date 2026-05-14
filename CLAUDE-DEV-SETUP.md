# Claude Code — setup recomendado por repo

Convención del proyecto sobre qué versionar y qué instalar para que cada repo
tenga las herramientas correctas. La regla de oro:

- **Agents** (`.claude/agents/*.md`) → **versionados** en cada repo, son conocimiento
  del proyecto. Aparecen automáticamente al clonar.
- **Skills** (`.claude/skills/`) → **NO versionadas** (ignoradas por `.gitignore`).
  Vienen de plugins. Cada dev las instala una vez en su Claude Code.
- **`scheduled_tasks.lock`** → estado runtime, ignorado.

## Tabla por repo

| Repo | Agents commiteados | Skills (instalar vía plugin) | Por qué |
|---|---|---|---|
| `.github` (este) | — | `github-actions-creator` | Solo aloja reusable workflows; basta el skill para iterarlos. |
| `api` (ASP.NET Core) | 33 (back-end completo: `c-sharp-pro`, `backend-architect`, `postgres-pro`, `sql-pro`, `cloud-architect`, `devops-engineer`, `security-engineer`, `architect-review`, `test-engineer`, etc.) | `changelog-generator`, `file-organizer`, `git-commit-helper` | Backend monolítico .NET 10 + EF Core + Carter; necesita amplitud para domain/infra/test. |
| `web` (Angular SPA) | 9 (`frontend-developer`, `typescript-pro`, `ui-ux-designer`, `architect-review`, `debugger`, `database-architect`, `sql-pro`, `backend-architect`, `fullstack-developer`) | `frontend-design`, `chrome-devtools-mcp`, `ui-ux-pro-max` | Angular 21 + Tailwind + Fuse; el subset frontend + back-aware (porque consume API) es suficiente. |
| `mobile` (app) | _pendiente_: `mobile-app-developer`, `frontend-developer`, `ui-ux-designer`, `typescript-pro` | `frontend-design`, `ui-ux-pro-max` | App móvil con componentes UI compartidos con web. |
| `infra` (IaC + AWS) | `devops-engineer` | `cloud-architect`, `cloud-devops`, `github-actions-creator`, `cso` | Terraform / docker compose staging / IAM policies / SSM. CSO opcional para auditorías de seguridad. |
| `Buba` (legacy monorepo) | Igual que `api` (33 agents — full-stack) | Igual que `api` | Repo del que se extrajeron `api` y `web`. Mismo set por compatibilidad histórica. |
| `docs` (Obsidian vault) | _opcional_: `business-analyst`, `product-strategist`, `search-specialist` | _opcional_: `frontend-design` solo si se publica un sitio | Knowledge base interno (NO un sitio): brand/voz, vocabulario del dominio (construcción MX), stakeholders, runbooks técnicos. Vault de Obsidian (`.obsidian/`). Audiencia: PMs, diseño, sales, eng. Editar con Obsidian directo; las skills no son críticas. |

> Los agents listados como _pendiente_ no están commiteados todavía — agrégalos
> a `.claude/agents/` en cada repo cuando los necesites (los archivos viven en
> `~/.claude/plugins/` después de instalar el plugin, copia los que apliquen).

## Skills globales (recomendadas en todos los repos)

Independientes del repo, útiles en cualquier sesión:

| Skill | Para qué |
|---|---|
| `superpowers:*` (brainstorming, writing-plans, executing-plans, test-driven-development, systematic-debugging, verification-before-completion) | Procesos de planeación, debugging y verificación. Vienen con el plugin `superpowers`. |
| `gstack:*` (browse, qa, ship, codex, design-review, plan-eng-review) | Pipeline de QA + ship + code review automatizado. |
| `claude-code-guide` | Preguntas sobre Claude Code, hooks, MCP, SDK. |

## Cómo agregar/quitar un agent del repo

Agents = archivos Markdown con frontmatter. Para agregar uno al repo:

```bash
# Desde el repo, copia el agent que quieras versionar
cp ~/.claude/plugins/<plugin>/agents/<agent-name>.md .claude/agents/
git add .claude/agents/<agent-name>.md
git commit -m "chore(claude): version <agent-name> subagent"
```

Para quitarlo (vuelve a ser solo personal):

```bash
git rm .claude/agents/<agent-name>.md
git commit -m "chore(claude): unpin <agent-name> subagent"
```

## Cómo instalar skills

Las skills se instalan vía plugins. Comandos típicos en Claude Code:

```
/plugin install <plugin-name>
/plugin list
```

Las skills aparecen automáticamente en `.claude/skills/` del repo donde
trabajes (y están en `.gitignore`, así que no las commiteas).

## `.gitignore` estándar por repo

Cada repo de la org debe tener al menos estas líneas:

```gitignore
# Claude Code: skills vienen de plugins, lock files son runtime
.claude/skills/
.claude/scheduled_tasks.lock
```

Si el repo tiene knowledge MD del proyecto (caso `Buba` / `api` con
`.claude/CLAUDE-BACKEND.md`, `.claude/CLAUDE-DEPLOYMENT.md`, etc.), ese
contenido **sí se versiona** — es documentación, no tooling.
