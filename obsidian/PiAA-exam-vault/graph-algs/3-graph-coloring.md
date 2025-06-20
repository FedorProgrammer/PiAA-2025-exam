# info
1) [algorithm](#algorithm)
	- [3-graph-coloring](##3-graph-coloring)
		- [brute-force](###brute-force)
		- [partial-enumeration](###partial-enumeration)
		- [set-enumeration](###set-enumeration)
		- [probabilistic-algorithm](###probabilistic-search)
	- [SAT](##SAT)
2) [usage](#usage)
3) [complexity](#complexity)
4) [content](#content)

# algorithm

## 3-graph-coloring
Алгоритм правильной раскраски графа в `3` различных цвета. NP трудная задача: проверить решение можно за полиномиальное время (но не найти его).

### brute-force
Полный перебор.

Для каждой вершины рассматривать один из трех цветов.

### partial-enumeration
Частичный перебор.

Для инцидентных (соседние) вершин рассматривать различные цвета. Таким образом, каждая вершина будет принимать `2` различных состояния (цвета).

### set-enumeration
Перебор множеств размера `n/3`.

По итогу раскраски должно появится три области (множества), содержащие вершины с различными цветами (**такое множество** - антиклика (независимое множество) (множество вершин, что НЕ соединены между собой ребром)).

Минимальное по размеру **такое множество** может содержать не более чем n/3 вершин. Если известно хотя бы одно **такое множество**, то остальные вершины красятся линейно (просто красим смежные вершины в разные цвета: количество способов выбрать цвет для "неокрашенных" вершин задается однозначно).

Количество множеств, что нужно перебирать для нахождения **такого** наименьшего:
`C_n_1 + C_n_2 + C_n_3 + ... + C_n_(n/3) <= n * C_n_(n/3) ~= 1,9 ^ n`, где: 
- `C_n_k` - формула числа сочетаний.
- `~=` - примерно равно.

### probabilistic-search
Вероятностный алгоритм.

Сведем задачу к выполнению булевой формулы (к SAT задаче).

**Обозначения**:
- `<letter><i>` - вершина `<letter>` покрашена в `<i>`-ый цвет.
- Покрасить вершину хотя бы в один цвет (для каждой вершины): `(a1 or a2 or a3)`
- Покрасить вершину только в один цвет (для каждой вершины): `(not(a1) or not(a2)) and (not(a2) or not(a3)) and (not(a1) or not(a3))`
- Покрасить инцидентные (соседние) вершины в различные цвета (для каждого ребра): `(not(a1) or not(b1)) and (not(a2) or not(b2)) and (not(a3) or not(b3))`

Если свести эти обозначения в одно целое, то получим булевую формулу для 3-SAT:
`((a1 or a2 or a3) and (b1 or b2 or b3) and ...) and ((not(a1) or not(a2)) and (not(a2) or not(a3)) and (not(a1) or not(a3)) and (not(b1) or not(b2)) ...) and ((not(a1) or not(b1)) and (not(a2) or not(b2)) and (not(a3) or not(b3)) and ...)`

Если случайным образом будет вычеркнут один из цветов для каждой из вершины, то задача 3-SAT перейдет в задачу 2-SAT (цвет для каждой вершины вычеркивается с вероятностью 1/3). 

Тогда вероятность, что раскраска "выжила": `P{раскраска выжила} >= (2/3)^n` (вероятность выбора правильного цвета для вершины == `2/3`, для n вершин == `(2/3)^n`).

Если мы хотим найти вероятность ошибки, то: `(1-p)^(1/p) < e^(-1)` (если мы повторяем алгоритм (выбор) `1/p` раз) или `(1-p)^(c/p) < e^(-c)` (если мы повторили алгоритм `c/p` раз).

Тогда при повторе алгоритма `c * 1.5^n` раз получаем очень маленькую вероятность провала.

2-SAT решается за линейное время: для каждого дизъюнкта `(a or b)` получаем, что состояние `a = 0 and b = 0` запрещено, т.е. клоза `(a or b)` эквивалентна импликациям `not(a) --> b` и `not(b) --> a`.

Тогда можно построить двудольный граф: в одной доле положительные литералы, в другой - отрицательные. Затем каждую клозу `(a or b)` раскрываем в импликации `not(a) --> b` и `not(b) --> a`, т.е. проводим грани из вершин `not(a)` в `b` и `not(b)` в `a`.

![[Pasted image 20250531214633.png]]

Общая формула 2-SAT будет выполнима тогда и только тогда, когда НЕ найдется компонента связности, в которую одновременно попадут переменная и ее отрицание. 

Если такая компонента есть, то тогда из `x1` следует `not(x1)` (`x1 <--> not(x1)` импликация в обе стороны).

## SAT
Задача выполнимости. NP трудная задача.

Дано `n` переменных, которые принимают значения `0` и `1` (ложь, истина), и из них составлена какая-то формула. Нужно проверить: можно ли присвоить этим переменным такие значения, что общая формула станет истинной.

Например: `(x1 or x2) and (not(x3)) and (not(x2) or x1)` (`x3 = 0, x1 = 1`)

**Разновидности:**
- **3-SAT:** каждый дизъюнкт содержит не более чем три литерала (в каждой скобке не более трех переменных).
- **2-SAT:** аналогично, но с двумя литералами.

# usage
- Планирования процессов (логистика).
- Контурные карты.
- Компьютерное зрение.

# complexity
- **Полный перебор:**
	- **По времени:** `O(3^n)` (для каждой вершины `3` "состояния")
	- **По памяти:** `O(n^2)` (хранение графа в виде таблицы)
	
- **Частичный перебор:**
	- **По времени:** `O(2^n)` (для каждой  вершины `2` "состояния")
	- **По памяти:** `O(n^2)` (хранение графа в виде таблицы)

- **Перебор множеств размера n/3:**
	- **По времени:** `O(1.9^n)` (сумма чисел сочетаний - `1.9 ^ n`)
	- **По памяти:** `O(n^2)` (хранение графа в виде таблицы)

- **Вероятностный алгоритм:**
	- **По времени:** `O(1.5^n)`
	- **По памяти:** `O(n^2)` (хранение графа в виде таблицы)

# content
- [lecture](https://youtu.be/wmmCILxyU-M?si=bGwTsZDmIKIpRDWJ)
- [complexity](https://youtu.be/TXS47WMaAow?si=RZk0SSj1hRe6wPBX&t=1211)