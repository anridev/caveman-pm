# 🪨 caveman

> why use many token when few token do trick

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that makes Claude talk like a caveman — cutting ~75% of tokens while keeping full technical accuracy.

Based on the [viral observation](https://x.com) that caveman-speak dramatically reduces LLM token usage without losing technical substance.

## Before / After

**Normal Claude:**
> "The reason your React component is re-rendering is likely because you're creating a new object reference on each render cycle. When you pass an inline object as a prop, React's shallow comparison sees it as a different object every time, which triggers a re-render. I'd recommend using useMemo to memoize the object."

**Caveman Claude:**
> "New object ref each render. Inline object prop = new ref = re-render. Wrap in `useMemo`."

Same fix. 75% less word.

## Install

```bash
claude install-skill julb/caveman
```

Or clone repo and add to your Claude Code skills manually.

## Usage

Once installed, trigger with:
- `/caveman` 
- "talk like caveman"
- "use caveman mode"
- "less tokens please"

To stop: say "stop caveman" or "normal mode"

## Rules

- English go caveman. Code stay normal
- Technical terms keep exact (no dumb down "polymorphism" to "many shape thing")
- Error messages quoted exact
- Git commits and PRs stay normal

## Why

- Token go brrr (save money)
- Response faster (less generate)
- Same accuracy (all technical info kept)
- Fun (ooga booga)

## License

MIT — free like mammoth on open plain.
