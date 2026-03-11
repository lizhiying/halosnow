<p align="center">
  <img src="HaloSnow/Assets.xcassets/AppIcon.appiconset/icon_256x256.png" width="128" height="128" alt="HaloSnow Icon">
</p>

<h1 align="center">HaloSnow</h1>

<p align="center">
  <b>电影级动态雪景壁纸 for macOS</b><br>
  <i>Cinematic Live Snow Wallpaper</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-macOS%2013%2B-blue" alt="Platform">
  <img src="https://img.shields.io/badge/framework-Metal-orange" alt="Metal">
  <img src="https://img.shields.io/badge/language-Swift%205.9-F05138" alt="Swift">
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
</p>

---

HaloSnow 让你的 Mac 桌面飘起电影级的雪花。基于 Metal GPU 渲染引擎,数万片雪花实时飘落,每一片都有独特的 3D 翻转姿态和真实光影效果。

## Features

- **GPU 粒子系统** — 最高 100,000 雪花粒子,全 GPU 物理模拟
- **电影级后处理** — 景深散景、体积光锥、丁达尔光线、大气雾气
- **方向性风场** — Perlin Noise 湍流 + 可调节方向风力 + 阵风系统
- **多显示器支持** — 每个屏幕独立渲染
- **智能节能** — 全屏遮挡暂停、屏幕休眠停止、热状态自适应帧率
- **菜单栏控制** — SwiftUI 偏好面板 (密度 / 风力 / 帧率 / 开机自启)
- **屏幕保护程序** — 可作为 macOS 壁纸模式屏保运行
- **App Sandbox** — 完全沙盒化,符合 Mac App Store 要求

## Screenshots

> 运行 HaloSnow 后,你的桌面会变成这样:

|          壁纸效果          |      菜单栏设置      |
| :------------------------: | :------------------: |
| 数万片雪花在暖色路灯下飘落 | 从菜单栏一键调整参数 |

## Requirements

- macOS 13.0 (Ventura) or later
- Apple Silicon (M1/M2/M3/M4) or Intel Mac with Metal GPU

## Installation

### Mac App Store

> 即将上架,敬请期待。

### 手动构建

```bash
# 克隆仓库
git clone https://github.com/你的用户名/halosnow.git
cd halosnow/source/HaloSnow

# 生成 Xcode 项目 (需要 xcodegen)
brew install xcodegen
xcodegen generate

# 构建
xcodebuild -project HaloSnow.xcodeproj -scheme HaloSnow -configuration Release build

# 运行
open ~/Library/Developer/Xcode/DerivedData/HaloSnow-*/Build/Products/Release/HaloSnow.app
```

## Architecture

```
HaloSnow/
├── HaloSnow/                     # 主程序 (菜单栏 App)
│   ├── main.swift                # 入口
│   ├── AppDelegate.swift         # 多屏管理、遮挡检测、系统事件
│   ├── Renderer.swift            # Metal 渲染器 (Compute + Render Pipeline)
│   ├── Shaders.metal             # GPU 着色器 (粒子物理、光照、后处理)
│   ├── PreferencesView.swift     # SwiftUI 菜单栏偏好面板
│   ├── Assets.xcassets/          # App Icon
│   └── Resources/                # 纹理 (背景图、雪花图集)
├── HaloSnowSaver/                # 屏幕保护程序 (.saver)
│   └── HaloSnowSaverView.swift
├── project.yml                   # XcodeGen 配置
└── docs/                         # 发布文档
```

### Rendering Pipeline

```
┌─────────────┐    ┌──────────────┐    ┌───────────────┐
│   Compute   │───>│  Render Pass │───>│ Post-Process  │
│  (Physics)  │    │  (Particles) │    │  (Effects)    │
├─────────────┤    ├──────────────┤    ├───────────────┤
│ Perlin Wind │    │ Texture Atlas│    │ DoF Bokeh     │
│ Gust System │    │ 3D Tumbling  │    │ Volumetric    │
│ Gravity     │    │ Rim Lighting │    │ Tyndall Rays  │
│ Boundary    │    │ Lambert      │    │ Atmospheric   │
│ Light Calc  │    │ Glint        │    │ Color Grading │
└─────────────┘    └──────────────┘    └───────────────┘
```

## Performance

在 Apple Silicon 上的典型资源占用:

| 模式        | CPU   | GPU    | 帧率   | 粒子数 |
| ----------- | ----- | ------ | ------ | ------ |
| 主程序      | ~3.5% | 低     | 30 FPS | 50,000 |
| 屏保 (壁纸) | ~3-5% | ~5-10% | 20 FPS | 20,000 |
| 屏保 (预览) | <1%   | <1%    | 10 FPS | 3,000  |

## Privacy

HaloSnow 不收集任何用户数据。详见 [隐私政策](PRIVACY.md)。

## License

MIT License - see [LICENSE](LICENSE) for details.
