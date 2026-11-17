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
livello utente, quindi sono disponibili **in tutti i progetti** aperti sul tuo
computer.

Dalla versione 2.1.275 di Claude Code basta un solo comando per plugin, che
aggiunge anche il marketplace:

```
/plugin install documenti --marketplace merendafrancesco/claude-skills
/plugin install sviluppo --marketplace merendafrancesco/claude-skills
/plugin install design-web --marketplace merendafrancesco/claude-skills
/plugin install scrittura --marketplace merendafrancesco/claude-skills
/plugin install extra --marketplace merendafrancesco/claude-skills
```

L'installazione scrive in `~/.claude/settings.json` le stesse chiavi
(`extraKnownMarketplaces` ed `enabledPlugins`) mostrate più sotto: puoi anche
incollarle lì a mano.

Il repository è privato: Claude Code usa le tue credenziali Git/GitHub
(per esempio `gh auth login`) per scaricarlo.

Per ricevere le modifiche fatte al repository:

```
/plugin marketplace update claude-skills
```

## Condividerle su tutti i progetti

Dove ti servono le skill decide dove va la configurazione:

| Dove lavori | Cosa fare | Vale per |
|---|---|---|
| Claude Code sul tuo computer | Installazione globale (sopra) | Tutti i progetti locali |
| Sessioni cloud di Claude Code (claude.ai/code) | `.claude/settings.json` committato in ogni repository | Quel repository, per chiunque lo apra |
| Chat di claude.ai | Upload degli `.zip` (vedi sotto) | Il tuo account claude.ai |

Le sessioni cloud partono da un clone pulito del repository e non vedono il
tuo `~/.claude/settings.json`: per averle lì serve il file di progetto
descritto qui sotto, in **ogni** repository in cui lavori (per esempio
`pianofreddo`).

### Attivarle in un progetto (anche nel cloud)

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
Rimuovi le righe dei plugin che non servono a quel progetto. Se il progetto
ha già un `.claude/settings.json`, aggiungi le due chiavi a quelle esistenti
invece di sostituire il file.

Esempio per `pianofreddo`:

```bash
cd pianofreddo
mkdir -p .claude
# crea .claude/settings.json con il contenuto qui sopra (o uniscilo a quello esistente)
git add .claude/settings.json
git commit -m "Attiva i plugin di claude-skills"
git push
```

Dalla sessione successiva, Claude Code (in locale o nel cloud) propone di
installare il marketplace e i plugin. Poiché `claude-skills` è privato, l'account
GitHub usato dalla sessione deve poter leggere
`merendafrancesco/claude-skills`; se l'installazione non riesce, controlla che
il repository sia accessibile alla sessione.

Per verificare che le skill siano attive, in Claude Code esegui `/plugin` e
controlla che i plugin `@claude-skills` risultino abilitati.

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
3. Controlla che il marketplace sia valido:
   ```bash
   claude plugin validate .
   ```
4. Fai commit e push, poi in Claude Code: `/plugin marketplace update claude-skills`.

## Licenze

Le skill provengono da [anthropics/skills](https://github.com/anthropics/skills)
e sono distribuite con licenza Apache 2.0: ogni cartella conserva il proprio
`LICENSE.txt`. Fa eccezione `doc-coauthoring`, che nel repository originale non
include un file di licenza. Le note sulle componenti di terze parti sono in
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
