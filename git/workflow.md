# Git Workflows & Conventions

## GitHub Flow — Đơn giản, phổ biến nhất

```
main  ──────────────────────────────────── (luôn deployable)
           ↑ merge PR       ↑ merge PR
feature/A ──────          feature/B ──────
```

```bash
# 1. Tạo branch từ main
git switch main && git pull
git switch -c feature/user-auth

# 2. Commit thường xuyên
git add .
git commit -m "feat(auth): add JWT validation"

# 3. Push và tạo Pull Request
git push -u origin feature/user-auth
# → Tạo PR trên GitHub/GitLab

# 4. Code review → PR approved → Merge vào main
# 5. Xóa branch sau khi merge
git fetch --prune
git branch -d feature/user-auth
```

**Rules:**
- `main` luôn deployable
- Branch cho mọi feature/fix
- Merge qua Pull Request (code review)
- Deploy ngay sau khi merge

---

## Git Flow — Nhiều môi trường

```
main     ──────────────────────────  (production)
              ↑ merge release
release/1.2  ────────
              ↑ merge
develop  ──────────────────────────  (staging)
         ↑ feature merge  ↑ feature merge
feature/A ──            feature/B ──
```

| Branch | Mục đích |
|--------|---------|
| `main` | Production code, luôn stable |
| `develop` | Integration branch |
| `feature/*` | New features (từ develop) |
| `release/*` | Release preparation |
| `hotfix/*` | Emergency fix trực tiếp vào main |

---

## Conventional Commits

Format chuẩn cho commit message — cho phép auto-generate CHANGELOG và versioning.

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types

| Type | Ý nghĩa | Version bump |
|------|---------|-------------|
| `feat` | Tính năng mới | MINOR (1.x.0) |
| `fix` | Bug fix | PATCH (1.0.x) |
| `docs` | Documentation | - |
| `style` | Format, không đổi logic | - |
| `refactor` | Refactor, không fix/feat | - |
| `perf` | Performance improvement | PATCH |
| `test` | Thêm/sửa tests | - |
| `chore` | Build, CI, config | - |
| `ci` | CI/CD | - |
| `revert` | Revert commit | - |

### Examples

```bash
feat(auth): add OAuth2 login with Google

fix(parser): handle empty string input correctly

docs(readme): update installation instructions

refactor(core): extract validation logic into separate module

perf(search): use binary search instead of linear scan
# Reduces search time from O(n) to O(log n)

feat!: change API response format
# BREAKING CHANGE: response now wraps data in { data: ... }
```

### Breaking Changes

```
feat!: rename config keys
# Hoặc trong footer:
feat(api): new endpoint

BREAKING CHANGE: /users endpoint now requires authentication
```

---

## Good Commit Message Habits

```
✓ Imperative mood: "Add feature" (không phải "Added feature")
✓ Ngắn gọn: < 72 ký tự dòng đầu
✓ Giải thích WHY, không chỉ WHAT
✓ Reference issue: "fix #123", "closes #456"

✗ "fix bug"
✗ "WIP"
✗ "asdfasdf"
✗ "Update file.cpp"

✓ "fix(login): prevent null pointer dereference on empty username"
✓ "feat(cache): add LRU eviction policy for memory efficiency"
```

---

## Pull Request Best Practices

```
PR nên:
  ✓ Nhỏ và focused (< 400 lines thay đổi)
  ✓ Có description rõ ràng: WHY, HOW, screenshots nếu có UI
  ✓ Tự review trước khi assign reviewer
  ✓ Link đến issue: "Closes #123"
  ✓ Tests pass, no lint errors

Review nên:
  ✓ Review trong 24h
  ✓ Approve khi OK, request changes khi cần fix
  ✓ Comment cụ thể, constructive
  ✓ Phân biệt must-fix và suggestion (nit:)
```

---

## Tóm tắt

```
GitHub Flow:
  main ← feature branch ← PR ← review ← merge

Conventional Commits:
  feat(scope): description    → new feature
  fix(scope): description     → bug fix
  feat!: description          → breaking change

PR size: nhỏ = review tốt hơn = merge nhanh hơn
```
