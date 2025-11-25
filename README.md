# Planetary Health Diet Index (PHDI) Questionnaire - Updated PRD

**Version**: 2.0
**Last Updated**: 2025
**Reference**: EAT-Lancet 2025 recommendations, PHDI 2024 scoring methodology
**Baseline**: 2400 kcal/day
**Total Score**: 0-140 points

---

## 1. QUESTIONNAIRE STRUCTURE

Create a clean, user-friendly form with **15 diet components** organized into 3 sections.

### Section 1: Plant-Based Foods (7 components)

| # | Emoji | Food Group | Frequency Unit | Serving Size | Example Foods | Options (Display → Internal Value) |
|---|-------|------------|----------------|--------------|---------------|-----------------------------------|
| 1 | 🌾 | Whole grains | servings per week | 100g | muesli, rice, pasta, cereal, bread | 0→0, 1-2→1.5, 3-4→3.5, 5+→5.5 |
| 2 | 🥔 | Tubers and starchy roots | servings per week | 150g | potato, casava, sweet potato | 0→0, 1-2→1.5, 3-5→4, 6-9→7.5, 10+→10 |
| 3 | 🥬 | Vegetables | servings per day | 100g | lettuce, broccoli, cabbage, green beans, cauliflower | 0→0, 1→1, 2→2, 3+→3 |
| 4 | 🍊 | Fruits | servings per day | 100-150g | apple, banana, orange, grapes, melons, pineapples | 0→0, 1-2→1.5, 3+→3 |
| 5 | 🥜 | Tree nuts and peanuts | servings per day | 30g | peanuts, hazelnuts, almonds, cashew nuts, walnuts | 0→0, 1-2→1.5, 3+→3 |
| 6 | 🫛 | Soy Legumes | servings per week | 100g | tofu, soy beans, tempeh, soy foods | 0→0, 1-2→1.5, 3-4→3.5, 4+→4 |
| 7 | 🫘 | Non-soy legumes | servings per week | 75g | lentils, peas, chickpeas, dry beans | 0→0, 1-2→1.5, 3-5→4, 6-8→7, 9+→9.33 |

### Section 2: Animal Products (5 components)

| # | Emoji | Food Group | Frequency Unit | Serving Size | Example Foods | Options (Display → Internal Value) |
|---|-------|------------|----------------|--------------|---------------|-----------------------------------|
| 8 | 🥛 | Dairy | servings per week | 1 cup milk OR 40g cheese OR 200g yogurt | milk, cheese, yogurt | 0→0, 1-2→1.5, 3-4→3.5, 5-6→5.5, 7+→7 |
| 9 | 🍗 | Chicken and poultry | servings per week | 100g | chicken breast, turkey, duck | 0→0, 1→1, 2→2, 3-4→3.5, 5+→5 |
| 10 | 🐟 | Fish and shellfish | servings per week | 100g | salmon, tuna, sardines, prawns | 0→0, 1→1, 2→2, 3-4→3.5, 5+→5 |
| 11 | 🥚 | Eggs | servings per week | 50g (1 egg) | chicken eggs | 0→0, 1→1, 2→2, 3-4→3.5, 5-6→5.5, 7+→7 |
| 12 | 🥩 | Red meat | servings per week | 100g | beef, pork, lamb | 0→0, 1→1, 2→2, 3-4→3.5, 5+→5 |

### Section 3: Fats & Other (3 components)

| # | Emoji | Food Group | Frequency Unit | Serving Size | Example Foods | Options (Display → Internal Value) |
|---|-------|------------|----------------|--------------|---------------|-----------------------------------|
| 13 | 🫒 | Unsaturated plant oils | servings per day | 30g | olive, vegetable oil, canola oil, sunflower oil | 0→0, 1→1, 2+→2 |
| 14 | 🧈 | Saturated oil and transfat | servings per day | 10g | Palm, coconut oil, lard, tallow, margarine, butter | 0→0, 1→1, 2→2, 3+→3 |
| 15 | 🍬 | Added sugar | servings per day | 30g | Added sugar or sugar from fruit juice, dessert, soft drinks (1 tbsp = 4g sugar, 1 soft drink can = ~40g sugar) | 0→0, 1→1, 2→2, 3→3, 4→4, 5+→5 |

---

## 2. QUESTION FORMAT PATTERN

Each question should follow this format:

```html
<div className="p-4 bg-gray-50 rounded-lg">
    <label className="block text-gray-700 font-semibold mb-2">
        [emoji] [Food group] - (servings per day/week)
        <span className="block text-sm text-gray-500 font-normal mt-1">
            [Serving size] • [Example foods]
        </span>
    </label>
    <div className="flex flex-wrap gap-3">
        {/* Radio buttons with options */}
    </div>
</div>
```

**Example:**
```html
🌾 Whole grains - (servings per week)
100g per serving • muesli, rice, pasta, cereal, bread
[0] [1-2] [3-4] [5+]
```

**Important:**
- NO targets shown (avoid gaming/bias)
- NO decimals in display labels (use ranges like "1-2" instead of "1.5")
- Use "+" for maximum option (e.g., "5+", "7+")

---

## 3. SCORING ALGORITHM

**Baseline**: 2400 kcal/day (EAT-Lancet 2025)
**Methodology**: PHDI 2024 linear scoring between thresholds

### Conversion Formula

```javascript
// For "servings per day" components
grams_per_day = frequency × serving_size

// For "servings per week" components
grams_per_day = (frequency × serving_size) / 7
```

### Linear Scoring Helper Function

```javascript
const linearScore = (value, minVal, maxVal, minScore, maxScore) => {
    if (value <= minVal) return minScore;
    if (value >= maxVal) return maxScore;
    return minScore + ((value - minVal) / (maxVal - minVal)) * (maxScore - minScore);
};
```

### Component Scoring (Max 10 points each)

#### OPTIMUM Components (more is better: 0 → 10 points)

| Component | Min (0 pts) | Max (10 pts) | Weight |
|-----------|-------------|--------------|--------|
| Whole grains | 0 g/day | ≥75 g/day | 1.0 |
| Vegetables | 0 g/day | ≥300 g/day | 1.0 |
| Fruits | 0 g/day | ≥200 g/day | 1.0 |
| Nuts | 0 g/day | ≥50 g/day | 1.0 |
| Soy legumes | 0 g/day | ≥50 g/day | **0.5** |
| Non-soy legumes | 0 g/day | ≥100 g/day | **0.5** |
| Fish | 0 g/day | ≥28 g/day | 1.0 |
| Vegetable oils | ≤10 g/day | ≥56 g/day | 1.0 |

**Scoring**: `score = linearScore(grams_per_day, 0, target, 0, 10)`

#### MODERATE Components (less is better: 10 → 0 points)

| Component | Max (10 pts) | Min (0 pts) | Weight |
|-----------|--------------|-------------|--------|
| Tubers | ≤50 g/day | ≥200 g/day | 1.0 |
| Dairy | ≤250 g/day | ≥1000 g/day | 1.0 |
| Chicken | ≤30 g/day | ≥100 g/day | 1.0 |
| Eggs | ≤13 g/day | ≥120 g/day | 1.0 |
| Red meat | ≤15 g/day | ≥100 g/day | 1.0 |
| Animal fats | 0 g/day | ≥30 g/day | 1.0 |
| Added sugar | ≤30 g/day | ≥150 g/day | 1.0 |

**Scoring**: `score = linearScore(grams_per_day, target, max, 10, 0)`

### Total Score Calculation

```javascript
totalScore = wholeGrains + tubers + vegetables + fruits + nuts +
             (soyLegumes × 0.5) + (nonSoyLegumes × 0.5) +
             dairy + chicken + fish + eggs + redMeat +
             vegetableOils + animalFats + addedSugar

// Maximum possible: 140 points
percentage = (totalScore / 140) × 100
```

---

## 4. RESULTS DISPLAY

### Score Display
- Large total score: **"85.5/140"**
- Percentage: **"61% alignment"**
- Visual progress bar (color-coded)

### Interpretation Messages

| Score Range | Message |
|-------------|---------|
| ≥100 (71%+) | "Excellent! Your diet aligns well with planetary health guidelines." |
| 75-99 (54-70%) | "Good progress! You're making healthy and sustainable choices." |
| 60-74 (43-53%) | "You're around average. There's room to improve for both your health and the planet." |
| 45-59 (32-42%) | "Your diet could use some adjustments to be healthier and more sustainable." |
| <45 (<32%) | "Significant changes would benefit both your health and the environment." |

### Progress Bar Colors
- Green: ≥75%
- Yellow: 60-74%
- Red: <60%

---

## 5. TOP IMPROVEMENT AREAS

Show 3-5 components with **percentage < 80%**, sorted by lowest percentage first.

### Recommendations by Component

| Component | Emoji | Recommendation |
|-----------|-------|----------------|
| Whole Grains | 🌾 | Aim for 1-2 servings (100g each) daily - brown rice, whole wheat bread, oatmeal |
| Tubers/Potatoes | 🥔 | Limit to ~1 serving (150g) per week - too much reduces score |
| Vegetables | 🥗 | Aim for 3+ servings (100g each) daily - half your plate at meals |
| Fruits | 🍎 | Aim for 2 servings (125g each) daily - berries, apples, citrus |
| Nuts and Seeds | 🥜 | Eat 1 serving (30g) daily - almonds, walnuts, pumpkin seeds |
| Soy Legumes | 🫛 | Aim for tofu, tempeh, edamame, or soy milk 3-4x/week (100g per serving) |
| Non-Soy Legumes | 🫘 | Aim for beans, lentils, chickpeas, peas daily (75g per serving) |
| Dairy | 🥛 | Aim for 1 serving daily - 1 cup milk OR 40g cheese OR 200g yogurt |
| Chicken/Poultry | 🍗 | Limit to ~2 servings (100g) per week or less |
| Fish/Seafood | 🐟 | Aim for 2 servings (100g each) per week - salmon, sardines, mackerel |
| Eggs | 🥚 | Limit to ~2 eggs per week or less |
| Red Meat | 🥩 | Limit to ~1 serving (100g) per week or less |
| Vegetable Oils | 🫒 | Use olive/canola oil for cooking ~2 times daily (30g per use) |
| Saturated Fats | 🧈 | Minimize saturated fats - avoid butter, palm oil, coconut oil, lard |
| Added Sugars | 🍬 | Minimize sugary foods/drinks - limit to ~1 serving (30g sugar) per day |

---

## 6. COMPONENT BREAKDOWN

Display all 15 components with individual scores:

**Format**:
```
[Emoji] Component Name        [Score]/[Max]
🌾 Whole Grains              7.5/10
🫛 Soy Legumes               3.2/5  ← Note: weighted 0.5
```

**Color coding by percentage:**
- Green (80-100%): Good alignment
- Yellow (40-79%): Room for improvement
- Red (0-39%): Needs attention

---

## 7. DESIGN REQUIREMENTS

### UI/UX
- Clean, modern interface with ample spacing
- Tailwind CSS utility classes only
- Section headers with colored borders (green/blue/orange)
- Radio buttons for all selections (no decimals in labels)
- Section completion indicators (✓ Complete)

### Form Validation
- "Calculate My Score" button disabled until all 15 fields completed
- Warning message showing incomplete sections
- Smooth scroll to results on calculation

### Responsive Design
- Mobile-first approach
- Single column on mobile
- Two columns for component breakdown on desktop
- Touch-friendly radio buttons (min 44×44px)

### Additional Features
- Reset button to clear form
- Brief PHDI explanation at top
- Results appear below form (not modal)
- Complete score breakdown table

---

## 8. TECHNICAL NOTES

### Data Structure
```javascript
formData = {
    fruits: '',           // per day
    vegetables: '',       // per day
    nutsSeeds: '',       // per day
    wholeGrains: '',     // per week
    potatoes: '',        // per week
    soyLegumes: '',      // per week
    nonSoyLegumes: '',   // per week
    redMeat: '',         // per week
    chicken: '',         // per week
    fish: '',            // per week
    eggs: '',            // per week
    dairy: '',           // per week
    vegetableOils: '',   // per day
    saturatedFats: '',   // per day
    addedSugars: ''      // per day
}
```

### Key Implementation Details
1. Handle both "per day" and "per week" frequency units
2. Use median values for ranges (1-2 → 1.5)
3. Use minimum value for "+" options (5+ → 5.5, 7+ → 7, 9+ → 9.33)
4. Apply 0.5 weight multiplier for legumes in total score
5. Total score out of 140, not 150

---

## 9. REMOVED COMPONENTS

The following components from the original PRD are **removed**:
- ❌ Dark green vegetables (ratio component)
- ❌ Red/orange vegetables (ratio component)

These were removed to simplify the questionnaire and align with the PHDI 2024 methodology.

---

**End of PRD v2.0**
