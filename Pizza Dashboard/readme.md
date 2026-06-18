# Pizza Operations & Sales Intelligence Dashboard 🍕📊

## 📝 Project Overview
This project is an advanced, enterprise-grade Business Intelligence and Revenue Analytics system developed to convert raw operational transaction records into real-time strategic insights. Designed with a high-end application style interface, this multi-screen analytical workspace automates core KPI tracking, breaks down sales performance, and visualizes customer ordering behaviors for menu optimization.

The primary goal was to construct a robust analytical platform that eliminates manual metric compilation and empowers management with interactive, data-driven decision capabilities to boost profitability.

---

## 🛠️ Key Technical Features & Implementations

### 1. Robust Data Modeling & Engineering
- **Unified Relational Schema:** Standardized over 48,000+ transactional rows under the Power BI `DataModel` layer, creating clean relationships between orders, dates, pricing, and product attributes.
- **Dynamic DAX Metric Calculations:** Engineered high-performance Data Analysis Expressions (DAX) to calculate operational KPIs on the fly, including Total Revenue ($817.86K), Average Order Value ($38.31), and Average Pizzas Per Order (2.32).

### 2. Multi-Screen Architecture & Interactivity
- **Cross-Filtering Workspace Navigation:** Built a seamless custom sidebar navigation system (`Home` vs `Best/Worst Sellers`) allowing end-users to switch between operational overviews and product performance grids.
- **Advanced Slicer Synchronization:** Configured synchronized dynamic timeline sliders (Jan 15 - Dec 15) and categorical dropdown filters to enable instant visual updates across all charts with a single user selection.

### 3. Executive Visualization & UI Engineering
- **High-Impact Corporate Themes:** Integrated a custom visual profile layout utilizing precise spacing, shadow borders, and cohesive typography designed to eliminate cognitive fatigue.
- **Localized Resource Integration:** Enhanced visual storytelling by mapping custom infographic assets (`pizza-slice.png`, `delivery-man.png`, `profit-growth.png`) directly into structural KPI components and category segments.

---

## 📊 Detailed Screen Structure & Analytical Breakdown

### Screen 1: Home / Macro Operations Panel 

<img width="1440" height="790" alt="Home" src="https://github.com/user-attachments/assets/6b4704d6-cbd1-46bf-a983-bd8ff70f414e" />

*   **Macro Scorecard Matrix:** Displays 5 floating executive KPI blocks outlining absolute metrics (Revenue, AOV, Total Units Sold, Total Orders, and Product Multiplier) for high-level health monitoring.
*   **Temporal Trend Sub-System:**
    *   *Daily Trend (Bar Chart):* Tracks week-day traffic, capturing crucial supply-chain signals where orders maximize on weekends, specifically **Friday (3.5K)** and **Saturday (3.2K) evenings**.
    *   *Monthly Trend (Line/Area Chart):* Maps long-term seasonality, exposing peak sales volumes during **July (1,935 orders)** and **January**, balanced against autumn drops.
*   **Product Segmentation Analytics:** 
    *   *Category Mix (Donut Chart):* Evaluates product share where **Classic Pizza** leads at `26.91%`, closely tracked by Supreme (`25.46%`), Veggie (`23.96%`), and Chicken (`23.68%`).
    *   *Size Breakdown (Donut Chart):* Exposes structural dominance of **Large (L) size pizzas driving 45.89%** of entire business revenue streams.
    *   *Volume Ledger:* A horizontal ranking displaying that the Classic cluster captures maximum operational capacity with **14,888 units distributed**.

### Screen 2: Product Performance & Best/Worst Sellers 

<img width="1440" height="791" alt="best-worst sellers" src="https://github.com/user-attachments/assets/28dfff8c-00b3-4ba9-bf57-81877baa38cb" />

*   **Menu Engineering Side-Panel:** A dedicated automated summary board that instantly pinpoints top-tier velocity variants against absolute bottom-tier menu drains.
*   **Top 5 Performance Quadrants:**
    *   *By Financial Returns:* Identifies **The Thai Chicken Pizza ($43K)** and *The Barbecue Chicken Pizza* as primary gross income pillars.
    *   *By Volume & Orders:* Confirms **The Classic Deluxe Pizza** as the ultimate operational velocity driver (2.5K units across 2.3K distinct orders).
*   **Bottom 5 Vulnerability Quadrants:**
    *   *Menu Laggards:* Isolates **The Brie Carre Pizza** as the single lowest-performing item across all corporate matrix variables, generating minimum revenue ($12K), lowest volume (490 units), and lowest order appearance (480).

---

## 🚫 Exclusions & Strategic Technical Decisions

- **No Rigid Static Elements:** Avoided manual value plotting anywhere on the dashboard canvases. All charts, card metrics, and sidebars are mathematically bound to the underlying transactional dataset.
- **Non-Standard Gridlines Suppressed:** Default chart background lines and canvas grids were systematically removed to create a clean dashboard application wrapper.
- **Actionable Menu Focus Over Overwhelming Metrics:** Avoided cluttering dashboards with excessive raw numbers, selecting 3 core dimension pillars (Revenue, Quantity, Orders) to present highly focused menu-optimization answers.

---

## 🌟 Strategic Benefits & Operational Insights

- **Staffing Optimization:** The system highlights specific weekend and evening hourly rushes, allowing supervisors to schedule delivery drivers and kitchen staff during high-traffic windows.
- **Inventory & Menu Rationalization:** Clearly defines low-margin, slow-moving items like *The Brie Carre* and *The Green Garden Pizza* for deletion or active marketing restructure.
- **Revenue Maximization:** Since Large pizzas drive nearly **46% of returns**, the system provides data backing for multi-buy promotions or upside bundles on Large offerings to protect the core margin.

---

## 🚀 How to Experience the System

1. Clone this repository onto your local system.
2. Review the structured clean transactional matrix via `pizza_sales.xlsx`.
3. Open `Pizza Sales Analytics Dashboard.pbix` inside **Power BI Desktop**.
4. Use the custom sidebar icons to navigate panels and toggle timeline sliders to filter live business operations.
