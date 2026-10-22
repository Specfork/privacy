---
specfork: 1
name: "@specfork/privacy"
summary: Export all my data, delete all my data, import a previous export, and a data inventory, for any app that stores personal data.
license: CC-BY-4.0
---

# Privacy

A base spec: requiring specs list the data they store and how it is exported; this spec defines how people get it out, take it elsewhere, or delete it. It works with or without accounts: server-mode rules apply when the app also requires `@specfork/auth`.
Designed from needs shared by the todo, habits, fitness and budget specs; no source text or code used.

| Area | Prefix | Covers |
|---|---|---|
| [privacy.md](specs/privacy.md) | PRV | Export, import, delete-all, inventory, third parties |
