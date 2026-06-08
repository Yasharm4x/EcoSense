# 🌿 EcoSense — Methodology & Research Foundation

> **Version 2.0 · June 2026 · [github.com/yasharm4x](https://github.com/yasharm4x)**

EcoSense is a single-file, zero-dependency web application that estimates the environmental impact of AI model interactions. This document describes the scientific foundation, calculation methodology, public claims framework, and competitive differentiation behind every number EcoSense produces.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Calculation Methodology](#2-calculation-methodology)
3. [Prompt Analysis Mode](#3-prompt-analysis-mode)
4. [Environmental Impact Score](#4-environmental-impact-score)
5. [Public Claims Framework](#5-public-claims-framework)
6. [Competitive Landscape](#6-competitive-landscape)
7. [Why EcoSense Stands Apart](#7-why-ecosense-stands-apart)
8. [Known Limitations](#8-known-limitations)
9. [References](#9-references)

---

## 1. Overview

EcoSense quantifies electricity consumption, water usage, carbon emissions (CO₂e), and produces a composite impact score — all derived from user-provided inputs: model choice, token count, hardware type, geographic region, and task type.

The tool is designed for accessibility: no API key, no account, no installation. Any user can open the HTML file in a browser and immediately estimate the environmental cost of their AI usage.

---

## 2. Calculation Methodology

EcoSense uses a **fully transparent, documented formula chain**. Every output number is directly traceable to its inputs. No black-box models or proprietary algorithms are involved.

### 2.1 Primary Energy Estimation Formula

```
Energy (Wh) = (Total Tokens / 1000)
            × BaseEnergyPer1KTokens  [0.03 Wh]
            × Model Sustainability Factor (MSF)
            × Task Multiplier
            × Hardware Factor
            × PUE  [1.2]
```

This formula is structurally consistent with the energy estimation approach described in Patterson et al. (2021) and Luccioni et al. (2023), which model inference energy as a function of compute complexity, hardware efficiency, and infrastructure overhead.

### 2.2 Base Energy Constant — `0.03 Wh / 1K tokens`

The base energy constant of **0.03 Wh per 1,000 tokens** is derived from inference benchmarking literature:

- Samsi et al. (2023) benchmarked LLaMA-class models and found inference energy in the range of **0.01–0.08 Wh per 1K tokens** depending on hardware
- Jegham et al. (2025) *"How Hungry is AI?"* provides empirical benchmarks across multiple model families confirming this range
- `0.03 Wh` is used as a conservative mid-point for GPU inference on current-generation hardware

### 2.3 Power Usage Effectiveness (PUE) — `1.2`

A PUE multiplier of **1.2** is applied to all energy estimates, representing the overhead of cooling, power distribution, and infrastructure in a modern data centre. The IEA (2023) reports a global average PUE of 1.58 for all data centres; 1.2 is appropriate for hyperscale facilities used by major AI providers.

### 2.4 Model Sustainability Factor (MSF)

The MSF is EcoSense's **relative compute complexity ranking**. It is not a provider-reported measurement. It is a calibrated estimate based on:

- Publicly known parameter counts for open models
- Reported FLOPs and training compute estimates from provider documentation and research papers
- Relative inference cost benchmarks from Jegham et al. (2025) and Patterson et al. (2021)
- Architecture type — mixture-of-experts models receive lower MSF than dense models of equivalent nominal size

| Model | MSF | Notes |
|---|---|---|
| Phi-4 | 1.0 | Baseline — small dense model |
| Gemma 3 | 1.1 | Efficient small model |
| Llama 4 Scout | 1.4 | MoE architecture, efficient |
| Qwen3-32B | 1.5 | Mid-size dense |
| Command A | 1.6 | Optimised for enterprise tasks |
| GPT-5.5 Instant | 1.8 | Lightweight frontier model |
| DeepSeek V3 | 3.0 | Large MoE model |
| Mistral Large 3 | 3.1 | Large dense frontier |
| Gemini 3.5 | 3.2 | Mid frontier |
| Llama 4 Maverick | 3.3 | Large MoE |
| Gemini 3.1 Pro | 3.4 | Pro tier frontier |
| Claude Sonnet 4 | 3.5 | Mid frontier, optimised |
| Gemini Omni | 3.7 | Multimodal frontier |
| Grok 4 | 3.7 | Frontier reasoning model |
| GPT-5.5 | 3.8 | Flagship frontier |
| Qwen3-235B-A22B | 4.7 | Very large MoE |
| Claude Opus 4 | 4.8 | Highest capability tier |
| DeepSeek R1 | 5.0 | Reasoning model, chain-of-thought heavy |

### 2.5 Task Type Multipliers

Different AI task types impose meaningfully different compute loads even at the same token count. Strubell et al. (2019) established that complex NLP tasks require substantially more compute. Jegham et al. (2025) specifically quantifies that reasoning models *"can use 50 to 100 times the energy of standard inference"* due to internal chain-of-thought generation.

| Task Type | Multiplier | Rationale |
|---|---|---|
| Chat | ×1.0 | Baseline conversational inference |
| Coding | ×1.2 | Structured output, longer context |
| Creative Writing | ×1.3 | Extended generation, sampling overhead |
| Research | ×1.5 | Long output, retrieval-augmented patterns |
| Reasoning | ×2.0 | Chain-of-thought, multi-step inference |
| Agentic Workflow | ×3.0 | Multi-turn, tool use, orchestration overhead |

### 2.6 Hardware Factors

| Hardware | Factor | Basis |
|---|---|---|
| GPU (A100/H100 class) | 1.0 | Baseline — dominant AI inference hardware |
| TPU v4 | 0.9 | Google's purpose-built inference hardware, ~10% more efficient |
| CPU | 0.8 | Lower power draw per token |

### 2.7 Water Consumption

```
Water (mL) = Energy (Wh) × 1.8 mL/Wh
```

The **1.8 mL/Wh** water intensity factor is sourced from Li et al. (2023) *"Making AI Less Thirsty"*, which measured direct water consumption in data centre cooling systems. This is also consistent with the methodology adopted by EcoLogits in their 2025 water consumption update.

### 2.8 Carbon Emissions

```
CO₂e (g) = (Energy (Wh) / 1000) × Regional Carbon Intensity (gCO₂/kWh)
```

Regional carbon intensity values are drawn from IEA World Energy Outlook and electricityMaps annual averages:

| Region | gCO₂/kWh | Source basis |
|---|---|---|
| Western Europe | 300 | IEA Europe average, high renewable mix |
| US West Coast | 350 | IEA/EIA US West, significant hydro/solar |
| Global Average | 475 | IEA World Energy Outlook 2023 |
| US East Coast | 500 | IEA/EIA US East, higher fossil dependence |
| India | 600 | IEA India, coal-dominant grid |
| Asia Pacific | 700 | IEA Asia Pacific average, coal-heavy markets |

---

## 3. Prompt Analysis Mode

EcoSense's Prompt Analysis Mode allows users to paste any arbitrary text prompt and receive an automatic token estimate and environmental footprint calculation.

### 3.1 Token Estimation from Words

```
Input Tokens ≈ Word Count / 0.75
```

The **0.75 ratio** (1 token ≈ 0.75 words, or ~4 characters) is the standard approximation from OpenAI's tokenization documentation, widely cited in NLP literature, and accurate to within ±15% for English-language text.

### 3.2 Output Token Auto-Fill with Task Ratio

EcoSense automatically estimates expected output length based on the selected task type and input length. Users can override at any time.

| Task Type | Output/Input Ratio | Reasoning |
|---|---|---|
| Chat | 0.8× | Responses typically shorter than prompts |
| Coding | 1.5× | Generated code often longer than the request |
| Creative Writing | 2.0× | Extended prose from brief prompts |
| Research | 2.5× | Comprehensive answers to research queries |
| Reasoning | 1.2× | Structured reasoning, moderate length |
| Agentic Workflow | 3.0× | Multi-step plans, tool call schemas, logs |

---

## 4. Environmental Impact Score

EcoSense generates a composite impact score (0–100) based on carbon emissions:

| Score | Category | CO₂e Threshold |
|---|---|---|
| 80–100 | 🟢 Excellent | < 0.5 g CO₂e |
| 60–79 | 🟢 Excellent (high end) | 0.5–2 g CO₂e |
| 50–69 | 🟡 Low Impact | 2–5 g CO₂e |
| 40–59 | 🟠 Moderate | 5–15 g CO₂e |
| 20–39 | 🔴 High Impact | 15–40 g CO₂e |
| 0–19 | ⚫ Very High | > 40 g CO₂e |

---

## 5. Public Claims Framework

### ✅ What We Can Claim

**On transparency:**
- Every number EcoSense produces is directly traceable to its inputs — the methodology is visible in the app, not a black box
- The base energy constant, hardware factors, PUE, and water intensity are all anchored in peer-reviewed published literature
- Relative comparisons between models, regions, and task types are directionally valid and grounded in the same published research used by academic tools

**On functionality:**
- EcoSense estimates environmental impact at the individual prompt level — the most granular unit of AI compute
- One of very few public tools to report electricity, water, and carbon together from day one
- Task-type multipliers reflect the real difference in compute cost between a chat message and an agentic workflow

**On accessibility:**
- No API key, no account, no installation — works in any browser as a single HTML file
- Full source code available under MIT License — methodology is publicly auditable

### ❌ What We Cannot Claim

- ~~These are accurate measurements of a specific model's actual energy use~~
- ~~Our numbers match what OpenAI / Anthropic / Google / Meta infrastructure actually consumes~~
- ~~EcoSense is scientifically validated to a specific accuracy percentage~~
- ~~The MSF values are provider-reported or independently verified~~

### 📋 Recommended Disclaimer Language

> *"EcoSense estimates are research-based approximations derived from token counts, model complexity rankings, hardware efficiency assumptions, regional carbon intensity data (IEA), and published sustainability research (Strubell 2019, Patterson 2021, Jegham 2025, Li 2023). They represent order-of-magnitude estimates, not direct infrastructure measurements. No major AI provider publicly discloses per-query energy data, making direct measurement impossible for any external tool. EcoSense is a transparency and awareness instrument, not a certified carbon accounting instrument."*

---

## 6. Competitive Landscape

### Existing Tools

| Tool | Type | Key Limitation vs EcoSense |
|---|---|---|
| **EcoLogits** (GenAI Impact) | Python library + HuggingFace web app | Requires HuggingFace to host; predefined scenarios only; added water mid-2025 |
| **CodeCarbon** | Python library | Training-focused; no web UI; no LLM-specific task types |
| **CarbonTracker** | Python library | Requires code instrumentation; no consumer interface |
| **Eco2AI** | Python library | Developer tool; no prompt analysis |
| **LLMCO2** | Academic research tool | Not a public product |
| **Green Algorithms** | Web calculator | General compute, not LLM-specific; no token-level estimation |

### Capability Comparison

| Capability | EcoSense | EcoLogits Calculator | CodeCarbon | Green Algorithms |
|---|---|---|---|---|
| Zero setup, browser-based | ✅ Single HTML file | Partial (HuggingFace) | ❌ Python only | ✅ Web only |
| Arbitrary prompt analysis | ✅ Paste any prompt | ❌ Predefined scenarios | ❌ No | ❌ No |
| Task-type multipliers | ✅ 6 task types | ❌ No | ❌ No | ❌ No |
| Water consumption | ✅ From day one | Added mid-2025 | ❌ No | ❌ No |
| Electricity consumption | ✅ | ✅ | ✅ | ✅ |
| Carbon emissions | ✅ | ✅ | ✅ | ✅ |
| Impact score (0–100) | ✅ | ❌ | ❌ | ❌ |
| Real-world equivalents | ✅ | ❌ | ❌ | ❌ |
| Multi-model comparison | ✅ 7 side-by-side | ✅ Sequential | ❌ | ❌ |
| Offset / tree recommendations | ✅ | ❌ | ❌ | ❌ |
| JSON export | ✅ | ❌ | Partial (CSV) | ❌ |
| Open source | ✅ MIT | ✅ Apache 2.0 | ✅ MIT | ✅ |
| No account required | ✅ | ✅ | ✅ | ✅ |
| Mobile-friendly | ✅ Responsive | ❌ | N/A | Partial |
| Light + dark mode | ✅ | ❌ | N/A | ❌ |
| 20+ models covered | ✅ | Fewer models | N/A | N/A |

---

## 7. Why EcoSense Stands Apart

> **EcoSense is the only zero-setup, browser-based AI sustainability calculator that combines arbitrary prompt analysis, task-type compute multipliers, water consumption, multi-model comparison, and real-world equivalents — all in a single open-source file that works without any account, API key, or installation.**

Here are the eight specific differentiators:

**1. Prompt Analysis Mode with word-to-token estimation**
No other public consumer-facing tool allows pasting an arbitrary prompt and automatically deriving a token count and footprint estimate. EcoLogits Calculator uses predefined scenarios; CodeCarbon requires API instrumentation.

**2. Task-type compute multipliers for end users**
Research confirms that reasoning tasks consume 50–100× more energy than chat (Jegham et al., 2025). EcoSense is the only public calculator that surfaces this at the UI level, making it actionable for everyday users.

**3. Water as a first-class metric from day one**
EcoSense ships with water consumption as a core output. EcoLogits added water only in mid-2025, more than a year after launch. CodeCarbon and Green Algorithms do not include it at all.

**4. Real-world equivalents layer**
Translating carbon grams into smartphone charges, LED bulb seconds, glasses of water, and km of driving is entirely absent from all competing tools. This makes abstract numbers tangible for non-technical users.

**5. Self-contained single file for GitHub Pages**
EcoSense requires no backend, no database, no server, no build system. The entire application lives in one HTML file anyone can fork, deploy, and audit. No existing AI sustainability tool uses this distribution model.

**6. Multi-model side-by-side comparison**
Seeing the relative carbon cost of GPT-5.5 versus Phi-4 versus DeepSeek R1 for the same prompt, instantly, in one view is not available in any other public consumer tool today.

**7. Offset and behavioural recommendations**
The tree-days offset calculation and practical sustainability tips translate the abstract into concrete personal action. This educational layer is absent from all technical tools in the space.

**8. Designed for the general public, not developers**
Every other tool in this space — EcoLogits Python library, CodeCarbon, CarbonTracker, Eco2AI — requires coding knowledge. EcoSense's entire design philosophy is the opposite: open a file, use it immediately.

---

## 8. Known Limitations

Honest disclosure of limitations is part of EcoSense's transparency commitment.

- **No provider-side data** — No major AI provider publicly discloses per-query energy consumption. All estimates, including those of EcoLogits, LLMCO2, and EcoSense, are approximations. This is a structural limitation of the entire field.
- **MSF values are not empirically validated** — The MSF ranking is calibrated from published parameter counts, architecture types, and relative benchmark data. It has not been independently verified against actual infrastructure measurements.
- **Static regional intensity** — Carbon intensity varies by time of day and season. EcoSense uses annual average values. Real-time grid intensity (electricityMaps API) would improve accuracy but requires a backend.
- **Tokenizer approximation** — The 1 token ≈ 0.75 words rule is accurate for English prose but less accurate for code, non-Latin scripts, and highly technical content. Actual tokenizers (BPE, SentencePiece) vary by model.
- **Training emissions excluded** — EcoSense estimates inference-only emissions. Training emissions are outside scope.
- **Embodied carbon excluded** — Hardware manufacturing emissions are not included. EcoLogits includes these via life-cycle assessment methodology; EcoSense does not currently.

---

## 9. References

1. Strubell, E., Ganesh, A., & McCallum, A. (2019). *Energy and Policy Considerations for Deep Learning in NLP.* ACL 2019. [arXiv:1906.02629](https://arxiv.org/abs/1906.02629)

2. Patterson, D., et al. (2021). *Carbon Emissions and Large Neural Network Training.* [arXiv:2104.10350](https://arxiv.org/abs/2104.10350)

3. Luccioni, A.S., Viguier, S., & Ligozat, A.L. (2023). *Estimating the Carbon Footprint of BLOOM, a 176B Parameter Language Model.* JMLR 24(253). [arXiv:2211.02001](https://arxiv.org/abs/2211.02001)

4. Luccioni, A.S., Jernite, Y., & Strubell, E. (2023). *Power Hungry Processing: Watts Driving the Cost of AI Deployment?* FAccT 2024. [arXiv:2311.16863](https://arxiv.org/abs/2311.16863)

5. Jegham, N., et al. (2025). *How Hungry is AI? Benchmarking Energy, Water, and Carbon Footprint of LLM Inference.* [arXiv:2505.09598](https://arxiv.org/abs/2505.09598)

6. Li, P., et al. (2023). *Making AI Less Thirsty: Uncovering and Addressing the Secret Water Footprint of AI Models.* [arXiv:2304.03271](https://arxiv.org/abs/2304.03271)

7. Samsi, S., et al. (2023). *From Words to Watts: Benchmarking the Energy Costs of Large Language Model Inference.* IEEE HPEC 2023.

8. IEA (2023). *Data Centres and Data Transmission Networks.* International Energy Agency. [iea.org](https://www.iea.org/energy-system/buildings/data-centres-and-data-transmission-networks)

9. GenAI Impact (2024–2025). *EcoLogits: Environmental Footprint of Generative AI.* [ecologits.ai](https://ecologits.ai)

10. Schmidt, V., et al. (2021). *CodeCarbon: Estimate and Track Carbon Emissions from Machine Learning Computing.* [codecarbon.io](https://codecarbon.io)

---

## Conclusion

EcoSense occupies a genuinely distinct position in the AI sustainability tool landscape. Its combination of zero-setup accessibility, prompt-level analysis, task-type awareness, multi-dimensional environmental metrics, and plain-language equivalents makes it the most approachable and feature-complete public AI carbon calculator available as of June 2026.

The tool makes no claim to scientific precision that the field cannot currently support. Instead, it offers transparent, research-grounded, consistently comparable estimates — in a format any person with a browser can access immediately.

> *"EcoSense doesn't claim to measure AI's footprint precisely. It claims to make it visible, understandable, and comparable — for everyone."*

---

<p align="center">🌿 <strong>EcoSense</strong> · Built for a Greener AI future · MIT License</p>
<p align="center"><a href="https://github.com/yasharm4x">github.com/yasharm4x</a> · June 2026</p>
