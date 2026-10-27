# Bundled third-party components

- React and React DOM: MIT.
- Vite: MIT (build tool, not a server requirement).
- PDF.js / pdfjs-dist: Apache-2.0. Bundled worker and application parser.
- Tesseract.js: Apache-2.0. Tesseract.js-core / Tesseract: Apache-2.0.
- English and French traineddata from @tesseract.js-data packages: Apache-2.0 / Tesseract language data.
- Lucide icons: ISC. See licenses/lucide.txt.
- Other transitive packages in package-lock.json: MIT, ISC, Apache-2.0 (idb-keyval, wasm-feature-detect), BSD-3-Clause (source-map-js, build tool) and BSD-2-Clause (webidl-conversions). Each package's own license file ships with its npm package.
- DM Sans and Libre Baskerville fonts: SIL Open Font License 1.1, copied under licenses/.

Package versions and integrity are pinned in package-lock.json. Dependency license files are copied under licenses/ where available. See upstream projects for full notices and source.

The application code itself is licensed under AGPL-3.0-only (see LICENSE). All components above are permissive licenses compatible with AGPL-3.0 distribution.
