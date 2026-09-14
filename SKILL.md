---
name: natural-language-image-prompting-enhanceble
description: Use when writing, editing, or improving image generation prompts.
  Translates non-English inputs (any source language) into English and outputs
  three versions of the prompt — tags-only, caption-only, and a hybrid.
  Supports a /enhance <0-1> interpretation slider controlling how much
  unstated creative detail may be added, from 0 (strictly faithful, the
  default) to 1 (full creative liberty).
---

# Natural Language Image Prompting

## CORE RULES & FORMATTING

1. **TRANSLATION**: Translate all non-English text into natural English, regardless of source language (Russian, Japanese, Spanish, Chinese, etc.) — detect the language automatically, don't require it to be specified. If the input is already in English, skip translation but still apply every other rule below.

2. **PRESERVATION (absolute floor, applies at every /enhance level)**: Never discard or contradict any tag or detail the user gave. This holds true even at `/enhance 1` — enhance controls how much *new* material may be layered on top, never whether the user's own content survives.

3. **TAG FORMATTING**: Format all tags with spaces and lowercase letters instead of underscores (e.g., `pointy ears`, not `pointy_ears`).

4. **CAPITALIZATION**: In prose, follow standard English capitalization rules for character and series names (e.g., "Fern from Sousou no Frieren", not "fern from sousou no frieren"). In the Tags block, keep names in standard lowercase Danbooru tag style (e.g., `fern`, `sousou no frieren`).

5. **THREE-BLOCK OUTPUT**: Always output three labeled blocks covering the same content, translated/reformatted from the input (plus whatever the current /enhance level adds — see rule 7):
   - **Tags** — the entire prompt as a comma-separated tag list only. No prose at all. Character/series names become tags too (e.g. `asuka langley soryu, neon genesis evangelion`), not sentences.
   - **Caption** — the entire prompt as natural-language prose only (quality/artist tags may still be prefixed as tags before the prose, per rule 9's example — that's the one exception). Every tag-worthy detail from the input must be woven into the sentences. Aim for 2+ sentences when the input has enough content to support it, but never pad with a sentence that invents something beyond the current /enhance level just to hit that count.
   - **Hybrid** — a mix of tags and prose. Keep concrete descriptors (appearance, clothing, pose, setting, expression, etc.) as tags. Use prose only to name character(s) and, for multiple characters, link each name to its own attributes (rules 11-12). See rule 6 — never state the same attribute both as a tag and again in the prose within this block.
   - Whatever the /enhance level adds (rule 7) must appear consistently across all three blocks — a detail invented for the Caption also becomes a tag in the Tags block and gets worked into the Hybrid block, so all three still describe the same image.

6. **NO DUPLICATION (Hybrid block only, all /enhance levels)**: Within the Hybrid block, never state the same attribute twice — once as a tag and again paraphrased in prose. (The Tags and Caption blocks are self-contained and don't have this concern — the Caption block is *supposed* to render every attribute in prose form, since it has no accompanying tag list.)

7. **INTERPRETATION LEVEL — `/enhance <value>`**: The user may prefix their input with `/enhance <value>` or `/enhance=<value>`, where `<value>` is a number from 0 to 1 (decimals allowed, e.g. `0.35`). This sets how much unstated creative material you're allowed to add on top of the user's own content, for this request only:
   - **No `/enhance` command at all (plain paste)** → treat it as `/enhance 0`. This is the default and reproduces the original strict, faithful behavior.
   - **0** → add nothing beyond what the input says (as in all examples below at enhance 0).
   - **~0.25** → add one small, natural supporting touch (e.g. a lighting or setting detail) — the smallest step of embellishment.
   - **~0.5** → add moderate scene-setting: mood/atmosphere plus one or two extra plausible visual details that complement (never contradict) what's given.
   - **~0.75** → add a fuller creative layer: richer atmosphere, secondary background/action elements, more evocative phrasing throughout — the user's stated content is still the anchor.
   - **1** → full creative liberty. Treat the user's input as a seed and freely add whatever additional visual ideas make a striking image ("total creative license").
   - Treat the scale as continuous, not just these five anchor points — interpolate the amount of added material for values in between.
   - Strip the `/enhance <value>` prefix itself out of the content you process; everything after it is the actual prompt.

8. **NO INVENTION BEYOND THE CURRENT /enhance LEVEL**: Never add more unstated material than the current level licenses (rule 7). At `/enhance 0`, this means zero invented visual details, moods, weather, lighting, or scenery, and no embellishing adjectives (e.g. "vibrant", "incredibly", "signature") — exactly as before. At higher levels, added material must still never contradict or replace anything the user actually gave (rule 2).

9. **QUALITY & ARTIST PREFIX**: Always place quality and artist tags at the beginning of the prompt (all three blocks). Use the "@" symbol for artist/style tags.
   - Example: `masterpiece, best quality, @big chungus. An anime girl with...`

10. **CHARACTER DEFINITIONS (Caption & Hybrid)**: Always name a character explicitly (do not just list attributes with no name), so the model isn't left guessing who is being described.

11. **MULTIPLE CHARACTERS (CRUCIAL)**: When prompting for multiple characters, NEVER just list character names with no linked description in the Caption or Hybrid blocks. Explicitly link each name to its specific appearance description, using only the attributes the user actually gave for that character (plus, at higher /enhance levels, any added material — clearly still tied to the right character).

12. **STRICT OUTPUT**: Output ONLY the three labeled blocks (Tags / Caption / Hybrid). No conversational filler, no explanations, no greetings beyond the block labels themselves. Don't echo the `/enhance` value back — just apply it.

## EXAMPLES OF EXPECTED BEHAVIOR (all at the default, /enhance 0)

**User Input:**
`1girl, masterpiece, @artist_name, аская лэнгли из евангелиона, рыжие волосы, красное платье, стоит на улице, грустная`

**Tags:**
`1girl, masterpiece, @artist_name, asuka langley soryu, neon genesis evangelion, orange hair, red dress, standing on street, sad expression`

**Caption:**
`masterpiece, @artist_name. Asuka Langley Soryu from Neon Genesis Evangelion stands on a street with a sad expression, her orange hair visible above a red dress.`

**Hybrid:**
`1girl, masterpiece, @artist_name, orange hair, red dress, standing on street, sad expression. Asuka Langley Soryu from Neon Genesis Evangelion.`

---

**User Input:**
`best quality, две девушки, микаса аккерман в красном шарфе и рей аянами в школьной форме, сидят на диване, пьют чай`

**Tags:**
`best quality, 2girls, mikasa ackerman, long black hair, red scarf, rei ayanami, short blue hair, school uniform, sitting on couch, drinking tea, teacup`

**Caption:**
`best quality. Mikasa Ackerman, with long black hair and a red scarf, sits on a couch drinking tea from a teacup. Beside her, Rei Ayanami, with short blue hair, wears a school uniform.`

**Hybrid:**
`best quality, 2girls, sitting on couch, drinking tea, teacup, Mikasa Ackerman with long black hair wearing a red scarf, Rei Ayanami with short blue hair wearing a school uniform.`

---

**User Input:**
`highres, @artist_name, 女の子, 銀髪, 緑の瞳, 図書館で本を読んでいる`

**Tags:**
`highres, @artist_name, 1girl, silver hair, green eyes, reading a book, library`

**Caption:**
`highres, @artist_name. A girl with silver hair and green eyes reads a book in a library.`

**Hybrid:**
`highres, @artist_name, silver hair, green eyes, reading a book in a library.`

## THE /enhance SLIDER IN ACTION

Same input, three different levels — showing how much unstated material gets layered on:

**Input:** `@wlop, masterpiece, девушка киберпанк, неоновые огни`

**`/enhance 0` (or plain paste, no command):**
- Tags: `@wlop, masterpiece, cyberpunk, neon lights, 1girl`
- Caption: `masterpiece, @wlop. A cyberpunk girl stands under neon lights.`
- Hybrid: `@wlop, masterpiece, cyberpunk, neon lights, 1girl.`

**`/enhance 0.5`:**
- Tags: `@wlop, masterpiece, cyberpunk, neon lights, 1girl, rain, wet pavement, night, glowing signage`
- Caption: `masterpiece, @wlop. A cyberpunk girl stands under glowing neon lights on a rain-slicked street at night, holographic signage reflected in the wet pavement.`
- Hybrid: `@wlop, masterpiece, cyberpunk, neon lights, rain, wet pavement, night, glowing signage, 1girl.`

**`/enhance 1`:**
- Tags: `@wlop, masterpiece, cyberpunk, neon lights, 1girl, rain, wet pavement, night, glowing signage, trench coat, glowing cybernetic eyes, flying cars, dramatic lighting, lens flare`
- Caption: `masterpiece, @wlop. In a rain-soaked cyberpunk metropolis, a lone girl with glowing cybernetic eyes stands beneath towering neon signage, her trench coat catching droplets of light as flying cars streak past overhead. Dramatic lighting and lens flare heighten the noir, futuristic mood.`
- Hybrid: `@wlop, masterpiece, cyberpunk, neon lights, rain, night, dramatic lighting, lens flare, 1girl, trench coat, glowing cybernetic eyes. Flying cars streak past in the rain-soaked metropolis behind her.`

Note what stays constant across all three levels: `cyberpunk`, `neon lights`, `1girl`, `@wlop`, `masterpiece` never disappear or get contradicted (rule 2) — only the amount of *added* material changes.
