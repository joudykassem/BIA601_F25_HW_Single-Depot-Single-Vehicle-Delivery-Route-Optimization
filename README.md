# BIA601_F25_HW_Single-Depot-Single-Vehicle-Delivery-Route-Optimization

**BIA601 – Intelligent Algorithms · F25 · Syrian Virtual University**
**الخوارزميات الذكية · الجامعة الافتراضية السورية · إشراف: د. عصام محمد سلمان**



<div dir="rtl">



### فكرة المشروع
مندوب توصيل واحد ينطلق من مستودع واحد (Depot)، يزور كل العملاء مرة واحدة فقط، ثم يعود إلى المستودع.
المطلوب إيجاد ترتيب الزيارة الذي يعطي **أقصر مسافة إجمالية** (مسافة إقليدية). وهي مسألة من نوع TSP.

تُحل المسألة نفسها بثلاث خوارزميات ذكية وبالبيانات نفسها، ثم تُقارن نتائجها:
- **GA**: الخوارزمية الجينية.
- **PSO**: سرب الجسيمات المتقطع (Discrete PSO).
- **ACO**: مستعمرة النمل (Ant System).

### محتويات المجلد
```
ITE_F25_BIA601_HW/
├── ITE_F25_BIA601_HW.docx            ← التقرير النهائي (Word)
└── Algorithms&data/
    ├── GA_PSO_ACO.ipynb              ← الكود الكامل: GA وPSO وACO والتجارب والمقارنة
    ├── Delivery_Route_Test_Data.xlsx ← بيانات الاختبار (10 و30 و100 عميل)
    └── results/three_run_comparison/ ← النتائج التي ينتجها الكود
```

### المتطلبات
- Python 3.10 أو أحدث (تمت التجربة على Python 3.13).
- المكتبات: numpy وpandas وmatplotlib وopenpyxl وjupyter.

### خطوات التشغيل
1. ثبّت Python من الموقع الرسمي python.org.
2. افتح الطرفية (Terminal / CMD) داخل المجلد `ITE_F25_BIA601_HW` وثبّت المكتبات:
```bash
pip install numpy pandas matplotlib openpyxl jupyter
   ```
3. افتح الملف `Algorithms&data/GA_PSO_ACO.ipynb` باستخدام Jupyter أو VS Code:
   ```bash
   jupyter notebook "Algorithms&data/GA_PSO_ACO.ipynb"
   ```
4. اختر **Run All** (تشغيل كل الخلايا). لا حاجة لأي تدخل يدوي.
5. يستغرق التشغيل الكامل من نصف دقيقة إلى دقيقتين تقريباً حسب الجهاز.

ملف البيانات موجود بجانب الكود، ويعثر عليه الكود تلقائياً.

### ماذا يحدث عند التشغيل؟
- **الأقسام 1 و2 و3:** تعريف الخوارزميات الثلاث (GA ثم PSO ثم ACO) وتحميل البيانات.
- **القسم 4:** تشغيل الخوارزميات الثلاث على الأحجام الثلاثة (10 و30 و100 عميل)، وتكرار ذلك **3 مرات تلقائياً** ببذور عشوائية مختلفة (42 و123 و456).
  - المجموع 27 تشغيلاً.
  - يتحقق الكود من أن كل مسار ناتج صالح، أي أن كل عميل يظهر مرة واحدة بالضبط، ويعيد حساب مسافته.
- **في النهاية يعرض الكود:**
  - جدول المسافات لكل تشغيل.
  - جدولاً إحصائياً لكل خوارزمية وحجم: أفضل وأسوأ نتيجة، والمتوسط، والانحراف المعياري، ومتوسط الزمن والتقييمات والدورات.
  - منحنيات التقارب Iteration → Best Fitness.

### الملفات الناتجة (`Algorithms&data/results/three_run_comparison/`)
| الملف | المحتوى |
|---|---|
| `three_run_raw_results.csv` | نتيجة كل تشغيل: الخوارزمية، الحجم، البذرة، أفضل مسافة، الزمن، عدد التقييمات، سبب التوقف، والمسار |
| `three_run_summary.csv` | الملخص الإحصائي لكل خوارزمية وحجم |
| `three_run_convergence.csv` | أفضل مسافة في كل دورة (لرسم منحنيات التقارب) |
| `iteration_vs_best_fitness_10/30/100.png` | منحنيات التقارب للخوارزميات الثلاث لكل حجم |

### إعدادات المقارنة العادلة (مشتركة بين الخوارزميات الثلاث)
| الإعداد | القيمة |
|---|---|
| حجم المجتمع / السرب / المستعمرة | 60 |
| الحد الأقصى لتقييمات دالة الهدف | 18,000 |
| التوقف المبكر | بعد 2,400 تقييم دون تحسن |
| دالة الهدف | المسافة الإجمالية: المستودع ← العملاء بالترتيب ← المستودع |

**المعاملات الخاصة بكل خوارزمية:**
- **GA:** معدل الطفرة 0.1 (تبديل عميلين). الآباء من النصف الأفضل، وتصالب يحافظ على الترتيب، ويُنقل أفضل حلين كما هما للجيل التالي.
- **PSO:** w = 0.7، c1 = 1.5، c2 = 1.5، v_max = 10، احتمال الاستكشاف 0.10.
- **ACO:** عدد النمل 60، α = 1، β = 3، ρ = 0.10، Q = 100، τ₀ = 1.

### تعديل التجربة
كل الإعدادات موجودة داخل الكود ويمكن تغييرها قبل التشغيل:
- **أحجام المسألة والبذور والميزانية:** المتغيرات `COMPARISON_SIZES` و`EXPERIMENT_SEEDS` و`COMPARISON_BUDGET` و`COMPARISON_PATIENCE` في أول خلية من القسم 4.
- **معاملات PSO:** القاموس `PSO_PARAMS` في أول خلية من القسم 2.
- **معاملات ACO:** القاموس `ACO_PARAMS` في أول خلية من القسم 3.
- **معاملات GA:** `pop_size` و`mutation_rate` في استدعاء `run_genetic_algorithm` داخل خلية القسم 4.

### النتائج (متوسط أفضل مسافة ± الانحراف المعياري على 3 تشغيلات، القيمة الأقل أفضل)
| عدد العملاء | GA | PSO | ACO |
|---|---|---|---|
| 10 | 269.33 ± 3.17 | 274.55 ± 2.85 | **260.93 ± 0.00** |
| 30 | 637.38 ± 37.55 | 713.71 ± 83.42 | **471.63 ± 0.12** |
| 100 | 2161.27 ± 132.90 | 2458.43 ± 20.02 | **829.86 ± 11.14** |

حققت ACO أقصر مسافة وأقل تذبذب في الأحجام الثلاثة. الشرح الكامل والمناقشة في التقرير `ITE_F25_BIA601_HW.docx`.

### إعادة التجربة
- البذور ثابتة، لذلك تعطي إعادة التشغيل **المسافات نفسها تماماً**.
- أزمنة التنفيذ تختلف من جهاز لآخر ومن تشغيل لآخر.

### فريق العمل
| المهمة | الطلاب |
|---|---|
| الخوارزمية الجينية (GA) مع الشرح | جودي قاسم، محمد العريضي |
| سرب الجسيمات (PSO) مع الشرح | عامر نعيم، نغم شلغين |
| مستعمرة النمل (ACO) مع الشرح | أحمد العضم، علي ديب |
| التقرير | آية مسعود، سارة طالب |
| ملف README | محمد أبي خولاني |

</div>

---

## English

### The problem
A single delivery vehicle starts at one depot, visits every customer exactly once, and returns to the depot.
The goal is the visiting order with the **shortest total Euclidean distance** (a TSP-like problem).

The same problem and the same data are solved with three intelligent algorithms, and their results are compared:
- **GA**: Genetic Algorithm.
- **PSO**: Discrete Particle Swarm Optimization (swap-based).
- **ACO**: Ant Colony Optimization (Ant System).

### Folder contents
```
ITE_F25_BIA601_HW/
├── ITE_F25_BIA601_HW.docx            ← final report (Word)
└── Algorithms&data/
    ├── GA_PSO_ACO.ipynb              ← full code: GA, PSO, ACO, experiments and comparison
    ├── Delivery_Route_Test_Data.xlsx ← test data (10, 30 and 100 customers)
    └── results/three_run_comparison/ ← outputs produced by the notebook
```

### Requirements
- Python 3.10 or newer (tested with Python 3.13).
- Packages: numpy, pandas, matplotlib, openpyxl, jupyter.

### How to run
1. Install Python from python.org.
2. Open a terminal in the `ITE_F25_BIA601_HW` folder and install the packages:
```bash
pip install numpy pandas matplotlib openpyxl jupyter
   ```
3. Open `Algorithms&data/GA_PSO_ACO.ipynb` in Jupyter or VS Code:
   ```bash
   jupyter notebook "Algorithms&data/GA_PSO_ACO.ipynb"
   ```
   (Keep the quotes: the folder name contains `&`.)
4. Choose **Run All**. No manual steps are needed.
5. A full run takes about 30 seconds to 2 minutes, depending on the machine.

The data file sits next to the notebook and is found automatically.

### What the notebook does
- **Sections 1–3:** define GA, PSO and ACO, and load the data.
- **Section 4:** runs all three algorithms on 10, 30 and 100 customers, and repeats this **3 times automatically** with different seeds (42, 123, 456).
  - That is 27 runs in total.
  - Every returned route is checked to contain each customer exactly once, and its distance is recomputed.
- **At the end it shows:**
  - The distance of every run.
  - A statistics table per algorithm and size: best, worst, mean, standard deviation, and mean time, evaluations and iterations.
  - The Iteration → Best Fitness convergence curves.

### Output files (`Algorithms&data/results/three_run_comparison/`)
| File | Contents |
|---|---|
| `three_run_raw_results.csv` | One row per run: algorithm, size, seed, best distance, time, evaluations, stopping reason, route |
| `three_run_summary.csv` | Statistics per algorithm and problem size |
| `three_run_convergence.csv` | Best distance at every iteration (used for the convergence curves) |
| `iteration_vs_best_fitness_10/30/100.png` | Convergence curves of the three algorithms for each size |

### Fair-comparison settings (shared by all three algorithms)
| Setting | Value |
|---|---|
| Population / swarm / colony size | 60 |
| Maximum objective evaluations | 18,000 |
| Early stopping | after 2,400 evaluations without improvement |
| Objective | total distance: depot → customers in order → depot |

**Algorithm-specific parameters:**
- **GA:** mutation rate 0.1 (swap two customers). Parents come from the better half, order-preserving crossover is used, and the 2 best routes are kept unchanged in the next generation.
- **PSO:** w = 0.7, c1 = 1.5, c2 = 1.5, v_max = 10, exploration probability 0.10.
- **ACO:** 60 ants, α = 1, β = 3, ρ = 0.10, Q = 100, τ₀ = 1.

### Changing the experiment
All settings are in the notebook and can be changed before running:
- **Problem sizes, seeds and budget:** `COMPARISON_SIZES`, `EXPERIMENT_SEEDS`, `COMPARISON_BUDGET` and `COMPARISON_PATIENCE` in the first cell of section 4.
- **PSO parameters:** the `PSO_PARAMS` dictionary in the first cell of section 2.
- **ACO parameters:** the `ACO_PARAMS` dictionary in the first cell of section 3.
- **GA parameters:** `pop_size` and `mutation_rate` in the `run_genetic_algorithm` call inside the section 4 cell.

### Data format
`Delivery_Route_Test_Data.xlsx` contains synthetic data:
- Sheets `Customers_10`, `Customers_30` and `Customers_100`.
- Columns `Node_ID`, `Node_Type` (`Depot` / `Customer`), `X` and `Y`.
- The depot is at (50, 50), and coordinates range from 0 to 100.

The notebook looks for the file in a `data/` folder first, then next to the notebook, searching the current folder and its parents. Results are written to `results/three_run_comparison/` next to the data file it finds.

### Results (mean best distance ± standard deviation over 3 runs, lower is better)
| Customers | GA | PSO | ACO |
|---|---|---|---|
| 10 | 269.33 ± 3.17 | 274.55 ± 2.85 | **260.93 ± 0.00** |
| 30 | 637.38 ± 37.55 | 713.71 ± 83.42 | **471.63 ± 0.12** |
| 100 | 2161.27 ± 132.90 | 2458.43 ± 20.02 | **829.86 ± 11.14** |

ACO found the shortest routes with the smallest spread at every size. The full explanation and discussion are in `ITE_F25_BIA601_HW.docx`.

### Reproducibility
- The seeds are fixed, so re-running gives **exactly the same distances**.
- Execution times vary between machines and runs.

### Team
| Task | Students |
|---|---|
| Genetic Algorithm (GA) and its explanation | جودي قاسم، محمد العريضي |
| Particle Swarm Optimization (PSO) and its explanation | عامر نعيم، نغم شلغين |
| Ant Colony Optimization (ACO) and its explanation | أحمد العضم، علي ديب |
| Report | آية مسعود، سارة طالب |
| README | محمد أبي خولاني |
