# Power BI Sales Dashboard Template Guide
## AdventureWorks 2022 Sales Analytics

---

## 📋 Table of Contents
1. [Dashboard Overview](#dashboard-overview)
2. [Page 1: Executive Summary](#page-1-executive-summary)
3. [Page 2: Product Performance](#page-2-product-performance)
4. [Page 3: Customer Insights](#page-3-customer-insights)
5. [Page 4: Sales Team Performance](#page-4-sales-team-performance)
6. [Page 5: Time Analysis](#page-5-time-analysis)
7. [Design Specifications](#design-specifications)
8. [Interactive Elements](#interactive-elements)

---

## Dashboard Overview

### Purpose
This dashboard provides comprehensive sales analytics for AdventureWorks, enabling stakeholders to:
- Monitor key performance indicators (KPIs)
- Track revenue trends and growth
- Analyze product and customer performance
- Evaluate sales team effectiveness
- Identify opportunities for improvement

### Data Source
- **Source**: AdventureWorks 2022 Database
- **Fact Tables**: Sales SalesOrderHeader, Sales SalesOrderDetail
- **Dimension Tables**: Production Product, Sales Customer, Sales Territory, DateDim
- **Refresh Frequency**: Daily (recommended)

### Key Metrics Available
- Total Revenue, Orders, Customers
- Gross Profit & Margin %
- YoY/YTD Growth
- Average Order Value
- Product & Territory Performance

---

## Page 1: Executive Summary

### Layout Design
```
┌─────────────────────────────────────────────────────────────────┐
│  SALES DASHBOARD - EXECUTIVE SUMMARY          [Date Slicer]     │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │  TOTAL   │  │  TOTAL   │  │ AVERAGE  │  │  GROSS   │       │
│  │ REVENUE  │  │  ORDERS  │  │  ORDER   │  │ PROFIT   │       │
│  │          │  │          │  │  VALUE   │  │ MARGIN % │       │
│  │ R 10.5M  │  │  25,432  │  │  R 413   │  │  38.5%   │       │
│  │ ▲ 12.3%  │  │ ▲ 8.5%   │  │ ▲ 3.6%   │  │ ▼ 1.2%   │       │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Revenue Trend (Line Chart)              Revenue by Category   │
│  ┌────────────────────────────┐          ┌──────────────────┐ │
│  │ Shows: Monthly Revenue     │          │ Donut Chart:     │ │
│  │ Lines: Current Year        │          │ - Bikes          │ │
│  │        Prior Year          │          │ - Components     │ │
│  │        3-Month Avg         │          │ - Clothing       │ │
│  │                            │          │ - Accessories    │ │
│  │ X-Axis: Month              │          │                  │ │
│  │ Y-Axis: Revenue            │          │ Shows: % and R   │ │
│  └────────────────────────────┘          └──────────────────┘ │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Top 5 Products (Table)                  Sales by Territory    │
│  ┌────────────────────────────┐          ┌──────────────────┐ │
│  │ Product    │ Revenue │ Rank│          │ Map Visual or    │ │
│  │──────────────────────────  │          │ Bar Chart:       │ │
│  │ Product A  │ R 850K  │  1  │          │ - North America  │ │
│  │ Product B  │ R 720K  │  2  │          │ - Europe         │ │
│  │ Product C  │ R 680K  │  3  │          │ - Pacific        │ │
│  │ ...                        │          │                  │ │
│  └────────────────────────────┘          └──────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Visual Specifications

#### KPI Cards (4 cards)
**Card 1: Total Revenue**
- Measure: `[Total Revenue]`
- Subtitle: `[Growth Indicator]`
- Format: Currency, no decimals
- Conditional formatting: Green if positive growth, Red if negative

**Card 2: Total Orders**
- Measure: `[Total Orders]`
- Subtitle: YoY change
- Format: Whole number with comma separator

**Card 3: Average Order Value**
- Measure: `[Average Order Value]`
- Subtitle: vs Prior Year
- Format: Currency, no decimals

**Card 4: Gross Profit Margin %**
- Measure: `[Gross Profit Margin %]`
- Subtitle: YoY change
- Format: Percentage, 1 decimal
- Conditional formatting: 
  - Green: > 35%
  - Amber: 25-35%
  - Red: < 25%

#### Line Chart: Revenue Trend
- **X-Axis**: DateDim[Year-Month] (sorted by DateDim[Date])
- **Y-Axis**: `[Total Revenue]`
- **Lines**: 
  - Current Year: `[Total Revenue]`
  - Prior Year: `[Revenue PY]`
  - Trend: `[3-Month Moving Avg]`
- **Legend**: Positioned at top
- **Data Labels**: Off (too cluttered)
- **Tooltip**: Custom tooltip showing detailed breakdown

#### Donut Chart: Revenue by Category
- **Legend**: Production ProductCategory[Name]
- **Values**: `[Total Revenue]`
- **Detail Labels**: Category, Percentage, Revenue
- **Colors**: Use consistent brand colors per category

#### Table: Top 5 Products
- **Columns**:
  1. Production Product[Name]
  2. `[Total Revenue]` (with data bars)
  3. `[Product Rank]`
  4. `[Gross Profit Margin %]`
- **Filter**: Top 5 by `[Total Revenue]`
- **Conditional Formatting**: Color scale on margin

#### Map/Bar Chart: Sales by Territory
- **Values**: `[Total Revenue]`
- **Legend**: Sales SalesTerritory[Name]
- **Option 1**: Filled Map (if geographic coordinates available)
- **Option 2**: Horizontal Bar Chart (simpler, clearer)

### Slicers
- **Year Slicer**: DateDim[Year] - Dropdown style
- **Territory Slicer**: Sales SalesTerritory[Name] - Dropdown style
- Position: Top right corner

---

## Page 2: Product Performance

### Layout Design
```
┌─────────────────────────────────────────────────────────────────┐
│  PRODUCT PERFORMANCE ANALYSIS                 [Filters Panel]   │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │ PRODUCTS │  │  TOTAL   │  │ AVERAGE  │  │ AVERAGE  │       │
│  │   SOLD   │  │ QUANTITY │  │   UNIT   │  │ DISCOUNT │       │
│  │   324    │  │  89,456  │  │  PRICE   │  │   12%    │       │
│  └──────────┘  └──────────┘  │  R 456   │  └──────────┘       │
│                                └──────────┘                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Product Performance Matrix (Table)                              │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Product      │ Category │ Revenue │ Qty │ Margin │ Rank │  │
│  │────────────────────────────────────────────────────────  │  │
│  │ Road-650     │ Bikes    │ R 850K  │ 425 │ 42.3%  │  1   │  │
│  │ Mountain-200 │ Bikes    │ R 720K  │ 380 │ 38.9%  │  2   │  │
│  │ ...          │          │         │     │        │      │  │
│  │                                                            │  │
│  │ [Show top 20 products, sortable columns]                  │  │
│  │ [Conditional formatting: Data bars on Revenue]            │  │
│  │ [Color scale on Margin %: Red < 20%, Green > 35%]        │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Treemap: Category & Subcategory        Scatter: Price vs Vol  │
│  ┌────────────────────────────┐          ┌──────────────────┐ │
│  │ Hierarchical view of:      │          │ X: Unit Price    │ │
│  │ - Product Category         │          │ Y: Quantity Sold │ │
│  │   - Product Subcategory    │          │ Size: Revenue    │ │
│  │                            │          │ Legend: Category │ │
│  │ Size by: Revenue           │          │                  │ │
│  │ Color by: Category         │          │ Identify:        │ │
│  │                            │          │ - High Vol/Low $ │ │
│  └────────────────────────────┘          │ - Low Vol/High $ │ │
│                                           └──────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Visual Specifications

#### Product Performance Table
- **Columns**:
  1. Production Product[Name]
  2. Production ProductCategory[Name]
  3. `[Total Revenue]` - with data bars
  4. `[Total Quantity Sold]`
  5. `[Gross Profit Margin %]` - color scale
  6. `[Product Rank]`
- **Sort**: Default by Revenue descending
- **Top N Filter**: Show top 20 products
- **Drill-through**: Enable to product detail page

#### Treemap Visual
- **Group**: 
  - Level 1: Production ProductCategory[Name]
  - Level 2: Production ProductSubcategory[Name]
- **Values**: `[Total Revenue]`
- **Tooltips**: Show Revenue, Quantity, Margin %
- **Colors**: By category

#### Scatter Chart
- **X-Axis**: Production Product[ListPrice]
- **Y-Axis**: `[Total Quantity Sold]`
- **Size**: `[Total Revenue]`
- **Legend**: Production ProductCategory[Name]
- **Purpose**: Identify pricing opportunities

### Slicers
- **Category**: Production ProductCategory[Name]
- **Subcategory**: Production ProductSubcategory[Name]
- **Price Range**: Production Product[ListPrice] (between slicer)

---

## Page 3: Customer Insights

### Layout Design
```
┌─────────────────────────────────────────────────────────────────┐
│  CUSTOMER ANALYTICS                           [Date Filter]     │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                     │
│  │  TOTAL   │  │ REVENUE  │  │ AVERAGE  │                     │
│  │CUSTOMERS │  │   PER    │  │  ORDERS  │                     │
│  │  19,820  │  │ CUSTOMER │  │   PER    │                     │
│  │ ▲ 5.2%   │  │  R 530   │  │ CUSTOMER │                     │
│  └──────────┘  └──────────┘  │   1.28   │                     │
│                                └──────────┘                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Customer Trend (Line)                   Customer Distribution  │
│  ┌────────────────────────────┐          ┌──────────────────┐ │
│  │ Monthly new vs returning   │          │ By Territory:    │ │
│  │ customers                  │          │ Stacked Bar or   │ │
│  │                            │          │ Column Chart     │ │
│  │ Legend:                    │          │                  │ │
│  │ - New Customers            │          │ Shows customer   │ │
│  │ - Returning Customers      │          │ count by region  │ │
│  │ - Total                    │          │                  │ │
│  └────────────────────────────┘          └──────────────────┘ │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Top 10 Customers (Table)                Revenue by Territory  │
│  ┌────────────────────────────┐          ┌──────────────────┐ │
│  │ Account    │ Revenue│Orders│          │ Matrix showing:  │ │
│  │────────────────────────────│          │ Rows: Territory  │ │
│  │ AW00011000 │ R 150K │  45 │          │ Cols: Year       │ │
│  │ AW00011001 │ R 142K │  38 │          │ Values: Revenue  │ │
│  │ ...                        │          │                  │ │
│  │ [Drill through enabled]    │          │ YoY comparison   │ │
│  └────────────────────────────┘          └──────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Visual Specifications

#### Customer KPI Cards
1. **Total Customers**: `[Total Customers]`
2. **Revenue per Customer**: `[Revenue per Customer]`
3. **Average Orders per Customer**: Calculate as `[Total Orders] / [Total Customers]`

#### Top Customers Table
- **Columns**:
  1. Sales SalesOrderHeader[AccountNumber]
  2. `[Total Revenue]` - data bars
  3. `[Total Orders]`
  4. `[Average Order Value]`
- **Filter**: Top 10 by Revenue
- **Drill-through**: To customer detail page

#### Territory Matrix
- **Rows**: Sales SalesTerritory[Name]
- **Columns**: DateDim[Year]
- **Values**: `[Total Revenue]`
- **Show**: YoY change as indicators

---

## Page 4: Sales Team Performance

### Layout Design
```
┌─────────────────────────────────────────────────────────────────┐
│  SALES TEAM PERFORMANCE                   [Territory Filter]    │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                     │
│  │  ACTIVE  │  │ REVENUE  │  │  ACTIVE  │                     │
│  │  SALES   │  │   PER    │  │TERRITORY │                     │
│  │  PEOPLE  │  │SALESPERSON│  │          │                     │
│  │    17    │  │ R 617K   │  │    10    │                     │
│  └──────────┘  └──────────┘  └──────────┘                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Salesperson Leaderboard (Table)                                │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Salesperson │ Territory │ Revenue │ Orders │ AOV │ Margin│  │
│  │──────────────────────────────────────────────────────────│  │
│  │ Linda M.    │ Northwest │ R 980K  │  1,245 │ R787│ 39.2% │  │
│  │ Jillian C.  │ Southwest │ R 925K  │  1,189 │ R778│ 38.5% │  │
│  │ Michael B.  │ Northeast │ R 890K  │  1,156 │ R770│ 37.8% │  │
│  │ ...                                                        │  │
│  │                                                            │  │
│  │ [Sortable by any column]                                  │  │
│  │ [Conditional formatting on Revenue and Margin]            │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Revenue by Salesperson (Column)     Territory Performance     │
│  ┌────────────────────────────┐      ┌───────────────────────┐│
│  │ Clustered column chart     │      │ Stacked bar showing:  ││
│  │ X: Salesperson             │      │ Revenue by Territory  ││
│  │ Y: Revenue                 │      │                       ││
│  │ Legend: Current vs Target  │      │ Compare to target or  ││
│  │        (if available)      │      │ prior year            ││
│  │                            │      │                       ││
│  │ Shows top 10 performers    │      │                       ││
│  └────────────────────────────┘      └───────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

### Visual Specifications

#### Salesperson Leaderboard
- **Columns**:
  1. Person Person[FirstName] & [LastName]
  2. Sales SalesTerritory[Name]
  3. `[Total Revenue]` - with data bars
  4. `[Total Orders]`
  5. `[Average Order Value]`
  6. `[Gross Profit Margin %]` - color scale
- **Sort**: Revenue descending
- **Conditional Formatting**:
  - Revenue: Data bars
  - Margin: Color scale (Red < 25%, Green > 35%)

---

## Page 5: Time Analysis

### Layout Design
```
┌─────────────────────────────────────────────────────────────────┐
│  TIME SERIES ANALYSIS                     [Date Range Slicer]  │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │   YTD    │  │   MTD    │  │   YTD    │  │SELECTED  │       │
│  │ REVENUE  │  │ REVENUE  │  │  GROWTH  │  │ PERIOD   │       │
│  │ R 8.2M   │  │ R 850K   │  │  15.8%   │  │ Jun 2014 │       │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Running Total Revenue (Area Chart)                             │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ X-Axis: Month                                            │  │
│  │ Y-Axis: Running Total Revenue                            │  │
│  │ Lines: Current Year, Prior Year                          │  │
│  │                                                           │  │
│  │ Shows cumulative revenue build-up through the year       │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Monthly Performance (Combo Chart)   │   YoY Growth by Month   │
│  ┌──────────────────────────────────┐│  ┌────────────────────┐│
│  │ Columns: Current Year Revenue    ││  │ Column chart of:   ││
│  │ Line: Prior Year Revenue         ││  │ Monthly YoY Growth ││
│  │                                  ││  │                    ││
│  │ X-Axis: Month                    ││  │ Color coding:      ││
│  │ Y-Axis: Revenue                  ││  │ Green: Positive    ││
│  │                                  ││  │ Red: Negative      ││
│  │ Shows clear comparison           ││  │                    ││
│  └──────────────────────────────────┘│  └────────────────────┘│
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Performance Table (Matrix)                                     │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │ Rows: Year-Month                                         │  │
│  │ Values: Revenue, Orders, AOV, Margin %, YoY Growth %     │  │
│  │                                                           │  │
│  │ [Conditional formatting throughout]                      │  │
│  │ [Drill-down capability: Year > Quarter > Month]          │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Design Specifications

### Color Scheme
```
Primary Colors:
- Blue:   #0078D4 (Revenue, Primary actions)
- Green:  #107C10 (Profit, Positive indicators)
- Red:    #D13438 (Costs, Negative indicators)
- Orange: #FF8C00 (Warnings, Attention items)

Neutral Colors:
- Dark:   #323130 (Headers, Primary text)
- Medium: #605E5C (Secondary text)
- Light:  #F3F2F1 (Backgrounds)
- White:  #FFFFFF (Card backgrounds)

Category Colors (Consistent across all pages):
- Bikes:       #0078D4 (Blue)
- Components:  #107C10 (Green)
- Clothing:    #FF8C00 (Orange)
- Accessories: #8764B8 (Purple)
```

### Typography
- **Headers**: Segoe UI Semibold, 14pt, #323130
- **KPI Values**: Segoe UI Bold, 28pt, #323130
- **KPI Labels**: Segoe UI Regular, 11pt, #605E5C
- **Body Text**: Segoe UI Regular, 10pt, #323130
- **Table Headers**: Segoe UI Semibold, 10pt, #605E5C

### Visual Styling
- **Cards**: 
  - White background (#FFFFFF)
  - Light gray border (1px, #E1DFDD)
  - Subtle shadow (2px offset, 10% opacity)
  - 8px border radius

- **Charts**:
  - Clean axes (no gridlines unless necessary)
  - Subtle background (#F3F2F1)
  - Clear legends (top or right position)
  - Consistent tooltips format

- **Tables**:
  - Alternating row colors (#FFFFFF / #FAF9F8)
  - Header background (#F3F2F1)
  - Bold column headers
  - Right-align numbers
  - Left-align text

### Spacing & Layout
- **Page Margins**: 20px all sides
- **Visual Spacing**: 15px between visuals
- **Card Spacing**: 10px between KPI cards
- **Header Height**: 60px
- **Slicer Panel Width**: 200px (right side)

---

## Interactive Elements

### Slicers Configuration

#### Date Range Slicer
- Type: Between
- Field: DateDim[Date]
- Position: Top right
- Style: Dropdown
- Default: Last 12 months

#### Year Slicer
- Type: Dropdown
- Field: DateDim[Year]
- Position: Top right
- Allow multiple selections: Yes

#### Territory Slicer
- Type: Dropdown
- Field: Sales SalesTerritory[Name]
- Position: Top right or left sidebar
- Allow multiple selections: Yes

#### Category Slicer (Product pages)
- Type: Dropdown
- Field: Production ProductCategory[Name]
- Position: Left sidebar
- Allow multiple selections: Yes

### Drill-through Pages

#### Product Detail Page
**Triggered from**: Any product visual
**Shows**:
- Product specifications
- Sales history chart
- Top customers for this product
- Profit trend
- Inventory status (if available)

#### Customer Detail Page
**Triggered from**: Customer tables/charts
**Shows**:
- Customer information
- Order history
- Revenue trend
- Product preferences
- Territory information

#### Territory Detail Page
**Triggered from**: Territory visuals
**Shows**:
- Territory performance over time
- Sales team in territory
- Top products in territory
- Customer breakdown

### Bookmarks

#### Executive View
- Shows only KPI cards and main charts
- Hides detailed tables
- Clean, presentation-ready view

#### Detailed View
- Shows all visuals including tables
- Expanded charts
- Analysis-ready view

#### Current vs Prior Year
- Side-by-side comparison mode
- Highlights YoY changes
- Focus on growth metrics

### Tooltips

#### Custom Tooltip Page
Create a dedicated tooltip page showing:
- Mini trend sparkline
- Key metrics summary
- Comparison indicators
- Context-sensitive details

Apply to:
- Revenue charts
- Product visuals
- Territory maps
- Customer charts

---

## Best Practices for Implementation

### Performance Optimization
1. Use star schema relationships
2. Hide unused columns/tables
3. Use measures instead of calculated columns where possible
4. Limit visual count per page (max 8-10)
5. Use aggregations for large datasets
6. Import mode preferred over DirectQuery

### User Experience
1. Consistent navigation across pages
2. Clear page titles and descriptions
3. Intuitive slicer placement
4. Logical visual grouping
5. Progressive detail (summary → drill-down)
6. Mobile-friendly layouts

### Data Freshness
1. Set up scheduled refresh
2. Display last refresh timestamp
3. Add data quality indicators
4. Handle missing/null values gracefully
5. Document data sources

### Accessibility
1. High contrast color combinations
2. Clear, readable fonts (minimum 10pt)
3. Descriptive alt text for visuals
4. Keyboard navigation support
5. Screen reader compatibility

---

## Measures Quick Reference

### Core Metrics
- `[Total Revenue]` - Sum of all sales
- `[Total Orders]` - Count of distinct orders
- `[Average Order Value]` - Revenue ÷ Orders
- `[Total Quantity Sold]` - Sum of quantities
- `[Total Customers]` - Count of distinct customers

### Profitability
- `[Total Cost]` - Cost of goods sold
- `[Gross Profit]` - Revenue - Cost
- `[Gross Profit Margin %]` - Profit ÷ Revenue
- `[Total Freight]` - Shipping costs
- `[Total Tax]` - Tax collected

### Time Intelligence
- `[Revenue PY]` - Prior year revenue
- `[YoY Growth %]` - Year-over-year growth
- `[MTD Revenue]` - Month-to-date
- `[YTD Revenue]` - Year-to-date
- `[YTD Growth %]` - YTD comparison
- `[Running Total Revenue]` - Cumulative
- `[3-Month Moving Avg]` - Smoothed trend

### Advanced Analytics
- `[Product Rank]` - Dynamic ranking
- `[% of Total Revenue]` - Contribution %
- `[Top 10 Products Revenue]` - Top performers only
- `[Margin Category]` - Performance classification
- `[Growth Indicator]` - Text with arrows

### Sales Channel
- `[Online Orders]` - Digital sales
- `[Offline Orders]` - Traditional sales
- `[Online Order %]` - Digital penetration

### Sales Team
- `[Active Salespeople]` - Count of sellers
- `[Revenue per Salesperson]` - Productivity
- `[Active Territories]` - Geographic coverage

---

## Tips for Portfolio Presentation

### Documentation
1. Create a one-page summary document
2. List key insights discovered
3. Highlight technical skills demonstrated
4. Document data modeling approach
5. Explain design decisions

### Showcase Elements
1. Screenshot each page
2. Create an animated GIF of interactions
3. Record a 2-minute video walkthrough
4. List DAX formulas used
5. Highlight advanced features (drill-through, bookmarks, etc.)

### GitHub/Portfolio Site
```
/AdventureWorks-Dashboard
  /screenshots
    - executive-summary.png
    - product-performance.png
    - customer-insights.png
    - sales-team.png
    - time-analysis.png
  /documentation
    - README.md (this guide)
    - insights.md
    - technical-notes.md
  /files
    - dashboard.pbix
    - measures-list.txt
  - demo-video.gif
```

### Key Insights to Highlight
1. **Revenue Growth**: "Identified 15.8% YoY revenue growth"
2. **Product Performance**: "Top 10 products contribute 68% of total revenue"
3. **Customer Behavior**: "Average customer value increased by 8.5%"
4. **Territory Analysis**: "Northwest territory shows strongest growth"
5. **Channel Mix**: "Online orders increased from 45% to 52%"

---

## Maintenance & Updates

### Regular Reviews
- Weekly: Check data refresh status
- Monthly: Review KPIs and update targets
- Quarterly: Assess dashboard usage and gather feedback
- Annually: Major refresh and redesign if needed

### Version Control
- Maintain backup copies
- Document major changes
- Track measure modifications
- Keep changelog

### Feedback Loop
- Collect user feedback
- Monitor usage patterns
- Identify pain points
- Iterate and improve

---

## Troubleshooting Common Issues

### Performance Issues
- Too many visuals → Split into multiple pages
- Slow refresh → Optimize model, add aggregations
- Large file size → Remove unused columns, optimize data types

### Data Issues
- Missing values → Add error handling in measures
- Incorrect totals → Check relationship directions
- Duplicate rows → Review data sources and keys

### Visual Issues
- Cluttered charts → Simplify, use Top N filters
- Misaligned elements → Use alignment guides
- Inconsistent colors → Create color theme, apply consistently

---

## Next Steps

1. **Build the Dashboard**: Follow this template page by page
2. **Customize**: Adapt to your specific requirements
3. **Test**: Verify all interactions and calculations
4. **Optimize**: Improve performance and user experience
5. **Document**: Create your portfolio documentation
6. **Present**: Share with potential employers or clients

---

**Version**: 1.0
**Last Updated**: February 2026
**Author**: Power BI Dashboard Template
**Data Source**: AdventureWorks 2022

---

## Additional Resources

- Power BI Documentation: https://docs.microsoft.com/power-bi
- DAX Guide: https://dax.guide
- Color Palette Generator: https://coolors.co
- Power BI Community: https://community.powerbi.com

