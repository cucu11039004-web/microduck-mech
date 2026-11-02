# microduck-mech

小黄鸭（microduck）硬件复现的记录与汇总仓库：一边记录作者本人复刻 microduck 硬件（机械结构、主控板、配件）的真实选型与装机过程，一边汇总各个版本的硬件复现开源仓库，供其他复刻者参考。强化学习训练与 sim2real 部署代码不在本仓库，见另一仓库 `microduck-rl`。

## 背景与目标

Pollen Robotics（Hugging Face 旗下机器人公司）2026 年推出官方开源机器人 [microduck](https://github.com/pollen-robotics/microduck)（25cm/800g 双足，$399 零售），国内已衍生出多个中文复刻仓库，舵机方案上大致分官方 Dynamixel 和国内飞特两条路线。本仓库记录作者实际走的那一条（方案 B：飞特 HD-1910），同时把调研过程中比较过的其他路线、仓库和选型整理成可复用的文档。

## 方案对比速览

| | [方案 A：官方 Dynamixel](docs/scheme-a-dynamixel/README.md) | [方案 B：飞特 HD-1910](docs/scheme-b-feetech/README.md)（✅ 本仓库采用） |
|---|---|---|
| 舵机货源 | 国内基本无现货，需国际直购/分销 | 淘宝现货 |
| 预估总成本 | ¥3150~7420（视是否完整复刻功能） | ¥2165~2660（核心走路部分） |
| 实践状态 | 未亲身实践，基于调研整理 | 本仓库实际采购/装机中 |

更细的主控板/舵机选型对比见 [docs/shared/mainboard-and-servo-options.md](docs/shared/mainboard-and-servo-options.md)。

## 相关仓库速览

| 仓库 | 舵机方案 | 一句话定位 |
|---|---|---|
| [pollen-robotics/microduck](https://github.com/pollen-robotics/microduck) | Dynamixel XL330 ×15 | 官方项目，商品化开源机器人 |
| [pollen-robotics/microduck_rl](https://github.com/pollen-robotics/microduck_rl) | — | 官方训练仓库（mjlab + PPO） |
| [AI-FanGe/Microduck-build-tutorial](https://github.com/AI-FanGe/Microduck-build-tutorial) | Dynamixel XL330 ×14 | 简化版复刻，专注走路 sim2real 闭环 |
| [7757/fanduck](https://github.com/7757/fanduck) | Dynamixel XL330 ×15+1 | 1:1 完整复刻，含摄像头/ToF/NFC |
| [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica)（+ [cad](https://github.com/fanhao375/microduck-replica-cad)） | 飞特 HD-1910 ×15 | **本仓库方案 B 的依据**，已实机验证 |
| [SaberOnGo/open-microduck](https://github.com/SaberOnGo/open-microduck) | 待确认 | 文档站，含溯源页 |
| [apirrone/Open_Duck_Mini](https://github.com/apirrone/Open_Duck_Mini) | 飞特 STS3215 ×14 | 前身项目（第一代，42cm，比例不同） |

完整列表（含活跃度、选型资源摘要、采购型号价格快照）见 [awesome/README.md](awesome/README.md)。

## 文档入口

| 文档 | 用途 |
|---|---|
| [LICENSE](LICENSE) | CC BY-NC-SA 4.0 |
| [AGENTS.md](AGENTS.md) | 工作环境、资源、协作与 Git 提交约定 |
| [CODEBASE_MAP.md](CODEBASE_MAP.md) | 目录结构与各部分职责，定位改动区域 |
| [docs/scheme-a-dynamixel/](docs/scheme-a-dynamixel/README.md) | 官方 Dynamixel 舵机路线调研（未亲身实践） |
| [docs/scheme-b-feetech/](docs/scheme-b-feetech/README.md) | 飞特 HD-1910 舵机路线——本仓库实际采用的方案，含真实 BOM 和装机进度 |
| [docs/shared/mainboard-and-servo-options.md](docs/shared/mainboard-and-servo-options.md) | 主控板 / 舵机通用选型对比（含未采用的备选项） |
| [awesome/README.md](awesome/README.md) | awesome-microduck 完整版：硬件复现仓库 + 选型资源 + 采购快照 |
