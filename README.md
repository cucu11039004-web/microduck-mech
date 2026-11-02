# microduck-mech

## 背景与目标

Pollen Robotics（Hugging Face 旗下机器人公司）2026 年推出官方开源机器人 [microduck](https://github.com/pollen-robotics/microduck)（25cm/800g 双足，$399 零售），国内已衍生出多个中文复刻仓库，舵机方案上大致分官方 Dynamixel 和国内飞特两条路线。

本仓库是作者本人的 microduck 硬件复现仓库：机械结构、主控板、配件的配置细节全部以作者实际购买、正在组装的 [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica) 飞特 HD-1910 版为准，同时附带一份主流 microduck 硬件复现仓库列表，供其他复刻者参考。只收录 microduck 相关内容；强化学习训练与 sim2real 部署代码不在本仓库，见作者另一仓库 `microduck-rl`。

## 方案对比

舵机型号基本决定了复刻路线（外壳尺寸、总线协议、货源、成本都随之确定），因此按舵机比较：

| | Dynamixel XL330-M288-T（官方原装） | 飞特 HD-1910（✅ 本仓库采用） |
|---|---|---|
| 代表仓库 | [pollen-robotics/microduck](https://github.com/pollen-robotics/microduck)、[AI-FanGe/Microduck-build-tutorial](https://github.com/AI-FanGe/Microduck-build-tutorial)、[7757/fanduck](https://github.com/7757/fanduck) | [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica) |
| 国内货源 | 淘宝基本无现货，需国际直购或国内分销 | 淘宝现货 |
| 舵机单价（调研时点） | ROBOTIS 国际官网 $23.9-27.5；国内分销约 ¥299 | 约 ¥95-120 |
| 电压匹配 | 超压运行（额定 3.7-6V，实际 6.6-8.2V，官方刻意为之） | 额定电压与工作电压匹配，力矩约为 XL330 的 2.5 倍 |
| 执行器建模 | 需第三方反推 BAM 参数 | 飞特官方维护 [HD-1910 BAM 模型](https://gitee.com/ftservo/ftservo-bam) |
| 预估总成本 | ¥3150~7420（据 AI-FanGe / 7757/fanduck 的 BOM 调研估算，视是否完整复刻功能） | 待确认（散件套餐价格待补充） |
| 实践状态 | 未亲身实践，仅调研 | 本仓库实际复现中 |

## 配置清单

作者实际购买的配置（淘宝散件套餐 + 单独购买的主控板）。每项的搜索关键词、购买价、选型说明见 [复现记录](docs/build/README.md#配置清单详解)。

| 类别 | 物料（型号）× 数量 |
|---|---|
| 主控 | Radxa Zero 3W（RK3566），2G 内存 / 无 eMMC × 1 |
| 舵机 | 飞特 HD-1910（HD-1910-C001）× 15——注意不是飞特常见的 STS3215 |
| 电路板 | HAT 扩展板 × 1；Hub 分线板 × 1；微雪舵机控制板 Waveshare Bus Servo Adapter (A) × 1 |
| 传感器 | IMU 姿态模块 × 1；IMX219 摄像头模块（160° 广角 + 排线）× 1；VL53L8CX ToF 模块 × 1 |
| 电源 | NP-F550 电池 × 1；电源适配器 × 1 |
| 交互 | 扬声器 × 1 |
| 存储 | Micro SD 卡（64G，V3）× 1；读卡器 × 1 |
| 机械件 | 3D 打印结构件（带轮滑，PLA + TPU）× 1 套；轴承 × 22（11 大 + 3 小 + 8 滑轮）；螺丝包 × 1 套 |
| 线材 | AMP2.0 转接线 + EH 线 × 1 套 |

套餐对应 fanhao375 的 HD-1910 变体；HAT 等电路板的元件级 BOM 见上游 [BOM.md](https://github.com/fanhao375/microduck-replica/blob/master/BOM.md)，打印件清单见 [CAD 仓库](https://github.com/fanhao375/microduck-replica-cad)。

## 相关仓库速览

| 仓库 | 舵机方案 | 定位 / 特点 | 活跃度（调研时点） | 备注 |
|---|---|---|---|---|
| [pollen-robotics/microduck](https://github.com/pollen-robotics/microduck) | Dynamixel XL330-M288-T ×15 | **官方项目**，Hugging Face 旗下 Pollen Robotics 出品，商品化开源机器人，25cm/800g，零售 $399 | 官方维护 | 从未公布完整 BOM；主控 Radxa Zero 3W，Rust 固件 `robotd`，50Hz 推理控制 |
| [pollen-robotics/microduck_rl](https://github.com/pollen-robotics/microduck_rl) | — | 官方训练仓库（mjlab + PPO），与硬件强相关的部分是其执行器建模方法论 | 官方维护 | 训练/部署相关内容见作者另一仓库 `microduck-rl` |
| [AI-FanGe/Microduck-build-tutorial](https://github.com/AI-FanGe/Microduck-build-tutorial) | Dynamixel XL330-M288-T ×14 | 简化版复刻，砍掉摄像头/ToF，专注走路 sim2real 闭环，提供预烧录 SD 卡镜像 | 1600+ star，更新活跃（2026-09 创建） | 主控 Raspberry Pi Zero 2W + ROBOTIS OpenRB-150 舵机总线板 |
| [7757/fanduck](https://github.com/7757/fanduck) | Dynamixel XL330-M288-T ×15+1 | 1:1 完整复刻官方版，含摄像头/ToF/NFC/喇叭，自制 PCB | 80+ star（2026-09 创建） | `imu_to_dxl` 板官方未开源，需自行设计，是最大工程风险点 |
| [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica)（+ [CAD 仓库](https://github.com/fanhao375/microduck-replica-cad)） | 飞特 HD-1910 ×15 | 已实机验证站立/行走/前滚翻起身，文档标注"官方事实 vs 社区推测"区分度高，社区（含微信群、B站教程）活跃 | 1500+ star，几乎每日更新 | **本仓库的复现依据**；CAD 仓库提供 HD-1910 / XL330 两种变体 |
| [SaberOnGo/open-microduck](https://github.com/SaberOnGo/open-microduck) | 待确认 | 文档站结构完整，含 legal/provenance 溯源页 | 待确认 | 调研时点未深入核查，仅作为交叉参考收录 |

与 [awesome/README.md](awesome/README.md) 的列表内容一致，两处同步更新。

## 文档入口

| 文档 | 用途 |
|---|---|
| [docs/build/README.md](docs/build/README.md) | 复现记录：配置清单详解、采购信息、装机进度、开放问题 |
| [awesome/README.md](awesome/README.md) | awesome-microduck：microduck 硬件复现仓库列表（与上方速览一致） |
| [CODEBASE_MAP.md](CODEBASE_MAP.md) | 目录结构与各部分职责，定位改动区域 |
| [AGENTS.md](AGENTS.md) | 工作环境、资源、协作与 Git 提交约定 |
| [LICENSE](LICENSE) | CC BY-NC-SA 4.0 |
