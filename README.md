# Определение липидного профиля по фотоплетизмограмме

> **1-е место** на ИИ-хакатоне МФТИ × Сбер 2025 (по результатам Яндекс контеста и питчинга)

---

## О задаче

Дислипидемия — нарушение липидного обмена — часто протекает бессимптомно, за что получила название «тихий убийца». Задача хакатона — по сигналу фотоплетизмограммы (PPG) определить, повышен ли уровень ЛПНП (липопротеинов низкой плотности) у пациента.

**Входные данные:** временные ряды PPG-сигналов (`.npy`), бинарная метка `ЛПНП` (0/1).  
**Метрика:** комбинированный скор на основе AUC-ROC.

## Подход

### 1. Выделение локальных экстремумов

Вместо работы с «сырым» сигналом из тысяч точек, извлекаются только локальные минимумы и максимумы — ключевые точки пульсовой волны. Это убирает шум и фокусирует модель на значимых паттернах.

### 2. Аугментация данных

Для борьбы с ограниченным объёмом выборки применяются четыре вида аугментаций:

- Масштабирование амплитуды (×0.9–1.1)
- Слабый гауссов шум (σ = 2.2% от std сигнала)
- Случайный нециклический сдвиг
- Отзеркаливание

### 3. Расчёт физиологических индексов (per-beat features)

Для каждого удара сердца рассчитываются клинически значимые параметры жёсткости артериальной стенки: индекс ригидности (SI), индекс аугментации (AI), индекс отражения (RI), время пика, время нарастания, ширина импульса, площади систолической и диастолической фаз, а также частота пульса.

### 4. Классификация

- **Основная модель** (`final_submit.ipynb`): Residual CNN с механизмом внимания, адаптированная из winning-решения PhysioNet/CinC Challenge 2021 для работы с PPG. PyTorch, 35 эпох, batch size 50.
- **Исследовательская модель** (`Per_beat_feature_analysis.ipynb`): Random Forest на извлечённых per-beat фичах с вейвлет-денойзингом — использовалась для анализа значимости признаков и валидации гипотез.

Использована нейросеть (PPG_NN). Residual CNN (4 блока, ядро 1x9, LeakyReLU, BatchNorm) + Multi-Head Self-Attention (8 голов) + Adaptive Max Pooling → линейный классификатор. 35 эпох, батч 50, BCEWithLogitsLoss, Adam.

## Стек

Python, PyTorch, scikit-learn, NumPy, Pandas, SciPy, PyWavelets, Matplotlib

## Команда «Профилакторий»

| Имя | Вуз | Контакт |
|-----|-----|---------|
| Курдина Анна | НИЯУ МИФИ, бизнес-информатика | [@anut_ku](https://t.me/anut_ku) |
| Скударь Мария | НИУ ВШЭ, геоинформатика | [@skudarmaria](https://t.me/skudarmaria) |
| Кураев Кирилл | ДВФУ ИМКТ ПМИ | [@Yust_R2hev](https://t.me/Yust_R2hev) |
| Кленова Анастасия | НИУ ВШЭ, компьютерная лингвистика | [@stasik1219](https://t.me/stasik1219) |

## Файлы

**final_submit.ipynb** — полный pipeline: фильтрация, экстремумы, аугментация, обучение Residual CNN, сабмит.

**Per_beat_feature_analysis.ipynb** — извлечение per-beat фичей, вейвлет-денойзинг, Random Forest.

_______________________________________________________________________________
<img src="https://github.com/user-attachments/assets/086520c7-2921-443e-b264-8736b616b949" width="30%">

<img src="https://github.com/user-attachments/assets/6b8237a3-f363-4059-a562-91ff53232c0d" width="70%">

<img src="https://github.com/user-attachments/assets/0b40f122-e4a3-4932-a841-326ab159d8c9" width="70%">

<img src="https://github.com/user-attachments/assets/85bfb8b6-453a-4d04-88b7-80923f9fb699" width="70%">

<img src="https://github.com/user-attachments/assets/2bbac0b2-9db6-4c12-b6d3-5afb29102e49" width="70%">

<img src="https://github.com/user-attachments/assets/9e06a215-ed87-4106-97c4-3626e80b80fd" width="70%">

<img src="https://github.com/user-attachments/assets/314ccc73-8166-419f-a98e-15934f0622a1" width="70%">

<img src="https://github.com/user-attachments/assets/ccf12f55-a607-4c3f-bd8d-686fa8204ae2" width="70%">

<img src="https://github.com/user-attachments/assets/b539e0c8-855b-4a50-a70a-b7faa13a3916" width="70%">

<img src="https://github.com/user-attachments/assets/da6e85e5-e204-4c6d-983b-94dc94425157" width="70%">

<img src="https://github.com/user-attachments/assets/85ad6126-4f15-487e-be05-9296d4d773e6" width="70%">

<img src="https://github.com/user-attachments/assets/09cdd1f7-efba-4433-b790-9d2fab5886ef" width="70%">

<img src="https://github.com/user-attachments/assets/95111fe8-2458-468c-b3ed-26ca517e62b5" width="70%">



