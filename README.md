# SA-CCR Exposure Modeling for Interest Rate Derivatives

## Overview
This project implements the **Standardized Approach for Counterparty Credit Risk (SA-CCR)** to assess counterparty exposure for an **interest rate derivatives portfolio as of 12/31/2023**, using an **Excel-based regulatory framework**.

The analysis covers **28+ interest rate derivative trades and SA-CCR risk positions** across **USD, JPY, and AUD**, with a **total notional exposure of $5.41 billion**, and follows Basel III SA-CCR specifications to compute **Adjusted Notional, Replacement Cost (RC), Potential Future Exposure (PFE), and Exposure at Default (EAD)**.

All calculations and results are derived directly from the file  
**`SA_CCR_Erya Jain.xlsx`**.

---

## File Structure
- **`SA_CCR_Erya Jain.xlsx`**  
  Excel-based SA-CCR implementation containing trade-level calculations, hedging-set aggregation, and netting-set PFE results.

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

Although the portfolio contains 18 primary trade rows, several option and basis instruments decompose into multiple **SA-CCR risk positions across hedging subsets**, resulting in **28+ trade-level risk positions** evaluated under the framework.

---

## Netting and Margin Structure

- **Netting Sets:** 3  
  - **ABC123 – 1101:** Margined  
  - **XYZ789 – 1102:** Margined  
  - **DEF123 – 1103:** Unmargined  

- **Hedging Sets:** 9 currency-level and basis hedging sets  
  (USD, JPY, AUD, and basis-specific hedging sets)

This structure enables direct comparison of **margined versus unmargined exposure profiles** under SA-CCR.

---

## SA-CCR Methodology and Formulas (Implemented in Excel)

### 1. Time Definitions
Mi = Year fraction from As-of Date to Maturity Date
Si = Year fraction from As-of Date to Start Date
Ei = Year fraction from As-of Date to End Date
Ti = Year fraction from As-of Date to Exercise Date


---

### 2. Supervisory Duration (Interest Rate)
SD = max( e^(−0.05 × S) − e^(−0.05 × E), 0.05 )

where:
- S = Start date (in years)
- E = End date (in years)

---

### 3. Adjusted Notional
Adjusted Notional (dᵢ) = Base Notional × SD


---

### 4. Maturity Factor (MF)

**Margined trades**
MF = √( MPOR / 250 )

**Unmargined trades**
MF = √( min(Mi, 1) )

where:
- MPOR = Margin Period of Risk (business days)

---

### 5. Supervisory Option Volatility
σ = 0.5

---

### 6. Option Delta (Swaptions, Caps/Floors)
d = [ ln(Pi / Ki) + 0.5 × σ² × Ti ] / ( σ × √Ti )
δ = N(d)
where:
- Pi = Underlying price  
- Ki = Strike price  
- Ti = Time to exercise  
- N(·) = Standard normal CDF  

**Put/Call adjustment**
- Call: +δ  
- Put: −δ  

---

### 7. Supervisory Delta
Δᵢ = Buy/Sell Indicator × Option Delta
where:
- Buy = +1  
- Sell = −1  

---

### 8. Trade-Level Effective Notional
ENᵢ = Adjusted Notional × MF × Δᵢ

---

### 9. Hedging Set Aggregation
EN_HS = | Σ ENᵢ |
Aggregation is performed by:
- Counterparty ID  
- Netting Set ID  
- Hedging Set  
- Hedging Subset (1 / 2 / 3)

---

### 10. PFE Add-On
AddOn = Σ ( Supervisory Factor × EN_HS )
Supervisory factors:
- 0.005 for standard IR hedging sets  
- 0.0025 for basis hedging sets  

---

### 11. Multiplier
Multiplier = min( 1, 0.05 + 0.95 × exp( −(V − C) / (1.9 × AddOn) ) )
where:
- V = Netting-set market value  
- C = Netting-set collateral value  

---

### 12. Potential Future Exposure (PFE)
PFE = Multiplier × AddOn

---

### 13. Replacement Cost (RC)
RC = max( 0, V − C + MTA + TH − NICA )
where:
- MTA = Minimum Transfer Amount  
- TH = Threshold Amount  
- NICA = Net Independent Collateral Amount  

---

### 14. Exposure at Default (EAD)
EAD = α × ( RC + PFE )
where:
- α = 1.4 (Basel III regulatory multiplier)

---

## Key Results (from `SA_CCR_Erya Jain.xlsx`)

### Netting-Set PFE Add-Ons
| Counterparty | Netting Set | PFE Add-On |
|--------------|-------------|------------|
| ABC123 | 1101 | $178,537,780 |
| DEF123 | 1103 | $459,417,045 |
| XYZ789 | 1102 | $33,152,737 |

**Total PFE Add-On:** **~$671 million**

---

## Risk Insights

- The **unmargined JPY netting set (DEF123 – 1103)** contributes the largest share of PFE, highlighting the capital impact of missing collateral agreements.
- **Margined netting sets** exhibit materially lower exposure due to collateralization and netting benefits.
- Netting across hedging subsets significantly reduces aggregate exposure relative to gross trade-level notionals.

---

## Conclusion
This project demonstrates a **practical SA-CCR implementation aligned with Basel III regulatory standards**, using Excel in a format commonly employed within bank risk and regulatory teams.

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

