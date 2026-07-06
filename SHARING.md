# GitHub 分享清单

这份清单用于区分“可以直接公开”“必须留在本地”和“脱敏后再公开”的内容。

## 适合直接分享

这些文件不包含真实节点、账号或密码，适合放到 GitHub：

- `README.md`
- `index.html`
- `双跳链路搭建分享文档.md`
- `examples/clash-verge.template.yaml`
- `examples/clash-android.template.yaml`
- `.gitignore`
- `.gitattributes`
- `LICENSE`
- `SHARING.md`

它们适合分享的原因：内容以原理、步骤、模板和占位符为主，读者可以替换成自己的服务器和凭据。

## 不适合公开

这些文件包含真实运维信息或可直接连接的配置，应只留在本地：

- `运维文档.md`
- `链式代理项目记录.md`
- `双跳VPN使用手册.md`
- `VPN首段/VPNROOT.yaml`
- `VPN首段/VPNROOT-android.yaml`
- `.claude/settings.local.json`

不公开的原因：它们包含或可能包含真实公网 IP、SSH 用户、系统密码、代理账号密码、Hysteria2 密码、本地路径和个人工作流细节。

## 可以脱敏后分享

以下内容可以改写成公开版，但不能原样发布：

- 真实排障案例：删除公网 IP、账号、密码、机器名、云厂商实例信息。
- 日常使用手册：把真实配置块替换为 `examples/` 下的模板。
- 迁移记录：保留“为什么迁移”和“怎么验证”，删除旧 IP、新 IP 和可识别供应商信息。

脱敏时建议统一替换为：

- IP：`<FIRST_IP>`、`<EXIT_IP>`
- 端口：`<FIRST_PORT>`、`<SOCKS_PORT>`
- 密码：`<HY2_PASSWORD>`、`<SOCKS_PASS>`
- 用户名：`<SOCKS_USER>`、`<SSH_USER>`
- 本地路径：`<PROJECT_DIR>`、`<CONFIG_PATH>`

## 推荐分享方式

1. 在本目录初始化 Git：

```powershell
git init
git branch -M main
```

2. 只添加公开文件：

```powershell
git add README.md index.html SHARING.md LICENSE .gitignore .gitattributes examples "双跳链路搭建分享文档.md"
git status --short --ignored
```

3. 确认 `运维文档.md`、`链式代理项目记录.md`、`双跳VPN使用手册.md`、`VPN首段/` 显示为 ignored，而不是 staged。

4. 做一次敏感信息扫描：

```powershell
git grep -n -I -E "password|secret|token|api[_-]?key|BEGIN .*PRIVATE|系统密码|代理密码|HY2_PASSWORD|SOCKS_PASS"
```

这个命令可能会命中模板里的占位符。只要命中的是 `<HY2_PASSWORD>`、`<SOCKS_PASS>` 这类占位符，而不是真实值，就可以继续。

5. 提交并推送：

```powershell
git commit -m "docs: prepare public double-hop guide"
git remote add origin https://github.com/AdolphGong/2HP.git
git push -u origin main
```

## 发布前最后检查

- GitHub 页面里不出现真实服务器 IP。
- GitHub 页面里不出现真实 SSH 用户、系统密码、代理账号或代理密码。
- `examples/` 里只有占位符。
- README 明确说明需要遵守当地法律法规、云厂商规则和目标服务条款。
- 如果曾经把真实配置推到远端，先轮换所有相关密码和密钥，再清理历史。