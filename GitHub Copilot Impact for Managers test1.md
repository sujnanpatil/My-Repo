# Guided Lab: Measuring GitHub Copilot Impact for Managers

**Duration:** 60 minutes  
**Lab Type:** Guided Hands-on  
**Audience:** Engineering / Delivery Managers  

---

## Lab Overview

In this lab, you will learn how to measure and evaluate the impact of GitHub Copilot in your organization. You will enable usage reporting, analyze adoption metrics, and calculate productivity improvements using GitHub data exports and visualization tools such as Power BI or Excel. The goal is to help managers interpret developer usage data and turn insights into actionable strategies for improving adoption and ROI.

---

## Lab Objectives

In this lab, you will complete the following tasks:

- Enable and access GitHub Copilot usage reports.  
- Analyze developer adoption metrics.  
- Calculate productivity improvement ratios.  
- Build and present an executive dashboard summarizing results.  

---

## Exercise 1: Enable and Access Copilot Usage Reports (15 minutes)

### Task 1.1: Activate Reporting Features

1. Sign in to your **GitHub Enterprise Cloud** account with **admin privileges**.  
2. In the top-right corner, click on your **organization avatar → Settings**.  
3. Navigate to **Copilot → Usage Reports**.  
4. Enable the toggle for **“Usage Reports”** to start data collection for your organization.  
5. Verify that your organization or team admins have the required permissions to access usage metrics.  

**Reference:**  
[Reviewing user activity data for Copilot](https://docs.github.com/en/copilot/business/collecting-and-reviewing-copilot-activity-data)  

![Mock Screenshot – Copilot Usage Report Settings](img/task1-enable-usage-report.png)  

---

### Task 1.2: Download Usage Data

1. In the **GitHub Admin Console**, go to **Copilot → Usage Reports**.  
2. Under **Report Type**, select **Copilot Usage per Developer**.  
3. Click **Export CSV** to download the data.  
4. Open the CSV file in **Excel**, **Power BI**, or **Google Sheets**.  
5. Review key columns such as:  
   - Developer Email  
   - Suggestions Accepted  
   - Completions Generated  
   - Active Days  

![Mock Screenshot – Copilot Usage CSV in Excel](img/task1-download-csv.png)  

---

## Exercise 2: Analyze Developer Adoption Metrics (15 minutes)

### Task 2.1: Import Data into Analytics Tool

1. Open **Excel**, **Power BI**, or **Google Sheets**.  
2. Import the **Copilot Usage CSV** file.  
3. Perform **data cleaning**:
   - Remove duplicate entries.  
   - Ensure consistent developer names or team tags.  
   - Format columns appropriately.  

![Mock Screenshot – Power BI Import Dialog](img/task2-import-powerbi.png)  

---

### Task 2.2: Visualize Adoption Trends

1. Create a **bar chart** or **line graph** showing **Suggestions Accepted per Week**.  
2. Add **filters** to view adoption by **Team**, **Department**, or **Region**.  
3. Identify:
   - **High adopters** (frequent users).  
   - **Low adopters** (minimal usage).  

**Tip:** Use color coding in Power BI or conditional formatting in Excel to highlight outliers.  

![Mock Screenshot – Adoption Trend Chart](img/task2-adoption-chart.png)  

---

## Exercise 3: Calculate Productivity Improvements (15 minutes)

### Task 3.1: Establish Baseline Metrics

1. Identify **pre-Copilot** developer metrics such as:  
   - Pull Request (PR) throughput (PRs per month).  
   - Average PR size.  
   - Review turnaround time.  
2. Retrieve baseline data from **GitHub Insights**, **CI/CD dashboards**, or **historical project reports**.  

![Mock Screenshot – GitHub Insights PR Metrics](img/task3-baseline-metrics.png)  

---

### Task 3.2: Compute Improvement Ratios

Use the formula:  

**Example:**  
Pre-Copilot PRs per month = 40  
Post-Copilot PRs per month = 52  

**Result:** 30% productivity improvement.  

**Reference:**  
[GitHub REST API – Get Copilot metrics for an organization](https://docs.github.com/en/rest/copilot/copilot-usage#get-copilot-metrics-for-an-organization)  

![Mock Screenshot – Excel Productivity Formula](img/task3-formula.png)  

---

## Exercise 4: Report and Drive Adoption (15 minutes)

### Task 4.1: Build an Executive Dashboard

1. In **Power BI** or **Excel**, create a new dashboard combining:
   - Usage metrics (adoption trends).  
   - Productivity gains (computed ratios).  
   - Team-level comparisons.  
2. Add key insights:
   - Estimated time saved per Copilot suggestion.  
   - ROI estimate (based on developer time saved vs. Copilot license cost).  

**Reference:**  
[Quantifying GitHub Copilot’s impact on developer productivity](https://github.blog/news-insights/research/research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/)  

![Mock Screenshot – Power BI Dashboard](img/task4-dashboard.png)  

---

### Task 4.2: Present Findings and Next Steps

1. Create an **executive summary slide deck** with:
   - Productivity improvements.  
   - Top-performing teams.  
   - Recommendations for low-adoption teams.  
2. Share your dashboard with leadership through **Microsoft Teams**, **SharePoint**, or internal reports.  
3. Suggest next steps, such as:
   - Conducting Copilot best practices workshops.  
   - Expanding licenses to additional teams.  

![Mock Screenshot – Summary Presentation Slide](img/task4-summary-slide.png)  

---

## Lab Summary

In this guided lab, you successfully:

- Enabled and accessed GitHub Copilot usage reports.  
- Analyzed developer adoption and engagement metrics.  
- Calculated productivity improvements.  
- Built and presented an executive summary dashboard.  

You now understand how to **measure**, **visualize**, and **communicate** GitHub Copilot’s impact within your organization and use data to drive adoption and ROI improvements.  

---

**End of Lab**
