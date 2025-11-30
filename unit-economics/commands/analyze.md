---
description: Analyze unit economics data and identify growth opportunities
allowed-tools:
  - mcp__*
  - Read
  - AskUserQuestion
---

# Analyze Unit Economics

Analyze business metrics, compare with benchmarks, and identify areas for improvement.

## Your Task

### Step 1: Get Data Source

Ask the user:
1. **Google Sheets URL** (created by /create-table)
2. **Or CSV/Excel file path**
3. **Or manual input** of key metrics

### Step 2: Extract Key Metrics

Read the data and extract these metrics:

**Funnel metrics:**
- Users (traffic)
- Conversion to lead/registration
- Conversion to buyer (C1)
- Buyers
- Payments

**Unit economics (CRITICAL):**
- **AMPU** (margin per USER - most important!)
- AMPPU (margin per PAYING user)
- CAC (customer acquisition cost)
- CPUser (cost per user)
- LTV = C1 × CLTV
- COGS %
- Churn rate

**The key relationship:**
```
AMPU = C1 × AMPPU

Profit = Users × (AMPU - CPUser)
       = Users × (C1 × AMPPU - CPUser)
```

**Financial:**
- Revenue
- Gross Profit = AMPPU × Buyers = AMPU × Users
- Profit = Gross Profit - Acquisition Costs

### Step 3: Apply Benchmarks

Use these rules from course materials (`${CLAUDE_PLUGIN_ROOT}/materials/glossary.md`):

#### AMPU vs CPUser (MOST IMPORTANT!)
```
AMPU > CPUser    → ✅ Profitable unit economics
AMPU = CPUser    → ⚠️ Break-even, no profit
AMPU < CPUser    → 🔴 Losing money on each user!
```

#### LTV/CAC Ratio
```
LTV/CAC ≥ 3     → ✅ Healthy business, can scale
LTV/CAC 1-3     → ⚠️ Needs optimization before scaling
LTV/CAC < 1     → 🔴 Losing money on each customer
```

#### Payback Period
```
Payback ≤ 6 months   → ✅ Excellent
Payback 6-12 months  → ⚠️ Acceptable
Payback > 12 months  → 🔴 Too long, cash flow risk
```

#### Conversion C1 (user → buyer)
```
C1 > 5%      → ✅ Strong conversion
C1 1-5%      → ⚠️ Average, room for improvement
C1 < 1%      → 🔴 Low, focus on funnel optimization
C1 < 0.1%   → 🔴 Critical, major funnel issues
```

#### COGS Margin
```
COGS < 30%   → ✅ Healthy margins
COGS 30-50%  → ⚠️ Moderate margins
COGS > 50%   → 🔴 Low margins, optimize costs
```

#### Churn (monthly, for SaaS)
```
Churn < 3%   → ✅ Good retention
Churn 3-7%   → ⚠️ Average
Churn > 7%   → 🔴 High churn, retention problem
```

### Step 4: Identify Problems & Opportunities

For each metric outside healthy range, provide:

1. **Problem statement** - what's wrong
2. **Impact** - how it affects the business
3. **Root causes** - common reasons from materials
4. **Recommendations** - specific actions to take

#### Common Issues & Recommendations

**AMPU < CPUser (losing money):**
- This is THE critical problem - you lose money on every user
- Either increase AMPU (better C1 or AMPPU) or decrease CPUser
- Check: AMPU = C1 × AMPPU, which component is weak?

**Low C1 (conversion):**
- Build detailed funnel to find blockers
- Check mobile vs desktop conversion (often 2-3x difference)
- Analyze new vs returning users separately
- Don't confuse sessions with users (common GA mistake)

**High CAC:**
- Calculate CAC by channel separately
- Check if CPUser × (1/C1) makes sense
- Consider viral coefficient to reduce effective CPUser
- Audit what's included in Acquisition Costs

**Low LTV:**
- Analyze retention/churn by cohort
- Check if AMPPU includes all payments (AvPaymentCount)
- Look at upgrade paths (Starter → Business)
- Consider annual plans to improve retention

**Long Payback:**
- Focus on AMPPU in first 30 days
- Reduce CAC through better targeting
- Improve C1 through funnel optimization
- Consider payment terms (annual vs monthly)

### Step 5: Generate Report

Output format:

```
## Unit Economics Analysis: {Company Name}

### Key Insight: Profit per User

AMPU = {value}     (margin per user)
CPUser = {value}   (cost per user)
AMPU - CPUser = {value}  → {✅ Profit / 🔴 Loss} per user

With {Users} users/month:
Monthly Profit = {Users} × {AMPU - CPUser} = {value}

### Summary
| Metric | Value | Status | Benchmark |
|--------|-------|--------|-----------|
| AMPU | $X | ✅/⚠️/🔴 | > CPUser |
| CPUser | $X | - | - |
| AMPU - CPUser | $X | ✅/🔴 | > 0 |
| C1 | X% | ✅/⚠️/🔴 | > 1% |
| AMPPU | $X | - | - |
| LTV/CAC | X | ✅/⚠️/🔴 | ≥ 3 |
| Payback | X mo | ✅/⚠️/🔴 | ≤ 6 mo |
| Churn | X% | ✅/⚠️/🔴 | < 3% |

### 🔴 Critical Issues
...

### ⚠️ Areas for Improvement
...

### ✅ Strengths
...

### Recommended Priority
1. ...
2. ...
3. ...
```

---

## Reference Formulas

From `${CLAUDE_PLUGIN_ROOT}/materials/glossary.md`:

```
# Core unit economics
AMPU = C1 × AMPPU                    ← margin per USER
AMPPU = Av.Price × Margin × AvPaymentCount  ← margin per PAYING user

# Costs
CAC = Acquisition Costs / buyers
CPUser = Acquisition Costs / users
CAC = CPUser / C1

# Lifetime metrics
CLTV = AMPPU over lifetime
LTV = C1 × CLTV = AMPU over lifetime

# Profitability
Profit = Users × (AMPU - CPUser)
Gross Profit = AMPPU × Buyers = AMPU × Users
Payback = CAC / AMPPU_monthly

# Conversion
C1 = buyers / users
```

---

## Example Analysis

**User**: "Analyze: 10000 users, 250 buyers, CPUser=$25, monthly price $49, margin 70%, churn 5%"

**You**:
```
Step 1: Calculate metrics
C1 = 250/10000 = 2.5%
AMPPU/month = $49 × 70% = $34.30
Lifetime = 1/0.05 = 20 months
AMPPU_lifetime (CLTV) = $34.30 × 20 = $686

AMPU = C1 × AMPPU_lifetime = 2.5% × $686 = $17.15
CPUser = $25
CAC = CPUser / C1 = $25 / 2.5% = $1000

Step 2: Key analysis
AMPU - CPUser = $17.15 - $25 = -$7.85 🔴

With 10000 users/month:
Monthly Loss = 10000 × (-$7.85) = -$78,500

LTV/CAC = $17.15 / $1000 × (1/C1) = ... wait, let me recalculate:
LTV = AMPU = $17.15 per user
CAC = $1000 per buyer, but per USER it's CPUser = $25
Actually: LTV/CAC = $686 / $1000 = 0.69 🔴

Step 3: Diagnosis
Problem: You're losing $7.85 per user acquired.
Root cause: CAC ($1000) > CLTV ($686)

Recommendations:
1. 🔴 Reduce CPUser (currently $25, need < $17.15 for profit)
2. ⚠️ Improve C1 from 2.5% to 4%+ (doubles AMPU)
3. ⚠️ Reduce churn from 5% to 3% (increases lifetime from 20 to 33 mo)
```

---

## Tips

- **Always calculate AMPU first** - it shows profit per user directly
- Read `materials/glossary.md` for accurate formulas
- Don't mix users/sessions/clicks - use users only
- Separate new vs old users in analysis
- Calculate metrics by channel when possible
- Use cohort analysis for retention insights
