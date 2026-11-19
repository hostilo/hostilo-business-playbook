# Publishing with the Hostilo connector or REST API

Connect Hostilo in Claude (Connectors directory, "Hostilo"), ChatGPT (Apps, "Hostilo"), or any MCP client at `https://hostilo.app/mcp` (OAuth). REST API with a key `hk_…`: `https://hostilo.app/api/v1/<action>`, OpenAPI at `https://hostilo.app/openapi.json`.

## Tools

| Tool | Use |
|---|---|
| `list_sites` | find the site (id or name) |
| `get_site` | without `path`: home page HTML, list of all files, form details and (Business) the site memory with rules and recent changes. With `path`: one text file (`en/about.html`, `assets/site.css`, `_memory/DESIGN.md`, `_posts/pl/x.html`) |
| `publish_files` | publish files to an existing site. Only a file named `index.html` changes the home page; without it only the files you send are added or replaced. HTML pages keep their folders and get clean URLs; other files go to `assets/` with folders kept and links are fixed. Binary files as base64. `dry_run` previews. |
| `update_site` | replace only the home page HTML |
| `import_zip` | a whole project from a ZIP (base64 or public URL) |
| `create_site` | new site (html, files or a Hostilo template) |
| `list_versions` / `restore_version` | home page versions; on Business also full-site snapshots `snap-…` (restore the whole site) |
| `get_site_stats` / `get_ai_traffic` | visitors and AI bots |

## Rules

- **Read before you write.** `get_site` first (memory and file list), then `get_site` with `path` for each file you change. Never rebuild a page from memory or from a screenshot.
- **Publish only changed files**, all related files in one call (e.g. a new page in 3 languages + menu changes).
- **Always pass `note`**: one sentence, what and why ("Added Infrastruktura 2026 post in PL/EN/DE with booth photo"). It goes to the change log that the next AI reads.
- If a response says `TRUNCATED`, the file was cut: do not publish it back.
- Pass `expected_version` (from `get_site`) when several people work on the site; on `version_conflict` read again and merge.
- `clear_assets: true` deletes all files in `assets/` before publishing: only in a full redesign that re-sends every asset.
- Uploaded photos (`assets/uploads/…`) are never deleted by normal publishes.
- After publishing, open the live URL and run the checklist.

## Snapshots and undo (Business)

Hostilo takes a full-site snapshot (all pages, assets, memory) automatically before a publish of 5+ files, with `clear_assets`, or with template changes. `list_versions` shows them first; `restore_version` with `snap-…` restores everything and keeps the current state as a new snapshot, so restoring can be undone too.
