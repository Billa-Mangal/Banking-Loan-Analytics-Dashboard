# Banking-Loan-Analytics-Dashboard
An end-to-end Power BI business intelligence project designed to analyze a bank's loan portfolio, monitor overall lending performance, and evaluate risk across various borrower segments. This dashboard helps financial stakeholders make data-driven decisions by classifying loans as "Good" or "Bad" and tracking key risk and repayment metrics.

---

## 📌 Project Overview & Objectives

In the banking industry, financial institutions handle thousands of loan applications. Tracking the health of these loans is critical to maintaining liquidity, minimizing defaults, and optimizing interest yield. 

The primary objectives of this project are:
* **Lending Performance Analysis:** Monitor critical KPIs like Total Loan Applications, Total Funded Amount, Total Cash Received (repayment), Average Interest Rate, and Debt-to-Income (DTI) ratio.
* **Good vs. Bad Loan Segments:** Differentiate between borrowers who repay on time and those who present high default risk. 
* **Borrower Demographics & Geography:** Slice portfolio data by state, grade, purpose, loan term, and home ownership status to uncover hidden lending risks.
* **UX/UI Best Practices:** Use a customized background canvas (designed in PowerPoint as an SVG template) to build a fully interactive, professional executive dashboard with dynamic drill-through and navigation controls.

---

## 🛠️ Technology Stack & Tools
* **BI Tool:** Microsoft Power BI Desktop
* **Data Sources:** Microsoft Excel / CSV portfolio data (38,576 records across 23 columns)
* **ETL & Data Transformation:** Power Query (M Language)
* **Analytical Modeling:** DAX (Data Analysis Expressions) for dynamic measures
* **Layout Design:** Microsoft PowerPoint (Custom SVG Canvas Background)

---

## 📊 Dataset & Schema Dictionary

The portfolio dataset consists of **38,576 borrower records** with 23 data fields. Key attributes include:

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `id` | Whole Number | Unique identifier for each loan application. |
| `issue_date` | Date | The date the loan was officially issued. |
| `loan_amount` | Decimal | The total funded loan amount approved by the bank. |
| `total_payment` | Decimal | The total cumulative amount repaid by the borrower back to the bank. |
| `int_rate` | Percentage | The interest rate charged on the loan. |
| `dti` | Percentage | Debt-to-Income (DTI) ratio, representing the borrower's monthly debt payments divided by monthly income. |
| `loan_status` | Text | High-level status of the loan: `Fully Paid`, `Current`, or `Charged Off`. |
| `grade` | Text | Loan risk grade assigned by the bank (A = Lowest risk, G = Highest risk). |
| `purpose` | Text | Borrower's stated reason for the loan (e.g., Debt Consolidation, Home Improvement). |
| `home_ownership`| Text | Borrower's residential status (e.g., Rent, Mortgage, Own). |
| `address_state` | Text | Geographic state of the borrower (US-based). |

---

## ⚙️ ETL & Data Transformation (Power Query)

Data cleaning and structural alignment were conducted using **Power Query Editor**:

1. **Text Standardization:** 
   The `purpose` column had inconsistent casing (lower, upper, mixed cases). A text transformation was applied to improve visual alignment:
   * **Action:** Right-click `purpose` $\rightarrow$ `Transform` $\rightarrow$ `Capitalize Each Word`.
2. **Geographical Categorization:**
   To ensure accurate map renderings in Power BI, the geographical field was explicitly mapped:
   * **Action:** Selected the `address_state` column $\rightarrow$ `Data Category` set to **State** (instead of Uncategorized). This successfully maps records to the US geographical context.
3. **Lending Classification (Good vs. Bad Loans):**
   There was no default classification column. Since banks classify loans by risk, a **Conditional Column** named `Good_vs_Bad` was created based on the `loan_status` field:
   * **Logic:**
     * **IF** `loan_status` = `"Fully Paid"` $\rightarrow$ **Good**
     * **ELSE IF** `loan_status` = `"Current"` $\rightarrow$ **Good**
     * **ELSE** $\rightarrow$ **Bad** *(Represents "Charged Off" defaulted loans)*

---

## 🧮 DAX Measures & KPIs

Advanced **DAX Measures** were written to power the dynamic cards, donor charts, and historical trends:

### 1. General Portfolio KPIs
* **Total Loan Applications:**
  ```dax
  Total Loan Applications = COUNT(financial_loan[id])
  ```
* **Total Funded Amount:**
  ```dax
  Total Funded Amount = SUM(financial_loan[loan_amount])
  ```
* **Total Amount Received (Repayments):**
  ```dax
  Total Amount Received = SUM(financial_loan[total_payment])
  ```
* **Average Interest Rate:**
  ```dax
  Average Interest Rate = AVERAGE(financial_loan[int_rate])
  ```
* **Average DTI Ratio:**
  ```dax
  Average DTI Ratio = AVERAGE(financial_loan[dti])
  ```

### 2. Good Loan Segment Measures
* **Good Loan Applications:**
  ```dax
  Good Loan Applications = CALCULATE([Total Loan Applications], financial_loan[Good_vs_Bad] = "Good")
  ```
* **Good Loan Percentage:**
  ```dax
  Good Loan Percentage = DIVIDE([Good Loan Applications], [Total Loan Applications], 0)
  ```
* **Good Loan Funded Amount:**
  ```dax
  Good Loan Funded Amount = CALCULATE([Total Funded Amount], financial_loan[Good_vs_Bad] = "Good")
  ```
* **Good Loan Amount Received:**
  ```dax
  Good Loan Amount Received = CALCULATE([Total Amount Received], financial_loan[Good_vs_Bad] = "Good")
  ```

### 3. Bad Loan Segment Measures (Default Risk)
* **Bad Loan Applications:**
  ```dax
  Bad Loan Applications = CALCULATE([Total Loan Applications], financial_loan[Good_vs_Bad] = "Bad")
  ```
* **Bad Loan Percentage:**
  ```dax
  Bad Loan Percentage = DIVIDE([Bad Loan Applications], [Total Loan Applications], 0)
  ```
* **Bad Loan Funded Amount:**
  ```dax
  Bad Loan Funded Amount = CALCULATE([Total Funded Amount], financial_loan[Good_vs_Bad] = "Bad")
  ```
* **Bad Loan Amount Received:**
  ```dax
  Bad Loan Amount Received = CALCULATE([Total Amount Received], financial_loan[Good_vs_Bad] = "Bad")
  ```

---

## 🎨 Professional Canvas Layout & PowerPoint Template

To achieve a modern, containerized dark-theme look, a bespoke layout was drafted in **Microsoft PowerPoint** and imported as an SVG (Scalable Vector Graphic) background. This technique prevents layout shift, keeps visuals light, and ensures standard pixel-perfect alignment.

* **Slide/Canvas Dimensions:** Full HD 16:9 (`1920 x 1080` pixels)
* **Background Hex Color:** Dark Navy/Grey (`#0F1015` or similar)
* **Visual Container Hex Color:** Soft Dark (`#161922` or similar)
* **Border Highlights:** Low-contrast borders (`#2C303E` or similar)
* **Process:**
  1. Shapes with rounded corners were drawn in PowerPoint to create container cards for KPIs and visual graphs.
  2. The layout was exported: `File` $\rightarrow$ `Save As` $\rightarrow$ Select **Scalable Vector Graphics (*.svg)** format.
  3. In Power BI, under the **Formatting Pane** $\rightarrow$ `Canvas Background` $\rightarrow$ Added the exported SVG background and set `Image Fit` to **Fill** and `Transparency` to **0%**.

---

## 🖥️ Dashboard Architecture & Visual Design

The workspace is structured across two high-density interactive pages:

### Page 1: Summary Page (Executive Overview)
This page is designed for high-level risk monitoring, comparing Good Loans vs. Bad Loans, and evaluating performance trends:
* **KPI Header Cards:** Includes individual cards with built-in financial icons representing Total Loan Applications, Funded Amount, Received Amount, Avg Interest, and Avg DTI.
* **Good vs. Bad Segment Breakdown:**
  * **Donut Charts & Multi-row KPIs** displaying the exact split: Good Loans represent **86.18%** of applications ($33,243$ loans, $\$370.2\text{M}$ funded, $\$435.8\text{M}$ received). Bad Loans represent **13.82%** of applications ($5,333$ loans, $\$65.5\text{M}$ funded, $\$37.3\text{M}$ received).
* **Funded Amount vs. Repayment (Clustered Column Chart):** Slices total funded capital side-by-side with repayment received across the three primary loan statuses (`Fully Paid`, `Current`, `Charged Off`).
* **Portfolio Metrics Tables:** Small-multiples comparing application volumes, average interest rates, and average DTI ratios across loan classifications.

### Page 2: Overview Page (Strategic Analysis)
This page acts as a multidimensional breakdown to allow department heads to dissect historical trends and structural portfolio risks:
* **Monthly Loan Applications (Line Chart):** Charts application volumes over time (Month-on-Month) to isolate seasonal credit expansion patterns.
* **Loan Applications by Term (Donut Chart):** Breaks down borrower term selection (36 Months vs. 60 Months). Shows that **73.2%** of borrowers opt for 3-year maturities.
* **Home Ownership Status (Horizontal Bar Chart):** Utilizes a logarithmic scale to properly represent borrower categories like Rent, Mortgage, and Own side-by-side without visual truncation.
* **Loan Purpose Breakdowns (Treemap):** Highlights the volume of loans across borrowing categories. Shows that **Debt Consolidation** represents the single largest purpose for taking out loans.
* **Regional Analysis Map (Azure Map Integration):** Highlights geographically where credit is concentrated across the United States, utilizing custom gold-colored bubble sizing matching the application volumes of each state.

---

## 🎛️ Interactive Elements & Navigation Features

The dashboard implements native Power BI features to optimize self-service analytics:
1. **Slicers:** Includes an dynamic list slicer for loan **Purpose** (14 standardized options) and a horizontal button slicer for credit **Grade** (A to G).
2. **Page Navigator:** Seamlessly toggle between **Summary** and **Overview** views using formatted rounded button indicators.
3. **Single-Click Filter Reset:** A custom-styled button mapped to the action **"Clear All Slicers"** allows users to immediately clear active filters with `Ctrl + Click` in Power BI desktop.
4. **Interactive Highlighting:** Selecting any data bar or slice in one chart automatically filters all related visuals in real-time across the canvas.

---

## 🚀 How to Run and Open the Project

1. Install **Power BI Desktop** (latest version recommended).
2. Clone this repository to your local directory:
   ```bash
   git clone https://github.com/your-username/bank-loan-analytics-dashboard.git
   ```
3. Open the `Bank_Loan_Analytics_Dashboard.pbix` file.
4. If prompt for the data source path:
   * Open **Power Query Editor** $\rightarrow$ Navigate to `Source` under Applied Steps.
   * Edit the path to point to your local copy of the `Financial_Loan_Data.csv` or `.xlsx` file.
   * Click **Close & Apply** to reload.

---

## 📂 Repository Structure
```
├── README.md                      <- Project documentation (You are here)
├── Bank_Loan_Analytics_Dashboard.pbix <- Power BI file containing reports, DAX, and modeling
├── Dataset/
│   └── Financial_Loan_Data.xlsx   <- Raw and cleaned financial data
└── Templates/
    ├── Dashboard_Layout.svg       <- Canvas background for Summary page
    └── Overview_Layout.svg        <- Canvas background for Overview page
```

---

*Project built by **[Your Name]** as part of a Finance Domain Analytics Portfolio. For any queries, feel free to reach out via GitHub or LinkedIn!*
