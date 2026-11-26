---
title: "AI Assistant & MCP"
seo_title: "Stratum Firewall AI Assistant: Read-Only MCP & Local Ollama"
description: "Ask Stratum Firewall in plain language with Diagnostics > AI Assistant. 16 read-only MCP tools, automatic public IP masking, Claude, OpenAI, DeepSeek, Gemini, and local Ollama."
seoDescription: "Natural-language FreeBSD firewall diagnostics via 16 read-only MCP tools, PUB-1 public IP masking, zero write risk, and 100% local Ollama or cloud AI support."
date: 2026-11-18
translationKey: "product-ai-assistant"
aliases: ["/ai-assistant/"]
---

<style>
.container *:has(.ai-wrap), main article:has(.ai-wrap), .prose:has(.ai-wrap), .content:has(.ai-wrap), .markdown:has(.ai-wrap), .single-content:has(.ai-wrap), .article-content:has(.ai-wrap), .page-content:has(.ai-wrap), .post-content:has(.ai-wrap) { max-width:100% !important; width:100% !important; margin-left:0 !important; margin-right:0 !important; }
.ai-wrap { width:100% !important; max-width:1240px !important; margin:1.5rem auto 3rem auto !important; display:flex !important; flex-direction:column !important; gap:2.5rem !important; }
.ai-dark { width:100% !important; box-sizing:border-box !important; background:#08101f !important; border:1px solid #1e293b !important; border-radius:20px !important; padding:2.5rem !important; box-shadow:0 24px 52px -12px rgba(8,16,31,0.48) !important; }
.ai-eyebrow { display:inline-block !important; font-size:0.75rem !important; font-weight:700 !important; letter-spacing:0.09em !important; text-transform:uppercase !important; color:#34d399 !important; background:rgba(16,185,129,0.12) !important; border:1px solid rgba(52,211,153,0.35) !important; padding:0.3rem 0.78rem !important; border-radius:999px !important; margin-bottom:0.8rem !important; }
.ai-dark h2 { color:#f8fafc !important; font-size:clamp(1.6rem,2.4vw,2.05rem) !important; line-height:1.22 !important; margin:0 0 0.7rem 0 !important; font-weight:800 !important; max-width:none !important; }
.ai-lead { color:#cbd5e1 !important; font-size:1rem !important; line-height:1.65 !important; margin:0 0 1.65rem 0 !important; max-width:1040px !important; }
.ai-kpis { display:grid !important; grid-template-columns:repeat(4,minmax(0,1fr)) !important; gap:1.15rem !important; margin-bottom:1.75rem !important; }
.ai-kpi { background:#0f172a !important; border:1px solid #1e293b !important; border-radius:14px !important; padding:1.15rem 1.3rem !important; }
.ai-kpi strong { display:block !important; font-size:1.55rem !important; font-weight:800 !important; color:#f8fafc !important; line-height:1.15 !important; white-space:nowrap !important; }
.ai-kpi span { display:block !important; font-size:0.84rem !important; color:#94a3b8 !important; margin-top:0.35rem !important; line-height:1.4 !important; }
.ai-fig { width:100% !important; max-width:100% !important; margin:0 !important; border-radius:16px !important; overflow:hidden !important; border:1px solid #1e293b !important; background:#050b16 !important; }
.ai-fig img { display:block !important; width:100% !important; max-width:100% !important; height:auto !important; margin:0 !important; }
.ai-fig figcaption { padding:0.8rem 1.2rem !important; font-size:0.86rem !important; color:#94a3b8 !important; text-align:center !important; border-top:1px solid #1e293b !important; max-width:none !important; }
.ai-grid2 { display:grid !important; grid-template-columns:repeat(2,minmax(0,1fr)) !important; gap:1.35rem !important; margin-top:1.5rem !important; }
.ai-grid3 { display:grid !important; grid-template-columns:repeat(3,minmax(0,1fr)) !important; gap:1.25rem !important; margin-top:1.5rem !important; }
.ai-card { background:#0f172a !important; border:1px solid #1e293b !important; border-radius:14px !important; padding:1.5rem 1.6rem !important; }
.ai-badge { display:inline-block !important; font-size:0.72rem !important; font-weight:700 !important; letter-spacing:0.07em !important; text-transform:uppercase !important; color:#fbbf24 !important; margin-bottom:0.45rem !important; }
.ai-card h3 { color:#f8fafc !important; font-size:1.16rem !important; font-weight:700 !important; margin:0 0 0.7rem 0 !important; }
.ai-card p { color:#cbd5e1 !important; font-size:0.9rem !important; line-height:1.58 !important; margin:0 !important; max-width:none !important; }
.ai-card ul { margin:0 !important; padding-left:1.2rem !important; display:flex !important; flex-direction:column !important; gap:0.5rem !important; max-width:none !important; }
.ai-card li { color:#cbd5e1 !important; font-size:0.9rem !important; line-height:1.56 !important; margin:0 !important; max-width:none !important; }
.ai-card li strong { color:#f8fafc !important; font-weight:700 !important; }
.ai-card code { background:#1e293b !important; color:#7dd3fc !important; padding:0.12rem 0.38rem !important; border-radius:4px !important; font-size:0.82rem !important; }
.ai-cta-row { display:flex !important; flex-wrap:wrap !important; gap:0.85rem !important; margin-top:1.6rem !important; }
.ai-btn-pri { display:inline-flex !important; align-items:center !important; padding:0.72rem 1.35rem !important; border-radius:9px !important; background:#f59e0b !important; color:#0f172a !important; font-weight:700 !important; font-size:0.9rem !important; text-decoration:none !important; }
.ai-btn-sec { display:inline-flex !important; align-items:center !important; padding:0.72rem 1.3rem !important; border-radius:9px !important; background:#1e293b !important; color:#f8fafc !important; border:1px solid #334155 !important; font-weight:600 !important; font-size:0.9rem !important; text-decoration:none !important; }
@media (max-width:1024px) {
.ai-kpis { grid-template-columns:repeat(2,minmax(0,1fr)) !important; }
.ai-grid2, .ai-grid3 { grid-template-columns:1fr !important; }
}
@media (max-width:640px) {
.ai-kpis { grid-template-columns:1fr !important; }
.ai-dark { padding:1.4rem !important; }
}
</style>
<div class="ai-wrap">
<section class="ai-dark">
<span class="ai-eyebrow">DIAGNOSTICS &rarr; AI ASSISTANT &bull; </span>
<h2>Ask Your Firewall in Your Own Words — Zero Write Risk</h2>
<p class="ai-lead">Under Diagnostics &rarr; AI Assistant, administrators can query live firewall telemetry in plain natural language ("What was blocked in the last hour and from where?", "Is WAN2 healthy?", "Is any inside device suspicious?"). The assistant answers in the language of your question using 14 deterministic read-only MCP tools, shows every tool executed beneath its answer, and guides you to the exact panel page whenever a configuration change is needed.</p>
<div class="ai-kpis">
<div class="ai-kpi"><strong>15 Tools</strong><span>Read-Only Live MCP Telemetry</span></div>
<div class="ai-kpi"><strong>0 Write Risk</strong><span>Cannot Alter pf, NAT or System Config</span></div>
<div class="ai-kpi"><strong>PUB-1 Mask</strong><span>Public IPs Aliased Before Cloud Calls</span></div>
<div class="ai-kpi"><strong>5 Engines</strong><span>Claude, OpenAI, DeepSeek, Gemini &amp; Ollama</span></div>
</div>
<figure class="ai-fig">
<img src="/images/ai-assistant-ui.jpg" alt="Stratum Firewall Built-in AI Assistant Live Interface" loading="eager" width="1376" height="768">
<figcaption>Live natural-language diagnostics with deterministic rule math, transparent MCP tool badges, and safe panel navigation pointers.</figcaption>
</figure>
</section>
<section class="ai-dark">
<span class="ai-eyebrow">PRIVACY-BY-DESIGN &bull; ZERO SECRET LEAKS</span>
<h2>How Stratum Protects Sensitive Network Data During AI Queries</h2>
<p class="ai-lead">Connecting an AI model to perimeter security telemetry demands strict boundaries. Stratum FW sanitizes public addresses automatically on-box, blocks secret access at the tool layer, and supports 100% air-gapped local inference via Ollama.</p>
<figure class="ai-fig">
<img src="/images/ai-assistant-privacy.jpg" alt="Stratum AI Assistant Privacy Filter and Local Ollama Architecture" loading="lazy" width="1200" height="896">
<figcaption>Outgoing public IPs are masked as PUB-1, PUB-2 before leaving the appliance and restored locally inside your browser answer; with local Ollama, zero packets leave your LAN.</figcaption>
</figure>
<div class="ai-grid2">
<div class="ai-card">
<span class="ai-badge">AUTOMATIC ANONYMIZATION</span>
<h3>Public IP Aliasing &amp; Ephemeral Memory</h3>
<ul>
<li><strong>On-by-default IP masking:</strong> Public IPv4 addresses are replaced with ephemeral aliases (<code>PUB-1</code>, <code>PUB-2</code>...) before reaching external providers and seamlessly restored to real addresses in the displayed answer.</li>
<li><strong>Zero secrets exposed:</strong> No MCP tool ever reads or returns passwords, <code>0600</code> private keys, API tokens, or VPN client profiles; provider API keys stay in <code>0600</code> root-only files (included in encrypted <code>.stbk</code> backups) with a one-click <strong>Test</strong> button.</li>
<li><strong>1-hour RAM-only sessions:</strong> Restricted strictly to Administrators; who asked and which tools ran is audit-logged, while questions and answers are never written to disk and expire from memory after one hour.</li>
</ul>
</div>
<div class="ai-card">
<span class="ai-badge">DETERMINISTIC ACCURACY &bull; </span>
<h3>Exact Rule Counts &amp; External MCP Clients</h3>
<ul>
<li><strong>Pre-computed rule &amp; NAT math:</strong> Instead of letting an LLM miscount long port-forward or rule tables, Stratum's MCP tools compute exact totals (<code>all</code>, <code>enabled</code>, <code>disabled</code>) deterministically.</li>
<li><strong>Clear counter semantics:</strong> Tools explicitly report that a <code>0</code> packet counter means <em>"not matched since the last Apply/reload"</em> rather than <em>"never used"</em>, eliminating false cleanup advice.</li>
<li><strong>Built-in panel + external MCP server:</strong> Use the in-panel chat or connect external desktop MCP clients (<code>System &rarr; Settings &rarr; MCP</code>) with revocable, IP-restricted, hashed tokens and per-tool on/off switches.</li>
</ul>
</div>
</div>
</section>
<section class="ai-dark">
<span class="ai-eyebrow">FLEXIBLE DEPLOYMENT &bull; CLOUD OR AIR-GAPPED LAN</span>
<h2>Choose Cloud Frontier Models or 100% Local Ollama</h2>
<div class="ai-grid3">
<div class="ai-card">
<span class="ai-badge">LOCAL AIR-GAPPED AI</span>
<h3>On-Premise Ollama Server</h3>
<p>Point Stratum FW to an <strong>Ollama</strong> instance on your internal LAN. Every prompt, log summary, and diagnostic answer stays 100% inside your building with zero external internet traffic and zero API cost.</p>
</div>
<div class="ai-card">
<span class="ai-badge">PRODUCTION-TESTED CLOUD</span>
<h3>Claude, OpenAI, DeepSeek &amp; Gemini</h3>
<p>Connect Anthropic Claude, OpenAI, DeepSeek, or Google Gemini with automatic <code>PUB-1</code> public IP masking. Production-verified with DeepSeek answering in the administrator's language while pointing cleanly to target panel screens.</p>
</div>
<div class="ai-card">
<span class="ai-badge">14 LIVE TELEMETRY TOOLS</span>
<h3>Full Visibility, Zero Lockout Risk</h3>
<p>Covers system health, interfaces, WAN latency/failover, services, live firewall logs, blocked-traffic summaries, firewall &amp; NAT rules, DHCP leases, VPN sessions, Suricata IDS alerts, config history, blocklists, and traffic graphs.</p>
</div>
</div>
<div class="ai-cta-row">
<a href="https://demo.stratumfw.com" target="_blank" rel="noopener" class="ai-btn-pri">Inspect Diagnostics in Live Demo &rarr;</a>
<a href="/download/" class="ai-btn-sec">Download Stratum FW</a>
<a href="/download/#changelog" class="ai-btn-sec">View Release Notes</a>
</div>
</section>
</div>
