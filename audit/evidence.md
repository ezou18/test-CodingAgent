# Conflict Evidence

This file documents one full merge-conflict scenario in this repository: how
the conflict was produced, what it looked like, and how it was resolved.

## Conflict scenario

Two branches modified the **same line** of `README.md` (the project description
right under the `# test-CodingAgent` heading), starting from the same `main`
commit:

| Branch                       | PR  | Line content |
|------------------------------|-----|--------------|
| `conflict/update-readme-a`   | [#4](https://github.com/ezou18/test-CodingAgent/pull/4) | `A practice repository for learning GitHub workflows and collaboration.` |
| `conflict/update-readme-b`   | [#5](https://github.com/ezou18/test-CodingAgent/pull/5) | `A hands-on coding agent test repository for experimenting with Git and GitHub features.` |

PR #4 was merged first while `main` had no conflict. PR #5 then conflicted
against the updated `main` because both branches had written different content
to the same line.

## Conflict markers (what Git actually showed)

After running `git merge origin/main` on `conflict/update-readme-b`, Git
inserted these markers into `README.md`:

```text
# test-CodingAgent

<<<<<<< HEAD
A hands-on coding agent test repository for experimenting with Git and GitHub features.
=======
A practice repository for learning GitHub workflows and collaboration.
>>>>>>> origin/main

## Project Structure
```

`git status` reported:

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

## Resolution

Instead of picking one side, both intents were combined into a single sentence
that preserves the meaning of both PRs:

```text
A hands-on coding agent test repository for learning GitHub workflows,
collaboration, and Git features.
```

Conflict markers were removed, then:

```bash
git add README.md
git commit -m "Resolve merge conflict in README description"
git push origin conflict/update-readme-b
```

## Resolve commit

- **SHA:** `07926af`
- **Message:** `Resolve merge conflict in README description`
- **Parents:** `dc0a668` (version B) + `f547e9c` (PR #4 merge into main)
- **Link:** https://github.com/ezou18/test-CodingAgent/commit/07926af542cb2b756e91447114604d468d36f3b1

This is a merge commit (two parents), which is the canonical shape of a
conflict-resolution commit produced by `git merge`. After this commit, PR #5
showed *no conflicts* on GitHub and was merged cleanly into `main` as commit
`6657697`.

## How to verify

1. Open https://github.com/ezou18/test-CodingAgent/pull/5
   - **Conversation** tab shows the PR was merged after the conflict commit
   - **Commits** tab lists `07926af Resolve merge conflict in README description`
2. Open the resolve commit directly:
   https://github.com/ezou18/test-CodingAgent/commit/07926af542cb2b756e91447114604d468d36f3b1
   - The diff view shows the conflict markers being removed and replaced with
     the combined sentence
3. Compare PR #4 and PR #5 diffs against the final `README.md` on `main` to
   see that both sides of the conflict contributed to the final wording.
