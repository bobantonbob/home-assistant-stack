# Lesson 12 — Sun Day Progress Template and Card

У цьому занятті ми додаємо на Home Assistant 3D Dashboard красиву шкалу руху сонця.

Ідея проста: спочатку створюємо окремий `template sensor`, який рахує прогрес світлового дня у відсотках, а потім виводимо цей сенсор на дашборд через `custom:button-card` у вигляді красивої лінії з іконкою сонця.

---

## Що отримуємо в результаті

Після додавання цього коду в Home Assistant у нас з’являється нова сутність:

```yaml
sensor.sun_day_progress
```

Вона показує прогрес світлового дня від `0%` до `100%`.

Наприклад:

```text
0%   — початок світлового дня
50%  — середина дня
100% — кінець світлового дня
```

На дашборді це виглядає як анімована шкала, по якій рухається сонце.

---

## Файли в цій папці

```text
lesson-12-sun-progress-template-and-card/
│
├── 01-sun-day-progress-template-sensor.yaml
├── 02-sun-day-progress-button-card.yaml
├── dashboard_full (1).png
├── sun-day-progress-card-explained.png
├── sun-day-progress-template-explained.png
├── sun-day-progress.png
└── README.md
```

---

## 01-sun-day-progress-template-sensor.yaml

Це перший файл, який створює сам сенсор прогресу дня.

Саме цей код потрібно додати у `template` Home Assistant.

### Що робить цей файл

Файл створює сенсор:

```yaml
sensor.sun_day_progress
```

Він бере дані зі стандартної сутності Home Assistant:

```yaml
sun.sun
```

Home Assistant уже знає, коли буде схід сонця, коли буде захід сонця, і чи зараз сонце знаходиться над горизонтом.

Далі template sensor рахує, скільки відсотків світлового дня вже пройшло.

---

## Куди вставляти перший файл

Є два варіанти.

### Варіант 1 — якщо template у вас прямо в configuration.yaml

Тоді код вставляється в блок:

```yaml
template:
  - sensor:
      # сюди вставляємо код з файлу 01-sun-day-progress-template-sensor.yaml
```

Приклад:

```yaml
template:
  - sensor:
      - name: Sun Day Progress
        unique_id: sun_day_progress
        unit_of_measurement: "%"
        state: >
          {% set now_ts = as_timestamp(now()) %}
          {% set next_rising = as_timestamp(state_attr('sun.sun', 'next_rising')) %}
          {% set next_setting = as_timestamp(state_attr('sun.sun', 'next_setting')) %}

          {% if is_state('sun.sun', 'above_horizon') %}
            {% set sunrise = next_rising - 86400 %}
            {% set sunset = next_setting %}
            {% set progress = ((now_ts - sunrise) / (sunset - sunrise)) * 100 %}
            {{ [0, [progress, 100] | min] | max | round(1) }}
          {% else %}
            0
          {% endif %}
```

### Варіант 2 — якщо у вас template винесений в окремий файл

У `configuration.yaml` може бути так:

```yaml
template: !include template.yaml
```

Тоді код із файлу:

```text
01-sun-day-progress-template-sensor.yaml
```

потрібно додати у ваш `template.yaml`.

---

## Пояснення template sensor

### name

```yaml
name: Sun Day Progress
```

Це назва сенсора, як вона буде відображатися в Home Assistant.

---

### unique_id

```yaml
unique_id: sun_day_progress
```

Це унікальний ID сенсора.

Він потрібний, щоб Home Assistant міг коректно ідентифікувати цю сутність.

---

### unit_of_measurement

```yaml
unit_of_measurement: "%"
```

Тут ми вказуємо одиницю вимірювання.

У нашому випадку це відсотки.

---

### state

```yaml
state: >
```

У цьому блоці знаходиться основна логіка сенсора.

Саме тут ми рахуємо прогрес світлового дня.

---

### now_ts

```yaml
{% set now_ts = as_timestamp(now()) %}
```

Беремо поточний час і переводимо його у формат timestamp.

---

### next_rising і next_setting

```yaml
{% set next_rising = as_timestamp(state_attr('sun.sun', 'next_rising')) %}
{% set next_setting = as_timestamp(state_attr('sun.sun', 'next_setting')) %}
```

Беремо наступний схід і наступний захід сонця зі стандартної сутності:

```yaml
sun.sun
```

---

### Перевірка above_horizon

```yaml
{% if is_state('sun.sun', 'above_horizon') %}
```

Тут ми перевіряємо, чи сонце зараз над горизонтом.

Якщо сонце над горизонтом — рахуємо прогрес дня.

Якщо вже ніч — сенсор показує `0`.

---

### sunrise = next_rising - 86400

```yaml
{% set sunrise = next_rising - 86400 %}
```

`86400` — це кількість секунд у добі.

Коли сонце вже зійшло, Home Assistant у `next_rising` показує вже наступний схід, тобто завтрашній.

А нам потрібен сьогоднішній схід сонця.

Тому ми віднімаємо одну добу.

---

### sunset

```yaml
{% set sunset = next_setting %}
```

Тут ми беремо найближчий захід сонця.

---

### progress

```yaml
{% set progress = ((now_ts - sunrise) / (sunset - sunrise)) * 100 %}
```

Це основна формула.

Вона рахує, яка частина світлового дня вже пройшла.

Логіка така:

```text
поточний час - схід сонця
ділимо на весь проміжок між сходом і заходом
множимо на 100
```

---

### min / max / round

```yaml
{{ [0, [progress, 100] | min] | max | round(1) }}
```

Цей рядок обмежує значення в межах від `0` до `100` і округлює результат до одного знака після коми.

Тобто сенсор не покаже випадково `-3%` або `104%`.

---

### else 0

```yaml
{% else %}
  0
{% endif %}
```

Якщо зараз ніч і сонце не над горизонтом, сенсор показує `0`.

---

## 02-sun-day-progress-button-card.yaml

Це другий файл.

Він відповідає вже не за розрахунок, а за виведення красивої шкали на Dashboard.

Картка використовує готовий сенсор:

```yaml
sensor.sun_day_progress
```

і малює:

- фонову лінію шкали;
- підсвічену пройдену частину дня;
- іконку сонця;
- поточний відсоток прогресу дня.

---

## Що потрібно для роботи картки

Для другого файлу потрібна кастомна картка:

```text
custom:button-card
```

Якщо вона ще не встановлена, її потрібно встановити через HACS.

---

## Куди вставляти другий файл

Код із файлу:

```text
02-sun-day-progress-button-card.yaml
```

вставляємо у потрібне місце вашого Dashboard.

У моєму випадку це елемент 3D Dashboard, де використовується `picture-elements` або подібна структура з позиціонуванням.

---

## Пояснення button-card

### entity

```yaml
entity: sensor.sun_day_progress
```

Підключаємо нашу сутність, яку створили першим файлом.

Саме з неї картка бере відсоток прогресу дня.

---

### show_name / show_state / show_icon

```yaml
show_name: false
show_state: false
show_icon: false
```

Вимикаємо стандартну назву, стан та іконку.

Ми не використовуємо стандартний вигляд картки, бо малюємо свою шкалу вручну.

---

### style

```yaml
style:
  left: 50%
  top: 84%
  width: 88%
  transform: translate(-50%, -50%)
  z-index: 15
```

Цей блок відповідає за розташування шкали на дашборді.

Тут можна змінювати позицію, ширину та шар відображення.

---

### custom_fields

```yaml
custom_fields:
  track: ""
  passed: ""
  sun: |
    ...
  value: |
    ...
```

Тут ми створюємо власні елементи всередині картки.

У нас є чотири частини:

```text
track  — фонова лінія
passed — пройдена частина дня
sun    — іконка сонця
value  — відсоток прогресу
```

---

### track

`track` — це базова лінія шкали.

Вона служить фоном, по якому рухається сонце.

---

### passed

`passed` — це активна частина шкали.

Її ширина залежить від значення сенсора `sensor.sun_day_progress`.

Чим більший відсоток дня, тим більша підсвічена частина.

---

### sun

`sun` — це іконка сонця.

Вона рухається по шкалі залежно від поточного відсотка.

У коді позиція рахується через JavaScript.

---

### value

`value` — це текстове значення справа.

Воно показує поточний прогрес дня у відсотках.

Наприклад:

```text
67%
```

---

## Зображення

У папці також є зображення для пояснення та прикладу результату.

### dashboard_full (1).png

Повний вигляд дашборду.

### sun-day-progress.png

Приклад готової шкали прогресу дня.

### sun-day-progress-template-explained.png

Пояснення коду template sensor.

### sun-day-progress-card-explained.png

Пояснення коду `custom:button-card`.

---

## Порядок встановлення

1. Додайте код з файлу `01-sun-day-progress-template-sensor.yaml` у template Home Assistant.
2. Перевірте конфігурацію Home Assistant.
3. Перезапустіть Home Assistant або перезавантажте template entities.
4. Перевірте, що з’явилась сутність `sensor.sun_day_progress`.
5. Додайте код з файлу `02-sun-day-progress-button-card.yaml` у ваш Dashboard.
6. За потреби змініть позицію шкали через параметри `left`, `top` та `width`.

---

## Важливо

Позицію шкали можна змінювати під свій дашборд.

Основні параметри:

```yaml
left: 50%
top: 84%
width: 88%
```

Якщо шкала стоїть не там, де потрібно, змінюйте саме ці значення.

---

## Для відео

Не переписуйте YAML з екрана вручну.

Повний код для заняття №12 знаходиться в цьому GitHub-репозиторії.

Перший файл створює сенсор:

```yaml
sensor.sun_day_progress
```

Другий файл додає красиву шкалу руху сонця на Home Assistant Dashboard.

---

## Назва віджета

```text
Sun Day Progress Widget
```

Українською:

```text
Шкала прогресу світлового дня
```

---

## Автор

Проєкт для курсу по Home Assistant 3D Dashboard.

Автор: Anton Babenko
