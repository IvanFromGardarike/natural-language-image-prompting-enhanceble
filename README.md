# natural-language-image-prompting-enhanceble

A Claude or local LLM client Skill that turns tag-style or natural-language image-generation
prompts — in any language — into three ready-to-use English prompt formats,
with an adjustable slider for how much unstated creative detail Claude is
allowed to add.

Built with Danbooru/Anima-style anime image models in mind, but the
underlying logic (tags vs. caption vs. hybrid, no silent duplication, no
invented detail unless you ask for it) applies to most tag-aware
text-to-image models.

## What it does

Give it a prompt — a list of tags, a plain-language description, or a mix of
both, in whatever language you write in — and it returns three parallel
versions of the same prompt:

- **Tags** — a pure, comma-separated tag list. No prose.
- **Caption** — pure natural-language prose. Every detail from your input is
  woven into full sentences, so it works as a genuine caption, not just a
  reworded tag dump.
- **Hybrid** — tags for concrete descriptors (appearance, pose, setting),
  natural language only where it earns its place (naming a character,
  linking names to attributes when there are several people in the scene).

All three describe the exact same image. Nothing you provide is dropped, and
nothing is duplicated across a block (an attribute is never stated as both a
tag *and* a paraphrased sentence in the same block).

## The `/enhance` slider

By default, the skill is strictly faithful: it only translates, reformats,
and restructures what you actually gave it — it never invents mood,
lighting, weather, or extra scenery on its own.

If you want creative help filling in the details you didn't specify, prefix
your prompt with `/enhance <value>`, where `<value>` is a number from `0` to
`1`:

| Value | Behavior |
|---|---|
| *(no command / plain paste)* | Same as `0` — fully faithful, nothing added |
| `0` | Nothing added beyond what you wrote |
| `~0.25` | One small supporting touch (e.g. a lighting detail) |
| `~0.5` | Moderate scene-setting: mood plus a couple of extra plausible details |
| `~0.75` | A fuller creative layer: richer atmosphere and background elements |
| `1` | Full creative liberty — your input is treated as a seed for a richer scene |

Whatever the slider adds is layered on consistently across all three output
blocks, and it never contradicts or removes anything you actually specified
— the slider only controls how much *new* material Claude may add on top.

```
/enhance 0.5 @wlop, masterpiece, cyberpunk girl, neon lights
```

## Language support

Input can be in any language — it's auto-detected, you don't need to
specify it. If the input is already in English, the skill skips translation
but still applies all the same formatting and structural rules.

## Installation

Drop the folder into your skills directory so the layout looks like:

```
.claude/skills/natural-language-image-prompting-enhanceble/
└── SKILL.md
```

or the equivalent `skills/` directory inside a Claude Code plugin. Once
installed, Claude will pick it up automatically when the context matches
(writing or editing an image prompt), or you can invoke it directly with:

```
/natural-language-image-prompting-enhanceble
```

## Example

**Input:**
```
1girl, masterpiece, @artist_name, аская лэнгли из евангелиона, рыжие волосы, красное платье, стоит на улице, грустная
```

**Tags:**
```
1girl, masterpiece, @artist_name, asuka langley soryu, neon genesis evangelion, orange hair, red dress, standing on street, sad expression
```

**Caption:**
```
masterpiece, @artist_name. Asuka Langley Soryu from Neon Genesis Evangelion stands on a street with a sad expression, her orange hair visible above a red dress.
```

**Hybrid:**
```
1girl, masterpiece, @artist_name, orange hair, red dress, standing on street, sad expression. Asuka Langley Soryu from Neon Genesis Evangelion.
```

See `SKILL.md` for the full rule set and more worked examples, including the
`/enhance` slider applied to the same prompt at several levels.

## License

Add whichever license you'd like this repo to use (e.g. MIT) before
publishing.
