# Git

## - 목차
1. [Git의 의미](#1-git의-의미)
2. [기존 문서환경과 비교](#2-기존-문서환경과-비교)
3. [분산버전관리시스템(DVCS)](#3-분산버전관리시스템dvcs)
4. [Git명령어](#4-git명령어)
5. [Git config 설정](#5-git-config-설정)
    - [사용자 정보 설정하기](#1-사용자-정보-설정하기)
    - [사용자 정보 삭제하기](#2-사용자-정보-삭제하기)
6. [Git관리](#6-git관리)

---

## (1) Git의 의미

> Git은 *협업툴, 버전관리툴* 로 흔히 인식되고 있으며 한 단어로 `'분산 버전 관리 시스템'`으로 정의 할 수 있다.

- 2005년 리눅스 커널을 위한 도구로서 *'리누스 토르발스'* 에 의해 개발되었다.

- 컴퓨터 파일의 변경사항을 추적 하고 여러 명의 사용자들 간에 해당 파일들의 작업을 조율하는 도구이다.

---

## (2) 기존 문서환경과 비교
- **기존  문서환경** : 작성한 문서를 계속해서 '다른이름으로 저장하기'를 통해서 저장할 경우, 자료 간에 어떠한 수정이 이루어졌는지 `차이점(diff), 변경부분`을 알기 어렵다.

- **Git 이용** : `차이점(diff)`와 `수정이유를 메세지`로 남길 수 있어 현재 파일들을 안전한 상태에서 과거 모습의 상태로 복원이 가능하다. 마찬가지로 과거파일에서 현재의 파일로도 롤백이 가능하다.

```
ex)

크로미움(크롬 브라우저의 오픈소스)의 경우, 모든 버전의 용량이 약 25GB에 달하지만 Git을 통해 관리되어 최신 버전의 용량은 1.58GB밖에 되지 않는다.
```

---

## (3) 분산버전관리시스템(DVCS)

- **중앙집중식버전관리시스템** : 중앙에서 버전을 관리하고 파일을 받아서 사용하는 시스템
- **분산버전관리시스템** : `원격저장소(remote repository)`를 통하여 협업하고, 모든 히스토리를 클라이언트들이 공유하는 시스템

---

## (4) Git명령어

- **git init** : 특정 폴더를 git 저장소로 만들어 git으로 정리

```
git bash에서 (master)로 표기된 것으로 확인 할 수 있다.
```

- **git add <file>** : working directory상의 `변경내용을 staging area에 추가(add)`하기 위해 사용하며 여러 개의 파일을 staging area에 추가하는 것이 가능하다.

```
- 'untracked' 상태의 파일을 'staged'로 변경
- 'modified' 상태의 파일을 'staged'로 변경
```

- **git commit -m'<커밋 메시지>'** : staged 상태의 파일들을 `커밋(commit)을 통해 버전으로 기록`하며, 커밋된 staged 파일은 동일한 버전이 부여된다.

```
!! 커밋 메시지는 '변경사항을 나타낼 수 있도록' 명확하게 작성해야 한다.
```

- **git status** : `working directory` 와 `staging area` 의 현재 상태를 조회 할 수 있다.

```
파일의 라이프사이클

- Unmodified : 아직 수정이 안 된 상태로 git status 에 나타나지 않음
- modified : 파일이 수정된 상태(add 명령어를 통해 staging area로 전달)
- staged : 수정한 파일을 곧 커밋할 것이라고 표시한 상태(commit 명령어로 저장소에 전달)
- committed : 커밋이 된 상태

<매우 중요!> Status 로 확인 할 수 있는 파일상태

- Untracked files : 버전으로 관리된 적 없는 파일(새파일) -> working directory 에 들어있음
- Tracked : 이전부터 버전으로 관리되고 있는 파일
- Changes not staged for commit : modified -> working directory 에 들어있음
- Changes to be committed : staged -> staging area 에 들어있음
- Nothing to commit, working tree clean : working directory, stagign area 모두 비어있음
```

- **git log** : 현재 `저장소(repository)` 에 기록된 커밋, 버전을 조회 할 수 있다.

- **git help** : git 의 모든 명령어 및 정보를 볼 수 있다.

---

## (5) Git config 설정

### **1) 사용자 정보 설정하기**


: 변경사항을 저장(commit)하기 위해서는 `사용자의 정보(이름과 이메일)`가 필요하다.

  ```bash
  <사용자 정보 등록>

  $ git config user.name "<user_name>"
  $ git config user.email"<user_email>"
  ```

### **2) 사용자 정보 삭제하기**


: 설정된 사용자를 지우기 위해서는 `unset`을 이용한다.

  ```bash
  <사용자 정보 삭제>

  $ git config --unset user.name
  $ git config --unset user.email
  ```

- **git config --system** : 시스템의 모든 사용자와 모든 저장소에 관리자 권한 적용

- **git config --global** : 현재 사용자에게 적용되는 설정 조회

- **git config --local** : 특정 저장소에만 적용되는 설정 조회

---

## (6) Git관리

**Q1) git으로 저장되는 폴더의 이름을 바꿔도 된다?**

```
(O) 바꿔도 문제되지 않음. `.git폴더`가 내부에 종속되어있기에 마스터 폴더의 이름을 변경하여도 상관없다.
```

**Q2) .git폴더 자체를 삭제하거나 수정해도 된다?**

```
(X) .git폴더를 삭제하거나 수정시 버전이 손상되고 삭제될 수 있다.
```