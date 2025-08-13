---
layout: post
title: "AKS + ArgoCD 이미지 태그 자동화"
date: 2025-08-13 16:10 +0900
categories: [aks, argocd]
tags: [aks, argocd, azure, github-actions, acr, kustomize]
---

> **TL;DR**: PR/Merge → GitHub Actions가 이미지 빌드·푸시(ACR) → `kustomization.yaml`의 `images.newTag` 갱신 → ArgoCD Auto-Sync로 배포.

## 준비물
- ACR/AKS/ArgoCD 구성 완료
- GitHub Secrets: `AZURE_CREDENTIALS`, `ACR_NAME`, `ACR_LOGIN_SERVER`

## 워크플로 (요약)
```bash
# 태그 예: build-${GITHUB_RUN_NUMBER}
docker build -t $ACR_LOGIN_SERVER/fastapi-demo:$TAG .
docker push  $ACR_LOGIN_SERVER/fastapi-demo:$TAG

