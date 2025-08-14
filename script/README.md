# 📊 KPK Agriculture Dashboard Generator
# Author: Owais Ali Shah
# Description: Generates an interactive HTML dashboard for agricultural expenditure in KPK (2015–2024)

# Load required libraries
library(data.table)
library(dplyr)
library(plotly)
library(DT)
library(htmltools)
library(RColorBrewer)

# 🔹 Load source data
raw_data <- read.csv("long_data_saved.csv", stringsAsFactors = FALSE)

# 🔹 Simulate July data for each year from 2015 to 2024
years <- 2015:2024
data <- do.call(rbind, lapply(years, function(y) {
  raw_data %>%
    mutate(
      Month = sprintf("%04d-07", y),  # Format as "2015-07", "2016-07", etc.
      Year = y
    )
}))

# 🔹 Set fixed filters (can be made dynamic later)
selected_month <- unique(data$Month)[1]
selected_cost_center <- unique(data$Cost.center.Desc)[1]
selected_item <- unique(data$Detail.Item.Desc)[1]

filtered_data <- data %>%
  filter(Month == selected_month,
         Cost.center.Desc == selected_cost_center,
         Detail.Item.Desc == selected_item)

# 🔹 Summary metrics
total_expenditure <- sum(filtered_data$Expenditure, na.rm = TRUE)
average_expenditure <- mean(filtered_data$Expenditure, na.rm = TRUE)

# 🔹 Monthly trends line plot
monthly_summary <- data %>%
  group_by(Month, Sub.Detail.Function) %>%
  summarise(Total = sum(Expenditure, na.rm = TRUE), .groups = "drop")

line_plot <- plot_ly(monthly_summary,
                     x = ~Month,
                     y = ~Total,
                     color = ~Sub.Detail.Function,
                     type = 'scatter',
                     mode = 'lines+markers') %>%
  layout(title = "Monthly Expenditure Trends",
         xaxis = list(title = "Month", tickangle = -45),
         yaxis = list(title = "Total Expenditure"))

# 🔹 Bar plot by function
n_colors <- length(unique(filtered_data$Sub.Detail.Function))
palette <- brewer.pal(max(3, min(n_colors, 8)), "Set2")

expenditure_plot <- plot_ly(filtered_data,
                            x = ~Sub.Detail.Function,
                            y = ~Expenditure,
                            type = 'bar',
                            color = ~Sub.Detail.Function,
                            colors = palette) %>%
  layout(title = "Expenditure by Function",
         xaxis = list(title = "Function"),
         yaxis = list(title = "Expenditure"))

# 🔹 Data table
datatable_html <- datatable(filtered_data, options = list(pageLength = 10))

# 🔹 Timestamp
timestamp <- format(Sys.time(), "%B %d, %Y at %I:%M %p")

# 🔹 Build dashboard layout
full_page <- tagList(
  tags$head(tags$style(HTML("
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #ffffff;
      margin: 40px;
      color: #2c3e50;
    }
    .header {
      text-align: center;
      margin-bottom: 30px;
    }
    h1 {
      font-size: 40px;
      font-weight: 600;
      color: #34495e;
      margin-bottom: 5px;
    }
    h2 {
      font-size: 22px;
      color: #16a085;
      margin-top: 0px;
      font-weight: 400;
    }
    hr {
      border: none;
      border-top: 2px solid #ecf0f1;
      margin: 20px 0;
    }
    .section {
      margin-top: 40px;
    }
    .section h3 {
      font-size: 20px;
      color: #2c3e50;
      margin-bottom: 10px;
    }
    .footer {
      margin-top: 60px;
      font-size: 14px;
      color: #7f8c8d;
      text-align: center;
    }
    .summary-card {
      background-color: #ecf0f1;
      padding: 20px;
      border-radius: 8px;
      flex: 1;
    }
    .summary-value {
      font-size: 24px;
      font-weight: bold;
      margin-top: 10px;
    }
  "))),
  
  # Header
  div(class = "header",
      tags$h1("Agricultural Expenditure Dashboard – Khyber Pakhtunkhwa"),
      tags$h2("Created by Owais Ali Shah – Data from 2015 to 2024"),
      tags$hr()
  ),
  
  # Purpose Statement
  div(class = "section",
      tags$p("This dashboard presents agricultural expenditure trends across Khyber Pakhtunkhwa from 2015 to 2024. It highlights key spending areas and supports planners in making data-driven decisions.", style = "font-size: 16px; color: #555;")
  ),
  
  # Summary Cards
  div(class = "section",
      tags$div(style = "display: flex; gap: 40px;",
        tags$div(class = "summary-card",
                 tags$h3("Total Expenditure"),
                 tags$div(class = "summary-value", format(total_expenditure, big.mark = ","))
        ),
        tags$div(class = "summary-card",
                 tags$h3("Average Expenditure"),
                 tags$div(class = "summary-value", round(average_expenditure))
        )
      )
  ),
  
  # Line Plot
  div(class = "section", line_plot),
  
  # Bar Plot
  div(class = "section", expenditure_plot),
  
  # Data Table
  div(class = "section", datatable_html),
  
  # Footer
  div(class = "footer",
      paste("Generated on", timestamp, "by Owais Ali Shah")
  )
)

# 🔹 Save dashboard
save_html(full_page, "dashboard.html")