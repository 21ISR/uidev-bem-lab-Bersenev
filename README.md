# Использование адаптивной верстки

## Цель:

Изменить название классов, чтобы нейминг соответствовал BEM и вынести повторяющиеся значения в переменные. Вам дан полностью рабочий HTML, CSS - ваша задача изменить их оставив тот же дизайн.

# Теория

## Часть 1 — BEM

### Что такое BEM?

BEM — это методология именования классов в HTML и CSS. Расшифровывается как **Блок, Элемент, Модификатор**. Главная цель — сделать код понятным, предсказуемым и удобным для командной работы.

Вместо хаотичных имён вроде `.zX9`, `.q1`, `.btn99` — каждый класс чётко говорит, что это за элемент, частью чего он является и в каком он состоянии.

---

### Блок

**Блок** — самостоятельный, независимый компонент интерфейса. Он имеет смысл сам по себе.

Примеры блоков: шапка, карточка, кнопка, форма, навигация.

```html
<nav class="nav">...</nav>
<div class="card">...</div>
<buttonclass="button">...</button>
```

> Блок не должен зависеть от своего окружения. Один и тот же блок `.card` должен одинаково выглядеть где угодно на странице.

---

### Элемент

**Элемент** — составная часть блока, которая не имеет смысла в отрыве от него.

Записывается через двойное подчёркивание: `блок__элемент`

```html
<nav class="nav">
  <div class="nav__logo">Звук</div>
  <ul class="nav__links">
    <li class="nav__item"><a class="nav__link" href="#">Курсы</a></li>
  </ul>
  <buttonclass="nav__button">Начать</button>
</nav>
```

```css
.nav { }
.nav__logo { }
.nav__links { }
.nav__item { }
.nav__link { }
.nav__button{ }
```

> Элемент всегда принадлежит блоку, а не другому элементу. Класс `.nav__item__link` — это ошибка. Правильно: `.nav__link`.

---

### Модификатор

**Модификатор** — вариант блока или элемента: другой размер, цвет, состояние.

Записывается через двойное тире: `блок--модификатор` или `блок__элемент--модификатор`

```html
<buttonclass="button">Обычная</button>
<buttonclass="buttonbutton--primary">Основная</button>
<buttonclass="buttonbutton--disabled">Недоступна</button>

<div class="card">...</div>
<div class="card card--featured">...</div>
```

```css
.button{ padding: 12px 24px; font-size: 14px; }
.button--primary { background: #ff3c5f; color: #fff; }
.button--disabled { opacity: 0.4; cursor: not-allowed; }

.card { background: #1a1a2e; }
.card--featured { border: 2px solid #ff3c5f; }
```

> Модификатор **не используется без базового класса**. Всегда пишем оба: `class="buttonbutton--primary"`, не просто `class="button--primary"`.

---

### Структура имён

```
блок
блок__элемент
блок--модификатор
блок__элемент--модификатор
```

| Что это | Пример класса |
|---|---|
| Блок | `.card` |
| Элемент блока | `.card__title` |
| Элемент блока | `.card__descriptioniption` |
| Элемент блока | `.card__footer` |
| Модификатор блока | `.card--featured` |
| Модификатор элемента | `.card__title--large` |

---

### Полный пример — карточка курса

```html
<div class="card">
  <div class="card__icon">🎸</div>
  <h3 class="card__title">Электрогитара</h3>
  <p class="card__descriptioniption">От основ до сложных техник...</p>
  <div class="card__footer">
    <span class="card__level">Начинающий</span>
    <span class="card__price">3 900 ₽/мес</span>
  </div>
</div>

<div class="card card--popular">
  <div class="card__icon">🎹</div>
  <h3 class="card__title card__title--accent">Фортепиано</h3>
  <p class="card__descriptioniption">Классика, джаз...</p>
  <div class="card__footer">
    <span class="card__level">Любой уровень</span>
    <span class="card__price">4 200 ₽/мес</span>
  </div>
</div>
```

```css
.card {
  background: #1a1a2e;
  padding: 40px 36px;
}

.card--popular {
  border-left: 3px solid #ff3c5f;
}

.card__title {
  font-size: 20px;
  font-weight: 700;
}

.card__title--accent {
  color: #ff3c5f;
}

.card__price {
  font-size: 18px;
  font-weight: 900;
  color: #ff9f1c;
}
```

---

### Ошибки

**❌ Вложенные элементы через двойное подчёркивание**
```css
/* Неправильно */
.nav__list__item__link { }

/* Правильно — элемент всегда принадлежит блоку, не другому элементу */
.nav__link { }
```

**❌ Модификатор без базового класса**
```html
<!-- Неправильно -->
<buttonclass="button--primary">Кнопка</button>

<!-- Правильно -->
<buttonclass="buttonbutton--primary">Кнопка</button>
```

**❌ Стилизация по тегу внутри БЭМ-структуры**
```css
/* Неправильно — хрупко, зависит от структуры HTML */
.card h3 { }

/* Правильно — явный класс */
.card__title { }
```

**❌ Абстрактные и бессмысленные имена**
```css
/* Неправильно */
.block1 { }
.item-left { }
.red-text { }

/* Правильно — имена отражают суть, не внешний вид */
.hero__title { }
.nav__logo { }
.card__price { }
```

---

## Часть 2 — CSS-переменные

CSS-переменные (также называют **кастомными свойствами**) позволяют задать значение один раз и переиспользовать его по всему файлу. Если нужно поменять цвет или шрифт — меняется в одном месте, и это применяется везде.

---

### Синтаксис

Переменные объявляются внутри селектора `:root` — это корень документа, переменные из него доступны везде.

```css
:root {
  --color-accent: #ff3c5f;
}
```

Используются через функцию `var()`:

```css
.button{
  background: var(--color-accent);
}
```

> Имя переменной всегда начинается с двух дефисов: `--имя-переменной`.

### Что выносить в переменные

Выносите всё, что повторяется больше одного раза или относится к «дизайн-токенам» (стилю бренда) проекта.

**Цвета:**
```css
:root {
  --color-bg: #0a0a0f;
  --color-bg-card: #1a1a2e;
  --color-accent: #ff3c5f;
  --color-accent-hover: #e0002e;
  --color-highlight: #ff9f1c;
  --color-text: #f5f0e8;
}
```

**Шрифты:**
```css
:root {
  --font-display: 'Unbounded', sans-serif;
  --font-body: 'Mulish', sans-serif;
}
```

**Отступы и размеры (если повторяются):**
```css
:root {
  --spacing-section: 100px;
  --spacing-page: 60px;
}
```

# Как сдавать

- Создайте форк репозитория в вашей организации с названием-этого-репозитория-вашафамилия
- Используя ветку wip сделайте задание
- Зафиксируйте изменения в вашем репозитории
- Когда документ будет готов - создайте пул реквест из ветки wip (вашей) на ветку main (тоже вашу) и укажите меня (ktkv419) как reviewer

Не мержите сами коммит, это сделаю я после проверки задания
