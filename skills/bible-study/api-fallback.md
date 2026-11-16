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

References in `refs` and `q` accept book names in the user's language,
including the singular and common abbreviations ("Salmo 23", "Jn 3:16").

Things the API does differently from the MCP tools:

- **Choose the version yourself.** There is no connection default. Pick from
  `/v1/versions` by `language`, preferring a modern translation for today's
  readers: `bsb` (en), `nva` (pt-br; `almeida-livre` for a traditional Almeida),
  `rvr1909` (es), `luth1912` (de), `lsg` (fr), `riveduta` (it), `cuvs` (zh),
  `synodal` (ru), `kor` (ko). For readers who know a traditional translation
  by heart, use its free edition instead (`almeida-livre`, `kjv`, `rvr1909`):
  `nva` writes the divine name "Yahweh" where Almeida has "SENHOR". The
  entry's `shortName` (`BSB`, `NVA`) is the label for quotes; its `slug` goes in
  requests and links.
- **Version swaps are announced.** A version with all rights reserved (`nvi`,
  `niv`, `esv`) is served by a free one in the same language, and the response
  carries `meta.versionNotice` (`requested`, `served`; its `message` is in
  English). Name the served version and tell the reader, in their language,
  that their usual translation was not available.
- **Book names follow the version's language** (`bookName`, `reference`):
  `nva` answers "Romanos 8:28". Add `&locale=en` (or another of the 9 site
  languages) to get them in a different one.
- **Verse boundaries.** Besides the joined `text`, each passage has a `verses`
  array, one string per verse starting at `verse`; quote from it when you need
  single verses.
- **Copyright** arrives in `meta.copyright`, and in each `/v1/versions` entry.
- **No `reader_url`.** Build the link from the version slug, the English book
  slug (`book` in the response) and the chapter:
  `https://midvash.com/{version}/{book}/{chapter}`, plus `#v={verse}` or
  `#v={first}-{last}` for verses.
- **Comparing versions** works: fetch the same verse once per version.
- **No commentary, cross-references, Strong's or study library.** Interpret
  from the passage and its chapter, and fetch the cross-references you can name.
  Skip the original-language step, leave out commentary links, and say the
  work draws on the text alone (anything from general knowledge is marked as
  such).
