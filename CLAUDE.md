# Family Meal Planner — Claude Context File
> Drop this file in the root of your GitHub repo. Claude will use it as context at the start of every session.

---

## 🏗️ PROJECT OVERVIEW

A progressive web app (PWA) for weekly family meal planning, shopping list generation, lunchbox ideas and a growing family recipe book. Built to work on mobile as the primary device.

**Live URL:** https://guamboy.github.io/family-meal-planner
**GitHub Repo:** https://github.com/guamboy/family-meal-planner
**Stack:** Vanilla HTML/CSS/JS · Supabase (Postgres) · GitHub Pages

---

## 🔧 TECH STACK

| Layer | Tool | Details |
|---|---|---|
| Frontend | Single `index.html` | Vanilla JS, no framework |
| Database | Supabase | Free tier, Postgres |
| Hosting | GitHub Pages | Auto-deploys on push to `main` |
| Version Control | GitHub | `github.com/guamboy/family-meal-planner` |
| Future | VS Code + Claude | Continuing development here |

### Supabase Config
```javascript
const SUPABASE_URL = 'https://uwomdntwazcflikqnxeo.supabase.co'
const SUPABASE_ANON_KEY = 'YOUR_KEY_IN_LOCAL_ENV' // Never commit real key
```

> ⚠️ The anon key is safe to include in client-side code (it's public by design)
> but keep it out of this context file. Store it in a local `.env` or just edit
> `index.html` directly before each push.

---

## 🗄️ DATABASE SCHEMA

```sql
-- All meals ever selected (the growing recipe book)
create table meals (
  id uuid default gen_random_uuid() primary key,
  name text not null,
  source text,                        -- e.g. 'RecipeTin Eats'
  url text,                           -- link to recipe
  category text,                      -- Stew, Roast, Seafood, Fish, Chicken etc
  notes text,                         -- cooking tips
  cook_time text,                     -- e.g. '2h 45m'
  kid_friendly boolean default true,
  created_at timestamp default now()
);

-- Links meals to a specific week
create table weekly_plans (
  id uuid default gen_random_uuid() primary key,
  week_number integer not null,
  week_label text,                    -- e.g. 'Week 1'
  meal_id uuid references meals(id),
  serving_notes text,                 -- e.g. 'served over rice'
  created_at timestamp default now()
);

-- Shopping list items per week
create table shopping_items (
  id uuid default gen_random_uuid() primary key,
  week_number integer not null,
  section text not null,              -- e.g. '🥩 Meat & Seafood'
  item_text text not null,
  item_note text,                     -- e.g. 'Buy fresh day-of'
  is_checked boolean default false,
  created_at timestamp default now()
);

-- Lunchbox ideas (grows over time)
create table lunchbox_ideas (
  id uuid default gen_random_uuid() primary key,
  name text not null,
  style text,                         -- Thermos Leftover, Wrap, Bento, Fresh Thermos
  based_on text,                      -- e.g. 'Dinner #1 — Beef Stew'
  notes text,
  created_at timestamp default now()
);
```

---

## 👨‍👩‍👧 FAMILY PROFILE

| | |
|---|---|
| **Household** | 2 adults + 1–2 kids |
| **Kids age** | Primary school (6–12) |
| **Diet style** | Meat heavy — this is intentional and not to change |
| **Protein focus** | High protein, animal fat welcomed |
| **Goals** | Increasing fish & seafood gradually |
| **Veg** | Want more but needs to be interesting for kids |
| **Allergies** | None |
| **Flavour — adults** | Adventurous |
| **Flavour — kids** | Milder but gradually pushing them |
| **Lunchbox** | Nut-free required · No reheating at school · Has thermos |

---

## 🗓️ WEEKLY MEAL PLANNING RULES

Each week suggest:
- **3–4 dinners** — family friendly, kids eat happily
- **2 dinners** — gentle flavour push for kids (familiar format, bolder flavours)
- **1 dinner** — more adult/adventurous
- **Variety across:** Stew · Roast · Rice dishes · Fish/Seafood · Soup · Meat & veg

Always:
- Link to **well-rated recipes with great photos**
- Preferred sources: **RecipeTin Eats, Serious Eats, BBC Good Food**
- Note cook time, kid-friendliness, and any useful tips
- Flag which meals generate good **lunchbox leftovers**

---

## 📋 MEALS HISTORY

### ✅ WEEK 1 — Confirmed

| # | Meal | Source | Notes |
|---|------|--------|-------|
| 1 | Classic Beef Stew | RecipeTin Eats | Dutch oven · [Recipe](https://www.recipetineats.com/beef-stew/) |
| 2 | Garlic-Herb Butter Roast Chicken | RecipeTin Eats | Pan juice sauce · [Recipe](https://www.recipetineats.com/roast-chicken/) |
| 3 | Creamy Garlic Prawns | RecipeTin Eats | Over rice · [Recipe](https://www.recipetineats.com/creamy-garlic-prawns-shrimp/) |
| 4 | Salmon Lemon Garlic Tray Bake | RecipeTin Eats | One pan, asparagus & tomatoes · [Recipe](https://www.recipetineats.com/lemon-garlic-salmon-tray-bake-easy-healthy/) |
| 5 | Mediterranean Baked Chicken | RecipeTin Eats | One pan, potatoes included · [Recipe](https://www.recipetineats.com/mediterranean-baked-chicken-dinner/) |

---

## 🎒 LUNCHBOX IDEAS — Week 1

| # | Name | Style | Based On |
|---|------|-------|----------|
| L1 | Beef Stew in Thermos | Thermos Leftover | Dinner #1 |
| L2 | Shredded Roast Chicken Rice Bowl | Thermos Leftover | Dinner #2 |
| L3 | Mediterranean Chicken & Potato Thermos | Thermos Leftover | Dinner #5 |
| L4 | Roast Chicken Wrap | Wrap | Dinner #2 |
| L5 | Salmon Cream Cheese Wrap | Wrap | Dinner #4 |
| L6 | The Protein Bento Box | Bento | — |
| L7 | The Grazing Box | Bento | — |
| L8 | Buttered Pasta with Parmesan | Fresh Thermos | — |
| L9 | Chicken & Rice Soup | Fresh Thermos | Dinner #2 |

---

## 📱 APP FEATURES — Current State

### ✅ Built & Live
- Shopping list with live Supabase sync (checks persist across devices)
- Weekly meal plan view with recipe links
- Lunchbox ideas panel
- Recipe book (auto-grows as weeks are added)
- Week selector chip bar (ready for Week 2+)
- PWA manifest (add to phone home screen)
- Progress bar tracking shopping completion
- Auto-seeds Week 1 data on first load
- Reset week functionality

### 🔜 Planned Features (add week by week)
- [ ] Week 2+ meal selection flow
- [ ] Shopping list auto-builds from meal selection
- [ ] Meal picker UI (select from suggested meals each week)
- [ ] Favourite meals flagging (⭐)
- [ ] Filter recipe book by category / kid-friendly
- [ ] Notes per meal (what the family thought)
- [ ] Serving size adjuster
- [ ] Push notifications for meal prep reminders

---

## 🔄 WEEKLY WORKFLOW

### In Claude (each week)
1. Paste this file at start of conversation
2. Say "Ready for Week X meal suggestions"
3. Claude suggests 7 meals across categories
4. Select 5–6 using the multi-select widget
5. Claude generates shopping list additions
6. Claude outputs updated `index.html` with new week data seeded

### Deploying Updates
```bash
cd ~/Documents/family-meal-planner
cp ~/Downloads/index.html .
git add index.html
git commit -m "Week X - [brief description]"
git push origin main
# GitHub Pages auto-publishes in ~30 seconds
```

---

## 💡 CLAUDE INSTRUCTIONS FOR THIS PROJECT

When continuing this project, Claude should:

1. **Always check this file first** for family profile, meal history and app state
2. **Never suggest meals already in the history** above unless explicitly asked
3. **Always provide recipe links** — no recipe suggestions without a URL
4. **Use the multi-select widget** for meal selection every week
5. **Generate shopping lists** split into Market/Fresh and Pantry/Supermarket
6. **Update the meals history** table in this file when new weeks are confirmed
7. **Output complete `index.html`** — never partial code snippets for the app
8. **Maintain Supabase integration** — all new weeks should seed data via the existing pattern
9. **Keep the single-file architecture** — no separate CSS/JS files until explicitly migrated to a framework
10. **Test mentally** that new week data seeds correctly before outputting code

---

## 📁 REPO STRUCTURE

```
family-meal-planner/
├── index.html          # The entire app (single file)
├── CLAUDE.md           # This file — Claude context
└── README.md           # GitHub repo readme
```

---

*Last updated: Week 1 complete · App live on GitHub Pages · Supabase connected*
