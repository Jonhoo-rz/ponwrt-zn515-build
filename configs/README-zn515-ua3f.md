# ZN515XG-D + UA3F 云端构建

新版本位于独立的 `zn515-ua3f` 分支。推送 UA3F 工作流或配置会自动触发
**Build ZN515XG-D + UA3F**；之后可在 Actions 中重新运行该构建。
原分支及原有 `build-zn515.yml` 均不修改。
流程编译触发运行时的本仓库提交，使用独立的 `build-zn515-ua3f.yml`。
两个版本的并发组和成品名称独立。
保留标准 ZN515XG-D 的 PON、Wi-Fi、USB、存储配置及禁用未发布 APK 源的处理。

UA3F 源码固定为 `SunBK201/UA3F@8747234bf24fe538b0358aabc7671b027fb37f20`，
由 Actions 检出到 `package/UA3F`，使用其 `openwrt/Makefile` 随固件交叉编译。
这是固定的源码提交，不能仅凭 Makefile 的版本号当成正式发行版。
`zn515-ua3f.config` 将 UA3F、LuCI 兼容层、Lua 依赖、ip-full 及 nftables TPROXY 支持设为 `y`。
内核模块通过 PonWrt 的 kmod 依赖规则生成，与固件内核一起编译，未使用发行版预编译 kmod。
保留 UA3F 上游声明的 iptables/NFQUEUE 等依赖；本配置未额外选择 nft NFQUEUE 或 eBPF 加速。

UA3F 保留上游默认配置：TPROXY、端口 1080、服务未启用。
刷入后在 **服务 → UA3F** 中配置并启用。构建并不代表设备运行验证完成。

成功产物包含 sysupgrade/recovery 镜像、SHA256SUMS、包清单及源码/feeds/UA3F 提交记录。
流程会检查最终配置、成品包清单和实际内核配置；缺少所需功能时构建失败。
完整配置与日志保存在 `zn515-ua3f-build-record-*` artifact，固件保存在 `ponwrt-ZN515XG-D-UA3F-*` artifact。
两类 artifact 保留 7 天。feeds 仍按原流程更新并记录提交，不保证不同时间运行完全相同。
