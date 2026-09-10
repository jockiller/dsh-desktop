# DSH Desktop Instructions

## Project Overview

DSH Desktop is a cross-platform desktop launcher/manager for DeepSeek Harness (DSH) Web services.

- **Stack**: Tauri 2, Rust 2024, React 19, TypeScript, and Vite.
- **Frontend**: `src/App.tsx`, `src/i18n.ts`, `src/styles.css`.
- **Backend**: `src-tauri/src/`.
- **Packaging scripts**: `scripts/`.

## Verification Commands

Run targeted type-checking and compilation checks before proposing changes:

```bash
# Frontend type check & build
pnpm run build
# or: ./node_modules/.bin/tsc --noEmit

# Rust / Tauri backend check & tests
cargo check --manifest-path src-tauri/Cargo.toml --offline
cargo test --manifest-path src-tauri/Cargo.toml --offline
```
Do not launch `pnpm dev` or `pnpm tauri dev` background servers during automated verification.

## Engineering Standards

### Rust & Tauri Backend (`src-tauri/`)
- **Process Management**: Track all spawned child processes and ensure graceful termination on exit; never leave orphaned process groups.
- **Resilience**: Keep background observers/watchers isolated so an error does not crash the core application.
- **Async Runtime**: Move blocking I/O (network requests, filesystem sweeps, process waits) off the main/UI thread.
- **API Errors**: Tauri command handlers should return user-friendly `Result<T, String>` errors.

### Frontend (`src/`)
- **i18n Compliance**: Every user-visible string must be added to both `zhDict` and `enDict` in `src/i18n.ts`.
- **Backend Message Translation**: Pass backend return messages through `translateBackendMessage()`.
- **Theme & Styles**: Preserve both light and dark theme CSS variables in `src/styles.css` and maintain accessible modal focus.
