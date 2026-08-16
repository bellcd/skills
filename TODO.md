# TODO

Notes to self about this repo. No skill loads this file, so nothing here costs context.

Anything tied to one skill lives as an inline `<!-- TODO -->` in that skill instead, next to the thing it is about.

## Make memory writes nudge toward a durable home

Memories are a staging buffer. The pull toward treating them as a destination is strong enough that a nudge at write time would help, without turning the feature off.

The open question was whether auto-memory writes are even observable to a hook. They are. Across 68 project transcripts every memory-directory mutation was an ordinary assistant tool call, with no internal write path:

- `Edit` 410
- `Write` 79

`Edit` outnumbers `Write` roughly five to one, because appending the index line is an edit. A `PostToolUse` hook matching `Write` alone would miss most of the traffic and look like it was working.

Sketch, unbuilt: `PostToolUse` on `Write|Edit`, filtered to the memory directory, emitting a `systemMessage`. The `if:` filter uses permission-rule syntax, which is worth proving fires before shipping, since a hook that silently never runs is worse than none. A `SessionStart` counter is the fallback if the filter proves unreliable.

Lives in user-scope settings.

## Revisit the fetching and Chrome split

`fetching-web-sources` and `driving-chrome` were split on the argument that driving the browser to check your own app is a real trigger with no research in it.

Unproven. Once each has fired in anger, check whether the escalation from a refused fetch to the browser actually crosses that boundary cleanly, or whether one skill would have served.
