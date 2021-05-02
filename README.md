##Кратко
Скрипт написан на Python 3.7 с использованием Jupyter Notebook 6.3.0. 
Программа считывает данные из файла `table.xls` и результат кладет в 
файл `out_table.xls`.<br>
*Stack: numpy, scipy, pandas*

##Входные данные
Файл формата `xls` имеет следующие столбцы:<br>
* `date` - дата в формате MySQL (YYYY-mm-dd)
* `installs` - количество установок приложения
* `costs` - суммарный расход игры на рекламные компании, $
* `revenue` - суммарный доход игры со всех игроков, $
* `payers share` - процент игроков, совершивших не менее 1 платежа
на указанную дату

##Принцип работы
1. Считывание данных из `xls` в `pandas.DataFrame`
2. Группировка методом `groupby` по столбцу `date` с периодом 1 неделя
3. Формируется список функций для всех столбцов, кроме `date`, применяемых при группировке:
    1. *numpy.mean* - среднее значение
    2. *numpy.std* - стандартное отклонение
    3. *scipy.stats.variation* - коэффициент вариации
    4. *np.medium* - медиана
    5. *np.percentile* - 25 и 75 процентили
4. После метода `groupby` появляются периоды с пустыми данными, которые отфильтровываются скриптом
5. Через дополнительную таблицу высчитывается коэффициент корреляции по группам, 
и затем этот столбец добавляется к первоначальной таблице
6. Запись получившейся таблицы в файл `out_table.xls`


##Выходные данные
Файл формат `xls`, имеющий 26 столбцов из-за работы функций, 
описанных в разделе **Принцип работы**. Столбцы `installs`,`costs`,
`revenue` и `payers share` имеют вложенные подстолбцы соответствующие 
5 функциям из **Принцип работы, пункт 3**. Помимо этих столбцов есть `date` 
и `r`, где последний есть коэффициент корреляции внутри группы.