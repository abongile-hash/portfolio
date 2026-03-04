# Power BI Dashboard Quick Reference Card

## 🎯 Dashboard Pages Overview

| Page | Purpose | Key Visuals | Main Metrics |
|------|---------|-------------|--------------|
| **Executive Summary** | High-level KPIs | Revenue trend, Category split, Territory map | Total Revenue, Orders, AOV, Margin % |
| **Product Performance** | Product analysis | Product table, Treemap, Scatter chart | Products sold, Quantity, Margin by product |
| **Customer Insights** | Customer metrics | Top customers, Territory breakdown | Total Customers, Revenue/Customer |
| **Sales Team** | Team performance | Leaderboard, Territory comparison | Active salespeople, Revenue/Salesperson |
| **Time Analysis** | Trend analysis | Running total, YoY comparison | YTD/MTD Revenue, Growth % |

---

## 📊 Key Measures Cheat Sheet

### Core Business Metrics
```dax
[Total Revenue] = SUM('Sales SalesOrderDetail'[LineTotal])
[Total Orders] = DISTINCTCOUNT('Sales SalesOrderHeader'[SalesOrderID])
[Average Order Value] = DIVIDE([Total Revenue], [Total Orders], 0)
[Total Customers] = DISTINCTCOUNT('Sales Customer'[CustomerID])
```

### Profitability
```dax
[Total Cost] = SUMX('Sales SalesOrderDetail', 
    [OrderQty] * RELATED('Production Product'[StandardCost]))
[Gross Profit] = [Total Revenue] - [Total Cost]
[Gross Profit Margin %] = DIVIDE([Gross Profit], [Total Revenue], 0)
```

### Time Intelligence
```dax
[Revenue PY] = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(DateDim[Date]))
[YoY Growth %] = DIVIDE([Total Revenue] - [Revenue PY], [Revenue PY], 0)
[YTD Revenue] = CALCULATE([Total Revenue], DATESYTD(DateDim[Date]))
[Running Total Revenue] = CALCULATE([Total Revenue], 
    FILTER(ALLSELECTED(DateDim), DateDim[Date] <= MAX(DateDim[Date])))
```

### Advanced Analytics
```dax
[Product Rank] = IF(ISINSCOPE('Production Product'[Name]),
    RANKX(ALLSELECTED('Production Product'[Name]), [Total Revenue],,DESC), BLANK())
[% of Total Revenue] = DIVIDE([Total Revenue], 
    CALCULATE([Total Revenue], ALLSELECTED()), 0)
```

---

## 🎨 Color Palette

### Primary Colors
- **Blue** `#0078D4` - Revenue, Primary elements
- **Green** `#107C10` - Profit, Positive indicators
- **Red** `#D13438` - Cost, Negative indicators
- **Orange** `#FF8C00` - Warnings

### Neutral Colors
- **Dark** `#323130` - Headers, Text
- **Medium** `#605E5C` - Secondary text
- **Light** `#F3F2F1` - Backgrounds
- **White** `#FFFFFF` - Cards

### Category Colors
- **Bikes** `#0078D4`
- **Components** `#107C10`
- **Clothing** `#FF8C00`
- **Accessories** `#8764B8`

---

## 📐 Standard Sizes

### Canvas
- Width: 1280px
- Height: 720px
- Ratio: 16:9

### KPI Cards
- Width: 280px
- Height: 120px
- Spacing: 20px

### Charts
- Small: 290x280
- Medium: 510x280
- Large: 650x280
- Full Width: 1240x280

---

## 🔧 Common Visual Settings

### Card Visual
- Font: Segoe UI Bold, 32pt
- Background: White
- Border: 1px, #E1DFDD
- Shadow: 2px offset, 10% opacity

### Line Chart
- Line width: 3px (primary), 2px (secondary)
- Data labels: Off
- Gridlines: Y-axis only
- Legend: Top position

### Table
- Header background: #F3F2F1
- Row colors: Alternating white/light gray
- Font: 10pt
- Alignment: Right for numbers, Left for text

---

## ⚡ Quick Actions

### Formatting
- Copy format: **Alt + Shift + C**
- Paste format: **Alt + Shift + V**
- Format pane: **F3**

### Navigation
- Next page: **Ctrl + PgDown**
- Previous page: **Ctrl + PgUp**
- Edit relationships: **Ctrl + 1**
- Manage relationships: **Ctrl + 2**

### Data
- Refresh: **Ctrl + Alt + R**
- Transform data: **Ctrl + T**
- New measure: **Ctrl + M**
- New column: **Ctrl + K**

---

## 🎯 Conditional Formatting Quick Guide

### Data Bars
- Use for: Revenue, Quantity, any magnitude comparison
- Color: Match your primary color (#0078D4)
- Direction: Left to right
- Minimum: 0 or auto

### Color Scales
- Use for: Percentages, Ratios, Performance metrics
- Bad: Red (#FFC7CE)
- Neutral: Yellow (#FFEB9C)
- Good: Green (#C6EFCE)

### Icons
- Use for: Growth indicators, Status, Trends
- Set: Arrows or Traffic lights
- Rules: Based on thresholds (e.g., >0 = up arrow)

---

## 📱 Mobile Layout Tips

1. **Optimize for portrait**: 320px width minimum
2. **Simplify**: Show fewer visuals on mobile
3. **Larger touch targets**: Minimum 44px
4. **Stack vertically**: Don't rely on horizontal space
5. **Priority visuals**: KPIs and key charts only

---

## 🔍 Troubleshooting Quick Fixes

| Problem | Solution |
|---------|----------|
| Blank visual | Check filters, verify data exists |
| Wrong total | Review measure formula, check relationships |
| Slow performance | Reduce visuals, add Top N filters |
| Can't drill-through | Verify drill-through field configuration |
| Slicer not filtering | Check visual interactions settings |
| Incorrect date order | Sort by date column, not month name |

---

## 📋 Pre-Publish Checklist

- [ ] All measures working correctly
- [ ] Slicers filtering as expected
- [ ] Cross-filtering configured
- [ ] Drill-through pages functional
- [ ] Bookmarks created
- [ ] Mobile layout tested
- [ ] Performance optimized
- [ ] Documentation created
- [ ] Stakeholder review complete
- [ ] Refresh schedule configured

---

## 🎓 Best Practices

### DO
✅ Use measures table for organization
✅ Name measures clearly and consistently
✅ Apply consistent color scheme
✅ Test with different data selections
✅ Document complex calculations
✅ Keep visuals simple and focused
✅ Use drill-through for details

### DON'T
❌ Overcrowd pages (max 8-10 visuals)
❌ Use too many colors (3-4 maximum)
❌ Create calculated columns for aggregations
❌ Ignore performance analyzer
❌ Skip testing edge cases
❌ Forget to add context (titles, labels)
❌ Use pie charts for more than 5 categories

---

## 📞 Key Resources

- **DAX Guide**: https://dax.guide
- **Power BI Docs**: https://docs.microsoft.com/power-bi
- **Community**: https://community.powerbi.com
- **Themes**: https://powerbi.tips/themes

---

## 💡 Pro Tips

1. **Use CTRL + Click** to select multiple visuals
2. **Format Painter** is your friend for consistency
3. **Bookmark blank states** before creating views
4. **Name measures** with brackets: [Total Revenue]
5. **Group related measures** in display folders
6. **Test filters** by clicking "Select all" then "Clear"
7. **Use DIVIDE()** instead of "/" to avoid errors
8. **Sort months** by date column, not month name
9. **Hide technical tables** to clean field list
10. **Create a measures table** for better organization

---

**Version**: 1.0 | **Page**: 1 of 1 | **Print this for quick reference!**

