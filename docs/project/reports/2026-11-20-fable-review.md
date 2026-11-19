# Whole-project review (2026-11-20)

Scope: every tracked file, read-only. Cast is a starter scaffold: one placeholder page, no generator code. So most of the value is in shaping what comes next, and the defects found are small. Ranked most valuable first. Sizes: S under an hour, M a session, L several sessions.

## 1. The product does not exist yet; settle the shadow model before any UI (L)

- Evidence: `src/routes/+page.svelte:9-12` is a heading and one sentence. `src/lib/index.ts:1` is a comment. README and `docs/project/brief.md` promise compose, compare and export of CSS and Tailwind shadow systems; none of the three exists.
- Why it matters: compose, compare and export all read the same data. If the UI is built first, the model gets shaped by the first screen and export becomes string surgery.
- Suggested fix: run the brief skill (the brief is a no-interview placeholder), then build `src/lib/shadow/` as pure TypeScript with no Svelte imports: a `ShadowLayer` type (offset, blur, spread, color, inset), a `ShadowStack` as an ordered list, and pure serializers `toCss(stack)` and `toTailwind(stack)`. UI state lives in one `.svelte.ts` module that holds stacks with `$state` and derives output with `$derived`. Store colors in OKLCH so opacity and tint ramps across layers stay perceptually even. Encode the current stack in the URL hash so a stack is shareable and "compare" is two hashes side by side, with no backend.

## 2. Choose the adapter now: adapter-static, fully prerendered (S)

- Evidence: `vite.config.ts:1,17` uses `adapter-auto`; every local build prints "Could not detect a supported production environment" and emits nothing deployable.
- Why it matters: the tool is client-side by nature (no accounts, no server data in the brief). A static build is the cheapest to host, fastest to load, and removes a whole class of server concerns. Leaving `adapter-auto` means the first deploy attempt doubles as an architecture decision.
- Suggested fix: swap to `@sveltejs/adapter-static` (Frisp runs 4.0.1 on kit 3), add `src/routes/+layout.ts` with `export const prerender = true`, and host on Cloudflare Pages or Workers static assets like the other Tavlean sites. Move to `adapter-cloudflare` only if a server feature appears (saved stacks, share links by id). Update `docs/project/brief.md` and `docs/gotchas.md` in the same change.

## 3. No tests, linter, formatter or CI; add them with the first real module (S to M)

- Evidence: `package.json:9-16` has only dev, build, preview, prepare and check. There is no `.github/`, no vitest, no prettier.
- Why it matters: the serializers in item 1 are the product's correctness surface (a wrong comma or unit ships broken CSS into someone's codebase), and they are the easiest code in the project to test. Formatting drift starts with the second contributor or agent.
- Suggested fix: add vitest with table tests for `toCss` and `toTailwind` (round trips, zero values, inset, multiple layers, color formats) in the same commit as the model. Add prettier with `prettier-plugin-svelte` using Frisp's settings. Add one GitHub Action that runs `pnpm install --frozen-lockfile`, `pnpm run check`, the tests and `pnpm run build`, with `pnpm/action-setup` before `setup-node`. Then update the gate line in `AGENTS.md`.

## 4. `engine-strict=true` enforces nothing (S)

- Evidence: `.npmrc:1` sets `engine-strict=true`, but `package.json` has no `engines` field, so there is no constraint to be strict about.
- Why it matters: it reads as a guard and is not one. An agent or host on an old Node will fail somewhere less clear.
- Suggested fix: add `"engines": { "node": ">=24.12.0 <25" }` to match Frisp (local Node is 24.22.1). Not verified: whether pnpm 12 still reads `engine-strict` from `.npmrc` or wants it in `pnpm-workspace.yaml`; check with a deliberately wrong range before trusting it.

## 5. Public-facing placeholders that should not ship (S)

- Evidence: `src/routes/+page.svelte:5` meta description is "A SvelteKit foundation for designing and exporting shadow stacks." and line 11 says "Shadow stack generator foundation."; both describe the scaffold, not the product. `src/lib/assets/favicon.svg` is the Svelte logo. `static/robots.txt:2-3` allows all crawling.
- Why it matters: if this is deployed before launch, search engines index a placeholder with another project's logo.
- Suggested fix: write the real title and description with the first real page and replace the favicon. Until launch, either do not deploy or set `Disallow: /`.

## 6. Favicon is inlined into every HTML response (S)

- Evidence: `src/routes/+layout.svelte:2,8` imports the SVG through `#lib`; Vite inlines small assets, so the built page carries the whole icon as a `data:` URI in `<head>` (seen in the preview HTML on 2026-11-19, roughly 3 KB).
- Why it matters: it is repeated in every page and cannot be cached separately. Small now, but it is the template's default and easy to fix while replacing the icon for item 5.
- Suggested fix: put the new icon in `static/favicon.svg` and link it from `src/app.html`; delete the import and the `<svelte:head>` block from the layout.

## 7. Design decisions a shadow tool needs before UI work (M, design session)

- Evidence: `docs/design.md` is a placeholder. `src/app.html` has no `color-scheme` or `theme-color` meta.
- Why it matters: shadows are judged against a surface. The preview background, light and dark surfaces, and the density of the controls are the product's core UX, more than in most apps.
- Suggested fix: in the design session decide light and dark preview surfaces from day one, a user-settable preview background, and whether the reference feel is the layered soft surfaces in the slift-design doctrine. Record the tokens in `docs/design.md` before building screens.

## 8. Small config and hygiene notes (S each)

- `vite.config.ts` is outside the type check: `tsconfig.json` `include` covers only `src/**`, copied from Frisp where including the config crashed svelte-check. Not verified that the crash happens here; try adding `vite.config.ts` once and keep it if check passes.
- `package.json:13`: `prepare` ends with `|| echo ''`, which hides a failing `svelte-kit sync` on install. Template default; harmless today, worth removing once CI exists so a broken sync fails loudly.
- `package.json:18-24` uses caret ranges while Frisp pins exact versions. The lockfile makes installs reproducible either way; pick one house style.
- TypeScript 7.0.2 is available and was held at 6.0.3 to match Frisp. Not verified whether svelte-check 4.7.8 supports 7; move both projects together.
- `src/lib/index.ts` is an empty placeholder; delete it when the first real module lands.
- `docs/lab.md` and `docs/project/roadmap.md` do not exist yet. That is correct for a scaffold; create them with the first lab and the first roadmap session.

## What is fine

The kit 3 setup (config inside `vite.config.ts`, `#lib` subpath import, `$app/tsconfig`), forced runes mode, the pnpm pin, and the docs layout all match the house standard and the Frisp reference. No dead code beyond the placeholders named above, and nothing to measure for performance yet.
