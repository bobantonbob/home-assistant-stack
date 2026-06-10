# 3D Dashboard для Home Assistant — дашборд мого даху | Заняття 8

У цьому занятті починаємо робити новий 3D Dashboard для Home Assistant — дашборд даху з сонячними панелями.

Головна ідея: беремо підготовлену 3D-картинку даху, завантажуємо її в Home Assistant і поверх неї додаємо живі елементи через `picture-elements` та `custom:button-card`.

---

## Структура папки

Рекомендована назва папки для GitHub:

```text
lesson-08-roof-solar-dashboard
```

Всередині папки:

```text
lesson-08-roof-solar-dashboard/
├── README.md
└── lesson-08-roof-solar-dashboard.yaml
```

Файл картинки даху потрібно покласти вже у Home Assistant:

```text
/config/www/roof.png
```

У дашборді Home Assistant ця картинка буде відкриватися так:

```text
/local/roof.png
```

---

## Що потрібно перед стартом

1. Встановлений Home Assistant.
2. Встановлений HACS.
3. Встановлена картка `custom:button-card`.
4. Готова картинка даху `roof.png`.
5. Сенсор сонячної генерації за сьогодні.

У прикладі використовується сенсор:

```yaml
sensor.today_sun
```

Якщо у вас інший сенсор, просто замініть його в YAML-коді.

---

# Головний дашборд

Основа всього дашборду — це `picture-elements`.

```yaml
type: picture-elements
image: /local/roof.png
elements:
```

Саме ця картка дозволяє взяти картинку як фон і накладати поверх неї кнопки, сенсори, іконки, анімації та інші елементи.

---

# Заняття 8 — секція “Сонце сьогодні”

У цьому занятті ми додаємо перший живий елемент на дашборд — блок **“СОНЦЕ СЬОГОДНІ”**.

Він показує, скільки кіловат-годин сонячна станція виробила за поточний день.

Блок має:

- іконку сонця;
- назву `СОНЦЕ СЬОГОДНІ`;
- значення в `kWh`;
- адаптивний розмір через `clamp()`;
- світіння, якщо генерація більше ніж `0.1 kWh`.

---

## Повний YAML-код заняття 8

```yaml
type: picture-elements
image: /local/roof.png
elements:
  - type: custom:button-card
    entity: sensor.today_sun
    name: СОНЦЕ СЬОГОДНІ
    icon: mdi:white-balance-sunny
    show_name: true
    show_state: true
    show_icon: true
    tap_action:
      action: more-info
    state_display: |
      [[[
        const v = Number(entity.state);
        if (isNaN(v)) return '0 kWh';
        return v.toFixed(1) + ' kWh';
      ]]]
    style:
      left: 50%
      top: 7%
      transform: translate(-50%, -50%)
      width: clamp(140px, 22vw, 230px)
    extra_styles: |
      @keyframes sunTotalPulse {
        0% {
          box-shadow:
            0 0 0 0 rgba(255, 210, 70, 0.65),
            0 0 12px rgba(255, 210, 70, 0.35);
        }
        50% {
          box-shadow:
            0 0 0 9px rgba(255, 210, 70, 0.08),
            0 0 26px rgba(255, 210, 70, 0.80);
        }
        100% {
          box-shadow:
            0 0 0 0 rgba(255, 210, 70, 0),
            0 0 12px rgba(255, 210, 70, 0.35);
        }
      }
    styles:
      grid:
        - grid-template-areas: '"i n" "i s"'
        - grid-template-columns: min-content 1fr
        - grid-template-rows: min-content min-content
        - column-gap: clamp(6px, 1vw, 10px)
      card:
        - background: rgba(58, 42, 5, 0.76)
        - border-radius: clamp(12px, 1.2vw, 18px)
        - padding: clamp(6px, 1vw, 10px)
        - border: 1px solid rgba(255, 210, 70, 0.95)
        - backdrop-filter: blur(8px)
        - animation: |
            [[[
              return Number(entity.state) > 0.1
                ? 'sunTotalPulse 2s ease-in-out infinite'
                : 'none';
            ]]]
      icon:
        - color: '#ffd54f'
        - width: clamp(20px, 2.6vw, 32px)
      name:
        - color: white
        - font-size: clamp(9px, 1.1vw, 13px)
        - font-weight: 700
        - letter-spacing: 0.4px
        - justify-self: start
      state:
        - color: '#ffd54f'
        - font-size: clamp(14px, 1.8vw, 22px)
        - font-weight: 800
        - justify-self: start
```

---

## Як змінити позицію блоку

За позицію відповідають два параметри:

```yaml
left: 50%
top: 7%
```

`left` — рухає блок ліворуч або праворуч.

`top` — рухає блок вверх або вниз.

Наприклад, якщо потрібно трохи нижче:

```yaml
top: 10%
```

Якщо потрібно лівіше:

```yaml
left: 45%
```

---

## Як змінити розмір блоку

За ширину відповідає цей рядок:

```yaml
width: clamp(140px, 22vw, 230px)
```

Це означає:

- мінімум: `140px`;
- адаптивно: `22vw`;
- максимум: `230px`.

Якщо блок замалий, можна зробити так:

```yaml
width: clamp(160px, 24vw, 260px)
```

---

## Як працює анімація

Анімація вмикається тільки тоді, коли значення сенсора більше `0.1`.

```yaml
return Number(entity.state) > 0.1
  ? 'sunTotalPulse 2s ease-in-out infinite'
  : 'none';
```

Тобто вдень, коли є генерація, блок світиться.

Вночі або коли значення `0`, блок не пульсує.

---

## Що буде далі

У наступних заняттях можна додавати:

- поточну генерацію PV;
- окремі PV1 / PV2 стрінги;
- мікроінвертори;
- батарею;
- навантаження будинку;
- імпорт/експорт з мережі;
- камери;
- прогноз Solcast;
- анімовані потоки енергії.

---

## Важливо

Цей код — базовий старт для заняття 8. Його потрібно адаптувати під свої сутності Home Assistant.

Найчастіше потрібно змінити тільки:

```yaml
entity: sensor.today_sun
```

на свій сенсор денного виробітку сонячної станції.
