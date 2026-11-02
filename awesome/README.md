# Awesome Microduck

> 本页是完整版。顶层 [README.md](../README.md) 有精简摘要（相关仓库速览 + 方案对比），想先看结论可以从那里开始。

小黄鸭（microduck）硬件复现相关的开源仓库 + 主控板/舵机选型参考合集，帮助新入坑的复现者少走弯路。本列表是本仓库（[microduck-mech](../README.md)）调研过程的副产品，欢迎 PR 补充/纠错。

与强化学习训练、sim2real 部署相关的资源不在本列表范围内，见作者另一仓库 `microduck-rl`（机械/主控/配件复现与训练部署分仓维护）。

## 1. microduck 直接相关的硬件复现仓库

| 仓库 | 舵机方案 | 定位 / 特点 | 活跃度（调研时点） | 备注 |
|---|---|---|---|---|
| [pollen-robotics/microduck](https://github.com/pollen-robotics/microduck) | Dynamixel XL330-M288-T ×15 | **官方项目**，Hugging Face 旗下 Pollen Robotics 出品，商品化开源机器人，25cm/800g，零售 $399 | 官方维护 | 从未公布完整 BOM；主控 Radxa Zero 3W，Rust 固件 `robotd`，50Hz 推理控制 |
| [pollen-robotics/microduck_rl](https://github.com/pollen-robotics/microduck_rl) | — | 官方训练仓库（mjlab + PPO），与硬件强相关的部分是其执行器建模方法论 | 官方维护 | 训练/部署相关细节见本作者 `microduck-rl` 仓库 |
| [AI-FanGe/Microduck-build-tutorial](https://github.com/AI-FanGe/Microduck-build-tutorial) | Dynamixel XL330-M288-T ×14 | 简化版复刻，砍掉摄像头/ToF，专注走路 sim2real 闭环，提供预烧录 SD 卡镜像 | 1600+ star，更新活跃（2026-09 创建） | 本仓库 [方案 A 文档](../docs/scheme-a-dynamixel/README.md) 的主要依据之一 |
| [7757/fanduck](https://github.com/7757/fanduck) | Dynamixel XL330-M288-T ×15+1 | 1:1 完整复刻官方版，含摄像头/ToF/NFC/喇叭，自制 PCB | 80+ star（2026-09 创建） | `imu_to_dxl` 板官方未开源，需自行设计，是最大工程风险点 |
| [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica)（+ [CAD 仓库](https://github.com/fanhao375/microduck-replica-cad)） | 飞特 HD-1910 ×15 | **本仓库采用的方案**，已实机验证站立/行走/前滚翻起身，文档标注"官方事实 vs 社区推测"区分度高，社区（含微信群、B站教程）活跃 | 1500+ star，几乎每日更新 | 详见本仓库 [方案 B 文档](../docs/scheme-b-feetech/README.md) |
| [SaberOnGo/open-microduck](https://github.com/SaberOnGo/open-microduck) | 待确认 | 文档站结构完整，含 legal/provenance 溯源页 | 待确认 | 调研时点未深入核查，仅作为交叉参考收录 |
| [apirrone/Open_Duck_Mini](https://github.com/apirrone/Open_Duck_Mini) | 飞特 STS3215 ×14 | **前身项目**（第一代，复刻迪士尼 BDX droid，42cm，比例与 25cm 版 microduck 不同），有预训练好的 `BEST_WALK_ONNX.onnx` | 4600+ star，主仓约9个月未更新（周边仓库需单独确认） | 执行器 BAM 参数、训练框架、部署运行时配套齐全，适合作为方法论参考 |

## 2. 主控板 / 舵机通用选型资源

详细对比表、取舍理由见 [主控板/舵机通用选型对比](../docs/shared/mainboard-and-servo-options.md)。这里是摘要：

**主控板**：Radxa Zero 3W（本仓库方案 B 采用，软件生态与 fanhao375 固件直接兼容）> Raspberry Pi Zero 2W（方案 A 使用，国内常溢价）> 香橙派 Zero 2/3、Luckfox Pico 系列（更便宜但需要自行移植固件，未见现成适配案例）> ESP32 MCU 直接推理（最便宜，但需要手工把策略网络转成 C 前向推理代码，工程量大）。

**舵机**：Dynamixel XL330-M288-T（官方原装，国内无现货）vs 飞特 HD-1910（本仓库方案 B 采用，国内现货+电压匹配更合理，适配 25cm 版外壳）vs 飞特 STS3215（第一代 42cm 版 Open_Duck_Mini 使用，国内生态最成熟但装不进 25cm 外壳）。

## 3. 采购参考（具体型号/价格快照，不提供购买链接，不含拼团，面向个人零买）

以下信息为调研/下单时点的快照，仅供搜索对应型号时参考价格量级，**不保证时效性，不提供跳转链接**，请自行在淘宝等平台搜索准确的商品名称核实当前价格和店铺信誉。

| 物料 | 型号关键词 | 参考价格 | 快照时间 | 备注 |
|---|---|---|---|---|
| 舵机（方案B采用） | 飞特 HD-1910 / HD1901M / HD-1910-C001 开源小鸭舵机 | 全套15个的套餐价约 ¥95-120/个 | 2026-11 | 命名不统一，下单前核对规格书 |
| 主控板（方案B采用） | 瑞莎 Radxa ZERO 3W RK3566 Microduck 适配款，2+0（2G/无eMMC） | ¥710 | 2026-11，已售75件，店铺评分4.6，广东深圳发货 | 卖家专门标注"Microduck"，推测经过兼容性验证 |
| 主控板（理论对比价） | Radxa Zero 3W 官方渠道裸板，1G/无eMMC | 约¥130-150（含32G卡） | 调研时点，未实测下单 | 与上面的"Microduck适配款"差价较大，原因未查明 |
| 舵机（方案A参考） | Dynamixel XL330-M288-T | 国内分销约¥299/个；ROBOTIS国际官网 $23.9-27.5/个 | 调研时点 | 国内基本无现货 |
| 舵机（第一代 Open_Duck_Mini 参考） | 飞特 STS3215 | 约¥89-150/个（参考LeRobot SO-100/101生态价位） | 调研时点 | 体型较大，不适配25cm外壳 |

---

欢迎提交 PR 补充你知道的其他 microduck 复现仓库或选型信息；采购类信息请遵循本列表的格式（型号+价格快照，不放链接、不推拼团），避免信息过时后误导他人。
