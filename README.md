# SupeRR Selector Season 2 (Round 1)
## Most Reliable Players Under Pressure: Methodology, Rankings & Rajasthan Royals Fit (IPL 2027 Auction)

**Researcher:** Anushya S  
**Registration No.:** 711524BBT005  
**Target Event:** IPL 2027 Auction Intelligence & Squad Planning  
**Franchise:** Rajasthan Royals (RR)  

---

## 1. Executive Summary

This repository delivers an empirical, ball-level win-probability modeling framework and an interactive executive analytics dashboard to identify the most dependable T20 players under pressure.

* **Dataset Scale:** 199,029 deliveries across 842 matches with a result (IPL, global T20 leagues, and T20Is from 2022 to 2026).
* **Pressure Definition:** High-leverage deliveries where one ball can swing match win probability the most (top 25% of each innings by WP swing).
* **Rating Engine:** **Pressure Reliability Rating (PRR)**, scaled so **50.0 represents a league-average performer**.
* **Core Findings:**
  * **Top Batters:** Rashid Khan (67.6), Virat Kohli (62.6), Dinesh Karthik (57.2).
  * **Top Bowlers:** Ravindra Jadeja (83.3), Rashid Khan (74.0), Haider Ali (72.8).
  * **Dual-Threat Phenom:** Rashid Khan ranks #1 as a batter (SR 171 under pressure) and #2 as a bowler (economy drops to 6.63).
  * **RR Strategic Synergy:** Ravindra Jadeja (#1 Bowler, PRR 83.3) was already acquired by RR via the trade window for IPL 2026.

---

## 2. Interactive Analytics Dashboard

The project includes a standalone web dashboard styled in Rajasthan Royals' signature aesthetic (`index.html`).

### Key Modules:
1. **Executive Leaderboards:** Full interactive rankings for all rated batters and bowlers with confidence filters (High/Medium/Low), search, and expandable deep-dive drawers.
2. **90% Confidence Interval & Risk Forest Plot:** Visualizes 300 match-level bootstrap resamples to differentiate low-variance anchors (Kohli, Jadeja) from volatile ceilings (Rashid bat, Samad).
3. **Head-to-Head Duel Engine:** Side-by-side dropdown comparator calculating PRR differentials, evidence gaps, and distribution overlaps.
4. **Rajasthan Royals 2027 Auction War Room:** Target tier categorization, overseas quota tracking (max 4 in playing XI), and live squad scenario simulator.
5. **PRR Leverage & Formula Engine:** Visual comparison of the 4.3x leverage swing multiplier, ROC curves (1st Innings AUC 0.67 vs 2nd Innings AUC 0.85), and interactive component weight simulator.
6. **Reproducibility & Statistical Diagnostics:** Full documentation of bootstrap tests, sensitivity checks, split-half tests, explicit limitations, and copyable Python code.

---

## 3. How Pressure is Defined

### Win Probability (WP) Models
Two gradient-boosted models were trained with 5-fold cross-validation grouped by match (`GroupKFold`), preventing data leakage:
* **1st Innings Model (Setting):** Features include runs, wickets lost, balls remaining, current run rate, and T20I indicator. (Out-of-sample AUC: 0.67, Brier: 0.231).
* **2nd Innings Model (Chasing):** Features include target, runs needed, balls remaining, required run rate, wickets lost, and revised targets. (Out-of-sample AUC: 0.85, Brier: 0.163).

### Leverage & Pressure Balls
Leverage is computed on every delivery:
$$\text{Swing} = WP(\text{after a six}) - WP(\text{after a wicket})$$

Deliveries in the **top 25% of swing** within each innings are classified as **Pressure Balls**.

| Diagnostic Metric | Pressure Balls (Top 25%) | Ordinary Balls (75%) | Empirical Finding |
| :--- | :---: | :---: | :--- |
| **Mean swing in WP (one ball)** | **0.267** | **0.062** | **4.3x more swing per delivery** |
| **Closeness $\|WP - 0.5\|$** | **0.150** | **0.270** | Contests are substantially closer |
| **Mean $\|WP\text{ change}\|$ per ball** | **0.041** | **0.015** | **2.7x greater volatility** |
| **Chase: mean required rate** | **9.45** | **11.85** | Ordinary includes lost causes |
| **League batting strike rate** | **137.6** | **143.9** | **-6.3 points drop (scoring is harder)** |
| **League bowling economy** | **8.48** | **8.85** | **-0.37 tighter bowling execution** |

---

## 4. Performance Measurement & PRR Formula

Batters receive the win probability added on balls faced (byes and leg byes excluded). Bowlers receive the negative of it on all deliveries (including wides and no-balls).

$$PRR = 50 + 10 \times z$$
$$z = 0.5 \, z_A + 0.3 \, z_B + 0.2 \, z_C$$

* **A (Impact - 50%):** $\frac{\sum WP_{\text{added}}}{\text{pressure balls} + 100}$ (Empirical Bayesian shrinkage towards league average).
* **B (Reliability - 30%):** Share of matches ($\ge 4$ bat / $6$ bowl pressure balls) with positive net win probability added, shrunk with a weight of 8 matches.
* **C (Pressure vs. Own Baseline - 20%):** Difference in WP added per unit leverage under pressure vs. ordinary balls, shrunk similarly.
* Standardized against ~150 league players with at least 100 pressure balls in that discipline.

---

## 5. Final Rankings

### Top Batters (Min 60 Pressure Balls)
| Rank | Batter | PRR (90% CI) | Balls / Matches | SR: Press vs Normal | Out Rate: Press vs Normal | Rank Range | Confidence |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | **Rashid Khan** | 67.6 (28–98) | 150 / 13 | 171 vs 153 (+18) | 8.0% vs 9.8% (-1.8%) | 1–9 | High |
| 2 | **Virat Kohli** | 62.6 (48–78) | 729 / 54 | 149 vs 146 (+3) | 3.4% vs 3.2% (+0.2%) | 1–6 | High |
| 3 | **Dinesh Karthik** | 57.2 (29–85) | 161 / 18 | 181 vs 167 (+14) | 5.0% vs 6.2% (-1.2%) | 1–9 | High |
| 4 | **MS Dhoni** | 54.3 (22–91) | 99 / 11 | 162 vs 150 (+12) | 7.1% vs 4.8% (+2.3%) | 1–10 | High |
| 5 | **Abdul Samad** | 53.7 (17–93) | 94 / 11 | 176 vs 149 (+27) | 9.6% vs 7.0% (+2.6%) | 1–10 | Medium |
| 6 | **Faf du Plessis** | 51.9 (34–68) | 364 / 33 | 135 vs 153 (-18) | 3.3% vs 4.7% (-1.4%) | 2–9 | Medium |
| 7 | **Wriddhiman Saha** | 44.9 (24–63) | 144 / 18 | 107 vs 135 (-28) | 5.6% vs 5.0% (+0.6%) | 3–10 | High |
| 8 | **Shikhar Dhawan** | 44.4 (27–62) | 172 / 18 | 125 vs 136 (-11) | 2.9% vs 3.7% (-0.8%) | 4–10 | Medium |
| 9 | **Rohit Sharma** | 40.5 (21–59) | 346 / 40 | 135 vs 149 (-14) | 5.8% vs 5.2% (+0.6%) | 4–10 | High |
| 10 | **Ravindra Jadeja** | 39.9 (21–57) | 243 / 22 | 147 vs 135 (+12) | 3.7% vs 4.8% (-1.1%) | 4–10 | High |

### Top Bowlers (Min 10 Pressure Balls)
| Rank | Bowler | PRR (90% CI) | Balls / Matches | Econ: Press vs Normal | Wkt Rate: Press vs Normal | Rank Range | Confidence |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | **Ravindra Jadeja** | 83.3 (67–99) | 270 / 22 | 8.47 vs 7.86 (+0.61) | 5.6% vs 4.0% (+1.6%) | 1–4 | High |
| 2 | **Rashid Khan** | 74.0 (62–90) | 607 / 44 | 6.63 vs 7.76 (-1.13) | 5.9% vs 5.7% (+0.2%) | 1–6 | High |
| 3 | **Haider Ali** (28756) | 72.8 (49–92) | 76 / 7 | 6.79 vs 6.34 (+0.45) | 6.6% vs 5.2% (+1.4%) | 1–8 | Medium |
| 4 | **Ishant Sharma** | 67.2 (53–83) | 123 / 11 | 8.73 vs 10.00 (-1.27) | 6.5% vs 6.0% (+0.5%) | 2–8 | Medium |
| 5 | **Muhammad Farooq** | 65.8 (54–73) | 30 / 2 | 10.00 vs 6.55 (+3.45) | 13.3% vs 6.1% (+7.2%) | 2–7 | Low |
| 6 | **Piyush Chawla** | 57.7 (40–73) | 129 / 12 | 7.81 vs 8.54 (-0.73) | 7.0% vs 6.0% (+1.0%) | 3–10 | Medium |
| 7 | **Basil Hameed** (8638) | 54.9 (38–68) | 33 / 3 | 9.45 vs 8.17 (+1.28) | 9.1% vs 3.8% (+5.3%) | 4–10 | Low |
| 8 | **Amit Mishra** | 54.1 (47–64) | 31 / 3 | 8.71 vs 7.96 (+0.75) | 6.5% vs 6.3% (+0.2%) | 5–9 | Low |
| 9 | **Dwayne Bravo** | 47.9 (32–66) | 27 / 2 | 13.56 vs 7.88 (+5.68) | 11.1% vs 7.7% (+3.4%) | 4–10 | Low |
| 10 | **Pradeep Sangwan** | 44.9 (40–50) | 11 / 1 | 7.64 vs 7.26 (+0.38) | 0.0% vs 7.0% (-7.0%) | 8–10 | Low |

*Note: Nathan Bracken has 0 deliveries in the 2022–2026 dataset and is excluded.*

---

## 6. Strategic Fit for Rajasthan Royals (IPL 2027)

* **Context:** RR finished 4th in IPL 2026. Ravindra Jadeja joined RR via the trade window prior to 2026. Retention deadline: 17 November; Auction: 16 December 2026.
* **The Dual-Threat Marquee Pick:** **Rashid Khan** offers unrivaled two-way leverage value (#1 Bat, #2 Bowl). Worth exhausting an overseas quota slot.
* **The Jadeja Polarity:** Jadeja is the premier clutch bowler (#1, PRR 83.3, A = +4.4), but the weakest pressure batter (#10, PRR 39.9, A = -2.2). RR must deploy him as a pure bowling anchor.
* **The Chase Anchor:** **Virat Kohli** (PRR 62.6, 729 balls, 1–6 range) provides the highest dependability floor across world cricket.
* **Domestic Finishers:** **Abdul Samad** (PRR 53.7, SR 176 under pressure) serves as a cost-effective domestic death power option.

---

## 7. Statistical Integrity & Reproducibility

* **Bootstrap Resampling:** 300 iterations grouped by match produce the reported 90% confidence intervals.
* **Threshold Sensitivity:** Varying the cutoff to top 30% or top 10% yields high rank correlations ($\rho = 0.99$ and $0.87$ for bowlers; $0.84$ and $0.71$ for batters).
* **Split-Half Reliability:** Odd vs. even match IDs yield raw $\rho = 0.11$ (bat) and $0.15$ (bowl), adjusting via Spearman-Brown to $0.20$ and $0.26$. This confirms substantial inherent variance in small T20 samples and justifies rigorous Bayesian shrinkage.

---

## 8. Quick Start

To view the dashboard locally:
```bash
# Clone this repository
git clone https://github.com/CurzliZ/RR_perf.git
cd RR_perf

# Open the dashboard directly in your browser
start index.html  # On Windows
open index.html   # On macOS
xdg-open index.html # On Linux
```
