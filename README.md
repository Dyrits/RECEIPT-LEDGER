# Receipt Ledger

Open source under AGPL-3.0. Created and maintained by Dyrits.

> **What this is, and is not.** Receipt Ledger is a local, rule-based reconciliation tool: PDF text extraction, on-device OCR and deterministic payment matching. It does **not** use generative AI or any model API. It is **not** an official filing service, it does not connect to URSSAF or any bank, and it is **not** tax or accounting advice. You review and decide everything.

A browser-first, local-first invoice and payment reconciliation workspace. No Python, bank login, API key or backend is needed to use the deployed app. It prepares review reports. It does not file declarations, make payments or certify tax treatment.

## Use

1. Open the deployed HTTPS app in a modern browser. Use one tab for the workspace.
2. Add invoices and bank statements as PDF, PNG, JPEG, WebP, CSV or structured JSON. PDF text extraction and English/French OCR run on the device using bundled assets. Documents are identified by SHA-256 and kept with their original bytes, extracted text and import history. Reimporting identical bytes does not rerun extraction.
3. Open each document. Compare suggested fields with the rendered original. Correct missing or wrong fields. Choose the right activity category yourself. Verify bank-row completeness and signs. Optional opening/closing balances check that reviewed bank rows reconcile. Archive irrelevant documents explicitly.
4. Review payment matches. Exact references, partial payments and grouped references receive suggestions, never automatic approval. Allocate gross receipts to invoices and explain differences, retained fees or exclusions. Missing invoices do not justify excluding credits. Add off-bank receipts separately.
5. Choose the receipt month or quarter. Resolve all period transactions and pending documents, then confirm outside accounts and review checks. Download the review CSV and gated reviewed JSON report.
6. Back up regularly in Settings. The backup includes original document bytes, extraction, records, decisions and history. Restore replaces the current device workspace after confirmation.

The included sample workspace uses synthetic records. Use it to understand the workflow before importing any sensitive data. The app has automated tests, but has not had independent accounting, accessibility or security certification.

## Extraction and import limits

- 20 MB per document, up to 40 pages for text PDFs or 12 pages for scanned PDFs. 500 combined structured rows per import validation. English and French OCR are bundled locally.
- Invoice suggestions use labelled fields and common English/French total/date labels. Unknown fields stay blank. You can enter them against the source rather than relying on fabricated data.
- Statement suggestions recognize ISO/French dated rows and explicit pipe-separated rows. Real banks have many layouts: signs, multiline descriptions, separate debit/credit columns and missed rows need review. Use CSV mapping for bank exports, including semicolon/tab delimiters, French date/amount formats and optional separate credit/debit columns. Review source-row previews and completeness before import.
- The app does NOT promise automatic extraction for every invoice or bank. Unknown layouts are usable through the source review/editor or CSV mapping, not silently treated as verified.
- Structured JSON uses arrays named `invoices` and `transactions`. See fixtures/example-bundle.json. Import is additive; duplicate IDs or duplicate document hashes are blocked.
- EUR only. Each invoice has one proportional HT/TTC relationship and one activity category. Mixed-category invoices, complex VAT, foreign exchange, credit notes, statutory rounding and cross-period refund corrections require separate review. Refunds/debits are never auto-subtracted from positive turnover.
- The reviewed total is gross received excluding VAT. Ordinary expenses/commissions do not reduce micro-social turnover. Confirm your own status and treatment using current rules. Official source: https://www.autoentrepreneur.urssaf.fr/portail/accueil/une-question/toutes-les-fiches-pratiques/determiner-mon-chiffre-daffaires.html

## Privacy and security

All default processing and storage are local. No analytics, model API, bank connector, login service or document-upload endpoint exists. Local fonts, OCR language data and workers are bundled; no runtime CDN is needed. A public static deployment exposes only generic application assets, not browser workspace data.

IndexedDB is not encrypted. Anyone with access to the browser profile may access the workspace. Browser eviction, private mode or clearing site data can remove it. Backups and downloaded reports contain sensitive information: store them safely. Clearing the workspace does not delete downloaded backups. Hosting providers can see normal asset requests, IP addresses and access logs. The app's privacy does not override a device or host compromise.

PDF.js uses evaluation disabled and XFA disabled; original PDFs are rendered on canvas, not embedded as active plugins. React renders document strings as text. CSV exports escape common spreadsheet formula prefixes. Inputs have size/count limits; duplicate IDs, invalid dates, unsafe amounts, over-allocation and incomplete categories are blocked. A browser Web Lock prevents concurrent editing tabs where supported. A restrictive `_headers` file is included for compatible static hosts. It blocks remote network destinations; no external BYOK provider call can occur in this build.

BYOK is not included in the browser app. It is unnecessary for default extraction and matching. A future model adapter would require explicit document-disclosure/cost consent, secure ephemeral key handling, verified provider policies and live output testing. Do not claim this build has a tested AI integration.

## Develop

Node 22+ recommended. `npm ci`, `npm run dev`, `npm run build`, `npm test`. The Vite production output is `dist/`. Serve at the root of an HTTPS origin; PDF/OCR paths are root-relative. Opening index.html via file:// is not supported. All extraction workers and language assets are bundled in public/ocr. No production secret is required.

## Deploy

Upload only `dist/` to a root static host. `_headers` is interpreted by compatible hosts such as Cloudflare Pages / Netlify; verify response headers on the selected host. Do not put source backups, real documents or test-downloads into the public deployment. Source package excludes node_modules, private test data, logs, tool output and credentials.

## Tests

`npm test`: twenty-one unit tests for cents, CSV syntax, date and locale parsing, quarter boundaries, invoice/bank candidate extraction, unknown formats, matching, cumulative allocation, categories and export escaping.

`node acceptance.mjs`: launches local Vite and headless Chrome. Covers sample exact/partial/grouped approvals, HT total, IndexedDB reload, six screens at 320/390/1280 with no page overflow, reviewed report gate, actual downloaded JSON/CSV contents, backup/clear/restore, PDF text pair, local OCR, duplicate-document cache, CSV mapping duplicate-ID block and original PDF bytes in backup. Evidence screenshots are generated under evidence/; not deployed.

Security dependency audit at build time: `npm audit --omit=dev` returns zero known vulnerabilities. This is not an independent security audit.

## Licensing

Receipt Ledger is free software under the GNU Affero General Public License, version 3 only (AGPL-3.0-only). See LICENSE. Copyright (C) 2026 Dyrits.

In short: you may use, study, modify and redistribute it, including commercially. If you distribute it, or let users interact with a modified version over a network, you must make the corresponding source of your version available to them under the same license (AGPL section 13). The software is provided without warranty of any kind; see LICENSE sections 15 and 16.

Third-party components keep their own licenses. See licenses/ and THIRD_PARTY.md. Fonts are open-source DM Sans and Libre Baskerville under the SIL Open Font License.

## Contributing and safety

Issues and pull requests are welcome; contributions are accepted under the same AGPL-3.0-only license. Never commit or attach real invoices, bank statements, backups or exports to this repository, issues or pull requests. Use only synthetic data like the files in `fixtures/`.
