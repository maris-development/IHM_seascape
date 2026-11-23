# Data discovery

Before carrying out this workflow, complete the shared **Source discovery and selection** step in `AGENTS.md`. Use the node scope confirmed there. The candidate list is preliminary; it does not replace the full table overview described below.

Purpose: give the user a clear overview of the available data, so that they can formulate a good research question.

Data discovery is read-only and lightweight: no notebook, no analysis, no plots. Use only metadata queries (metadata, `LIMIT`, `MIN/MAX`) and avoid full counts on large tables.

## Actions
1. **Scope.** Use the node scope confirmed in the shared Source discovery and selection step. If the user has not narrowed it, use BeaconIHM, beaconwod and beaconargo.
2. **Discover.** Use MCP metadata tools to list all tables within the confirmed node scope. Never guess names. Show WADAR and RWS_CTD first, then other tables with environmental content, then the rest. The preliminary candidate list from the shared step is not a substitute for this full table overview.
3. **Deliver CSV 1 (default).** Create `beacon_tabellen_overzicht.csv` in Dutch with these columns, in this order:
   `node;tabel;groep;bronbeschrijving;viewer_link`
   - `groep`: the group or theme of the table. Take it from MCP metadata if available; otherwise derive a theme from the table's content and mark it as an interpretation. Use `niet vastgesteld` if neither is possible.
   - `bronbeschrijving`: a short Dutch description of what the table contains and represents. Use `niet te verifiëren` if no metadata is available.
   - `viewer_link`: verified IHM viewer URL(s) for spatial tables, following the shared IHM viewer links rules in `AGENTS.md`; leave blank if no layer is verified.
4. **Offer CSV 2 (optional).** Ask the user whether they want column and parameter detail per table, and wait for the answer. Do not continue on your own. If yes, create `beacon_kolommen_detail.csv` with these columns, in this order:
   `node;tabel;kolom;type;betekenis;viewer_link`
   Put verified IHM viewer URL(s) in `viewer_link` for spatial tables, following the shared IHM viewer links rules in `AGENTS.md`; leave blank if no layer is verified.
5. **Chat summary.** In chat, give a short summary: the main tables per node (WADAR and RWS_CTD first).
6. **Hand-off.** End by inviting the user to pick or refine a question and, if they want, to ask for a notebook (Notebook generation).

## CSV format
- Separator `;`, encoding UTF-8 with BOM (so that Dutch Excel opens the columns correctly), header row, one row per table (CSV 1) or per column/parameter (CSV 2).
- Quote fields that contain a semicolon, quote or line break.
- Deliver the files to the user with a short context line. If a file cannot be delivered in the environment, say so and give the closest usable alternative (e.g. the table in chat).
