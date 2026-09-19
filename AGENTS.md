# Repository Guidelines

## Project Structure & Module Organization

This is a TypeScript Electron desktop application built with Electron Forge and Vite.

- `src/main.ts` is the Electron main-process entry point; native integrations live in `src/native/`.
- `src/world/` contains window and configuration logic for the web-facing world, while `src/renderer.ts`, `src/preload.ts`, and `src/index.css` support the renderer and preload layers.
- `assets/` is a Git submodule containing Stoat branding used by packaging; initialize it before building.
- `for-web/` is the frontend Git submodule; its `packages/client/dist/` output is bundled into the desktop app.
- `forge.config.ts` and `vite.*.config.ts` define packaging and process-specific builds.
- `.mise/tasks/` provides the preferred developer task wrappers; `.github/workflows/` contains CI and release automation.

## Build, Test, and Development Commands

Use the pinned tools from `mise` (`mise install`) and install dependencies with `mise install:frozen`.

```bash
mise assets             # Initialize/update branding assets
mise dev                # Start Electron in development mode
mise lint               # Run ESLint
mise format             # Check Prettier formatting
mise build              # Create the packaged app bundle
mise make               # Build distributable targets
```

`mise build` builds the `for-web` dependencies and Solid.js frontend before packaging Electron. Use `cd for-web; mise dev` for frontend hot reload, then start the desktop app with `pnpm start -- --force-server http://localhost:5173`.

Equivalent package scripts are available through `pnpm`, including `pnpm start`, `pnpm package`, and `pnpm make`. For a local Flatpak check after `make`, use `pnpm install:flatpak` and `pnpm run:flatpak`.

## Coding Style & Naming Conventions

Use TypeScript with two-space indentation and no tabs. Run Prettier before committing; imports are sorted by the configured Prettier import-order plugin. Follow ESLint recommendations, use `camelCase` for variables/functions, `PascalCase` for classes and types, and descriptive lower-case filenames consistent with existing `src/` files. Prefix intentionally unused variables with `_`.

## Testing Guidelines

There is currently no dedicated unit-test framework or test script. Before opening a change, run `mise lint`, `mise format`, and `mise build`; manually exercise affected Electron/native behavior when relevant. CI performs the same checks and also builds on supported platforms.

## Commit & Pull Request Guidelines

Use Conventional Commit-style subjects such as `fix: ...`, `chore: ...`, or `feat: ...`; release automation relies on this history. Keep commits focused. PRs should explain the user-visible change, include reproduction or validation steps, link related issues, and attach screenshots or recordings for UI changes. Ensure the PR title passes semantic pull-request validation and all CI checks are green.

## Configuration & Packaging Notes

Do not commit generated output such as `.vite/`, `out/`, or local configuration. Keep the `pnpm-lock.yaml` in sync with dependency changes. Changes affecting native integrations, packaging targets, or release workflows should be tested on the relevant operating system when possible.
