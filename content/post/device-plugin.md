---
title: "Kubernetes Device Plugins 筆記 - 以 NVIDIA GPU 為例"
date: 2026-11-07T22:38:00+08:00
draft: false
toc: false
images:
tags:
  - Kubernetes
  - AI Infra
---

## 前言

近期因工作上接觸 GPU 在 Kubernetes (下稱 K8s) 中如何被運用的相關議題，花不少時間在看 Device Plugin 的實作，因此記錄下來。

## 什麼是 Device Plugin？

由於 K8s 本身並不認識 GPU 這個資源，如果只是單純在節點上插 GPU、安裝驅動程式，並不足以讓 K8s 可以利用 GPU 資源。K8s 官方提出了 Device Plugin 這個機制，讓各個特定廠商的硬體裝置（包括但不限於 GPU、FPGA、InfiniBand 等）可以向 K8s「註冊」，更準確的說，是透過 kubelet gRPC service 暴露的 Registration 介面，由 Device Plugin 向 kubelet 註冊該硬體裝置的資源名稱、Unix Socket、API 版本等（例如 `nvidia.com/gpu` 就是 NVIDIA 系列 GPU 註冊在 K8s 中的資源名稱），並由 kubelet 定期向 API Server 回報裝置數量，Patch 在節點的資訊中。

kubelet 暴露的 rpc 介面：

```go
service Registration {
	rpc Register(RegisterRequest) returns (Empty) {}
}
```

## Device Plugin 介面
