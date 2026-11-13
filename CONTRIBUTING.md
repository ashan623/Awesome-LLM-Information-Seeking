# Contributing

Thank you for helping build this paper list.

## Scope

Please add papers that help answer at least one of the survey questions:

- What information-seeking capability does the model acquire?
- How is that capability trained or optimized?
- What evidence shows that it generalizes beyond the original task or environment?

The repository is not intended to be a generic list of all RAG or search-agent papers. Prefer papers with a clear connection to information-seeking behavior, search learning, evidence use, adaptive control, or capability generalization.

## Entry requirements

For each paper, include the title, authors, year, stable paper link, and—when available—an official code or project link. Assign a primary capability and record the learning mechanism, update target, supervision signal, evaluation evidence, and at least one limitation or open question.

Use the following compact format in the root README:

```markdown
- **Paper Title**  
  *Authors* [[paper](URL)] [[code](URL)] YEAR. ![](TYPE_BADGE) ![](CAPABILITY_BADGE) ![](VENUE_BADGE)
```

When useful, add substantive paper-level context to the relevant thematic file under `papers/`. Distinguish **claimed capability** from **directly measured capability**, and do not treat an in-domain task-score improvement as proof of generalization. Search logs, temporary drafts, and round-by-round status notes do not belong in the public catalog.

## Quality checks

Before opening a pull request:

1. Verify that the paper link resolves to an official publisher, conference, or arXiv page.
2. Check that the paper is not already listed under a different title or preprint/version.
3. Use the existing A1–A6 capability labels and the current learning/generalization labels where applicable.
4. Keep the root README concise and move long explanations to the detailed annotation files.
5. Run `git diff --check` before committing.
