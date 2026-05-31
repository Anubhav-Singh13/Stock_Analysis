---
name: stock-fundamental-analysis
description: Performs end-to-end stock fundamental and technical analysis using an 11-step framework synthesized from canonical investing texts (Graham, Buffett, Fisher, Porter, Fridson, O'Glove, Schilit, McKinsey, Marks, Klarman, Weinstein, O'Neil's CANSLIM, Mauboussin's Expectations Investing). Pulls data from authentic primary sources — BSE/NSE corporate filings, SEC EDGAR, company annual reports, concall transcripts, IR pages — and produces a 2-3 page investment report with a clear analytical verdict (Strong Setup, Quality at Wrong Price, Wait for Confirmation, Cheap but Flawed, or Avoid). Uses a full 10-year financial dataset wherever available to read business-cycle behaviour and management decision quality, and applies Mauboussin's Price-Implied Expectations (PIE) framework to decode what the current stock price is betting on before forming an independent view. Use this skill whenever the user asks to analyze a stock, evaluate a company as an investment, asks "should I buy/sell [ticker]", wants a fundamental analysis or deep dive, mentions a ticker with evaluative intent, asks for an intrinsic value estimate, or requests an investment report — even when they don't explicitly ask for a framework. Also use when comparing two stocks as investment candidates.
---

# Stock Fundamental Analysis

## Purpose

This skill performs a disciplined, end-to-end fundamental and technical evaluation of a single publicly-traded stock, then produces a 2-3 page investment report with an explicit verdict. The framework is "outside-in": macro context → sector/stock cycle → industry structure → company quality → financials → earnings quality → valuation → margin of safety gate → technical timing → execution criteria.

Two analytical commitments underpin every analysis:

1. **Full-decade data** — wherever a 10-year financial history is available, use it. A decade captures at least one full business cycle, exposes management decision quality under stress, reveals whether a moat genuinely holds across downturns, and shows business-model pivots that only become visible over time. Five-year views hide the peaks that preceded the current trough and the troughs that preceded the current peak.

2. **Read the price before forming a view (Mauboussin)** — before doing your own DCF, back out the Price-Implied Expectations (PIE): what revenue growth, operating margin, and investment rate the *current stock price is already betting on*. This prevents falling in love with a thesis before checking whether the market has already priced it in. The Expectations Gap — the difference between PIE and your own modeled expectations — is where investment alpha lives.

The goal is an *analytical view* the user can act on — not personalized financial advice. The report ends with a categorized verdict (Strong Setup, Quality at Wrong Price, Wait for Confirmation, Cheap but Flawed, or Avoid) and a clear explanation of what would change that view.

## When this skill applies

Trigger on any of these patterns:
- "Analyze [company/ticker]"
- "Should I buy/sell/hold [stock]?"
- "Is [stock] a good investment?"
- "What do you think of [company] at current price?"
- "Do a fundamental analysis of [stock]"
- "Give me an intrinsic value estimate for [stock]"
- "Compare [stock A] vs [stock B] as investments"
- "Deep dive on [company]"
- Any mention of a ticker symbol with evaluative intent

Do NOT trigger on: general questions about investing theory, requests to explain a single financial metric, or questions about portfolio construction.

## The 11-step workflow

Execute these steps in order. Each step has a pass/fail or scoring outcome that feeds into the final verdict. Do not skip steps. If data is unavailable for a step, note it explicitly in the report and proceed.

```
[1 ] Broad market cycle (Marks)
       ↓
[1b] Sector cycle — current headwinds / tailwinds
       ↓
[1c] Stock cycle — company-specific positioning
       ↓
[2 ] Industry structure (Porter's Five Forces)
       ↓
[3 ] Business quality & moat (Buffett, Fisher)
       ↓
[4 ] Financial statements (Fridson — read as one)
       ↓
[5 ] Earnings quality (O'Glove, Schilit red flags)
       ↓
[6 ] Intrinsic value + PIE (McKinsey, Buffett, Mauboussin)
       ↓
[7 ] Margin of safety GATE (Graham, Klarman) ←── decision point
       ↓
[8 ] Breakout setup (Weinstein Stage 2 + CANSLIM)
       ↓
[9 ] Execution criteria (stop-loss, position sizing)
```

## Data sourcing — use primary sources only

Before opinion, get the data. **Always prefer primary sources** (company filings, exchange-mandated disclosures, concall transcripts) over secondary commentary. See `references/data-sources.md` for the full list with URLs.

**Indian stocks (BSE/NSE)** — fetch in this priority order:
1. **BSE corporate announcements**: bseindia.com → Corporates → Announcements (latest filings, results, board meetings)
2. **NSE corporate filings**: nseindia.com → Companies → Corporate Information
3. **Company IR page**: usually `investors.[company].com` — for annual reports, investor presentations, concall transcripts
4. **Annual report (latest)**: full PDF from IR page; this is the single most important document
5. **Last 4 quarterly results + concall transcripts**: from IR page or trendlyne.com / researchbytes.com
6. **Screener.in**: best aggregated 10-year financials view for Indian stocks (`screener.in/company/[TICKER]/`)
7. **SEBI filings** for governance/shareholding pattern: sebi.gov.in

**Global / US stocks** — fetch in this priority order:
1. **SEC EDGAR**: sec.gov/edgar — 10-K (annual), 10-Q (quarterly), 8-K (material events), DEF 14A (proxy/governance)
2. **Company IR page**: latest earnings release, investor presentation, transcript
3. **Earnings call transcripts**: Seeking Alpha, Motley Fool, or company IR archive
4. **Aggregated financials**: stockanalysis.com, macrotrends.net, finbox.com

**Technical data (for steps 8-9)**:
- Weekly chart with 30-week simple moving average, volume bars, RS line
- TradingView, Investing.com, Chartink (India), or any charting platform
- 52-week high/low, current price vs. base structure

**Critical sourcing rule**: For every claim in the report, cite the source (e.g., "Q3FY25 concall", "FY24 annual report p.47", "10-K filed 2024-02-15"). If sourcing fails, say so — never fabricate numbers.

## Step-by-step execution

### Step 1: Market cycle context (Marks)

**Goal**: Establish whether the broader market and the stock's sector are in a phase favorable to new positions.

**Inputs to gather**:
- Current level of major index (Nifty 50, S&P 500) vs. 52-week range
- Sectoral index level and trend vs. broad market
- Sentiment indicators: VIX level, high-yield credit spreads (HYG/JNK ETF), put-call ratio
- Recent IPO activity and pricing (frothy markets show in IPO pops)
- Marks's checklist signal: "Is the market closer to greed or fear?"

**Assessment**: Categorize the environment as Euphoric / Optimistic / Neutral / Cautious / Fearful. Defensive in euphoric, aggressive in fearful, normal sizing in neutral.

**Pass condition**: This step does not gate the analysis but sets sizing context. A great setup in a euphoric market still gets a smaller position than the same setup in a fearful one.

### Step 1b: Sector cycle — current headwinds / tailwinds

**Goal**: Determine whether the specific sector/segment the company operates in is currently in a favorable or unfavorable phase, independent of broad market direction. Step 1 tells you where the sea is; this step tells you whether the company's particular current is running with or against it.

**Inputs to gather** (go beyond annual report generalizations — seek recent, specific data):
- **End-market demand — and secular vs. cyclical decomposition**: Is the company's primary customer industry currently growing, stagnating, or contracting? Use industry body data, government export/import statistics, and peer earnings commentary from the last 2 quarters. Crucially, decompose observed demand growth into its **secular** component (structural growth independent of the economic cycle — demographics, regulatory mandates, technology adoption, rising incomes) and its **cyclical** component (post-destocking restocking, post-COVID normalisation, capex upcycle in a customer industry, pent-up demand release). A company reporting 35% revenue growth that is 10% secular + 25% cyclical recovery will mean-revert toward 10–12% once the cyclical tailwind fades; embedding 35% in a DCF inflates intrinsic value 3–4×. State explicitly: what % of current demand growth is cyclical and likely to fade over 2–3 years, and what is the durable structural baseline that should anchor the DCF's mid-period growth rate.
- **Input cost cycle**: Are key raw material or energy prices rising, falling, or stable? A falling input cycle structurally widens margins; a rising one compresses them regardless of revenue growth.
- **Inventory cycle**: Is the sector in a restocking or destocking phase? Destocking suppresses near-term order flow even when underlying end-demand is intact.
- **Regulatory & policy calendar**: Are there active tailwinds (PLI schemes, anti-dumping duties, mandated adoption, export incentives) or headwinds (price controls, compliance costs, import liberalization, tariff removal) specific to this sector right now?
- **Global competition dynamics**: Is cheap foreign competition (especially China) intensifying or retreating? Is a China+1 sourcing shift currently materializing in order books, or still aspirational?
- **Peer operating metrics**: What are 2-3 direct competitors reporting in their most recent quarters — capacity utilization, pricing trends, order book direction? Concall commentary from peers is more actionable than generic industry research.
- **Competitive capacity pipeline**: Are direct competitors also commissioning new capacity in the same 12–24 month window? A sector where two or three players all bring on capacity simultaneously faces structural oversupply and price pressure regardless of demand conditions. Conversely, if peers are deferring or cancelling capex, the supply picture tightens — a pricing tailwind independent of demand. Source: competitor annual reports, concall transcripts, and CRISIL/ICRA/Ind-Ra credit reports, which list capex pipelines for rated entities.
- **Geopolitical risk map**: Identify which of the company's key revenue geographies, raw-material origins, and logistics routes carry active geopolitical risk — and whether that risk is transient or structural. Specifically: (a) Are any top-5 export markets subject to active tariff disputes, sanctions, import quotas, or currency controls that could interrupt order flow? (b) Are critical raw materials sourced from a geopolitically concentrated region (e.g., China for specialty chemicals/APIs, Russia for fertilisers/palladium, Middle East for petrochemicals)? (c) Do key shipping routes pass through active disruption zones (Red Sea/Suez, Strait of Hormuz, Panama Canal, Taiwan Strait)? For each identified exposure, assess: can it be re-routed or hedged within a quarter (transient — adjust margin assumption), or does it require sourcing redesign or market exit (structural — widen MoS). Do not rely on boilerplate annual-report risk disclosures — check the last 2 quarters of concall transcripts for evidence of actual freight cost or order-delay impact.
- **Net FX exposure and P&L sensitivity**: For any company with meaningful cross-border revenue or costs, quantify the net currency position: (a) % of revenue in foreign currency, broken down by major currency (USD, EUR, GBP, BRL, etc.); (b) % of costs naturally offset in the same currency (raw-material imports, royalties, USD-denominated debt service); (c) net exposure after natural hedges; (d) P&L sensitivity to a ±5% move in the dominant currency. A company with 70% USD revenue and 10% USD costs has a 60% net long-USD exposure — a 5% INR appreciation at 12% operating margins wipes out ~25% of operating profit in that year. Unhedged FX exposure in a high-margin business is manageable; in a thin-margin business it can eliminate the full year's profit. State whether the company has a formal hedging policy and, if so, hedge ratio and tenor.
- **Sector index vs. broader market**: Has the sector index outperformed or underperformed the broad market over the last 3 and 6 months? Sustained underperformance often precedes analyst downgrades; outperformance can signal a re-rating in progress.

**Sources**:
- India: Ministry of Textiles/Commerce for textile export data; CMIE, DPIIT for industrial output; Nifty sectoral indices (Nifty Chemicals, Nifty Pharma, Nifty IT, Nifty Auto, etc.) on NSE for sector vs. market performance
- Global: Bloomberg commodity indices, FRED (US macro), trade body reports (FICCI, CII, relevant industry associations)
- Competitor concall transcripts (last 2 quarters) — most reliable primary source for sector sentiment
- Trendlyne sector dashboard or Screener.in industry filter for peer operating metrics

**Assessment**: Score the current sector environment as **Tailwind / Neutral / Headwind**. In one sentence, name the primary driver and its expected duration (transient quarter-level vs. multi-year structural).

**Pass condition**: This step does not gate the analysis but calibrates two later steps. (a) In Step 6, a sector tailwind should *not* be embedded as a permanent growth assumption — it inflates the DCF. A company growing faster than the sector during a tailwind needs to demonstrate it can hold that when the tide recedes. (b) In Step 7, a sector headwind warrants a wider margin of safety than the moat quality alone implies, because near-term earnings are less predictable.

---

### Step 1c: Stock cycle — company-specific positioning

**Goal**: Establish where this specific company sits in its own operating and growth cycle, independent of both the broad market and its sector. A stock can be an excellent buy at the right point in its own cycle even in a neutral or headwind sector — provided the cycle turn is identifiable and the entry price compensates for the uncertainty.

**Inputs to gather**:
- **Revenue growth trajectory**: Is the revenue growth *rate* accelerating, stable, or decelerating over the last 6–8 quarters? Plot it explicitly — acceleration is a buy signal; multi-quarter deceleration is a warning even if absolute growth remains positive.
- **Revenue growth decomposition — volume, price, mix**: Break down the last 3–4 years of revenue CAGR into its three components. *Volume growth* is physically bounded by the capacity ceiling and has a hard floor at zero when the plant is full. *Price growth* is the most fragile — it is usually the first component surrendered when competition intensifies or input costs normalise. *Mix shift* (moving toward higher-value products, regulated markets, or premium geographies) is the most durable because it improves realization per unit without requiring more volume or pricing power. Identify which driver has dominated the historical CAGR, then assess its durability forward. A company growing 20% entirely on pricing in a competitive market has a fragile model; a company growing 15% through volume + positive mix shift is far more defensible. Source: management concall commentary often breaks this out; alternatively, compare revenue CAGR vs. utilization trends (volume proxy) and gross margin direction (mix/price proxy).
- **Capacity cycle phase**: Is the company in an expansion phase (heavy capex, temporarily depressed margins and FCF) or a harvest phase (capex light, FCF maximizing)? New facilities typically suppress near-term margins and OCF while setting up the next earnings leg; this is acceptable if the capacity rationale is sound.
- **Capacity headroom and ceiling**: For manufacturing/asset-heavy companies, quantify: (a) installed capacity in physical units (MT, KL, MW, beds, seats — whatever the relevant unit is), (b) current utilization rate, (c) implied revenue per unit of capacity (back-calculate from current revenue ÷ utilized units — this is your "realization" anchor), (d) remaining headroom at current utilization. A company running at 90%+ utilization with no new expansion announced cannot grow revenue faster than pricing alone allows — volume headroom is essentially zero. A company at 60% utilization in rising demand has a multi-year organic runway without any new capex. Always establish the physical ceiling before accepting any revenue growth assumption. Carry the capacity model forward into Step 6 as the hard constraint on the DCF.
- **Margin cycle**: Are operating margins expanding (early recovery or pricing power intact), stable, or compressing (late cycle, cost pressure, or deliberate mix shift)? Distinguish between cyclical compression (recoverable) and structural compression (permanent).
- **Management guidance trajectory and accuracy**: Has management raised, held, or cut guidance over the last 3–4 concalls? Consistent upgrades signal cycle momentum; a first downward revision often precedes several more. Beyond direction, score the *accuracy* of guidance over the last 6–8 quarters: did the company deliver within ±10% of stated targets? Management that guides conservatively and beats is a systematic long-term alpha source because their DCF inputs are trustworthy; management that habitually over-promises and under-delivers destroys your model assumptions before you build them. Check this across at least two full years of concall transcripts — one data point is noise, a pattern is signal.
- **Order book / pipeline visibility**: Does the company disclose backlog, signed contracts, or customer pipeline data? A growing, multi-quarter order book provides earnings visibility that reduces DCF uncertainty.
- **Working capital cycle**: Is the cash conversion cycle (debtor days + inventory days − payable days) tightening or lengthening over the last 4–6 quarters? A lengthening CCC during a revenue ramp can be normal (supporting new customers); persistent lengthening after ramp-up signals stress or aggressive recognition.
- **Earnings revision trend**: Are sell-side consensus estimates being revised up or down over the last 2–3 quarters? Rising estimates confirm fundamental momentum. (Sources: Trendlyne or Tickertape for India; Seeking Alpha / Bloomberg consensus for US.)
- **Promoter / insider activity**: Open-market promoter buying signals conviction; selling signals caution. Check BSE/NSE bulk and block deal data, SAST disclosure filings.

**Assessment**: Classify the company's current cycle position as one of four:
- **Early cycle / Ramp-up**: Capex heavy, margins suppressed, revenue accelerating — buy if thesis is sound and entry price compensates for near-term FCF weakness
- **Mid cycle / Steady compounder**: Revenue and margins stable, FCF strong — quality check is the primary filter; pay up only for wide moat
- **Late cycle / Maturing**: Growth rate decelerating, margins at or near peak, capex moderating — requires strict valuation discipline; avoid paying full price
- **Trough / Recovery candidate**: Revenue or margins temporarily depressed by a identifiable, reversible cause — highest potential return but needs earnings quality validation before entry

**Pass condition**: This step does not gate the analysis but directly adjusts three downstream inputs:
1. **Step 6 DCF assumptions**: Early-cycle ramp-up justifies higher near-term growth rates; late-cycle requires aggressive fade toward terminal.
2. **Step 7 margin of safety**: Late-cycle and trough stocks warrant a 5–10 percentage-point wider MoS than the moat quality alone implies.
3. **Step 9 position sizing**: Trough-recovery setups warrant smaller initial positions until the cycle turn shows up in at least one quarter of financial results.

---

### Step 2: Industry structure (Porter's Five Forces)

**Goal**: Determine whether the industry's structural economics support long-run profitability.

**For each of the five forces, score Low / Medium / High** based on observable evidence:
1. **Threat of new entrants** — capital intensity, regulatory barriers, scale economies, network effects, brand strength
2. **Bargaining power of suppliers** — supplier concentration, switching costs, input commoditization
3. **Bargaining power of buyers** — customer concentration, price sensitivity, switching costs
4. **Threat of substitutes** — availability and price/performance of alternatives outside the industry
5. **Rivalry among existing competitors** — number of competitors, growth rate, fixed costs, differentiation, exit barriers

**Source the inputs from**:
- Industry section of the annual report (MD&A)
- Concall Q&A (management talks about competition)
- Trade publications, regulatory filings of competitors

**Assessment**: Industry is Attractive (3+ forces low/medium) / Mixed / Unattractive (3+ forces high). Note which forces are *changing* and in which direction.

**Pass condition**: Unattractive industries get a high bar at later steps. Only the best companies in bad industries make it through.

### Step 3: Business quality & moat (Buffett, Fisher)

**Goal**: Identify whether this specific company has a durable competitive advantage within its industry.

**Moat checklist** — name the moat type(s) if any:
- Cost advantage (scale, process, location, unique input access)
- Brand / intangible asset (pricing power evidence: gross margins vs. peers)
- Switching costs (customer retention rates, contract length)
- Network effects (does value grow with users?)
- Regulatory / legal barriers (patents, licenses)
- Efficient scale (limited market that supports few players)

**Quality checklist (Fisher's 15 points, condensed)**:
- **Addressable market (TAM) ceiling relative to current scale**: Size the total addressable market in ₹/$ terms and state the company's current market share. A company at 2–3% share of a large TAM has genuine multi-decade volume runway; a company already at 35–40% share of a niche TAM has limited organic growth ahead and must either expand into adjacent markets or grow the TAM itself — both of which introduce execution risk not captured in historical numbers. Without this check, the terminal value in the DCF can embed growth rates the market structure cannot arithmetically support. Source: industry body reports, management IR presentations, or a bottom-up build from (total industry volume × average selling price) cross-checked against stated market size from two independent sources.
- R&D / innovation track record
- Sales effectiveness (gross margin trend)
- Management depth and succession
- Labor / culture indicators (Glassdoor, attrition if disclosed)
- Capital allocation history (acquisitions, buybacks, dividends)
- Insider ownership and incentive alignment
- Communication transparency (does management address bad news directly in concalls?)
- **Customer concentration**: What % of revenue comes from the top 1, 3, and 5 customers? A single customer representing >15% of revenue creates event risk — loss of contract, pricing renegotiation, or the customer's own financial stress — that does not appear anywhere in historical financials but dominates forward earnings risk. Conversely, a base where no customer exceeds 10% indicates structural pricing resilience. Source: annual report revenue concentration disclosures and related-party transaction notes. If the company doesn't disclose this, flag the opacity itself as a governance concern.
- **Regulatory approval pipeline** (pharma, chemicals, financial services, telecom, defence): For regulated industries, the future moat is partly a function of approvals in-flight today. Tabulate: (a) number and status of pending approvals (DMFs, ANDAs, CEPs, manufacturing licences, product registrations); (b) expected timelines and the regulatory authority's current review backlog; (c) estimated addressable revenue per approval; (d) any active warning letters, import alerts, show-cause notices, or consent decrees that could suspend existing approvals. An empty regulatory pipeline means the moat is not being refreshed — current products will face generic competition without replacement. A dense pipeline with near-term approvals signals a moat in active expansion and justifies higher near-term growth assumptions in Step 6.
- **Promoter and governance quality** (beyond pledging): High promoter holding is cited as alignment, but structure matters as much as percentage. Assess: (a) Is the promoter group a simple direct holding or a multi-layered holding-company structure (opacity risk)? (b) Have related-party transactions as a % of revenue been stable, rising, or falling over 5 years? (c) Are independent directors genuinely independent (tenure, backgrounds, committee memberships — 10+ year "independent" directors are rarely independent)? (d) Has the company received any SEBI notices, regulatory show-cause orders, or qualified auditor opinions in the last 5 years? A promoter holding 72% through a clean, direct structure with genuinely independent oversight is a different risk profile than a 72% stake routed through a web of subsidiaries.

**10-year moat durability retrospective** — the moat checklist above tells you what the moat *is*; the 10-year look tells you whether it has been *durable*. Ask these questions across the decade:
- Has ROCE stayed consistently above cost of capital for most of the last 10 years, or has it been declining? (Declining ROCE over a decade is the earliest sign of moat erosion.)
- How did gross margins and operating margins behave during the worst 2–3 years in the dataset? A genuine moat shows up as margin resilience under stress, not just strong margins in good years.
- What did management do with capital at the peak of the cycle? (Over-leveraged acquisitions at cycle peaks are a management quality tell — see STL FY19–22 as a cautionary example.)
- Has revenue mix shifted in a way that strengthens or weakens the moat? (Shift from services to products, or from domestic to export, or from branded to commodity — each has a different moat implication.)
- Has the company consistently reinvested at returns above CoC, or did it return capital because it couldn't find high-return opportunities? Both can be correct; knowing which is true matters for the growth assumption in Step 6.

**Source the inputs from**:
- Annual report Chairman's letter and MD&A (read the last 5–7 years, not just the latest)
- Latest 4-8 concall transcripts (look for consistency, and compare what management said at the prior-cycle peak vs. trough)
- Insider holding pattern (from BSE/NSE disclosures or SEC Form 4)
- Glassdoor, LinkedIn for management/employee signals
- 10-year ROCE and gross margin data from Screener.in / stockanalysis.com (pull and trend visually)

**Assessment**: Moat is Wide / Narrow / None. Quality is High / Medium / Low. Add a one-line verdict on moat durability: *Holding / Eroding / Strengthening* based on the 10-year retrospective.

**Pass condition**: At least Narrow moat + Medium quality to proceed without heavy discount. Otherwise treat as a deep-value play only. A moat rated "Eroding" across the decade requires a 10 percentage-point wider MoS in Step 7, even if current metrics look acceptable.

### Step 4: Financial statements (Fridson — read as one, across the full decade)

**Goal**: Confirm that financial performance matches the business quality narrative — and use a full 10-year dataset to read the business across at least one complete cycle, not just the recent upswing.

**Pull the full 10 years wherever available** (Screener.in provides this for Indian stocks in one view; macrotrends.net or stockanalysis.com for US). Use the most recent 5 years for precision and the 10-year view for pattern-reading. If the company is younger than 10 years, use all available history and note the limitation.

**Metrics to pull for each year**:
- Revenue (absolute and YoY growth rate — plot the rate, not just the level)
- Gross margin, operating margin, net margin
- Net income vs. operating cash flow (divergence over time is the most important signal)
- Free cash flow (OCF minus capex) and FCF yield
- ROCE / ROIC (compare each year to the estimated cost of capital)
- Working capital: receivables days, inventory days, payable days, cash conversion cycle
- Net Debt / EBITDA and interest coverage ratio; also note the *structure* of debt — short-term vs. long-term tenor, fixed vs. floating rate, secured vs. unsecured, and whether material bank covenants exist (common forms: Net Debt/EBITDA < X, minimum ICR threshold). Short-dated debt that needs annual renewal is a refinancing risk the headline leverage ratio hides; a covenant breach at a cyclical trough can force asset sales or equity dilution that permanently impairs shareholder value in ways a DCF does not capture
- Shares outstanding and dilution pattern (cumulative dilution over 10 years matters)
- Dividend payout and buyback history (capital return vs. reinvestment split)

**What a 10-year dataset reveals that 5 years cannot**:

1. **Full-cycle ROIC**: Average ROIC across a decade — including at least one downturn — is the definitive test of whether the moat is real. A company with 20% ROIC in boom years but 5% ROIC in troughs has a cyclical business dressed up as a compounder. Average must beat CoC across the full cycle.

2. **Business model pivots**: Revenue mix shifts (product to services, domestic to export, B2B to B2C), segment additions or exits, and geographic diversification are most visible over 10 years. Ask: is the business today fundamentally different from what it was 10 years ago? If yes, treat recent financials with more caution — there is less comparable history.

3. **Management quality under stress**: What happened to margins, debt, and capital allocation in the worst 2–3 years of the decade? Did management over-invest at the cycle peak (capex surge, dilutive acquisitions)? Did they cut capex and protect the balance sheet during the trough? These decisions reveal capital allocation character.

4. **Debt cycle discipline**: Plot net debt over 10 years alongside EBITDA. A company that consistently levers up at peaks and struggles to deleverage has a structural leverage trap. A company that funds expansion internally and exits every trough with a cleaner balance sheet is disciplined.

5. **Earnings compounding vs. P&L engineering**: Over 10 years, the gap between cumulative reported earnings and cumulative FCF becomes undeniable. If reported profits have compounded at 20% but cumulative FCF barely moved, the reported earnings are largely accounting rather than cash. This is impossible to hide over a decade.

6. **Readiness for future growth**: Does the 10-year capex and R&D investment pattern match the growth ambitions management now articulates? A company claiming to enter a high-growth segment that has invested negligible R&D in it for the past 5 years is aspirational, not prepared. Conversely, a company with heavy capex investment in a segment over 3–4 years that is now showing early revenue traction is genuinely prepared.

7. **Asset turnover restoration** (manufacturing / asset-heavy companies): Compute revenue ÷ (gross fixed assets + CWIP) for each year of the decade. A plant under construction sits in CWIP and earns nothing — asset turnover falls and ROCE is suppressed. The year it transitions from CWIP to productive fixed assets and begins generating revenue is the ROCE inflection signal. If the 10-year pattern shows consistent post-capex asset turnover recovery within 2–3 years of commissioning, management has a proven execution track record and the current expansion is de-risked. If prior capex cycles took 4–5 years to show up in asset turnover, extend the discount period in Step 6 accordingly and widen the MoS in Step 7.

8. **Capital allocation quality score**: Across the decade, assess four dimensions: (a) *Self-funding ratio* — cumulative capex ÷ cumulative OCF. Below 0.8× means the company funded most growth from operations (capital-light discipline); above 1.2× means it consistently relied on external capital (debt or equity dilution) to grow, which is only acceptable if ROIC on that capital was demonstrably above WACC. (b) *Acquisition track record* — for any material acquisitions, compare ROCE in the acquired business pre- and post-acquisition, and check whether the parent's consolidated ROCE improved or declined post-deal. Most acquisitions destroy value; the ones that don't are a strong management quality signal. (c) *Buyback timing quality* — did management buy back shares when the stock was below intrinsic value (sensible capital return) or at cycle peaks (value-destroying signalling)? (d) *Dividend consistency* — a company that maintained or grew dividends through the trough years has genuine FCF confidence; a company that cut dividends at the first sign of stress has uncertain underlying cash generation. A high-quality allocator on all four dimensions justifies a multiple premium in Step 6 and a narrower MoS in Step 7 relative to the moat quality alone.

**Source**: Screener.in for Indian stocks (10-year financials in a single view). For US: macrotrends.net or stockanalysis.com. Cross-check at least 3 data points against the actual annual report. Note any restated figures.

**Assessment**: Financials are Strong / Adequate / Weak / Deteriorating. Add a one-line 10-year summary: *what did the decade reveal about this business that the last 2 years alone would not?*

**Pass condition**: Average ROIC above cost of capital (typically 12–15%) measured across the full available cycle — not just the most recent good years. If absent, the moat thesis from Step 3 is suspect regardless of current metrics.

### Step 5: Earnings quality — red flags (O'Glove, Schilit)

**Goal**: Test whether the reported financials are honest. This is the step that catches frauds and aggressive accounting.

**Run the full checklist** in `references/red-flags.md`. Key items to always check:
- Receivables growing significantly faster than revenue
- Inventory growth outpacing revenue (possible demand weakness hidden)
- Operating cash flow consistently below net income (earnings quality eroding)
- Frequent "one-time" or restructuring charges (recurring non-recurring items)
- Capitalization of expenses (R&D, software, customer acquisition) that peers expense
- Changes in accounting policies, auditors, or CFO at suspicious times
- Pension assumptions flattering income
- Related-party transactions disclosed in notes
- Large gap between reported EPS and adjusted/non-GAAP EPS (and what reconciles them)
- Promoter pledging of shares (for Indian stocks — major red flag if high)

**Source**: Annual report notes (don't skip them), auditor's report (qualified opinions are a five-alarm fire), MD&A "Critical Accounting Policies" section.

**Assessment**: Earnings quality is Clean / Aggressive but Defensible / Concerning / Likely Manipulated.

**Pass condition**: Concerning or worse → STOP. Do not proceed regardless of how cheap the stock looks. Report this as the dominant finding and recommend Avoid.

### Step 6: Intrinsic value + Price-Implied Expectations (McKinsey, Buffett, Mauboussin)

**Goal**: Arrive at a defensible value range using your own modeled expectations, *and* decode what the current stock price is already betting on before comparing the two. The gap between market expectations and your expectations is where investment decisions live.

**ALWAYS START WITH METHOD 5 (PIE) BEFORE BUILDING YOUR OWN DCF.** Reading the price first forces you to understand what you are arguing against, prevents confirmation bias in your own model, and surfaces the single assumption that most drives the current valuation.

---

**Method 5 — Price-Implied Expectations (Mauboussin: Expectations Investing)**

*This is the most important addition to the framework. Do this before your own DCF.*

**What it is**: The stock price is not just a number — it is a statement of expectations. Every price embeds a specific view on (a) how fast sales will grow, (b) what operating profit margins the business will sustain, and (c) how much incremental investment the business needs per unit of growth. Back these out from the current EV before forming your own view. If you cannot articulate what the market is betting on, you are not ready to take the other side.

**Step 5a — Identify the three Price-Implied Value Drivers**:
- **Implied sales growth rate**: At the current EV, WACC, and assuming margins stay at current levels — what revenue CAGR over 5–10 years is required to justify the price? Back-solve this from the DCF formula.
- **Implied NOPAT margin**: At the current EV and a plausible growth rate — what operating margin is the market assuming? (NOPAT = Net Operating Profit After Tax = EBIT × (1 − tax rate))
- **Implied investment rate**: Incremental capital (capex + working capital change) as a proportion of incremental revenue. A high investment rate means the company must spend heavily to grow, reducing free cash flow. Back out whether the market is assuming capital-efficient or capital-intensive growth.

**Quick PIE calculation**:
```
EV = NOPAT_0 × (1 + g) / (WACC − g)       [simple perpetuity, use as a first approximation]

Solve for g:  g = (EV × WACC − NOPAT_0) / (EV + NOPAT_0)

Then check: is the implied g plausible given the 10-year history? Is the implied NOPAT margin achievable given the competitive structure from Step 2?
```
For a finite-period model (preferred), use a two-stage reverse DCF: fix WACC, fix terminal growth at 4–5%, fix a 5 or 7-year explicit period, then back-solve for the revenue CAGR and margin that makes DCF = current EV. Most valuation tools (Excel, finbox.com) support this.

**Step 5a-addendum — Physical plausibility check (manufacturing / asset-heavy companies)**: After back-solving the PIE-implied revenue CAGR, overlay it against the capacity model from Step 1c. If the market's implied CAGR requires more volume than announced capacity can deliver, the market is pricing in (a) an unannounced expansion, (b) a step-change realization improvement, or (c) both. State which explicitly — this is often the single most actionable disagreement between the market's assumption and a grounded analysis, and it frequently explains why a stock appears "cheap on multiples" but cannot actually deliver the growth the multiple requires.

**Step 5b — Compare PIE to your own modeled expectations**:
- **Optimistic gap** (PIE > your realistic view): the market is betting on more than you think is achievable — stock is likely to disappoint even if the company performs well. This is the most common condition in bull markets.
- **Pessimistic gap** (PIE < your realistic view): the market is assuming worse-than-realistic outcomes — stock likely to surprise positively even on ordinary execution. This is where genuine value is found.
- **Fairly priced** (PIE ≈ your view): the company must exceed *these specific expectations* to generate alpha above the cost of capital — not just "do well".

**Step 5c — The Expectations Treadmill**: A company that performs *exactly* as the market's PIE implies earns investors *exactly* the cost of capital — no more. For the stock to outperform, the company must beat the price-implied expectations, not just report good results. This is why a company can grow earnings 20% and the stock can still fall — if the price already implied 25% growth. Always ask: "What must happen for this stock to outperform its own PIE?"

**Step 5d — Identify Expectation Triggers**: Expectation triggers are observable, near-term business events that could shift the market's view of one of the three value drivers:
- **Positive triggers**: operating leverage inflecting margins above the implied level; a new large contract reducing the implied growth rate premium to certainty; capacity coming online that de-risks the growth assumption; a competitor exiting the market
- **Negative triggers**: margin miss in the next quarter; a competitor announcement that challenges the growth narrative; working capital deterioration that signals the investment rate is higher than implied; management guidance cut

List 2–3 positive and 2–3 negative triggers specific to this company. These form the basis of the "What would change this view" section of the report.

**Step 5e — Shareholder Value at Risk (SVAR)**:
Calculate the stock price implied by a downside scenario — typically: PIE growth rate cut by 25–30%, margins at the low end of the 10-year range. The difference between current price and the downside scenario price is SVAR. If SVAR is more than 40%, the asymmetry is unfavorable regardless of the upside. If SVAR is less than 15%, the downside is well-cushioned.

---

**Method 1 — DCF (your own forward model)**:
- Now that you know what the price implies (Method 5), build your own model with explicit assumptions that may differ.
- **For manufacturing / asset-heavy companies — run a capacity-constrained revenue build BEFORE setting the CAGR**. Do not anchor revenue growth to historical trend alone; anchor it to what installed + announced capacity can physically deliver:
  1. State installed capacity in physical units (MT / KL / MW / units) and current utilization rate
  2. Back-calculate current realization per unit = current revenue ÷ utilized units; segment by product tier if realization differs materially (e.g. regulated-market premium vs. generic)
  3. For each announced expansion phase: state capacity addition in units, commissioning date, and ramp curve. Standard regulated pharma / specialty chemical ramp: ~25–35% utilization in first 6 months, ~60–70% in the first full year, ~85–90% by Year 2–3. Adjust faster for brownfield (faster ramp, known process) vs. greenfield (slower ramp, regulatory audits needed before first sale)
  4. Build year-by-year revenue as: (existing capacity × utilization × existing realization) + (new capacity × ramp utilization × new realization). Where new capacity serves higher-value markets (regulated, export-to-regulated), apply the documented realization premium rather than blending it away
  5. Identify the **capacity ceiling year**: the year when the combined post-expansion capacity reaches ~88–92% utilization. After this, volume growth stalls unless a further expansion is announced. Build the terminal value to reflect this — do not assume perpetual growth from volume; growth must come from pricing or unannounced capacity
  6. **Physical plausibility check on PIE**: compare the PIE-implied revenue CAGR (from Method 5) against what the capacity model can actually support. If PIE implies more volume than announced capacity allows, the market is pricing in either (a) an unannounced Phase N+1 expansion, or (b) realization improvement beyond what competitive dynamics support. Name which — this is frequently the single most important gap between the market's assumption and a disciplined analysis
  7. **ROCE restoration check**: compute EBIT ÷ capital employed for each forecast year. Identify the year ROCE first crosses WACC again after the expansion. If it is more than 4 years away, the stock should be priced as a recovery play (wide MoS required), not a steady compounder (narrow MoS). If it crosses WACC within 2 years, the expansion is already delivering and a narrower MoS may be appropriate
- Forecast revenue growth for 5–10 years (anchor to the capacity model above, sector cycle from Step 1b, and stock cycle from Step 1c — then fade toward GDP + inflation)
- Operating margin trajectory (anchor to 10-year range; use trough margin as the floor, cycle peak as ceiling; base case should be the mid-cycle average)
- **Operating leverage — model margins against utilization, not just revenue**: For manufacturing and asset-heavy businesses, separate the cost base into fixed (depreciation, maintenance crew, plant overheads, minimum energy load) and variable (raw materials, packaging, variable utilities, freight). Fixed costs are largely set by the installed capacity regardless of how much product runs through it; variable costs scale linearly with volume. At low utilization, fixed costs dominate and margins are depressed; as utilization rises, the fixed cost is spread over more units and margins expand non-linearly. Estimate: (a) what is the approximate fixed-cost base as a % of revenue at full utilization, (b) what does the margin look like at 50%, 70%, and 90% utilization of the new capacity — this directly answers how quickly margins recover as Phase N ramps. This prevents the common error of assuming mid-cycle average margins apply immediately after commissioning.
- **New product pipeline contribution**: For companies that regularly file new product approvals (pharma DMFs/ANDAs/CEPs, specialty chemicals registrations, FMCG launches), model the revenue contribution from the in-flight pipeline *separately* from the capacity model. Inputs needed: (a) number of filings currently in-process and expected approval timeline; (b) historical approval rate from similar filings; (c) estimated revenue per new product in Year 1, Year 2, and steady state (management usually gives the addressable market size per filing in concalls). New product revenue is additive to the capacity-utilization model and can be an important growth source even when the plant is at full utilization — new products at higher realization improve the revenue mix without needing more volume.
- **Debt and interest cost trajectory**: For companies that borrowed to fund the expansion, model three phases explicitly: (a) *peak debt year* — when does total borrowing stop increasing (typically when the last phase of capex completes); (b) *repayment schedule* — most Indian term loans carry 5–7 year tenors; compute annual principal + interest outflow; (c) *interest cost as % of revenue* in each forecast year — this falls meaningfully post-commissioning and is a standalone EPS lever that does not appear in the EBITDA model. Ignoring this understates EPS growth in Years 3–5 for capex-phase companies. Also flag: does the company have floating-rate debt that creates earnings sensitivity to RBI rate decisions?
- Tax rate, working capital changes, maintenance capex
- **Financial leverage as earnings amplifier and downside risk**: Compute the Degree of Financial Leverage (DFL = EBIT ÷ (EBIT − Interest expense)) using both current-year and the peak-debt-year figures. DFL > 2× means a 10% EBIT decline causes a >20% EPS decline — the stock will behave far more violently than the underlying business warrants. For companies that also carry high fixed costs (high operating leverage), compute the Degree of Total Leverage (DTL = DOL × DFL): this is the multiplier by which a revenue shortfall translates into an EPS miss. A DTL of 3× means a 5% revenue miss → ~15% EPS miss — this needs to be reflected in position sizing and MoS width, not just noted as a risk. Forward-project the Interest Coverage Ratio (EBIT ÷ Interest expense) for each year in the forecast period; if ICR is projected to fall below 2.5× in any year, widen the MoS by an additional 5–10 percentage points regardless of moat quality, because sub-2.5× ICR introduces lender-behaviour risk (covenant waiver requests, forced prepayment, margin calls on floating-rate facilities) that is binary and not captured in a DCF.
- **Working capital normalisation and FCF inflection year**: Identify the year in which *both* conditions hold simultaneously: (a) capex drops to maintenance-only levels (growth capex is complete), and (b) working capital as a % of revenue stabilises (the ramp-related WC build is complete and CCC stops expanding). That year is the FCF inflection point — the year free cash flow first materially exceeds net income and the company transitions from a cash consumer to a cash generator. In a two-stage DCF, this is the most important year to get right: if the inflection is in Year 3 vs. Year 6, the intrinsic value difference is often 30–50%. Model CCC forward explicitly for each year; use the trough CCC from the 10-year history as the normalised target.
- Discount rate: CAPM (risk-free + beta × ERP); for India use 10-yr G-sec + 6–7% ERP
- Terminal value: Gordon growth (g = 4–5% for India, 2–3% for US/developed markets)
- Show the explicit assumption differences between your DCF and the PIE — this is your investment thesis in quantified form.

**Method 2 — Owner earnings multiple (Buffett)**:
- Owner earnings = Net income + D&A + non-cash charges − maintenance capex − incremental working capital
- Apply a multiple a rational private buyer would pay (10–20× depending on moat width and growth durability)
- Anchor the multiple to the 10-year average P/E and EV/EBITDA range of the company itself — a stock's own history is the most honest benchmark

**Method 3 — Multiples cross-check**:
- P/E, EV/EBITDA, EV/EBIT, P/B (for financials), EV/Sales (for high-growth)
- Compare to: (a) the company's own 10-year historical range, (b) direct peers, (c) industry average
- If trading at a premium to 10-year history, identify whether the premium is justified by structurally higher future ROIC, or narrative

**Method 4 — Asset-based** (only if relevant — financials, holding companies, distressed):
- Book value, NAV, sum-of-the-parts

**Output**: Value range with low, base, and high estimate. Show key assumptions for each. Explicitly state where your assumptions differ from PIE and why.

**See `references/valuation-methods.md` for detailed mechanics, formulas, and worked examples.**

### Step 7: Margin of safety GATE (Graham, Klarman)

**Goal**: This is the central decision point. Apply the margin of safety discount and check if current price clears it.

**Formula**:
```
Required price = Base intrinsic value × (1 − Margin of safety %)
```

**Margin of safety by quality**:
- Wide moat, predictable: 20-25%
- Narrow moat, moderately predictable: 30-35%
- No moat, cyclical, or speculative: 40-50%+

**Decision**:
- **Current price ≤ Required price** → PASS gate, proceed to Step 8
- **Current price > Required price but ≤ Base intrinsic value** → FAIL gate, but add to watchlist with a "buy below ₹X" target
- **Current price > Base intrinsic value** → FAIL gate decisively; the market has more enthusiasm than the framework supports

**Pass condition**: This gate has teeth. Failing it means the report's verdict cannot be "Strong Setup" no matter how good Steps 1-6 looked. It can be at best "Quality at Wrong Price."

### Step 8: Breakout setup — Weinstein Stage 2 + CANSLIM

**Goal**: Confirm that the technical picture supports an entry now, vs. waiting.

**Weinstein stage check** (weekly chart):
- Stage 1 (basing): price flat, 30-week MA flat → WAIT
- **Stage 2 (advancing): price > 30-week MA, MA sloping up, base breakout on volume → BUY ZONE**
- Stage 3 (topping): MA flattening, volatility up → don't add
- Stage 4 (declining): price < 30-week MA, MA sloping down → AVOID regardless of fundamentals

**Stage 2 entry requirements (Weinstein)**:
1. Clear breakout above multi-week/month resistance (the base top)
2. Breakout volume ≥ 2× recent average weekly volume
3. Broader market in Stage 2 (or at minimum not Stage 4)
4. Positive and rising relative strength vs. the index

**CANSLIM confirmation (O'Neil)** — score how many criteria are met (out of 7):
- **C** — Current quarterly EPS growth ≥ 25% YoY, accelerating
- **A** — Annual EPS growth ≥ 25%/yr over last 3 years; ROE ≥ 17%
- **N** — New product, new management, new highs from a sound base
- **S** — Supply favorable (smaller float, recent buybacks); demand visible (volume on up days)
- **L** — Leader in its industry group (RS rating ≥ 80, ideally 90+)
- **I** — Institutional sponsorship rising over recent quarters
- **M** — Market direction in confirmed uptrend

**Base patterns to recognize**: cup-with-handle, flat base, double bottom, high-tight flag. See `references/breakout-criteria.md` for visual patterns and rules.

**Assessment**: Breakout is Confirmed / Building / Premature / Failed / Not Applicable.

**Pass condition**: For "Strong Setup" verdict, need Stage 2 + at least 5/7 CANSLIM. Otherwise the verdict is "Wait for Technical Confirmation" even if fundamentals are perfect.

### Step 9: Execution criteria

**Goal**: Define exactly what entry, stop-loss, and position sizing look like *before* committing.

**Specify**:
- **Entry trigger**: e.g., "Buy on weekly close above ₹X with volume > 2× avg"
- **Stop-loss**: 7-8% below entry price (O'Neil mechanical rule), OR just below the breakout base (Weinstein structural rule) — use the tighter of the two
- **Position size**: based on max acceptable loss (e.g., risk 1% of portfolio on this position → position size = 1% / stop-loss %)
- **Profit-taking framework**: trailing stop on 30-week MA (Weinstein), or scale-out at predefined intrinsic value (fundamental), or both
- **Re-evaluation triggers**: what events (next earnings, regulatory change, MoS gate violation) prompt a fresh look

## Output: the investment report

After completing all 11 steps, produce a 2-3 page investment report (approximately 1200-1800 words) using the template below. Save it as a Markdown file in the user's `scrip analysis` folder (`C:\Users\AnubhavSingh\.claude\scrip analysis\`) named `[TICKER]_investment_report_[YYYY-MM-DD].md`. Also present the full report in chat.

### Report template

```markdown
# Investment Report: [Company Name] ([TICKER])

**Date**: [YYYY-MM-DD]
**Current price**: [₹/$ amount] (as of [date], source: [exchange])
**Market cap**: [amount]
**Sector / Industry**: [classification]

---

## Verdict

**[STRONG SETUP / QUALITY AT WRONG PRICE / WAIT FOR CONFIRMATION / CHEAP BUT FLAWED / AVOID]**

[1-2 sentence rationale]

| Framework gate | Status |
|---|---|
| Market cycle | ✅/⚠️/❌ [one phrase] |
| Sector cycle | ✅/⚠️/❌ [Tailwind / Neutral / Headwind — one phrase] |
| Stock cycle position | ✅/⚠️/❌ [Early / Mid / Late / Trough — one phrase] |
| Industry structure | ✅/⚠️/❌ [one phrase] |
| Business quality | ✅/⚠️/❌ [one phrase] |
| Financial health | ✅/⚠️/❌ [one phrase] |
| Earnings quality | ✅/⚠️/❌ [one phrase] |
| Valuation (MoS gate) | ✅/⚠️/❌ [one phrase] |
| Technical setup | ✅/⚠️/❌ [one phrase] |

---

## 1. Macro, sector & stock cycle context

**Broad market (Step 1):** [2-3 sentences on market cycle position — index level, VIX, sentiment, sizing implication.]

**Sector cycle (Step 1b):** [2-3 sentences on current sector headwinds/tailwinds — end-market demand, input cost cycle, inventory cycle, regulatory drivers, peer commentary. State: Tailwind / Neutral / Headwind and the primary driver.]

**Stock cycle position (Step 1c):** [2-3 sentences on where this company sits in its own operating cycle — revenue growth trajectory, capacity phase, margin cycle, management guidance trend, order book visibility. State: Early cycle / Mid cycle / Late cycle / Trough and the implication for DCF assumptions and MoS.]

**Industry structure (Step 2):** [3-5 sentences on Porter's 5 Forces with summary table or inline scoring.]

## 2. Business quality

[3-5 sentences identifying the moat (if any), management quality, capital allocation track record, and key qualitative strengths/weaknesses.]

## 3. Financial analysis (10-year view)

[A summary of the full available history — minimum 10 years where data exists — covering revenue, margins, ROIC, cash flow, and balance sheet. The 10-year view reveals what the recent 2-3 year snapshot cannot: full-cycle ROIC, business model shifts, management behaviour under stress, and earnings compounding quality.]

| Metric | FY-9 | FY-8 | FY-7 | FY-6 | FY-5 | FY-4 | FY-3 | FY-2 | FY-1 | Latest |
|---|---|---|---|---|---|---|---|---|---|---|
| Revenue (₹ Cr / $M) | | | | | | | | | | |
| Operating margin (%) | | | | | | | | | | |
| ROCE / ROIC (%) | | | | | | | | | | |
| Net profit (₹ Cr / $M) | | | | | | | | | | |
| OCF (₹ Cr / $M) | | | | | | | | | | |
| Net debt / EBITDA | | | | | | | | | | |

[3-4 sentences interpreting the table. Address: (1) average ROIC vs. CoC across the full cycle; (2) the worst 2-3 years — what caused them and how management responded; (3) any business model pivot visible in the data; (4) what the decade reveals about the company's preparedness for the growth it is now claiming.]

## 4. Earnings quality

[2-4 sentences on red flag check results from Step 5. If clean, say so explicitly with what was verified. If concerns, list them.]

## 5. Valuation

### 5a. Price-Implied Expectations (Mauboussin)

*Read the price before building your own model.*

| PIE Value Driver | Market-implied assumption | Your base-case assumption | Gap |
|---|---|---|---|
| Revenue CAGR (5-yr) | [X]% | [Y]% | [positive / negative] |
| NOPAT margin (steady-state) | [X]% | [Y]% | [positive / negative] |
| Investment rate (capex+WC / ΔRevenue) | [X]% | [Y]% | [positive / negative] |

**Expectations assessment**: [1-2 sentences. Is the market too optimistic, too pessimistic, or fairly priced on each driver? Which driver is the market most wrong about — and why?]

**Expectations treadmill**: [1 sentence. What must the company achieve — specifically — for the stock to outperform its own PIE? E.g., "NOPAT margin must expand above 14% within 3 years; 13.9% merely earns CoC."]

**Expectation triggers (positive)**: [2-3 specific, observable events that could cause the market to revise PIE upward]
**Expectation triggers (negative)**: [2-3 specific, observable events that could cause the market to revise PIE downward]

**Shareholder Value at Risk (SVAR)**: At the downside scenario ([X]% growth, [Y]% margin), implied price = ₹/$ [Z]. SVAR = [W]% from current price. Asymmetry is [Favorable / Neutral / Unfavorable].

### 5b. Intrinsic value (your own model)

[State the value range with methods used and key assumptions. Show where your assumptions explicitly differ from PIE.]

- **DCF base case**: ₹/$ [value] (assumes [X]% revenue CAGR vs. PIE-implied [X2]%, [Y]% NOPAT margin vs. PIE [Y2]%, [Z]% WACC)
- **Owner earnings multiple**: ₹/$ [value] (assumes [N]× multiple on ₹[X] normalized owner earnings; 10-yr avg multiple = [M]×)
- **Peer / historical multiples**: ₹/$ [value] (trading at [X]× EV/EBITDA vs. own 10-yr avg [Y]×, peers [Z]×)

**Value range**: ₹/$ [low] — [base] — [high]
**Current price**: ₹/$ [X], which is [Y]% [discount / premium] to base estimate
**Required price for [Z]% margin of safety**: ₹/$ [W]
**MoS gate**: [PASSED / FAILED]

## 6. Technical setup

[2-4 sentences on Weinstein stage, CANSLIM score, and current chart position. State whether breakout is confirmed, building, or absent.]

## 7. Execution plan (if applicable)

[Only include if verdict is Strong Setup or Wait for Confirmation. State entry trigger, stop-loss, position sizing logic, and re-evaluation triggers.]

## 8. Key risks

[3-5 bullets on the most material risks: business risk, valuation risk, execution risk, regulatory/macro risk. Be specific to this company, not generic.]

## 9. What would change this view

[Specific, observable triggers that would move the verdict in either direction. E.g., "Verdict would upgrade to Strong Setup if Q4 EPS growth confirms acceleration and price clears ₹X on volume. Verdict would downgrade to Avoid if receivables-to-sales ratio exceeds 35% next quarter."]

---

## Sources

- [Annual report FY24, p.X]
- [Q3FY25 concall transcript, date]
- [10-K filed YYYY-MM-DD]
- [Screener.in / NSE / BSE / SEC EDGAR pages accessed]
- [Technical chart data from TradingView / etc.]

---

**Disclaimer**: This report is an analytical exercise applying a fundamental and technical framework synthesized from public investing literature. It is not personalized financial advice. The author of this analysis is not a registered investment advisor. Do your own due diligence; past performance does not predict future results; markets can stay irrational longer than you can stay solvent.
```

## Verdict definitions

Use these five categories — be precise:

- **STRONG SETUP**: All 7 framework gates passed. Business quality good, earnings clean, MoS achieved, breakout confirmed. Highest-conviction signal the framework produces.
- **QUALITY AT WRONG PRICE**: Steps 1-5 pass strongly, but Step 7 (MoS) fails because price is too high. Add to watchlist with a specific buy-below price.
- **WAIT FOR CONFIRMATION**: Fundamentals and valuation pass, but technical breakout (Step 8) hasn't formed yet. Set price alerts for the breakout level.
- **CHEAP BUT FLAWED**: Valuation attractive (Step 7 passes), but Step 3 (quality) or Step 5 (earnings quality) is weak. Deep-value or special-situation play only; not a long-term hold.
- **AVOID**: Step 5 (earnings quality) is concerning, OR multiple gates fail, OR business is in Weinstein Stage 4. Regardless of how exciting the story sounds.

## Important constraints

- **Cite every number**. If you state revenue grew 22%, the report must show the source. Fabrication is the worst possible failure mode of this skill.
- **Note missing data explicitly** rather than guessing. If concall transcripts are paywalled, say so. If the latest quarter isn't yet filed, work with what is available and note the cutoff.
- **Use the most recent available data** — latest annual report, latest quarter, latest concall. Note the as-of date for every key metric.
- **Avoid personalized advice phrasing**. Say "the framework yields X verdict" not "you should buy this stock". The user makes the decision.
- **Be willing to be unfashionable**. If a celebrated stock fails the framework, say so with evidence. If a hated stock passes, say so. The point of a framework is to override narrative.
- **Indian and global context both supported**. Default to Indian sources (BSE/NSE/Screener) if the ticker is Indian or the user mentions India; otherwise default to SEC EDGAR and US sources.

## Reference files

The skill has two kinds of supporting documents — **reference files** explain the *why* and depth of each step; **checklists** give atomic, verifiable line items to confirm nothing was missed.

### References (concepts and depth)

- `references/data-sources.md` — Comprehensive list of authentic data sources (Indian + global) with URLs and what to extract from each
- `references/red-flags.md` — Full O'Glove + Schilit accounting red-flag explanations with examples
- `references/valuation-methods.md` — DCF mechanics, owner earnings formula, reverse DCF, multiples cross-checks, and PIE reverse-engineering mechanics (Mauboussin)
- `references/breakout-criteria.md` — Weinstein stage diagnosis and CANSLIM base patterns with chart-reading rules
- `references/expectations-investing.md` — Mauboussin framework: PIE calculation worked examples, Expectations Gap table, Expectations Treadmill concept, Expectation Trigger taxonomy, SVAR calculation, and the link between expectation revisions and stock price moves

### Checklists (operational, line-by-line verification)

- `checklists/master-analysis-checklist.md` — **Required**. Atomic checkable items across all 9 steps. Use this to confirm completeness before publishing the report.
- `checklists/sector-specific.md` — **Required when applicable**. Sector-specific metrics, ratios, and red flags for banks, NBFCs, IT services, SaaS, pharma, FMCG, manufacturing, commodities, real estate, insurance, and power. Load the relevant section based on the company being analyzed.
- `checklists/pre-buy-final-check.md` — **Required before finalizing verdict**. Behavioral and process discipline checks (thesis falsifiability, inversion test, circle of competence, asymmetry, Buffett's 5-year test). Catches analyst failure modes, not company issues.

### How to use them together

1. Read SKILL.md for the workflow
2. Steps 1, 1b, 1c set the context frame — complete all three before moving to structural analysis. Step 1b (sector cycle) and Step 1c (stock cycle) findings must be explicitly carried forward into Step 6 (DCF assumptions) and Step 7 (MoS width).
3. In Step 4, always pull the full 10-year dataset first. Build the long-run pattern before reading the recent quarters.
4. In Step 6, **always run Method 5 (PIE) before building your own DCF**. The PIE output directly shapes the assumptions you challenge or accept in Methods 1–3.
5. As you execute each step, load the matching reference file for depth (e.g., `red-flags.md` during Step 5; `expectations-investing.md` during Step 6)
6. After completing all 11 steps, run `checklists/master-analysis-checklist.md` to confirm completeness
7. Load `checklists/sector-specific.md` and run the relevant section
8. Run `checklists/pre-buy-final-check.md` before finalizing the verdict
9. Only then publish the report to `C:\Users\AnubhavSingh\.claude\scrip analysis\`

The references give you the *vocabulary*; the checklists enforce the *discipline*. Both are required for a defensible analysis.
