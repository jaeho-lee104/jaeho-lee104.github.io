---
title: git commit name, email 설정
author: leejaeho
date: 2021-07-26 01:45:00 +0900
categories: [Git]
tags: [git config]
toc: false
---

## 전체 repository 에 name, email 설정
1. 터미널 열기
2. git config --global user.name "NAME"
3. git config --global user.email "EMAIL@example.com"
4. git config --list 로 확인

## repository 별 name, email 설정
1. 터미널 열기
2. git config user.name "NAME"
  - git config --local user.name "NAME" 과 같음
3. git config user.email "EMAIL@example.com"
4. cat .git/config 로 확인

