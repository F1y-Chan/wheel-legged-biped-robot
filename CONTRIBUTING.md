# 参与贡献 / Contributing

感谢你对本项目的关注！无论是提交 Bug、改进文档还是贡献代码，都非常欢迎。

## 提交 Issue

提 Issue 前请先搜索是否已有相同问题。一个高质量的 Issue 应包含：

- **问题现象**：实际发生了什么，期望发生什么；
- **复现步骤**：尽可能精确，能稳定复现；
- **运行环境**：Keil MDK 版本、ARM Compiler 版本、MATLAB/Simulink 版本、硬件型号与接线；
- **日志与截图**：编译报错全文、串口输出、波形或示波器截图。

## 提交 Pull Request

1. Fork 本仓库并从 `main` 拉出特性分支，例如 `feat/vmc-tuning`、`fix/can-timeout`；
2. 保持改动聚焦：一个 PR 只解决一个问题；
3. 提交前自检：
   - 工程可以正常编译（Keil `Rebuild` 无 Error）；
   - **不要提交编译产物**，仓库已通过 `.gitignore` 排除 `.o`、`.crf`、`.axf`、`.map`、`slprj/` 等文件；
   - 不要提交本机绝对路径相关的文件（`*.uvguix.*`、`*.uvoptx` 等）；
4. 提交信息建议使用 [Conventional Commits](https://www.conventionalcommits.org/)：`feat:`、`fix:`、`docs:`、`refactor:`、`chore:`；
5. 在 PR 描述中说明**改动内容、验证方式、对硬件行为的影响**。

## 代码风格

- **C**：变量使用小写下划线（`chassis_speed`），函数使用 `模块_动作`（`chassis_task_init`）；缩进 4 空格；物理量注释单位（`rad/s`、`N·m`、`A`）。
- **MATLAB**：脚本首行注明输入、输出与单位；参数集中在文件开头，便于调参。
- 新增文件请加入对应的 Keil 工程分组，不要只放在目录里。

## 安全要求

本项目涉及**力矩控制**的电机。涉及控制逻辑、限幅、保护策略的 PR，请在描述中注明测试条件（是否架空、限流值、急停措施），未在实物上验证的改动请显式说明。

## 许可证

你提交的贡献将以本仓库的 [MIT License](LICENSE) 发布。
