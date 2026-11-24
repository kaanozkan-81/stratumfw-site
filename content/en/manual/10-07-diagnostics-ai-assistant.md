---
title: "Diagnostics > AI Assistant"
seo_title: "10.7 Diagnostics > AI Assistant — Stratum FW User Manual"
description: "Asking about the firewall in plain language: providers (Claude, ChatGPT, DeepSeek, Gemini, local Ollama), API keys, masking public addresses, tool steps, example questions and privacy."
weight: 1007
chapter_num: "10.7"
url: "/manual/diagnostics-ai-assistant/"
aliases:
  - "/manual/10-07-diagnostics-ai-assistant/"
translationKey: "manual-10-07-diagnostics-ai-assistant"
---

The AI Assistant answers questions about **this firewall** in your own words - *"what was blocked in the last
hour, and from where?"*, *"are both WAN connections healthy?"* - with its **real, live state**. It reads the
firewall with the same 16 read-only tools as the MCP server ([**System > Settings > MCP**](/manual/system-settings-snmp-mcp/)) and **cannot change
anything**: when a change is needed, it explains where in the panel and how to make it.

It answers in the language of the question.

![Diagnostics > AI Assistant](/images/manual/ai.png)

Only administrators can use the AI Assistant.

## Settings

| Setting | Meaning |
| :--- | :--- |
| **Enable the AI assistant** | Off by default. |
| **Provider** | **Anthropic (Claude)**, **OpenAI (ChatGPT)**, **DeepSeek**, **Google Gemini**, or **Ollama (local, on your network)**. |
| **Model** | The model to use - for example `claude-sonnet-5-5` or `claude-haiku-5-5` for Anthropic, `deepseek-chat` for DeepSeek, the model name from your OpenAI account or Google AI Studio. For Ollama: a model that supports tools, for example `qwen2.5` or `llama3.1`. |
| **API key** | Your key from the provider (not for Ollama). Stored in a root-only file and never shown again; leave the field empty to keep it, or **Remove the saved key**. |
| **Ollama address** | (Ollama.) The server on your network that runs Ollama, for example `http://10.200.0.50:11434`. |
| **Mask public IP addresses (PUB-1, PUB-2 ...) before they are sent** | On by default - see **Privacy**. |
| **Tool steps per question** | How many times the model may read the firewall for one answer, 1-20 (default 8). More steps allow deeper answers and cost more. |

**Test** checks the provider, key and model; **Save** saves. When it is ready, the page says **Ready** with the
number of tools available.

The tools that are switched off on [**System > Settings > MCP**](/manual/system-settings-snmp-mcp/) are not available to the assistant either.

## Ask

Type a question and press **Ask** (Enter sends, Shift+Enter starts a new line). Suggestions to start with:

- *What was blocked in the last hour, and from where?*
- *Are both WAN connections healthy? Any loss or high latency?*
- *Summarise today's intrusion detection alerts. Is any inside device suspicious?*
- *Which firewall rules on LAN have never matched?*

More examples:

- *Which devices are on the guest network right now?*
- *Who is connected to the VPN?*
- *Why can't 192.168.1.50 reach the internet?*
- *Which applications used the most bandwidth today, and what did Application Control block?*
- *Which interface was busiest in the last 24 hours?*
- *What changed in the configuration today?*
- *How do I forward port 443 to my web server?*

Under each answer, badges show **which tools were used** (hover for the arguments) and how many public addresses
were masked - so you can see what the answer is based on.

A chat remembers the previous questions, so you can ask follow-ups (*"and from which countries?"*). **New chat**
starts over. Chats are kept in memory only, for an hour after their last use, and their content is not logged.

## Privacy

With a cloud provider, **the question and what the tools return are sent to the provider** - for example log
entries, addresses and rule names. Passwords, keys, tokens and VPN configurations are never returned by any tool.

- **Mask public IP addresses** replaces every public IPv4 address in the tool results with an alias (PUB-1,
  PUB-2 ...) before it leaves the firewall; the answer is shown with the real addresses again. Private addresses
  of your internal networks are sent as they are.
- Switch off the MCP tools whose data you do not want to share ([**System > Settings > MCP**](/manual/system-settings-snmp-mcp/)).
- With **Ollama**, nothing leaves your network: questions, data and answers stay on your own server.

## Choosing a provider

| Provider | Notes |
| :--- | :--- |
| Anthropic (Claude) | Strong at reading logs and rules; recommended. |
| OpenAI, Gemini, DeepSeek | Work through their OpenAI-compatible APIs. |
| Ollama | Free and private; needs a server with a GPU for good speed, and a model that supports tool calls. Smaller models give simpler answers. |

Cloud providers charge per use; the tool steps setting limits how much one question can cost.
