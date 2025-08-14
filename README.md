📥 How to View the Dashboard
⚠️ To view the dashboard correctly, please download the entire repository, not just the dashboard.html file.
The dashboard relies on supporting files and dependencies that are generated alongside the HTML.
Without these, the dashboard may appear blank or broken.

✅ Recommended Steps:
- Click the green “Code” button and select “Download ZIP”

# 🌾 KPK Agriculture Expenditure Dashboard (2015–2024)

This repository contains a fully interactive, multi-year dashboard visualizing agricultural expenditures across **Khyber Pakhtunkhwa (KPK)** from **2015 to 2024**. Built in **R** using `plotly`, `DT`, and `htmltools`, the dashboard is designed to support planners, analysts, and community stakeholders in making data-driven decisions.

---

## 📊 Features

- **Multi-year analysis**: Simulated July data across 10 years (2015–2024)
- **Interactive plots**: Monthly trends and function-wise expenditure breakdown
- **Summary cards**: Key metrics like total and average expenditure
- **Clean layout**: Modern styling with purpose statement and footer
- **Portable output**: Generates a standalone `dashboard.html` file
- **Locally relevant**: Built with authentic cost center and item names for KPK

---

## 🛠️ Technologies Used

- **R** (base + tidyverse)
- `data.table`, `dplyr` – data wrangling  
- `plotly` – interactive visualizations  
- `DT` – searchable, paginated data tables  
- `htmltools` – custom HTML layout and styling  
- `RColorBrewer` – accessible color palette

---

## 📁 Files

| File               | Description                                                  |
|--------------------|--------------------------------------------------------------|
| `long_data_saved.csv` | Source data (simulated July entries for each year)         |
| `dashboard.R`      | Main R script to generate the dashboard                      |
| `dashboard.html`   | Final output – interactive dashboard (auto-generated)        |
| `README.md`        | This file                                                    |

---

## 🚀 How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/econ-owais/-kpk-agriculture-dashboard-2015-2024
