# OnnoMeal (V1): Early Prototype of OnnoMitro

> **This is an early prototype.** The final version of this project is **[OnnoMitro](https://github.com/Sh-Ifti/onnomitro-meal-recommender)**, our CSE 499 senior design project at North South University.

A web app that builds **personalized daily meal plans from traditional Bangladeshi food**. It estimates each user's nutritional needs from their body measurements, activity level and goal, then recommends recipes from a curated database of **305 Bangladeshi dishes** that best match those targets.

Early prototype of the CSE 499 Senior Design Project, North South University · 2024. Shahariar Ifti, Tasin Tayeba Khan, Moshiur Rahman · Supervised by Dr. Sifat Momen

## Why
Most diet and meal-planning apps are built around Western foods, portion sizes and ingredients that are unfamiliar or hard to find in Bangladesh. OnnoMeal ("onno" means rice/food in Bangla) starts from what people here actually cook and eat: biryani, bhuna, bhorta, dal, pitha.

## How recommendations work
1. **Energy needs:** basal metabolic rate from the Mifflin–St Jeor equation, multiplied by an activity factor (sedentary 1.2 → super active 1.9)
2. **Goal adjustment:** −20% calories for weight loss, −40% for extreme weight loss, +20% for weight gain
3. **Macronutrient targets:** 50% carbohydrate, 25% protein, 25% fat, plus 25 g fiber per day
4. **Per-meal budget:** breakfast 25%, lunch 35%, dinner 30%, snacks 10% of daily calories
5. **Ranking:** filter recipes by meal type and dietary preference (e.g. vegetarian), then score each by its weighted distance from the meal's nutrient targets (calories 0.4, protein 0.2, carbohydrate 0.2, fat 0.1, fiber 0.1) and recommend the closest three

The app also computes BMI, tracks the meals a user selects against their remaining daily calories, keeps a meal history, and lets users swap a recommendation for alternatives.

## Recipe database
`recipes_final.csv`: 305 recipes with ingredients, step-by-step instructions, serving size, prep and cook times, meal-type tags (breakfast, lunch, dinner, snack), a vegetarian flag, and per-serving energy, carbohydrate, protein, fat and fiber.

## Features
- Sign up and profile (age, sex, height, weight, activity level, goal, dietary preference)
- Personalized daily meal plan and calorie tracking
- Recipe browsing with full details and nutrition
- Meal history
- Admin dashboard to add, edit and delete recipes

## Tech stack
**Backend:** Python, Flask, SQLAlchemy, Flask-Migrate (Alembic), MySQL
**Recommendation:** pandas, NumPy
**Frontend:** HTML/CSS, JavaScript, Jinja templates

## Repository structure
```
app.py                  Flask app: routes for auth, profile, meal plans, recipes, admin
models.py               Database models (User, Recipe, Admin)
backend/
  recommendation.py     Nutrition calculation and recipe ranking (DietRecommender)
templates/              Page templates (dashboard, recommendations, recipes, admin...)
frontend/, static/      CSS and JavaScript
recipes_final.csv       Recipe and nutrition database
import_recipes.py       Loads recipes into the database
database.sql            MySQL schema
migrations/             Database migrations
docs/feature_ideas.md   Planned features for later versions
```

## How to run
Requires Python 3.8+ and a local MySQL server.

```bash
git clone https://github.com/Sh-Ifti/OnnoMeal_V1.git
cd OnnoMeal_V1
pip install -r requirements.txt

# Create the database (the app expects a database named diet_database)
mysql -u root -e "CREATE DATABASE diet_database"
mysql -u root diet_database < database.sql

python app.py
```
Then open http://127.0.0.1:5000. Database credentials are set in `app.py` (`SQLALCHEMY_DATABASE_URI`).

> V1 is a demo for local use. Before any public deployment it needs a real secret key from an environment variable, hashed passwords on every login path, and the default admin account removed.

## Team
Shahariar Ifti · Tasin Tayeba Khan · Moshiur Rahman · Supervised by Dr. Sifat Momen

## License
Code is released under the MIT License (see `LICENSE`). Recipe images link to their original sources.
