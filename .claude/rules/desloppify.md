# Code Quality — desloppify

Run desloppify to systematically identify and fix code quality issues. Install and configure before scanning (requires Python 3.11+):

```bash
pip install --upgrade "desloppify[full]"
desloppify update-skill claude
```

Add `.desloppify/` to `.gitignore` — it contains local state that should not be committed. Before scanning, exclude directories that should not be analyzed (vendor, build output, generated code, worktrees) with `desloppify exclude <path>`. Share questionable candidates with the project owner before excluding.

```bash
desloppify scan --path .
desloppify next
```

`--path` is the directory to scan (use `.` for the whole project, or a subdirectory like `src/`). Your goal is to get the strict score as high as possible. The scoring resists gaming — the only way to improve it is to actually make the code better.

**The loop:** run `next`. It is the execution queue from the living plan, not the whole backlog. It tells you what to fix now, which file, and the resolve command to run when done. Fix it, resolve it, run `next` again. This is your main job. Use `desloppify backlog` only when you need to inspect broader open work not currently driving execution.

Do not be lazy. Large refactors and small detailed fixes — do both with equal energy. No task is too big or too small. Fix things properly, not minimally.

Use `plan` / `plan queue` to reorder priorities or cluster related issues. Rescan periodically. The scan output includes agent instructions — follow them, do not substitute your own analysis.
