# Git Basics

## Khởi tạo

```bash
git init                        # tạo repo mới trong thư mục hiện tại
git clone <url>                 # clone repo từ remote
git clone <url> my-folder       # clone vào tên folder khác
git clone --depth 1 <url>       # shallow clone (chỉ commit mới nhất)
```

---

## Config

```bash
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"   # VS Code
git config --global init.defaultBranch main

git config --list               # xem tất cả config
git config user.name            # xem 1 giá trị
```

---

## Staging & Committing

```bash
git status                      # xem working tree + staging area
git status -s                   # short format: M = modified, A = added, ? = untracked

git add file.cpp                # stage 1 file
git add src/                    # stage cả folder
git add .                       # stage tất cả thay đổi
git add -p                      # interactive — chọn từng hunk

git commit -m "feat: add login" # commit với message
git commit -am "fix: typo"      # add tracked files + commit (bỏ qua untracked)
git commit --amend              # sửa commit cuối (message hoặc thêm file)
git commit --amend --no-edit    # amend mà không đổi message
```

---

## Diff

```bash
git diff                        # working directory vs staging area
git diff --staged               # staging area vs last commit
git diff HEAD                   # working directory vs last commit
git diff main..feature          # so sánh 2 branch
git diff abc123..def456         # so sánh 2 commit
git diff -- file.cpp            # chỉ 1 file
```

---

## Log

```bash
git log                         # full log
git log --oneline               # ngắn gọn: hash + message
git log --oneline --graph --all # đồ thị branch
git log -5                      # 5 commits gần nhất
git log --author="Alice"        # lọc theo tác giả
git log --since="2 weeks ago"
git log --grep="fix"            # lọc theo message
git log -- file.cpp             # commits ảnh hưởng file này
git log -p                      # kèm diff của mỗi commit

git show <hash>                 # xem chi tiết 1 commit
git show HEAD~2                 # xem commit 2 bước trước HEAD
```

---

## Ignoring Files — `.gitignore`

```gitignore
# Build output
build/
*.o
*.exe

# IDE
.vscode/
.idea/
*.user

# OS
.DS_Store
Thumbs.db

# Logs
*.log
logs/

# Secrets
.env
config/secrets.json
```

```bash
git check-ignore -v file.txt    # kiểm tra tại sao file bị ignore
git rm --cached file.txt        # untrack file đã commit (sau khi add vào .gitignore)
```

---

## Tóm tắt

```bash
git init           → tạo repo
git clone <url>    → clone từ remote

git status         → trạng thái
git add .          → stage
git commit -m ""   → lưu snapshot
git log --oneline  → xem lịch sử

git diff           → working vs staged
git diff --staged  → staged vs last commit
```
