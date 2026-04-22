# Advanced Git

## Interactive Rebase — Rewrite History

Sắp xếp lại, squash, edit, xóa commits **local** trước khi push.

```bash
git rebase -i HEAD~4            # interactive rebase 4 commits gần nhất
git rebase -i abc123            # từ commit abc123 đến HEAD
```

```
Editor mở ra:
pick abc1234 feat: add login
pick def5678 fix: typo in login
pick ghi9012 wip: cleanup
pick jkl3456 feat: add logout

Các lệnh:
  pick   → giữ nguyên
  reword → giữ nhưng đổi message
  edit   → dừng lại để amend
  squash → gộp vào commit trước (giữ cả 2 messages)
  fixup  → gộp vào commit trước (bỏ message của commit này)
  drop   → xóa commit

Ví dụ — squash typo fix vào feat:
pick abc1234 feat: add login
fixup def5678 fix: typo in login
drop ghi9012 wip: cleanup
pick jkl3456 feat: add logout
```

---

## `git bisect` — Tìm commit gây bug

Binary search qua lịch sử để tìm commit đầu tiên gây lỗi.

```bash
git bisect start
git bisect bad                  # commit hiện tại có bug
git bisect good v1.0.0          # commit này OK

# Git checkout commit giữa, bạn test rồi đánh dấu:
git bisect bad                  # commit này có bug
git bisect good                 # commit này OK

# Git tiếp tục bisect cho đến khi tìm ra commit đầu tiên có bug
# Kết quả: "abc1234 is the first bad commit"

git bisect reset                # kết thúc và quay về HEAD

# Tự động với script
git bisect run ./test.sh        # script return 0 = good, non-zero = bad
```

---

## `git worktree` — Nhiều Working Directory

Checkout nhiều branch đồng thời trong các folder khác nhau — không cần stash.

```bash
git worktree add ../hotfix hotfix/critical-bug    # tạo worktree cho branch hotfix
git worktree add ../review origin/feature/login   # checkout remote branch

git worktree list               # xem danh sách
git worktree remove ../hotfix   # xóa worktree
```

---

## `git submodule` — Repo lồng Repo

Nhúng một git repo khác vào trong repo của bạn.

```bash
git submodule add https://github.com/lib/repo libs/repo
git submodule update --init --recursive     # khởi tạo sau khi clone
git submodule update --remote               # cập nhật lên commit mới nhất

# Clone với submodules
git clone --recurse-submodules <url>
```

---

## Git Hooks — Automation

Scripts tự động chạy trước/sau git operations. Nằm trong `.git/hooks/`.

```bash
# pre-commit — chạy trước khi commit
# Dùng để: format code, run linter, run tests

# .git/hooks/pre-commit
#!/bin/bash
echo "Running clang-format..."
find src/ -name "*.cpp" -o -name "*.h" | xargs clang-format --dry-run --Werror
if [ $? -ne 0 ]; then
    echo "Code not formatted! Run: clang-format -i src/*.cpp"
    exit 1
fi

# commit-msg — validate commit message format
# pre-push — chạy tests trước khi push
```

```bash
chmod +x .git/hooks/pre-commit  # cần set executable

# Dùng pre-commit tool (chia sẻ được trong team)
pip install pre-commit
# .pre-commit-config.yaml
```

---

## Git Attributes — File-specific Settings

```bash
# .gitattributes
*.cpp    diff=cpp               # diff syntax highlight
*.h      diff=cpp
*.md     diff=markdown
*.png    binary                 # treat as binary
*.sh     eol=lf                 # enforce LF line endings
```

---

## Useful Aliases

```bash
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.st "status -s"
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.unstage "restore --staged"
git config --global alias.last "log -1 HEAD --stat"
git config --global alias.undo "reset --soft HEAD~1"

# Dùng
git lg    # đẹp hơn git log --graph
git undo  # undo commit cuối
```

---

## Tóm tắt

```
rebase -i HEAD~N    → squash, reword, drop commits local
bisect              → binary search tìm commit gây bug
worktree            → nhiều branch cùng lúc, khác folder
submodule           → nhúng repo khác
hooks               → automation (format, lint, test) tự động
aliases             → shortcut cho lệnh hay dùng
```
