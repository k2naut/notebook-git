# Remote Repositories

## Remote setup

```bash
git remote -v                           # xem remotes
git remote add origin <url>             # thêm remote
git remote add upstream <url>           # thêm upstream (fork workflow)
git remote rename origin new-name
git remote remove origin
git remote set-url origin <new-url>     # đổi URL
```

---

## Push & Pull

```bash
git push origin main                    # push branch main lên origin
git push                                # push branch hiện tại (nếu có tracking)
git push -u origin feature/login       # push + set upstream tracking
git push --force-with-lease             # force push an toàn (check remote chưa có commit mới)
git push --force                        # force push (nguy hiểm — rewrite history)
git push origin --delete feature/login # xóa remote branch

git pull                                # fetch + merge (tracking branch)
git pull origin main                    # fetch + merge từ origin/main
git pull --rebase                       # fetch + rebase (linear history)
git pull --rebase=interactive           # fetch + interactive rebase
```

---

## Fetch

```bash
git fetch                       # lấy changes từ remote, KHÔNG merge
git fetch origin                # fetch từ origin
git fetch --all                 # fetch tất cả remotes
git fetch --prune               # xóa remote-tracking branches đã bị xóa ở remote

# Sau fetch, xem changes trước khi merge
git log HEAD..origin/main --oneline
git diff HEAD origin/main
git merge origin/main           # merge thủ công
```

---

## Tracking Branches

```bash
git branch -vv                  # xem tracking info của mỗi branch
# * main    abc123 [origin/main] latest commit
# feature   def456 [origin/feature: ahead 2] 2 commits ahead

git branch --set-upstream-to=origin/main main   # set tracking
git branch -u origin/feature feature            # short form
```

---

## Fork Workflow (GitHub/GitLab)

```bash
# 1. Fork repo trên GitHub
# 2. Clone fork của bạn
git clone https://github.com/yourname/repo.git
cd repo

# 3. Add upstream (original repo)
git remote add upstream https://github.com/original/repo.git

# 4. Sync với upstream
git fetch upstream
git checkout main
git merge upstream/main          # hoặc: git rebase upstream/main
git push origin main             # cập nhật fork

# 5. Tạo feature branch
git switch -c feature/my-feature

# 6. Push và tạo Pull Request
git push -u origin feature/my-feature
```

---

## SSH vs HTTPS

```bash
# HTTPS (cần token hoặc credential helper)
git clone https://github.com/user/repo.git
git remote set-url origin https://github.com/user/repo.git

# SSH (cần SSH key setup)
git clone git@github.com:user/repo.git
git remote set-url origin git@github.com:user/repo.git

# Generate SSH key
ssh-keygen -t ed25519 -C "your@email.com"
cat ~/.ssh/id_ed25519.pub       # copy vào GitHub Settings → SSH Keys
ssh -T git@github.com           # test connection
```

---

## Tóm tắt

```bash
git remote add origin <url>     → thêm remote
git push -u origin main         → push + set tracking lần đầu
git push                        → push sau đó
git pull --rebase               → lấy về + rebase (linear history)
git fetch + git merge           → fetch + merge thủ công (an toàn hơn)

Force push:
  git push --force-with-lease   → safe force push (recommended)
  git push --force              → nguy hiểm, chỉ trên private branch
```
