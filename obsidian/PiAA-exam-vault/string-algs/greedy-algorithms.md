# info
1) [algorithm](#algorithm)
	- [MST-build](##MST-build)
	- [activity-selection](##activity-selection)
	- [huffman-code](##huffman-code)
	- [greedy-alg-optimality](##greedy-alg-optimality)
2) [usage](#usage)
3) [complexity](#complexity)
4) [content](#content)

# algorithm
Жадные алгоритмы.

Вид алгоритма, что на каждом шаге выбирает локально оптимальное решение в надежде получить глобально оптимальное решение.

Жадные алгоритмы обычно эффективны с точки зрения времени выполнения и использования памяти, но требуют доказательства оптимальности для каждой конкретной задачи.

Особенностью жадных алгоритмов - использование сортировок.

Задачу о рюкзаке 0-1 через жадные алгоритмы не решить! (может получится так, что останется незанятое свободное место в рюкзаке и суммарная стоимость будет хуже, чем при другом наборе товаров, хотя по удельной стоимости (стоимость / вес) брался самый выгодный товар).

## MST-build
Построение минимального остовного дерева методом Краскала.

**Формулировка:**
Требуется построить минимальное остовное дерево, т.е. дерево минимальной стоимости, что связывает все вершины взвешенного графа.

**Шаги алгоритма:**
- Сортировка всех ребер по возрастанию стоимости.
- Из списка ребер берется минимальное.
- Если обработанных вершин больше 3, то проверяется наличие циклов: если текущее минимальное ребро образует цикл с теми, что уже используются, то пропускаем его.
- Иначе: смежные вершины для минимального ребра записываются в обработанные.
- Повторяем алгоритм, пока в списке обработанных вершин не окажутся все вершины для данного графа.

**Доказательство применимости ЖА:**
**Требуется доказать:** ребра с меньшим весом гарантированно входят в МОД.

Допустим, что существует два остовных дерева, отличающиеся в одном ребре. Тогда, очевидно, остовное дерево с более дорогим ребром будет весить больше --> МОД будет то, что содержит более дешевое ребро. 

## activity-selection
Задача об оптимальном расписании.

**Формулировка:**
Даны `N` задач в виде временных пар `(start, finish)`. Определен временной промежуток `t`. Задача состоит в максимизации числа задач, что можно выполнить за временной промежуток `t`.

**Шаги алгоритма:**
- Отсортировать все пары `(start, finish)` задач по возрастанию `finish` (если сортировка будет идти по возрастанию `start`, то вполне вероятна ситуация, при которой будет взята задача с такой временной парой `(-1, t_max)`).
- Взять из отсортированного списка максимальное число непересекающихся задач.

**Доказательство применимости ЖА:**
**Требуется доказать:** задачи с более ранним временем выполнения гарантированно входят в оптимальное решение.

Из отсортированного списка задач можно получить пару `(s1, f1)` и `(s2, f2)`,  идущих первыми (завершающихся раньше всех). Предположим, что с задачей `(s2, f2)` уже существует оптимальное решение. 

Тогда т.к. эти задачи заканчиваются раньше всех, то не существует такого `f3`, что будет между `f1` и `f2` или раньше `f1`. Следовательно, задач, что входят в оптимальное решение и находятся в пересечении `(s1, f1)` и `(s2, f2)` попросту не существует.

Тогда можно заменить задачу `(s2, f2)` на `(s1, f1)`, причем число задач в оптимальном решении не изменится (изменится набор задач).

Таким образом, задачи с более ранним временем выполнения гарантированно входят в оптимальное решение, ч.т.д. 

## huffman-code
Код Хаффмана.

**Формулировка:**
Зная частоты появления символов алфавита в тексте, закодировать алфавит минимальным префиксным кодом переменной длины.
**Префиксный код:** ни одно кодовое слово не является началом другого кодового слова.

**Шаги алгоритма:**
- Сортируем символы по частоте.
- Объединяем два символа с минимальными частотами в один символ (частоты складываем), запоминая слияния.
- Повторяем алгоритм, пока не останется один символ.
- Проходим по списку слияний в обратном порядке, строим по нему бинарное дерево.
- Двоично кодируем полученное дерево.

![[Pasted image 20250602210512.png]]

## greedy-alg-optimality
Принцип оптимальности жадных алгоритмов.

Говорят, что к оптимизационной задаче применим принцип жадного выбора, если последовательность локально оптимальных выборов даёт глобально оптимальное решение. 

**Схема доказательства оптимальности:**
- Доказывается, что жадный выбор на первом шаге не закрывает пути к оптимальному решению: для всякого решения есть другое, согласованное с жадным выбором и не хуже первого.
- Показывается, что подзадача, возникающая после жадного выбора на первом шаге, аналогична исходной.
- Рассуждение завершается по индукции.

**Элементы принципа оптимальности жадных алгоритмов:**
- **Жадный выбор:** на каждом шаге выбирается локально оптимальный вариант, который кажется наилучшим в данный момент.
- **Неизменность выбора:** принятое решение не пересматривается в дальнейшем.
- **Доказательство оптимальности:** чтобы убедиться, что жадный алгоритм находит глобальный оптимум, необходимо доказать, что жадный выбор на каждом шаге не исключает возможности получить оптимальное решение. 

**Основные методы доказательства:**
- **Свойство жадного выбора:** доказать, что локально оптимальный (жадный) выбор на каждом шаге совместим хотя бы с одним глобально оптимальным решением.
- **Свойство оптимальной подструктуры:** доказать, что после первого жадного выбора оставшаяся подзадача имеет ту же структуру, и оптимальное решение подзадачи ведет к оптимальному решению исходной задачи.
- **Доказательство методом обмена:** доказать, что что любое решение, отличное от полученного жадным алгоритмом, можно заменить на жадное решение без ухудшения результата.
- **Доказательство от противного:** предположить, что существует лучшее решение, чем жадное, и выйти к противоречию.

# usage
- Решение задач оптимизации.

# complexity
ОЦЕНКИ НА ПАРАХ НЕ ДАВАЛИ!!!

**MST-build:**
- **По времени:** `O(N*logN)` (сортировка)
- **По памяти:** `O(N^2)` (хранение графа в виде таблицы)

**activity-selection:**
- **По времени:** `O(N*logN)` (сортировка)
- **По памяти:** `O(N)` (хранения в списке)

**haffman-code:**
- **По времени:** `O(N*logN)` (сортировка)
- **По памяти:** `O(N)` (хранения `N` вершин)

# content
- [greedy-alg](https://ru.wikipedia.org/wiki/%D0%96%D0%B0%D0%B4%D0%BD%D1%8B%D0%B9_%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC)