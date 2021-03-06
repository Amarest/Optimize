# Матричные игры
Тушавин В. А.  
30 ноября 2015 г.  

### Постановка задачи

Удобным способом задания игры двух участников с нулевой суммой является платежная матрица. Отсюда, кстати, происходит еще одно их название — матричные игры. Каждый элемент платежной матрицы a~ij~ содержит числовое значение выигрыша игрока I (проигрыша игрока II), если первый применяет стратегию i, а второй —- стратегию j.

Термины выигрыш и проигрыш следует понимать в широком смысле, т. к. они могут принимать отрицательные значения и с житейской точки зрения означать противоположное. Нетривиальность задачи прежде всего заключается в том, что каждый из игроков делает свой выбор, не зная о выборе другого, что существенно осложняет процесс оптимизации выбираемой стратегии.

Пусть в игре участвуют первый и второй игрок, каждый из них может
записать цифры 1,2,3. Если разница между цифрами положительна,
то выигрывает первый игрок, если отрицательна, то второй.
Число выигранных очков равно разности между цифрами.


```r
(mtx<-matrix(c(0,1,2,-1,0,1,-2,1,0),ncol=3))
```

```
##      [,1] [,2] [,3]
## [1,]    0   -1   -2
## [2,]    1    0    1
## [3,]    2    1    0
```

#### Стратегия первого игрока


Наилучшая стратегия первого игрока.
Если игрок выбирает стратегию 1, то в худшем случае он получает выигрыш


```r
min(mtx[1,])
```

```
## [1] -2
```

Если стратегию 2


```r
min(mtx[2,])
```

```
## [1] 0
```

Если стратегию 3


```r
min(mtx[3,])
```

```
## [1] 0
```

Максимизируем свой минимальный выигрыш


```r
max(min(mtx[1,]),min(mtx[2,]),min(mtx[3,]))
```

```
## [1] 0
```

Это величина $\alpha$ -- гарантированный выигрыш игрока A или нижняя цена игры. Сама стратегия называется максиминной.


#### Стратегия второго игрока

Второй игрок в худшем случае при стратегии 1 получит проигрыш


```r
max(mtx[,1])
```

```
## [1] 2
```
При второй стратегии


```r
max(mtx[,2])
```

```
## [1] 1
```

При третьей стратегии


```r
max(mtx[,3])
```

```
## [1] 1
```
Минимизируем свой максимальный проигрыш/


```r
min(max(mtx[,1]),max(mtx[,2]),max(mtx[,3]))
```

```
## [1] 1
```

Это величина $\beta$ -- гарантированный проигрыш игрока B или верхняя цена игры. Сама стратегия называется минимаксной.

#### Седловая точка


Для матричных игр справедливо неравенство $\alpha \le \beta$

Если $\alpha=\beta=\gamma$, то такая игра называется игрой с седловой точкой. Если платежная матрица не имеет седловой точки, то поиск решения приводит к сложной стратегии, состояшей в случайном применении двух и более стратегий с определенными частотами. такая сложная стратегия называется смешанной.

#### Упрощение матрицы


```
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    8    6    4    4    3
## [2,]    5    3    2    2    1
## [3,]    4    7    7    3    5
## [4,]    5    3    2    2    1
## [5,]    1    4    4    2    3
```
Решение

```r
(a<-apply(mtx,1,min))
```

```
## [1] 3 1 3 1 1
```

```r
max(a)
```

```
## [1] 3
```


```r
(b<-apply(mtx,2,max))
```

```
## [1] 8 7 7 4 5
```

```r
min(b)
```

```
## [1] 4
```
$3 \le \nu \le 4$



```r
mtx
```

```
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    8    6    4    4    3
## [2,]    5    3    2    2    1
## [3,]    4    7    7    3    5
## [4,]    5    3    2    2    1
## [5,]    1    4    4    2    3
```
Для первого игрока стратегии 2 и 4 одинаковы, Все эелементы стратегии 2 меньше стратегии 1, значит тоже можно исключить. Все элементы 5 стратегии меньше 3. Исключаем пятую стратегию.



```r
mtx[c(1,3),]
```

```
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    8    6    4    4    3
## [2,]    4    7    7    3    5
```

Для второго игрока сравниваем 1 и 4, исключаем 1. Сравниваем 2 и 5, исключаем 2.


```r
(mtx1<-mtx[c(1,3),c(3,4,5)])
```

```
##      [,1] [,2] [,3]
## [1,]    4    4    3
## [2,]    7    3    5
```

```r
max(apply(mtx1,1,min))
```

```
## [1] 3
```

```r
min(apply(mtx1,2,max))
```

```
## [1] 4
```
### Решение матричных игр сведением к линейному программированию

Рассмотрим игру двух лиц с нулевой суммой заданную платежами

$$A = {\left\| {{a_{ij}}} \right\|_{m \times n}}$$

Применение первым игроком оптимальной стратегии должно обеспечить ему при любых действиях второго игрока выигрыш не менее цены игры

$$\sum\limits_{i = 1}^m {{a_{ij}}{x_{i\;\text{опт}}} \ge \nu ,\;j = \overline {1,n} } $$

Рассмотрим задачу отыскания оптимальной стратегии игрока при огрнаничениях 

$$\left\{ {\begin{array}
{{a_{11}}{x_1} + {a_{21}}{x_2} +  \ldots  + {a_{m1}}{x_m} \ge \nu }\\
{{a_{12}}{x_1} + {a_{22}}{x_2} +  \ldots  + {a_{m2}}{x_m} \ge \nu }\\
 \cdots \\
{{a_{1n}}{x_1} + {a_{2n}}{x_2} +  \ldots  + {a_{mn}}{x_m} \ge \nu }
\end{array}} \right.$$

Величина $\nu$ неизвестна, однако можно считать что цена игры $\nu>0$. Последнее условие выполняется всегда, если все элементы платежной матрицы неотрицательны, а это можно достигнуть прибавив ко всем элементам некую константу. Преобразуем ограничения поделив неравентва на $\nu$.

$$\left\{ {\begin{array}
{{a_{11}}{t_1} + {a_{21}}{t_2} +  \ldots  + {a_{m1}}{t_m} \ge 1}\\
{{a_{12}}{t_1} + {a_{22}}{t_2} +  \ldots  + {a_{m2}}{t_m} \ge 1}\\
 \cdots \\
{{a_{1n}}{t_1} + {a_{2n}}{t_2} +  \ldots  + {a_{mn}}{t_m} \ge 1}
\end{array}} \right.$$

где

$${t_i} = \frac{{{x_i}}}{\nu } \ge 0$$

По условию $x_1+x_2+\ldots +x_m=1$ (сумма вероятностей). Разделим обе части этого неравенства на $\nu$.

$$t_1+t_2+\ldots +t_m=\frac{1}{\nu}$$

Оптимальная стратегия игрока A должна максимизировать величину $\nu$, следовательно, функция:

$$L(\bar t) = \sum\limits_1^m {{t_i} \to \min } $$

Для второго игрока проигрыш не должен превышать цену игры. В результате имеем симметричную пару двойственных задач.

#### Пример решения в R

Дана матрица игры


```
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    3    7    1    1    5
## [2,]    4    9    3    6    2
## [3,]    2    3    1    4    7
```

```r
(a<-max(apply(mtx,1,min)))
```

```
## [1] 2
```

```r
(b<-min(apply(mtx,2,max)))
```

```
## [1] 3
```
Игра не имеет седловой точки. Оптимальное решение следует искать в области смешанных стратегий.


```r
library(lpSolve)
(result<-lp("min",c(1,1,1), t(mtx), rep(">=",5),c(1,1,1,1,1)))
```

```
## Success: the objective function is 0.3684211
```

```r
result$objval
```

```
## [1] 0.3684211
```

```r
result$solution
```

```
## [1] 0.00000000 0.31578947 0.05263158
```

```r
(a<-result$solution/result$objval)
```

```
## [1] 0.0000000 0.8571429 0.1428571
```

```r
(result<-lp("max",c(1,1,1,1,1), mtx, rep("<=",3),c(1,1,1)))
```

```
## Success: the objective function is 0.3684211
```

```r
result$objval
```

```
## [1] 0.3684211
```

```r
result$solution
```

```
## [1] 0.0000000 0.0000000 0.2631579 0.0000000 0.1052632
```

```r
(b<-result$solution/result$objval)
```

```
## [1] 0.0000000 0.0000000 0.7142857 0.0000000 0.2857143
```
Таким образом цена игры равна 2.7142857, оптимальная стратегия A равна (0, 0.8571429, 0.1428571), оптимальная стратегия B равна  (0, 0, 0.7142857, 0, 0.2857143).

#### Построение имитационной модели

```r
# Функция, возвращающая индекс стратегии
get.k<-function(vec){
  cusum=0
  tst=runif(1)
  for(i in 1:length(vec)) {
   if(vec[i]==0) next
   cusum<-cusum+vec[i]
   if(tst>cusum) next
  return(i)
  }
}
set.seed(2015)
test<-c()
for(i in 1:1000) test=c(test,mtx[get.k(a),get.k(b)])

table(test)
```

```
## test
##   1   2   3   7 
##  86 221 656  37
```

```r
summary(test)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   2.000   3.000   2.755   3.000   7.000
```
Пусть A выбирает стратегию случайно


```r
test<-c()
for(i in 1:1000) test=c(test,mtx[sample(1:3, 1),get.k(b)])

table(test)
```

```
## test
##   1   2   3   5   7 
## 478  79 249  97  97
```

```r
summary(test)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   1.000   2.000   2.547   3.000   7.000
```
Как видим, результат хуже. Аналогично рассмотрим вариант для B.


```r
test<-c()
for(i in 1:1000) test=c(test,mtx[get.k(a),sample(1:5, 1)])

table(test)
```

```
## test
##   1   2   3   4   6   7   9 
##  22 183 211 203 164  22 195
```

```r
summary(test)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   3.000   4.000   4.726   6.000   9.000
```

В этом случае, B проигрывает больше.

### Решение в LibreOffice

Пример решения приведен в файле [matrix.ods](Samples/LibreOffice/matrix.ods).

Решение сводится к решениям прямой и обратной задач линейного программирования с помощью встроенной системы Решатель.

![Решение в LibreOffice](fig/mtx_1.png)

![Решение в LibreOffice](fig/mtx_2.png)

![Решение в LibreOffice](fig/mtx_3.png)

Результаты получаются аналогичными вышеприведенному решению.

#### Информация о параметрах R


```r
sessionInfo()
```

```
## R version 3.2.0 (2015-04-16)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 7 x64 (build 7601) Service Pack 1
## 
## locale:
## [1] LC_COLLATE=Russian_Russia.1251  LC_CTYPE=Russian_Russia.1251   
## [3] LC_MONETARY=Russian_Russia.1251 LC_NUMERIC=C                   
## [5] LC_TIME=Russian_Russia.1251    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] lpSolve_5.6.11
## 
## loaded via a namespace (and not attached):
##  [1] magrittr_1.5    formatR_1.2.1   tools_3.2.0     htmltools_0.2.6
##  [5] yaml_2.1.13     stringi_0.4-1   rmarkdown_0.6.1 knitr_1.10.5   
##  [9] stringr_1.0.0   digest_0.6.8    evaluate_0.8
```
