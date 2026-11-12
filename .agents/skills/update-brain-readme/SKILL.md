---
name: update-brain-readme
description: Use when adding to, correcting or restructuring this repo's README.md (the bot-team guide and backup).
---

# Update the brain README

[README.md](../../../README.md) is meant to grow. Justin and any AI agent, including bots on his work and personal Grok Bot accounts and other agents, can add findings to it as they discover them. When you do:

- Keep the body generic, so it works for any account.
- Put account-specific setup only in [Account-specific setup](../../../README.md#account-specific-setup).
- Never add secrets, dollar amounts, internal hostnames, company or product internals, people, tickets, ids, tokens or credential locations. Employer specifics go in a private brain doc (see [Private brain docs](../../../README.md#private-brain-docs)).
- Update existing text in place instead of appending duplicates.
- Define each rule or concept exactly once, in the section where it fits best, and link to it from elsewhere instead of repeating it.
- Keep each part doing its job: the story stays narrative, the how-to stays generic, and the team reference matches the team as it runs today.
- Refresh [Results](../../../README.md#results) only with newly measured figures, and name their sources and windows.
- Write the current state with brief historical context, in official, certain wording. No Q&A or open-questions sections, and no back-and-forth narration.
- Keep word-for-word copies, such as routine prompts and bot descriptions, identical to the live text. Mark any part generalized for the public README as noted.
- Keep code style out of the README. It belongs in the styles repo (see [Code style](../../../README.md#code-style)).
- Keep the [contents](../../../README.md#contents) list in sync with the H2 sections.
- Commit with Conventional Commits, with Justin as author and committer, and no trailers.
- Add changes to the one open rolling brain PR: pull its latest head first, commit on top, never open a second rolling PR, and never force-push. Open a new draft PR only when none is open. This applies from every Grok Bot account.
