# Restless Client

A strict native, non-Chromium API client prototype written in Rust.

This starter intentionally avoids Electron, Tauri, CEF, WebView2, and Chromium. It uses `eframe/egui` for the native desktop UI, `reqwest` for HTTP, `tokio` for async execution, `rusqlite` for local persistence, and `tokio-tungstenite` for WebSocket support.

## Current status

Implemented so far:

- Multi-request tabs
  - HTTP tabs
  - GraphQL tabs
  - WebSocket tabs
  - Compact styled tab bar
  - New request buttons moved to the header: `+ HTTP`, `+ GQL`, `+ WS`
  - Close selected tab
  - Close other tabs
  - Close all tabs
  - Full request editor hides cleanly when every tab is closed
  - Duplicate tab
  - Dirty-state indicator
  - Rename saved request
- Workspaces and collections
- Collection folders
  - Saved requests can target the collection root or a folder
  - Folder records include `sort_order` metadata for future drag/reorder support
- Native file-dialog import/export
  - Postman Collection JSON import/export
  - OpenAPI JSON import/export
- Request editor
  - Query params
  - Auth helpers
  - Headers
  - Body editor
  - JSON pretty/minify actions
  - Lightweight JSON syntax highlighting for JSON editors
- GraphQL mode
  - Query editor
  - Variables JSON editor
  - Pretty/minify variables
- WebSocket mode
  - Persistent connect/send/disconnect session
  - Session log
  - WebSocket auth/query/header support
- Professional desktop UI
  - Icon-only dark/light mode switch
  - Persisted appearance preference
  - Styled top bar, sidebar, compact tab bar, cards, status chips, and response panels
  - Softer light theme background
  - Icon-labeled header actions for new HTTP, GraphQL, and WebSocket tabs
  - Top status banner removed for a cleaner header
  - Save target selector aligned with the save buttons
  - Header and tab strip constrained to avoid horizontal overflow
- Response viewer
  - Status summary
  - Response headers available from a compact headers menu beside the status summary
  - Pretty JSON response view with syntax colors
  - Raw response view
  - JSONPath Plus-style response filter
    - Wildcard selection
    - Numeric filters
    - Negative slices
    - `.length` helper
    - `.match(/pattern/i)` regex filters
    - Bracketed multi-field projection such as `$.[_links.show.name,season,number,name]`
    - Multi-field projections return value arrays rather than rebuilding the original object structure
  - Copy response body
  - Copy minimal cURL command
- Request history
  - Recent executions
  - Detail pane with full request/response snapshot
  - Open history snapshot as a new request tab
- SQLite local persistence
  - Workspaces
  - Collections
  - Folders
  - Saved request drafts
  - Environments
  - Request history snapshots
  - App settings such as selected appearance theme

## Run locally

```bash
cargo check
cargo run
```

## Build release binary

```bash
cargo build --release
```

## Notes

- The project currently pins `eframe = 0.35.0`.
- The project uses `reqwest = 0.13.4` with the `rustls` feature.
- OpenAPI import currently expects JSON, not YAML.
- Drag/reorder for folders and requests is intentionally deferred, but the database now contains sort metadata to support that later.
- JSONPath filtering is implemented natively in Rust for the app's current response-filtering needs. It supports the listed JSONPath Plus-style examples, but it is not a full embedded Node/jsonpath-plus runtime.
- I could not compile-check this package inside the generation environment because Rust/Cargo is not installed there. Run `cargo check` locally first.

## Suggested next work

1. Add request-level folders/actions context menus.
2. Add real drag/reorder for folders and saved requests.
3. Add collection runner with batch execution.
4. Add OpenAPI YAML support.
5. Add request/response diffing.
6. Add certificate/proxy settings.
7. Add plugin sandbox with Rhai or WASM.
