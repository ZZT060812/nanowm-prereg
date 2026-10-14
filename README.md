# NanoWM 预注册时间戳库

本仓库用于 NanoWM 论文全部预注册（预测带 / 判决分支 / 协议常数）的**第三方时间戳验证**：
预测在结果产出之前即已固化，防止 HARKing（先看数据再写假设）质疑。

## 验证方法（给审稿人/读者）

1. **提交历史**：每个预注册文件首次出现的 commit 即其"出生时间"
   `git log --follow --format="%H %aI" -- prereg/<file>`
2. **服务器侧时间（防伪造的关键）**：commit 的 author date 理论上可伪造，但 GitHub 的
   **pushed_at / commit push 事件**是服务器记录、不可回拨——用 GitHub API 核对：
   `https://api.github.com/repos/<owner>/nanowm-prereg/commits/<sha>` 的 push 时间线
3. **标签冻结点**：每个批次开跑前打 annotated tag（如 `batch0-anchor-frozen`），
   tag 与其推送时间构成"跑前快照"；重大冻结点（数据冻结/工程冻结）额外发 GitHub Release，
   Release 的创建时间是纯服务器侧时间戳
4. 验证主张的形式："prediction X was timestamped before experiment Y ran"——
   由 本仓库 commit/push 时间 vs 实验日志时间（节点上的训练日志 mtime/时间戳）双向比对

## 目录约定

```
prereg/YYYY-MM/        按月归档：协议 JSON（机器读）+ MD（人读）
queue_sync/            从实验队列闸门（queue/prereg/*.json）自动同步的投放件
tags:                  batch<N>-<名>-frozen = 该批开跑前冻结点
releases:              freeze-<里程碑> = 数据冻结/工程冻结
```

## 纪律（与排期总表 §四 一致）

- 预测行/判决分支**先落本仓库再开跑**（队列闸门文件即本仓库对应文件的副本）
- 出数后不得改写已冻结文件——新读数以新文件追加（后缀 _result），保持预测原样供打分
- 每次投放 = commit + （批次级）tag；commit message 用中文一行说明批次与内容
