# Awesome Microduck

小黄鸭（microduck）硬件复现相关的开源仓库列表，帮助新入坑的复现者少走弯路。本列表与顶层 [README.md](../README.md) 的“相关仓库速览”内容一致，两处同步更新；只收录 microduck 本身及其复刻仓库。

| 仓库 | 舵机方案 | 定位 / 特点 | 活跃度（调研时点） | 备注 |
|---|---|---|---|---|
| [pollen-robotics/microduck](https://github.com/pollen-robotics/microduck) | Dynamixel XL330-M288-T ×15 | **官方项目**，Hugging Face 旗下 Pollen Robotics 出品，商品化开源机器人，25cm/800g，零售 $399 | 官方维护 | 从未公布完整 BOM；主控 Radxa Zero 3W，Rust 固件 `robotd`，50Hz 推理控制 |
| [pollen-robotics/microduck_rl](https://github.com/pollen-robotics/microduck_rl) | — | 官方训练仓库（mjlab + PPO），与硬件强相关的部分是其执行器建模方法论 | 官方维护 | 训练/部署相关内容见作者另一仓库 `microduck-rl` |
| [AI-FanGe/Microduck-build-tutorial](https://github.com/AI-FanGe/Microduck-build-tutorial) | Dynamixel XL330-M288-T ×14 | 简化版复刻，砍掉摄像头/ToF，专注走路 sim2real 闭环，提供预烧录 SD 卡镜像 | 1600+ star，更新活跃（2026-09 创建） | 主控 Raspberry Pi Zero 2W + ROBOTIS OpenRB-150 舵机总线板 |
| [7757/fanduck](https://github.com/7757/fanduck) | Dynamixel XL330-M288-T ×15+1 | 1:1 完整复刻官方版，含摄像头/ToF/NFC/喇叭，自制 PCB | 80+ star（2026-09 创建） | `imu_to_dxl` 板官方未开源，需自行设计，是最大工程风险点 |
| [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica)（+ [CAD 仓库](https://github.com/fanhao375/microduck-replica-cad)） | 飞特 HD-1910 ×15 | 已实机验证站立/行走/前滚翻起身，文档标注"官方事实 vs 社区推测"区分度高，社区（含微信群、B站教程）活跃 | 1500+ star，几乎每日更新 | **本仓库的复现依据**；CAD 仓库提供 HD-1910 / XL330 两种变体 |
| [SaberOnGo/open-microduck](https://github.com/SaberOnGo/open-microduck) | 待确认 | 文档站结构完整，含 legal/provenance 溯源页 | 待确认 | 调研时点未深入核查，仅作为交叉参考收录 |

与强化学习训练、sim2real 部署相关的资源不在本列表范围内，见作者另一仓库 `microduck-rl`。

---

欢迎提交 PR 补充你知道的其他 microduck 复现仓库，或纠正上表中的信息。
