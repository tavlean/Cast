# Gotchas

- `$lib` does not exist in SvelteKit 3. Import from `src/lib` through `#lib/...`, which is a Node subpath import declared in package.json `imports`, so a new top-level source folder needs its own entry there.
- SvelteKit config lives in `vite.config.ts` (the `sveltekit({...})` plugin options). There is no `svelte.config.js`.
