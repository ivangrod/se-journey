---
title: "Git Commit Message"
date: 2020-04-13T18:07:27+02:00
---

```
Capitalized, short (50 chars or less) summary

More detailed explanatory text, if necessary.  Wrap it to about 72
characters or so.  In some contexts, the first line is treated as the
subject of an email and the rest of the text as the body.  The blank
line separating the summary from the body is critical (unless you omit
the body entirely); tools like rebase can get confused if you run the
two together.

Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
or "Fixes bug."  This convention matches up with commit messages generated
by commands like git merge and git revert.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space, with blank lines in between, but conventions vary here

- Use a hanging indent
```

* git log --pretty=oneline shows a terse history mapping containing the commit id and the summary
* git rebase --interactive provides the summary for each commit in the editor it invokes
* If the config option merge.summary is set, the summaries from all merged commits will make their way into the merge commit message
git shortlog uses summary lines in the changelog-like output it produces
* git format-patch, git send-email, and related tools use it as the subject for emails
reflogs, a local history accessible with git reflog intended to help you recover from stupid mistakes, get a copy of the summary
* gitk has a column for the summary
* GitHub uses the summary in various places in their user interface

