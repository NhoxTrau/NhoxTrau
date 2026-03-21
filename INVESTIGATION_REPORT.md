# 🔍 CTF Investigation Report — tamuctf/phantom

> **Target file**: [`https://github.com/tamuctf/phantom/blob/main/README.md`](https://github.com/tamuctf/phantom/blob/main/README.md)  
> **Author (cobra)**: `cobradev4` (Noah Mustoe) — member of `@tamuctf` + `@tamu-edu-students`  
> **Issue #2**: https://github.com/tamuctf/phantom/issues/2  
> **Scope**: tamuctf/phantom only  
> **Date**: 2026-03-21  

---

## 🎯 Target: tamuctf/phantom → README.md

### File Content (exact)

```
# phantom
```

- **Size**: 9 bytes (NO trailing newline — confirmed with `\No newline at end of file` diff marker)
- **Blob SHA**: `aa7a5b445c490056c256153ca186fdae8b61281e`
- **Hex**: `23 20 70 68 61 6e 74 6f 6d`
- **Unicode check**: All standard 7-bit ASCII, no zero-width or lookalike characters
- **Verified**: `SHA1("blob 9\0# phantom") = aa7a5b445c490056c256153ca186fdae8b61281e` ✓

---

## 📦 Complete Git Object Inventory

All objects in the tamuctf/phantom repository (including PR #3):

| SHA | Type | Size | Content |
|-----|------|------|---------|
| `35ecf7068d2aa0bc295f69cfc622c7904dab42cf` | commit | 1023 B | "Initial commit" + PGP sig |
| `483cd4d468d002123cff4dc29645e032216c2302` | tree | 37 B | `100644 README.md → aa7a5b4` |
| `aa7a5b445c490056c256153ca186fdae8b61281e` | blob | **9 B** | `# phantom` |
| `5579d8d89ec47e7446312199ac3c307d1ff3a474` | commit | 1063 B | PR head "Fix header formatting" |
| `5d9a91ee1ddd1a37adbb7dd85faaa1ef940d4d29` | tree | 37 B | `100644 README.md → 3664c63` |
| `3664c63304aecba872e4840bfe64fe0eb92690a0` | blob | **11 B** | `# phantom ` + `\n` (trailing space!) |
| `0862ea7f92f15bcf4290f3a046f0f3cfca053a84` | commit | 1170 B | PR merge commit |

**Total: 7 objects.** No dangling/orphaned/phantom objects found **in our clone**.

---

## 🔎 Git References (Complete)

```
35ecf7068d2aa0bc295f69cfc622c7904dab42cf  HEAD
35ecf7068d2aa0bc295f69cfc622c7904dab42cf  refs/heads/main
5579d8d89ec47e7446312199ac3c307d1ff3a474  refs/pull/3/head
0862ea7f92f15bcf4290f3a046f0f3cfca053a84  refs/pull/3/merge
```

- ✅ No tags
- ✅ No git notes (`refs/notes/*`)  
- ✅ No hidden branches
- ✅ No stash objects
- ✅ Only 1 PR (PR #3)

---

## 📌 Commit Analysis (Initial Commit)

```
commit 35ecf7068d2aa0bc295f69cfc622c7904dab42cf
tree   483cd4d468d002123cff4dc29645e032216c2302
author Noah Mustoe <62711423+cobradev4@users.noreply.github.com>
       1773625935 -0500   ← 2026-03-16T01:52:15Z (CDT, Texas)
committer GitHub <noreply@github.com>
message: "Initial commit"
PGP signed by GitHub key: B5690EEEBB952194
```

- Commit SHA `35ecf7068d` — no visible ASCII/flag encoding in hex pairs
- Timestamp `1773625935` = `0x69B7624F` — no flag encoding found
- Single parent: none (root commit)
- No parent commit = no "deleted" history

---

## 🔍 PR #3 Analysis — Key Finding!

**PR Title**: "Fix header formatting in README.md"  
**Author**: Kahunser (`kahunser@proton.me`)  
**Status**: Open, not merged

**Diff**:
```diff
-# phantom
\ No newline at end of file
+# phantom 
```

> ⚠️ **The PR changes README from `# phantom` (9 bytes) to `# phantom ` + newline (11 bytes).**  
> This adds a **trailing space** before the newline!  
> In Markdown, trailing space on a heading line has no effect — this change is cosmetic only.  
> The new blob SHA is `3664c63304aecba872e4840bfe64fe0eb92690a0`.

---

## 📣 Issues Analysis

### Issue #1 — "Bug Check #0001"
- Created by: `TrooperZ`
- Body: *"Happy to report there are no bugs... mainly because there is effectively no implementation."*
- Comment (by CupNudous): *"Where flag bruh"*
- Reactions: 19 total (12 👍 + 7 😄)

### Issue #2 — "CRITICAL: No flag, pls fix" ⭐
- Created by: `02loveslollipop`  
- Body: Humorous bug report demanding a flag (or fake flag, treasure map, etc.)
- **KEY COMMENT** (by `Pokebro278`, 2026-03-21T05:18:52Z): **"Cope cus I found it"**
- Reactions on comment: 4 👀 (people watching)
- > **Someone found the flag!** This confirms it IS findable.

---

## 🕵️ Flag Location Hypotheses (Ranked)

### 🥇 #1 — Phantom/Orphaned Git Object (MOST LIKELY)

**Theory**: The challenge name "phantom" is a direct hint to git's concept of *phantom commits* — orphaned/dangling objects not reachable from any ref.

**Attack surface**: GitHub retains unreachable objects for ~30 days before garbage collection.

**Scenario**:
1. cobradev4 initially committed with flag in README
2. Used `git commit --amend` or `git reset` to create clean "# phantom" commit
3. Force-pushed to GitHub: old flag commit became **unreachable**
4. The orphaned commit still lives on GitHub's server, accessible by SHA

**How to find the phantom SHA**:
```bash
# With authenticated GitHub API access:
curl "https://api.github.com/repos/tamuctf/phantom/events" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  | jq '.[] | select(.type=="PushEvent") | .payload.before'
```
→ The `before` SHA from the push event is the orphaned "phantom" commit.

**Once you have the SHA**:
```bash
git clone https://github.com/tamuctf/phantom.git
cd phantom
git fetch origin <PHANTOM_SHA>
git show <PHANTOM_SHA>:README.md
# → Flag revealed here
```

> ⚠️ **This approach requires a GitHub OAuth token** (rate-limited to 60 req/hr without auth).
> The Events API returns only the last 10 events with auth.

---

### 🥈 #2 — Repository Metadata (Requires GitHub Auth)

The following fields are NOT accessible without a GitHub account:
- Repository **description** (`null` in default view)
- Repository **topics** (might include `gigem{...}`)
- Repository **custom social preview image** (none detected)

---

### 🥉 #3 — Commit SHA as Flag

`gigem{35ecf7068d2aa0bc295f69cfc622c7904dab42cf}` — the initial commit SHA  
or  
`gigem{35ecf706}` — the short SHA

---

### #4 — Simple Flag from Challenge Name

`gigem{phantom}` — the simplest possibility

---

## 🛠️ Complete Investigation Checklist

### Git & Repository
- [x] Full git pack file analysis (`xxd` level inspection)
- [x] All 7 objects enumerated and inspected
- [x] All refs scanned (`git ls-remote --refs origin`)
- [x] Git notes scan (`refs/notes/*`) — none found
- [x] Git stash — none found
- [x] Tags and releases — none found
- [x] PR #3 head and merge commits — inspected
- [x] Dangling/orphaned object check (`git fsck --full`) — none in local clone
- [x] Deep fetch (`git fetch origin "+refs/*:refs/backup/*"`) — no new refs

### File Content
- [x] README.md raw bytes (hex level): `23 20 70 68 61 6e 74 6f 6d` = clean ASCII
- [x] Unicode/lookalike character check — clean
- [x] Hidden null bytes or zero-width chars — none
- [x] Trailing space in PR blob — confirmed (`# phantom \n`)

### GitHub Metadata
- [x] Repository HTML page scraped — no hidden HTML comments
- [x] Rendered README HTML — just `<h1>phantom</h1>` 
- [x] File metadata: `1 lines (1 loc) · 9 Bytes`
- [x] Social preview image — default (no custom image)
- [x] Repository ID: `1182824794`

### Author Analysis
- [x] `cobradev4` confirmed member of `@tamuctf` + `@tamu-edu-students`
- [x] cobradev4 repos: `nsa-codebreaker-2024`, `block-mechanic`, `cobradev4` profile — no flags
- [x] cobradev4 GitHub profile README — default "Hi there 👋"
- [x] Pull Shark achievement (×3) — active PR contributor

### Organization Analysis
- [x] tamuctf has 24+ public repos, checked misc/git-related ones
- [x] tamuctf/vanity — "don't be so vain!" (SHA mining challenge from 2022) — README says "nothing to see here"
- [x] tamuctf/tamuctf-2025 — past year challenges inspected
- [x] GitHub Events API — blocked (403 without auth)
- [x] Forks list (46 forks) — blocked without auth

### Techniques Tried
- [x] Commit SHA decoding (ASCII, base64, ROT13, hex)
- [x] Timestamp encoding analysis
- [x] SHA null-byte analysis
- [x] Git dumb HTTP protocol — blocked (GitHub deprecated)
- [x] Git upload-pack low-level protocol
- [x] Direct object URL access — blocked

---

## 🚨 Critical Next Step

**To definitively find the flag, authenticate with GitHub and run**:

```bash
# Step 1: Get push events (requires GitHub token)
curl -H "Authorization: Bearer <TOKEN>" \
  "https://api.github.com/repos/tamuctf/phantom/events" | \
  python3 -c "
import json, sys
for e in json.load(sys.stdin):
    if e['type'] == 'PushEvent':
        print('before:', e['payload']['before'])
        print('head:', e['payload']['head'])
"

# Step 2: Fetch the phantom commit by its SHA
cd /path/to/phantom_clone
git fetch origin <BEFORE_SHA>
git cat-file -p <BEFORE_SHA>
git show <BEFORE_SHA>:README.md
```

---

## 📝 Raw Data

### Commit SHA Analysis
```
35 ec f7 06 8d 2a a0 bc 29 5f 69 cf c6 22 c7 90 4d ab 42 cf
```
No ASCII flag pattern found in any position.

### Timestamp
- `1773625935` = `0x69B7624F` = `2026-03-16T01:52:15 UTC`
- No flag encoding detected

### Related tamuctf Repos
- `tamuctf/vanity` (2022): SHA-mining challenge, README = "nothing to see here"
- `tamuctf/tamuctf-2025`: misc/forward-to-the-past (no phantom-related content)
- `tamuctf/tamuctf-2022`: includes vanity challenge source

---

*Investigation performed: 2026-03-21 | Method: Git forensics + GitHub API + HTML analysis*
