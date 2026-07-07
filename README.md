# 2HP：双跳链路搭建指南

这个仓库整理的是一套可公开分享的双跳代理链路方案：

```text
本地 Clash/Mihomo 客户端
  -> 第一跳 VPS：Hysteria2 入站 + sing-box 分流
  -> 尾段 SOCKS5 出口：目标地区落地
  -> 目标网站
```

设计目标是把“接入”和“最终出口”拆开：第一跳负责稳定接入和分流，尾段负责目标地区出口。普通流量可以直连或按本地规则处理，只有明确需要指定出口地区的域名才转发到尾段。

请在使用前确认你的用途符合所在地法律法规、云服务商规则和目标服务条款。

## 内容

| 文件 | 说明 |
| --- | --- |
| [index.html](index.html) | 可用于 GitHub Pages 的可视化阅读页。 |
| [双跳链路搭建分享文档.md](双跳链路搭建分享文档.md) | 从零搭建指南，使用占位符，不包含真实凭据。 |
| [双跳链路使用手册.md](双跳链路使用手册.md) | 脱敏后的日常使用、验证和排障手册。 |
| [桌面配置模板.md](桌面配置模板.md) | PC 客户端配置模板，便于网页下载。 |
| [Android-iOS配置模板.md](Android-iOS配置模板.md) | Android / iOS 移动端配置模板，便于网页下载。 |
| [examples/clash-verge.template.yaml](examples/clash-verge.template.yaml) | PC 端 Clash Verge / Mihomo YAML 模板。 |
| [examples/clash-android.template.yaml](examples/clash-android.template.yaml) | 移动端 Clash / Mihomo YAML 模板。 |

## 快速开始

1. 准备第一跳 VPS、尾段 SOCKS5 出口和本地 Clash/Mihomo 客户端。
2. 按 [双跳链路搭建分享文档.md](双跳链路搭建分享文档.md) 配置服务端。
3. 复制 `examples/` 下的模板，把 `<FIRST_IP>`、`<FIRST_PORT>`、`<HY2_PASSWORD>` 等占位符替换成自己的值。
4. 导入本地客户端，使用 `Rule` 模式，并确认目标规则没有误落到 `DIRECT`。
5. 参考 [双跳链路使用手册.md](双跳链路使用手册.md) 做日常验证和排障。

## 安全边界

这个公开版本只保存可视化首页、教程、模板和通用排障经验。真实服务器 IP、SSH 登录方式、系统密码、代理账号密码、本地可直接导入的配置文件都不应该进入公开仓库。

本地私密文件已经通过 `.gitignore` 排除。发布前请仍然执行一次检查：

```powershell
git status --short --ignored
git grep -n -I -E "password|secret|token|api[_-]?key|BEGIN .*PRIVATE|系统密码|代理密码|HY2_PASSWORD|SOCKS_PASS"
```

这个命令可能会命中模板里的占位符。只要命中的是 `<HY2_PASSWORD>`、`<SOCKS_PASS>` 这类占位符，而不是真实值，就可以继续。

如果任何真实凭据已经被提交到公开仓库，请先轮换或吊销凭据，再清理 Git 历史。

## License

MIT License，见 [LICENSE](LICENSE)。
