# Niri PR #4181 GitHub Actions 构建方案

## 目标

基于 Deck 当前使用的 Niri `v26.04`（commit `8ed0da4`），只应用 PR #4181 的提交 `82f59e2`，通过 GitHub Actions 构建不覆盖系统 Niri 的 Arch Linux 安装包。

## 项目结构

```text
niri-center-build/
├── .github/
│   └── workflows/
│       └── build.yml          # Arch Linux 构建并上传 artifact
├── patches/
│   └── 82f59e2.patch          # PR #4181 的单提交补丁
├── PKGBUILD                   # 基于 Niri v26.04 构建自定义包
└── README.md                  # 构建、安装、验证和回滚说明
```

## 构建链路

```text
Niri v26.04 / 8ed0da4
└── 应用 patches/82f59e2.patch
    └── Arch/CachyOS 环境完整编译
        └── 打包为 niri-center
            └── GitHub Actions 上传 .pkg.tar.zst
```

## 文件职责

```text
patches/82f59e2.patch
└── 固定保存 PR 提交补丁，避免 PR 更新或删除影响构建

PKGBUILD
├── 固定源码：8ed0da4
├── prepare()：应用 82f59e2.patch
├── build()：cargo build --release --locked
└── package()：将二进制安装为 /usr/bin/niri-center

.github/workflows/build.yml
├── checkout 构建仓库
├── 启动 Arch Linux 构建环境
├── 安装 base-devel 和 PKGBUILD 依赖
├── 使用非 root 构建用户运行 makepkg
├── 检查生成的软件包
└── 上传 niri-center-*.pkg.tar.zst
```

## GitHub Actions 流程

```text
本地项目
└── 推送到 iMaxTomas/niri-center-build
    └── GitHub → Actions → Build niri-center
        └── Run workflow
            └── 下载 niri-center-*.pkg.tar.zst
```

当前 `gh` 登录账号为 `iMaxTomas`，已有 `repo` 和 `workflow` scopes，不需要额外 API Key。

## Deck 安装结构

```text
系统原版
└── /usr/bin/niri

PR 测试版本
└── /usr/bin/niri-center

回滚
└── 重新启动 /usr/bin/niri
```

自定义包不得覆盖 `/usr/bin/niri`。

## 行为验证

测试配置：

```kdl
layout {
    center-single-window
}
```

```text
测试会话
├── 较矮的单窗口列垂直居中
├── 同列加入第二个窗口后恢复普通排列
├── Tabbed 列不错误居中
├── 全高窗口位置不变
├── 横向滚动、列宽调整和跨列移动正常
└── DP-1 的旋转、缩放和逻辑分辨率不变
```

`center-single-window` 只负责居中，不会自动缩短窗口。第一轮测试应手动调整窗口高度，确认功能正常后再决定是否配置 `default-window-height`。

## 实施顺序

```text
1. 保存 82f59e2.patch
└── 2. 编写 PKGBUILD
    └── 3. 编写 build.yml
        └── 4. 本地检查文件内容
            └── 5. 创建 GitHub 仓库并推送
                └── 6. 手动触发 Action
                    └── 7. 下载并检查软件包
                        └── 8. 在 Deck 隔离测试
```
