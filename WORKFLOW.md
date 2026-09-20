# Dolphin Cup --- Team Workflow / Командный процесс / 团队工作流程

## 1. Architecture / Архитектура / 架构

**GitHub = code. Google Drive = data and artifacts. Colab = compute.**\
**GitHub = код. Google Drive = данные и результаты. Colab =
вычисления.**\
**GitHub = 代码。Google Drive = 数据和实验产物。Colab = 计算资源。**

``` text
VS Code → GitHub → Google Colab → Shared Google Drive
  code       code       compute        data/results
```

Do not store datasets, checkpoints, models, or submissions in GitHub.\
Не хранить датасеты, checkpoints, модели и submissions в GitHub.\
不要把数据集、checkpoint、模型或提交文件存入 GitHub。

------------------------------------------------------------------------

## 2. Repository structure / Структура репозитория / 仓库结构

``` text
dolphin-cup/
├── notebooks/
│   └── dolphin_cup.ipynb
├── experiments/
│   └── experiments.csv
├── docs/
│   ├── ideas.md
│   └── findings.md
├── WORKFLOW.md
├── README.md
├── requirements.txt
└── .gitignore
```

`main` must always contain the current best reproducible version.\
`main` всегда содержит текущую лучшую воспроизводимую версию.\
`main` 必须始终保存当前最佳且可复现的版本。

------------------------------------------------------------------------

## 3. Shared Drive / Общий Drive / 共享 Drive

Every team member must have **Editor** access to the same shared project
folder and add a shortcut to **My Drive** with the same name:

Каждый участник должен иметь права **Редактора** на общую папку и
добавить её ярлык в **My Drive** под одинаковым именем:

每位成员都必须拥有共享项目文件夹的**编辑权限**，并将其快捷方式添加到
**My Drive**，所有人使用相同名称：

``` text
DolphinCup/
├── data/
│   ├── train.csv
│   └── test.csv
├── results/
├── checkpoints/
└── submissions/
```

Mount it in Colab:

Подключение в Colab:

在 Colab 中挂载：

``` python
from google.colab import drive
from pathlib import Path

drive.mount("/content/drive")

PROJECT_DIR = Path("/content/drive/MyDrive/DolphinCup")
DATA_DIR = PROJECT_DIR / "data"
RESULTS_DIR = PROJECT_DIR / "results"
CHECKPOINTS_DIR = PROJECT_DIR / "checkpoints"
SUBMISSIONS_DIR = PROJECT_DIR / "submissions"
```

------------------------------------------------------------------------

## 4. One experiment = one hypothesis / Один эксперимент = одна гипотеза / 一个实验 = 一个假设

Change **one meaningful thing at a time**.\
За один эксперимент меняем **одну существенную вещь**.\
每次实验只修改**一个关键变量**。

Good:

``` text
E012 — change ngram_range (3,5) → (4,5)
```

Bad:

``` text
E012 — change ngram + alpha + class_weight + threshold
```

Otherwise we cannot know what caused the score change.\
Иначе невозможно понять причину изменения score.\
否则无法判断究竟是哪项修改导致分数变化。

------------------------------------------------------------------------

## 5. Experiment naming / Имена экспериментов / 实验命名

Every experiment gets a unique ID:

Каждый эксперимент получает уникальный ID:

每个实验必须有唯一 ID：

``` text
E001_baseline
E002_ngram_3_4
E003_sublinear_tf
E004_threshold_tuning
```

Use the same ID for the Git branch and result directory.

Использовать тот же ID для Git-ветки и папки результатов.

Git 分支和结果文件夹使用同一个 ID。

``` text
branch:  exp/E004_threshold_tuning
Drive:   results/E004_threshold_tuning/
```

Never save different experiments into the same result directory.\
Никогда не сохранять разные эксперименты в одну папку.\
不同实验绝不能保存到同一个结果文件夹。

------------------------------------------------------------------------

## 6. Starting an experiment / Начало эксперимента / 开始实验

Always start from the latest `main`.

Всегда начинать от актуального `main`.

始终从最新的 `main` 开始。

``` bash
git checkout main
git pull
git checkout -b exp/E012_ngram_3_4
```

Edit the notebook/code in VS Code.

Изменять notebook/код в VS Code.

在 VS Code 中修改 notebook/代码。

Commit and push:

``` bash
git add .
git commit -m "E012: test ngram range 3-4"
git push -u origin exp/E012_ngram_3_4
```

------------------------------------------------------------------------

## 7. Running on Colab / Запуск в Colab / 在 Colab 上运行

Clone once:

Первый запуск:

首次运行：

``` bash
!git clone <REPOSITORY_URL>
%cd dolphin-cup
```

For later runs, update the repository:

При следующих запусках обновить репозиторий:

之后运行时更新仓库：

``` bash
!git pull
```

Checkout the experiment branch:

``` bash
!git checkout exp/E012_ngram_3_4
```

Run the notebook using the same shared dataset and validation protocol.

Запускать на общем датасете и с одинаковым validation protocol.

所有实验必须使用相同的共享数据集和验证方案。

------------------------------------------------------------------------

## 8. Team parallelization / Параллельная работа / 团队并行工作

Work on **one common pipeline**, not three independent solutions.

Работать над **одним общим pipeline**, а не над тремя независимыми
решениями.

围绕**同一条公共 pipeline**工作，而不是做三个互相独立的方案。

Current areas:

``` text
A — Features / Model
B — Validation / Thresholds
C — Diagnostics / Experiment tracking
```

Roles may rotate. Everyone should understand the full pipeline.

Роли можно менять. Каждый должен понимать весь pipeline.

角色可以轮换，每个人都应该理解完整 pipeline。

All experiments start from the same current best version:

``` text
                    main (current best)
                           │
              ┌────────────┼────────────┐
              ↓            ↓            ↓
          Experiment A  Experiment B  Experiment C
              │            │            │
              └────────────┼────────────┘
                           ↓
                      compare results
                           ↓
                       new best
                           ↓
                          main
```

------------------------------------------------------------------------

## 9. Experiment log / Журнал экспериментов / 实验记录

Every completed experiment must be recorded in:

Каждый законченный эксперимент записывается в:

每个完成的实验都必须记录到：

``` text
experiments/experiments.csv
```

Recommended columns:

``` text
id
date
branch
commit
change
macro_f1
macro_ap
precision
recall
runtime
status
notes
```

Example:

``` text
E012 | exp/E012_ngram_3_4 | a81df2 | ngram (3,4) | 0.4381 | keep
```

Use statuses:

``` text
best
keep
rejected
failed
```

One designated team member updates `experiments.csv` to avoid merge
conflicts.

Один назначенный участник обновляет `experiments.csv`, чтобы избежать
merge conflicts.

指定一名成员统一维护 `experiments.csv`，避免合并冲突。

------------------------------------------------------------------------

## 10. After an experiment / После эксперимента / 实验完成后

Compare the result only against experiments using the **same validation
split and metric**.

Сравнивать только результаты с **одинаковым validation split и
метрикой**.

只能比较使用**相同验证集划分和评价指标**的实验。

If worse:

``` text
record result → status=rejected → do not merge into main
```

Если хуже:

``` text
записать результат → rejected → не merge в main
```

如果更差：

``` text
记录结果 → rejected → 不合并到 main
```

If better:

``` text
record result → verify → merge into main
```

Если лучше:

``` text
записать → проверить → merge в main
```

如果更好：

``` text
记录 → 验证 → 合并到 main
```

After merge, this becomes the baseline for the next experiments.

После merge эта версия становится базой следующих экспериментов.

合并后，该版本成为下一轮实验的新 baseline。

------------------------------------------------------------------------

## 11. Daily routine / Ежедневный цикл / 每日流程

**Start of day:** choose 2--3 hypotheses from the current best version.\
**Начало дня:** выбрать 2--3 гипотезы от текущего best.\
**每天开始：**基于当前最佳版本选择 2--3 个实验假设。

**During the day:** run experiments in parallel on separate Colab
runtimes.\
**Днём:** параллельно запускать эксперименты в отдельных Colab runtime.\
**白天：**在各自独立的 Colab runtime 上并行运行实验。

**End of day:** record results, compare, select the new best, merge only
verified improvements.\
**Конец дня:** записать результаты, сравнить, выбрать новый best, merge
только проверенные улучшения.\
**每天结束：**记录并比较结果，选择新的最佳版本，只合并经过验证的改进。

------------------------------------------------------------------------

## 12. Rules / Правила / 规则

1.  **Never experiment directly on `main`.** / **Не экспериментировать
    напрямую в `main`.** / **不要直接在 `main` 上做实验。**
2.  **One experiment = one hypothesis.** / **Один эксперимент = одна
    гипотеза.** / **一个实验 = 一个假设。**
3.  **Same validation split and metric for everyone.** / **У всех
    одинаковые validation split и metric.** /
    **所有人使用相同的验证集划分和指标。**
4.  **Every experiment must be logged, including failed ones.** /
    **Записывать даже неудачные эксперименты.** /
    **失败的实验也必须记录。**
5.  **Do not overwrite another experiment's results.** / **Не
    перезаписывать результаты других экспериментов.** /
    **不要覆盖其他实验的结果。**
6.  **Only verified improvements go to `main`.** / **В `main` попадают
    только проверенные улучшения.** / **只有经过验证的改进才能进入
    `main`。**
7.  **GitHub stores code; Drive stores heavy artifacts.** / **GitHub
    хранит код; Drive --- тяжёлые файлы.** / **GitHub 存代码；Drive
    存大型实验文件。**
8.  **The score belongs to the team, not to a person.** / **Score
    принадлежит команде, а не человеку.** /
    **分数属于团队，而不是某个个人。**
