# Using the REST API when the MCP server is not connected

`https://api.midvash.com` serves the same text: 119 public-domain and openly
licensed versions, no key, no sign-up. Fetch it with whatever HTTP tool you
have (`curl`, a fetch tool).

| Need | Request |
|---|---|
| Several passages at once (up to 50) | `GET /v1/passages?refs=romans 8:28-30,psalms 23&version=bsb` (each item carries its own `reference`) |
| A chapter | `GET /v1/{version}/{book}/{chapter}` — `/v1/bsb/romans/8` |
| A verse or range | `GET /v1/{version}/{book}/{chapter}/{verse}` — `/v1/bsb/romans/8/28-30` |
| Versions in a language | `GET /v1/versions` (filter on `language`) |
| Parse a free-text reference | `GET /v1/parse?q=João 3:16` |

References in `refs` and `q` accept book names in the user's language,
including the singular and common abbreviations ("Salmo 23", "Jn 3:16").

Things the API does differently from the MCP tools:

- **Choose the version yourself.** There is no connection default. Pick from
  `/v1/versions` by `language`. Today's readers get the modern one; readers who
  know a traditional translation by heart get the traditional one:

  | Language | Modern | Traditional |
  |---|---|---|
  | en | `bsb` | `kjv` |
  | pt-br | `nva` | `almeida-livre` (or `alm1911`, the 1911 Almeida many know by heart, in 1911 spelling) |
  | es | `pdt` | `rvr1909` |
  | de | `schl1951` | `luth1912` |
  | fr | `ncl` | `lsg` |
  | zh | `ccb` | `cuvs` |
  | it, ru, ko | `riveduta`, `synodal`, `kor` (one choice each) | |

  The `shortName` (`BSB`, `NVA`), in each passage's `meta` and in the
  `/v1/versions` entry, is the label for quotes; the `slug` goes in requests.
- **Quote the text as it comes.** Some versions write the divine name as a
  transliteration (`nva` "Yahweh", `pdt` "ʼElohim") or mark words the
  translators added with [brackets]. Keep them, and add a line for readers
  when it would surprise them.
- **Version swaps are announced.** A version with all rights reserved (`nvi`,
  `niv`, `esv`) is served by a free one in the language that slug belongs to, and
  the response carries `meta.versionNotice` (`requested`, `served`; its
  `message` is in English). A slug can belong to another language than the
  reader's: `nvi` is the Portuguese NVI and answers in Portuguese, so for a
  Spanish reader asking for "NVI" use `pdt` or `onbv-es`. Always check that the
  served version is in the reader's language, name it, and tell the reader, in
  their language, that their usual translation was not available.
- **Book names follow the version's language** (`bookName`, `reference`):
  `nva` answers "Romanos 8:28". Add `&locale=en` (or another of the 9 site
  languages) to get them in a different one.
- **Verse boundaries.** Besides the joined `text`, each passage has a `verses`
  array, one string per verse starting at `verse`; quote from it when you need
  single verses, and number from it when the passage is read aloud (`text` has
  no verse numbers). A verse can end mid-sentence: quote on to the end of the
  sentence.
- **Copyright** arrives in `meta.copyright`, and in each `/v1/versions` entry.
- **Links:** each passage (and each item of `/v1/passages`) carries `readerUrl`,
  the reader's address in the version's language
  (`https://midvash.com/pt-br/nva/salmos/23#v=1-2`). Link quotes with it.
- **Comparing versions** works: fetch the same verse once per version.
- **No commentary, cross-references, Strong's or study library.** Interpret
  from the passage and its chapter, and fetch the cross-references you can name.
  Skip the original-language step, leave out commentary links, and say the
  work draws on the text alone (anything from general knowledge is marked as
  such).
