# van Emde Boas tree
## Вступление
**Дерево ван Эмде Боаса** также известное как дерево vEB или приоритетная очередь ван Эмде Боаса, структура данных, представляющая собой дерево поиска, позволяющее хранить целые неотрицательные числа в интервале [0;2k) и осуществлять над ними все соответствующие дереву поиска операции.
Проще говоря, данная структура позволяет хранить k-битные числа и производить над ними операции find, insert, remove, next, prev, min, max и некоторые другие операции, которые присущи всем деревьям поиска.
___
## Операции
### empty
Чтобы определить, пусто ли дерево, будем изначально инициализировать поле **min** числом, которое не лежит в интервале [0;2k). Назовем это число **none**. Например, это может быть −1, если мы храним в числа в знаковом целочисленном типе, или 2k, если в беззнаковом. Тогда проверка на пустоту дерева будет заключаться лишь в сравнении поля min с этим числом.
```
boolean empty(t: Tree):
  if t.min == none
    return true
  else 
    return false
```
### min и max
Так как мы храним в дереве минимальное и максимальное значения, то данные операции не требуют ничего, кроме вывода значения поля min или max в соответствии с запросом. Время выполнения данных операций соответственно O (1).
### find
Алгоритм поиска сам напрашивается из вышеописанной структуры:
- если дерево пусто, то число не содержится в нашей структуре.
- если число равно полю min или max, то число в дереве есть.
- иначе ищем число low(x) в поддереве children[high(x)].
```
boolean find(t: Tree, x: int):
  if emty(t)
    return false
  if t.min == x or t.max == x
    return true 
  return find(t.children[high(x)], low(x))
```
### next и prev
Алгоритм нахождения следующего элемента, как и два предыдущих, сводится к рассмотрению случая, когда дерево содержит не более одного элемента, либо к поиску в одном из его поддеревьев:
-	если дерево пусто, или максимум этого дерева не превосходит x, то следующего элемента в этом дереве не существует.
-	если x меньше поля min, то искомый элемент и есть min.
- если дерево содержит не более двух элементов, и x <max, то искомый элемент max.
-	если же в дереве более двух элементов, то:
-	если в дереве есть еще числа, большие xx, и чьи старшие биты равны high(x), то продолжим поиск в поддереве children[high(x)], где будем искать число, следующее после low(x).
-	иначе искомым элементом является либо минимум следующего непустого поддерева, если такое есть, либо максимум текущего дерева в противном случае.
### remove
Удаление из дерева также делится на несколько подзадач:
-	если min=max=x, значит в дереве один элемент, удалим его и отметим, что дерево пусто.
-	если x=min, то мы должны найти следующий минимальный элемент в этом дереве, присвоить min значение второго минимального элемента и удалить его из того места, где он хранится. Второй минимум — это либо max, либо children[aux.min].min (для случая x=max действуем аналогично).
-	если же x≠min и x≠max, то мы должны удалить low(x)l из поддерева children[high(x)].

Так как в поддеревьях хранятся не все биты исходных элементов, а только часть их, то для восстановления исходного числа, по имеющимся старшим и младшим битам, будем использовать функцию merge. Также нельзя забывать, что если мы удаляем последнее вхождение x, то мы должны удалить high(x) из вспомогательного дерева.
___
## Преимущества и недостатки
### Преимущества
Главным преимуществом данной структуры является ее быстродействие. Асимптотически время работы операций дерева ван Эмде Боаса лучше, чем, например, у АВЛ, красно-черных, 2-3, splay и декартовых деревьев уже при небольшом количестве элементов. Конечно, из-за довольно непростой реализации возникают немалые постоянные множители, которые снижают практическую эффективность данной структуры. Но все же, при большом количестве элементов, эффективность дерева ван Эмде Боаса проявляется и на практике, что позволяет нам использовать данную структуру не только как эффективное дерево поиска, но и в других задачах. Например:
-	сортировка последовательности из n чисел. Вставим элементы в дерево, найдем минимум и n−1n−1 раз вызовем функцию nextnext. Так как все операции занимают не более O(logk) времени, то итоговая асимптотика алгоритма O(n⋅logk), что даже лучше, чем цифровая сортировка, асимптотика которой O(n⋅k).
-	Алгоритм Дейкстры. Данный алгоритм с использованием двоичной кучи для поиска минимума работает за O(E⋅logV), где V — количество вершин в графе, а E — количество ребер между ними. Если же вместо кучи использовать дерево ван Эмде Боаса, то релаксация и поиск минимума будут занимать уже не logV, а logk, и итоговая асимптотика этого алгоритма снизится до O(E⋅logk).

### Недостатки
-	существенным недостатком данной структуры является то, что она позволяет хранить лишь целые неотрицательные числа, что существенно сужает область ее применения, по сравнению с другими деревьями поиска, которые не используют внутреннюю структуру элементов, хранящихся в них.
-	другим серьезным недостатком является количество занимаемой памяти. Дерево, хранящее k-битные числа, занимает Θ(2k) памяти, что несложно доказывается индукцией, учитывая, что S(2k) 
