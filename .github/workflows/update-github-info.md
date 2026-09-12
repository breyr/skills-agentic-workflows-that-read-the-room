---
name: update-github-info
description: Refresh the GitHub Info page with practical updates from official GitHub sources.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
engine: copilot
network:
  allowed:
    - defaults
    - github.blog
    - github.com
    - awesome-copilot.github.com
tools:
  github:
    mode: remote
    toolsets:
      - repos
    allowed-repos: "${{ github.repository }}"
    min-integrity: approved
  web-fetch:
  edit:
safe-outputs:
  create-pull-request:
    title-prefix: "[mona] "
    draft: true
    max: 1
    fallback-as-issue: false
---

# Update GitHub Info

Refresh Mona's GitHub Info content for human review.

1. Read `notes/mona-notes.md` with the edit tool before making any changes.
2. Use the GitHub repository API tools to read repository guidance and reference files. Do not use shell commands, the GitHub CLI, or sandboxed commands to read repository guidance or reference files.
3. Use web-fetch to read `https://github.blog/latest`, `https://github.blog/changelog`, and `https://awesome-copilot.github.com/workflows/`.
4. Identify concise, practical updates that help developers learn GitHub faster. Prefer information that is current, useful, and supported by the fetched GitHub Blog, GitHub Changelog, or Awesome Copilot workflow sources.
5. Update `site/content/github-info.md` with the selected updates. Keep the summaries short and practical, preserve the existing editorial angle, and mention the source for every GitHub Blog or Changelog update.
6. Review the diff and ensure the change is limited to `site/content/github-info.md`.
7. Create a new branch, commit the change, and use the `create_pull_request` safe output exactly once to open a draft pull request for Mona to review. Do not push changes manually. If there is no meaningful update, do not modify files and do not create a pull request.
