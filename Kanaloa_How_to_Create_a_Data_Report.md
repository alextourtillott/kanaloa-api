# How to Create a Data Report

As a marine expert, your field time is precious. Whether you need to analyze Autonomous Underwater Vehicle (AUV) telemetry, verify coral growth metrics, or export site health data for a grant application -- Kanaloa makes compiling this information seamless.

This guide shows you how to transform raw oceanic and robotic data into a polished, actionable PDF or CSV report with a few clicks.

### Goal

Extract aggregated site data, e.g. sensor logs, drone path efficiencies, and biological growth tracking into one shareable report for your team or applicable stakeholders.

---

## Before You Begin

Make sure you have:

* A registered Kanaloa account with **Viewer** access or higher.
* The **Site ID** or name of the specific ocean region you're monitoring.

---

## Step-by-Step Instructions

### Step 1: Select Your Active Restoration Site

1. Log into your **Kanaloa Dashboard**.
2. Click on **Restoration Sites** in the left navigation.
3. Select your target site from the list, e.g. *Great Barrier Reef - Sector Delta*.

### Step 2: Choose Your Data Modules

Kanaloa reports are modular. You only build what you need.

1. Click the **Generate Report** button in the top-right corner of the site overview page.
2. Under **Report Modules**, check the boxes next to the data suites to include:
* **Biological Analytics:** Coral growth metrics, target species density, and health indicators.
* **Environmental Telemetry:** Water temperature, pH levels, salinity, and dissolved oxygen trends.
* **Robotics & Operations:** AUV dive paths, battery efficiency, automated seeding logs, and coverage map validation.

### Step 3: Define Your Timeframe

1. Locate the **Date Range** dropdown menu.
2. Select a preset timeframe, *Last 24 Hours*, *Last 30 Days*, *Current Expedition* or choose **Custom Range** to select specific calendar dates on the timeline.

### Step 4: Configure Smart Predictive Insights (Optional)

Because Kanaloa utilizes an adaptive API, you can include predictive models in your report.

1. Toggle the switch labeled **Include Smart Forecasting**.
2. Select a forecast horizon, e.g. *3-Month Outlook* or *6-Month Outlook*. Kanaloa will auto-generate an environmental projection based on the historical data of your selected timeframe.

### Step 5: Generate and Export

1. Click the blue **Preview Report** button at the bottom of the page to review your data visualizations and layout.
2. Select your preferred **Export Format**:
* **PDF:** Ideal for sharing, printing, and stakeholder presentations.
* **CSV ZIP archive:** Ideal if you want the raw sensor tabular data for custom Python/R scripts.

3. Click **Download Report**.

Your file will process and automatically download to your (local machine) device.

---

## Quick Troubleshooting

* **Missing Marine Robotic Data?** If your AUV telemetry isn't appearing in the modules list, ensure your deployment drone has successfully completed its end-of-day data sync. Check the **Devices** tab in the sidebar to verify the device status.
* **Report Generation Timing Out?** If you're pulling more than six months of high-frequency sensor data, the file may take up to a minute to compile. If it fails, select the **Send to Email** option, and Kanaloa will process it in the background and mail you a link when it's complete.