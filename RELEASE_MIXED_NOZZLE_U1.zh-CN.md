# U1 混合口径校验固件发布草稿

## 标题与目标

- 标题：Snapmaker U1 mixed-nozzle validation Experimental Alpha
- Tag：`mixed-nozzle-u1-alpha.1`
- 目标分支：`Min/mixed-nozzle-u1-alpha`

这是配套 Snapmaker Orca 混合口径实验分支的 U1 Extended Firmware 测试构建。它只把喷嘴校验改成按 `extruder_map_table` 对应逻辑工具和物理喷嘴。

## 资产

- `U1_extended_mixed_nozzle_alpha.1.bin`
- 同名 SHA-256 文件
- 精确源码提交链接
- GPL-3.0 许可证与对应源码说明

## 已验证

- 测试固件已在一台 U1 启动。
- 2026-06-18 完成 0.2 mm 外墙 + 0.4 mm 内墙/填充真实打印。

## 风险与恢复

该补丁不调节流量、压力提前、排料、擦嘴、工具偏移或首层。刷机前必须准备已知可用的恢复镜像；首个测试应使用小模型，并在前几层持续观察。

ESP32 延时摄影盒子与此固件补丁相互独立，只是在配套切片器构建中可以同时存在。
