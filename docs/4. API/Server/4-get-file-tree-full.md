---
id: get-file-tree-full
title: Get file tree (full match)
slug: /api/server/get-file-tree-full
---

# Get file tree

Returns repository URLs for every file in the source tree for the desired chain and address. Searches only for full matches.

**URL** : `/files/tree/:chain/:address`

**Method** : `GET`

## Responses

**Condition** : Contract is available in the repository.

**Code** : `200 OK`

**Content** :

```json
[
  "https://repo.sourcify.dev/contracts/full_match/5/0x1fE5d745beABA808AAdF52057Dd7AAA47b42cFD0/metadata.json",
  "https://repo.sourcify.dev/contracts/full_match/5/0x1fE5d745beABA808AAdF52057Dd7AAA47b42cFD0/sources/browser/ERC20Standard.sol"
]
```

---
**Condition** : Contract is not available in the repository.

**Code** : `404 Not Found`

**Content** :

```json
{
  "error": "Files have not been found!"
}
```
