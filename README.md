<div align="center">

<h1>在 Claude Code 里使用 DeepSeek API</h1>

<h4>把 Claude Code 接到 DeepSeek 官方的 Anthropic 兼容接口上<br>用 DeepSeek 的模型和价格来跑 Claude Code</h4>

<sub><kbd>注册</kbd> &nbsp;→&nbsp; <kbd>实名认证</kbd> &nbsp;→&nbsp; <kbd>充值</kbd> &nbsp;→&nbsp; <kbd>创建 Key</kbd> &nbsp;→&nbsp; <kbd>放进目录</kbd> &nbsp;→&nbsp; <kbd>跑通</kbd></sub>

<br><br>

<sub>本仓库的核心文件：<a href="settings_example.json"><code>settings_example.json</code></a> — 复制到正确位置、填上 Key 即可使用</sub>

</div>

---

<table>
<thead>
<tr>
<th align="center">①</th>
<th align="center">②</th>
<th align="center">③</th>
<th align="center">④</th>
<th align="center">⑤</th>
<th align="center">⑥</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center"><b>安装</b><br><sub>Claude Code</sub><br><sub>第 0 步</sub></td>
<td align="center"><b>注册</b><br><sub>开放平台</sub><br><sub>第 1 步</sub></td>
<td align="center"><b>充值</b><br><sub>先付费后调用</sub><br><sub>第 2 步</sub></td>
<td align="center"><b>建 Key</b><br><sub>明文只显示一次</sub><br><sub>第 3 步</sub></td>
<td align="center"><b>放目录</b><br><mark>最容易错</mark><br><sub>第 4 步</sub></td>
<td align="center"><b>验证</b><br><sub><code>/status</code></sub><br><sub>第 5 步</sub></td>
</tr>
</tbody>
</table>

<blockquote>
<p>📖 <b>本说明有两个版本</b></p>
<ul>
<li>你正在看的 <b>README.md</b> — 下文<b>「安装」和「动手操作」两处就是标签栏</b>：点 <kbd>🪟 Windows</kbd> 会展开 Windows 的完整步骤，同时自动收起 macOS，<mark>一次只显示一个系统</mark></li>
<li><a href="index.html"><b>index.html</b></a> — 独立页面，带切换动画、代码复制按钮、深浅色适配。仓库页不能直接渲染它，需下载后本地打开</li>
</ul>
<p><sub>README 里这个标签栏是怎么做出来的？GitHub 会过滤掉 <code>&lt;script&gt;</code>、<code>&lt;style&gt;</code>、<code>style=""</code>、<code>class=""</code>，所以用不了 JS 和 CSS。这里靠的是 HTML 原生的 <code>&lt;details name="..."&gt;</code> —— 同一个 <code>name</code> 的多个折叠块会自动互斥，点开一个就关掉其他的，<mark>不需要一行脚本</mark>。展开的那块还会自动撑满整行宽度。</sub></p>
</blockquote>

---

<!-- ═══════════════════════════════════════════════════════════════════ -->
## <sub>第 0 步</sub> 安装 Claude Code

<div align="center"><sub>👇 &nbsp;点标签切换系统 &nbsp;·&nbsp; 一次只展开一个</sub></div>

<table>
<tr>
<td valign="top"><details name="install-os" open><summary><kbd>🍎 macOS</kbd></summary>

需要 Node.js 18 或更高版本。推荐用官方安装脚本：

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

或者用 npm 全局安装：

```bash
npm install -g @anthropic-ai/claude-code
```

装完验证：

```bash
claude --version
```

</details></td>
<td valign="top"><details name="install-os"><summary><kbd>🪟 Windows</kbd></summary>

**方式 A · 原生 PowerShell 安装**（推荐，比 WSL 省事）

```powershell
irm https://claude.ai/install.ps1 | iex
```

安装完 `claude` 会加入 PATH（系统查找可执行程序的目录列表）。<mark>关掉当前 PowerShell 再开一个新的窗口</mark>，让 PATH 刷新，然后验证：

```powershell
claude --version
```

**方式 B · 通过 WSL2**

先装 WSL2，在 Ubuntu 里按 Linux 标签页的方式装。

<blockquote>
<p>⚠️ <b>注意</b>　在 WSL 里跑 Claude Code，配置文件要走 <b>WSL 内部的 Linux 家目录</b>（<code>/home/你的用户名/.claude/settings.json</code>），不是 Windows 的 <code>C:\Users\...</code>。这两个是<ins>两套完全独立的文件系统</ins>，放错了必然不生效。</p>
</blockquote>

</details></td>
<td valign="top"><details name="install-os"><summary><kbd>🐧 Linux</kbd></summary>

脚本与 macOS 相同：

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

或者：

```bash
npm install -g @anthropic-ai/claude-code
```

装完验证：

```bash
claude --version
```

<blockquote>
<p>💡 WSL2 用户执行 <code>echo $HOME</code> 可以确认家目录到底在哪，配置就放在 <code>$HOME/.claude/settings.json</code>。</p>
</blockquote>

</details></td>
</tr>
</table>

---

## <sub>第 1 步</sub> 注册 DeepSeek 开放平台

| # | 操作 |
| :-: | :--- |
| **1** | 打开 <https://platform.deepseek.com/> |
| **2** | 点右上角 **Sign up / 注册**，用**邮箱**或**手机号**注册（也支持第三方登录） |
| **3** | 完成邮箱 / 短信验证，登录进控制台 |

<blockquote>
<p>⚠️ <b>别搞混两个站点</b></p>
<table>
<thead>
<tr><th>网址</th><th>是什么</th><th>有 API Key 吗</th></tr>
</thead>
<tbody>
<tr><td><a href="https://chat.deepseek.com/">chat.deepseek.com</a></td><td>网页版聊天</td><td>❌ 没有</td></tr>
<tr><td><a href="https://platform.deepseek.com/">platform.deepseek.com</a></td><td><b>开放平台</b></td><td>✅ 有，充值也在这</td></tr>
</tbody>
</table>
</blockquote>

---

## <sub>第 2 步</sub> 实名认证与充值

DeepSeek 的 API 是 **预付费** 模式：先往账户余额里充钱，再按实际消耗的 Token 从余额扣除。<br><sub>Token 是模型处理文本的最小单位，中文大约 1 个字 ≈ 1 个 Token</sub>

> 🔴 **余额为 0 时，即使 Key 是有效的，请求也会返回 `402 Payment Required`。**

| # | 操作 | 说明 |
| :-: | :--- | :--- |
| **1** | 右上角头像 → **账户设置** → **实名认证** | 个人上传身份证，企业上传营业执照。审核通过后状态显示「已通过」 |
| **2** | 右上角头像 → **账户余额 / 充值中心** | 支持 **支付宝 · 微信支付 · 对公银行转账**，最低金额以页面实时显示为准 |
| **3** | 回到 **API Keys** 页面确认状态 | 充值到账后，Key 的状态会变成「✅ 可用」 |

<blockquote>
<p>💰 新账号可能有赠送额度，但<b>不要依赖</b>老教程里的具体数字，以你自己的账单页为准。</p>
</blockquote>

---

## <sub>第 3 步</sub> 创建 API Key

| # | 操作 |
| :-: | :--- |
| **1** | 左侧导航进入 **API Keys** → <https://platform.deepseek.com/api_keys> |
| **2** | 点 **Create new API key** |
| **3** | 起个名字，建议「用途_环境」格式：<kbd>claude-code-mac</kbd> <kbd>claude-code-win</kbd> |
| **4** | **立刻复制弹出的 Key** — 它只在弹窗里显示这一次，关掉后平台不再提供明文，只能删掉重建 |

Key 的格式形如：

```text
sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

<blockquote>
<p>🔒 给不同设备 / 用途<b>分别建 Key</b>，方便单独吊销和统计用量。泄露了直接在同一页面删除即可。</p>
</blockquote>

---

## <sub>第 4 步</sub> 把配置放到正确的目录 &nbsp;<mark>关键</mark>

这是最容易出错的一步。Claude Code 会从**多个位置**读取 `settings.json`，放错地方就会出现「明明配了却不生效」。

<details open>
<summary>&nbsp;📂&nbsp;&nbsp;<b>有效的路径有哪些</b>&nbsp;&nbsp;<sub>（Mac / Windows 对照）</sub></summary>

<br>

<sub>Claude Code 会按下面的顺序查找 <code>settings.json</code>：</sub>

<table>
<thead>
<tr>
<th>位置</th>
<th>macOS</th>
<th>Windows</th>
<th>生效范围</th>
</tr>
</thead>
<tbody>
<tr>
<td>⭐ <b>用户级</b><br><sub>最常用</sub></td>
<td><code>~/.claude/<br>settings.json</code></td>
<td><code>%USERPROFILE%\.claude\<br>settings.json</code></td>
<td>这台机器上你的<br><b>所有项目</b></td>
</tr>
<tr>
<td><b>项目共享</b></td>
<td><code>&lt;项目&gt;/.claude/<br>settings.json</code></td>
<td><code>&lt;项目&gt;\.claude\<br>settings.json</code></td>
<td>该项目所有人<br><sub>会提交到 git</sub></td>
</tr>
<tr>
<td><b>项目本地</b></td>
<td><code>&lt;项目&gt;/.claude/<br>settings.local.json</code></td>
<td><code>&lt;项目&gt;\.claude\<br>settings.local.json</code></td>
<td>仅你自己<br><sub>不会被提交</sub></td>
</tr>
</tbody>
</table>

<sub>Windows 的 <code>%USERPROFILE%</code> 展开后就是 <code>C:\Users\你的用户名</code>。</sub>

</details>

<details open>
<summary>&nbsp;⚖️&nbsp;&nbsp;<b>优先级：谁覆盖谁</b></summary>

<br>

<table>
<thead>
<tr><th align="center">优先级</th><th>文件</th><th>谁在用</th></tr>
</thead>
<tbody>
<tr><td align="center"><b>高</b><br>↑<br><br><br><br><br><br><b>低</b></td>
<td>
<b>1</b> &nbsp;<code>managed-settings.json</code> &nbsp;<sub>托管策略</sub><br>
<b>2</b> &nbsp;<code>claude --settings</code> &nbsp;<sub>命令行启动参数</sub><br>
<b>3</b> &nbsp;<code>.claude/settings.local.json</code> &nbsp;<sub>当前项目 · 仅你</sub><br>
<b>4</b> &nbsp;<code>.claude/settings.json</code> &nbsp;<sub>当前项目 · 共享</sub><br>
<b>5</b> &nbsp;<code>~/.claude/settings.json</code> &nbsp;<sub>全局 · 优先级最低</sub>
</td>
<td>
组织<br>
本次会话<br>
你<br>
团队<br>
你 · 所有项目
</td>
</tr>
</tbody>
</table>

> ⚠️ **同一个配置项在上层文件里也写了，下层文件里的会被覆盖。** 比如当前项目里有 `.claude/settings.local.json`，它会盖掉你写在用户级的配置。

</details>

<details open>
<summary>&nbsp;🎯&nbsp;&nbsp;<b>那我该放哪个？</b></summary>

<br>

<table>
<tbody>
<tr>
<td align="center">🌍</td>
<td><b>想全局生效</b><br><sub>推荐大多数人</sub></td>
<td>放 <b>用户级</b> —— <code>~/.claude/settings.json</code></td>
</tr>
<tr>
<td align="center">📁</td>
<td><b>只想某个项目用</b><br><sub>其他项目继续用官方模型</sub></td>
<td>放该项目的 <code>.claude/settings.local.json</code></td>
</tr>
<tr>
<td align="center">👥</td>
<td><b>团队一起用</b></td>
<td>放 <code>.claude/settings.json</code> 并提交<br><sub>但<ins>千万别把 API Key 提交上去</ins>，见文末安全提醒</sub></td>
</tr>
</tbody>
</table>

</details>

### 动手操作

<div align="center"><sub>👇 &nbsp;点标签切换系统 &nbsp;·&nbsp; 一次只展开一个</sub></div>

<table>
<tr>
<td valign="top"><details name="config-os" open><summary><kbd>🍎 macOS</kbd></summary>

```bash
# 1. 确认目录存在（装完 Claude Code 通常已经有了）
mkdir -p ~/.claude

# 2. 备份已有配置（如果之前配过；没配过会报错，忽略即可）
cp ~/.claude/settings.json ~/.claude/settings.json.bak

# 3. 把仓库里的模板复制过去并改名
cp settings_example.json ~/.claude/settings.json

# 4. 打开编辑器，把 your_deepseek_api_key_here 换成你自己的 Key
open -e ~/.claude/settings.json
```

<blockquote>
<p>💡 在 Finder 里想看到 <code>~/.claude</code> 这个隐藏目录：按 <kbd>⌘</kbd> + <kbd>⇧</kbd> + <kbd>G</kbd>，输入 <code>~/.claude</code> 回车。</p>
</blockquote>

</details></td>
<td valign="top"><details name="config-os"><summary><kbd>🪟 Windows</kbd></summary>

```powershell
# 1. 确认目录存在
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"

# 2. 备份已有配置
if (Test-Path "$env:USERPROFILE\.claude\settings.json") {
    Copy-Item "$env:USERPROFILE\.claude\settings.json" "$env:USERPROFILE\.claude\settings.json.bak"
}

# 3. 把仓库里的模板复制过去并改名（先 cd 到本仓库目录）
Copy-Item ".\settings_example.json" "$env:USERPROFILE\.claude\settings.json"

# 4. 用记事本打开，替换 Key
notepad "$env:USERPROFILE\.claude\settings.json"
```

<blockquote>
<p>⚠️ <b>记事本保存时注意两件事</b></p>
<ul>
<li>编码选择 <b>UTF-8</b></li>
<li>别把文件存成了 <code>settings.json.txt</code> —— 在资源管理器「查看」里<mark>勾上「文件扩展名」</mark>，就能看清真实后缀</li>
</ul>
</blockquote>

想快速进到那个目录：在文件资源管理器地址栏直接输入 <code>%USERPROFILE%\.claude</code> 回车。

</details></td>
<td valign="top"><details name="config-os"><summary><kbd>🐧 Linux</kbd></summary>

```bash
# 1. 确认目录存在
mkdir -p ~/.claude

# 2. 备份已有配置
cp ~/.claude/settings.json ~/.claude/settings.json.bak

# 3. 把仓库里的模板复制过去并改名
cp settings_example.json ~/.claude/settings.json

# 4. 编辑，把 your_deepseek_api_key_here 换成你自己的 Key
nano ~/.claude/settings.json
```

<blockquote>
<p>💡 WSL2 同样适用，配置放在 WSL 内部的 <code>$HOME/.claude/settings.json</code>。</p>
</blockquote>

</details></td>
</tr>
</table>

### 配置每一项是什么意思

<details>
<summary>&nbsp;📋&nbsp;&nbsp;<b>展开看完整配置文件与逐项说明</b></summary>

<br>

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_deepseek_api_key_here",
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_DEFAULT_FABLE_MODEL": "deepseek-v4-pro[1M]",
    "ANTHROPIC_DEFAULT_FABLE_MODEL_NAME": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1M]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL_NAME": "deepseek-v4-pro",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1M]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL_NAME": "deepseek-v4-pro",
    "ANTHROPIC_MODEL": "deepseek-v4-pro",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1,
    "API_TIMEOUT_MS": "3000000"
  },
  "includeCoAuthoredBy": false,
  "model": "haiku",
  "effortLevel": "medium"
}
```

<br>

<table>
<thead>
<tr><th align="center">键</th><th>作用</th></tr>
</thead>
<tbody>
<tr>
<td><mark><code>ANTHROPIC_<br>AUTH_TOKEN</code></mark></td>
<td><b>你要改的就是这个。</b>填 DeepSeek 的 API Key，Claude Code 会自动加上 <code>Bearer&nbsp;</code> 前缀</td>
</tr>
<tr>
<td><code>ANTHROPIC_<br>BASE_URL</code></td>
<td>把请求指向 DeepSeek 的 Anthropic 兼容端点，而不是 Anthropic 官方</td>
</tr>
<tr>
<td><code>ANTHROPIC_DEFAULT_<br>OPUS / SONNET /<br>HAIKU_MODEL</code></td>
<td>分别定义 <code>opus</code> / <code>sonnet</code> / <code>haiku</code> 三个别名实际指向哪个 DeepSeek 模型</td>
</tr>
<tr>
<td><code>..._MODEL_NAME</code></td>
<td>界面上显示的模型名称，<sub>纯显示用</sub></td>
</tr>
<tr>
<td><code>ANTHROPIC_MODEL</code></td>
<td>默认使用的模型</td>
</tr>
<tr>
<td><code>CLAUDE_CODE_DISABLE_<br>NONESSENTIAL_TRAFFIC</code></td>
<td>关掉自动更新、遥测、错误上报等非必要网络请求<br><sub>走第三方接口时的推荐做法</sub></td>
</tr>
<tr>
<td><code>API_TIMEOUT_MS</code></td>
<td>请求超时时间（毫秒）。这里设得比较宽松，避免长回答被提前掐断</td>
</tr>
<tr>
<td><code>model</code></td>
<td>默认档位。<code>haiku</code> 即默认走 <code>deepseek-v4-flash</code>（便宜）。<br>想默认用 pro 就改成 <code>"opus"</code> 或 <code>"sonnet"</code></td>
</tr>
<tr>
<td><code>includeCoAuthoredBy</code></td>
<td>是否在 git 提交信息里加 <code>Co-Authored-By</code> 署名行</td>
</tr>
<tr>
<td><code>effortLevel</code></td>
<td>思考 / 努力程度档位</td>
</tr>
</tbody>
</table>

</details>

---

## <sub>第 5 步</sub> 启动并验证

```bash
claude
```

| # | 在交互界面里做 |
| :-: | :--- |
| **1** | 输入 <kbd>/status</kbd> ，确认模型和接口地址已经是 DeepSeek 的 |
| **2** | 随便问一句「你好，你是什么模型」测试连通性 |
| **3** | 用 <kbd>/model</kbd> 在 `opus` / `sonnet` / `haiku` 之间切换 |

<details>
<summary>&nbsp;🚨&nbsp;&nbsp;<b>报错了？对照这张表</b></summary>

<br>

<table>
<thead>
<tr><th align="center">报错</th><th>原因与处理</th></tr>
</thead>
<tbody>
<tr><td align="center"><code>401</code><br><sub>authentication_error</sub></td><td>Key 填错了，或者 Key 和账号不匹配</td></tr>
<tr><td align="center"><code>402</code><br><sub>Insufficient Balance</sub></td><td>余额不足，去充值</td></tr>
<tr><td align="center"><code>404</code><br><sub>model not found</sub></td><td>模型名写错了，对照 <a href="https://api-docs.deepseek.com/">api-docs.deepseek.com</a> 上的最新模型列表</td></tr>
<tr><td align="center">⏳</td><td>一直卡住不返回 —— 检查网络能否访问 <code>api.deepseek.com</code></td></tr>
</tbody>
</table>

</details>

---

## <sub>第 6 步</sub> 模型对照与费用控制

<details open>
<summary>&nbsp;🔀&nbsp;&nbsp;<b>模板建立的映射关系</b></summary>

<br>

<table>
<thead>
<tr><th>Claude Code 里的档位</th><th>实际调用的模型</th><th>定位</th></tr>
</thead>
<tbody>
<tr><td align="center"><kbd>haiku</kbd></td><td align="center"><code>deepseek-v4-flash</code></td><td>便宜、快，适合日常问答和后台任务</td></tr>
<tr><td align="center"><kbd>sonnet</kbd></td><td align="center"><code>deepseek-v4-pro[1M]</code></td><td>主力模型，长上下文</td></tr>
<tr><td align="center"><kbd>opus</kbd></td><td align="center"><code>deepseek-v4-pro[1M]</code></td><td>主力模型，长上下文</td></tr>
<tr><td align="center"><kbd>fable</kbd></td><td align="center"><code>deepseek-v4-pro[1M]</code></td><td>主力模型，长上下文</td></tr>
</tbody>
</table>

</details>

<details>
<summary>&nbsp;💸&nbsp;&nbsp;<b>省钱建议</b></summary>

<br>

- 日常就用 `haiku`（flash），遇到复杂任务再用 <kbd>/model</kbd> 切到 pro
- 在 [用量页](https://platform.deepseek.com/usage) 定期查看消耗，设置余额告警
- 不同设备用**不同的 Key**，方便定位是谁在消耗额度

</details>

---

<div align="center">
<h2>❓ 常见问题</h2>
</div>

<details>
<summary>&nbsp;<b>配好了但完全没生效，还是连的 Anthropic 官方？</b></summary>

<br>

按顺序排查：

| # | 检查 |
| :-: | :--- |
| **1** | 文件名是不是叫 `settings.json`<br><sub>不是 `settings_example.json`，也不是 `settings.json.txt`</sub> |
| **2** | 路径对不对<br><sub>macOS 是 `~/.claude/settings.json`，Windows 是 `%USERPROFILE%\.claude\settings.json`</sub> |
| **3** | JSON 格式是否合法 —— <mark>多一个逗号整个文件就会失效</mark><br><sub>用 `python3 -m json.tool ~/.claude/settings.json` 校验</sub> |
| **4** | 是不是被更上层的配置覆盖了<br><sub>检查当前项目的 `.claude/settings.json` 和 `.claude/settings.local.json`</sub> |
| **5** | 重启 Claude Code |

</details>

<details>
<summary>&nbsp;<b>环境变量和 settings.json 冲突时听谁的？</b></summary>

<br>

对 `ANTHROPIC_MODEL` 这类变量，Claude Code **先读环境变量**，环境变量没设时才用 `settings.json` 里的 `model` 值。

而 <kbd>--model</kbd> 命令行参数和 <kbd>/model</kbd> 命令的优先级又高于 `ANTHROPIC_MODEL`。

<blockquote>
<p>💡 如果你之前手动 <code>export</code> 过 <code>ANTHROPIC_*</code>，记得先清掉。</p>
</blockquote>

</details>

<details>
<summary>&nbsp;<b>想临时切回官方 Claude 模型怎么办？</b></summary>

<br>

把 `~/.claude/settings.json` 改名备份（例如 `settings.json.deepseek`），需要时再改回来。

更干净的做法：**不放用户级**，只放在具体项目的 `.claude/settings.local.json` 里。

</details>

<details>
<summary>&nbsp;<b>Windows 上装完 claude 命令找不到？</b></summary>

<br>

关掉当前终端窗口重开一个，让 PATH 刷新。

如果还不行，检查 `%USERPROFILE%\.local\bin` 是否在 PATH 中。

</details>

---

<div align="center">
<h2>🔒 安全提醒</h2>
</div>

<table>
<tbody>
<tr>
<td align="center">🔑</td>
<td><b>API Key 等同于账号密码</b></td>
<td>不要提交到 Git、不要贴在截图里、不要发到群里</td>
</tr>
<tr>
<td align="center">📄</td>
<td><b>保持模板是占位符</b></td>
<td>本仓库 <code>settings_example.json</code> 里是 <code>your_deepseek_api_key_here</code>，<mark>提交前确认它没被换成真 Key</mark></td>
</tr>
<tr>
<td align="center">👥</td>
<td><b>想共享配置给团队</b></td>
<td>把 Key 部分改成读环境变量，或只写到 <code>.claude/settings.local.json</code><br><sub>Claude Code 会自动把它加进全局 gitignore</sub></td>
</tr>
<tr>
<td align="center">🚨</td>
<td><b>Key 泄露了</b></td>
<td>立即去 <a href="https://platform.deepseek.com/api_keys">API Keys 页面</a> 删除并重建</td>
</tr>
</tbody>
</table>

---

<div align="center">

<h3>🔗 相关链接</h3>

<sub><a href="https://platform.deepseek.com/">DeepSeek 开放平台</a> &nbsp;·&nbsp; <a href="https://api-docs.deepseek.com/">DeepSeek API 文档</a> &nbsp;·&nbsp; <a href="https://code.claude.com/docs/en/settings">Claude Code · 设置文件与优先级</a> &nbsp;·&nbsp; <a href="https://code.claude.com/docs/en/env-vars">Claude Code · 环境变量</a></sub>

<br><br>

<sub><sub>本 README 只使用 GitHub 白名单内的 HTML 标签：<code>&lt;details name=""&gt;</code> <code>&lt;summary&gt;</code> <code>&lt;table&gt;</code> <code>&lt;kbd&gt;</code> <code>&lt;mark&gt;</code> <code>&lt;sub&gt;</code> <code>&lt;sup&gt;</code> <code>&lt;ins&gt;</code> <code>&lt;code&gt;</code> <code>&lt;blockquote&gt;</code> <code>&lt;div align=""&gt;</code> — 全部经 GitHub 渲染接口验证可正常显示。<code>&lt;button&gt;</code>、<code>&lt;script&gt;</code>、<code>&lt;style&gt;</code>、<code>class=""</code> 会被过滤，故未使用。</sub></sub>

</div>
