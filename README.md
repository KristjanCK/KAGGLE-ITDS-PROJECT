# Gift Bundle Discovery

**Project ID:** C12  
**Author:** Kristjan Caius Kasuk  

---

## Goal

Identify which gift products are frequently purchased together to inform bundle recommendations sing association rule mining on real-world retail data.

**Key Question:** What products do customers naturally buy together, and how can retailers leverage these insights?

---

## Repository Contents

```
IDSProject/
├── data/
│   └── online_retail_II.csv          # Dataset
│
├── notebooks/                      # Analysis & Visualization
│   ├── gift_bundle_analysis_v2.ipynb      # Main FP-Growth analysis
│   ├── curate_top10.ipynb                 # Bundle diversity curation
│   ├── final_poster_visualizations.ipynb  # Generate all 4 poster charts
│   └── [archive/]                         # Some deprecated/experimental notebooks
│
├── outputs/
│   ├── results/
│   │   ├── strong_pairs.csv              # All 545 association pairs (lift>1.5)
│   │   └── final_bundles_for_poster.csv  # Curated top 10 diverse bundles
│   │
│   └── visualizations/                   # Generated PDFs for poster
│       ├── bundle_details_table.pdf
│       ├── lift_distribution.pdf
│       ├── support_confidence_scatter.pdf
│       └── confidence_by_lift.pdf
│
├── README.md                              # This file
└── requirements.txt                       # Python dependencies(not needed if on jupyter)
```

---

## Replication Guide

### Step 1: Setup
```bash
# Download repo
# Install dependencies (unless using jupyter(I mainly did on python but both should work perfectly fine))
pip install -r requirements.txt # Not sure if the txt file is correctly formatted, but it does show what is needed 
```

**Dependencies:**
- `pandas, numpy` - Data manipulation
- `mlxtend` - FP-Growth algorithm
- `matplotlib, seaborn` - Visualizations

### Step 2: Run main analysis
```bash
cd notebooks
jupyter notebook gift_bundle_analysis_v2.ipynb
```

**Runtime:** Over 8 minutes on a modern laptop sadly

### Step 3: Curate bundles
```bash
jupyter notebook curate_top10.ipynb
```

### Step 4: Generate visualizations
```bash
jupyter notebook final_poster_visualizations.ipynb
```

All outputs saved to `outputs/visualizations/` as PDF + PNG


## Code organization attempt

### Core Analysis (`notebooks/`)
- **`gift_bundle_analysis_v2.ipynb`**
  - Data loading and cleaning
  - Product selection
  - Basket creation
  - FP-Growth execution
  - Association rules generation
  - Results export to CSV

- **`curate_top10.ipynb`**
  - Load 545 pairs
  - Remove bags/non-gifts
  - Product categorization function
  - Pair deduplication logic
  - Category-based diversity selection
  - Manual review and adjustment
  - Export final 10 bundles

- **`final_poster_visualizations.ipynb`**
  - Setup and data loading
  - Table visualization
  - Lift distribution charts
  - Support-confidence scatter
  - Confidence-by-lift violin plots
  - Export all PDFs

### Supporting Files
- **`archive/`** - Earlier experimental notebooks (Apriori attempts, different thresholds, various product counts)

## Methodology

### Dataset
- **Source:** Online Retail II (UCI Machine Learning Repository)
- **Citation:** Chen, D. (2019). https://doi.org/10.24432/C5CG6D
- **Scope:** UK gift shop, 2009-2011
- **Size:** 1M+ line items → 36,422 transactions → 26,360 multi-item baskets

### Algorithm Selection
**FP-Growth** chosen over Apriori:
- Initial testing with Apriori failed due to insufficient memory
- FP-Growth uses compact tree structure (no candidate itemset generation)
- Successfully processed dataset with identical parameters

### Parameters
- **Min Support:** 0.5% of transactions
- **Min Confidence:** 30% (if customer buys A, 30%+ chance of buying B)
- **Min Lift:** 1.5× (items bought together 1.5× more than random)

### Threshold Tuning
- **Initial:** lift>3, confidence>0.5 → 60 pairs (only matching color sets)
- **Final:** lift>1.5, confidence>0.3 → 545 pairs (discovered cross-category patterns)
- **Insight:** Lowering thresholds revealed hidden themed bundles (  Christmas napkins + cake cases)

### Curation Strategy
1. Remove utility items (bags, non-gifts)
2. Deduplicate symmetric pairs (A→B and B→A)
3. Categorize into 6 groups: Food & Dining, Kids, Home Decor, Clothing & Accessories, Storage, Entertainment
4. Select 10 diverse bundles spanning multiple categories

## If necessary I have mutiple and multiple more ipynb file attempts, ranging from apriori attempts to more unique graphs and visuals
---
