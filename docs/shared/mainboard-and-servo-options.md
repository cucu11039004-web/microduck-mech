# 主控板 / 舵机通用选型对比

本文档汇总复刻过程中调研/对比过的主控板和舵机选项，供后来者参考取舍依据。价格为调研时点（2026-11）的参考数量级，不是实时报价，下单前请自行核实。

## 主控板

| 型号 | 芯片 | 参考价（淘宝） | 优点 | 缺点 / 风险 | 本仓库使用情况 |
|---|---|---|---|---|---|
| **Radxa Zero 3W**（2G/无eMMC，Microduck适配款） | RK3566 四核 | ¥710（卖家预配置/验证款） | 与官方 microduck、[fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica) 的固件（Rust 运行时 `robotd`、摄像头/ToF 驱动）直接兼容，软件移植风险最低 | 该特定款式价格偏高，原因未完全查明 | ✅ 方案 B 采用 |
| Radxa Zero 3W（官方渠道裸板） | RK3566 四核 | 理论约 ¥130-150（1G 无eMMC裸板 + 32G 卡） | 同芯片，价格更低 | 调研阶段未验证是否与 Microduck 适配款完全等价（PCB 版本、外设排布等细节未核实） | 未采用，仅作价格参照 |
| Raspberry Pi Zero 2W | BCM2710A1 四核 | 国内常溢价至 ¥700-800（进口/缺货） | [AI-FanGe](https://github.com/AI-FanGe/Microduck-build-tutorial) 方案直接支持，生态成熟 | 国内渠道溢价严重，货源不稳定 | 方案 A（AI-FanGe）使用，本仓库未采用 |
| 香橙派 Zero 2 / Zero 3 | Allwinner H618 等 | ¥100-250 | 国产，无进口溢价，GPIO/USB/UART/I2C 齐全，跑 Linux+onnxruntime 做小型 MLP 策略推理完全够用 | 需要自行移植 fanhao375 的 Radxa 专用固件和摄像头驱动，工作量不小；未见现成适配案例 | 未采用，调研阶段的备选 |
| Luckfox Pico 系列（RV1103/RV1106） | 瑞芯微 RV1103/1106 | ¥30-150（基础款¥30-70，Ultra/Max款¥100-150） | 已知调研到的最低价 Linux SBC 之一，自带 NPU，国内现货充足 | 系统定制化（buildroot）门槛比 Debian 系的 Radxa/树莓派/香橙派更高；未见跑通 PPO 策略推理的现成案例 | 未采用，调研阶段的备选 |
| ESP32（MCU 直接推理，不跑 Linux） | — | ¥15-30 | 价格比任何 Linux SBC 低一个量级；已有社区案例证明 MCU 直接跑小型策略网络 + 串口驱动总线舵机可行 | 不能直接用 onnxruntime，需要把策略网络手工转成 C 前向推理代码（类似 TinyML 做法），工程量明显更大；是否满足 50Hz 控制频率、精度要求需要自行验证 | 未采用，作为未来"极致降成本"方向记录 |

**结论**：主控板不建议脱离软件生态单独比价——Radxa Zero 3W 之所以被选中，核心原因是 fanhao375/microduck-replica 的整套固件栈是围绕它开发的，换板子等于要自己重新移植驱动层，这部分工程成本通常比省下的几百元硬件差价更高。

## 舵机

| 型号 | 厂商 | 参考单价（淘宝） | 国内现货 | 电压匹配 | 使用该型号的仓库 | 本仓库使用情况 |
|---|---|---|---|---|---|---|
| Dynamixel XL330-M288-T | ROBOTIS（韩国） | $23.9-27.5 国际直购 / 约 ¥299 国内分销 | 差，基本无现货 | 超压运行（额定3.7-6V，实际喂6.6-8.2V，官方刻意为之） | 官方 [pollen-robotics/microduck](https://github.com/pollen-robotics/microduck)、[AI-FanGe](https://github.com/AI-FanGe/Microduck-build-tutorial)、[7757/fanduck](https://github.com/7757/fanduck) | 方案 A，本仓库未采用 |
| 飞特 HD-1910（HD1901M / HD-1910-C001） | 飞特科技（深圳） | ¥95-120 | 好，淘宝有现货套装 | 额定电压与工作电压匹配，力矩比 XL330 大约 2.5 倍 | [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica) | ✅ 方案 B 采用 |
| 飞特 STS3215 | 飞特科技（深圳） | 约 ¥89-150（参考 LeRobot 生态价位） | 很好，LeRobot SO-100/SO-101 机械臂同款，国内生态极成熟 | 额定7.4V，与官方 BAM 预标定参数直接对应 | [apirrone/Open_Duck_Mini](https://github.com/apirrone/Open_Duck_Mini)（第一代项目，42cm，比例与 25cm 版 microduck 不同） | 未采用——体型是为容纳更大的 STS3215 设计的，装不进 microduck 原装 25cm 外壳 |

**结论**：HD-1910 和 STS3215 都是飞特的总线舵机，但不能混为一谈——前者是专门适配 microduck 小尺寸外壳的型号，后者体型更大，对应的是第一代 Open_Duck_Mini（42cm）的设计。选型时认准外壳尺寸对应的舵机型号，而不是单纯比较"飞特 vs Dynamixel"。

## 舵机总线驱动方案（技术参考）

飞特 STS 系列（含 HD-1910 所在总线协议家族）使用半双工 TTL UART，**协议与 Dynamixel Protocol 2.0 不同**，不能直接套用 Dynamixel SDK。国内常见驱动方案：

- 飞特官方 `scservo_sdk`
- [LeRobot](https://huggingface.co/docs/lerobot/feetech) 的 `feetech.py` 电机驱动层（SO-100/SO-101 机械臂同款，Python 驱动现成且维护活跃）
- Rust：[rustypot](https://docs.rs/rustypot/latest/src/rustypot/servo/feetech/sts3215.rs.html)
- C/STM32：[Mowibox/stm32-sts3215-lib](https://github.com/Mowibox/stm32-sts3215-lib)

调试工具：飞特 URT2 调试板（配置舵机 ID、波特率等参数，可多机复用），对应 Dynamixel 生态里 U2D2 的角色。
