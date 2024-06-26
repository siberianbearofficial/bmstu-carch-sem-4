# Числа с фиксированной и плавающей запятой: сравнение способов кодирования и области применения

## Числа с фиксированной запятой

### Способы кодирования

- Числа с фиксированной запятой представляют собой целые числа, масштабированные на определенное количество десятичных
  разрядов.
- Например, число 1234 может быть интерпретировано как 12.34, если мы предполагаем, что десятичная точка находится между
  12 и 34.
- Для представления используются знаковые и беззнаковые целые числа, где старший бит часто используется для обозначения
  знака (0 для положительных, 1 для отрицательных чисел).

### Области применения

- Подход фиксированной запятой часто используется в финансовых приложениях, где требуется высокая точность и
  предсказуемость, например, при расчетах валют.
- Подходит для встроенных систем и микроконтроллеров, где ресурсы ограничены, так как такие числа требуют меньше
  вычислительных ресурсов.

### Преимущества

- Простота и высокая скорость вычислений, так как операции над числами с фиксированной запятой можно выполнять так же,
  как над целыми числами.
- Точность значений остается постоянной, что важно для критичных к ошибкам вычислений.

### Недостатки

- Ограниченный диапазон представляемых чисел, что может привести к переполнению при работе с большими значениями.
- Меньшая гибкость в представлении чисел с большим диапазоном или сильно различающимися по величине.

## Числа с плавающей запятой

### Способы кодирования

Числа с плавающей запятой представляют собой числа в виде m * b^e, где m - мантисса, b - основание (обычно 2), e - порядок.
Чуть более подробно можно прочитать [тут](https://skillbox.ru/media/code/chisla-s-plavayushchey-tochkoy-chto-eto-takoe-i-kak-oni-rabotayut/).

### Стандарт IEEE 754

Число с плавающей запятой в формате IEEE 754 (одинарной точности) представляется следующим образом:
- **Знак** (1 бит): 0 для положительных чисел и 1 для отрицательных.
- **Порядок** (8 бит): Порядок сдвинут на 127 (сдвиговый порядок).
- **Мантисса** (23 бита): Дробная часть числа.

### Перевод десятичной дроби в двоичное представление формата IEEE 754

Рассмотрим пример: перевод числа `5.75` в двоичное представление формата IEEE 754.

1. **Десятичное число:** `5.75`

2. **Перевод целой части в двоичное представление:**
   - `5` в 10 сс = `101` в 2 сс

3. **Перевод дробной части в двоичное представление:**
   - `0.75` в 10 сс
   - `0.75 * 2 = 1.5` (целая часть `1`, дробная `0.5`)
   - `0.5 * 2 = 1.0` (целая часть `1`, дробная `0`)
   - Таким образом, `0.75` в 10 сс = `0.11` в 2 сс

4. **Объединение целой и дробной частей:**
   - `5.75` в 10 сс = `101.11` в 2 сс

5. **Нормализация числа:**
   - Представляем в виде `1.0111 * 2^2`
   - Мантисса: `0111` (неявная единица слева от точки по [ведущей битной конвенции](https://ru.wikipedia.org/wiki/IEEE_754-2008#:~:text=%D0%B2%D0%B5%D0%B4%D1%83%D1%89%D0%B5%D0%B9%20%D0%B1%D0%B8%D1%82%D0%BD%D0%BE%D0%B9%20%D0%BA%D0%BE%D0%BD%D0%B2%D0%B5%D0%BD%D1%86%D0%B8%D0%B5%D0%B9))
   - Порядок: `2`

6. **Порядок со сдвигом (Bias):**
   - Для одинарной точности (32 бита) Bias = `127`
   - Порядок = `2 + 127 = 129`
   - `129` в 10 сс = `10000001` в 2 сс

7. **Составление итогового двоичного представления:**
   - Знак: `0` (число положительное)
   - Порядок: `10000001`
   - Мантисса: `01110000000000000000000`

Итак, двоичное представление числа `5.75` в формате IEEE 754 (одинарной точности): `0 10000001 01110000000000000000000`

#### Перевод десятичной дроби из двоичного формата IEEE 754

Теперь переведем это двоичное представление обратно в десятичное число.

1. **Извлечение знака, порядка и мантиссы:**
   - Знак: `0` (положительное число)
   - Порядок: `10000001`
   - Мантисса: `01110000000000000000000`

2. **Вычисление порядка:**
   - `10000001` в 2 сс = `129` в 10 сс
   - Порядок = `129 - 127 = 2`

3. **Восстановление мантиссы:**
   - Добавляем неявную единицу: `1.0111`

4. **Вычисление значения:**
   - `1.0111` в 2 сс = `1 + 0.25 + 0.125 + 0.0625` = `1 + 0.4375 = 1.4375`
   - Умножаем на `2^2`: `1.4375 * 4 = 5.75`

Таким образом, мы получили исходное число `5.75`.

### Области применения

- Широко используются в научных вычислениях, графике, инженерных приложениях и других областях, где важен широкий
  диапазон значений и возможность представления очень маленьких или очень больших чисел.
- Также применяются в компьютерной графике и обработке сигналов, где требуется высокая динамика и точность вычислений.

### Преимущества

- Большой диапазон представляемых значений, что позволяет работать с очень малыми и очень большими числами.
- Возможность представления чисел с высокой точностью, что важно для научных и инженерных расчетов.

### Недостатки

- Сложность операций, так как вычисления с числами с плавающей запятой требуют больше времени и ресурсов.
- Возможны погрешности при округлении, что может приводить к ошибкам накопления при многократных вычислениях.

## Сравнение

| Характеристика             | Фиксированная запятая                  | Плавающая запятая                     |
|----------------------------|----------------------------------------|---------------------------------------|
| **Точность**               | Высокая для фиксированной части        | Высокая, но зависит от мантиссы       |
| **Диапазон**               | Ограничен                              | Очень широкий                         |
| **Скорость вычислений**    | Высокая                                | Ниже, из-за сложности операций        |
| **Использование ресурсов** | Меньше памяти и процессорного времени  | Больше памяти и процессорного времени |
| **Области применения**     | Финансовые расчеты, встроенные системы | Научные вычисления, графика           |
| **Простота реализации**    | Простая                                | Сложная                               |
