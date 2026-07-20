# niri-center-build Spec

## Goal

```text
Niri v26.04 / 8ed0da4
└── 应用 PR #4181 / 82f59e2
    └── GitHub Actions 构建 Arch 安装包
        └── 提供独立的 /usr/bin/niri-center
```

## Project

```text
niri-center-build/
├── .github/workflows/build.yml
├── patches/82f59e2.patch
├── PKGBUILD
├── README.md
└── SPEC.md
```

## Contract

```text
Input
├── Niri commit 8ed0da4
└── Patch commit 82f59e2

Output
└── niri-center-*.pkg.tar.zst
    └── /usr/bin/niri-center
```

## Constraints

```text
Build
├── 只编译 Niri 及其依赖
├── 使用 Arch Linux 构建环境
└── 不引入 PR 基线之后的其他 Niri 提交

Install
├── 不覆盖 /usr/bin/niri
├── 不修改现有 Niri 配置
└── 保留系统原版作为回滚入口
```

## Acceptance

```text
GitHub Actions
├── 构建成功
└── 上传 .pkg.tar.zst artifact

Package
├── 可由 pacman 安装
└── 提供 /usr/bin/niri-center

Behavior
├── center-single-window 配置可解析
├── 较矮的单窗口列垂直居中
├── 多窗口列恢复普通排列
└── 横向滚动及 DP-1 输出配置不变
```
