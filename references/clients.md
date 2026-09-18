# 各 AI 客户端接入 FunSkill MCP

## 第一步：拿令牌（只需一次）

两种方式任选：

- **最快**：对你的 AI 说「打开 FunSkill 连接器接入页」→ 它调 `open_app(page="token")` 给你一条链接 → 打开后登录 → 生成并复制 `fsk_` 令牌
- **网页路径**：打开 `https://funskill.cn` → 登录 → 账户中心 → 连接器接入 → 生成令牌

统一服务地址：`https://funskill.cn/mcp`（Streamable HTTP）；鉴权：`Authorization: Bearer <你的令牌>`。

> 还没账号？对 AI 说「打开 FunSkill」，它会给你注册链接，打开后按提示注册即可。

---

## 豆包（Doubao 电脑版）

设置 → 扩展 / MCP（入口名称随版本可能变化，以客户端实际界面为准）→ 添加自定义 MCP：

- 服务地址：`https://funskill.cn/mcp`
- 鉴权头：`Authorization` = `Bearer fsk_你的令牌`

验证：对豆包说「有哪些 FunSkill 技能」。

## WorkBuddy

连接中心 → 添加自定义 MCP：

- 名称：`funskill`
- 网关地址：`https://funskill.cn/mcp`
- 鉴权：Bearer Token → 粘贴你的令牌

验证：对它说「有哪些 FunSkill 技能」。

## Cursor

`~/.cursor/mcp.json`（或项目内 `.cursor/mcp.json`）：

```json
{
  "mcpServers": {
    "funskill": {
      "url": "https://funskill.cn/mcp",
      "headers": { "Authorization": "Bearer fsk_你的令牌" }
    }
  }
}
```

## Claude Code / Claude Desktop

```bash
claude mcp add --transport http funskill https://funskill.cn/mcp \
  --header "Authorization: Bearer fsk_你的令牌"
```

## Codex CLI（OpenAI）

`~/.codex/config.toml`：

```toml
[mcp_servers.funskill]
url = "https://funskill.cn/mcp"
bearer_token_env_var = "FUNSKILL_TOKEN"
```

并 `export FUNSKILL_TOKEN="fsk_你的令牌"`。

## CodeBuddy

设置 → MCP → 添加服务器：

- 名称：`funskill`
- 类型：HTTP / Streamable HTTP
- URL：`https://funskill.cn/mcp`
- 请求头：`Authorization = Bearer fsk_你的令牌`

---

## 验证接入

对 AI 说：**「有哪些 FunSkill 技能」**

- 返回技能列表与价格 → 接入成功（逛市场匿名即可）
- 提示「需要提供 token」→ 令牌没配上，检查 Authorization 头
- 提示「入场资格未解锁 / 需要先购买」→ 令牌已生效，购买任意一款技能后解锁执行能力

## 常见问题

**Q：换设备/多客户端能用吗？**
一个账号最多登记 3 台设备；新设备在常用环境（同 IP+同浏览器内核）自动接管、免验证码；第 4 台走验证码复核（15 分钟内有效）；换设备不会轮换你的 MCP 令牌，配置无需重做。

**Q：令牌泄露？**
账户中心 → 连接器接入 → 重置令牌，旧令牌立即失效，更新配置即可。

**Q：为什么能看市场却用不了技能？**
FunSkill 是买断制两层授权：①入场资格——购买过任意一款有效技能（赠品不计）才解锁技能执行与开源生态技能的搜索/调用；②逐款授权——付费技能按款单独授权，购买 A 仅可调用 A。先逛市场、按需购买。

**Q：开源社区技能安全吗？**
安装前请通读其 SKILL.md 与脚本，检查是否有数据外传、危险命令或与描述不符的指令；优先选择官方源高安装量的。
