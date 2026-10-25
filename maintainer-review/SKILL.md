---
name: maintainer-review
description: Senior-maintainer review of a pull request, remote branch, or local changes, with severity-rated findings and a merge recommendation.
argument-hint: "[pr-number | branch]"
disable-model-invocation: true
allowed-tools: Bash(gh pr view *) Bash(gh pr diff *) Bash(gh pr checks *) Bash(git diff *) Bash(git log *) Bash(git status *) Bash(git merge-base *) Bash(git show *) Bash(git rev-parse *)
---

Run a senior-maintainer review in one fresh subagent in the background, so the review's legwork stays out of this conversation and you stay free while it runs. The target is the pull request or branch named in the arguments; with no arguments, it is the current branch.

## 1. Check the target

Before going further, confirm the target exists and has something to review: the pull request opens with `gh pr view`, the branch resolves locally or on the remote (for example with `git rev-parse`), or the current branch has commits or uncommitted changes beyond its base. If it doesn't, tell the user what was wrong and stop: a mistyped target should fail here, not after a background review.

## 2. Write the brief

The reviewer starts with none of this conversation, so the brief carries everything it needs:

- the repository path and the target as given: a PR number or URL, a branch name, or "the current branch";
- what this conversation knows that the repository does not: what the change is meant to do, invariants or constraints the user stated, areas they're worried about, and the base branch if one was named;
- the results of any tests, lint, or type checks already run on the target in this conversation, and the commit they ran on, so the reviewer uses them in place of its own runs;
- the absolute path of the `REVIEW.md` that sits next to this SKILL.md, and the instruction to read and follow it, with its final message being the complete review.

The brief is done when a reviewer holding only the brief and the repository could start without asking anything.

## 3. Launch the reviewer

Launch the reviewer as a fresh general-purpose subagent in the background with the brief, then tell the user the review is running. While it runs, relay any progress update it sends, and message it only to pass on new information from the user or to check on it after 15 minutes without an update.

## 4. Relay the review

When the reviewer finishes, give the user its review in full and unedited: the review's structure is the deliverable.
