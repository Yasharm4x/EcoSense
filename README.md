# 🌿 EcoSense — AI Sustainability Calculator

> Estimate the **electricity**, **water**, and **carbon emissions** of AI model interactions across hardware, regions, and task types.

![EcoSense Screenshot](https://img.shields.io/badge/version-1.0-brightgreen) ![License](https://img.shields.io/badge/license-MIT-green) ![GitHub Pages](https://img.shields.io/badge/deployed-GitHub%20Pages-success)

---

## 🔗 Live Demo

**https://yasharm4x.github.io/EcoSense/**

---

## 📖 What is EcoSense?
 
Every time you ask an AI a question, something happens that nobody talks about. Electricity gets consumed. Water gets used for cooling. Carbon gets emitted. Nobody sends you the bill. Nobody even tells you it happened.
 
EcoSense is a free, open-source, browser-based sustainability calculator that helps users understand the **environmental footprint of AI usage**. As AI becomes more deeply embedded in everyday workflows, understanding its energy and carbon cost becomes increasingly important — and EcoSense makes that cost visible, understandable, and comparable for everyone.
 
No account. No API key. No installation. Open the file in any browser and start immediately.
 
EcoSense supports:
 
- **20+ modern AI models** across OpenAI, Anthropic, Google, Meta, DeepSeek, Alibaba, Mistral, xAI, Microsoft, and Cohere
- **Manual mode** — configure model, tokens, hardware, and region directly
- **Prompt Analysis mode** — paste your prompt and optionally the AI's response for an accurate footprint measurement
- **Post-interaction measurement** — paste the actual AI response to measure its real token count, not just estimate it
- **6 task types** with compute multipliers (Chat, Coding, Creative, Research, Reasoning, Agentic)
- **Multi-dimensional output** — electricity (Wh), water (mL/L), carbon (g/kg CO₂e), and an impact score (0–100)
- **Real-world equivalents** — smartphone charges, LED bulb seconds, minutes of HD video streaming, glasses of water, km of driving
- **Tree-day offset estimates** — with practical behavioural tips to balance your impact
- **Model comparison** — see 7 key models side-by-side for the same configuration
- **JSON export** and **copy to clipboard**
- **Light & Dark mode**
- **Fully responsive** — works on desktop, tablet, and mobile
- **Single self-contained HTML file** — no build step, no dependencies, no framework
---
 
## 🆕 Prompt Analysis Mode — Two-Box Design
 
Prompt Analysis mode has two input boxes, making EcoSense the only public AI sustainability calculator that supports both pre-interaction estimation and post-interaction measurement.
 
**Box 1 — Your prompt** *(required)*
Paste what you typed to the AI. EcoSense counts the words, converts to input tokens using the standard `1 token ≈ 0.75 words` approximation, and shows the count live as you type.
 
**Box 2 — AI response** *(optional)*
Paste the actual response the AI wrote back. If filled, EcoSense measures the output token count directly from the response text — no estimation, no guessing. The output token field updates instantly and is marked as "measured from response." If left empty, EcoSense falls back to a task-type ratio estimate automatically.
 
This unlocks a workflow no other tool offers: finish a conversation with Claude, ChatGPT, or Gemini, copy the response, paste it in, and get the real footprint of that interaction based on what the AI actually produced — not a proxy.
 
> EcoSense is the only public AI sustainability calculator that supports **post-interaction measurement** — paste the AI's actual response to measure its real token count rather than estimating it.
 
---
 
## 🧮 Calculation Methodology
 
EcoSense uses a fully transparent, research-grounded formula chain. Every output number is directly traceable to its inputs. No black-box models, no proprietary algorithms.
 
```
Energy (Wh) = (Total Tokens / 1000)
            × BaseEnergyPer1KTokens  [0.03 Wh]
            × Model Sustainability Factor (MSF)
            × Task Multiplier
            × Hardware Factor
            × PUE  [1.2]
 
Water (mL)  = Energy (Wh) × 1.8
 
CO₂e (g)    = (Energy (Wh) / 1000) × Regional Carbon Intensity (gCO₂/kWh)
```
 
The base energy constant of **0.03 Wh per 1,000 tokens** is derived from Samsi et al. (2023) and Jegham et al. (2025), which measured LLM inference energy in the 0.01–0.08 Wh/1K token range on GPU hardware. A PUE of **1.2** reflects hyperscale data centre efficiency per IEA (2023). Water intensity of **1.8 mL/Wh** is sourced from Li et al. (2023).
 
For the complete research foundation, derivation of all constants, and full academic citations, see **[methodology.md](./methodology.md)**.
 
### Model Sustainability Factor (MSF)
 
MSF is EcoSense's relative compute complexity ranking. It represents how much energy a model consumes per token relative to the lightest model in the list (Phi-4 = 1.0 baseline). A model with MSF 3.8 consumes approximately 3.8× more energy per token than Phi-4.
 
MSF is a calibrated estimate based on publicly available model positioning, architecture type (dense vs mixture-of-experts), and published inference benchmarks. **It is not a provider-reported figure.** No major AI provider publicly discloses per-token energy data, making direct measurement impossible for any external tool.
 
| Model | MSF | Architecture |
|---|---|---|
| Phi-4 | 1.0 | Small dense — baseline |
| Gemma 3 | 1.1 | Small dense |
| Llama 4 Scout | 1.4 | MoE — efficient |
| Qwen3-32B | 1.5 | Mid dense |
| Command A | 1.6 | Optimised enterprise |
| GPT-5.5 Instant | 1.8 | Lightweight frontier |
| DeepSeek V3 | 3.0 | Large MoE |
| Mistral Large 3 | 3.1 | Large dense |
| Gemini 3.5 | 3.2 | Mid frontier |
| Llama 4 Maverick | 3.3 | Large MoE |
| Gemini 3.1 Pro | 3.4 | Pro frontier |
| Claude Sonnet 4 | 3.5 | Mid frontier, optimised |
| Gemini Omni | 3.7 | Multimodal frontier |
| Grok 4 | 3.7 | Frontier reasoning |
| GPT-5.5 | 3.8 | Flagship frontier |
| Qwen3-235B-A22B | 4.7 | Very large MoE |
| Claude Opus 4 | 4.8 | Highest capability tier |
| DeepSeek R1 | 5.0 | Reasoning, chain-of-thought heavy |
 
### Task Multipliers
 
Different AI task types impose meaningfully different compute loads even at the same token count. Jegham et al. (2025) quantified that reasoning models can use 50–100× the energy of standard inference. EcoSense applies multipliers that capture this directional reality at a practical scale.
 
| Task | Multiplier | Rationale |
|---|---|---|
| Chat | ×1.0 | Baseline conversational inference |
| Coding | ×1.2 | Structured output, longer context windows |
| Creative Writing | ×1.3 | Extended generation, higher sampling overhead |
| Research | ×1.5 | Long output, retrieval-augmented patterns |
| Reasoning | ×2.0 | Chain-of-thought, multi-step inference |
| Agentic Workflow | ×3.0 | Multi-turn, tool use, orchestration overhead |
 
### Hardware Factors
 
| Hardware | Factor | Notes |
|---|---|---|
| CPU | 0.8 | Lower power draw per token |
| GPU | 1.0 | Baseline — dominant AI inference hardware |
| TPU v4 | 0.9 | Purpose-built inference hardware, ~10% more efficient |
 
### Regional Carbon Intensity
 
Values drawn from IEA World Energy Outlook and electricityMaps annual averages:
 
| Region | gCO₂/kWh | Grid character |
|---|---|---|
| Western Europe | 300 | High renewable mix |
| US West Coast | 350 | Significant hydro and solar |
| Global Average | 475 | IEA World Energy Outlook 2023 |
| US East Coast | 500 | Higher fossil dependence |
| India | 600 | Coal-dominant grid |
| Asia Pacific | 700 | Coal-heavy markets |
 
---
 
## 🌱 Impact Score
 
EcoSense generates a composite impact score (0–100) based on carbon emissions, calibrated to provide intuitive guidance:
 
| Score | Category | CO₂e threshold |
|---|---|---|
| 80–100 | 🟢 Excellent | < 0.5 g |
| 60–79 | 🟢 Excellent (high end) | 0.5–2 g |
| 50–69 | 🟡 Low Impact | 2–5 g |
| 40–59 | 🟠 Moderate | 5–15 g |
| 20–39 | 🔴 High Impact | 15–40 g |
| 0–19 | ⚫ Very High | > 40 g |
 
---
 
## 🆚 How EcoSense Compares
 
The closest existing tool is EcoLogits by GenAI Impact (France) — a Python library and HuggingFace-hosted calculator. Others include CodeCarbon, CarbonTracker, and Green Algorithms. EcoSense was built after reviewing all of them.
 
| Capability | EcoSense | EcoLogits | CodeCarbon | Green Algorithms |
|---|---|---|---|---|
| Zero setup, browser-based | ✅ Single HTML file | Partial — HuggingFace hosted | ❌ Python only | ✅ Web only |
| Paste any arbitrary prompt | ✅ | ❌ Predefined scenarios | ❌ | ❌ |
| Paste AI response for exact measurement | ✅ | ❌ | ❌ | ❌ |
| Task-type compute multipliers | ✅ 6 types | ❌ | ❌ | ❌ |
| Water consumption | ✅ From day one | Added mid-2025 | ❌ | ❌ |
| Real-world equivalents | ✅ | ❌ | ❌ | ❌ |
| Multi-model side-by-side comparison | ✅ 7 models | ✅ Sequential | ❌ | ❌ |
| Environmental impact score (0–100) | ✅ | ❌ | ❌ | ❌ |
| Offset and sustainability tips | ✅ | ❌ | ❌ | ❌ |
| JSON export | ✅ | ❌ | Partial (CSV) | ❌ |
| Mobile-friendly | ✅ Fully responsive | ❌ | N/A | Partial |
| Light + dark mode | ✅ | ❌ | N/A | ❌ |
| Single file, self-hostable | ✅ | ❌ | ❌ | ❌ |
| 20+ models covered | ✅ | Fewer models | N/A | N/A |
| Open source | ✅ MIT License | ✅ Apache 2.0 | ✅ MIT | ✅ |
 
---
 
## 🚀 Getting Started
 
### Option 1 — GitHub Pages (recommended)
 
1. Fork or clone this repository
2. Rename `ecosense.html` → `index.html`
3. Go to **Settings → Pages → Source → Deploy from branch (main)**
4. Your site will be live at `https://yourusername.github.io/repo-name`
### Option 2 — Run locally
 
No build tools required. Just open the file:
 
```bash
git clone https://github.com/yasharm4x/ecosense.git
cd ecosense
open index.html     # macOS
start index.html    # Windows
xdg-open index.html # Linux
```
 
---
 
## 📁 Project Structure
 
```
ecosense/
├── index.html              # Main application — fully self-contained
├── ecosense-glass.html     # Alternative glass/iOS UI variant
├── README.md               # This file
├── methodology.md          # Full research methodology and citations
├── LICENSE                 # MIT License
├── .gitignore
└── .github/
    ├── CONTRIBUTING.md     # Contribution guidelines
    └── ISSUE_TEMPLATE.md   # Bug and feature request template
```
 
EcoSense is intentionally a **single-file application** — no build step, no dependencies, no framework. Everything (HTML, CSS, JS, and embedded assets) lives in `index.html`. Fork it, change one line, and it works.
 
---
 
## ⚠️ Transparency & Disclaimer
 
> These figures are **research-based estimates** derived from token counts, model complexity rankings, hardware assumptions, regional carbon intensity data, and published sustainability research.
>
> They are **not direct measurements** from OpenAI, Anthropic, Google, Meta, DeepSeek, xAI, Alibaba, Microsoft, Mistral, or Cohere infrastructure. No major AI provider publicly discloses per-query energy data, making direct measurement impossible for any external tool.
>
> Model Sustainability Factors (MSF) are relative compute complexity estimates calibrated from public model positioning, architecture type, and available benchmarks — not provider-reported values.
 
EcoSense is designed to raise awareness and provide order-of-magnitude estimates — not to replace rigorous lifecycle analysis. It is a transparency and awareness tool, not a certified carbon accounting instrument.
 
---
 
## 🤝 Contributing
 
Contributions are welcome. Particularly useful areas:
 
- Adding new models with sourced MSF estimates (please cite your sources)
- Adding regions with verified carbon intensity data from electricityMaps or IEA
- Improving tokenizer accuracy for non-English languages and code
- Adding a comparison chart visualisation
- Adding PWA / offline support
- UI improvements and accessibility enhancements
To contribute:
 
```bash
git checkout -b feature/your-feature-name
# make changes to index.html or ecosense-glass.html
git commit -m "feat: your description"
git push origin feature/your-feature-name
# open a Pull Request
```
 
Please include a brief note on the source or rationale for any changes to calculation values.
 
---
 
## 📚 References & Further Reading
 
1. Strubell, E., Ganesh, A., & McCallum, A. (2019). *Energy and Policy Considerations for Deep Learning in NLP.* ACL 2019. [arXiv:1906.02629](https://arxiv.org/abs/1906.02629)
2. Patterson, D., et al. (2021). *Carbon Emissions and Large Neural Network Training.* [arXiv:2104.10350](https://arxiv.org/abs/2104.10350)
3. Luccioni, A.S., Jernite, Y., & Strubell, E. (2023). *Power Hungry Processing: Watts Driving the Cost of AI Deployment.* [arXiv:2311.16863](https://arxiv.org/abs/2311.16863)
4. Jegham, N., et al. (2025). *How Hungry is AI? Benchmarking Energy, Water, and Carbon Footprint of LLM Inference.* [arXiv:2505.09598](https://arxiv.org/abs/2505.09598)
5. Li, P., et al. (2023). *Making AI Less Thirsty: Uncovering and Addressing the Secret Water Footprint of AI Models.* [arXiv:2304.03271](https://arxiv.org/abs/2304.03271)
6. Samsi, S., et al. (2023). *From Words to Watts: Benchmarking the Energy Costs of Large Language Model Inference.* IEEE HPEC 2023.
7. IEA (2023). *Data Centres and Data Transmission Networks.* [iea.org](https://www.iea.org/energy-system/buildings/data-centres-and-data-transmission-networks)
8. Google Environmental Report — [sustainability.google/reports](https://sustainability.google/reports/)
9. Microsoft Sustainability Report — [microsoft.com/sustainability](https://www.microsoft.com/en-us/sustainability)
---
 
## 📄 License
 
MIT License — free to use, modify, and distribute. See [LICENSE](./LICENSE) for details.
 
---
 
## 🙏 Acknowledgements
 
Built as part of an effort to make AI's environmental impact more transparent and understandable for everyone. The research community doing actual measurement work — particularly the teams behind EcoLogits, CodeCarbon, and the papers referenced above — deserves full credit for the foundational work EcoSense builds on.
 
---
 
<p align="center">Made with 🌿 by <a href="https://github.com/yasharm4x">@yasharm4x</a></p>
