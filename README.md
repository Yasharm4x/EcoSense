# 🌿 EcoSense — AI Sustainability Calculator

> Estimate the **electricity**, **water**, and **carbon emissions** of AI model interactions across hardware, regions, and task types.

![EcoSense Screenshot](https://img.shields.io/badge/version-2.0-brightgreen) ![License](https://img.shields.io/badge/license-MIT-green) ![GitHub Pages](https://img.shields.io/badge/deployed-GitHub%20Pages-success)

---

## 🔗 Live Demo

👉 **[ecosense.yourgithubusername.github.io](https://yourgithubusername.github.io/ecosense)**

---

## 📖 What is EcoSense?

EcoSense is a free, open-source, browser-based sustainability calculator that helps users understand the **environmental footprint of AI usage**. As AI becomes more deeply embedded in everyday workflows, understanding its energy and carbon cost becomes increasingly important.

EcoSense supports:

- **20+ modern AI models** across OpenAI, Anthropic, Google, Meta, DeepSeek, Alibaba, Mistral, xAI, Microsoft, and Cohere
- **Manual mode** — configure model, tokens, hardware, region directly
- **Prompt Analysis mode** — paste any prompt and get an automatic footprint estimate
- **6 task types** with compute multipliers (Chat, Coding, Creative, Research, Reasoning, Agentic)
- **Multi-dimensional output** — electricity (Wh), water (mL/L), carbon (g/kg CO₂e), and an impact score
- **Real-world equivalents** — smartphone charges, LED bulb seconds, km of driving, and more
- **Model comparison** — see 7 key models side-by-side for the same configuration
- **JSON export** and **copy to clipboard**
- **Light & Dark mode**
- **Fully responsive** — works on desktop, tablet, and mobile

---

## 🧮 Calculation Methodology

EcoSense uses a research-based estimation pipeline:

```
Energy (Wh) = (Total Tokens / 1000)
            × BaseEnergyPer1KTokens (0.03 Wh)
            × Model Sustainability Factor (MSF)
            × Task Multiplier
            × Hardware Factor
            × PUE (1.2)

Water (mL)  = Energy (Wh) × 1.8

CO₂e (g)    = (Energy (Wh) / 1000) × Regional Carbon Intensity (gCO₂/kWh)
```

### Model Sustainability Factor (MSF)

MSF is a relative compute complexity estimate — not a provider-reported measurement.

| Model | MSF |
|---|---|
| Phi-4 | 1.0 |
| Gemma 3 | 1.1 |
| Llama 4 Scout | 1.4 |
| Qwen3-32B | 1.5 |
| Command A | 1.6 |
| GPT-5.5 Instant | 1.8 |
| DeepSeek V3 | 3.0 |
| Mistral Large 3 | 3.1 |
| Gemini 3.5 | 3.2 |
| Llama 4 Maverick | 3.3 |
| Gemini 3.1 Pro | 3.4 |
| Claude Sonnet 4 | 3.5 |
| Gemini Omni | 3.7 |
| Grok 4 | 3.7 |
| GPT-5.5 | 3.8 |
| Qwen3-235B-A22B | 4.7 |
| Claude Opus 4 | 4.8 |
| DeepSeek R1 | 5.0 |

### Task Multipliers

| Task | Multiplier |
|---|---|
| Chat | ×1.0 |
| Coding | ×1.2 |
| Creative Writing | ×1.3 |
| Research | ×1.5 |
| Reasoning | ×2.0 |
| Agentic Workflow | ×3.0 |

### Hardware Factors

| Hardware | Factor |
|---|---|
| CPU | 0.8 |
| GPU | 1.0 |
| TPU v4 | 0.9 |

### Regional Carbon Intensity

| Region | gCO₂/kWh |
|---|---|
| Western Europe | 300 |
| US West Coast | 350 |
| Global Average | 475 |
| US East Coast | 500 |
| India | 600 |
| Asia Pacific | 700 |

---

## 🌱 Impact Score

| Score | Category |
|---|---|
| 80–100 | 🟢 Excellent |
| 60–79 | 🟡 Low Impact |
| 40–59 | 🟠 Moderate |
| 20–39 | 🔴 High Impact |
| 0–19 | ⚫ Very High |

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
git clone https://github.com/yourusername/ecosense.git
cd ecosense
open index.html   # macOS
# or
start index.html  # Windows
```

---

## 📁 Project Structure

```
ecosense/
├── index.html        # Main application (fully self-contained)
├── README.md         # This file
├── LICENSE           # MIT License
└── .github/
    └── FUNDING.yml   # Optional: sponsor links
```

EcoSense is intentionally a **single-file application** — no build step, no dependencies, no framework. Everything (HTML, CSS, JS, embedded assets) lives in `index.html`.

---

## ⚠️ Transparency & Disclaimer

> These figures are **research-based estimates** derived from token counts, model complexity, hardware assumptions, utilization estimates, regional carbon intensity, and published sustainability research.
>
> They are **not direct measurements** from OpenAI, Anthropic, Google, Meta, DeepSeek, xAI, Alibaba, Microsoft, Mistral, or Cohere infrastructure.
>
> Model Sustainability Factors (MSF) are relative compute complexity estimates, not provider-reported values.

EcoSense is designed to raise awareness and provide order-of-magnitude estimates — not to replace rigorous lifecycle analysis.

---

## 🤝 Contributing

Contributions are welcome! Ideas for improvement:

- Add more models and update MSF values as new research emerges
- Add more regions with accurate carbon intensity data
- Improve the tokenizer estimation logic
- Add a comparison chart visualization
- Add PWA support for offline use

To contribute:

```bash
git checkout -b feature/your-feature-name
# make changes to index.html
git commit -m "feat: your description"
git push origin feature/your-feature-name
# open a Pull Request
```

---

## 📚 References & Further Reading

- [Google Environmental Report](https://sustainability.google/reports/)
- [Microsoft Sustainability Report](https://www.microsoft.com/en-us/sustainability)
- [Strubell et al. — Energy and Policy Considerations for Deep Learning in NLP (2019)](https://arxiv.org/abs/1906.02629)
- [Patterson et al. — Carbon Emissions and Large Neural Network Training (2021)](https://arxiv.org/abs/2104.10350)
- [IEA — Data Centres and Data Transmission Networks](https://www.iea.org/energy-system/buildings/data-centres-and-data-transmission-networks)

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

## 🙏 Acknowledgements

Built with ❤️ as part of an effort to make AI's environmental impact more transparent and understandable for everyone.

---

<p align="center">Made with 🌿 by <a href="https://github.com/yourusername">@yourusername</a></p>
