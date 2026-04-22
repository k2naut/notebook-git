# Git Reference

## Tổng quan

```
Working Directory  →  [git add]   →  Staging Area (Index)
Staging Area       →  [git commit] →  Local Repository
Local Repository   →  [git push]  →  Remote Repository
Remote Repository  →  [git pull / fetch + merge] → Local
```

---

## Files trong folder này

| File | Nội dung |
|------|---------|
| [basics.md](basics.md) | init, clone, add, commit, status, log, diff |
| [branching.md](branching.md) | branch, checkout, merge, rebase, cherry-pick |
| [remote.md](remote.md) | remote, push, pull, fetch, tracking branches |
| [undoing.md](undoing.md) | reset, revert, restore, stash, reflog |
| [advanced.md](advanced.md) | rebase -i, bisect, worktree, submodule, hooks |
| [workflow.md](workflow.md) | Git Flow, GitHub Flow, conventional commits |

---

## Tóm tắt nhanh

```bash
# Setup
git config --global user.name  "Name"
git config --global user.email "email@example.com"

# Daily workflow
git status                  → trạng thái working directory
git add .                   → stage tất cả thay đổi
git commit -m "message"     → lưu snapshot
git push                    → đẩy lên remote
git pull                    → lấy về + merge

# Branch
git checkout -b feature/xyz → tạo và switch branch mới
git merge feature/xyz       → merge vào branch hiện tại
git rebase main             → rebase branch hiện tại lên main

# Undo
git stash                   → tạm lưu thay đổi chưa commit
git reset --soft HEAD~1     → undo commit, giữ staged changes
git revert <hash>           → tạo commit ngược lại (an toàn)
```
