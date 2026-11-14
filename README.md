# LotJitsu Public Documents

Canonical, versioned source for LotJitsu's customer-facing legal documents.

| Document | Current version | Effective date |
| --- | --- | --- |
| [Terms of Service](terms-of-service.md) | 1.3.0 | October 14, 2026 |
| [Privacy Policy](privacy-policy.md) | 1.2.1 | November 14, 2026 |

## How versioning works

- Each document carries a semantic version and an effective date in its header.
- Every change is recorded in [CHANGELOG.md](CHANGELOG.md) and in this repo's git history.
- When a user accepts the Terms in the LotJitsu app (at signup, when the app asks them to accept updated Terms, or before an export), the app records which version they accepted, identified by the file's git blob SHA, with a timestamp. This repo is the public reference for what each version said.
- The marketing site (lotjitsu.com/terms and lotjitsu.com/privacy), the web app and the phone app show the version the LotJitsu API last read from this repository (see "How the documents reach the site and the apps"). The site's pages do not link here; the Privacy Policy names this repository in its section 11.

## How the documents reach the site and the apps

- The LotJitsu API reads `terms-of-service.md` and `privacy-policy.md` from this repository's `main` branch every 15 minutes, and again when someone reads a document and the last check is older than that. The testing, staging and production APIs all read the same branch, so a push to `main` is live on every environment within about 15 minutes, with no deploy. Keep drafts on another branch until they are final.
- The API stores each new file content as a new version, named by the file's git blob SHA (`gh-<sha7>` for the Terms, `privacy-gh-<sha7>` for the Privacy Policy), not by the number in the header. Reverting a file brings back the version stored for that content.
- Any change to `terms-of-service.md`, a typo fix included, is a new Terms version. Every signed-in user is asked to accept it the next time they open the web or phone app, and the API refuses their exports until they have. The Privacy Policy is never accepted, and the API does not read `README.md` or `CHANGELOG.md`.
- The website's /terms and /privacy, the web app and the phone app show what the API serves (`GET /api/v1/terms/active`, `GET /api/v1/privacy/active`). The website also bakes in a fallback copy for visitors without JavaScript and for API outages. Once a change has reached the production API, refresh that copy in the `lotjitsu-marketing` repository with `npm run sync:legal` and `npm run build:site`, then commit the result.
- When one document links the other, write the link exactly as `[Privacy Policy](privacy-policy.md)` or `[Terms of Service](terms-of-service.md)`. The website and the phone app rewrite only those two links to their own pages, and any other relative link works on GitHub only.

## Status

**Final.** The current version and effective date of each document are in the table above.

A definitive statement on AI model training and customer photos is deliberately absent pending an owner decision. Do not add one to either document until that decision is made.
