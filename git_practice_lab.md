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

### 4-6. 충돌 해결하기 (VS Code GUI 기능 활용)
코드 최상단에 작게 표시되는 **VS Code의 GUI 선택 버튼**을 클릭하여 클릭 한 번으로 해결할 수 있습니다.
* **Accept Current Change** 클릭 시: `main`의 내용만 남고 특수문자는 자동 삭제됩니다.
* **Accept Incoming Change** 클릭 시: `feature/greeting`의 내용만 남고 특수문자는 자동 삭제됩니다.
* **Accept Both Changes** 클릭 시: 두 내용이 다 남습니다.

> 💡 **이번 실습에서는 `Accept Both Changes`를 선택하거나, 두 내용을 자연스럽게 한 문장으로 합친 후 특수 문자(`<<<<`, `====`, `>>>>`)를 직접 지우고 저장해 보세요.**

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

## 👥 Track 2: 2인 1조 협업 실습 코스

> 💡 **이 트랙은 2인 1조(팀원 A, 팀원 B)로 구성하여 실제 협업 중에 충돌을 겪고 머지하는 실전 롤플레잉입니다.**
> * **역할 분담**: 팀원 중 1명이 대표로 GitHub 레포지토리를 만들고, 다른 팀원을 **Collaborator**(Settings -> Collaborators)로 초대하여 권한을 부여한 뒤 시작합니다.

### 1단계: 각자 브랜치 파서 작업하기 (팀원 A, B 공통)
두 팀원 모두 로컬의 최신 `main` 브랜치에서 출발합니다.
```bash
# [공통] 최신 소스 가져오기
git switch main
git pull origin main

# [팀원 A] 브랜치 생성 및 이동
git switch -c feature/member-a

# [팀원 B] 브랜치 생성 및 이동
git switch -c feature/member-b
```

### 2단계: 같은 파일의 같은 줄 다르게 수정하기 (팀원 A, B 공통)
* **파일**: 두 팀원 모두 레포지토리에 있는 `README.md` 파일을 수정합니다.
* **수정 위치**: 파일 가장 아래쪽에 각자의 한 줄 인사를 적습니다.
  * **팀원 A 수정**: `## 개발자 A의 인사말: 안녕하세요. 저는 팀원 A입니다.`
  * **팀원 B 수정**: `## 개발자 B의 인사말: 반가워요. 저는 팀원 B입니다.`
* **수정 후 각자 커밋 & 푸시**:
  ```bash
  # [공통]
  git add README.md
  git commit -m "docs: README 인사말 작성"
  git push origin feature/member-a (또는 feature/member-b)
  ```

### 3단계: 팀원 A가 먼저 PR 생성 및 머지하기 (선공)
1. **팀원 A**가 GitHub에서 `feature/member-a` ➔ `main` 방향으로 PR을 생성합니다.
2. 팀 내 **PR 담당자(혹은 팀원 B)**가 PR 코드를 리뷰하고 **Merge pull request**를 눌러 머지를 완료합니다.
3. 이제 GitHub의 `main` 브랜치에는 **팀원 A가 쓴 한 줄**이 포함되었습니다.

### 4단계: 팀원 B가 로컬 최신화하며 충돌 마주하기 (후공)
팀원 B는 본인의 작업을 올리기 전에, main의 최신 코드(A가 수정한 것)를 끌어와야 합니다.
```bash
# [팀원 B] 내 브랜치 상태에서 main을 끌어옵니다
git pull origin main
```
#### 💥 충돌 발생!
```
CONFLICT (content): Merge conflict in README.md
```
* **이유**: 팀원 B가 로컬에서 작성한 파일 줄과, 이미 GitHub main에 올라간 팀원 A의 파일 줄이 서로 다르기 때문에 발생합니다.

### 5단계: 팀원 B가 충돌 해결하고 마무리하기 (협업 완료)
1. **팀원 B**는 VS Code에서 `README.md`를 열고, 상단의 GUI 버튼 중 **`Accept Both Changes`**를 클릭하여 팀원 A와 팀원 B의 인사말이 위아래로 둘 다 남도록 조율합니다.
2. 기호가 말끔히 정리된 것을 확인한 후 저장하고 머지를 매듭짓습니다.
   ```bash
   # [팀원 B]
   git add README.md
   git commit -m "merge: A의 인사말과 B의 인사말 병합"
   git push origin feature/member-b
   ```
3. GitHub으로 돌아가 `feature/member-b` ➔ `main` PR을 열면 **충돌 없이(This branch has no conflicts) 즉시 머지 가능한 상태**가 됩니다.
4. 머지 버튼을 클릭하여 마무리합니다!

---

## 🏆 실습 완료! 배운 것 정리

| 실습 코스 | 핵심 학습 개념 | 추천 대상 |
|---|---|---|
| **Track 1 (1인)** | 로컬 내 브랜치 교차 머지 충돌 체험, IDE 충돌 해결 버튼 사용법 | 개인 학습자 |
| **Track 2 (2인)** | GitHub Collaborator 초대, PR 코드 리뷰 및 머지 승인, 실전 동시 작업 충돌 해결 | 팀 프로젝트 준비팀 |
