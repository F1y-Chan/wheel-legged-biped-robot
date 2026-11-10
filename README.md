# 轮腿式双足机器人 · Wheel-Legged Biped Robot

面向零基础同学的轮腿式双足机器人开源资料：**控制理论推导与仿真**（LQR、VMC、动力学建模、杆长参数优化）＋ **基于 RoboMaster 开发板 C 型的嵌入式工程**。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![MCU](https://img.shields.io/badge/MCU-STM32F407IGHx-blue.svg)
![Toolchain](https://img.shields.io/badge/Toolchain-Keil%20MDK%205.x-green.svg)
![MATLAB](https://img.shields.io/badge/MATLAB-R2020b%2B-orange.svg)

## 目录

- [项目简介](#项目简介)
- [仓库结构](#仓库结构)
- [硬件平台](#硬件平台)
- [控制理论与仿真](#控制理论与仿真)
- [嵌入式工程](#嵌入式工程)
- [快速开始](#快速开始)
- [已知限制](#已知限制)
- [致谢](#致谢)
- [参与贡献](#参与贡献)
- [许可证](#许可证)

## 项目简介

本项目是一款轮腿式（两轮）自平衡双足机器人，以「理论 → 仿真 → 实机」的完整链路组织资料：

- 用 LQR 与 VMC 两种方法解决「平衡 + 腿部姿态」的控制问题，并给出完整的建模推导与 Simulink 仿真；
- 用拉格朗日法建立动力学模型，并对腿部连杆长度做参数优化；
- 在 RoboMaster 开发板 C 型上实现嵌入式控制工程，通过 CAN 总线以力矩模式驱动关节电机与轮毂电机。

开源初衷是为零基础的同学提供一套可直接上手的学习资料与仿真调试工具，帮助他们更快地进入轮腿式机器人项目。

## 仓库结构

```text
.
├── 嵌入式开发/
│   ├── C板资料/                # RoboMaster 开发板 C 型硬件手册与官方例程
│   ├── 关节电机资料/            # HT-03 关节电机：手册、机械图纸、例程与底层 FOC 固件
│   ├── 嵌入式代码/
│   │   └── chassis_task/       # Keil MDK 工程（STM32F407IGHx + FreeRTOS）
│   ├── 硬件接线图/              # 平衡步兵 / 底盘 / 遥控系统接线图
│   └── 程序流程图/              # 主程序、位姿解算、底盘控制、关节电机控制流程
├── 理论研究/
│   ├── LQR控制算法/             # 两轮模型建模 + LQR 控制器（.m / .slx）
│   ├── VMC控制算法/             # 虚拟模型控制（.slx）与雅可比矩阵推导
│   ├── 动力学建模/              # 拉格朗日动力学推导与力矩映射
│   └── 杆长参数优化计算/         # 连杆长度优化脚本与设计尺寸总表
├── docs/
│   └── 原始说明.txt             # 项目首次公开时的说明（历史存档）
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## 硬件平台

| 部件 | 说明 |
| --- | --- |
| 主控 | RoboMaster 开发板 C 型（STM32F407IGHx，Cortex-M4F，168 MHz） |
| 关节电机 | 海泰机电 HT-03 关节电机，配套 FOC 驱动板（资料见 `嵌入式开发/关节电机资料`） |
| 轮系 | 轮毂驱动，连接方案参考东莞理工学院 ACE 战队的开源设计 |
| 惯性传感 | BMI088（六轴 IMU）+ IST8310（磁力计），均为 C 板板载 |
| 通信 | CAN 总线（电机）、DBUS（遥控接收机）、串口 |

> 机械结构总装、零件模型（SolidWorks）、DXF/STL/PDF 图纸与测试视频未包含在本仓库中，本文档仅覆盖嵌入式与理论部分。

## 控制理论与仿真

四个子课题各自包含**说明文档（PDF）+ 源码**，建议按「建模 → 控制器 → 仿真」的顺序阅读：

| 子课题 | 内容 | 主要文件 |
| --- | --- | --- |
| LQR 控制算法 | 两轮倒立摆简化模型、状态空间方程、LQR 增益整定与仿真 | `two_wheel_model.m`、`two_wheel_model_control.slx`、`LQR控制算法.pdf` |
| VMC 控制算法 | 虚拟模型控制：以虚拟弹簧-阻尼描述腿部位姿，经雅可比矩阵映射为关节力矩 | `jacobi.m`、`vmc.slx`、`VMC控制算法.pdf` |
| 动力学建模 | 拉格朗日法建立轮腿动力学方程，分析固定力矩下的腿部摆动 | `v_angle_FixedTorque.m`、`动力学建模.pdf` |
| 杆长参数优化计算 | 以控制性能为目标优化连杆长度，输出推荐尺寸 | `optimization.m`、`y_angle.m`、`设计尺寸总表.xls`、`杆长参数优化计算.pdf` |

**环境要求**：MATLAB R2020b 或更高版本，需安装 Simulink 与 Optimization Toolbox（杆长优化部分）。

## 嵌入式工程

`嵌入式开发/嵌入式代码/chassis_task` 是在 RoboMaster 官方例程
[17.chassis_task](https://github.com/RoboMaster/Development-Board-C-Examples/tree/master/17.chassis_task)
基础上修改得到的 Keil MDK 工程。

- **MCU / 工具链**：STM32F407IGHx，Keil MDK 5.x（ARM Compiler 5 或 6）
- **RTOS**：FreeRTOS + CMSIS-RTOS，任务划分见 `application/`
- **驱动**：BMI088、IST8310、CAN、PWM、Flash 等位于 `bsp/boards/` 与 `components/devices/`
- **控制**：关节电机与轮毂电机均为**力矩控制**，PID 与滤波位于 `components/controller/`、`components/algorithm/`

```text
chassis_task/
├── application/    # 底盘、云台、关节电机、INS、遥控、检测等任务
├── bsp/boards/     # 板级支持包
├── components/     # 算法（AHRS/CMSIS-DSP）、控制器、设备驱动
├── Drivers/        # STM32F4 HAL + CMSIS
├── Inc/ Src/       # CubeMX 生成的外设初始化
├── Middlewares/    # FreeRTOS
└── MDK-ARM/        # Keil 工程文件（chassis_task.uvprojx）
```

## 快速开始

### 1. 跑通理论仿真（无需硬件）

1. 用 MATLAB 打开 `理论研究/LQR控制算法/two_wheel_model.m`，运行后可得到线性化模型与 LQR 增益；
2. 打开同目录 `two_wheel_model_control.slx` 观察闭环响应；
3. 参照 `VMC控制算法/vmc.slx` 与 `动力学建模/v_angle_FixedTorque.m` 对比两种控制思路。

### 2. 编译并下载嵌入式工程

1. 安装 Keil MDK 5.x 与 STM32F4xx 器件包；
2. 打开 `嵌入式开发/嵌入式代码/chassis_task/MDK-ARM/chassis_task.uvprojx`；
3. 确认目标器件为 STM32F407IGHx，`Rebuild` 后通过 ST-Link / J-Link 下载；
4. 上电前请核对硬件接线图，并确认 CAN 电机 ID 与实物一致。

> ⚠️ **安全提示**：关节电机与驱动轮电机均为力矩控制，参数不当可能造成剧烈动作。首次调试请务必架空底盘、限制输出力矩，并保证急停手段可用。

## 已知限制

- 受疫情影响，样机仅完成了**轮式运动**的调试；腿部运动的 VMC 算法**未在实物样机上验证**；
- 轮式运动的控制效果并非最优，仍有较大调参空间；
- 本项目完成度有限，与同类优秀开源项目相比存在差距，内容仅供参考。

## 致谢

本项目的机械总体布局与腿部连杆机构参考借鉴了 **ARX-4** 开源模型，轮系结构学习借鉴了 **东莞理工学院 ACE 战队**开源的轮毂连接设计方案，嵌入式代码基于 **RoboMaster 组委会**开源的开发板 C 型官方步兵代码修改而来。诚挚感谢上述开源项目对本项目的启发与帮助。

同时感谢海泰机电提供的 HT-03 关节电机资料。

## 参与贡献

欢迎提交 Issue 与 Pull Request，具体流程与约定见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可证

本项目以 **MIT License** 开源，详见 [LICENSE](LICENSE)。

仓库内的第三方资料（RoboMaster 官方例程与手册、HT-03 厂商资料与固件、参考论文 PDF、比赛规则文档等）版权归各自所有者，遵循其原始许可，不在本项目 MIT 许可覆盖范围内。

## 联系

- 邮箱：997798398@qq.com
- GitHub：[@F1y-Chan](https://github.com/F1y-Chan)
