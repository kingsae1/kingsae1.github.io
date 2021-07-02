---
title: Git Gitflow
subtitle: Gitflow에 대한 개념 정리
description: Gitflow에 대한 개념 정리
date: 2017-08-01 23:30:09
layout: post
category: git
tags:
  - git
  - gitflow
author: kingsae
paginate: true
image: https://cdn-images-1.medium.com/max/1600/1*cMj8QLeDCUryuZjp93GuYQ.png
---

## Gitflow

현재 프로젝트들은 신규프로젝트 이외에 유지보수등으로 점차 코드들에 무결함에 대해서 관심이 많아지고 있다.
이를 위해 Git을 통해 코드리뷰하는 기업들이 많아지고 있어 어떤식으로 이뤄지는지 확인해보자.

### 1. 업무 흐름 (Workflow)

<img src="https://cdn-images-1.medium.com/max/1600/1*cMj8QLeDCUryuZjp93GuYQ.png">

Pic 1. Gitlab Workflow프로젝트가 진행되면 다음과 같이 진행될 것이다. 이때 Master권한을 가진 사람이 Group > Member를 생성하고 이들에게 Permission을 설정하게 될것이다.
gitflow에서는 Permission을 아래와 같이 정의한다
Owner

- Group 등록한 팀원으로 모든 권한을 가지고 있다.
- PM이 이에 해당한다.
  Master
- Group 수정/삭제와 Project 이동/삭제를 제외하고 Owner와 같은 권한을 가지고 있다.
- 업무를 할당하고 소스코드에 대한 Merge Reqeust를 승인하는 팀원으로 PL이 이에 해당한다.
  Developer
- 프로젝트를 수행하는 팀원이 이에 해당한다.
- 새로운 브랜치를 만들고 push가 가능하지만 protected 브랜치에는 push가 불가능하다.
- 기본적으로 master 브랜치가 protected 브랜치로 생성되는데 Developer는 master 브랜치로 push가 불가하다.
- master 브랜치로 Merge하기 위해서는 Merge Reqeust 작성해서 Asignee의 승인이 필요하다.
  Reporter
- Project 조회가 가능하고 Issue와 Label에 대한 관리도 가능하다.
- QA나 빌드/배포 팀원이 이에 해당한다.
  Guest
- Issue 등록과 comment 남기는 것, Group & Project 조회 가능하다.

### 2. GitFlow

<img src="https://cdn-images-1.medium.com/max/1600/1*DMCY5LUOHUoBr2Xfwy3KFQ.png">
Pic 2. GitflowStep 1. (Developer) Clone Project (최초 Project 체크아웃 시에):

```
    git clone git@example.com:project-name.git
```

Step 2. (Developer) feature 브랜치 생성 및 체크아웃:

```
git checkout -b $feature_name
```

Step 3. (Developer) 코드 작성 후 코드 commit:

```
git commit -am "My feature is ready"
```

Step 4. (Developer) 코드를 GitLab으로 push:

```
git push origin $feature_name
```

Step 5. (Developer) Commits 페이지에서 자체 코드 리뷰. 동료 리뷰가 필요한 경우에는 팀 동료에게 함께 리뷰해 달라고 한다.

Step 6. (Developer) Merge request 생성.

Step 7. (Master) Developer와 함께 코드 리뷰를 하고 master 브랜치로 Merge 한다.

Step 8. (Master) issue를 close(완료) 한다.

Step 9. (Master) 제품이 Release(상용반영)가 되면 해당 commit 시점으로 Tag를 생성한다.

> 참고 : http://developer.gaeasoft.co.kr/development-guide/workflow/gitlab-workflow-guide/
