# Git Reference

## Tổng quan

|Vùng|Vị trí|Lưu ở đâu|
|-|-|-|
|Working Directory|Thư mục project trên ổ đĩa|Files bình thường, bạn edit trực tiếp|
|Staging Area|Bên trong **_index_**|File nhị phân, quản lý bởi Git|
|Local Repository|Bên trong **_objects_**|Commit history, branches, tags|
|Remote Repository|Server (GitHub, GitLab...)|Trên internet / mạng nội bộ|

```
[Máy local của bạn]
├── Working Directory   ← bạn nhìn thấy, edit được
├── .git/
│   ├── index          ← Staging Area
│   └── objects/       ← Local Repo (commits, trees, blobs)
│
[Remote Server]
└── Remote Repo        ← GitHub / GitLab / Bitbucket
```
## Git Commands Flow

![alt text](GitCommandsFlow.png)

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
