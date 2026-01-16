# SA-CCR Exposure Modeling for Interest Rate Derivatives

## Overview
This project implements the **Standardized Approach for Counterparty Credit Risk (SA-CCR)** to assess counterparty exposure for an **interest rate derivatives portfolio as of 12/31/2023**, using an **Excel-based regulatory framework**.

The analysis covers **28+ interest rate derivative trades and SA-CCR risk positions** across **USD, JPY, and AUD**, with a **total notional exposure of $5.41 billion**, and follows Basel III SA-CCR specifications to compute **Adjusted Notional, Replacement Cost (RC), Potential Future Exposure (PFE), and Exposure at Default (EAD)**.

All calculations and results are derived directly from the file  
**`SA_CCR_Erya Jain.xlsx`**.

---

## File Structure
- **`SA_CCR_Erya Jain.xlsx`**  
  Excel-based SA-CCR implementation containing trade-level calculations, hedging set aggregation, and netting-set PFE results.

---

## Portfolio Summary (from Excel Model)

- **Asset Class:** Interest Rate Derivatives  
- **As-of Date:** 12/31/2023  
- **Total Notional:** **$5.41B**  
- **Currencies:** USD, JPY, AUD  
- **Instruments Covered:**  
  - Interest Rate Swaps (IRS)  
  - Forward Rate Agreements (FRAs)  
  - Swaptions  
  - Caps/Floors  
  - Basis Swaps  

Although the portfolio contains 18 primary trade rows, several instruments (options and basis products) decompose into multiple **SA-CCR risk positions across hedging subsets**, resulting in **28+ trade-level risk positions** evaluated under the framework.

---

## Netting and Margin Structure

- **Netting Sets:** 3  
  - **ABC123 – 1101:** Margined  
  - **XYZ789 – 1102:** Margined  
  - **DEF123 – 1103:** Unmargined  

- **Hedging Sets:** 9 currency-level and basis hedging sets  
  (USD, JPY, AUD, and basis-specific hedging sets)

This structure allows direct comparison of **margined vs. unmargined exposure profiles** under SA-CCR.

---

## Methodology (Implemented in Excel)

### 1. Adjusted Notional
For each trade:

Adjusted Notional = Base Notional × Supervisory Duration

Supervisory Duration:
SD = max(e^(−0.05 × S) − e^(−0.05 × E), 0.05)

where:
- **S** = Start date  
- **E** = End date  

---

### 2. Trade-Level Effective Notional
Each trade’s exposure is adjusted using:
- Supervisory Delta (including option delta treatment)
- Maturity Factor
- Buy/Sell direction

This produces **trade-level effective notionals**, which are aggregated by **hedging subset**.

---

### 3. Hedging Set Aggregation
Trade-level effective notionals are aggregated within each hedging set and scaled by **Basel III supervisory factors** to obtain **hedging-set effective notionals**.

---

### 4. Potential Future Exposure (PFE)
PFE is computed at the **netting set level**:
PFE = Multiplier × AddOn_Aggregate


The SA-CCR multiplier is applied using net market value and collateral inputs where applicable.

---

## Key Results (from `SA_CCR_Erya Jain.xlsx`)

### Netting-Set PFE Add-Ons
| Counterparty | Netting Set | PFE Add-On |
|--------------|------------|------------|
| ABC123 | 1101 | $178,537,780 |
| DEF123 | 1103 | $459,417,045 |
| XYZ789 | 1102 | $33,152,737 |

**Total PFE Add-On:** **~$671 million**

---

## Risk Insights

- The **unmargined JPY netting set (DEF123 – 1103)** contributes the largest share of PFE, illustrating the capital impact of missing collateral agreements.
- **Margined netting sets** (ABC123 and XYZ789) exhibit materially lower exposure due to collateral and netting benefits.
- Netting across hedging subsets significantly reduces aggregate exposure compared to gross trade-level notionals.

---

## Conclusion
This project demonstrates a **practical SA-CCR implementation consistent with Basel III regulatory standards**, using Excel in a format commonly employed within bank risk and regulatory teams.

The framework:
- Quantifies counterparty credit exposure for a large IR derivatives portfolio
- Highlights the importance of **margining and netting** in reducing regulatory capital
- Provides transparent, auditable calculations suitable for **CCR, XVA, and regulatory capital analysis**

All figures and conclusions are **fully supported by calculations in `SA_CCR_Erya Jain.xlsx`**.

---

## Usage
1. Open `SA_CCR_Erya Jain.xlsx`
2. Review trade-level SA-CCR inputs and calculations
3. Inspect hedging-set aggregation and netting-set PFE results
4. Use the framework for scenario or stress analysis as needed

---

## References
- Basel III: Standardized Approach for Counterparty Credit Risk (SA-CCR)
- Basel III Endgame Proposal
