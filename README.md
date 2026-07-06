# 2HP：双跳链路搭建指南

这个仓库整理的是一套可公开分享的双跳代理链路方案：

```text
本地 Clash/Mihomo 客户端
  -> 第一跳 VPS：Hysteria2 入站 + sing-box 分流
  -> 第二跳 SOCKS5 出口：目标地区落地
  -> 目标网站
```

设计目标是把“接入”和“最终出口”拆开：第一跳负责稳定接入和分流，第二跳负责目标地区出口。普通流量可以直连或走第一跳直连，只有明确需要指定出口地区的域名才转发到第二跳。

请在使用前确认你的用途符合当地法律法规、云厂商规则和目标服务条款。

## 内容

| 文件 | 说明 |
| --- | --- |
| [index.html](index.html) | 可用于 GitHub Pages 的可视化项目首页。 |`n| [双跳链路搭建分享文档.md](双跳链路搭建分享文档.md) | 从零搭建指南，使用占位符，不包含真实凭据。 |
| [examples/clash-verge.template.yaml](examples/clash-verge.template.yaml) | 电脑端 Clash Verge / Mihomo 配置模板。 |
| [examples/clash-android.template.yaml](examples/clash-android.template.yaml) | 安卓端 Clash / Mihomo 配置模板。 |
| [SHARING.md](SHARING.md) | 哪些内容适合公开、哪些必须保留本地，以及 GitHub 发布步骤。 |

## 快速开始

1. 准备第一跳 VPS、第二跳 SOCKS5 出口和本地 Clash/Mihomo 客户端。
2. 按 [双跳链路搭建分享文档.md](双跳链路搭建分享文档.md) 配置服务端。
3. 复制 `examples/` 下的模板，把 `<FIRST_IP>`、`<FIRST_PORT>`、`<HY2_PASSWORD>` 等占位符替换成自己的值。
4. 导入本地客户端，使用 `Rule` 模式，并确认代理组不包含不希望回退的 `DIRECT`。
5. 用目标网站的响应头、客户端日志和第一跳日志确认分流是否符合预期。

## 安全边界

这个公开版本只保存可视化首页、教程、模板和通用排障经验。真实服务器 IP、SSH 登录方式、系统密码、代理账号密码、本地可直接导入的配置文件都不应该进入公开仓库。

本地私密文件已经通过 `.gitignore` 排除。发布前请仍然执行一次检查：

```powershell
git status --short --ignored
git grep -n -I -E "password|secret|token|api[_-]?key|BEGIN .*PRIVATE|系统密码|代理密码|HY2_PASSWORD|SOCKS_PASS"
```

如果任何真实凭据已经被提交到公开仓库，请先轮换/吊销凭据，再清理 Git 历史。

## License

MIT License，见 [LICENSE](LICENSE)。