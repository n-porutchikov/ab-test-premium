# A/B-тест: изменение стоимости премиум-подписки в приложении

## Инструменты:

<div>
  <img src="https://img.shields.io/badge/python-white?logo=python&style=for-the-badge" title="Python" alt="Python" height="40"/>&nbsp;
  <img src="https://img.shields.io/badge/pandas-white?logo=pandas&logoColor=blue&style=for-the-badge" title="Pandas" alt="Pandas" height="40"/>&nbsp;
  <img src="https://img.shields.io/badge/numpy-white?logo=numpy&logoColor=black&style=for-the-badge" title="Numpy" alt="Numpy" height="40"/>&nbsp;
  <img src="https://img.shields.io/badge/scipy-white?logo=scipy&logoColor=black&style=for-the-badge" title="SciPy" alt="SciPy" height="40"/>&nbsp;
  <img src="https://img.shields.io/badge/matplotlib-white?logo=matplotlib&logoColor=black&style=for-the-badge" title="Matplotlib" alt="Matplotlib" height="40"/>&nbsp;
  <img src="https://img.shields.io/badge/seaborn-white?logo=seaborn&logoColor=black&style=for-the-badge" title="Seaborn" alt="Seaborn" height="40"/>&nbsp;
  <img src="https://img.shields.io/badge/pingouin-white?logo=pingouin&logoColor=black&style=for-the-badge" title="Pingouin" alt="Pingouin" height="40"/>&nbsp;
  <img src="https://img.shields.io/badge/statsmodels-white?logo=statsmodels&logoColor=black&style=for-the-badge" title="Statsmodels" alt="Statsmodels" height="40"/>&nbsp;
</div>

## Зачем это нужно

В приложении была изменена стоимость премиум-подписки для новых пользователей при покупке через две новые платёжные системы. Стоимость пробного периода осталась прежней.

В этом проекте я проверил:

- Сопоставимы ли группы теста и контроля
- Как изменилась конверсия в платную подписку
- Как изменилась средняя выручка на пользователя (ARPU) и на платящего (ARPPU)
- Есть ли влияние на разные платёжные системы

Результаты помогают принять решение: внедрять новую ценовую политику или откатить изменения.

---

## Что сделано

- Загружены и обработаны данные **12 882 пользователей** и **1 608 транзакций** (3 группы: тестовая + 2 контрольных)
- Проведена **очистка данных**: удалены дубликаты, транзакции с датой платежа раньше даты регистрации, обработаны пропуски
- Проверена **сопоставимость групп** по возрасту, полу, стране, вовлечённости и просмотрам
- Рассчитаны ключевые метрики: **CR**, **ARPU**, **ARPPU**
- Использованы **непараметрические тесты** (Kruskal-Wallis, Mann-Whitney, хи-квадрат) и **бутстреп** для оценки разницы средних
- Построены визуализации распределений выручки

---

## Ключевые выводы:

###  Группы сопоставимы
Проверка показала, что группы не различаются по возрасту (p = 0.574), вовлечённости (p = 0.877), просмотрам (p = 0.456), полу (p = 0.508) и стране (p = 0.570). Эксперимент можно анализировать.

###  Конверсия (CR) снизилась
| Группа       | Конверсия в платную подписку |
|--------------|------------------------------|
| Контроль 1   | 2.06%                        |
| Контроль 2   | 2.17%                        |
| **Тест**     | **1.38%**                    |

Хи-квадрат: χ² = 7.64, p = 0.022.  

###  ARPU / ARPPU — различия не значимы
Бутстреп разницы средних показал, что доверительные интервалы для ARPU и ARPPU **включают ноль** — статистически значимого роста выручки нет.

| Метрика | test − control_1 | test − control_2 |
|---------|------------------|------------------|
| ARPU    | −47.7 [−175.6; 89.4] | 100.1 [−10.9; 226.8] |
| ARPPU   | 3706.3 [−723.1; 9353.4] | 4148.2 [−267.2; 9756.7] |

###  Платёжные системы
Только для `payment_id = 147` обнаружены значимые различия (p ≈ 2.8e-09), но после удаления выброса результат стал **нестабильным** — значимо только в одной из двух пар сравнения.

---

## Итоговый вывод

> **Эксперимент не привёл к статистически значимому росту выручки (ни общей, ни на платящего). Конверсия снизилась, что является негативным сигналом.**  
