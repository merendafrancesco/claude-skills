# claude-skills

Marketplace personale di plugin per **Claude Code** con le skill di
[anthropics/skills](https://github.com/anthropics/skills) (licenza Apache 2.0),
raggruppate per argomento. Si installa una volta e vale in tutti i progetti.

## Plugin disponibili

| Plugin | Skill incluse | A cosa serve |
|---|---|---|
| `documenti` | docx, pdf, pptx, xlsx | Word, PDF, PowerPoint ed Excel |
| `sviluppo` | skill-creator, mcp-builder, claude-api | Creare skill, server MCP e app con la Claude API |
| `design-web` | frontend-design, canvas-design, theme-factory, web-artifacts-builder, webapp-testing, algorithmic-art | Design frontend, arte visiva, temi, artifact web, test di webapp |
| `scrittura` | doc-coauthoring, internal-comms, brand-guidelines | Co-scrittura di documenti, comunicazioni interne, linee guida di brand |
| `extra` | slack-gif-creator, academy-guide | GIF animate per Slack, guida ai corsi di Claude Academy |

## Installazione globale (consigliata)

In Claude Code, da qualsiasi cartella:

```
/plugin marketplace add merendafrancesco/claude-skills
/plugin install documenti@claude-skills
```

Ripeti `/plugin install <nome>@claude-skills` per ogni plugin che vuoi
(`sviluppo`, `design-web`, `scrittura`, `extra`). I plugin si installano a
livello utente, quindi sono disponibili **in tutti i progetti**.

Il repository è privato: Claude Code usa le tue credenziali Git/GitHub
(per esempio `gh auth login`) per scaricarlo.

Per ricevere le modifiche fatte al repository:

```
/plugin marketplace update claude-skills
```

## Attivarle per un singolo progetto (anche nel cloud)

Copia questo file in `.claude/settings.json` del progetto e fai commit:

```json
{
  "extraKnownMarketplaces": {
    "claude-skills": {
      "source": { "source": "github", "repo": "merendafrancesco/claude-skills" }
    }
  },
  "enabledPlugins": {
    "documenti@claude-skills": true,
    "sviluppo@claude-skills": true,
    "design-web@claude-skills": true,
    "scrittura@claude-skills": true,
    "extra@claude-skills": true
  }
}
```

Chiunque apra il progetto con Claude Code (sul computer o in una sessione
cloud) riceve la proposta di installare il marketplace e i plugin elencati.
Rimuovi le righe dei plugin che non servono a quel progetto.

## In alternativa: copia manuale

Copia le cartelle delle skill che ti servono in `~/.claude/skills/`:

```bash
git clone https://github.com/merendafrancesco/claude-skills.git
mkdir -p ~/.claude/skills
cp -r claude-skills/skills/docx claude-skills/skills/pdf ~/.claude/skills/
```

Ogni cartella deve contenere il suo `SKILL.md` direttamente al suo interno
(`~/.claude/skills/docx/SKILL.md`).

## Usarle anche su claude.ai (chat e sessioni cloud)

Le skill installate in Claude Code restano sul tuo computer. Per averle anche
nelle chat di claude.ai e nelle sessioni di Claude Code sul web:

1. Crea uno `.zip` per ogni skill, con la cartella della skill alla radice:
   ```bash
   cd skills && zip -r ../docx.zip docx
   ```
2. Su claude.ai apri **Impostazioni → Capabilities → Skills**.
3. Carica gli `.zip` uno alla volta e attiva le skill.

## Aggiungere una nuova skill

1. Crea `skills/<nome-skill>/SKILL.md` con il frontmatter:
   ```markdown
   ---
   name: nome-skill
   description: Cosa fa e quando Claude deve usarla.
   ---

   Istruzioni per Claude...
   ```
   (puoi farti aiutare dalla skill `skill-creator`).
2. Aggiungi `"./skills/<nome-skill>"` all'array `skills` del plugin giusto in
   `.claude-plugin/marketplace.json` (o crea un nuovo plugin).
3. Fai commit e push, poi in Claude Code: `/plugin marketplace update claude-skills`.

## Licenze

Le skill provengono da [anthropics/skills](https://github.com/anthropics/skills)
e sono distribuite con licenza Apache 2.0: ogni cartella conserva il proprio
`LICENSE.txt`. Fa eccezione `doc-coauthoring`, che nel repository originale non
include un file di licenza. Le note sulle componenti di terze parti sono in
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
