# Branch

## (1) Branch 정의

> 프로그래밍 함에 있어서 `'독립적인 작업흐름'`을 만들고 관리하기 위해 활용하는 작업공간이다.

---

## (2) 명령어

### **1) 브랜치 생성**

```bash
$ git branch {branch_name}
```

### **2) 브랜치 이동**

```bash
<2가지 방법>

$ git checkout {branch_name}

$ git switch {branch_name}
```

### **3) 브랜치 생성 및 이동**

```bash
$ git checkout -b {branch_name}
```

### **4) 브랜치 목록확인**

```bash
$ git branch
```

### **5) 브랜치 삭제**

```bash
$ git branch -d {branch_name}
```

### **6) Merge**

: 각 branch에서 작업을 한 이후, 이력을 합치기 위해 `merge` 명령어를 사용한다.

**<병합시, 서로 다른 commit에서 `동일한 파일`을 수정한 경우>**
  - 충돌(coflict)이 발생하기에 해당 문제 파일을 확인하여 적절하게 수정해야 한다.
  - 수정한 이후에 commit을 실행한다.

**<병합시, 서로 다른 commit에서 `다른 파일`을 수정한 경우>**
  - 충돌없이 자동으로 `Merge Commit`이 생성됨

```bash
(master) $ git merge {branch_name}

<주의!> 상단의 명령의 의미 : 현재 있는 브랜치(master)로 다른 브랜치(branch_name)의 작업을 가져온다.
```

- **Merge -fast forward**

: 기존 master 브랜치에 변경사항이 없어 단순히 앞으로 이동한다.

```
1. 다른 브랜치로 이동하여 작업 후 commit을 한다.
2. 기존의 master 브랜치에서 변경사항이 없을 경우, master 브랜치로 병합한다.
```

- **Merge -merge commit**

: 기존 master 브랜치에 변경사항이 있어 병합 커밋이 발생하였다.

```
1. 다른 브랜치로 이동하여 작업 후 commit을 한다.
2. 기존의 master 브랜치도 수정 후 commit을 하고 master 브랜치로 병합한다.