# krestikinoliki
Математическое обоснование алгоритма проверки валидности поля выигрышей


Примеры упорядоченных матриц для крестиков-ноликов.
*Пометка: поскольку речь идёт в программно-алгоритмическом ключе, счёт начинается с 0 для удобства реализации.*

---

### Для 3х3:

```
0 1 2
3 4 5
6 7 8

Столбцы:   9, 12, 15
Ряды:      3, 12, 21
Диагонали: 12, 12
```
---

### Для 4х4:

```
0  1  2  3
4  5  6  7
8  9  10 11
12 13 14 15

Столбцы:   24, 28, 32, 36
Ряды:      6, 22, 38, 54
Диагонали: 30, 30
```
---

Доступ к ячейке матрицы осуществляется по индексам ![A[y][x]](https://github.com/anon1352/krestikinoliki/blob/master/f0.gif) (да, сначала вертикаль).

### Рассмотрим формальную модель.
Нетрудно убедиться, что в линейно упорядоченных матрицах сумма по главным диагоналям всегда будет одинаковой.
Кроме того, мы видим, что:

	~ суммы столбцов кратны размерности матрицы с элементами вида a_yx:
	для всех y, где y<=Dim(M), верно sum(M[y][i]) % 4 = 0
	и сверх того, sum(M[y][i]) = Dim(M) * (sum(Dim(M)-1) + y)
	Проверим: для первого (нулевого) столбца: (4 * ((1+2+3) + 0)) = 24, для второго: (4 * ((1+2+3) + 1)) = 28 и так далее. ~

	~ cуммы рядов, sum(M[i][x]) = sum(Dim(M) - 1) + (Dim(M)^2)*i
	Проверим: для первого (нулевого) ряда 6 + 16*0 = 6, для второго 6 + 16*1 = 22 и так далее. ~

	~ для диагоналей, sum(M[i][i]) = sum(M[i][Dim(M)-1-i]) = sum[i=0,i<Dim(M)](i*(Dim(M)+1))
	Проверим для матрицы 4х4: действительно, (0*5 + 1*5 + 2*5 + 3*5) = 30. ~

---

### Теперь к алгоритму реализации.
Условимся, что в крестиках-нуликах выигрышные комбинации - это полная диагональ, полный ряд или полный столбец.
Условимся также, что проверять выигрыш будем сначала для того, кто первый ходил (начинал игру). Обычно это крестик.

```
 0 │ 1 │ 2
───┼───┼───
 3 │ 4 │ 5
───┼───┼───
 6 │ 7 │ 8

Выигрыши:
	Столбцы:   	9, 12, 15
	Ряды: 		  3, 12, 21
	Диагонали: 	12, 12
```
Так, для поля 3х3 (обычного) мы проверяем выигрыш крестиков следующим образом:
- обозначим теоретические позиции крестиков через Х[i][j], где i,j это оси y,x соответственно матрице
- вычислим все выигрышные суммы для нашей КН-матрицы (количество комбинаций равно 2+Dim(M)*2, это и будет размером для матрицы выигрышей; можно не делать матрицу, а проверять на месте, исходя из данных по столбцу и ряду, но тогда вычисления потребуются на каждом шаге, однако расход памяти снизится, особенно для крупных полей типа 10000*10000)
- в цикле по i={0;Dim(M)-1} пробежимся по заполненной крестиками и ноликами матрице и для каждого ряда просуммируем крестики
- если ряд пройден, но сумма не соответствует формуле выигрыша для ряда, идём к следующему ряду и снова собираем сумму (в ту же переменную, лол)
- так для всех рядов
- аналогично для столбцов
- аналогично для диагоналей
- если найдена сумма, делаем break и выкидываем congratulations
- затем делаем это всё для ноликов
- если так и не нашли выигрышных комбинаций, пишем "Draw..." (или как там "ничья" по-английски)
- если первыми ходили нолики, соответственно, начинаем проверку с ноликов
- ???
- PROPIT

---

Оценочная сложность алгоритма: O(N^2)

Можно улучшить до O(n * log(n)), но это уже ваше домашнее задание.

Спасибо за прочтение этого математического бреда!
