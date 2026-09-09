# Gig brief — referent image sourcing & attribution (core process)

**What this is:** paid piecework finding clear, free-to-use images for lang-pages.
No programming or maths — the value is careful curation and getting the licensing
right. One "unit" of work = one *referent* (a meaning or a thing, e.g. `fire`,
`tree`, `Mount Fuji`, `to eat`) with **about 5** good images picked, verified, and
recorded — a small set the editor then picks the best from.

**This file is the process — it never changes.** *What* you source (the list of
referents) and *what a good image means for that subject* live in a separate
**subject sheet** you'll be given alongside this one (e.g.
[`subjects/kangxi-radicals.md`](subjects/kangxi-radicals.md)). The ask is always the
same shape: *"do this process, for the referents on that sheet."* Everything below
is subject-agnostic; wherever a judgement depends on the subject, this brief points
you at the sheet.

**You never touch JSON.** Images the fetch script pulls are recorded for you
automatically; images you find yourself get written as a simple fill-in-the-blanks
text block (details below).

Companion docs: [authoring.md](authoring.md) (the wider ingest process — you don't
need it), [content-graph-schema.md](content-graph-schema.md).

---

## The one rule that matters most: licensing

Only these licenses are acceptable, because we redistribute the image:

- **Public Domain (PD)** / **CC0**
- **CC BY** (attribution required)
- **CC BY-SA** (attribution required)

**Anything else — reject it.** No "all rights reserved", no NC (non-commercial),
no ND, no "free for personal use only", no unknown/blank license. If you can't find
a clear license statement on the source page, treat it as unusable and move on.

Every accepted image **must** have its attribution recorded exactly. A great image
with the wrong or missing credit is a *reject*, not a "fix later".

---

## One-time setup

The fetch script talks to Wikimedia, which asks for a real contact address. Set it
once per terminal session (use whatever email you're comfortable with):

```bash
export WIKIMEDIA_CONTACT="you@example.com"
```

---

## Workflow, per referent

1. **Get candidates from Wikimedia** using the script. The `<slug>` and a good
   `<search query>` for each referent come from your subject sheet:

   ```bash
   python3 data/fetch-referent.py <slug> "<search query>" -n 5
   # e.g.
   python3 data/fetch-referent.py tree "tree isolated white background" -n 5
   ```

   This pulls thumbnails into `shared/referents/`, keeps only free-licensed ones,
   and **records them for you** — no typing needed.

2. **Look at what came back.** Keep the ones that are sharp, obvious and well-licensed
   — you're aiming for a set of about 5. Glance at each Wikimedia file page to confirm
   the credit and license look right, and drop any that are weak or wrongly licensed.

3. **If nothing good came back, search deeper yourself** (this is where the real
   value is). Good CC/PD sources:
   - **Openverse** (openverse.org) — searches CC images across many sites at once; **start here** for a deeper hunt.
   - **Wikimedia Commons** (commons.wikimedia.org) — browse the category, not just search.
   - **Flickr** with the license filter set to Creative Commons.
   - PD collections (e.g. museum open-access, Wikimedia PD categories).

   *(Avoid Unsplash / Pexels / Pixabay for now — their images are "free" but under
   their own non-CC licenses that don't fit our attribution format. Stick to CC/PD.)*

   When you find a keeper: **download it into `shared/referents/`** with a sensible
   filename (`<slug>-NN.jpg`), then **record it as a text block** — see next section.

---

## Recording a hand-found image (the fill-in block)

For images you found yourself, open **`data/referents-inbox.toml`** and add one
block per image. It's just labelled lines — copy the template, fill in the values,
keep the quotes:

```toml
[[image]]
slug    = "tree"          # the referent id, from your subject sheet
label   = "tree"          # how it reads on the card (usually same as slug)
file    = "tree-03.jpg"   # the filename you saved into shared/referents/
credit  = "Jane Doe"      # author, exactly as the source page states
license = "CC BY-SA 4.0"  # must be PD / CC0 / CC BY / CC BY-SA — nothing else
source  = "https://commons.wikimedia.org/wiki/File:..."   # the file PAGE url
```

- Add as many `[[image]]` blocks as you like — one per image.
- `source` is the **file page URL** (the page that shows the license), not the raw
  image link.
- Type the `credit` and `license` **exactly** as the source states them. Don't
  paraphrase — this is the bit that has to be precise.

That's it. Wynand converts the inbox into the site's data himself; you never edit
the JSON.

---

## What makes a *good* referent image

**Always, whatever the subject:**

- **Clean / uncluttered** — single clear subject, ideally isolated or on a simple
  background. No text overlays, watermarks, collages, or busy scenes.
- **Neutral & safe** — no gore, nothing offensive, no obvious brand logos or
  identifiable private individuals as the subject.
- **Decent quality** — in focus, not tiny, not heavily compressed.
- **Correctly licensed and credited** (see the licensing rule above).

**How you judge "does it depict the meaning?" depends on the *type* of referent.**
Your subject sheet marks each referent's type (or says the whole subject is one
type). Here's what each type asks for:

1. **Object / being** — a concrete thing (`knife`, `dog`, `mountain`). *Around 5 clear,
   typical examples* of the same thing, so the editor has good ones to choose from. If a
   stranger couldn't name the meaning from an image alone, drop that one; skip odd or
   edge-case examples and keep the ones they'd name instantly.

2. **Broad / umbrella** — a meaning many different things fit under (`person` covers
   men, women, children; `animal` covers dogs, birds, fish). These want the *opposite*
   of an object: **show the variety, not one example.** Your ~5 images should genuinely
   differ (a mixed group, a range), and **avoid any single image that reads as a
   narrower meaning** — a lone woman photo under `person` looks like `woman`, which we
   teach separately. Lead with the most general-looking image (a mixed group beats any
   individual).

3. **Action (verb)** — `eat`, `run`, `see`. The image must show the **activity
   happening**, not the object involved. `eat` = a person eating, *not* a plate of
   food. Choose a shot where the action is unmistakably mid-motion and the actor is
   clearly doing it.

4. **Quality (colour / property)** — `red`, `big`, `dry`. The **property** has to be
   the obvious point, not the thing carrying it. Show it on a clear, neutral carrier
   (a red swatch or a plainly-red single object for `red`), and where you can, pick a
   carrier that isn't itself an important referent — so the learner reads the *quality*,
   not the object. For comparative qualities (`big`, `long`) a shot that implies the
   contrast reads best.

5. **Identity / named entity** — a specific named thing (`Mount Fuji`, a landmark, a
   named person). It must be **that exact one**. A generic lookalike is a **hard
   reject** — the opposite of the broad rule: here a "typical mountain" for `Mount Fuji`
   teaches a wrong fact. Confirm the file page actually names the right subject before
   accepting.

6. **Abstract / conventional** — `freedom`, `and`, `sound`, a grammatical particle.
   Often **no literal photo exists.** If there's a widely-understood convention or
   symbol for it, use that; otherwise **flag it as not image-suitable and move on** —
   don't force a misleading picture. Your subject sheet will tell you when to expect
   these and how it wants them handled.

Aim for **about 5** per referent, so the editor has room to pick the best — but 5
near-identical or mediocre shots don't help. Every one should be clean and clearly on
the meaning; for **broad** referents they should be genuinely *varied*. If you can only
honestly find 2 or 3 good ones, that's fine — quality beats hitting the number.

---

## Definition of done (per referent)

- Around 5 images saved in `shared/referents/` (a *varied set* for **broad** referents;
  fewer is fine if that's all you can honestly find).
- Script-fetched images: confirmed the auto-recorded credit/license looks right.
- Hand-found images: a complete `[[image]]` block in `data/referents-inbox.toml`
  (all six lines filled).
- Every license is one of PD / CC0 / CC BY / CC BY-SA.
- The image clearly and unambiguously depicts the meaning, **judged by the referent's
  type** (see the type menu above — object vs broad vs action vs quality vs identity).
- **Abstract / not-suitable** referents: flagged as such (per your sheet's
  instruction), not forced with a misleading image.

**Don't:** invent or guess a credit; use a non-free image "just this once"; add text
overlays or edit images; paraphrase a license string; force a picture onto a referent
that has no honest one.

---

## How the work is checked

Wynand runs two things — you don't need to:

1. `python3 data/referents-from-toml.py` — folds your inbox blocks into the site data
   **exactly as you typed them** (no LLM, no rewording), and rejects any non-free
   license or incomplete block on the spot.
2. `python3 data/check-source.py` — confirms every referent resolves and is sound.

If something's off you'll get it back with a note — no problem, it's expected on the
first batch while we calibrate.

---

## Pay

- **Rate:** R20 per referent completed (a set of ~5 verified images + accurate recording).
- **Trial:** first small batch (≈5 referents) at full rate, so we can check the
  workflow fits and adjust anything before you do volume.
- Work at your own pace — there's no clock, just completed referents.

*(The subject sheet may set a different rate or trial size for a subject where the
sourcing is unusually hard — if so, its numbers win over these.)*
