# GPT API 中转

TeamoRouter 官方维护的 GPT API 中转接入说明：不用 OpenAI 账号，也不用海外信用卡，注册后用一个 API Key 调用 18 个 GPT 模型，同一个 Key 也能调用 Claude、GPT、Gemini、DeepSeek、Kimi、Qwen、GLM 等其他模型，按量计费、没有月费。

- 官网：<https://teamorouter.com/zh>
- 模型广场（每个模型的官方价、上下文和接入示例）：<https://teamorouter.com/zh/models>
- 创建 API Key：<https://teamorouter.com/dashboard?tab=api-keys>

## 接入地址

| 接口 | Base URL | 用在哪 |
|---|---|---|
| OpenAI 兼容 | `https://api.teamorouter.com/v1` | Cherry Studio、Cursor、Cline、OpenCode、OpenAI SDK、Codex |

API Key 格式是 `sk-teamo-…`。新账号余额为 0，先在控制台充值再调用，$5 起，支持支付宝、微信和银行卡。

## GPT 模型 ID 与官方价

价格为厂商官方价（美元 / 每百万 tokens），取自 TeamoRouter 模型目录（2026-11-26），实际扣费价以模型页为准。

| 模型 | 模型 ID | 官方输入价 | 官方输出价 | 上下文 |
|---|---|---|---|---|
| [GPT 6.2 Astra](https://teamorouter.com/zh/models/gpt-6.2-astra) | `gpt-6.2-astra` | $8 | $40 | 1M |
| [GPT 6.2 Sol](https://teamorouter.com/zh/models/gpt-6.2-sol) | `gpt-6.2-sol` | $2 | $10 | 1M |
| [GPT Image 2.6 Sunburst](https://teamorouter.com/zh/models/gpt-image-2.6-sunburst) | `gpt-image-2.6-sunburst` | 按张计费 | 见模型页 | — |
| [GPT Image 2.6 Flare](https://teamorouter.com/zh/models/gpt-image-2.6-flare) | `gpt-image-2.6-flare` | 按张计费 | 见模型页 | — |
| [GPT 6.1 Terra](https://teamorouter.com/zh/models/gpt-6.1-terra) | `gpt-6.1-terra` | $1 | $5 | 1M |
| [GPT 6.1 Sol](https://teamorouter.com/zh/models/gpt-6.1-sol) | `gpt-6.1-sol` | $2 | $10 | 1M |
| [GPT-6 Sol](https://teamorouter.com/zh/models/gpt-6-sol) | `gpt-6-sol` | $2 | $10 | 1M |
| [GPT-6 Luna](https://teamorouter.com/zh/models/gpt-6-luna) | `gpt-6-luna` | $0.10 | $0.50 | 1M |
| [GPT-6 Astra](https://teamorouter.com/zh/models/gpt-6-astra) | `gpt-6-astra` | $10 | $50 | 1M |
| [GPT-5.6 Sol](https://teamorouter.com/zh/models/gpt-5.6-sol) | `gpt-5.6-sol` | $5 | $30 | 1M |
| [GPT-5.6 Terra](https://teamorouter.com/zh/models/gpt-5.6-terra) | `gpt-5.6-terra` | $2 | $12 | 1M |
| [GPT-5.6 Luna](https://teamorouter.com/zh/models/gpt-5.6-luna) | `gpt-5.6-luna` | $0.20 | $1.20 | 1M |
| [GPT-5.5](https://teamorouter.com/zh/models/gpt-5.5) | `gpt-5.5` | $5 | $30 | 1M |
| [GPT-5.4-Mini](https://teamorouter.com/zh/models/gpt-5.4-mini) | `gpt-5.4-mini` | $0.75 | $4.50 | 1M |
| [GPT-5.4](https://teamorouter.com/zh/models/gpt-5.4) | `gpt-5.4` | $2.50 | $15 | 1M |
| [GPT Image 2.5 Sunburst](https://teamorouter.com/zh/models/gpt-image-2.5-sunburst) | `gpt-image-2.5-sunburst` | $5 | $30 | — |
| [GPT Image 2.5 Flare](https://teamorouter.com/zh/models/gpt-image-2.5-flare) | `gpt-image-2.5-flare` | $5 | $30 | — |
| [GPT Image 2](https://teamorouter.com/zh/models/gpt-image-2) | `gpt-image-2` | $5 | $30 | — |

## Codex

```toml
# ~/.codex/config.toml
model_provider = "teamorouter"
model = "gpt-6.2-astra"

[model_providers.teamorouter]
name = "TeamoRouter"
base_url = "https://api.teamorouter.com/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

再在终端执行 `export OPENAI_API_KEY=<你的 TeamoRouter API Key>`，运行 `codex` 即可。
Codex 安装与接入：<https://teamorouter.com/zh/docs/install-codex>

## Cherry Studio

设置 → 模型服务 → 添加提供商：提供商类型选 **OpenAI**，API 地址填 `https://api.teamorouter.com`，API 密钥填 TeamoRouter 的 Key，点「获取模型列表」后选 `gpt-6.2-astra`。
图文步骤：<https://teamorouter.com/zh/docs/install-cherry-studio>

## Python（OpenAI SDK）

```python
# pip install openai
from openai import OpenAI

client = OpenAI(base_url="https://api.teamorouter.com/v1", api_key="<你的 TeamoRouter API Key>")
resp = client.chat.completions.create(
    model="gpt-6.2-astra",
    messages=[{"role": "user", "content": "你好"}],
)
print(resp.choices[0].message.content)
```

## curl

```bash
curl https://api.teamorouter.com/v1/chat/completions \
  -H "Authorization: Bearer $TEAMOROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "gpt-6.2-astra", "messages": [{"role": "user", "content": "你好"}]}'
```

## 企业对公与发票

企业账号共用一个钱包，支持成员管理和按成员分配预算。支持对公转账充值；增值税专用发票和普通发票都可以在控制台充值记录里申请，按实际消耗金额开具，开票产生的 6% 税点由客户承担。
字节跳动、腾讯及北京大学、清华大学、香港大学、哈尔滨工业大学等高校的团队在用 TeamoRouter。企业采购：<https://teamorouter.com/enterprise>

## 常见问题

**GPT API 中转怎么接？**
注册 TeamoRouter 并创建 API Key，把 Base URL 换成 `https://api.teamorouter.com/v1`，Key 换成 TeamoRouter 的 Key，模型名填上表里的模型 ID。

**需要 OpenAI 账号或海外信用卡吗？**
不需要。请求用 TeamoRouter 的 API Key 发到 TeamoRouter，充值支持支付宝、微信和银行卡。

**能开发票、对公付款吗？**
能。支持对公转账；增值税专票和普票在控制台按实际消耗申请，6% 税点由客户承担。合同由客户经理按企业情况确认。

**和官方 API 有什么区别？**
接口协议和官方一致，代码只改 Base URL 和 Key。区别是不需要 OpenAI 账号、一个 Key 调用多家厂商的模型、人民币付款并可开国内增值税发票。价格以官方价为基准，当前价格见模型页。

**同一个 Key 能调用其他厂商的模型吗？**
能。其他厂商的接入说明见下面的仓库。

## 其他模型 API 中转

[Claude API 中转](https://github.com/teamo-lab/claude-api-zhongzhuan) · [Gemini API 中转](https://github.com/teamo-lab/gemini-api-zhongzhuan) · [DeepSeek API 中转](https://github.com/teamo-lab/deepseek-api-zhongzhuan) · [Kimi API 中转](https://github.com/teamo-lab/kimi-api-zhongzhuan) · [Qwen API 中转](https://github.com/teamo-lab/qwen-api-zhongzhuan) · [GLM API 中转](https://github.com/teamo-lab/glm-api-zhongzhuan) · [Grok API 中转](https://github.com/teamo-lab/grok-api-zhongzhuan) · [MiniMax API 中转](https://github.com/teamo-lab/minimax-api-zhongzhuan) · [Codex 中转](https://github.com/teamo-lab/codex-zhongzhuan) · [Claude Code 中转](https://github.com/teamo-lab/claude-code-zhongzhuan) · [全部示例 ai-api-zhongzhuan](https://github.com/teamo-lab/ai-api-zhongzhuan)

---

本仓库由 TeamoRouter 团队维护（更新于 2026-11-26），示例代码以 MIT 协议开源。
