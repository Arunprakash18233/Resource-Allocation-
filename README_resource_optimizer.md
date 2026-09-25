# Resource Allocation Optimizer

Automatically allocates a team's limited capacity across competing project demand — maximizing fulfillment of high-priority work while never overallocating anyone — using **linear programming**, with the results published to a live, formula-driven Excel/Power BI-ready dashboard.

## Problem

PMO and resourcing teams routinely have more project demand than available capacity, spread across multiple roles (Developers, Testers, BAs, PMs, DevOps). Deciding who works on what — while respecting everyone's hours and prioritizing the right projects — is usually done manually in a spreadsheet, which doesn't scale and rarely finds the *best* allocation, just *an* allocation.

## Approach

This is modeled as a **Linear Program** and solved with [PuLP](https://coin-or.github.io/pulp/) (CBC solver):

- **Decision variable:** hours assigned from each resource to each project's role demand
- **Constraint:** a resource can't be assigned more hours than their weekly capacity
- **Constraint:** a project's role demand can't be over-fulfilled beyond what's needed
- **Matching rule:** a resource can only be assigned to demand matching their role
- **Objective:** maximize total *priority-weighted* hours fulfilled — so High-priority projects get filled first, and any remaining capacity flows to lower-priority work, minimizing idle time

This is the same class of problem used in real workforce/production planning tools — not a heuristic or rule-of-thumb script.

## What's in the Dashboard

- **KPI tiles:** total capacity, total demand, total allocated, overall utilization %, demand fulfillment %
- **Utilization by Resource:** flags underutilized (<50%) and overallocated (>100%) resources
- **Demand Fulfillment by Project:** flags projects fulfilled below 90%
- **Charts:** utilization % by resource, fulfillment % by project

All KPI/table values are **live Excel formulas** (`SUMIF`) referencing the raw allocation output — not hardcoded — so the dashboard recalculates if you re-run the optimizer with new data.

## Tech Stack

- **Python** — orchestration and data generation
- **PuLP** — linear programming solver (CBC backend)
- **pandas** — data handling
- **openpyxl** — Excel dashboard generation, formulas, conditional formatting, charts

## How to Run

```bash
pip install -r requirements.txt
python resource_optimizer.py
```

Outputs:
- `resource_allocation_dashboard.xlsx` — full 4-sheet workbook (Team Capacity, Project Demand, Allocation Results, Dashboard)
- `allocation_results.csv` — the optimizer's output, ready to import into **Power BI** (Get Data → Excel/CSV) for further visualization

## Sample Output

![Dashboard Screenshot](dashboard_screenshot.png)

*(Replace with your own screenshot of the Dashboard tab after running the script.)*

## Why I Built This

Resource forecasting and allocation was a core part of my work managing 22+ projects across German, Dutch, and UK clients — balancing who's available against what each engagement needed. This project turns that manual, judgment-based process into a repeatable, optimization-driven pipeline that scales beyond what's practical to do by hand in Excel.

## About Me

**Arun Prakash** — Data Analyst with 6+ years in PMO and support engagement management, specializing in KPI tracking, resource forecasting, and reporting automation.
[LinkedIn](https://linkedin.com/in/) | aprakash309@gmail.com
