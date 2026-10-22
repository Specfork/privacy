# Privacy

**Status:** Draft · **Prefix:** PRV

## Purpose

People can always take all their data with them, bring it back, or delete it, and see what is stored.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| PRV-1 | "Export all my data" runs as a job and produces one ZIP: `manifest.json` `{format (the requiring spec's name), version, exported_at, time_zone, files: [{path, entity, count}]}` plus one JSON Lines file per entity the requiring spec lists. | export → manifest counts match records |
| PRV-2 | The export holds every record the user owns, archived and trashed included, and never secrets (password hashes, 2FA secrets, API keys, third-party tokens) or sync bookkeeping (tombstones, change logs). | export contains no `password_hash` field |
| PRV-3 | An export of ≤ 1 GB is ready within 10 minutes; the user is told when; the download link works for 7 days, only for that user, and, when the app has accounts, needs `@specfork/auth:AUTH-23` re-authentication (server mode; see PRV-11). | other user opens link → 404 |
| PRV-4 | At most one export at a time and 3 per day per user. | 4th export in a day → refused |
| PRV-5 | Import accepts an export of the same `format` and a compatible `version` into the user's data: records get new ids, references are remapped, nothing existing is overwritten; a failed import changes nothing. | import twice → two copies, originals untouched |
| PRV-6 | "Delete all my data" (account kept, if any) needs re-authentication where there are accounts, and typing a confirmation word; it removes every record the user owns in every requiring spec, within 24 hours, and confirms by email when the user has one, otherwise on screen. | after delete-all → export is empty |
| PRV-7 | Where the app has accounts, deleting the account is delete-all plus `@specfork/auth:AUTH-16`. | account deleted → sign-in fails, data gone |
| PRV-8 | Deleted data is purged from backups within 30 days; the inventory page says so. | manual: inventory shows backup purge period |
| PRV-9 | Inventory page: each category of data stored (from the requiring spec), why, how long it is kept, and which third parties receive any of it. | manual: inventory lists every entity |
| PRV-10 | No personal data goes to a third party unless the user turns that integration on; each one is listed in the inventory and can be turned off, which stops sending at once. | integration off → no further requests to it |
| PRV-11 | Local mode, for data kept on the device with no account: export writes the same ZIP (PRV-1) to a location the user picks, at once, with no link, re-authentication or PRV-4 limits; import reads a file the user picks; delete-all runs on the device after the typed confirmation. | offline: export → ZIP saved where chosen |

## Out of scope

- Legal texts (privacy policy, terms), consent banners, data processing agreements.
- Admin-side legal holds.

## Decisions

- JSON Lines per entity with a manifest: easy to read, stream and re-import, independent of any stack.
- Import never overwrites: merging is safer done by the user after import.
