# mihub

个人 clash / mihomo 代理配置用到的第三方资源镜像站。**不改一个字节，原样转发**——纯粹是因为直连下面这些项目在部分网络环境下不稳定，才每天自动转存一份，供 Forgejo / Gitee 等下游 pull mirror。

仓库分两个分支：`main` 只放 workflow/README/LICENSE 这些"源码"，人工维护，正常线性历史；实际同步下来的文件（下面表格里的所有东西）都在 **`release` 分支**，引用本仓库文件时用 `raw/branch/release/<路径>`，例如：
`https://git.zopiya.dev/mirror/mihub/raw/branch/release/ui/metacubexd.zip`。

## 📡 最新同步状态

<!-- SYNC_STATUS:START -->
> 最后一次运行：**从未运行**（UTC）

| 资源 | 当前版本 | 更新时间 (UTC) |
| --- | --- | --- |
<!-- SYNC_STATUS:END -->

这张表由 [sync.yml](.github/workflows/sync.yml) 每天自动更新，不用手动维护。

## 镜像了什么

| 资源 | 上游 | 许可证 |
| --- | --- | --- |
| metacubexd | [MetaCubeX/metacubexd](https://github.com/MetaCubeX/metacubexd) | MIT |
| Yacd-meta | [MetaCubeX/Yacd-meta](https://github.com/MetaCubeX/Yacd-meta) | 未附带 LICENSE；上游 haishanh/yacd 为 MIT |
| zashboard | [Zephyruso/zashboard](https://github.com/Zephyruso/zashboard) | MIT |
| geoip / geosite / country.mmdb | [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat) | GPL-3.0 |
| Qure（图标，整仓库镜像） | [Koolson/Qure](https://github.com/Koolson/Qure) | 未声明 |
| clash-rules 规则文件（整仓库镜像） | [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules) | GPL-3.0 |
| GeoLite2-ASN.mmdb（暂未接入任何配置） | [xishang0128/geoip](https://github.com/xishang0128/geoip) | CC-BY-SA-4.0 + [MaxMind EULA](https://www.maxmind.com/en/geolite2/eula) |
| mihomo 核心 | [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo) | GPL-3.0 |
| Clash Verge Rev | [clash-verge-rev/clash-verge-rev](https://github.com/clash-verge-rev/clash-verge-rev) | GPL-3.0 |
| FlClash | [chen08209/FlClash](https://github.com/chen08209/FlClash) | GPL-3.0 |

客户端安装包只镜像本人设备实际会用的平台/架构，不做全平台全架构：

| 项目 | macOS | Windows | Linux | Android |
| --- | --- | --- | --- | --- |
| mihomo 核心 | arm64 | amd64 | amd64（原生压缩包，上游无 deb/rpm） | — |
| Clash Verge Rev | aarch64 | x64 | amd64（deb + rpm） | — |
| FlClash | arm64 | x64 | amd64（deb + rpm） | arm64-v8a |

## 怎么运作的

每天一次，`sync.yml` 对每个资源：

1. 查上游最新版本，和 `release` 分支 `_meta/*.json` 里记的上次版本比对，没变就跳过（省流量）。
2. 变了就下载校验：压缩包完整性、体积不低于上次的一半、关键文件存在等。**校验不过就保留旧版本，绝不用坏数据覆盖好数据。**
3. 有更新就把 `release` 分支当前状态压成单个 commit 强推（历史永远只有一个 commit，体积不会随时间累积）；`main` 只追加一条很小的 README 状态更新 commit，正常历史，不 squash。下游对本仓库做 pull mirror 就行，不需要额外配置。
4. 只要有资源校验失败，这次 run 就会标红并触发 GitHub 邮件通知——不会悄悄带病运行。

## 手动跑一次 / 排查问题

Actions 页面 `Run workflow` 时勾上 `force`，可以无视版本比对强制刷新全部资源，调试正则或验证修复很好用。

Clash Verge Rev / FlClash 那几条资产名匹配正则是按官方打包习惯写的，没有逐一核对过真实文件名。如果日志报"未匹配到 asset"或"未找到对应 asset"，去对应仓库的 Releases 页面看一眼当次实际文件名，改一下 [sync.yml](.github/workflows/sync.yml) 里那条正则就行，不影响其它资源。

如果给 `release` 分支开了 branch protection，记得给这个 workflow 的 push 开例外，否则 squash 之后的强推会被拒绝。

## 许可证

本仓库自己的原创内容（README、workflow 脚本）按 [MIT](LICENSE) 授权；镜像的第三方文件原样转发，各自遵循上表里列的上游许可证。

版本更新完全跟随上游节奏，本仓库不对内容的时效性/可用性/安全性做任何保证，功能问题请优先反馈给对应上游项目。
