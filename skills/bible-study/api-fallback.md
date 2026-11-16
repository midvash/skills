# Using the REST API when the MCP server is not connected

`https://api.midvash.com` serves the same text: 119 public-domain and openly
licensed versions, no key, no sign-up. Fetch it with whatever HTTP tool you
have (`curl`, a fetch tool).

| Need | Request |
|---|---|
| Several passages at once (up to 50) | `GET /v1/passages?refs=romans 8:28-30,psalms 23&version=bsb` |
| A chapter | `GET /v1/{version}/{book}/{chapter}` — `/v1/bsb/romans/8` |
| A verse or range | `GET /v1/{version}/{book}/{chapter}/{verse}` — `/v1/bsb/romans/8/28-30` |
| Versions in a language | `GET /v1/versions` (filter on `language`) |
| Parse a free-text reference | `GET /v1/parse?q=João 3:16` |

References in `refs` and `q` accept book names in the user's language.

Things the API does differently from the MCP tools:

- **Pass a version the catalog has.** A slug it does not know (`nvi`, `niv`,
  `esv`) silently falls back to another version — check the `version` field in
  the response and name that one. Common picks: `bsb` (en), `almeida-livre` or
  `nva` (pt-br), `rvr1909` (es), `luth1912` (de), `lsg` (fr), `riveduta` (it),
  `cuvs` (zh), `synodal` (ru), `kor` (ko).
- **Copyright** arrives in `meta.copyright` on single-passage requests.
- **No `reader_url`.** Build the link from the response's `version`, `book` and
  `chapter`: `https://midvash.com/{version}/{book}/{chapter}`, plus `/{verse}`
  for a single verse.
- **No commentary, cross-references or study library.** Do interpretation from
  the passage's own context and the cross-references you can name and then
  fetch; say the study draws on the text alone.
