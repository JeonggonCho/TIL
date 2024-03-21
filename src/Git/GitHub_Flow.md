# GitHub Flow

## 목록

1. [Git Flow 의미](#1-git-flow-의미)
2. [Git Flow 유형](#2-github-flow-유형)
    1. [Shared Repository Model](#2-1-shared-repository-model)
    2. [Fork $ Pull Model](#2-2-fork--pull-model)

<br>
<br>

## 1. Git Flow 의미

> Git Flow는 Git을 활용하여 `협업하는 일련의 과정`으로 Branch를 통해 이루어진다. 정해진 원칙이 있는 것이 아니며, 각 서비스 및 기능에 따라 제안되는 흐름이 있으며, 각각의 프로젝트 및 회사에 따라 `상이하게 활용`되고 있다.

<br>
<br>

## 2. GitHub Flow 유형

### 2-1. Shared Repository Model

-   동일 저장소를 `공유`하여 활용하는 유형이다.
-   작업은 Master + feature 브랜치로 구성하여 진행한다.
-   구성원 : repository owner(Project manager) + `collaborator`
-   collaborator는 해당 저장소에 대한 `push` 권한이 부여된다.

<br>

### 2-2. Fork $ Pull Model

-   collaborator로 `등록되지 않은 상태`에서 진행되는 유형이다.
-   주로 오픈소스 참여시, 쓰이는 과정이다.
-   구성원 : repository owner(Project manager) + `contributor`
-   가져올 repository를 `Fork`하고, `clone`을 통해 작업을 한 후, 본인의 저장소에 `push`를 한다.
-   기존 Project manager에게 기여하려는 경우, `pull request`를 보내 상대방 수락한다면 contributor가 될 수 있다.
