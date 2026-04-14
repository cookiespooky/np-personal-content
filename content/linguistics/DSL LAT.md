---
type: article
slug: dsl-linguistic-agency-transformation
title: "DSL: Linguistic Agency Transformation (LAT)"
description: LAT domain-specific-language
draft: false
---
# DSL: Linguistic Agency Transformation (LAT)

## 1. Общая модель

```
input(statement)

  → parse()
  → detect()
  → classify()
  → transform()
  → output(actionable_statement)
```

## 2. Базовые сущности

2.1 Statement

```
Statement {
  text: string
}
```

2.2 Agent

```
Agent {
  type: "explicit" | "implicit" | "absent"
  value: string | null
}
```

2.3 Action

```
Action {
  type: "present" | "absent" | "abstract"
  verb: string | null
}
```

2.4 Modality

```
Modality {
  type: "external" | "internal" | "undefined"
  markers: string[]
}
```

Примеры маркеров:

- external: “надо”, “должен”, “приходится”
- internal: “хочу”, “решаю”, “делаю”

2.5 Observability

```
Observability {
  level: 0..2
}
```

- 0 — не наблюдаемо («что-то менять»)
- 1 — частично («начать делать»)
- 2 — конкретное действие («пишу 1 сообщение клиенту»)

## 3. Парсинг

```
parse(statement) → {
  tokens,
  verbs,
  subjects,
  markers
}
```

## 4. Детекция

```
detect(statement) → {
  agent: Agent,
  action: Action,
  modality: Modality,
  observability: Observability
}
```

## 5. Классификация

`classify(state) → mode`

Возможные режимы:

```
mode = 
  "action"        // есть агент + действие
  "intention"     // есть желание, нет действия
  "reflection"    // описание, без действия
  "avoidance"     // уход от агентности
```

## 6. Правила трансформации

Rule 1: Добавление агента

```
IF agent.type == "absent"
THEN insert("я")
```

Rule 2: Замена внешней модальности

```
IF modality.type == "external"
THEN replace_with_internal()
```

пример:

```
"надо сделать" → "я делаю"
```

Rule 3: Конкретизация действия

```
IF action.type != "present"
THEN define_observable_action()
```

Rule 4: Повышение наблюдаемости

```
WHILE observability.level < 2
  → уточнить действие
```

## 7. Трансформация

`transform(statement) → actionable_statement`

## 8. Пример

Input

```
"Надо что-то менять"
```

Detect

```
agent: absent
action: absent
modality: external
observability: 0
mode: avoidance
```

Transform

```
→ "я выбираю изменить X"
→ "я делаю конкретное действие Y"
```

Output

```
"Я пишу одному клиенту сегодня"
```


## 9. Минимальный интерфейс (как API)

```
analyze(statement) → {
  agent,
  action,
  modality,
  observability,
  mode
}

transform(statement) → {
  variants: [neutral, direct, radical, aggressive, toxic],
  recommended: actionable_statement
}
```

## 10. DSL-синтаксис (человеко-читаемый)

Почти как язык:

`S: "не получается начать"`

→ detect:

```
  agent: absent
  action: abstract
  modality: undefined
```

→ transform:

  ```
  +agent("я")
  +action("начинаю с X")
  +constraint("сегодня")
  ```

→ result:

  ```
  "я начинаю с X сегодня"
  ```

## 11. Главное правило языка

Нет агента → нет действия  
Нет действия → нет изменения

---

Приглашаю поучаствовать в [[linguistic-laboratory-landing|совместном исследовании]]

Агентность и режим высказывания как базовый слой анализа речи - читать [[Агентность и режим высказывания white paper|White Paper]]

[[Области прикладного применения LAT|Области прикладного применения LAT]]

[[Описание метода простыми словами|Описание метода]] простыми словами
