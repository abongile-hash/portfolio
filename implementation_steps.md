# Power BI Dashboard Implementation Steps
## Step-by-Step Build Guide

---

## Prerequisites Checklist

✅ Power BI Desktop installed
✅ AdventureWorks data loaded
✅ All measures created (26 measures in _Measures table)
✅ Relationships configured correctly
✅ DateDim table connected to Sales SalesOrderHeader[OrderDate]

---

## PHASE 1: Page Setup

### Step 1: Configure Canvas Settings
1. Go to **View** tab
2. Set **Page View** to "Fit to Width"
3. Go to **Format** > **Canvas settings**
   - Type: Custom
   - Width: 1280 px
   - Height: 720 px (16:9 ratio)

### Step 2: Apply Theme
1. Go to **View** tab > **Themes** > **Customize current theme**
2. Set these colors:
   ```
   Primary: #0078D4
   Accent 1: #107C10 (Green)
   Accent 2: #D13438 (Red)
   Accent 3: #FF8C00 (Orange)
   Background: #F3F2F1
   ```
3. Save theme as "AdventureWorks_Theme.json"

### Step 3: Create Page Structure
1. Rename Page 1 to "Executive Summary"
2. Add new pages:
   - Product Performance
   - Customer Insights
   - Sales Team Performance
   - Time Analysis
3. Add hidden pages:
   - Tooltip Template
   - Product Detail (drill-through)
   - Customer Detail (drill-through)

---

## PHASE 2: Executive Summary Page

### Step 1: Add Page Header
1. Insert **Text Box**
   - Text: "SALES DASHBOARD - EXECUTIVE SUMMARY"
   - Font: Segoe UI Semibold, 18pt
   - Position: X=20, Y=10, Width=1000, Height=40

### Step 2: Create Date Slicer
1. Insert **Slicer**
2. Add field: DateDim[Year]
3. Format:
   - Style: Dropdown
   - Position: X=1060, Y=10, Width=200, Height=40
   - Selection: Allow multiple selections

### Step 3: Build KPI Cards

**Card 1: Total Revenue**
1. Insert **Card** visual
2. Add field: `[Total Revenue]`
3. Position: X=20, Y=60, Width=280, Height=120
4. Format:
   - **Callout value**:
     - Font: Segoe UI Bold, 32pt
     - Color: #323130
   - **Category label**:
     - Text: "Total Revenue"
     - Font: Segoe UI, 12pt
     - Color: #605E5C
   - **Background**: White (#FFFFFF)
   - **Border**: Light gray, 1px
   - **Effects**: Shadow (subtle, 2px offset)

5. Add subtitle showing growth:
   - Click card > Format > Callout value > Display units: None
   - Add second card below with `[Growth Indicator]` measure

**Repeat for Cards 2-4**:
- Card 2: Total Orders (X=320, Y=60)
- Card 3: Average Order Value (X=620, Y=60)
- Card 4: Gross Profit Margin % (X=920, Y=60)

### Step 4: Revenue Trend Line Chart
1. Insert **Line Chart**
2. Position: X=20, Y=200, Width=650, Height=280
3. Configuration:
   - **X-axis**: DateDim[Year-Month]
     - Sort by: DateDim[Date]
     - Type: Continuous
   - **Y-axis**: `[Total Revenue]`
   - **Legend**: Add these as separate lines:
     - `[Total Revenue]` (rename to "Current Year")
     - `[Revenue PY]` (rename to "Prior Year")
     - `[3-Month Moving Avg]` (rename to "3-Mo Avg")
4. Format:
   - **Title**: "Revenue Trend"
   - **Legend**: Position top, 10pt
   - **X-axis**:
     - Title: Off
     - Labels: 10pt, #605E5C
   - **Y-axis**:
     - Title: "Revenue (R)"
     - Display units: Millions
     - Labels: 10pt
   - **Data colors**:
     - Current Year: #0078D4 (solid line, 3px)
     - Prior Year: #605E5C (dashed line, 2px)
     - 3-Mo Avg: #107C10 (solid line, 2px)
   - **Data labels**: Off
   - **Gridlines**: Y-axis only, light gray

### Step 5: Revenue by Category Donut Chart
1. Insert **Donut Chart**
2. Position: X=690, Y=200, Width=290, Height=280
3. Configuration:
   - **Legend**: Production ProductCategory[Name]
   - **Values**: `[Total Revenue]`
   - **Detail labels**: All
4. Format:
   - **Title**: "Revenue by Category"
   - **Legend**: Position right, 10pt
   - **Detail labels**:
     - Category name: On
     - Percentage: On
     - Value: On (format as currency)
   - **Data colors**:
     - Bikes: #0078D4
     - Components: #107C10
     - Clothing: #FF8C00
     - Accessories: #8764B8

### Step 6: Top 5 Products Table
1. Insert **Table** visual
2. Position: X=1000, Y=200, Width=260, Height=280
3. Configuration:
   - **Columns**:
     1. Production Product[Name]
     2. `[Total Revenue]`
     3. `[Product Rank]`
4. Add **Top N filter**:
   - Filter type: Top N
   - Show items: Top 5
   - By value: `[Total Revenue]`
5. Format:
   - **Title**: "Top 5 Products"
   - **Style**: Minimal
   - **Grid**:
     - Vert: On
     - Horiz: On
     - Color: #E1DFDD
   - **Column headers**:
     - Background: #F3F2F1
     - Font: Segoe UI Semibold, 11pt
     - Padding: 8px
   - **Values**:
     - Font: 10pt
     - Text alignment: Right for numbers
   - **Conditional formatting** on Revenue:
     - Data bars: On
     - Gradient: #0078D4 to #0078D4 (solid)

### Step 7: Sales by Territory Visual
1. Insert **Clustered Bar Chart**
2. Position: X=20, Y=500, Width=650, Height=200
3. Configuration:
   - **Y-axis**: Sales SalesTerritory[Name]
   - **X-axis**: `[Total Revenue]`
4. Format:
   - **Title**: "Revenue by Territory"
   - **Data labels**: On (inside end)
   - **Data colors**: #0078D4
   - **Sort**: Descending by revenue

---

## PHASE 3: Product Performance Page

### Step 1: Add Slicers Panel
1. Insert **Rectangle** shape
   - Position: X=0, Y=0, Width=200, Height=720
   - Fill: #F3F2F1
   - Border: None

2. Add **Text Box** header:
   - Text: "FILTERS"
   - Position: X=10, Y=10
   - Font: Segoe UI Semibold, 14pt

3. Insert **Slicer** - Category
   - Field: Production ProductCategory[Name]
   - Position: X=10, Y=50, Width=180
   - Style: Dropdown
   - Multi-select: Yes

4. Insert **Slicer** - Subcategory
   - Field: Production ProductSubcategory[Name]
   - Position: X=10, Y=110, Width=180
   - Style: List
   - Multi-select: Yes

### Step 2: Product Performance Table
1. Insert **Table** visual
2. Position: X=220, Y=80, Width=1040, Height=300
3. Add columns:
   - Production Product[Name]
   - Production ProductCategory[Name]
   - `[Total Revenue]`
   - `[Total Quantity Sold]`
   - `[Gross Profit Margin %]`
   - `[Product Rank]`
4. **Conditional Formatting**:
   - Revenue column:
     - Data bars: On (#0078D4)
   - Margin % column:
     - Background color rules:
       - If value >= 0.35: #C6EFCE (light green)
       - If value >= 0.20 and < 0.35: #FFEB9C (light yellow)
       - If value < 0.20: #FFC7CE (light red)
5. **Top N Filter**: Top 20 by Revenue

### Step 3: Treemap Visual
1. Insert **Treemap**
2. Position: X=220, Y=400, Width=510, Height=300
3. Configuration:
   - **Group**:
     - Production ProductCategory[Name]
     - Production ProductSubcategory[Name]
   - **Values**: `[Total Revenue]`
4. Format:
   - **Title**: "Product Hierarchy"
   - **Data labels**: Category name + Value
   - **Colors**: By category (use theme colors)

### Step 4: Scatter Chart
1. Insert **Scatter Chart**
2. Position: X=750, Y=400, Width=510, Height=300
3. Configuration:
   - **X-axis**: Production Product[ListPrice]
   - **Y-axis**: `[Total Quantity Sold]`
   - **Size**: `[Total Revenue]`
   - **Legend**: Production ProductCategory[Name]
4. Format:
   - **Title**: "Price vs Volume Analysis"
   - **X-axis title**: "Unit Price (R)"
   - **Y-axis title**: "Quantity Sold"
   - **Markers**: Size 50-150%

---

## PHASE 4: Customer Insights Page

### Step 1: KPI Cards
Create 3 cards at top:
- Total Customers (X=20)
- Revenue per Customer (X=340)
- Avg Orders per Customer (X=660)

### Step 2: Top Customers Table
1. Insert **Table** visual
2. Position: X=20, Y=200, Width=600, Height=300
3. Columns:
   - Sales SalesOrderHeader[AccountNumber]
   - `[Total Revenue]` (with data bars)
   - `[Total Orders]`
   - `[Average Order Value]`
4. Top N Filter: Top 10 by Revenue

### Step 3: Customer Distribution Chart
1. Insert **Clustered Column Chart**
2. Position: X=640, Y=200, Width=620, Height=300
3. Configuration:
   - **X-axis**: Sales SalesTerritory[Name]
   - **Y-axis**: `[Total Customers]`
   - **Legend**: DateDim[Year]
4. Format: Stacked or clustered view

---

## PHASE 5: Sales Team Performance Page

### Step 1: Territory Filter Slicer
1. Insert **Slicer**
2. Field: Sales SalesTerritory[Name]
3. Position: Top right
4. Style: Dropdown

### Step 2: Team KPI Cards
Create 3 cards:
- Active Salespeople
- Revenue per Salesperson
- Active Territories

### Step 3: Salesperson Leaderboard
1. Insert **Table** visual
2. Position: Full width, centered
3. Add columns:
   - Person Person[FirstName] & [LastName] (combined in Power Query)
   - Sales SalesTerritory[Name]
   - `[Total Revenue]` (data bars)
   - `[Total Orders]`
   - `[Average Order Value]`
   - `[Gross Profit Margin %]` (color scale)
4. Sort: Descending by Revenue

---

## PHASE 6: Time Analysis Page

### Step 1: Date Range Slicer
1. Insert **Slicer**
2. Field: DateDim[Date]
3. Type: Between
4. Position: Top right
5. Default: Last 12 months

### Step 2: Time KPI Cards
Create 4 cards:
- YTD Revenue
- MTD Revenue
- YTD Growth %
- Selected Period (text)

### Step 3: Running Total Chart
1. Insert **Area Chart**
2. Configuration:
   - X-axis: DateDim[Year-Month]
   - Y-axis: `[Running Total Revenue]`
   - Legend: Current vs Prior Year
3. Format: Smooth line, filled area

### Step 4: Monthly Comparison Combo Chart
1. Insert **Line and Clustered Column Chart**
2. Configuration:
   - Columns: `[Total Revenue]` (current year)
   - Line: `[Revenue PY]`
   - X-axis: DateDim[Month Name]
3. Format: Side-by-side comparison

---

## PHASE 7: Interactivity Setup

### Enable Cross-filtering
1. Select each visual
2. Format > Edit interactions
3. Set appropriate filter/highlight behavior:
   - Slicers → Filter all visuals
   - Charts → Highlight related visuals
   - Cards → No interaction

### Create Bookmarks

**Bookmark 1: Executive View**
1. Hide detailed tables
2. Show only KPIs and main charts
3. Save as "Executive View"
4. Assign to button

**Bookmark 2: Detailed View**
1. Show all visuals
2. Save as "Detailed Analysis"
3. Assign to button

### Create Drill-through Pages

**Product Detail Page**
1. Create new page
2. Set as drill-through page
3. Add drill-through field: Production Product[ProductID]
4. Add visuals:
   - Product name header
   - Revenue trend line
   - Customer list
   - Sales by territory

**Customer Detail Page**
1. Create new page
2. Set as drill-through page
3. Add drill-through field: Sales Customer[CustomerID]
4. Add visuals:
   - Customer info cards
   - Order history table
   - Revenue trend

### Create Custom Tooltip Page
1. Create new page
2. Set page type: Tooltip
3. Set canvas size: Tooltip (320x240)
4. Add compact visuals:
   - Mini trend line (7 days)
   - Key metric cards
   - YoY comparison

---

## PHASE 8: Formatting & Polish

### Apply Consistent Styling
1. Select all similar visuals (Ctrl+Click)
2. Use **Format Painter** to apply formatting
3. Align visuals using alignment tools
4. Distribute spacing evenly

### Add Navigation
1. Insert buttons on each page
2. Set actions to navigate between pages
3. Style consistently:
   - Background: #0078D4
   - Text: White
   - Hover: #005A9E

### Optimize Performance
1. Review **Performance Analyzer**
2. Identify slow visuals
3. Optimize:
   - Reduce visual count if needed
   - Use Top N filters
   - Simplify complex calculations

### Test Functionality
- [ ] All slicers filter correctly
- [ ] Cross-filtering works as expected
- [ ] Drill-through pages open properly
- [ ] Bookmarks switch views
- [ ] Tooltips display correctly
- [ ] Mobile layout is functional

---

## PHASE 9: Final Checks

### Data Accuracy
- Verify measure calculations
- Check for unexpected blanks
- Validate totals against source data
- Test edge cases (no data, single value, etc.)

### User Experience
- Logical visual placement
- Clear labeling
- Intuitive navigation
- Appropriate level of detail
- Mobile-responsive design

### Performance
- Load time < 3 seconds
- Smooth interactions
- No visual lag
- Appropriate data granularity

---

## Publishing Checklist

### Before Publishing
- [ ] Remove test/development elements
- [ ] Hide technical columns
- [ ] Set default filters/slicers
- [ ] Add last refresh date
- [ ] Test on different screen sizes
- [ ] Review with stakeholders

### Publish to Service
1. Click **Publish** button
2. Select workspace
3. Configure refresh schedule
4. Set row-level security (if needed)
5. Share with appropriate users

### Documentation
- [ ] Create user guide
- [ ] Document measures and calculations
- [ ] List data sources
- [ ] Provide refresh schedule
- [ ] Include support contact

---

## Troubleshooting Tips

### Visual Not Displaying
- Check if data exists for selection
- Verify relationships
- Review filters applied
- Check field data types

### Incorrect Totals
- Verify measure formulas
- Check relationship cardinality
- Review filter context
- Test with simplified data

### Slow Performance
- Reduce visual count per page
- Use Top N filters
- Optimize DAX measures
- Consider aggregations

---

## Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| Save | Ctrl + S |
| Undo | Ctrl + Z |
| Redo | Ctrl + Y |
| Copy visual | Ctrl + C |
| Paste visual | Ctrl + V |
| Duplicate visual | Ctrl + D |
| Group visuals | Ctrl + G |
| Align left | Ctrl + Shift + L |
| Distribute horizontal | Ctrl + Shift + H |
| Open format pane | F3 |
| Refresh data | Ctrl + Alt + R |

---

## Next Steps

1. **Build**: Follow these steps to create your dashboard
2. **Customize**: Adapt visuals to your needs
3. **Test**: Verify all functionality
4. **Optimize**: Improve performance
5. **Document**: Create user guide
6. **Present**: Share with stakeholders

---

**Estimated Build Time**: 4-6 hours
**Difficulty**: Intermediate
**Skills Required**: Power BI Desktop, DAX basics, Data modeling

