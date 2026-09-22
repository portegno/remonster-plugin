# Remonster Titles, for Claude Code

Eighty-one deterministic animated title presets for [Remotion](https://remotion.dev),
in nineteen families, each settable in any of twenty-four typefaces.

This plugin is free and holds no components. It gives Claude two things: a
catalogue server that answers what exists and what every preset takes, and a
skill that knows how to compose with them. The presets themselves come from
`@remonster/titles`, which [remonster.io](https://remonster.io) sells.

## Install

```
/plugin marketplace add portegno/remonster-plugin
/plugin install remonster-titles@remonster
```

Nothing to configure. The catalogue server is public and holds no account.

## What it adds

**A catalogue over MCP.** Four read-only tools: list the families, list or
search the titles, describe one with every prop and its real default, and get
the JSX for a title with only the props that differ written out. It holds no
account, no session and no state, and it never returns a line of a component.

**A skill.** How to compose a title in Remotion: asking for a duration rather
than writing the number, the props the presets share, and the two rules that
are otherwise found the hard way, which are that nothing may be random and that
an unknown prop is ignored in silence.

## Why a preset rather than writing the animation

- **Deterministic.** No `Math.random` anywhere. Remotion renders frames out of
  order and the player scrubs backwards, so anything non-deterministic
  disagrees with itself; two renders of the same video are byte-identical.
- **Text is measured, not assumed.** Titles shrink to fit whatever sentence
  they are handed, down to a floor.
- **One title, four formats.** Sizes come off the short edge, so the same
  preset sits the same way at 1920x1080, 1080x1080, 1080x1350 and 1080x1920.
- **A published version never changes what it draws.** A change to how a title
  looks is a major version, so a video already shipped keeps rendering the same
  frames.

## The catalogue is free, the components are not

The tools here answer with what is already on the shop page. Installing the
package needs a licence: one payment of USD 69, for life, covering
`@remonster/titles` and every title added to it. Without a credential,
`npm i @remonster/titles` answers 401 saying what it is and where to buy it.

The plugin stays useful either way: the skill is guidance about Remotion, and
the catalogue tells you what exists before you decide to buy anything.
