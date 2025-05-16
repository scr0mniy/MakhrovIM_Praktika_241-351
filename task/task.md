# 🧩 SimpleTemplateEngine

**SimpleTemplateEngine** — это минималистичный шаблонизатор на Python, поддерживающий вставку переменных и условные блоки (`if`/`endif`). Реализация подходит для базового понимания принципов шаблонизации и может быть расширена для более сложных задач.

---

## 📁 Структура

- `SimpleTemplateEngine` — основной класс шаблонизатора
- `template_text` — строка-шаблон с переменными и условными блоками
- `data` — словарь контекста с данными
- `output.txt` — файл, в который сохраняется результат шаблонизации

---

## 🚀 Пример использования

```bash
$ python template_engine.py
Введите ваше имя: Иван
Введите ваш результат (0–100): 86
```

📤 Вывод в консоль:

```
==== РЕЗУЛЬТАТ ====

Привет, Иван!

Ваш результат: 86 (Отлично!)

Поздравляем! Вы сдали тест.

Ваши теги:

- python
- template
- if-support
```

---

## 🔧 Поддерживаемые возможности

### ✅ Переменные

Шаблон поддерживает вставку значений из словаря контекста:

```text
Привет, {{ name }}!
Ваш результат: {{ score }} ({{ grade }})
```

👉 Все `{{ ключ }}` заменяются значением из `context['ключ']`.

### ✅ Условные блоки

Шаблон поддерживает простую конструкцию `if/endif`:

```text
{{ if passed }}
Поздравляем! Вы сдали тест.
{{ endif }}
```

👉 Блок отображается, только если значение `passed` в контексте — `True`.

---

## 📦 Код

<details>
<summary>Открыть основной код</summary>

```python
import re

class SimpleTemplateEngine:
    def __init__(self, template: str):
        self.template = template

    def render(self, context: dict) -> str:
        lines = self.template.splitlines()
        output = []
        skip_block = False

        for line in lines:
            if_match = re.match(r"\s*{{\s*if\s+(\w+)\s*}}", line)
            endif_match = re.match(r"\s*{{\s*endif\s*}}", line)

            if if_match:
                key = if_match.group(1)
                skip_block = not context.get(key, False)
                continue

            elif endif_match:
                skip_block = False
                continue

            if skip_block:
                continue

            line = re.sub(r"{{\s*(\w+)\s*}}", lambda m: str(context.get(m.group(1), "")), line)
            output.append(line)

        return "\n".join(output)
```

</details>

---

## 📥 Ввод данных

Пользователь вручную вводит имя и результат теста:

```python
name = input("Введите ваше имя: ")
score = int(input("Введите ваш результат (0–100): "))
```

Также определяется уровень оценки:

```python
if score >= 85:
    grade = "Отлично!"
elif score >= 70:
    grade = "Хорошо"
elif score >= 50:
    grade = "Удовлетворительно"
else:
    grade = "Неудовлетворительно"
```

---

## 💾 Сохранение результата

Итог шаблонизации сохраняется в файл `output.txt`:

```python
with open("output.txt", "w", encoding="utf-8") as f:
    f.write(result)
```

---

## 📚 Полезные ресурсы

- [Jinja2 Docs](https://jinja.palletsprojects.com) — продвинутый шаблонизатор Python
- [Python](http://server.aesc.msu.ru/materials/PYTHON/pythonworldru.pdf) — гайд по Python


