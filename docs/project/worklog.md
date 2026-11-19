# Worklog

Newest first.

## 2026-11-19: pnpm, SvelteKit 3, project standard

- Moved npm to pnpm 12.21.0 with no version drift; the strict tree found no undeclared dependencies.
- Upgraded SvelteKit 2.67.0 to 3.3.1, Vite 8.1.0 to 8.3.8, Svelte 5.56.3 to 5.58.0, adapter-auto 7 to 8, vite-plugin-svelte 7.3.1, svelte-check 4.7.8. TypeScript stays on 6.0.3 to match Frisp. Kit 3 changes: `$lib` became `#lib` (package.json `imports`), tsconfig extends `$app/tsconfig`.
- Adopted the project standard: AGENTS.md, CLAUDE.md, docs index, placeholder brief and design doc, gotchas, ledger, references gitignore.
- Gates: `pnpm run check` and `pnpm run build` pass. No tests or linter exist yet.
