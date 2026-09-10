# 🔄 How AI Companies Get Fresh Training Data

> Category: AI Technology | File: 11 of 15

---

## 📊 The Data Crisis

| Metric | Value |
|---|---|
| Total Human-Written Internet Text | ~4.6 Trillion words |
| Already Used for Training | ~90% consumed |
| Problem | Internet data is RUNNING OUT |
| Solution | Companies finding NEW ways |

---

## 🌐 Source 1 — Public Internet Scraping

### Major Web Crawlers

| Crawler | Owner | Scale |
|---|---|---|
| **Common Crawl** | Non-profit | 250 Billion pages/month |
| **Google Bot** | Google | Trillions of pages |
| **GPTBot** | OpenAI | Billions of pages |
| **ClaudeBot** | Anthropic | Billions of pages |
| **Bytespider** | ByteDance | Billions of pages |
| **PerplexityBot** | Perplexity | Millions/day |

### What Gets Scraped

**✅ Scraped:**
- Wikipedia (all 60 languages)
- Reddit (billions of conversations)
- Project Gutenberg (70,000+ books)
- GitHub (all public code)
- Stack Overflow (Q&A)
- ArXiv (2M+ research papers)
- News websites (CNN, BBC, Reuters)

**❌ Blocked:**
- Twitter/X (blocked all scrapers 2023)
- Reddit (charged $60M/year for API)
- LinkedIn (blocked aggressively)
- Facebook (walled garden)
- Paywalled content (NYT, WSJ)

---

## 💰 Source 2 — Buying Data (Paid Partnerships)

### Publisher Deals

| Deal | Amount |
|---|---|
| OpenAI + Associated Press | $millions/year |
| OpenAI + Axel Springer | $millions |
| OpenAI + Financial Times | Licensing deal |
| **Google + Reddit** | **$60M/year** |
| Apple + Publishers | Undisclosed |
| xAI + Twitter/X | Elon owns it (free advantage!) |

### Book Deals

- OpenAI + Penguin Random House (negotiations)
- Google Books → 40M books scanned (lawsuits ongoing)
- Meta + publishers → legal disputes

---

## 🤖 Source 3 — Synthetic Data Generation
### AI Training AI (BIGGEST TREND 2025–2026!)

> ### 🔄 The Synthetic Data Flywheel
> 1. **Step 1 (Teacher):** Employ an existing state-of-the-art model as an automated data synthesizer.
> 2. **Step 2 (Generation):** Synthesize millions of complex edge cases, math proofs, and code snippets.
> 3. **Step 3 (Automated Quality Filter):** Run automated verification and unit tests to retain the top 10% highest-quality outputs.
> 4. **Step 4 (Student Training):** Train the next-generation foundation model on this curated synthetic corpus.
> 5. **Step 5 (Self-Improvement):** The superior student model generates higher-tier data.
> 6. **Step 6 (Continuous Loop):** Compound intelligence gains across successive training cycles.

### Real Examples

| Company | What They Create |
|---|---|
| **OpenAI** | GPT-4 generates data for GPT-5 |
| **Anthropic** | Constitutional AI self-critique |
| **Google** | Gemini generates Gemini training data |
| **Meta** | Llama 3 trained on Llama 2 output |
| **DeepSeek** | Self-generated math/code proofs |

### Types of Synthetic Data

| Type | Example |
|---|---|
| Math & Reasoning | Millions of math problems + solutions |
| Code Synthesis | Code in all languages + unit tests |
| Conversations | User-AI dialogues, customer service |
| Multilingual | Content in underrepresented languages |
| Medical | Synthetic patient records |

---

## 👥 Source 4 — Human Feedback (RLHF)

**What it is:** Real humans rating AI outputs to teach AI

> ### 🎯 The RLHF Optimization Loop
> - **Expert Human Annotators** ➔ Score AI candidate completions on factual accuracy, tone, and safety (1–5 scale).
> - **Preference Modeling** ➔ Label preferred completions (A > B) to build a Reward Model.
> - **Policy Optimization (PPO / DPO)** ➔ Fine-tune the base LLM to maximize reward scores aligned with human judgment.

### Scale

| Company | Workers |
|---|---|
| **Scale AI** | 500,000+ labelers worldwide |
| **Remotasks** | 240,000+ (Kenya, Philippines) |
| **Appen** | 1M+ freelance contributors |
| **Surge AI** | 10,000+ USA workers |
| **iMerit** | 5,000+ India workers |

> OpenAI spent $100M+ on human feedback alone

---

## 📱 Source 5 — User Interaction Data

### Your Conversations Train AI!

| Platform | Data Collected | Users |
|---|---|---|
| ChatGPT | Conversations (opt-out available) | 1B+ |
| Google Gemini | Search, Gmail, Drive (with permission) | Billions |
| Meta (FB/IG) | Posts, images, comments, reactions | 3.2B |
| Microsoft | LinkedIn posts, GitHub code, Bing queries | 1B+ |
| Apple | Siri queries (on-device, anonymized) | 1B+ |
| TikTok | Videos watched, comments, captions | 1.5B |

---

## 📚 Source 6 — Books & Academic Papers

| Source | Count | Cost |
|---|---|---|
| Project Gutenberg | 70,000+ books | FREE |
| Google Books | 40M books | $125M settlement |
| Books3 Dataset | 196,640 books | Scraped illegally |
| LibGen | 4M+ books | Pirated (lawsuits!) |
| ArXiv | 2M+ papers | FREE |
| PubMed | Medical research | FREE |

### ⚖️ Major Lawsuits

- NY Times vs OpenAI ($billions claim)
- Authors Guild vs OpenAI
- Sarah Silverman vs Meta AI
- George R.R. Martin vs OpenAI
- Getty Images vs Stability AI

---

## 💻 Source 7 — Code Data

| Source | Scale | Owner |
|---|---|---|
| GitHub public repos | 500M+ repositories | Microsoft |
| Stack Overflow | 50M+ Q&A pairs | Community |
| LeetCode solutions | Millions | Community |
| PyPI packages | Millions | Community |
| npm packages | Millions | Community |

> GitHub Copilot knows ALL public code ever written

---

## 🌍 Source 8 — Multimodal Data

### Images
- LAION-5B: 5 Billion image-text pairs
- ImageNet: 14M labeled images
- Adobe Stock → Firefly training
- Shutterstock AI partnerships

### Videos
- YouTube (Google owns = massive advantage)
  - 800M videos, 500 hours uploaded/minute

### Audio
- Common Voice (Mozilla): 30,000+ hours
- Podcasts (with permission)
- YouTube audio tracks

---

## 🏭 Source 9 — Proprietary Enterprise Data

| Company | Exclusive Data Advantage |
|---|---|
| **Google** | 8.5 Billion search queries/DAY |
| **Meta** | 20 years of human social posts (3B users) |
| **Amazon** | 1 Billion+ product reviews + Alexa voice |
| **Tesla** | 6 Billion miles of driving camera footage |
| **Apple** | 1B devices, most private (on-device only) |

---

## 🚗 Source 10 — Real World Sensors

| Domain | Company | Data |
|---|---|---|
| Driving | Tesla | 6B miles, 5M cars |
| Driving | Waymo | 20M miles (detailed) |
| Robots | Figure AI | Household task data |
| Health | Apple Watch | 100M+ heart readings |
| Medical | Hospitals | Vitals, scans |
| Weather | Satellites | Climate data |

---

## 📊 Data Source Timeline

| Phase | Period | Method |
|---|---|---|
| Phase 1 | 2018–2023 | Scrape everything on internet ✅ Done |
| Phase 2 | 2023–2026 | Buy exclusive deals + synthetic data 🔥 Now |
| Phase 3 | 2026+ | Real-world sensors, robots, AI trains AI 🚀 Starting |

---

## 🏆 Data Advantage by Company

| Company | Secret Advantage |
|---|---|
| **Google** | 8.5B daily search queries (30 years!) |
| **Meta** | 20 years of human social behavior |
| **Tesla** | 6B miles real driving data |
| **Microsoft** | ALL public code ever written (GitHub) |
| **Amazon** | Billions of purchase behaviors + Alexa |
| **DeepSeek** | Chinese language + cheap synthetic data |

---

> 📌 **Key Insight:** The company with the most UNIQUE, HIGH-QUALITY data wins. Google (search+YouTube) and Meta (social data) have structural advantages startups can NEVER match.

---
*← [10_Who_Is_Winning_AI_Race.md](10_Who_Is_Winning_AI_Race.md) | Next → [12_Why_AI_Training_Is_Costly.md](12_Why_AI_Training_Is_Costly.md)*
