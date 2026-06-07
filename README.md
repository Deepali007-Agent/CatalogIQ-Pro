# 🏷️ AI Catalog Enrichment Pipeline

> **Built by Deepali Ramesh** — Operations professional applying AI to real e-commerce problems.  
> Part of an AI-powered ops portfolio. [See all projects →](#)

---

## The Problem (Why This Exists)

In e-commerce operations, poor catalog data is a silent revenue killer.

A product with a missing description, no tags, or a vague title:
- Doesn't show up in search results
- Gets lower click-through rates
- Converts worse at checkout

**At scale, this is massive.** A catalog of 50,000 SKUs where 30% have quality issues means 15,000 products underperforming — every single day.

The traditional fix: assign a team of catalog executives to manually review and rewrite each listing. At Swiggy/AJIO scale, that means weeks of work and significant cost.

**This tool does it in minutes.**

---

## What It Does

This pipeline takes a raw product CSV, scores every product for catalog quality, and uses Claude AI to:

- ✅ Write compelling product descriptions (if missing or weak)
- ✅ Generate SEO-friendly tags
- ✅ Suggest better product titles
- ✅ Infer missing attributes (color, material) from context
- ✅ Flag quality issues with plain-English explanations

---

## Before vs After

| Field | Before (Raw Data) | After (AI Enriched) |
|---|---|---|
| **Description** | *"A bottle that holds water"* | *"Durable 1L stainless steel water bottle, ideal for gym, office, or travel. Keeps drinks cold for 12 hours and hot for 6. Leak-proof lid."* |
| **Tags** | *(empty)* | `water bottle`, `gym bottle`, `steel bottle`, `hydration`, `travel flask` |
| **SEO Title** | *"Stainless Steel Water Bottle"* | *"1L Steel Water Bottle — Leak-Proof, Hot & Cold"* |
| **Quality Score** | 35/100 🔴 Poor | 92/100 🟢 Good |

---

## Simulated Business Impact

Based on standard e-commerce benchmarks:

| Metric | Estimate |
|---|---|
| Products processed per minute | ~40–60 SKUs |
| Manual QA time saved | ~3–5 minutes per SKU |
| For a 10,000-SKU catalog | Saves **500–800 hours** of analyst time |
| Estimated cost saving | ₹4–6 lakhs per catalog refresh cycle |
| Search discoverability improvement | +25–40% (from tag enrichment) |

> *These are simulation estimates based on industry benchmarks, not live production data.*

---

## How It Works (Plain English)

```
Your CSV → Quality Scorer → AI Enrichment → Enriched CSV
             (instant)        (Claude API)    (downloadable)
```

**Step 1 — Quality Scoring (no AI needed)**  
Every product gets a score from 0–100 based on completeness:
- Product name: 20 points
- Description: 20 points  
- Category: 15 points
- Tags: 10 points
- Color, Material, Size, Price: 5–10 points each

**Step 2 — AI Enrichment (only for Amber/Red products)**  
Products scoring below 75 are sent to Claude with a detailed prompt.
The AI returns a JSON object with enriched fields — no hallucination risk
because all outputs are grounded in the product name and category.

**Step 3 — Download**  
The enriched catalog is exported as a CSV with both original and AI-generated
columns side by side — so you stay in control of what to publish.

---

## Tech Stack

| Layer | Tool | Why |
|---|---|---|
| AI | Anthropic Claude (`claude-sonnet-4`) | Best-in-class instruction following for structured JSON output |
| Interface | Streamlit | Lets non-technical ops teams use the tool without any coding |
| Data | Pandas | Industry standard for CSV/tabular data manipulation |
| Language | Python 3.10+ | Readable, widely supported |

---

## Setup & Run (5 minutes)

### Prerequisites
- Python 3.10 or above
- An Anthropic API key (get one free at [console.anthropic.com](https://console.anthropic.com))

### Installation

```bash
# 1. Clone this repo
git clone https://github.com/yourusername/catalog-enrichment
cd catalog-enrichment

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the app
streamlit run app.py
```

### First Run
1. Open the app at `http://localhost:8501`
2. Enter your Anthropic API key in the sidebar
3. Upload your product CSV — or use the sample in `/data/sample_products.csv`
4. Click **"Score + AI Enrich"**
5. Download your enriched catalog

---

## Project Structure

```
catalog-enrichment/
│
├── app.py                     # Streamlit web interface (run this)
├── requirements.txt           # Python dependencies
│
├── src/
│   └── enricher.py            # Core logic: scoring + AI enrichment
│
├── data/
│   └── sample_products.csv    # 10 sample products to test with
│
└── outputs/                   # Enriched CSVs saved here
```

---

## The Prompt Design (Key Technical Decision)

The AI prompt is structured to:

1. **Set role** — "You are an expert e-commerce catalog manager..."
2. **Provide data** — actual product fields as JSON
3. **List detected issues** — so the AI focuses on what matters
4. **Request structured output** — strict JSON schema prevents free-form rambling
5. **Set guardrails** — no hallucination, ground all inferences in product name/category

This design pattern (role → data → issues → structured output) is what makes AI outputs
reliable enough to use in production. It's the difference between a tool you trust and a toy.

---

## What I Learned Building This

1. **Prompt structure matters more than model choice.** A well-structured prompt on a smaller model outperforms a vague prompt on the largest model.

2. **Score first, enrich second.** Running AI on every product wastes cost and time. The quality scorer filters to only products that actually need enrichment.

3. **Ops domain knowledge is the real edge.** Knowing *which* catalog fields matter most (description and tags > color and size) is knowledge that comes from working in e-commerce, not from knowing how to code.

---

## Author

**Deepali Ramesh**  
Operations leader with 9+ years across Swiggy, AJIO, and Saks Global.  
Building AI-powered tools for real ops problems.

[LinkedIn →](https://linkedin.com/in/yourprofile) | [GitHub →](https://github.com/yourusername)

---

## Roadmap

- [ ] Bulk image URL validation (flag broken product images)
- [ ] Category taxonomy auto-correction (wrong category → suggested correct one)
- [ ] Multi-language description generation (Hindi, Tamil, Telugu)
- [ ] Integration with Shopify / WooCommerce via API
