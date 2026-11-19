Cast is a SvelteKit app for a shadow stack generator: compose, compare, and export CSS and Tailwind shadow systems. Start with docs/README.md (the docs index) and docs/project/brief.md (what this is and where it stands). Read the docs related to your task BEFORE working and update the ones you invalidated after; the index routes you, so load only what the task needs.

## Style tie-breakers
Match the surrounding code by default. The app is still a scaffold, so no exemplar files are named yet; deep-pass adds 2 to 4 once real code exists.

## Hard rules (every task; all how-to detail lives in docs/)
- Commit checkpoint-sized changes regularly. Push finished work whose gates pass without asking; never force-push.
- One source of truth per topic.
- Use pnpm only; the version is pinned once, in package.json `packageManager`.
- Before finishing code work: `pnpm run check` and `pnpm run build`. Add nothing, break nothing (silent traps: docs/gotchas.md).
- No em dashes in anything you write; rewrite the sentence to be simpler instead.
- Note your gotcha discoveries in your final report so the worklog can capture them.
- Svelte work: follow ~/.agents/svelte-mcp.md (read the docs through the Svelte MCP tools; svelte-check is the gate, no autofixer loop).
