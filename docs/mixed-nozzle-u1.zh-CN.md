# Snapmaker U1 混合口径校验补丁

该补丁配套 Snapmaker Orca 的 `Min/2.3.5-beta-mixed-nozzle` 实验分支。

## 解决的问题

原 U1 打印任务校验会用切片器报告的第一个喷嘴直径检查所有已使用的物理头。混合口径任务即使物理喷嘴安装正确，也可能因此被拒绝。

补丁改为按下面的映射逐个校验：

`逻辑 T 槽 -> extruder_map_table -> 物理工具头 -> 实际喷嘴直径`

未使用的逻辑工具会按 `filament_used_g` 和 `filament_used_mm` 跳过。

补丁文件位于：

`overlays/firmware-extended/11-patch-klipper/patches/home/lava/klipper/09_mixed_nozzle_validation.patch`

## 范围与风险

它只修改喷嘴直径校验，不修改运动规划、挤出标定、压力提前、排料擦嘴、工具偏移、调平或断电恢复。打印质量和安全仍取决于正确的喷嘴安装、中心偏移、首层、流量和换头参数。

2026-06-18 已完成一次 0.2 mm 外墙 + 0.4 mm 内墙/填充的真实 U1 打印验证。其他口径、复杂模型和长时间打印仍未验证。

## 构建、许可与恢复

按仓库 Docker 流程构建：

```sh
./dev.sh make build PROFILE=extended OUTPUT_FILE=firmware/U1_extended_mixed_nozzle.bin
```

仓库和被修改的 Klipper 组件均使用 GPL-3.0。发布二进制时必须标明精确源码提交、保留许可证，并提供对应源码。不要把旧本地二进制冒充为新提交的构建。

刷机前保留已知可用的官方 U1 镜像，并按当前 Extended Firmware 对应的恢复/重刷流程操作。这是独立社区补丁，不是 Snapmaker 官方固件。

ESP32 延时摄影盒子与本补丁相互独立。它使用切片器输出的 `ESP_TIMELAPSE_SHOT` Klipper 宏，不依赖这个固件补丁。
