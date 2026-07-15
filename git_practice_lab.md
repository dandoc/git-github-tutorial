# 🏋️ Git 실전 실습: 혼자서 해보는 브랜치 & 충돌 해결

> **소요 시간**: 약 20~30분
> **준비물**: Git이 설치된 컴퓨터, GitHub 계정, 터미널(Git Bash 또는 PowerShell)
> **목표**: 브랜치를 만들고, 커밋하고, 병합 충돌을 직접 만들어 해결해 보기

---

## 📌 실습 목차
1. [실습 환경 준비](#1-실습-환경-준비)
2. [첫 번째 커밋 만들기](#2-첫-번째-커밋-만들기)
3. [브랜치를 나눠서 작업하기](#3-브랜치를-나눠서-작업하기)
4. [병합 충돌 만들고 해결하기](#4-병합-충돌-만들고-해결하기)
5. [GitHub에 올리고 Pull Request 만들기](#5-github에-올리고-pull-request-만들기)

---

## 1. 실습 환경 준비

### 1-1. GitHub에서 실습용 저장소 만들기
1. [GitHub](https://github.com)에 로그인합니다.
2. 우측 상단 **[+]** → **New repository** 클릭
3. Repository name: `git-practice` 입력
4. ✅ **Add a README file** 체크
5. **Create repository** 클릭

### 1-2. 내 컴퓨터로 가져오기 (Clone)
```bash
# GitHub 저장소를 내 컴퓨터로 복사합니다
git clone https://github.com/내아이디/git-practice.git

# 복사된 폴더로 이동합니다
cd git-practice
```

### ✅ 확인
```bash
git status
```
아래와 비슷한 메시지가 나오면 성공입니다:
```
On branch main
nothing to commit, working tree clean
```

---

## 2. 첫 번째 커밋 만들기

### 2-1. 파일 만들기
에디터 또는 터미널에서 `hello.txt` 파일을 생성합니다.
```bash
echo "안녕하세요! Git 실습 중입니다." > hello.txt
```

### 2-2. 상태 확인하기
```bash
git status
```
**예상 결과** — 빨간색 글씨로 새 파일이 표시됩니다:
```
Untracked files:
  hello.txt
```
> 💡 **의미**: `hello.txt`는 방금 만든 새 파일이라 Git이 아직 추적하지 않는(Untracked) 상태입니다.

### 2-3. 스테이징 → 커밋
```bash
# 커밋 준비 상자에 담기
git add hello.txt

# 상태 확인 — 초록색으로 바뀌었는지 확인!
git status

# 커밋 생성
git commit -m "feat: hello.txt 파일 추가"
```

### 2-4. GitHub에 올리기
```bash
git push origin main
```

### ✅ 확인
GitHub 저장소 페이지를 새로고침하면 `hello.txt` 파일이 보입니다!

---

## 3. 브랜치를 나눠서 작업하기

이제 **main 브랜치를 건드리지 않고** 별도의 작업 공간(브랜치)에서 개발하는 연습을 합니다.

### 3-1. 작업 브랜치 만들고 이동하기
```bash
# 새 브랜치를 만들면서 동시에 이동
git switch -c feature/greeting
```

### ✅ 확인
```bash
git branch
```
**예상 결과** — 현재 브랜치에 `*` 표시가 붙습니다:
```
* feature/greeting
  main
```

### 3-2. 브랜치에서 파일 수정하기
`hello.txt`를 열어 내용을 수정합니다:
```bash
echo "안녕하세요! Git 실습 중입니다." > hello.txt
echo "이 줄은 feature/greeting 브랜치에서 추가했습니다!" >> hello.txt
```

### 3-3. 수정사항 커밋하기
```bash
git add hello.txt
git commit -m "feat: greeting 메시지 추가"
```

### 3-4. 브랜치를 GitHub에 올리기
```bash
git push origin feature/greeting
```

### ✅ 확인
GitHub 저장소 페이지에서 브랜치 드롭다운을 클릭하면 `feature/greeting`이 보입니다!

---

## 4. 병합 충돌 만들고 해결하기

> 🎯 **이 실습의 핵심입니다!** 같은 파일의 같은 줄을 두 브랜치에서 다르게 수정하면 Git이 자동으로 합칠 수 없어서 **충돌(Conflict)**이 발생합니다. 이것을 직접 만들어 보고 해결해 봅니다.

### 4-1. main 브랜치로 돌아가기
```bash
git switch main
```

### 4-2. main에서 같은 파일의 같은 줄을 다르게 수정하기
```bash
echo "안녕하세요! Git 실습 중입니다." > hello.txt
echo "이 줄은 main 브랜치에서 추가한 다른 내용입니다!" >> hello.txt
```

### 4-3. main에서 커밋하기
```bash
git add hello.txt
git commit -m "docs: main에서 hello.txt 수정"
```

### 4-4. feature/greeting 브랜치를 main에 병합 시도하기
```bash
git merge feature/greeting
```

### 💥 충돌 발생!
터미널에 아래와 비슷한 메시지가 나타납니다:
```
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### 4-5. 충돌 확인하기
`hello.txt`를 에디터(VS Code 등)로 열어봅니다:
```
안녕하세요! Git 실습 중입니다.
<<<<<<< HEAD
이 줄은 main 브랜치에서 추가한 다른 내용입니다!
=======
이 줄은 feature/greeting 브랜치에서 추가했습니다!
>>>>>>> feature/greeting
```

**읽는 법:**
| 구간 | 의미 |
|---|---|
| `<<<<<<< HEAD` ~ `=======` | 현재 내가 서 있는 브랜치(main)의 코드 |
| `=======` ~ `>>>>>>> feature/greeting` | 합치려는 브랜치(feature/greeting)의 코드 |

### 4-6. 충돌 해결하기
특수 기호(`<<<<<<<`, `=======`, `>>>>>>>`)를 **모두 지우고**, 최종적으로 남기고 싶은 내용만 적습니다.

예시 — 두 내용을 합치기로 결정:
```
안녕하세요! Git 실습 중입니다.
이 줄은 main과 feature/greeting의 내용을 합쳐서 정리한 최종본입니다!
```

### 4-7. 해결 완료 후 커밋하기
```bash
# 충돌이 해결된 파일을 스테이징
git add hello.txt

# 머지 커밋 생성
git commit -m "merge: feature/greeting 병합 및 충돌 해결"
```

### ✅ 확인
```bash
git log --oneline -5
```
**예상 결과** — 머지 커밋이 맨 위에 보입니다:
```
abc1234 merge: feature/greeting 병합 및 충돌 해결
def5678 docs: main에서 hello.txt 수정
ghi9012 feat: greeting 메시지 추가
jkl3456 feat: hello.txt 파일 추가
mno7890 Initial commit
```

🎉 **축하합니다! 병합 충돌을 직접 만들고 해결하는 데 성공했습니다!**

---

## 5. GitHub에 올리고 Pull Request 만들기

마지막으로, 실무에서 가장 많이 쓰는 **PR(Pull Request) 워크플로우**를 연습합니다.

### 5-1. 새로운 작업 브랜치 만들기
```bash
# main 브랜치 최신화
git pull origin main

# 새 브랜치 생성 및 이동
git switch -c feature/goodbye
```

### 5-2. 새 파일 만들고 커밋하기
```bash
echo "안녕히 가세요! 실습이 끝났습니다. 👋" > goodbye.txt

git add goodbye.txt
git commit -m "feat: goodbye.txt 파일 추가"
```

### 5-3. GitHub에 브랜치 올리기
```bash
git push origin feature/goodbye
```

### 5-4. GitHub에서 Pull Request 생성하기
1. GitHub 저장소 페이지로 이동합니다.
2. 상단에 **"Compare & pull request"** 버튼이 나타나면 클릭합니다.
   - (안 보이면 **Pull requests** 탭 → **New pull request** → base: `main`, compare: `feature/goodbye` 선택)
3. 제목: `feat: goodbye 파일 추가`
4. 설명: 자유롭게 작성
5. **Create pull request** 클릭!

### 5-5. PR 머지하기
1. PR 페이지 하단의 **Merge pull request** 클릭
2. **Confirm merge** 클릭

### 5-6. 로컬 main 브랜치 최신화
```bash
git switch main
git pull origin main
```

### ✅ 최종 확인
```bash
git log --oneline -5
```
GitHub에서 머지한 커밋이 로컬에도 반영되어 있으면 **모든 실습이 완료**된 것입니다!

---

## 🏆 실습 완료! 배운 것 정리

| 실습 | 배운 명령어 | 핵심 개념 |
|---|---|---|
| 환경 준비 | `git clone`, `git status` | Working Directory, 로컬/원격 연결 |
| 첫 커밋 | `git add`, `git commit`, `git push` | Staging → Commit → Push 흐름 |
| 브랜치 작업 | `git switch -c`, `git branch` | Feature Branch 전략 |
| 충돌 해결 | `git merge`, 수동 편집, `git add` | Merge Conflict 발생 원리와 해결법 |
| PR 워크플로우 | `git pull`, GitHub PR | 코드 리뷰 기반 협업 |
