# NYC Energy Star Score — регрессия / regression

> **RU.** Учебный проект по регрессионному предсказанию рейтинга
> энергоэффективности зданий Нью-Йорка (`ENERGY STAR Score`).
>
> **EN.** A study project that predicts the energy-efficiency rating of New
> York City buildings (`ENERGY STAR Score`) with linear regression models.

---

## Описание / Overview

**RU.** В работе строятся две линейные модели — `LinearRegression` и
`LinearSVR` — для предсказания рейтинга `ENERGY STAR Score` зданий
Нью-Йорка. Источник данных — публичный отчёт *Energy and Water Data
Disclosure for Local Law 84* за 2016 год. Помимо обучения моделей,
ноутбук показывает полный цикл подготовки данных: чистку типов,
обработку пропусков и выбросов, отбор признаков несколькими методами
(`SelectKBest` + `chi²`, важность признаков `ExtraTreesClassifier`,
анализ корреляционной матрицы) и сборку препроцессинг-пайплайна
(`ColumnTransformer` + `StandardScaler` + `OneHotEncoder`).

**EN.** The project trains two linear models — `LinearRegression` and
`LinearSVR` — to predict the `ENERGY STAR Score` of New York City
buildings. The data comes from the public *Energy and Water Data
Disclosure for Local Law 84* report (2016). Beyond model training, the
notebook walks through a full data-prep cycle: dtype coercion,
missing-value and outlier handling, multi-method feature selection
(`SelectKBest` with `chi²`, `ExtraTreesClassifier` importances,
correlation-matrix analysis) and a preprocessing pipeline
(`ColumnTransformer` + `StandardScaler` + `OneHotEncoder`).

## Датасет / Dataset

- **Источник / Source:** <https://www1.nyc.gov/html/gbee/html/plan/ll84_scores.shtml>
- **Зеркало (Google Sheets export):** загружается в первой секции ноутбука /
  downloaded automatically in the first section of the notebook.
- **Целевая переменная / Target:** `ENERGY STAR Score` — целочисленный
  рейтинг 1–100, где 100 — наиболее энергоэффективные здания.

## Стек / Stack

- Python 3.11
- `numpy`, `pandas`
- `matplotlib`, `seaborn`
- `scikit-learn` (`LinearRegression`, `LinearSVR`,
  `SelectKBest`/`chi2`, `ExtraTreesClassifier`,
  `ColumnTransformer`, `Pipeline`, `StandardScaler`,
  `OneHotEncoder`)
- `requests`

## Структура / Structure

```
nyc-energy-star-score/
├── README.md
└── nyc_energy_star_score.ipynb
```

Логические разделы ноутбука / notebook sections:

1. Импорты и настройки / Imports and setup
2. Загрузка данных / Data loading
3. Первичный осмотр / Initial inspection
4. Конвертация типов / Type conversion
5. Анализ пропусков / Missing values
6. Удаление колонок с >50% пропусков / Drop columns with >50% missing
7. Разделение на числовые и категориальные / Numeric vs. categorical split
8. Заполнение пропусков медианой / Imputation
9. Удаление аномальной строки / Outlier row drop
10. Оценка категориальных признаков / Categorical feature overview
11. Выделение целевой переменной / Target extraction
12. Поиск отрицательных значений и удаление координат / Sanity check & coordinate drop
13. Отбор признаков / Feature selection
14. Анализ мультиколлинеарности / Multicollinearity analysis
15. Объединение признаков / Concatenate features
16. Train/val split + ColumnTransformer
17. Вспомогательная функция оценки / Evaluation helper
18. LinearRegression
19. LinearSVR
20. Сравнение моделей / Model comparison
21. Выводы / Conclusions

## Результаты / Results

**RU.**

- Топ-5 признаков, наиболее сильно влияющих на `ENERGY STAR Score`
  (по согласию `chi²` и feature importances `ExtraTreesClassifier`):
  `Site EUI`, `Source EUI`, `Weather Normalized Site EUI`,
  абсолютное потребление природного газа / электричества,
  совокупные выбросы парниковых газов.
- `LinearRegression` с `positive=True` показывает более устойчивое
  предсказание, чем `LinearSVR` на стандартных гиперпараметрах
  (детальные метрики MAE / RMSE / R² — в таблице сравнения в ноутбуке).

**EN.**

- The top-5 drivers of `ENERGY STAR Score` (where `chi²` and
  `ExtraTreesClassifier` importances agree) are: `Site EUI`,
  `Source EUI`, `Weather Normalized Site EUI`, absolute natural-gas /
  electricity consumption, and total greenhouse-gas emissions.
- `LinearRegression` with `positive=True` produces a more stable fit
  than an out-of-the-box `LinearSVR` (see the comparison table at the
  end of the notebook for MAE / RMSE / R²).

## Как запустить / How to run

```bash
# 1. Создать виртуальное окружение / create a virtual environment
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate

# 2. Установить зависимости / install dependencies
pip install numpy pandas matplotlib seaborn scikit-learn requests jupyter

# 3. Открыть ноутбук / open the notebook
jupyter notebook nyc_energy_star_score.ipynb
```

**RU.** Датасет (~1 МБ) скачивается автоматически в первой секции
ноутбука; интернет нужен только при первом запуске.

**EN.** The dataset (~1 MB) is downloaded automatically in the first
section of the notebook; an internet connection is only needed on the
first run.
