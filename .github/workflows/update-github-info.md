---
name: update-github-info
description: Refresh Mona's GitHub Info page with useful updates from official GitHub sources.
intent: Keep Mona's GitHub Info page accurate and practically useful by proposing evidence-backed updates from official GitHub Blog and Changelog posts.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
  copilot-requests: write
engine:
  id: copilot
  copilot-sdk: true
tools:
  edit:
  web-fetch:
  github:
    toolsets: [repos]
network:
  allowed:
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    allowed-files:
      - site/content/github-info.md
    max: 1
---

# Update GitHub Info

Read `notes/mona-notes.md` and `site/content/github-info.md` through GitHub
repository API tools. Do not use shell commands to read repository files. Fetch
both https://github.blog/latest/ and https://github.blog/changelog/ using web
fetch.

Find recent, useful updates that fit Mona's practical developer focus and
existing homepage themes. Verify every proposed detail against the fetched
official sources; do not invent facts or make unsupported claims. Keep the
page concise, preserve its existing structure and tone, and cite the relevant
GitHub Blog or Changelog source for each new update.

Edit only `site/content/github-info.md`. If a source cannot be fetched or the
evidence is insufficient, do not guess; report the task as incomplete. If there
is no new, useful information to add, call `noop` with a brief reason.

When the page changes, use the configured `create-pull-request` safe output to
open one pull request for Mona to review. Include a concise summary and the
source links in the pull request description. Never write changes directly to
the default branch or use another write mechanism.
