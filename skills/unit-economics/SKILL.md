---
name: unit-economics
description: Unit economics methodology - use when analyzing CAC, LTV, AMPPU, COGS, conversion, cohorts, A/B testing, channel effectiveness, or calculating business metrics
---

# Unit Economics Methodology

This skill provides comprehensive knowledge of unit economics for analyzing and optimizing business models, based on Heroes.camp course materials.

## When to Use This Skill

Automatically invoke this skill when the user asks about:
- **Metrics**: CAC, LTV, AMPPU, COGS, conversion, CPUser, payback period, retention, churn
- **Calculations**: "как посчитать", "формула", "calculate", "расчет метрик"
- **Analysis**: "проанализируй экономику", "найди bottlenecks", "оптимизируй"
- **Methodology**: "юнит-экономика", "unit economics", "конверсия", "когорты", "cohorts"
- **Testing**: "A/B тест", "сплит-тест", "эксперимент"

## Core Principles

### How Unit Economics Works

```
Users → 1st Sale → Buyers → Payments → Revenue
   ↓        ↓         ↓         ↓          ↓
CPUser   COGS    AMPPU    Margin    Profit
```

**Economy formula:**
- Input: Users (привлеченные пользователи)
- Process: Conversion → Payment
- Output: Money (деньги)

**Scalable costs:**
- **Acquisition Costs** - до 1-й продажи (marketing, sales)
- **COGS** - после 1-й продажи (delivery, support, servers)

**Non-scalable costs:**
- **Fix Costs** - можно ограничить сверху (team salaries, office)

---

## Key Metrics & Formulas

### Funnel Metrics

**Users** (пользователи)
- Уникальные люди на сайте/в приложении
- ⚠️ НЕ сессии! (1 user = 1.1-2+ sessions)

**C1 - Conversion to Buyer**
```
C1 = buyers / users (усредненная)
C1 = buyers_new / users_new (когортная, более точная)
```

**Leads** (лиды, регистрации)
- Пользователи с контактными данными
- Conversion to lead = leads / users

⚠️ **NEVER** считайте "конверсию в заказ":
```
❌ WRONG: orders / sessions (vanity metric!)
✅ RIGHT: buyers / users
```

### Unit Economics Metrics

**AMPPU** (Average Margin per Paying User)
```
AMPPU = Av.Price × Margin × Av.Payment Count
```
- Маржа с платящего пользователя
- Учитывает COGS или margin

**AMPU** (Average Margin per User)
```
AMPU = C1 × AMPPU
AMPU = Gross Profit / Users
```
- Маржа с привлеченного пользователя

**CAC** (Customer Acquisition Cost)
```
CAC = Acquisition Costs / buyers
```
- Стоимость привлечения покупателя
- Включает ВСЕ расходы до 1-й оплаты

**CPUser** (Cost per User)
```
CPUser = Acquisition Costs / users
Связь: CAC = CPUser / C1
```

**LTV** (LifeTime Value)
```
LTV = AMPU за все время жизни
CLTV = AMPPU за все время жизни
LTV = C1 × CLTV
```

### Cost Metrics

**COGS** (Cost of Goods Sold)
- Масштабируемые издержки на каждой продаже
- Включает: себестоимость, доставка, комиссия платежным системам, налоги с продаж
- Тест: "При росте в 100x как вырастет?" → Если кратно = COGS

**COGS 1st sale**
- Разовые издержки только на первой продаже
- Пример: внедрение, премия за первую продажу

**Margin** (маржинальность)
- Сколько достается нам из среднего чека
- Margin = 20% → COGS = 80%

### Financial Metrics

**Gross Profit**
```
Gross Profit = AMPPU × buyers
Gross Profit = revenue × margin
Gross Profit = revenue - COGS
```

**Profit** (прибыль)
```
Profit = users × (-CPUser + AMPU)
Profit = Gross Profit - Acq Costs
```

**Profit Net** (чистая прибыль)
```
Profit Net = Profit - Fix Costs
```

**Revenue** (выручка)
```
Revenue = Av.Price × payments
```
⚠️ Сама по себе мало что показывает! Смотрите на Gross Profit и Profit.

---

## Common Mistakes & Pitfalls

### Conversion Mistakes

❌ **Ошибка №1: Считать конверсию через сессии**
```
GA/Яндекс.Метрика: conversion = goals / sessions
Проблема: 1 user = 1.5-2+ sessions → искажение!
```

✅ **Правильно:**
```
Conversion = buyers / users (по уникальным пользователям)
```

❌ **Ошибка №2: "Конверсия в заказ"**
```
WRONG: payments / sessions
Проблема: 1 buyer делает 1.1-2+ payments
```

✅ **Правильно:** Считайте конверсию в покупателя, не в заказ.

❌ **Ошибка №3: Игнорировать двойной эффект конверсии**

Рост C1 на +0.3 п.п.:
- ✅ Увеличивает buyers
- ✅ Снижает CAC (можно платить больше за трафик!)

### Metric Categorization Mistakes

❌ **CAC — не actionable метрика**
```
CAC зависит от: C1 × CPUser
```
Нельзя понять, где проблема - в конверсии или в стоимости трафика.

✅ **Actionable метрики:**
- **C1** (конверсия) - проблемы с продуктом/лендингом
- **CPUser** - проблемы с таргетингом/каналами

### A/B Testing Mistakes

❌ **Ошибка №1: Перебирать 41 оттенок синего**
```
Вероятность случайного срабатывания:
1 - 0.95^41 = 88%
```

❌ **Ошибка №2: Не учитывать зависимость действий**
- Все действия пользователя связаны между собой
- Решение: считайте per user, не per action

❌ **Ошибка №3: Игнорировать сезонность**
- Конверсия в пятницу ≠ конверсия в субботу
- Держите эксперимент полную неделю

❌ **Ошибка №4: Фильтры и отсечения**
- Исключайте операторов call-центра
- Отсекайте экстремальные значения (top 3-5%)
- Проверяйте равномерность split 50/50

❌ **Ошибка №5: Смотреть только на конверсию**
```
Правильная метрика: Revenue per Visit
Не забывайте про средний чек!
```

### Cohort Analysis Mistakes

❌ **GA показывает "вернувшиеся пользователи" по сессиям**
- 21% retention по сессиям → 3.67% по пользователям!

✅ **Правильно:** Используйте Cohort Analysis в GA или считайте по ga_clientID.

---

## Business Models & Formulas

### E-commerce / Marketplace
```
AMPPU = Av.Price × Margin × Av.Payment Count
```

### Subscription (SaaS)
```
AMPPU = Monthly Price × Margin × LifeTime
LifeTime = среднее число месяцев подписки
```

### Revenue Share
```
AMPPU = Av.Price × Commission % × Av.Payment Count
```

---

## Cohort Analysis

**Key metrics:**
- **Retention** - доля вернувшихся пользователей
- **Churn rate** - доля ушедших пользователей
- **Gross Profit by cohort** - доход от когорты по месяцам

**Формула retention:**
```
Retention Month 2 = (users active in M2) / (users new in M1)
```

**Churn calculation:**
```
Churn = buyers lost / buyers old
Churn rate = churn / (buyers old + buyers new)
```

---

## Optimization Strategies

### Growing Profit

Profit зависит от:
```
Profit = users × (-CPUser + C1 × AMPPU)
```

**Варианты роста:**
1. ↑ Users - масштабирование трафика
2. ↓ CPUser - оптимизация каналов
3. ↑ C1 - улучшение конверсии (двойной эффект!)
4. ↑ AMPPU - рост среднего чека, маржи, повторных покупок

### Growing AMPPU

```
AMPPU = Av.Price × Margin × Av.Payment Count
```

**Варианты:**
1. ↑ Av.Price - поднять цены, upsell
2. ↑ Margin - снизить COGS, увеличить комиссию
3. ↑ Av.Payment Count - retention, повторные покупки

### Channel Optimization

**Сравнивайте каналы по:**
- C1 (conversion) - качество трафика
- CPUser - стоимость привлечения
- CAC = CPUser / C1 - итоговая стоимость покупателя
- AMPU - доход с пользователя

**Не сравнивайте только по CAC!** Разбейте на C1 и CPUser.

---

## Working with Course Materials

For detailed information, formulas, and examples, read from course materials:

**${CLAUDE_PLUGIN_ROOT}/materials/glossary.md**
- Complete metrics glossary (405 lines)
- All formulas with explanations
- Common mistakes and edge cases
- Metric naming variations in different companies

**${CLAUDE_PLUGIN_ROOT}/materials/conversion.md**
- Why conversion is underestimated
- How to calculate conversion correctly
- GA/Яндекс.Метрика mistakes
- Cohort analysis methodology
- Double effect of conversion growth

**${CLAUDE_PLUGIN_ROOT}/materials/ab_testing.md**
- A/B testing pitfalls
- Statistical significance
- Common errors (99% tests are wrong!)
- How to conduct proper tests
- Bayesian vs frequentist approaches

**${CLAUDE_PLUGIN_ROOT}/materials/cases/**
- Flowwow.csv - marketplace case with channels
- EnglishMeow.csv - EdTech subscription model
- Cohorts.csv - cohort analysis example

---

## Practical Guidelines

### Before Analysis

1. **Verify data quality:**
   - Считайте по users, не по sessions
   - Проверьте дубли и фильтры
   - Исключите операторов и тестовые транзакции

2. **Choose right metrics:**
   - Actionable метрики (C1, CPUser) > Vanity (CAC alone)
   - Gross Profit > Revenue
   - Per user > Per session

3. **Use cohort analysis:**
   - Считайте new/old пользователей отдельно
   - Смотрите retention и churn
   - Анализируйте LTV по когортам

### During Analysis

1. **Calculate full picture:**
   ```
   Users → C1 → Buyers → AMPPU → Gross Profit
     ↓              ↓               ↓
   CPUser        CAC           Profit
   ```

2. **Break down by channels:**
   - Organic vs Paid
   - По источникам (Google, Yandex, Facebook)
   - По устройствам (mobile vs desktop)

3. **Find bottlenecks:**
   - Где наибольшее падение в воронке?
   - Считайте в рублях, не в %!
   - Оцените потенциал роста

### Generating Hypotheses

1. **↑ Conversion:**
   - Улучшить UX на критичных шагах
   - Исправить мобильную версию
   - Упростить форму checkout

2. **↓ CAC:**
   - Отключить неэффективные каналы
   - Улучшить таргетинг
   - Оптимизировать ставки на аукционах

3. **↑ AMPPU:**
   - Cross-sell / upsell
   - Увеличить средний чек
   - Retention программы

---

## Important Reminders

⚠️ **ALWAYS:**
- Считайте конверсию по users, не по sessions
- Записывайте все издержки до 1-й продажи в Acquisition Costs (честнее!)
- Считайте cohort analysis правильно (по ga_clientID)
- Проводите A/A тесты для проверки системы

⚠️ **NEVER:**
- Не считайте "конверсию в заказ" (vanity метрика!)
- Не перебирайте параметры без гипотез (41 оттенок синего)
- Не игнорируйте зависимость действий пользователя
- Не забывайте про сезонность

---

## Quick Reference

**Main economics formula:**
```
Profit = Users × (AMPU - CPUser)
где AMPU = C1 × AMPPU
```

**Payback period:**
```
Payback = CAC / AMPPU_monthly
```

**LTV/CAC ratio:**
```
Healthy: LTV / CAC > 3
Warning: LTV / CAC < 1 (убыток на каждом клиенте!)
```

---

Use this skill to analyze business metrics, optimize unit economics, and make data-driven decisions based on proven methodology from Heroes.camp course.
