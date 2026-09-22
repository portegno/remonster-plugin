---
name: titles
description: Animate a title card in Remotion with the @remonster/titles presets: pick one of the sixty-three, set its props, and get the JSX to paste.
when_to_use: Use when composing an opening title, a lower third, a callout, a quote card, a terminal or typewriter effect, a logo lockup, or any animated text over video in a Remotion project; when choosing between the presets or setting their props; and whenever a title animation is about to be written by hand.
---

# Animated titles in Remotion

Sixty-three deterministic title presets, each settable in any of twenty-four
typefaces. Use them instead of writing a title animation by hand: they are
already measured, already fit their text, and render the same pixels every time.

## Find the title before writing any code

The `remonster-titles` MCP server answers about the catalogue. Ask it rather
than guessing at ids or props, which is the failure that is otherwise silent:
**an unknown prop is ignored and nothing is drawn differently.**

1. `list_families` when you do not know what kind of effect the shot wants.
2. `list_titles` with a `query` describing the shot: "opening", "over footage",
   "quote", "error". Filter by `family` once you know which one.
3. `describe_title` for the one you picked: every prop, its real default read
   off the component, and what it drives.
4. `title_snippet` to get the JSX, with only the props that differ from the
   title's own defaults written out. It names any prop the title does not take.

The eighteen families, and what each is for:

| family | titles | what it does |
|---|---|---|
| Echo | 3 | ghost copies trailing the text |
| Reveal | 3 | the line uncovered, boxed or masked |
| Glitch | 3 | tearing and chromatic break-up, seeded |
| Callout | 4 | a line with a subtitle or a marker beside it |
| Terminal | 4 | a shell, a prompt, a file being read |
| Typewriter | 6 | typed letter by letter; the length follows the text |
| Gradient | 2 | a travelling fill, for a headline that must look expensive |
| Mark | 4 | a logo lockup |
| Gravity | 1 | a physics simulation, seeded and solved once |
| Erosion | 8 | the text arriving out of, or breaking into, particles |
| Rain | 1 | falling characters |
| Arcade | 2 | pixel type and game motion |
| Letter build | 7 | assembled one letter at a time |
| Settle | 4 | the line arriving and coming to rest |
| Word build | 6 | assembled one word at a time |
| Swap | 3 | one line replaced by another |
| Board | 1 | an airport departures board: split flaps turning to each letter |
| Glow | 1 | letters coming into focus out of a coloured halo |

## Composing

Every title reads `useCurrentFrame()` from zero inside its own `<Sequence>`, so
it does not know where in the video it sits. Give the sequence the title's own
length:

```jsx
import { Sequence } from 'remotion';
import { T10, framesOf } from '@remonster/titles';

const lines = ['LET IT BE'];

<Sequence from={30} durationInFrames={framesOf(T10.meta, lines)}>
  <T10 lines={lines} />
</Sequence>
```

**Ask for the length, never write the number.** Some titles type themselves and
last as long as their text; every title gets longer when held longer.
`framesOf(meta, lines, props)` takes the same props you hand the title, so
`framesOf(T10.meta, lines, { stay: 60 })` answers for the held version. The
catalogue counts in frames at 30 fps, and a card whose `frames` reads "from the
text" has no fixed number at all.

## The props most titles share

`lines` (one string per line), `font`, `weight`, `size`, `fit`, `align`,
`position` ({ x, y } from 0 to 1 over the frame), `color` / `accent` /
`background`, the three beats `enter` / `stay` / `out`, and `seed` on the ones
with randomness in them. Each title also names a few of its own; `describe_title`
is the only reliable list.

Two that are easy to get wrong:

- **`out={0}` means it never leaves.** Use it for a title that should still be
  on screen when the sequence cuts.
- **`fit` is `true` by default**, so text too wide shrinks to a floor rather
  than overflowing. Set it `false` only when you want the size respected
  whatever the text.

Sizes come off the short edge, `min(width, height) / 1080`, so one title sits
the same way at 1920x1080, 1080x1080, 1080x1350 and 1080x1920. There is nothing
to configure per format.

## Rules worth keeping

**Never introduce randomness.** No `Math.random`, no `Date.now`, nothing that
reads the clock. Remotion renders frames out of order and the player scrubs
backwards, so anything non-deterministic disagrees with itself and a preview
stops matching the render. The titles that look random are seeded; change the
`seed` prop to get another variation.

**Prefer a prop over a fork.** If a title is nearly right, check
`describe_title` for a prop that covers the difference before copying the
component and editing it. A forked title is not covered by the guarantee that a
published version keeps drawing the same pixels.

**Do not invent ids or props.** Ids look like `T10`, `T29`, `T112`. They are not
sequential and the numbering has gaps, so an id that sounds plausible is not
evidence that it exists. Confirm with the MCP server.

## Installing

`@remonster/titles` is not on npm. It comes from a private registry and needs
two lines in the project's `.npmrc`:

```
@remonster:registry=https://npm.remonster.io/
//npm.remonster.io/:_auth=<the credential from the purchase email>
```

Without them `npm i @remonster/titles` answers 401 with where to buy it. If the
project does not have a credential, say so plainly rather than writing code that
cannot install; the licence is one payment at https://remonster.io.

React, Remotion and matter-js are peer dependencies the project already has.
Nothing else arrives with the package, and nothing phones home at render time.
