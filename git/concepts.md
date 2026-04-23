# Git Key Concepts & Keywords

## HEAD

`HEAD` là con trỏ (pointer) chỉ đến **commit hiện tại bạn đang đứng**.

```
main:    A ── B ── C ── D
                        ↑
                       HEAD   (đang ở commit D, branch main)
```

| Trạng thái | Mô tả |
|-----------|-------|
| `HEAD` | Commit đang được checkout (thường là tip của branch hiện tại) |
| `HEAD~1` | Commit cha (1 bước trước HEAD) |
| `HEAD~2` | Commit ông (2 bước trước HEAD) |
| `HEAD^` | Commit cha đầu tiên (giống `HEAD~1`) |
| `HEAD^2` | Commit cha thứ 2 (dùng sau merge — merge commit có 2 cha) |
| `ORIG_HEAD` | HEAD trước khi thực hiện merge/rebase/reset (backup tự động) |
| `FETCH_HEAD` | Commit mới nhất vừa được fetch từ remote |
| `MERGE_HEAD` | Commit đang được merge vào HEAD |

### Detached HEAD
Xảy ra khi bạn `checkout` trực tiếp vào một commit (không phải branch):
```bash
git checkout <commit-hash>
# → HEAD không trỏ vào branch nào → "detached HEAD state"
# Commit mới tạo ra sẽ bị mất nếu không tạo branch mới
git checkout -b new-branch   # thoát detached HEAD
```

---

## Branch

Một **branch** là pointer trỏ đến một commit. Khi tạo commit mới, branch tự động di chuyển về phía trước.

```
main:    A ── B ── C          ← main trỏ vào C
                   \
feature:            D ── E    ← feature trỏ vào E
```

| Khái niệm | Mô tả |
|-----------|-------|
| `main` / `master` | Branch mặc định chính |
| `origin/main` | **Remote-tracking branch** — bản sao local của branch `main` trên remote `origin` |
| `upstream` | Remote gốc (thường dùng khi fork repo) |
| `tracking branch` | Branch local được liên kết với remote branch |

---

## Commit

Một **commit** là snapshot của toàn bộ project tại một thời điểm — không phải diff.

```
Commit object gồm:
  - tree     : trỏ đến snapshot của files
  - parent   : hash của commit cha
  - author   : tên + email + timestamp
  - committer: tên + email + timestamp
  - message  : commit message
  - hash     : SHA-1 (40 ký tự hex) định danh duy nhất
```

### Commit hash
```bash
git log --oneline
# a1b2c3d  fix: login bug
# e4f5a6b  feat: add dashboard
# 7c8d9e0  init commit

# Có thể dùng 7 ký tự đầu thay vì full hash
git show a1b2c3d
```

---

## Index (Staging Area)

**Index** là tên kỹ thuật của Staging Area — file `.git/index` lưu trạng thái sẽ được đưa vào commit tiếp theo.

```
Working Dir  →  git add  →  Index  →  git commit  →  Repo
(untracked/modified)      (staged)                  (committed)
```

---

## Tree, Blob, Tag (Git Objects)

Git lưu mọi thứ dưới dạng **object** trong `.git/objects/`:

| Object | Mô tả |
|--------|-------|
| **blob** | Nội dung của một file (không lưu tên file) |
| **tree** | Danh sách files + subdirectories (tương đương folder) |
| **commit** | Trỏ đến 1 tree + parent commit + metadata |
| **tag** | Trỏ đến 1 commit, có tên cố định (annotated tag có thêm message) |

```
commit ──→ tree (root)
               ├──→ blob (README.md)
               ├──→ blob (main.c)
               └──→ tree (src/)
                       ├──→ blob (helper.c)
                       └──→ blob (helper.h)
```

---

## Remote & Origin

| Keyword | Mô tả |
|---------|-------|
| `origin` | Tên mặc định cho remote khi clone. Chỉ là alias cho URL |
| `upstream` | Thường dùng để chỉ repo gốc khi bạn fork |
| `remote-tracking branch` | `origin/main` — bản copy local của remote branch, chỉ update khi `fetch` |

```bash
git remote -v                  # xem danh sách remotes
git remote add origin <url>    # thêm remote tên "origin"
git remote rename origin new   # đổi tên remote
```

---

## Ref (Reference)

**Ref** là tên dễ đọc trỏ đến một commit hash. Tất cả đều nằm trong `.git/refs/`.

```
.git/
├── HEAD                    ← "ref: refs/heads/main"
└── refs/
    ├── heads/
    │   ├── main            ← hash của commit mới nhất trên main
    │   └── feature/xyz
    ├── remotes/
    │   └── origin/
    │       └── main        ← remote-tracking branch
    └── tags/
        └── v1.0.0
```

| Ref | Trỏ đến |
|-----|---------|
| `refs/heads/main` | Branch main local |
| `refs/remotes/origin/main` | Remote-tracking branch |
| `refs/tags/v1.0` | Tag |

---

## Reflog

**Reflog** ghi lại lịch sử di chuyển của HEAD và các branch — kể cả sau `reset --hard`. Đây là "lưới an toàn" của Git.

```bash
git reflog                  # xem lịch sử HEAD
# HEAD@{0}: commit: fix bug
# HEAD@{1}: reset: moving to HEAD~1
# HEAD@{2}: commit: add feature

git checkout HEAD@{2}       # quay lại trạng thái cũ
git reset --hard HEAD@{1}   # khôi phục sau reset nhầm
```

---

## Stash

**Stash** là stack lưu tạm thay đổi chưa commit.

```bash
git stash                   # push vào stash stack
git stash push -m "wip"     # push với tên
git stash list              # xem stack: stash@{0}, stash@{1}...
git stash pop               # pop stash@{0} + xóa khỏi stack
git stash apply stash@{1}   # áp dụng stash cụ thể, không xóa
git stash drop stash@{0}    # xóa stash cụ thể
git stash clear             # xóa toàn bộ stash
```

---

## Tag

**Tag** là ref cố định, không di chuyển như branch.

| Loại | Mô tả |
|------|-------|
| **Lightweight tag** | Chỉ là pointer đến commit, không có metadata |
| **Annotated tag** | Object đầy đủ: tên, message, tác giả, ký GPG được |

```bash
git tag v1.0.0                        # lightweight
git tag -a v1.0.0 -m "Release 1.0"   # annotated
git push origin v1.0.0                # push tag lên remote
git push origin --tags                # push tất cả tags
```

---

## Merge vs Rebase vs Squash

```
Trước:
  main:    A ── B ── C
                      \
  feature:             D ── E

Sau git merge main (fast-forward nếu có thể, không có diverge):
  main:    A ── B ── C ── D ── E
                                ↑ HEAD

Sau git merge --no-ff (tạo merge commit):
  main:    A ── B ── C ──────── M   ← merge commit
                      \        /
  feature:             D ── E

Sau git rebase main (viết lại history):
  main:    A ── B ── C ── D' ── E'  ← D, E được viết lại thành D', E'

Sau git merge --squash (gộp D+E thành 1 commit):
  main:    A ── B ── C ── DE        ← 1 commit duy nhất
```

| Phương pháp | History | Khi nào dùng |
|-------------|---------|-------------|
| `merge --ff` | Tuyến tính | Branch chưa diverge |
| `merge --no-ff` | Có merge commit | Muốn thấy rõ feature branch |
| `rebase` | Tuyến tính, sạch | Feature branch chưa public |
| `squash` | Gọn, 1 commit | PR nhỏ, không cần chi tiết |

> **Quy tắc vàng:** Không rebase branch đã push lên remote (người khác đang dùng).

---

## Các ký hiệu tham chiếu commit

| Ký hiệu | Ý nghĩa |
|---------|---------|
| `HEAD` | Commit hiện tại |
| `HEAD~1` hoặc `HEAD~` | 1 commit trước |
| `HEAD~N` | N commit trước |
| `HEAD^` | Commit cha (giống `~1`) |
| `HEAD^2` | Commit cha thứ 2 (sau merge) |
| `main@{yesterday}` | Trạng thái branch main ngày hôm qua |
| `HEAD@{2}` | HEAD 2 bước trước trong reflog |
| `v1.0` | Tag v1.0 |
| `origin/main` | Remote-tracking branch |
| `abc1234` | Commit hash (7+ ký tự) |
