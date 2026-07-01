# Research Map — Техническое задание

**Статус:** v0.1 — черновик  
**Дата:** 2026-06-30  
**Авторы:** Sabrina

---

## 1. Что это и зачем

**Research Map** — открытая интерактивная карта математических структур, найденных в LLM эмпирически.

Исследователи постоянно обнаруживают в моделях геометрические и алгебраические структуры (торы, линейные подпространства, иерархии, схемы), но эти находки разбросаны по сотням статей. Карта агрегирует их в единой навигируемой базе с двумя осями: **тип структуры** и **модель**.

**Цели продукта:**
- Дать исследователям быстрый обзор поля: "что уже известно о Llama-3?"
- Стать точкой входа в школу для академической аудитории
- Создать живой open-source ресурс, который растёт через сабмиты
- Путь студенческих проектов: Track B → сабмит → карта → LessWrong → arXiv

**Что это не:**
- Не энциклопедия (нет длинных статей)
- Не база данных (нет SQL, нет сложных запросов)
- Не форум (нет комментариев, нет обсуждений)

---

## 2. Аудитория

| Кто | Зачем приходит |
|-----|----------------|
| ML-исследователь | Проверить, что уже найдено в интересующей модели / структуре |
| Студент курса по XAI/Студент внешний | Выбрать тему финального проекта, сабмитнуть результат |
| Профессор / ревьюер | Быстрый обзор поля перед написанием бумаги |
| Случайный посетитель | Понять mech interp через конкретные находки |

---

## 3. Таксономия математических структур

Дерево — основа навигации "по структуре". Дополняется со временем через PR.

```
Многообразия (Manifolds)
  Тор (Torus)
    Примеры: дни недели, месяцы, циклическое время
  Гиперболическое многообразие
    Примеры: семантические иерархии (гипонимия)
  Сфера / гиперсфера
  Аффинное подпространство

Линейные структуры (Linear structures)
  Направление / линейный зонд (Linear direction)
    Примеры: gender direction, sentiment direction
  Линейное подпространство (Subspace)
    Примеры: части речи, named entities
  Low-rank аппроксимация весовых матриц

Иерархии (Hierarchies)
  Семантическое дерево (WordNet-like)
  Концептуальная решётка

Схемы (Circuits)
  Induction heads
  IOI circuit (indirect object identification)
  Docstring circuit
  Greater-than circuit

Суперпозиция и словарное разложение (Superposition / Dictionary)
  Полисемантичный нейрон (Polysemantic neuron)
  SAE feature (Sparse Autoencoder feature)
  Dictionary atom

Композиционные структуры (Compositional)
  Аналогии (King - Man + Woman = Queen)
  Binding structure
```

---

## 4. Модель записи

Каждая запись — YAML-файл в `data/entries/`. Одна запись = одна находка из N papers. За основу добавляются фундаментальные статьи. Статьи, расширяющие существование структуры на семейство новое моделей -- спрятаны как  [1, 2, 3, ...] в семействах; 

```yaml
id: circular-representations-of-time-gurnee-2024
title: "Circular Representations of Time in Language Models"
authors:
  - "Gurnee, W."
  - "Tegmark, M."
year: 2024
arxiv: "2405.XXXXX"
code: "https://github.com/..."

structure:
  type: manifold/torus
  description: >
    Модели кодируют циклические временные концепты (дни, месяцы)
    как точки на торе в residual stream

models:
  - name: llama-2-7b
    layer_range: [8, 20]
    confirmed: true
  - name: qwen-2.5-7b
    confirmed: true
  - name: gpt-2-xl
    confirmed: false

methods:
  - probing
  - PCA
  - activation patching

keywords:
  - time representation
  - torus
  - cyclical structure

submitted_by: community
reviewed_by: sabrina
added: 2026-07-01
```

---

## 5. Страницы и функциональность

### 5.1 Главная (/)

- Короткое объяснение что такое карта (2-3 предложения)
- Два CTA: "Explore by structure" / "Explore by model"
- Счётчики: N записей, M моделей, K структур
- 3-5 последних добавленных записей
- Ссылка на Submit

### 5.2 По структуре (/structures)

Интерактивное дерево таксономии. Кликнуть на узел -> раскрыть дочерние -> на листе — список записей с бейджами моделей.

Каждый узел имеет два слоя:
- `/structures/manifolds/` — список находок (эмпирика)
- `/structures/manifolds/theory` — теоретическая страница

Теоретическая страница покрывает определения. Вспомогательные определения -- например, гомеоморфизм для какой-то структуры -- тоже должно быть дано, мб всплывающим окошком. 

### 5.3 По модели (/models)

Сетка карточек моделей. Кликнуть -> страница модели.

Страница модели (`/models/llama-2-7b`):
- Все записи по этой модели
- Группировка по типу структуры
- Фильтр по типу

### 5.4 Страница записи (/entries/:id)

- Название, авторы, год, arXiv + код
- Хлебная крошка в дереве структур
- Модели с деталями (слои, подтверждено / нет)
- Методы
- Описание находки

### 5.5 Submit (/submit)

MVP: инструкция по PR + ссылка на шаблон. Форма — в v2.

### 5.6 About (/about)

О проекте, команде, ссылка на школу по XAI, которую я сделаю.

---

## 6. Теоретический слой

Каждый узел таксономии имеет два слоя контента:

| Слой | Что | Кто пишет |
|------|-----|-----------|
| **Теория** | Что такое эта структура математически, почему она важна для LLM | Сабрина |
| **Эмпирика** | Конкретные находки из бумаг (YAML-записи) | Community + команда |

**Структура теоретической страницы:**

1. Математическое определение — строго, но доступно, с примерами из геометрии
2. Почему это важно для LLM — интуиция: что значит, что модель "использует" эту структуру
3. Как обнаружить — методы: probing, PCA, activation patching, визуализация
4. Ключевые свойства — для тора: цикличность, топология; для линейного зонда: разделимость
5. Ссылки на записи — автоматически из YAML (все entries с этим type)
6. Дополнительное чтение — 2-3 бумаги

**Пример: страница "Тор" (/structures/manifolds/torus/theory)**

```markdown
TBD -- надо делать
```

**Хранение:**

```
data/
  theory/
    manifolds.md
    manifolds-torus.md
    manifolds-hyperbolic.md
    linear-structures.md
    linear-structures-direction.md
    hierarchies.md
    circuits.md
    circuits-induction-heads.md
    superposition.md
    superposition-sae.md
    compositional.md
```

Frontmatter каждого файла:

```yaml
---
id: manifolds-torus
title: "Тор (Torus)"
parent: manifolds
level: 2
difficulty: intermediate
prerequisites:
  - linear-algebra-basics
  - manifolds
summary: "Замкнутая поверхность S1 x S1 для кодирования циклических структур"
---
```

**Теория входит в MVP** — без неё карта выглядит как просто список бумаг, а не образовательный ресурс. Минимум для v1: теория для 5 верхних категорий. Подкатегории — постепенно. 

---

## 7. Submit workflow

### Вариант A: GitHub PR (MVP)

1. Fork репо
2. Скопировать `data/entries/_template.yaml`, заполнить
3. Открыть PR: `[entry] <title> (Year)`
4. Ревью ~1 неделя
5. Merge -> автодеплой

### Критерии ревью

Принять:
- Опубликованная бумага (arXiv / конференция)
- Воспроизведённый эксперимент с публичным кодом
- Студенческий проект с явной методологией и кодом

Отклонить:
- Claim без эксперимента
- Дубль без нового вклада (новая модель или метод — ок)
- Некорректный YAML

---

## 8. Технический стек

| Компонент | Решение | Почему |
|-----------|---------|--------|
| Фреймворк | Astro | Статический сайт, быстро, YAML из коробки |
| Данные | YAML-файлы в `data/entries/` | Git-версионирование, PR как workflow |
| Граф/дерево | D3.js | Гибкая визуализация дерева таксономии |
| Стили | Tailwind CSS | Быстро, утилитарно |
| Деплой | Vercel | Автодеплой из main |
| Домен | TBD | Решить отдельно |

---

## 9. Структура репо

```
research-map/
  data/
    entries/
      _template.yaml
      circular-representations-of-time-gurnee-2024.yaml
      induction-heads-olsson-2022.yaml
    theory/
      manifolds.md
      manifolds-torus.md
      linear-structures.md
      circuits.md
      superposition.md
    taxonomy.yaml
    models.yaml
  src/
    pages/
      index.astro
      structures/
        index.astro
        [slug]/
          index.astro
          theory.astro
      models/
        index.astro
        [slug].astro
      entries/
        [id].astro
      submit.astro
      about.astro
    components/
      TaxonomyTree.jsx
      ModelGrid.astro
      EntryCard.astro
      TheoryPage.astro
    layouts/
      Base.astro
  public/
  CONTRIBUTING.md
  README.md
```

---

## 10. MVP (v1)

- [ ] Структура репо + Astro-проект
- [ ] `taxonomy.yaml` — первичное дерево (5-7 верхних категорий)
- [ ] `models.yaml` — ~10 популярных моделей
- [ ] 10-15 начальных записей (Elhage 2022, Olsson 2022, Gurnee 2024, Meng 2022, Cunningham 2023...)
- [ ] Теория для 5 верхних категорий (manifolds, linear, hierarchies, circuits, superposition)
- [ ] Страницы: главная, /structures, /models, /entries/:id, /structures/:slug/theory
- [ ] Интерактивное дерево (D3)
- [ ] CONTRIBUTING.md + шаблон записи
- [ ] Деплой Vercel + домен

---

## 11. Out of scope (v1)

- Веб-форма для сабмита -> v2
- Поиск по тексту -> v2
- Фильтрация по году / методу -> v2
- Граф связей между записями -> v3
- Автопарсинг arXiv -> v3
- Теория для подкатегорий -> постепенно

---

## 12. Открытые вопросы

- [ ] Финальное название и домен
- [ ] Дизайн: минималистичный академический или визуально богатый?
- [ ] Тёмный режим с v1?
- [ ] Лицензия данных: CC BY 4.0?
- [ ] Как обрабатывать prepublication?
- [ ] Кто ревьюит сабмиты — Сабрина, Лена, оба?