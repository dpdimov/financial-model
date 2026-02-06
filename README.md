# Financial Model Simulator

An interactive 5-year financial planning tool for entrepreneurship education, built as a React application. Students build financial models by following a structured four-step process — from revenue decomposition through to funding requirements — and then explore scenarios and run Monte Carlo simulations to stress-test their assumptions.

Developed for the **V-733-ENTR Entrepreneurial Finance** course at the University of Bath School of Management.

---

## Pedagogical Framework

The tool is organised around four steps that mirror the logic of building a financial model from first principles:

### Step 1: Revenue Model
> *What are the revenues? How can we model them?*

Revenue is decomposed using the fundamental formula:

**Revenue = Customers × Units per Transaction × Price per Unit × Frequency**

Each lever can be varied independently. Students define one or more revenue streams, each with its own customer growth rate and churn dynamics. This makes explicit that revenue is not a single number but the product of distinct, measurable drivers.

### Step 2: Operating Assets & Activities
> *What activities does revenue generation require? What operating assets need to be in place?*

Students identify the fixed assets (equipment, fit-outs, machinery — depreciated over their useful life) and operating costs (people, rent, services) needed to deliver the revenue defined in Step 1. Each operating cost has its own payment terms — payroll is paid immediately (0 days), while suppliers may have 15–30 day terms. This is where the causal link between operations and costs is made explicit.

### Step 3: Variable Costs
> *What are the costs associated with producing each unit of revenue?*

Costs that scale directly with revenue: COGS, materials, shipping, commissions, platform fees, etc. The tool calculates the contribution margin in real time, giving immediate feedback on the margin structure.

### Step 4: Funding & Working Capital
> *How do we fund the necessary operating assets?*

This is where the model comes together. The working capital cycle (cash conversion cycle) is computed from customer payment terms, inventory days, and the weighted-average payment terms of operating costs. The funding gap — how much additional capital is needed and when — emerges automatically from the model. Funding rounds can be equity or debt, each with distinct effects on the P&L and cash flow.

---

## Features

### Model Builder
- **Eight business model templates** as starting points (see below), all fully editable
- Step-by-step guided navigation or "Show All" view
- Live Year 1 revenue preview per stream
- Real-time contribution margin and monthly cost summaries
- Automatic funding gap detection with warnings
- Per-item payment terms on operating costs (payroll at 0 days, suppliers at 30 days, etc.)

### 5-Year Projections
- Quarterly P&L chart showing Revenue, EBIT, and Net Income (after interest)
- Cash flow trajectory including working capital effects and debt service
- Working capital chart showing net WC and period-on-period changes
- Annual summary table with Revenue, EBIT, Interest, Net Income, Operating Cash Flow, Cash Position, Net Working Capital, Debt Outstanding, and Customers
- **Balance sheet**: end-of-year snapshots showing Assets (cash, receivables, inventory, fixed assets at NBV), Liabilities (payables, debt outstanding), and Equity (invested equity, retained earnings). Includes stacked bar charts for asset composition and funding structure, plus a verification row confirming Assets = Liabilities + Equity
- Key metrics: Year 5 ARR, break-even month, cash position, funding gap, cash conversion cycle, outstanding debt

### Scenario Explorer
- **Revenue levers**: price adjustment, purchase frequency, customer growth rate, churn rate
- **Cost levers**: operating cost multiplier, variable cost adjustment
- **Working capital levers**: days receivable, days payable, days inventory
- **Funding levers**: additional equity, additional debt (with configurable term and interest rate)
- Side-by-side base vs. scenario comparison across three charts: Revenue, EBIT, and Cash Position
- Summary metrics showing the impact of each scenario

### Monte Carlo Simulation
- Configurable number of runs (100–2,000)
- Three volatility parameters: growth, churn, and cost
- Adjustable default threshold (% of total funding)
- **Output distributions**: break-even timing, IRR, NPV (at 10% discount rate), Year 5 cash position
- **Summary statistics**: survival rate, default risk, break-even probability, median IRR/NPV with P10–P90 ranges
- IRR vs. NPV scatter plot colour-coded by survival/default

---

## Templates

| Template | Icon | Revenue Model | Key Financial Characteristics |
|----------|------|---------------|-------------------------------|
| **Custom (Blank)** | ⚡ | Generic | Start from scratch |
| **SaaS / Subscription** | ☁️ | Recurring subscriptions with churn | Low COGS, high operating costs (engineering), 0-day receivables, negative working capital possible |
| **Marketplace / Platform** | 🏪 | Transaction commissions + premium plans | Two-sided dynamics, payment processing costs, moderate WC |
| **E-Commerce / DTC** | 📦 | Online product sales | 35%+ COGS, shipping costs, 30-day inventory, debt-funded |
| **Professional Services** | 💼 | Retainers + project work | High margins, 45-day receivables, minimal fixed assets, people-intensive |
| **Manufacturing / Hardware** | 🏭 | Wholesale B2B + DTC | Capital equipment (depreciated), 52% variable costs, 45-day inventory + receivables, equipment finance debt |
| **Retail Shop** | 🏬 | In-store + click-and-collect | Shop fit-out as fixed asset, 45% COGS, 0-day receivables (POS), 30-day inventory |
| **Café / Restaurant** | ☕ | Dine-in + takeaway + drinks | Kitchen equipment, split kitchen/FOH staff, perishable inventory (5 days), ~46% food/bev costs, start-up loan |

Each template pre-populates realistic values that students can customise. The templates are designed to highlight contrasting financial dynamics — e.g. SaaS vs. manufacturing working capital, services vs. retail margin structures, or the cash cycle differences between digital and physical businesses.

---

## Debt vs. Equity Funding

Funding rounds can be either:

- **Equity**: Capital injection at a specified month. No repayment, no interest expense. Increases cash position without affecting P&L.
- **Debt**: Loan with a specified term (months) and annual interest rate. Creates monthly interest expense (below EBIT on the P&L) and straight-line principal repayments (cash flow, not P&L). Outstanding balance is tracked over time.

This allows students to explore capital structure decisions — e.g. using equipment finance for fixed assets and equity for working capital — and see the impact on profitability, cash flow, and risk.

---

## Working Capital Model

The working capital cycle is computed from three components:

1. **Days Receivable** (customer payment terms) — set globally
2. **Days Inventory** — set globally
3. **Days Payable** — set per operating cost item (payroll = 0, suppliers = 30, etc.)

The cash conversion cycle = Days Receivable + Days Inventory − Weighted Average Days Payable.

Changes in net working capital consume or release cash each month. This means that even profitable businesses can run out of cash if they're growing fast with long receivables or high inventory — a key insight for students.

---

## Deployment

The application is a single React (.jsx) file that uses:
- **React** (with hooks: useState, useMemo, useCallback)
- **Recharts** for all charts (LineChart, AreaChart, BarChart, ComposedChart, ScatterChart)
- **Google Fonts**: Instrument Sans (UI) and JetBrains Mono (numbers)

No build step is required if deploying via a platform that supports JSX rendering (e.g. as a Claude artifact, or via a simple React setup). For standalone deployment:

```bash
# Using Vite
npm create vite@latest financial-simulator -- --template react
# Copy financial-simulator-v3.jsx into src/App.jsx
# Install recharts
npm install recharts
npm run dev
```

Alternatively, deploy as a static site using any React-compatible hosting.

---

## Integration with Course Materials

This tool complements the broader suite of interactive tools developed for V-733-ENTR:

- **Business Model Canvas Financial Tool** — links business model design to revenue and cost structures
- **Cash Flow Management Tool** — spreadsheet-based cash flow analysis
- **LTV / CAC Analyser** — customer economics and acquisition efficiency
- **J-Curve Analyser** — venture fund return dynamics
- **Risk Visualisation Engine** — financial risk assessment

The Financial Model Simulator brings these together: students use the business model canvas to identify their model, then build the full 5-year financial plan here, exploring scenarios and running simulations to understand robustness.

### Suggested Classroom Use

1. **In-class demonstration**: Load a template, walk through the four steps, show how changing one assumption (e.g. churn rate, payment terms) cascades through the entire model
2. **Group exercise**: Each group selects a template closest to their venture and customises it with researched assumptions
3. **Assignment component**: Build a complete financial model, document key assumptions, run scenarios, and interpret Monte Carlo results
4. **Comparative analysis**: Run the same scenario adjustments across different templates to illustrate how business model structure determines financial resilience

---

## Limitations

- **Monthly granularity**: All calculations are monthly; no daily or weekly resolution
- **No seasonality**: Revenue and costs grow smoothly; seasonal patterns are not modelled
- **Simplified depreciation**: Straight-line only; no accelerated or units-of-production methods
- **No tax**: The model does not include corporation tax or VAT
- **Simple debt**: Straight-line principal repayment; no balloon payments, revolving credit, or covenants
- **Stochastic model**: Monte Carlo uses independent monthly shocks; no autocorrelation or regime changes

These are deliberate simplifications for pedagogical clarity. The goal is to teach the logic of financial modelling, not to replicate a full corporate finance model.

---

**Version**: 3.0
**Last Updated**: February 2026
**University of Bath School of Management**
