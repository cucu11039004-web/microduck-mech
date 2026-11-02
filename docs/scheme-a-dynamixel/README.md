# 方案 A：官方舵机路线（Dynamixel XL330-M288-T）

> **状态：未亲身实践，基于公开资料调研整理。** 本文档内容来自对公开仓库和官方资料的调研交叉核对，未经本仓库作者实机验证。价格、库存信息会随时间变化，下单前请自行复核。本仓库作者实际采用的是 [方案 B（飞特 HD-1910）](../scheme-b-feetech/README.md)，原因见该文档说明。

## 这是什么

Pollen Robotics（Hugging Face 旗下机器人公司）2026 年推出的官方开源机器人 [microduck](https://github.com/pollen-robotics/microduck) 原生使用的舵机方案：**15 个 Dynamixel XL330-M288-T**（ROBOTIS/韩国）。方案 A 指所有沿用这一原装舵机型号的复刻路线，调研中覆盖了两个代表性的中文复刻仓库：

| 仓库 | 定位 | 自由度 | 活跃度 |
|---|---|---|---|
| [AI-FanGe/Microduck-build-tutorial](https://github.com/AI-FanGe/Microduck-build-tutorial) | 简化版复刻，砍掉摄像头/ToF，专注走路 sim2real 闭环 | 14（双腿10+头颈4） | 1600+ star，更新活跃 |
| [7757/fanduck](https://github.com/7757/fanduck) | 1:1 完整复刻官方版，含摄像头/ToF/NFC/喇叭 | 15+1备用 | 80+ star |

## 为什么没有采用这条路线

- **国内现货差**：Dynamixel XL330-M288-T 在淘宝基本没有现货，需走国际直购（ROBOTIS 官网 $23.9-27.5/个）或国内分销商（实测报价约 ¥299/个），单舵机成本显著高于飞特方案。
- **舵机是超压运行**：该型号额定电压 3.7-6V，官方实际喂 6.6-8.2V，是 Pollen 官方刻意为之的设计选择，但也意味着复刻时要留意电压匹配和舵机寿命。
- 对比详见 [主控板/舵机通用选型对比](../shared/mainboard-and-servo-options.md)。

## BOM（综合两个仓库的调研，未核实国内现时价格）

### AI-FanGe 版（14 舵机，无摄像头/ToF）

| 类别 | 物料 | 数量 | 说明 |
|---|---|---|---|
| 主控 | Raspberry Pi Zero 2W | 1 | 运行控制程序、蓝牙手柄、Wi-Fi、SSH |
| 舵机总线板 | ROBOTIS OpenRB-150 | 1 | USB 连接树莓派，负责和 XL330 总线通信 |
| 舵机 | Dynamixel XL330-M288-T | 14 | 双腿10 + 头颈4；预留 ID15 嘴部舵机，当前行走策略不使用 |
| 电池 | 成品 6V 可充电电池 | 1 | |
| IMU | BNO080/085/086 模块 | 1 | I2C 直连树莓派，集成方式简单 |
| 存储 | 16GB+ microSD | 1 | |
| 结构件 | 3D 打印 PLA 套件 | 1套 | |
| 可选工具 | U2D2 + Power Hub | 1套 | 配置舵机ID，可多机复用 |

sim2real 闭环：`mjlab_microduck/`（MuJoCo/MjLab 训练导出 `walk.onnx`）→ `microduck/`（树莓派端加载 ONNX 推理），有现成 `make run`/`make sync` 工作流，并提供预烧录好的 SD 卡镜像。致谢原文提到参考了 [Rhoban/microban](https://github.com/Rhoban/microban) 和官方 [pollen-robotics/microduck](https://github.com/pollen-robotics/microduck)。

### 7757/fanduck 版（15+1 舵机，完整复刻）

| 类别 | 物料 | 数量 | 说明 |
|---|---|---|---|
| 主控 | Radxa Zero 3W（RK3566） | 1 | 与官方一致 |
| 舵机 | Dynamixel XL330-M288-T | 15+1备用 | 含嘴部舵机 |
| 摄像头 | IMX219 | 1 | |
| 深度传感 | VL53L8CX | 1 | |
| 电源 | NP-F550 电池 ×2 + 双槽充电器 | 1套 | |
| PCB | Robot HAT 克隆板（4层，123元件，含 VQFN/LGA，不可手焊） | 1 | 官方 Gerber 已开源，需打样+贴片 |
| PCB | imu_to_dxl 板 | 1 | ⚠️ **官方未开源此板**，需自行设计验证，是本路线最大的工程风险点 |
| 轴承/紧固件/3D打印件 | — | — | 含滚轮(轮滑)变体 |
| 交互 | 喇叭 + NFC标签×10 + Xbox手柄 | — | |

采购清单位于仓库内腾讯文档（JS 动态渲染，调研时未能抓取到具体单价，下单前请直接打开核实）。`imu_to_dxl` 板是否已被该仓库简化替代（例如直接 I2C 接主控而非伪装成舵机总线从机）需要使用者自行确认仓库最新代码。

## 成本量级参考（调研时点，仅供数量级参考）

| | AI-FanGe | 7757/fanduck |
|---|---|---|
| 预估总成本 | ¥3150~5960（≈$440-830） | ¥3870~7420（≈$540-1030），不含 imu_to_dxl 自研设计的时间成本 |

高于官方整机零售价 $399（约¥2900）——第三方逆向 BOM（见 [fanhao375/microduck-replica BOM](https://github.com/fanhao375/microduck-replica/blob/master/BOM.en.md)）指出，仅 15 颗舵机按欧洲零售价采购就要 €603-629，个人散件复刻在经济上天然难以打平官方供应链价格。

## 适用场景

如果你已经有稳定的 Dynamixel 舵机货源（比如所在地有 ROBOTIS 代理商/实验室库存），或者需要 1:1 复刻官方摄像头/ToF 交互功能，方案 A 仍然值得考虑。纯粹做走路类 sim2real 研究、且在国内淘宝采购，方案 B 性价比更高（见 [方案 B 文档](../scheme-b-feetech/README.md)）。
