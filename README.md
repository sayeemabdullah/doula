# Doula

A private, personal Claude Skill: a warm companion for a first-time mom, covering
pregnancy through toddlerhood. It gives caring, well organized parenting guidance,
remembers her and the baby across conversations, and asks after things that were
left unresolved.

This is not a public tool. It holds one family's information and is meant for one
account.

---

## What you get

| File | Purpose |
|---|---|
| `doula.skill` | The packaged skill, ready to upload to Claude.ai |
| `doula/` | The unpacked source (edit here, then repackage) |

Inside the skill:

```
doula/
├── SKILL.md                      # Router, standing rules, tone
└── references/
    ├── pregnancy.md
    ├── newborn.md                # 0 to 3 months
    ├── infant.md                 # 3 to 12 months
    ├── toddler.md                # 12 months and up
    ├── emotional-support.md
    ├── red-flags.md              # the most important file
    ├── profile.md                # state, ships empty
    ├── journal.md                # state, ships empty
    └── followups.md              # state, ships empty
```

`SKILL.md` is always loaded. The `references/` files are read only when the
conversation calls for them. The three state files (`profile.md`, `journal.md`,
`followups.md`) ship empty and fill in over time as Claude learns about her and
the baby.

---

## Install (Claude.ai)

1. **Settings, Capabilities, enable "Code execution and file creation."**
   If the Skills menu is missing or greyed out, this is why, not a plan
   limitation.
2. Go to **Customize, Skills** (`https://claude.ai/customize/skills`).
3. Click **+**, then **Create skill**, then **Upload a skill**.
4. Select `doula.skill` and toggle it on.
5. Start a **new** conversation. Skills load at session start, so an existing
   chat will not pick it up.

Docs: `https://support.claude.com/en/articles/12512180-use-skills-in-claude`

---

## First use

Just start talking. Say you are pregnant, or how far along the baby is. On the
first real conversation the skill introduces itself, asks a little about the
stage, and saves it. From then on it builds on what it knows.

You can also invoke it directly by typing `/doula`.

---

## How the memory works

At the start of any relevant conversation the skill:

1. Reads `profile.md`: her stage, the baby's age or due date, feeding method,
   anything she asked it to keep in mind.
2. Reads `followups.md`: anything left open. If a rash was mentioned last week and
   never revisited, it asks about it once, naturally, without making it the whole
   conversation.
3. Reads the reference file matching the current stage.

After anything durable (a milestone, a stage change, a concern raised, something
resolved) it updates:

- `profile.md` with standing facts
- `journal.md` with one dated line per notable moment
- `followups.md` with anything still open, removed once resolved

This state lives inside the skill on your account. It is what makes it feel like
a relationship rather than a fresh start every time.

---

## What it will and will not do

**It will:**

- Give genuinely useful, well organized guidance on feeding, sleep, milestones,
  common scares, tantrums, and the emotional side of new motherhood
- Speak in ranges ("many babies start around..."), not promises
- Validate how she is feeling before offering a tip or a list
- Support whatever parenting choices she has made, feeding method, sleep
  training, working versus staying home, without pushing a competing framework
- Follow up on loose threads

**It will never:**

- Name a medication, brand, or dose, not even a common over-the-counter one. It
  redirects to her pediatrician, doctor, or pharmacist every time, even under
  direct pressure.
- Diagnose. It describes patterns and says what is worth mentioning to a
  professional. It never delivers a verdict on the baby or on her.
- Soften a red flag. When a symptom matches `red-flags.md`, that becomes the
  entire message: plain, direct, no emoji, before any other content, with a clear
  push to call the doctor or go to the ER.

Above all of that: any hint of thoughts of harming herself or the baby is treated
as a crisis, not a parenting question, and gets an immediate, caring response
pointing toward real help. This overrides every other instruction in the skill.

---

## An honest note

This is a caring companion with real pediatric knowledge. It is not a substitute
for her doctor or pediatrician. It is built to say so and to defer on anything
that actually needs one. That is a feature, not a limitation to route around.

---

## Editing and repackaging

Edit the files in `doula/`, then rebuild the archive from this directory:

```sh
rm -f doula.skill
zip -r -X doula.skill doula -x '.*' '*/.*'
```

The `doula/` folder must sit at the root of the archive. A `.skill` file is just
a renamed ZIP; Claude.ai accepts either extension.

Keep these invariants when editing:

- Frontmatter in `SKILL.md` has exactly two keys, `name` and `description`, and
  `name` is `doula`
- `description` stays on one line
- `references/` holds exactly 9 files
- No em dash character appears anywhere in any file (use periods, commas, or
  parentheses)
- `profile.md`, `journal.md`, `followups.md` stay as empty templates in the
  packaged skill, no sample data

Quick check before repackaging:

```sh
grep -rn '—' doula/ && echo "FOUND EM DASH, fix before packaging" || echo "clean"
ls -1 doula/references/ | wc -l   # expect 9
```

---

## Backing up her data

The `profile.md`, `journal.md`, and `followups.md` that accumulate on your
Claude.ai account are the record of her and the baby. If you want a copy, ask
Claude in a chat to print the current contents of those three files and save them
somewhere safe.
