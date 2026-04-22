# Branching, Merging & Rebasing

## Branch

```bash
git branch                      # list local branches (* = current)
git branch -a                   # list all (local + remote)
git branch feature/login        # tạo branch mới (không switch)
git checkout -b feature/login   # tạo + switch (classic)
git switch -c feature/login     # tạo + switch (modern, Git 2.23+)
git switch main                 # switch branch
git branch -d feature/login     # xóa branch (đã merge)
git branch -D feature/login     # force delete (chưa merge)
git branch -m old-name new-name # rename branch
```

---

## Merge

Kết hợp history của 2 branch.

```bash
git checkout main
git merge feature/login         # merge feature vào main
git merge --no-ff feature/login # luôn tạo merge commit (recommended)
git merge --squash feature/login # squash tất cả commits thành 1
```

### Fast-forward vs Merge Commit

```
Fast-forward (main chưa có commit mới):
  main:    A → B
  feature:     B → C → D
  Sau merge: A → B → C → D  (không có merge commit)

Merge commit (main có commit mới):
  main:    A → B → E
  feature:     B → C → D
  Sau merge: A → B → C → D → M  (M = merge commit)
                   ↘ E ↗
```

---

## Resolve Conflict

### Conflict xảy ra khi nào?

- `git merge` — 2 branch sửa cùng 1 chỗ trong file
- `git rebase` — commit được reapply mà có xung đột với base mới
- `git cherry-pick` — commit được apply lên branch khác có xung đột
- `git stash pop` — stash apply lên working dir đã có thay đổi

### Markers trong file conflict

```cpp
<<<<<<< HEAD                    // code ở branch hiện tại (main)
int calculate(int a, int b) {
    return a + b;
}
=======                         // phân cách
int calculate(int a, int b) {
    return a * b;               // code ở branch đang merge vào
}
>>>>>>> feature/multiply        // tên branch đang merge
```

3 vùng:
- `<<<<<<< HEAD` đến `=======` → **ours** (branch hiện tại)
- `=======` đến `>>>>>>>` → **theirs** (branch đang merge)

### Giải quyết thủ công

```bash
git merge feature/login
# CONFLICT (content): Merge conflict in src/main.cpp
# Automatic merge failed; fix conflicts and then commit the result.

# 1. Xem tất cả files bị conflict
git status
# both modified: src/main.cpp
# both modified: src/utils.cpp

# 2. Sửa từng file — xóa markers, giữ code đúng
# (dùng editor, xóa <<<<, ====, >>>> và giữ code muốn)

# 3. Đánh dấu đã resolve
git add src/main.cpp
git add src/utils.cpp

# 4. Hoàn thành merge
git commit                      # Git tự tạo merge commit message

# Hủy merge nếu muốn quay lại
git merge --abort
```

### Dùng merge tool

```bash
git mergetool                   # mở tool configured (vimdiff, meld, ...)
git mergetool --tool=vscode     # dùng VS Code

# Config VS Code làm merge tool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

### Conflict trong Rebase

```bash
git rebase main
# CONFLICT (content): Merge conflict in src/main.cpp
# error: could not apply abc1234... feat: add feature

# 1. Sửa file conflict
# 2. Stage file đã sửa
git add src/main.cpp
git rebase --continue           # tiếp tục rebase (không dùng git commit)

# Hoặc bỏ qua commit này
git rebase --skip

# Hoặc hủy toàn bộ
git rebase --abort
```

### Conflict Strategies

```bash
# Chấp nhận toàn bộ của 1 bên (không cần sửa tay)
git merge -X ours feature/login    # giữ tất cả của HEAD
git merge -X theirs feature/login  # giữ tất cả của branch đang merge

git checkout --ours   src/file.cpp # giữ version của HEAD cho 1 file
git checkout --theirs src/file.cpp # giữ version của theirs cho 1 file
git add src/file.cpp
```

### Xem 3-way diff

```bash
git diff                        # working dir vs staged (sau khi sửa 1 phần)
git diff --staged               # staged vs HEAD

# Xem base + ours + theirs cùng lúc
git log --merge --oneline       # commits liên quan đến conflict
git diff ORIG_HEAD              # so sánh với trước khi merge
```

---

## Rebase

Đưa commits của branch hiện tại lên **đỉnh** của branch khác — lịch sử thẳng hàng.

```
Trước rebase:
  main:    A → B → C
  feature: A → B → D → E

git checkout feature
git rebase main

Sau rebase:
  main:    A → B → C
  feature: A → B → C → D' → E'  (D, E được reapply lên C)
```

```bash
git rebase main                 # rebase feature lên main
git rebase --onto main A B      # rebase range A..B lên main
git rebase --continue           # tiếp tục sau khi resolve conflict
git rebase --abort              # hủy rebase
git rebase --skip               # bỏ qua commit đang conflict
```

**⚠️ Không rebase branch đã push public** — thay đổi history gây vấn đề cho người khác.

---

## Merge vs Rebase

| | Merge | Rebase |
|--|-------|--------|
| History | Giữ nguyên (merge commit) | Viết lại (linear) |
| Conflict | 1 lần | Mỗi commit |
| Dùng khi | Feature branch, public | Cleanup local commits |
| Rule | Merge to main | Rebase before merge |

---

## Cherry-pick

Áp dụng 1 commit cụ thể từ branch khác.

```bash
git cherry-pick abc123          # apply commit abc123 vào branch hiện tại
git cherry-pick abc123..def456  # apply range of commits
git cherry-pick --no-commit abc123  # apply changes nhưng không commit
git cherry-pick --abort         # hủy
```

---

## Tóm tắt

```bash
git switch -c feature/xyz       # tạo + switch branch
git merge --no-ff feature/xyz   # merge với merge commit
git rebase main                 # linear history

Conflict:
  sửa file → git add → git commit (merge)
           → git add → git rebase --continue (rebase)

cherry-pick abc123  → lấy 1 commit từ branch khác
```
