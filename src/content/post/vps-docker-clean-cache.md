---
title: VPS Docker Clean Cache
description: 清理Docker上的垃圾
tags:
  - Docker
  - VPS
pubDate: 2026-01-05
draft: false
---
![](images/vps-docker-clean-cache.png)
*Photo by [Rubaitul Azad](https://unsplash.com/@rubaitulazad?utm_source=Obsidian%20Image%20Inserter%20Plugin&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=Obsidian%20Image%20Inserter%20Plugin&utm_medium=referral)*

在 VPS 上使用 Docker，特别是频繁使用 `docker compose pull` 更新服务时，确实非常容易积累“垃圾”。这些通常是**悬空镜像 (Dangling Images)**，即旧版本的镜像被新版本替换后，标签变成了 `<none>`，但文件依然占用着硬盘。

从**检查**到**清理**的完整指南：

---

### 第一步：检查磁盘占用 (诊断)

在清理之前，建议先看一眼到底是谁吃掉了空间。

**1. 查看 Docker 整体空间占用** 这是最直观的命令，它会列出镜像、容器、数据卷和构建缓存的占用情况。

Bash

```
docker system df
```

- **Images:** 这一栏通常是重灾区。如果 `Reclaimable`（可回收）比例很高，说明有很多未使用的镜像。
    
- **Build Cache:** 如果你曾在 VPS 上构建过镜像（`docker build`），这里也可能很大。
    

**2. 查看具体的悬空镜像** 如果你想看具体哪些镜像变成了“垃圾”（即 `<none>`）：

Bash

```
docker images -f "dangling=true"
```

---

### 第二步：清理指令 (实操)

根据你的需求，由轻到重有几种清理方式。

#### 1. 核心推荐：标准清理 (最安全，针对 `compose pull`)

这个命令是专门用来清理“垃圾”的。它会删除：

- 所有**已停止**的容器
    
- 所有**悬空**的镜像 (正是 `docker compose pull` 产生的那些 `<none>` 镜像)
    
- 所有**未被使用**的网络
    
- 所有构建缓存
    

Bash

```
docker system prune
```

- _执行后会提示确认，输入 `y` 即可。_
    
- **注意：** 正在运行的容器（Up 状态）不会受影响，所以这通常是安全的。
    

#### 2. 进阶清理：强力删除 (慎用)

如果你发现空间还是不够，且确定**只保留当前正在运行的容器所需的镜像**，其他所有没用到的镜像（哪怕有标签）都不要了：

Bash

```
docker system prune -a
```

- **区别：** 加了 `-a` (all)，它不仅删除 `<none>` 镜像，还会删除所有**当前没有被任何容器使用**的镜像。
    
- _场景：_ 比如你之前 `pull` 了 nginx、redis 玩了一下然后停掉了容器，标准清理不会删这些镜像，但 `-a` 会删掉。
    

#### 3. 针对性清理 (手术刀式)

如果你只想清理镜像，不想动容器或缓存：

- **只清理悬空镜像 (推荐):**
    
    Bash
    
    ```
    docker image prune
    ```
    
- **清理构建缓存 (如果 Build Cache 很大):**
    
    Bash
    
    ```
    docker builder prune
    ```
    

---

### 第三步：关于数据卷 (Volumes) 的特别警告

**千万注意：** 默认的 `docker system prune` **不会** 清理数据卷（Volumes）。这是 Docker 的保护机制，防止你误删数据库文件。

如果你确定有很多**已经废弃**的数据卷（比如之前的测试项目留下的数据库数据）：

Bash

```
docker volume prune
```

> **⚠️ 警告：** 执行此命令前，请务必确认你没有重要数据存储在“已停止”的容器关联的 Volume 中。一旦删除，数据无法恢复。

---

### 总结：你的日常维护流程

针对你提到的 `docker compose pull` 引起的问题，建议的日常更新流程如下：

1. 拉取新镜像：
    
    Bash
    
    ```
    docker compose pull
    ```
    
2. 重启服务（后台运行）：
    
    Bash
    
    ```
    docker compose up -d
    ```
    
3. **立刻清理旧镜像（垃圾回收）：**
    
    Bash
    
    ```
    docker image prune -f
    ```
    
    - `-f` 表示 force，不需要手动输入 `y` 确认。
        

你可以把这三步写成一个简单的 shell 脚本，或者每次更新完顺手跑一下 `docker image prune -f`。