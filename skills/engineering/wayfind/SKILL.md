---
name: wayfind
description: "Launch a wayfinder agent on the VPS for a seed issue: dedicated worktree + remote-control session that charts the map. Invoke as /wayfind <issue-number> [owner/repo]."
disable-model-invocation: true
---

Launch a wayfinder effort for a seed issue. The heavy lifting lives in
`~/bin/wayfind.sh` on the VPS (factory repo: `scripts/vps/wayfind.sh`); this
skill is the front door from any machine.

## Process

1. Parse the arguments: issue number (required), repo (optional, default
   `joshuabke/IRevolution`).
2. Sanity-check the seed: `gh issue view <n> -R <repo>` — confirm it exists
   and is open. Show the user title + a one-line gist before launching.
3. Launch on the VPS:
   - On the VPS itself: `~/bin/wayfind.sh <n> [repo]`
   - From any tailnet machine: `ssh ubuntu@vps '~/bin/wayfind.sh <n> [repo]'`
     (Mac has a `Host vps` SSH alias; fallback IP `100.114.138.57`)
4. Relay the script's output to the user: session name
   (`<n>-<title-slug>`), the claude.ai/code URL, and
   `tmux attach -t <name>` as the terminal path.

The session then charts the map per `/wayfinder` (own worktree, map +
decision tickets on GitHub). When the map is complete the session stops —
consuming the map (to-spec / to-tickets) is a separate, fresh session.
