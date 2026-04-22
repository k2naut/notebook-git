# Undoing Changes in Git

## Tổng quan so sánh

```
Tình huống                        Lệnh
─────────────────────────────────────────────────────
Chưa stage (working directory)  → git restore <file>
Đã stage, chưa commit           → git restore --staged <file>
Đã commit (local)               → git reset HEAD~1
Đã commit (đã push)             → git revert <hash>
Muốn tạm lưu chưa commit        → git stash
```

---

## `git restore` — Khôi phục file (Git 2.23+)

```bash
git restore file.cpp            # hủy thay đổi trong working dir (discard)
git restore .                   # hủy tất cả thay đổi working dir
git restore --staged file.cpp   # unstage file (giữ thay đổi trong working dir)
git restore --staged .          # unstage tất cả
git restore --source=HEAD~2 file.cpp  # lấy version của file từ 2 commit trước
```

---

## `git reset` — Di chuyển HEAD (thay đổi history)

```bash
# --soft: undo commit, giữ changes ở STAGED
git reset --soft HEAD~1         # undo 1 commit, giữ staged
git reset --soft HEAD~3         # undo 3 commits, tất cả các thay đổi staged

# --mixed (default): undo commit, giữ changes ở WORKING DIR (unstaged)
git reset HEAD~1
git reset --mixed HEAD~1

# --hard: undo commit, XÓA cả working dir changes (nguy hiểm)
git reset --hard HEAD~1         # undo + discard all changes
git reset --hard origin/main    # reset về trạng thái remote

# Reset về commit cụ thể
git reset --soft abc123
```

```
--soft:  commit bị undo, changes ở Staging Area
--mixed: commit bị undo, changes ở Working Directory
--hard:  commit bị undo, changes BỊ XÓA hoàn toàn ⚠️
```

---

## `git revert` — Undo an toàn (không thay đổi history)

Tạo commit mới đảo ngược commit cũ — **an toàn để dùng trên branch đã push**.

```bash
git revert abc123               # tạo commit đảo ngược abc123
git revert HEAD                 # revert commit mới nhất
git revert HEAD~3..HEAD         # revert 3 commits gần nhất
git revert --no-commit abc123   # revert nhưng không tự commit (để review)
git revert -m 1 <merge-commit>  # revert merge commit (1 = mainline parent)
```

---

## `git stash` — Tạm lưu thay đổi

```bash
git stash                       # lưu working dir + staged changes
git stash push -m "WIP: login"  # lưu với message
git stash push -- src/          # chỉ stash 1 folder
git stash -u                    # kèm untracked files
git stash -a                    # kèm cả ignored files

git stash list                  # xem danh sách stashes
# stash@{0}: WIP on main: abc123 latest commit
# stash@{1}: WIP: login

git stash pop                   # apply stash mới nhất + xóa khỏi list
git stash apply                 # apply nhưng giữ trong stash list
git stash apply stash@{1}       # apply stash cụ thể
git stash drop stash@{1}        # xóa 1 stash
git stash clear                 # xóa tất cả stashes
git stash branch feature/from-stash  # tạo branch từ stash
```

---

## `git reflog` — Lịch sử đầy đủ (cứu data đã mất)

`reflog` ghi lại **mọi thao tác** thay đổi HEAD — kể cả khi reset --hard hoặc xóa branch.

```bash
git reflog                      # xem history của HEAD
# HEAD@{0}: reset: moving to HEAD~1
# HEAD@{1}: commit: feat: add login
# HEAD@{2}: commit: fix: typo

git checkout HEAD@{1}           # khôi phục về state cũ
git reset --hard HEAD@{1}       # khôi phục hoàn toàn
git branch recovered HEAD@{1}  # tạo branch từ điểm trước đó
```

**Reflog giữ data 90 ngày mặc định** — cứu được kể cả sau `reset --hard`.

---

## `git commit --amend` — Sửa commit cuối

```bash
git add forgotten-file.cpp
git commit --amend              # sửa message + thêm file
git commit --amend --no-edit    # chỉ thêm file, giữ nguyên message
git commit --amend -m "new msg" # chỉ đổi message
```

**⚠️ Không amend commit đã push** — thay đổi hash, cần force push.

---

## Tóm tắt

```
Chưa stage:      git restore <file>
Đã stage:        git restore --staged <file>
Undo commit (local):
  giữ changes:   git reset --soft HEAD~1
  unstage:       git reset HEAD~1 (--mixed)
  xóa changes:   git reset --hard HEAD~1  ⚠️

Đã push:         git revert <hash>  ← TẠO commit mới, an toàn

Tạm lưu:         git stash / git stash pop

Cứu data mất:    git reflog → git reset --hard HEAD@{N}
```
