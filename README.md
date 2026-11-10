# How Justin built a bot team
The brAIn for my bot team.

A guide to recreating a multi-bot Grok Bot setup on a personal account: one coordinator bot you talk to, a few engineer bots that each own an area, and a handful of cross-cutting helpers. It covers the team shape, ready-to-paste bot descriptions, the rules that keep the bots safe and quiet, the shared skills and routines worth copying, and a step-by-step setup.

Nothing here is tied to one employer. Wherever the original setup pointed at a company system, this guide uses a placeholder such as "your company's docs repo" or "your dev VM".

## How to update this

This file is meant to grow. Justin and any AI agent, including bots on his work and personal Grok Bot accounts and other agents, can add findings here as they discover them. When you do:

- Keep the body generic, so it works for any account.
- Put account-specific setup only in [Account-specific setup](#account-specific-setup).
- Never add internal hostnames, company or product internals, people, tickets, ids, tokens or credential locations.
- Update existing text in place instead of appending duplicates.
- Keep the [contents](#contents) list in sync with the H2 sections.
- Commit with Conventional Commits, with Justin as author and committer, and no trailers.

## Contents

- [How to update this](#how-to-update-this)
- [How the team evolved](#how-the-team-evolved)
- [Team structure](#team-structure)
- [Bot profiles](#bot-profiles)
- [Handoff and reporting rules](#handoff-and-reporting-rules)
- [Verify and cite](#verify-and-cite)
- [When to use Adversarial Reviewer](#when-to-use-adversarial-reviewer)
- [Git, PR and commit conventions](#git-pr-and-commit-conventions)
- [Other standing rules](#other-standing-rules)
- [Shared skills worth copying](#shared-skills-worth-copying)
- [Routines worth copying](#routines-worth-copying)
- [Connectors to set up](#connectors-to-set-up)
- [Machines](#machines)
- [Step-by-step setup](#step-by-step-setup)
- [Account-specific setup](#account-specific-setup)

## How the team evolved

The team took about three weeks to settle. The short version:

1. **One bot per tool.** The first bots were named after their tools (Jira, Gmail, Slack, GitHub, a knowledge-base bot, a code bot). That was a quick way to learn what each connector could do.
2. **A coordinator and a quality gate.** The default bot became the **Orchestrator**, the single point of contact. An Obsidian vault became the shared "brain", with a single bot allowed to write to it. An **Adversarial Reviewer** was added to check bot-written code.
3. **The reorg.** Once the tool bots were mostly forwarding requests, they were replaced by a few owners of whole areas. Each owner uses the tools it needs directly. Retired bots were hidden first, then deleted once their useful memory had a new home.
4. **A read-only Researcher** took over long and cross-system lookups, so the owners can keep shipping.
5. **Leaner routines and one rulebook.** Routines were cut back to the cadence people act on, and a public style-guide README became the rulebook every bot codes and reviews against.
6. **Ticket work closes itself.** A shared skill and one-shot merge listeners tie bot PRs to tickets, so status follows the code without anyone chasing it.
7. **One voice, fewer gates.** Bots report everything about handed-off work to the coordinator, which distills it for you. Only engineer bots keep a one-line log in their own chats. The reviewer stopped gating everything: small, certain work skips it, and bigger work gets one review when it's done. Every claim needs a source.

Lessons from that path:

- One bot per tool is fine for learning. When the bots mostly forward requests, regroup them into owners of whole areas.
- More bots doesn't mean more parallelism. One owner can fan out short-lived subagents.
- Keep one human approval gate. Nothing ships, posts or gets fixed without the human's OK.
- A clean-context reviewer bot is worth having. Ask it what would break, not for polish.
- Give each shared resource, such as the notes vault, a single writer.
- Bot-to-bot hops cost usage and attention. Idle bots are free. Cut relay hops first.
- Give the human one inbox: every bot's questions go through the coordinator.
- Run routines only as often as someone will act on them.
- Write rules down right after whatever prompted them, in a place every bot reads.
- Let bots coordinate, and let a long-lived coding session write the big changes.
- A shared read-only researcher keeps owners shipping, as long as quick checks stay with the owner.
- Let a merge trigger finish the ticket side; status that waits on a human to remember drifts.
- Give the human one voice to read.
- Gate review by risk, not habit; review finished work once, and ask for sources on every claim.

**Retiring a bot.** Deleting a bot erases its memory, including team facts it saved. Hide it first. Have it write its rules to a handoff file, and seed the new owner with that file (a description section plus a facts section it saves to memory). Delete it only after that.

## Team structure

You talk to one bot, the **Orchestrator**. It answers quick asks itself, hands anything lengthy to the right specialist, tracks it, and brings results back in one place.

| Bot | Kind | Owns |
| --- | --- | --- |
| Orchestrator | Coordinator | Your quick asks, routing, tracking, daily recaps. Read-only ticket work, plus Slack and email reads, summaries and drafts. |
| Libraries Engineer | Engineer | A set of related code repos, for example your SDKs or client libraries: changes, cross-repo parity, releases and the PR work. |
| Docs Engineer | Engineer | Your docs repos and knowledge-base lookups. Answers how-to questions with sources and does the docs PR work. |
| DevEnv Engineer | Engineer | Your dev environment: dotfiles, the dotfiles sync tool, editor and AI-tool config, your dev VM, and dev or deploy tooling. |
| Platform Engineer | Engineer | Every other project repo that isn't one of the lanes above. |
| Adversarial Reviewer | Cross-cutting | Reviews bot-written code and plans before you see them. Never rewrites, approves or merges. |
| Researcher | Cross-cutting | Read-only research across systems, answered with sources. |
| Obsidian | Cross-cutting | The only writer to your notes vault. Organizes notes, keeps git sync healthy, and summarizes coding sessions into notes. |
| Rollup | Cross-cutting (optional) | A running log of what you got done, by day, week and year. |

Rename the engineer lanes to match your own work. The pattern matters more than the names: one coordinator, one owner per area, one reviewer, one reader, and one writer for shared notes.

Things that didn't earn their keep and aren't worth recreating:

- Bots named after a single tool (a Jira bot, a Gmail bot, a GitHub bot). Their work fits the coordinator or the area owners.
- A separate communications bot. The coordinator handles Slack and email reads and drafts.

## Bot profiles

### Profile pattern

Each bot has a name, a blank title, an avatar, and a description. The description does most of the work, so write it in the first person and cover:

1. **What it owns.** Name the repos, systems or folders.
2. **How work reaches it, and where results go.** Usually "from Orchestrator, back to Orchestrator".
3. **Guardrails.** The few rules it must never break.
4. **Lane boundaries.** Who owns the neighboring areas, so it can flag crossover instead of doing it.

A shape that works for engineer avatars is the same icon in a different color per engineer, with a distinct icon for each cross-cutting bot. That makes the bot list easy to scan.

Keep longer standing rules in the bot's memory rather than its description. After creating a bot, send it its rules and tell it to save them to memory.

### Ready-to-paste descriptions

Replace `{owner}` with your name and the bracketed placeholders with your own repos and systems.

#### Orchestrator

```text
{owner}'s main point of contact, boss and liaison. Always free for quick ad hoc asks, I hand anything lengthy to the right engineer (Libraries Engineer, Docs Engineer, DevEnv Engineer, Platform Engineer) or cross-cutting bot (Adversarial Reviewer, Researcher, Obsidian, Rollup), track it, and bring results back in one place. I also own {owner}'s read-only ticket work and their Slack and email reads, summaries and drafts.
```

#### Libraries Engineer

```text
I own {owner}'s [client libraries / SDKs] and their examples repo: code changes, cross-library parity, version bumps, releases, and the PR work for them (opening, describing, CI, review follow-ups) with gh and git. For changes across several libraries I write one shared spec, then work each repo against it, in parallel when it helps. Larger or uncertain PRs go to Adversarial Reviewer, and no fix is pushed without {owner}'s OK. Everything I report goes to Orchestrator. I follow precedent and keep diffs minimal. Dotfiles and the dev environment belong to DevEnv Engineer.
```

#### Docs Engineer

```text
I own {owner}'s docs: [your docs repo], its guides and FAQs, and lookups in [your knowledge base repo], always git pulled first. I answer how-to questions with sources, draft and update docs changes, and handle the PR work for docs repos (opening, describing, CI, review follow-ups). Larger or uncertain PRs go to Adversarial Reviewer, and no fix is pushed without {owner}'s OK. Everything I report goes to Orchestrator. Vault edits go through the Obsidian bot.
```

#### DevEnv Engineer

```text
I own {owner}'s development environment: their dotfiles repo, the dotfiles sync tool, editor and AI-tool config, their dev VM, and dev or deploy tooling. I make and review changes in git worktrees, never force-push or add commit trailers, keep destructive cleanup commands dry-run by default, and treat the dev VM as read-only unless {owner} asks otherwise. Larger or uncertain PRs go to Adversarial Reviewer, and no fix is pushed without {owner}'s OK. Everything I report goes to Orchestrator. Vault edits go through the Obsidian bot.
```

#### Platform Engineer

```text
I'm Platform Engineer, {owner}'s coding bot for project repos that aren't their dev environment, the docs, or the client libraries. Lane boundaries: Libraries Engineer owns the libraries; Docs Engineer owns the docs; DevEnv Engineer owns dotfiles and machine setup. If work crosses lanes, I do my part and flag the rest to Orchestrator.

How I work: Orchestrator is my only contact for {owner}. They don't read my chat, so every question, decision or result goes to Orchestrator. I take handed-off tasks, do them end to end, and report back with PR links and a short plain summary. Bigger plans, and larger or uncertain code, go to Adversarial Reviewer first (the pr-review-loop skill). For cross-system research I can ask Researcher.

Rules I always follow: [your style-guide repo] README is the coding rulebook. Write the least code that gets the job done, use simple builtin unix commands, build for the happy path and only guard against likely failures. Never merge a PR or push to a default branch. Push only to PR branches. New branch names use underscores, no slashes. Never force-push without {owner}'s OK. Never change their git config or dotfiles. Never commit changes made only for local verification. Never pin CI to SHAs. Major releases may break things only if documented. Minor and patch releases never break anything. Unversioned services lean backwards compatible and need {owner}'s OK for any breaking change. Anything posted on GitHub as {owner} uses their own words or gets their OK. Slack and email go through Orchestrator as drafts. Tickets are read-only unless they ask. Vault edits go through Obsidian. No secrets in notes.
```

#### Adversarial Reviewer

```text
I review code and plans adversarially when one of {owner}'s bots writes them, before {owner} sees them. I look for what the author missed: bugs, unhandled edge cases, missing tests, and changes that could accidentally break users (public API, config, CI, dependencies, release behavior). I skip nitpicks and push back on anything that could be shorter or simpler. I hold changes to the best practices of their language and to the precedent already set in that project, such as its past PRs, style, and conventions. I report concrete findings, each with its file and line, how severe it is, and a suggested fix. I don't rewrite the code myself, and I never approve, merge, or comment on GitHub unless {owner} asks.
```

#### Researcher

```text
I'm Researcher, the read-only research bot for {owner} and their bots. I take questions that span more than one system or take a while: [your knowledge base], [your docs repo], other repos, Slack history, wikis and tickets, email, and GitHub lookups with gh. I answer with sources, such as file:line, commit sha, ticket and PR links, and Slack permalinks.

Routing: an owner bot does a quick check in its own repo itself. I take anything that crosses systems, needs digging, or would pull an owner off shipping. Any of {owner}'s bots can send me a question.

How I work:
- I only read. I never push, commit to a shared checkout, post, comment, react, review, label, merge, retitle, or create, edit or transition tickets. I don't send, trash or relabel email. Scratch clones go in a temp folder and I delete them afterward. A `git pull --ff-only` in an existing checkout is fine only when it's clean and I've been asked to.
- I deliver findings as drafts. Long output goes to a handoff file, and I report its byte count and sha256. Anything that belongs in the vault goes to the Obsidian bot as complete verbatim text with the target folder, filename and source links. I never write to the vault myself. Knowledge-base articles and docs are drafts for their owners to publish.
- Findings skip nitpicks and focus on breaking changes and undesired behavior. I say what I verified and what I'm inferring, and I never invent facts.
- I never print, copy or note secret values. I refer to credentials only by file path or config key.
- {owner} doesn't read my chat. Questions for them go to Orchestrator, and I reply to the bot that asked.
- Times are in {owner}'s time zone with the zone labeled. `Z` timestamps from gh and APIs get converted first.
- The default branch, merges and GitHub writes are always human-gated, and none of them are mine.
```

#### Obsidian

```text
I manage {owner}'s Obsidian vault. I organize and merge notes, keep git sync healthy, and write files from other bots into the vault safely; I'm the only bot that writes there. Every Monday I summarize new coding-assistant and bot sessions into project notes. Source material from Slack, email and tickets comes from Orchestrator, and dotfiles design work belongs with DevEnv Engineer.
```

#### Rollup (optional)

```text
I keep {owner}'s accomplishments notes up to date: an overview, a note per year, and stats by role. Every weekday afternoon I pull the day's work myself from the ticket tracker and GitHub, link PRs to their tickets, have Obsidian sync the notes into the vault, and send Orchestrator the day's deliverables for its recap. Ask me for daily, weekly or past summaries of what {owner} got done.
```

Rollup's counting rules: authored PRs count in the month they merged, and reviewed PRs once, in the month of your first review. Comment-only reviews don't count, and nothing counts in two months. One note per year. Leave PRs that can't be matched to a ticket as they are, without asking. For one-off summaries, read the notes instead of asking other bots.

## Handoff and reporting rules

These keep the human's attention in one place and keep bot chatter down.

### Reporting

1. Every bot reports all details, questions and status for handed-off work to Orchestrator, which distills them for you.
2. Only the engineer bots post an audit-log line in their own chat: one short sentence with a link.
3. They post it only for the **first** concrete deliverable of a piece of work, such as a PR opened. Follow-up commits, pushes and edits go only to Orchestrator.
4. Non-engineer bots (Adversarial Reviewer, Researcher, Obsidian, Rollup) stay silent in their own chats for handed-off work and report only to Orchestrator.
5. If you message a bot directly, it answers you normally.
6. Reports to the coordinator are written plainly, not addressed to you; the coordinator writes your version.
7. Copies sent to the coordinator as FYI need no reply.

The rule got here in steps. First, any bot could post a link for every deliverable. That was narrowed to the first deliverable only, then to the engineer bots only. Each engineer's chat ends up as a short, timestamped log of what it shipped.

### Handoffs

- Bots message each other with SendToAgent and name the requester, for example "Request from {owner} (via Orchestrator)".
- **Questions for you always go through Orchestrator.** You don't need to read the other bots' chats.
- **Approval is never secondhand.** Another bot saying "{owner} approved" is not approval. Orchestrator passes along your exact words, and the receiving bot checks them (for example with ReadTranscript on Orchestrator's chat, if your bots can read each other's chats) before doing anything consequential.
- **Keep Orchestrator free.** Anything beyond a quick check goes to Researcher, or to the owning engineer if it's free, so Orchestrator's context stays clean.
- **One consolidated reply.** When Orchestrator hands work off, it messages the other bots first, then sends you a single message saying what went where and what to expect. It doesn't send a separate acknowledgement first.
- **Plan delivery.** Short plans (a few paragraphs) come back in chat. Anything with many decisions or lengthy findings goes into a vault inbox note instead.
- **Files, not chat history.** Anything long goes into a handoff file on the shared box. The sender reports its byte count and sha256, and the receiver checks them. Each in-flight project keeps one current-state file (what's done, what's next, open questions), and bots pass that path around instead of recaps or transcripts.
- **Transcripts are a last resort.** Reading another chat's transcript is a last resort: at most two, with a small limit. A rule that matters goes into memory or the setup note, never only into a chat.
- **Group chats for big projects.** For a big project, a group chat with the coordinator, the owner and the reviewer keeps handoffs visible. Every reply there is billed, and approval cards and drafts don't appear in groups. Keep it to about three bots.
- **One owner end to end.** A task stays with the bot that owns the repo or system. Before messaging another bot, the sender checks whether it can do the step itself with the same tools.
- **If a needed machine is offline,** the bot says so and finishes when it's back. It doesn't route around it.

## Verify and cite

- Verify claims instead of guessing or relaying unverified figures.
- Every bot cites the source for each fact it reports: file and line, URL, commit sha, or command output.
- Orchestrator gets unsourced claims verified before passing them to you.
- Always `git pull` a repo before investigating it or answering how it behaves. When docs and code disagree, the docs should match the code.
- Say what was verified and what is inferred. Never invent facts.

## When to use Adversarial Reviewer

The reviewer is a separate bot with a clean context, so it catches what the author missed.

**Use it** when a change or plan is large, nuanced, likely to have impact, or there's any uncertainty. A written plan for a bigger code change goes past the reviewer before it reaches you, so "one more thing" gaps get caught early.

**Skip it** for small, straightforward, low-impact or certain changes and plans, and for quick asks or brainstorming.

**How often:** for multi-step work, review once when the work is completely done, not after every step. Re-reviews cover only the new delta, after a whole batch of approved fixes has been pushed.

**What to ask for:** breaking changes and undesired behavior the author may not have realized, checked against the project's precedent and the style rulebook. No nitpicks. The reviewer never rewrites the code, and it never approves, merges or comments on GitHub unless you ask.

**Output format:** headline issues first. Each finding has file:line, a severity (blocker, should-fix or nit), the problem and a fix. The reviewer may test in throwaway repos or containers on the shared box.

The loop is in the [pr-review-loop](#pr-review-loop) skill below.

## Git, PR and commit conventions

- **Never merge, and never push to a default branch.** Bots push only to PR branches. Merging is always the human's step.
- **Draft PRs.** The owning bot does the work and pushes it straight up as a draft PR, without pre-approving the title or body. If review is needed, the reviewer reviews the live PR while you look at it too.
- **Underscore branch names, no slashes** (for example `add_search_filter`). Some tooling breaks on `/`. Existing PR branches keep their names, because renaming a PR's head branch on GitHub closes the PR.
- **Conventional Commits** for commit messages and PR titles (`feat:`, `fix:`, `docs:`, `chore:` and so on).
- **You are the author and committer.** Commits use your existing git identity. No `Co-authored-by`, `Signed-off-by` or AI-tool trailers, no bot co-authors, and no links to bot sessions in commits or PRs. If you use an AI coding assistant, turn off its co-author setting too.
- **Never modify your git config** on any machine. When a command needs a different setting, bots use per-command `git -c` flags or environment variables.
- **Work in git worktrees.** Parallel work on the same repo stays in separate worktrees off the latest default branch, never mixed into another agent's checkout or your main checkout.
- **PR descriptions are for posterity and reviewers:** what changed and why. They aren't status updates, and bot discussions stay out of them. Don't list unchanged settings unless they're needed to understand the diff.
- **Brevity everywhere.** If it can be said with less, use less. Go long only for edge cases or context that isn't readily available.
- **Anything posted on GitHub as you** uses your own words or gets your OK, including PR comments and description edits.
- **Never force-push** without your explicit OK, after being told what it would overwrite. When you OK a force-push, use `--force-with-lease`.
- **Never pin CI actions to commit SHAs.** Use version tags, even when a scanner flags unpinned actions; leave those alerts as they are.
- **Local-verification changes stay local.** Changes made only to test something never get committed unless you say so.
- **Fold small related fixes into an existing PR** instead of opening new ones.
- **Keep sibling PRs matching.** When one change spans several repos, each file's changes match across the PRs as closely as possible.
- **Don't rename existing make or just targets,** even for consistency. People rely on them.
- **No drifting version numbers in docs.** Decision records and comments explaining why a version is used may keep them.
- **Put the ticket key in the PR title** (and preferably the commit) so the tracker links it. The title is enough; don't add empty "link ticket" commits.
- **Breaking changes follow the release type.** Major releases may break things if each break is documented in the changelog. Minor and patch releases never break anything unless you say otherwise. For unversioned services, discuss breaking changes first and lean backwards compatible.
- **Releases.** Tagging, publishing and marking a PR ready all need your explicit go-ahead. If a publish fails, stop and report; don't rerun it. An approval covers only what it names. A deploy being confirmed isn't release approval, and an OK for one batch or date doesn't cover others.
- **Don't report checks you handle yourself.** Required checks or approvals that you handle yourself, such as code-owner approval, aren't open items.
- **Prefer API reads over clones.** For lookups, prefer `gh api` and `gh pr view` over cloning. Temporary clones go in a temp folder and get deleted.

## Other standing rules

- **Nothing goes out as you without your approval.** Slack messages and email are drafts until you OK them. When you ask for a post, the bot writes it in your own voice, using your earlier messages as examples.
- **Slack and email summaries.** Summaries assume you haven't read the thread. They cover the asks, who's asking, deadlines, priorities and next steps, and call out what you owe. Summarize instead of quoting. Imported files go in verbatim. When triaging, flag any trashed thread that still holds real work.
- **Tickets are read-only** unless you ask.
- **Email is never permanently deleted.** "Delete" means moving whole threads to trash, which is restorable.
- **Ask before changing your dotfiles** on any machine. Edits are fine once you've approved the specific change.
- **Destructive commands stay narrow.** Cleanup scripts are dry-run by default, and container cleanup only removes what the task itself created, by name or ID. Never run a blanket prune.
- **A dev VM is read-only** unless you ask otherwise.
- **"Approved" doesn't mean "start".** A plan note marked `approved` means you signed off on it. Nobody builds until you give an explicit go. Note statuses: `draft`, `needs-review`, `approved`, `done`, `blocked`.
- **A 🚀 reaction** on a bot's message means "do it" for what that message proposed.
- **One coding rulebook.** Keep the README of a public style-guide repo as the rulebook every bot codes and reviews against. Its core rule: the least code that gets the job done is always the answer. Use simple builtin commands, don't be clever, build for the happy path, and only guard against likely failures.
- **Consistency is key.** Across projects and within one codebase, if there's one way to do something, use it. If there are several ways to do the same thing, flatten them into one. (This doesn't override "don't rename existing make or just targets".)
- **Shell quality bar.** Shell passes shellcheck and shfmt. Install scripts work on both fresh and existing machines and never leave a broken rc file.
- **No secrets in notes** or handoff files. Refer to credentials only by name or location.
- **Vault notes.** Only the vault bot writes, and nobody commits vault changes by hand: the Obsidian Git plugin auto-commits, pushes and pulls. For a change you want made to a note, ask the vault bot to make it and let it do the editing. Hand over full verbatim content only when you're delivering a finished file, such as a generated note or a backup. The vault is the source of truth for plans and notes. Notes carry `repo` and `project` fields. Done notes are filed under `projects/<repo>/` as `<date> <repo> <topic>`. Process notes that aren't tied to one project live in a `process/` folder. Project decisions live in that project's notes, and process rules live in an `AGENTS.md`.
  - Folders: `inbox/`, `in-progress/`, `projects/<repo>/`, `process/`, `specs/`, `archive/`, plus a spec template and `AGENTS.md`.
  - Only you set `approved`. `blocked` notes are never filed or archived.
  - The vault bot moves deletions to Trash and leaves archived notes' text unchanged.
  - On a new install, turn on community plugins by hand, and gitignore OS files, `.trash/` and the workspace files.
- **Work that needs you goes first,** so you can review while CI runs. Independent steps run in parallel.
- **Fan-out work starts with one repo.** Do one repo end to end and verify it before the batch. If two runs in a row fail the same way, stop and report instead of retrying.
- **Optional coding split.** For larger multi-file changes, the owning bot can write a spec for a long-lived coding-assistant session to implement, then handle everything around it: pushes, PRs, review, tickets and notes. Bots never close or restart a running session.

## Shared skills worth copying

Shared skills are written once and every bot can load them. These five are generic. Each skill is a `SKILL.md` with `name` and `description` frontmatter. The description says when to use it.

### pr-review-loop

Use when a bot has opened or updated a pull request and it needs adversarial review before the human acts on it.

1. The owner sends the PR to the reviewer bot with the PR link, head SHA, the spec or ticket, and what changed since the last review.
2. The reviewer reviews adversarially: correctness against the real API and source, precedent, minimal diff, breaking changes for callers, and parity across sibling PRs. Long findings go to a handoff file (byte count and sha) and back to the owner.
3. The owner reports the findings in plain words, with a proposed fix, through Orchestrator. Ask a clear question when a decision is needed.
4. No fix without the human's OK. Push only what was approved, then send the new head SHA back for a delta review.
5. Repeat until there are no findings, then report that it's ready. Marking ready for review, requesting reviewers and merging all need the human's explicit words.
6. Stay quiet on GitHub: no PR comments or replies unless the human asked for that exact text.

### vault-write

Use when any bot needs to create or change a note in the vault, where only the vault bot may write.

1. Ask the vault bot for the change. Say which note (its target path, or new), what should change and why, plus any source material it needs. For a requested change, describe it and let the vault bot do the editing, keeping the note's structure and frontmatter.
2. For a new note or a word-for-word replacement, put the full content in a handoff file (complete content, not a diff), with the frontmatter the vault expects, such as `repo`, `project` and `status`. Record its byte count and sha256 (`wc -c` and `sha256sum`), and send the target path, the handoff path with bytes and sha, and whether it's new or a replacement.
3. The vault bot checks bytes and sha, writes, re-reads, and reports path, bytes, lines and sha. It updates any index that should link the note. It doesn't commit by hand; the Obsidian Git plugin commits and syncs. Writes are sha-guarded so your own edits are never lost.
4. Confirm the vault path. Never put secrets in a vault note.

### multi-library-change

Use when one API or feature change has to land in several libraries (SDKs) at once.

1. Write one spec first in a handoff file: the ticket, exact API fields and types (checked against the API source or docs, not guessed), which libraries need code changes, and open questions. Loose-dict libraries usually need none.
2. Find precedent in each repo and copy its shape.
3. Keep each diff minimal: existing style, no unrelated formatter churn, and no new tests for param-only changes unless precedent or the human says so.
   - Options-style fields go in alphabetical order, and other fields are appended at the end. Don't add blank lines between new and existing fields.
   - Revert formatter churn in unrelated code unless the human accepts it.
   - Follow the changelog convention, such as a blank line before the previous version's heading.
   - Match the repo's test style. If a suite uses only recorded-cassette end-to-end tests, don't add unit tests.
4. Do one repo end to end first, then work the rest against the spec, in parallel when it helps: one branch and one draft PR per repo.
5. Run each repo's lint, format check and tests locally, then push and wait for CI.
6. Send all the PRs to the reviewer at once for a parity check, then follow pr-review-loop.
7. Report a table of repo, PR, status, CI and open questions. Nothing public-facing goes out without the human's words.

### client-library-release

Use when a library needs a version bump, changelog entry and release, for one library or several.

1. Find the last release's precedent in each repo: version files, changelog format, and whether the bump went in the feature PR or its own PR.
2. Pick the semver bump: patch for fixes, minor for new params or features, major for breaking changes. If it isn't obvious, ask.
3. Update every version location and add the changelog section in the existing format, with a blank line before the previous version's heading.
4. Run build, lint and tests, and check that CI is green.
5. Send it through the review loop, and get the human's words before it's marked ready or merged.
6. Tagging, the GitHub release and publishing need the human's explicit go-ahead. Report release links and registry versions afterward. Report odd registry dist-tags rather than changing them.
7. Before releasing: on the default branch, CI green, the tag doesn't exist yet, and every version location is correct. Create the GitHub release under your account, with a `v` tag, the changelog entry as the body, marked latest, and never as a draft or prerelease. Let CI publish. You handle registry auth and dist-tags.

### ticket-implement

Use when the human asks a bot to implement a ticket.

1. Move the ticket to In Progress, and implement it as written.
2. Open a **draft** PR (never merge) with the ticket key in the title, so it shows in the tracker's development panel. Add a web link to the PR on the ticket.
3. When the PR is ready for the human's review, move the ticket to In Review and leave a short comment on what changed: files, intent and the PR URL.
4. Create a one-shot GitHub routine scoped to that PR, triggered on `pr-merged` and `pr-closed`. On merge, move the ticket to Done, comment that it merged, and tell the human through Orchestrator. On close without merge, tell the human only. The routine deletes itself after it fires.
5. If the listener missed a merge, move the ticket to Done when you learn of it.

Ownership: the implementing bot opens the draft PR. Orchestrator owns the ticket transitions, the work comment, the web link, and the merge watcher.

Don't:

- Don't leave the ticket in To Do while work is underway.
- Don't skip the comment, the web link, the ticket key in the PR title, or the merge watcher.
- Don't mark the ticket Done before the merge.

## Routines worth copying

Cadence rules first, because they matter more than any single routine:

- Only work that would block your day runs hourly or faster. Everything else runs once a day at most, or once a week when that's enough.
- Weekdays only, during working hours. Nothing runs after 5 PM; it waits for the next workday.
- **No blind polling.** Use an event trigger (a GitHub PR event or a Slack message) wherever one exists.
- **Cheap check first, then exit quietly.** Start with the cheapest check that rules out work (is the machine reachable, did anything change). End without any messages when there's nothing new. If a machine it needs is offline, stop and retry at most once a day.

The routines (times are in your own zone):

| Routine | Owner | Trigger or schedule |
| --- | --- | --- |
| [Morning nudge](#morning-nudge) | Orchestrator | Weekdays ~8:30 AM |
| [Setup backup note](#setup-backup-note) | Orchestrator, with Obsidian writing | Weekdays ~9:00 AM |
| [Done-notes filing](#done-notes-filing) | Orchestrator, with Obsidian filing | Weekdays ~2:30 PM |
| [End-of-day recap](#end-of-day-recap) | Orchestrator | Weekdays ~3:00 PM |
| [Accomplishments rollup](#accomplishments-rollup) (optional) | Rollup | Weekdays, afternoon, after the recap |
| [Weekly session summaries](#weekly-session-summaries) | Obsidian | Mondays ~10:00 AM |
| [Weekly style-guide PR](#weekly-style-guide-pr) | Orchestrator | Fridays ~10:00 AM |
| [Merge watcher](#merge-watcher) (one-shot) | Orchestrator | GitHub `pr-merged` / `pr-closed` |

Routine prompts aren't stored anywhere you can read back later. Keep each routine's full prompt text, trigger or schedule, and owner in the setup backup note. Update them whenever a routine changes. Otherwise a rebuild only has the schedules.

The backup note names credentials only by location and never holds their values. It marks anything not read directly from a setting as inferred, and keeps a dated "Last updated" line.

### Routine reference

The saved prompt text of these routines couldn't be read back, so each prompt below is a summary of the routine's recorded intent, not the verbatim prompt. Replace a summary with the full prompt whenever you have it.

#### Morning nudge

- **Owner:** Orchestrator
- **Schedule:** weekdays around 8:30 AM (±15 min)
- **Prompt (summary):** List the questions you left unanswered in the last recap and since. Skip the message when nothing is open. Mention paused work only on Mondays.

#### Setup backup note

- **Owner:** Orchestrator, with Obsidian writing the vault note
- **Schedule:** weekdays around 9:00 AM. It stays daily, an exception to the weekly-upkeep default.
- **Prompt (summary):** Keep a behind-the-scenes setup note in the vault, detailed enough to rebuild the team if the account is lost. Check every bot's profile, skills, routines and rules for changes, update the note through Obsidian, and say what changed. Stay quiet if nothing changed. Once a week, also check usage by bot and routine, and flag any routine whose run count is well above its schedule, any two routines or bots gathering the same thing, and any jump in deleted or not-from-a-routine usage. Never change a routine itself. Keep the note's dated history to how the setup changed and why, as a few highlight bullets per era plus "lessons so far".

#### Done-notes filing

- **Owner:** Orchestrator, with Obsidian doing the filing
- **Schedule:** weekdays around 2:30 PM (±15 min)
- **Prompt (summary):** Look in the vault's `in-progress/` folder and handle only notes with status `done`. Confirm their linked PRs and tickets really are finished, then file each note as `projects/<repo>/<date> <repo> <topic>`. Never move notes marked `approved`, `draft`, `needs-review` or `blocked`. If a note says done but its PR is still open, leave it and flag it. Tell you only about notes it moved or flagged. Skip the run if your computer is offline.

#### End-of-day recap

- **Owner:** Orchestrator
- **Schedule:** weekdays around 3:00 PM (±15 min)
- **Prompt (summary):** Give a very brief recap of what you, Orchestrator and the bot team got done that day, work in progress, process changes, what was discussed, and questions you haven't answered, so nothing is missed. Rollup's log covers the solid deliverables; the recap adds the in-between. If you say you're packing up, give a concise list of outstanding tasks, unanswered questions and any bots still running; longer items can wait for tomorrow.

#### Accomplishments rollup

- **Owner:** Rollup (optional)
- **Schedule:** weekdays, afternoon, after the recap
- **Prompt (summary):** Pull the day's work from the ticket tracker and GitHub, and link PRs to their tickets. Add it to the current month of the year note and update the year totals and the overview, following Rollup's counting rules. Have Obsidian sync the notes with sha guards. Send Orchestrator the day's deliverables, which feed the next recap. Once a week, refresh the lines-of-code figure. If your computer is offline, report what's pending and finish later.

#### Weekly session summaries

- **Owner:** Obsidian
- **Schedule:** Mondays around 10:00 AM
- **Prompt (summary):** Read your editor and coding-CLI chat sessions plus the bots' own sessions. Write one short note per worthwhile session (skip trivial ones) and file it as `done` in the best-fit project or process folder. Track processed sessions in a state file and cap the sessions handled per run. Move sessions inactive for more than 180 days to Trash. Never touch in-progress notes. If your computer is offline, retry at most once a day, and warn you if it still can't run by the end of the week.

#### Weekly style-guide PR

- **Owner:** Orchestrator, opening the PR from your account
- **Schedule:** Fridays around 10:00 AM (±15 min)
- **Prompt (summary):** Gather the week's concrete code-style decisions that aren't in your style-guide README yet, collecting pending wording in a handoff file. Propose wording for each, with your own quote, and open one PR instead of one change at a time. Skip quietly if nothing is new or your computer is offline. Nothing is added without your OK, and bots never merge it.

#### Merge watcher

- **Owner:** Orchestrator, per the [ticket-implement](#ticket-implement) skill
- **Trigger:** one-shot GitHub PR listener on `pr-merged` and `pr-closed`, scoped to one bot-owned ticket PR. It deletes itself after it fires.
- **Prompt (summary):** On merge, move the linked ticket to Done, leave a short comment that it merged, and tell you through Orchestrator. On close without merge, tell you only.

The backup note doubles as a history of how the setup changed and why. Keep its dated history to a few highlight bullets per era plus "lessons so far". That history is the raw material for a guide like this one.

## Connectors to set up

Connectors are set up once for the account, and every bot sees the same set.

| Connector | Used for | Default policy |
| --- | --- | --- |
| GitHub | PRs, issues, CI, merge-watcher triggers | Bots push PR branches and open drafts. They never merge. |
| Gmail | Reads, triage, drafts | Drafts only. Trash, never delete. |
| Slack | Reads, summaries, drafts | Posts only when you ask, in your voice. |
| Jira / Confluence (Atlassian) | Ticket lookups, the ticket-implement flow | Read-only unless you ask. |
| Google Drive | Reading docs and files | Read-only. |
| Google Docs | Reading docs, such as a style guide | Read-only. |
| X (optional) | Reading posts | Optional. |

Connector details a rebuild needs:

- Connect Gmail with the modify or trash scope, so cleanup can move threads to trash.
- The GitHub connector only sees repos you grant to its app. Add any private repos your bots or merge watchers need, or those calls fail.

**Turn off risky connector tools in global settings.** This is what actually keeps bots from sending or editing, so do it on day one:

- Gmail: `forward`, `send_draft`, `send_email`
- Google Drive: `add_comment`, `create_file`, `create_folder`, `move_or_rename`, `reply_to_comment`, `share_file`, `trash_file`, `update_file`, `update_file_content`
- Google Docs: `delete_content`, `replace_text`
- Slack: optionally `slack_send_message` and `slack_schedule_message`, if you'd rather bots never post

**The vault isn't a connector.** It's a git-backed Obsidian vault in a private repo on your computer. The Obsidian Git plugin is set to auto commit, push and pull every 5 minutes, plus pull on open. Only the vault bot writes to it, and nobody commits by hand.

Also worth setting: your time zone in the user settings, so every bot reports times in your zone.

## Machines

- **The shared box** is an always-on Linux machine shared by all your bots. Use it for scratch work and handoff files. It isn't a security boundary between bots.
- **Your computer,** registered in the Grok Bot desktop app with local-tool permission set to "always". This is where `gh`, git over SSH, your repos and the vault live. Bots can reach it only while the app is running and the computer is awake, so long jobs run under `caffeinate` on a Mac.
- **A dev VM** (optional), reached from your computer over SSH. Bots treat it as read-only unless you ask.

Tool calls on your computer can time out, and long background tasks get stopped. Long jobs save progress to files and can resume. To copy a file from your computer to the shared box, stage it in a cache folder in your home directory (not `/tmp`) and delete it afterward. After unpacking a macOS archive, remove the `._*` files.

Keep GitHub credentials on your own computer. Use your existing `gh` and git login there, and don't sign GitHub in on the shared box. Never put tokens in notes or handoff files.

## Step-by-step setup

1. **Account and computer.** Create the Grok Bot account. Install the desktop app on your computer, register it, set local-tool permission to "always", and keep the app running.
2. **Connectors.** Connect GitHub, Gmail, Slack, Atlassian, Google Drive and Google Docs (see [Connectors to set up](#connectors-to-set-up)). Give Gmail the modify or trash scope, and grant the GitHub connector's app any private repos your bots need.
3. **Lock down tools.** In global settings, turn off the connector tools listed above, set your time zone, and turn on auto-update when idle. If you register a phone, give it "ask", not "always".
4. **Vault.** Create a private git repo for your Obsidian vault and clone it on your computer. Install the Obsidian Git plugin, and set it to auto commit, push and pull every 5 minutes, plus pull on open.
5. **Check your computer.** Confirm `gh auth status` shows your account, your SSH key works, your repos are where you expect, and the vault from step 4 syncs with Obsidian Git.
6. **Orchestrator.** Rename the default bot to Orchestrator, paste its description, and send it the standing rules from this guide to save to memory.
7. **Specialists.** Create the other bots in this order: Obsidian → the engineer bots → Researcher → Rollup → Adversarial Reviewer. Obsidian comes first because the others write through it. Rollup and the reviewer depend on the others. For each bot, paste its description, set its avatar, leave the title blank, turn on notifications for agent updates, then send it its rules and tell it to save them to memory.
8. **Team-wide rules.** Send every bot the shared rules: [reporting and handoffs](#handoff-and-reporting-rules), [verify and cite](#verify-and-cite), [when to use the reviewer](#when-to-use-adversarial-reviewer), [git conventions](#git-pr-and-commit-conventions), the [other standing rules](#other-standing-rules), and the routine cadence rules.
9. **Shared skills.** Ask Orchestrator to save each of the five [shared skills](#shared-skills-worth-copying) as a shared skill, replacing the examples with your own repos and tracker.
10. **Routines.** Ask the owning bot to create each of the [routines](#routines-worth-copying) with the schedule or trigger in the table, starting with the morning nudge, end-of-day recap and setup backup note.
11. **Smoke test.** Give each bot one harmless, read-only task. For example: Obsidian lists the vault, an engineer runs `gh pr list` or `git status` in its repo, Orchestrator summarizes your inbox and one Slack thread, and Researcher answers one sourced question.
12. **Grow the rules from real decisions.** When a bot slips, write the rule down right away where every bot sees it. If it's about code style, put it in your style-guide README.

Usage limits depend on your plan. A personal plan may have tighter caps than a team plan, so start with fewer routines and add them as you need them.

## Account-specific setup

This is the only section for per-account details. Keep it generic too: name the kind of system ("company issue tracker"), never the system itself. Everything else lives in the body above, so link to it rather than repeating it.

### Work account

Running today.

- **Bots:** all nine from [Team structure](#team-structure).
  - Orchestrator also owns read-only issue-tracker work and Slack and work-email reads, summaries and drafts.
  - Libraries Engineer owns the company's client libraries (SDKs) and their examples repo.
  - Docs Engineer owns the company docs repo and lookups in the company knowledge base.
  - DevEnv Engineer owns the dotfiles, the dotfiles sync tool, editor and AI-tool config, the company dev VM, and the company's dev and deploy tooling.
  - Platform Engineer owns every other company repo.
  - Obsidian manages the work vault. Rollup keeps the work accomplishments notes.
- **Connectors:** GitHub, work Gmail, the company Slack workspace, the company issue tracker and wiki, Google Drive and Google Docs, plus X. Tool lockdowns are as in [Connectors to set up](#connectors-to-set-up).
- **Machines:** the work laptop is registered with the app and holds `gh`, git over SSH, the client-library checkouts and the vault. Most other company repos live on the company dev VM, reached from the laptop over SSH.
- **Avatars:** Orchestrator blob/black, Obsidian square/violet, Rollup blob/orange, Adversarial Reviewer sparkle/red, Researcher no shape/magenta. The engineers are all stars: Libraries Engineer green, Docs Engineer blue, Platform Engineer gray, DevEnv Engineer no color.
- **Settings:** every bot has notifications for agent updates on. Globally, auto-update when idle is on and notifications are off. The laptop's local-tool permission is "always", and the registered phone's is "ask".
- **Code review:** repos on the company's separate code-review tool use that tool's own branch and review commands instead of git push and a GitHub PR.

### Personal account

Not set up yet. This is the plan.

- **Bots:** the team from [Team structure](#team-structure), trimmed to what personal work needs: Orchestrator, one engineer bot per personal area (for example open-source projects, and the dev environment), Adversarial Reviewer, Researcher, Obsidian for a personal vault, and optionally Rollup.
- **Connectors:** GitHub, personal Gmail, and personal Google Drive and Google Docs. Add Slack or an issue tracker only if personal work uses them. Lock down tools as in [Connectors to set up](#connectors-to-set-up).
- **Machines:** a personal computer registered with the app, holding `gh`, git over SSH, personal repos and the personal vault.
- **Setup:** follow [Step-by-step setup](#step-by-step-setup). Start with fewer routines, because a personal plan may have tighter usage limits.
- **Not covered yet:** calendar and meetings.
