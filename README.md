# Курсовая работа. Блок 1

*Pandas и Matplotlib*


**Описание работы:**

Перед вами стоит бизнес-задача – на основании имеющихся данных подготовить аналитический отчет, который в дальнейшем поможет продюсерам образовательных программ эффективно выстраивать стратегию по модернизированию и улучшению курсов. В начале отчета предлагается оформить емкий описательный блок по каждому курсу на основании рассчитанных показателей. Далее предлагается посчитать потенциальную нагрузку на преподавателей, чтобы оценить необходимость расширения штата сотрудников. Затем идет блок из двух пунктов по анализу качества контента курсов, где необходимо выявить проблемные модули, которые, возможно, требуют доработки. Также стоит задача выявить потенциальную сезонность. Наконец, предложено задание для самостоятельной разработки метрики успеваемости студентов для нахождения тех, кто значительно хуже справляются с прохождением курса. Каждый из пунктов анализа предполагается сопроводить аналитическим выводом на основании рассчитанных метрик.

<br><br>

_________
Обозначения:<br><br>
&nbsp;&nbsp;&nbsp;&nbsp;**(p)** – задание может быть выполнено после прохождения модулей по Pandas <br>
&nbsp;&nbsp;&nbsp;&nbsp;**(m)** – задание может быть выполнено после прохождения модуля по Matplotlib <br>
&nbsp;&nbsp;&nbsp;&nbsp;⭐ – необязательное задание повышенной сложности
_________

[Codebook](#Codebook) <br>
[1. Описание и начальная работа с данными](#1.1-Описание-и-начальная-работа-с-данными)<br>
[2. Расчет потенциальной нагрузки на преподавателей](#2.-Расчет-потенциальной-нагрузки-на-преподавателей)<br>
[3. Выявление проблемных модулей](#3.-Выявление-проблемных-модулей)<br>
[4. Расчет конверсии](#4.-Расчет-конверсии) <br>
[5. Метрика успеваемости ](#5.-Метрика-успеваемости)

## Codebook

`courses.csv` содержит следующие значения: <br><br>
&nbsp;&nbsp;&nbsp;&nbsp; `id` – идентификатор курса <br>
&nbsp;&nbsp;&nbsp;&nbsp; `title` – название курса <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `field` – сфера, к которой относится курс <br> <br><br>
`students.csv` содержит следующие значения: <br><br>
&nbsp;&nbsp;&nbsp;&nbsp; `id` – идентификатор студента <br>
&nbsp;&nbsp;&nbsp;&nbsp; `city` – город студента <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `birthday` – день рождения студента <br> <br><br>
`course_contents.csv` содержит следующие значения: <br><br>
&nbsp;&nbsp;&nbsp;&nbsp; `course_id` – идентификатор курса <br>
&nbsp;&nbsp;&nbsp;&nbsp; `module_number` – номер модуля <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `module_title` – название модуля <br> 
&nbsp;&nbsp;&nbsp;&nbsp; `lesson_number` – номер урока <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `lesson_title` – название урока <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `lesson_token` – токен урока <br> 
&nbsp;&nbsp;&nbsp;&nbsp; `is_video` – наличие видео *(true/false)* <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `is_homework` – наличие домашней работы *(true/false)* <br>
<br><br>
`progresses.csv` содержит следующие значения: <br><br>
&nbsp;&nbsp;&nbsp;&nbsp; `id` – идентификатор прогресса <br>
&nbsp;&nbsp;&nbsp;&nbsp; `student_id` – идентификатор студента <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `course_id` – идентификатор курса <br> <br><br>
`progress_phases.csv` содержит следующие значения: <br><br>
&nbsp;&nbsp;&nbsp;&nbsp; `progress_id` – идентификатор прогресса <br>
&nbsp;&nbsp;&nbsp;&nbsp; `module_number` – номер модуля <br>
&nbsp;&nbsp;&nbsp;&nbsp; `lesson_number` – номер урока <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `status` – статус прохождения урока <br>
&nbsp;&nbsp;&nbsp;&nbsp;  `start_date` – дата начала <br> 
&nbsp;&nbsp;&nbsp;&nbsp; `finish_date` – дата окончания <br>
<br><br>

## 1. Описание и начальная работа с данными

Вам необходимо подготовить данные и описать их. Данные реальные и содержат пропущенные значения, а также лишние относительно друг друга данные. <br>
Опишите данные: <br>
1. **(p)** Посчитайте
      * общее количество курсов в датасете, 
      * количество модулей на каждом курсе, 
      * количество уроков в каждом модуле на каждом курсе,  
      * медианное количество уроков в модуле на каждом курсе, 
      * количество учеников на каждом курсе
      * минимальный, максимальный, средний, медианный возраст студентов
      * минимальный, максимальный, средний, медианный возраст студентов на каждом курсе
2. **(m)** Постройте bar-chart, отражающий количество студентов на каждом курсе. Ticks нужно развернуть так, чтобы они были читаемы
3. **(m)** Постройте горизонтальный (столбцы должны располагаться горизонтально) bar-chart, отражающий количество студентов на каждом курсе. График должен иметь заголовок. Значения должны быть отсортированы. Цвет столбцов должен содержать информацию о сфере, к которой относится курс (то есть нужна легенда). Прозрачность должна стоять на отметке 0.1. На график должна быть нанесена линия медианы. У медианы должен быть свой цвет. Рамки у графика быть не должно ⭐
4. **(m)** На основании рассчитанных значений опишите данные (описание должно быть полным и покрывать все полученные выше метрики)

_____________________________________________________________________



```python
# Импортируем всё что нужно
import numpy as np
import pandas as pd 
import matplotlib.pyplot as plt
from datetime import * 
import seaborn as sns
from pandas.tseries.offsets import MonthEnd
from matplotlib.dates import MonthLocator, DateFormatter
import matplotlib.colors as colors


```


```python
# Считываем данные 
courses = pd.read_csv('courses.csv')
course_contents = pd.read_csv('course_contents.csv')
progresses = pd.read_csv('progresses.csv')
progress_phases = pd.read_csv('progress_phases.csv')
students = pd.read_csv('students.csv')
```


```python
#Соеденяем все датасеты, кроме students
merged_df = progresses.merge(courses, left_on='course_id', right_on='id')

# Добавление детальной информации о курсах
merged_df = merged_df.merge(course_contents)

# Добавление информации о прогрессе прохождения курсов
merged_df = merged_df.merge(progress_phases,
                        left_on=['id_x', 'module_number', 'lesson_number'], 
                        right_on=['progress_id', 'module_number', 'lesson_number'])

# Проверка корректности сформированного общего датасета на примере одно из студентов
merged_df[merged_df.student_id == '768c2987a744c51ce64a5993a2a94eaf'].head()
  
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_x</th>
      <th>student_id</th>
      <th>course_id</th>
      <th>Unnamed: 0</th>
      <th>id_y</th>
      <th>title</th>
      <th>field</th>
      <th>module_number</th>
      <th>module_title</th>
      <th>lesson_number</th>
      <th>lesson_title</th>
      <th>lesson_token</th>
      <th>is_video</th>
      <th>is_homework</th>
      <th>progress_id</th>
      <th>status</th>
      <th>start_date</th>
      <th>finish_date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>164816</th>
      <td>6407c290505e790e92207a1dbc41a2cd</td>
      <td>768c2987a744c51ce64a5993a2a94eaf</td>
      <td>dad6f6ffc086caa89e2f40c28a9c7490</td>
      <td>8</td>
      <td>dad6f6ffc086caa89e2f40c28a9c7490</td>
      <td>UX-дизайн</td>
      <td>Design</td>
      <td>1</td>
      <td>Профессия дизайнера в эпоху цифровых перемен</td>
      <td>1</td>
      <td>Приветствие</td>
      <td>86d0d49c-5590-4c0b-8fca-927191bb3fd5</td>
      <td>True</td>
      <td>False</td>
      <td>6407c290505e790e92207a1dbc41a2cd</td>
      <td>start</td>
      <td>2018-06-20 14:25:13.010259+00</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>187756</th>
      <td>c90ebe1431eac5cbb11692100b7a0f8d</td>
      <td>768c2987a744c51ce64a5993a2a94eaf</td>
      <td>e9bb9205eeed307ee7cbaa08bfd166c3</td>
      <td>5</td>
      <td>e9bb9205eeed307ee7cbaa08bfd166c3</td>
      <td>JavaScript с нуля</td>
      <td>Development</td>
      <td>1</td>
      <td>Знакомство с языком</td>
      <td>1</td>
      <td>Интро</td>
      <td>0d4678b0-abfe-4132-9193-97f9b0f08d3a</td>
      <td>True</td>
      <td>False</td>
      <td>c90ebe1431eac5cbb11692100b7a0f8d</td>
      <td>done</td>
      <td>2018-06-19 10:40:05.063485+00</td>
      <td>2018-06-19 14:56:16.346353+00</td>
    </tr>
    <tr>
      <th>187757</th>
      <td>c90ebe1431eac5cbb11692100b7a0f8d</td>
      <td>768c2987a744c51ce64a5993a2a94eaf</td>
      <td>e9bb9205eeed307ee7cbaa08bfd166c3</td>
      <td>5</td>
      <td>e9bb9205eeed307ee7cbaa08bfd166c3</td>
      <td>JavaScript с нуля</td>
      <td>Development</td>
      <td>1</td>
      <td>Знакомство с языком</td>
      <td>2</td>
      <td>Что умеет JavaScript и почему он так популярен?</td>
      <td>6af5b93a-593b-48a0-bb03-42fa2571ede5</td>
      <td>True</td>
      <td>False</td>
      <td>c90ebe1431eac5cbb11692100b7a0f8d</td>
      <td>done</td>
      <td>2018-06-19 14:56:16.570129+00</td>
      <td>2018-06-19 15:08:13.930725+00</td>
    </tr>
    <tr>
      <th>187758</th>
      <td>c90ebe1431eac5cbb11692100b7a0f8d</td>
      <td>768c2987a744c51ce64a5993a2a94eaf</td>
      <td>e9bb9205eeed307ee7cbaa08bfd166c3</td>
      <td>5</td>
      <td>e9bb9205eeed307ee7cbaa08bfd166c3</td>
      <td>JavaScript с нуля</td>
      <td>Development</td>
      <td>1</td>
      <td>Знакомство с языком</td>
      <td>3</td>
      <td>Инструменты разработчика</td>
      <td>460c54ea-d899-44d3-8940-00302ff5f2e5</td>
      <td>True</td>
      <td>False</td>
      <td>c90ebe1431eac5cbb11692100b7a0f8d</td>
      <td>done</td>
      <td>2018-06-19 15:08:14.103923+00</td>
      <td>2018-06-19 15:39:53.661163+00</td>
    </tr>
    <tr>
      <th>187759</th>
      <td>c90ebe1431eac5cbb11692100b7a0f8d</td>
      <td>768c2987a744c51ce64a5993a2a94eaf</td>
      <td>e9bb9205eeed307ee7cbaa08bfd166c3</td>
      <td>5</td>
      <td>e9bb9205eeed307ee7cbaa08bfd166c3</td>
      <td>JavaScript с нуля</td>
      <td>Development</td>
      <td>1</td>
      <td>Знакомство с языком</td>
      <td>4</td>
      <td>Hello, world!</td>
      <td>73315f69-8587-4f46-ab6b-fe57c8f1aa52</td>
      <td>True</td>
      <td>False</td>
      <td>c90ebe1431eac5cbb11692100b7a0f8d</td>
      <td>done</td>
      <td>2018-06-19 15:39:53.923777+00</td>
      <td>2018-06-19 18:10:52.1737+00</td>
    </tr>
  </tbody>
</table>
</div>





```python
#Преобразуем столбцы с датами в формат для вычислений
merged_df[['start_date','finish_date']] = merged_df[[
    'start_date','finish_date']].astype('datetime64[ms]')

```

###  Общее количество курсов в датасете 


```python
len(merged_df['title'].unique())
```




    15



### Количество модулей на каждом курсе


```python
all_mod_df = merged_df.groupby(
    'title')[['module_title']].agg(
    lambda x: len(set(x)))

all_mod_df

```




<div>
     
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>module_title</th>
    </tr>
    <tr>
      <th>title</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Excel Базовый</th>
      <td>9</td>
    </tr>
    <tr>
      <th>Java-разработчик</th>
      <td>17</td>
    </tr>
    <tr>
      <th>Java-разработчик c нуля</th>
      <td>9</td>
    </tr>
    <tr>
      <th>JavaScript с нуля</th>
      <td>18</td>
    </tr>
    <tr>
      <th>PHP-разработчик с 0 до PRO. Часть 1</th>
      <td>8</td>
    </tr>
    <tr>
      <th>SMM-маркетолог от А до Я</th>
      <td>11</td>
    </tr>
    <tr>
      <th>UX-дизайн</th>
      <td>20</td>
    </tr>
    <tr>
      <th>Анимация интерфейсов</th>
      <td>21</td>
    </tr>
    <tr>
      <th>Веб-вёрстка для начинающих 2.0</th>
      <td>8</td>
    </tr>
    <tr>
      <th>Веб-дизайн PRO 2.0</th>
      <td>17</td>
    </tr>
    <tr>
      <th>Веб-дизайн Базовый</th>
      <td>17</td>
    </tr>
    <tr>
      <th>Веб-дизайн с нуля 2.0</th>
      <td>19</td>
    </tr>
    <tr>
      <th>Веб-разработчик</th>
      <td>20</td>
    </tr>
    <tr>
      <th>Интернет-маркетолог от Ingate</th>
      <td>18</td>
    </tr>
    <tr>
      <th>Руководитель digital-проектов</th>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



### Количество уроков в каждом модуле на каждом курсе


```python
all_lessons_df = merged_df.groupby(
    ['title', 'module_title'])[['lesson_number']].agg(
    lambda x: len(set(x)))

all_lessons_df
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>lesson_number</th>
    </tr>
    <tr>
      <th>title</th>
      <th>module_title</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Excel Базовый</th>
      <th>Визуализация данных Excel</th>
      <td>5</td>
    </tr>
    <tr>
      <th>Основной функционал Excel</th>
      <td>11</td>
    </tr>
    <tr>
      <th>Основной функционал Excel (продолжение)</th>
      <td>7</td>
    </tr>
    <tr>
      <th>Сводные таблицы Excel</th>
      <td>5</td>
    </tr>
    <tr>
      <th>Формулы и функции Excel. Более сложные формулы</th>
      <td>5</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">Руководитель digital-проектов</th>
      <th>Решение факапов. Lean/TOC. Обзор.</th>
      <td>5</td>
    </tr>
    <tr>
      <th>Требовательность digital-продюсера</th>
      <td>4</td>
    </tr>
    <tr>
      <th>Управление временем</th>
      <td>4</td>
    </tr>
    <tr>
      <th>Управление дизайнерами. Разработка дизайна по scrum</th>
      <td>7</td>
    </tr>
    <tr>
      <th>Экологичный путь менеджера</th>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>229 rows × 1 columns</p>
</div>



### Медианное количество уроков в модуле на каждом курсе


```python
med_lessons_df = all_lessons_df.groupby('title').median()
med_lessons_df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>lesson_number</th>
    </tr>
    <tr>
      <th>title</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Excel Базовый</th>
      <td>5.0</td>
    </tr>
    <tr>
      <th>Java-разработчик</th>
      <td>7.0</td>
    </tr>
    <tr>
      <th>Java-разработчик c нуля</th>
      <td>13.0</td>
    </tr>
    <tr>
      <th>JavaScript с нуля</th>
      <td>7.0</td>
    </tr>
    <tr>
      <th>PHP-разработчик с 0 до PRO. Часть 1</th>
      <td>4.0</td>
    </tr>
    <tr>
      <th>SMM-маркетолог от А до Я</th>
      <td>6.0</td>
    </tr>
    <tr>
      <th>UX-дизайн</th>
      <td>3.5</td>
    </tr>
    <tr>
      <th>Анимация интерфейсов</th>
      <td>3.0</td>
    </tr>
    <tr>
      <th>Веб-вёрстка для начинающих 2.0</th>
      <td>7.0</td>
    </tr>
    <tr>
      <th>Веб-дизайн PRO 2.0</th>
      <td>5.0</td>
    </tr>
    <tr>
      <th>Веб-дизайн Базовый</th>
      <td>3.0</td>
    </tr>
    <tr>
      <th>Веб-дизайн с нуля 2.0</th>
      <td>4.0</td>
    </tr>
    <tr>
      <th>Веб-разработчик</th>
      <td>2.0</td>
    </tr>
    <tr>
      <th>Интернет-маркетолог от Ingate</th>
      <td>6.5</td>
    </tr>
    <tr>
      <th>Руководитель digital-проектов</th>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>



### Количество учеников на каждом курсе 


```python
students_count = merged_df.groupby('title')[['student_id']].agg(
    lambda x: len(set(x)))

students_count
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>student_id</th>
    </tr>
    <tr>
      <th>title</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Excel Базовый</th>
      <td>782</td>
    </tr>
    <tr>
      <th>Java-разработчик</th>
      <td>763</td>
    </tr>
    <tr>
      <th>Java-разработчик c нуля</th>
      <td>581</td>
    </tr>
    <tr>
      <th>JavaScript с нуля</th>
      <td>966</td>
    </tr>
    <tr>
      <th>PHP-разработчик с 0 до PRO. Часть 1</th>
      <td>854</td>
    </tr>
    <tr>
      <th>SMM-маркетолог от А до Я</th>
      <td>506</td>
    </tr>
    <tr>
      <th>UX-дизайн</th>
      <td>1151</td>
    </tr>
    <tr>
      <th>Анимация интерфейсов</th>
      <td>598</td>
    </tr>
    <tr>
      <th>Веб-вёрстка для начинающих 2.0</th>
      <td>2004</td>
    </tr>
    <tr>
      <th>Веб-дизайн PRO 2.0</th>
      <td>1711</td>
    </tr>
    <tr>
      <th>Веб-дизайн Базовый</th>
      <td>518</td>
    </tr>
    <tr>
      <th>Веб-дизайн с нуля 2.0</th>
      <td>2014</td>
    </tr>
    <tr>
      <th>Веб-разработчик</th>
      <td>628</td>
    </tr>
    <tr>
      <th>Интернет-маркетолог от Ingate</th>
      <td>2168</td>
    </tr>
    <tr>
      <th>Руководитель digital-проектов</th>
      <td>685</td>
    </tr>
  </tbody>
</table>
</div>



### Минимальный, максимальный, средний, медианный возраст студентов


```python

#Переводим дни рождения в datetime
students['birthday'] = pd.to_datetime(
    students['birthday'], errors='coerce')

#Из дней рождения вычисляем возраст
students['age'] = ((pd.to_datetime(
    date.today()) - students['birthday']).dt.days) // 365.25

#Выводим сводную информацию о возрастах
students['age'].describe()
```




    count    25490.000000
    mean        30.384661
    std          8.246238
    min       -179.000000
    25%         25.000000
    50%         30.000000
    75%         35.000000
    max        137.000000
    Name: age, dtype: float64




```python
#Выводим медиану
students['age'].median()
```




    30.0




```python
#Графически визуализируем значения возрастов
sns.histplot(students.age)
```








    
![output_20_1](https://user-images.githubusercontent.com/73818584/134919534-9b7daa5b-c879-42a5-9187-85498ac2e9fd.png)
    


В датафрейме присутствуют некорректные значения возрастов, вероятно, форма сайта позволяет пользователям выбирать некорректные значения
Для наиболее корректной статистики отсортируем возраста по следующим границам:
-14 лет - получение паспорта, возможности оформления договоров
-70 лет - усредненная продолжительность жизни


```python
# Отсортируем аномальные значения, возьмём
correct_ages_students = students[(students['age'] >= 14) & (
    students['age'] <= 70)]
correct_ages_students.describe()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>25359.000000</td>
      <td>25359.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>82817.378564</td>
      <td>30.405615</td>
    </tr>
    <tr>
      <th>std</th>
      <td>37878.295560</td>
      <td>7.478391</td>
    </tr>
    <tr>
      <th>min</th>
      <td>3898.000000</td>
      <td>14.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>37522.000000</td>
      <td>25.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>102751.000000</td>
      <td>30.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>113509.500000</td>
      <td>35.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>124547.000000</td>
      <td>70.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Выводим медиану
correct_ages_students.age.median()
```




    30.0




```python
#Графически визуализируем корректные значения возрастов
sns.histplot(correct_ages_students.age)
```




 




    
![output_24_1](https://user-images.githubusercontent.com/73818584/134919753-2f7a5aba-538f-47c8-81a6-dd6e4cae1a7e.png)
    


### Минимальный, максимальный, средний, медианный возраст студентов на каждом курсе



```python
#Соединяем нужные таблицы
students_courses = progresses.merge(
    correct_ages_students, left_on='student_id', 
    right_on='id').merge(courses, 
    left_on='course_id', 
    right_on='id'
)
#Группируем и агрегируем 
students_courses\
    .groupby('title')\
    .agg({'age' : ['min', 'median', 'mean', 'max']})

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">age</th>
    </tr>
    <tr>
      <th></th>
      <th>min</th>
      <th>median</th>
      <th>mean</th>
      <th>max</th>
    </tr>
    <tr>
      <th>title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Excel Базовый</th>
      <td>17.0</td>
      <td>35.0</td>
      <td>35.000000</td>
      <td>62.0</td>
    </tr>
    <tr>
      <th>Java-разработчик</th>
      <td>15.0</td>
      <td>30.0</td>
      <td>30.688501</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>Java-разработчик c нуля</th>
      <td>15.0</td>
      <td>30.0</td>
      <td>30.870690</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>JavaScript с нуля</th>
      <td>15.0</td>
      <td>29.0</td>
      <td>29.951860</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>PHP-разработчик с 0 до PRO. Часть 1</th>
      <td>15.0</td>
      <td>30.0</td>
      <td>30.804726</td>
      <td>67.0</td>
    </tr>
    <tr>
      <th>SMM-маркетолог от А до Я</th>
      <td>18.0</td>
      <td>30.0</td>
      <td>30.368209</td>
      <td>53.0</td>
    </tr>
    <tr>
      <th>UX-дизайн</th>
      <td>16.0</td>
      <td>31.0</td>
      <td>31.487952</td>
      <td>59.0</td>
    </tr>
    <tr>
      <th>Анимация интерфейсов</th>
      <td>16.0</td>
      <td>30.0</td>
      <td>30.959032</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>Веб-вёрстка для начинающих 2.0</th>
      <td>15.0</td>
      <td>29.0</td>
      <td>29.974785</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>Веб-дизайн PRO 2.0</th>
      <td>16.0</td>
      <td>29.0</td>
      <td>29.813676</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>Веб-дизайн Базовый</th>
      <td>17.0</td>
      <td>29.0</td>
      <td>29.954787</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>Веб-дизайн с нуля 2.0</th>
      <td>14.0</td>
      <td>28.0</td>
      <td>29.512310</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>Веб-разработчик</th>
      <td>15.0</td>
      <td>29.0</td>
      <td>29.501066</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>Интернет-маркетолог от Ingate</th>
      <td>17.0</td>
      <td>32.0</td>
      <td>32.575040</td>
      <td>61.0</td>
    </tr>
    <tr>
      <th>Руководитель digital-проектов</th>
      <td>20.0</td>
      <td>33.0</td>
      <td>32.908766</td>
      <td>56.0</td>
    </tr>
  </tbody>
</table>
</div>



### Постройте bar-chart, отражающий количество студентов на каждом курсе. Ticks нужно развернуть так, чтобы они были читаемы



```python
#Размечаем график
fig, ax = plt.subplots(figsize=(18,6))

# Выбираем нужные данные

students_count = merged_df.groupby(
    ['field', 'title'])[['student_id']].agg(
    lambda x: len(set(x))).sort_values(
    'student_id', ascending=False).reset_index()
# Вызываем график

ax.bar(students_count['title'], students_count['student_id'])

# Настраиваем параметры отображения 

ax.set_title('Количество студентов на каждом курсе')
ax.set_xlabel('Курс')
ax.set_ylabel('Количество студентов')
plt.xticks(rotation=90)

plt.show()
```


    
![output_28_0](https://user-images.githubusercontent.com/73818584/134919817-1c92f33f-074f-4bfd-ba42-dfa6afc43248.png)
    


### Постройте горизонтальный (столбцы должны располагаться горизонтально) bar-chart, отражающий количество студентов на каждом курсе. График должен иметь заголовок. Значения должны быть отсортированы. Цвет столбцов должен содержать информацию о сфере, к которой относится курс (то есть нужна легенда). Прозрачность должна стоять на отметке 0.1. На график должна быть нанесена линия медианы. У медианы должен быть свой цвет. Рамки у графика быть не должно


```python
#Размечаем график
fig, ax = plt.subplots(figsize=(18,6))

 
# Вызываем график
sns.barplot(data=students_count,
            x='student_id',
            y='title', 
            hue='field', 
            alpha=0.1,
            orient = 'h')

#Настраиваем медианную черту
ax.axvline(x=students_count['student_id'].median(), 
           color='purple')

# Настраиваем параметры отображения 
ax.set_title('Количество студентов на каждом курсе')
ax.set_ylabel('Курс')
ax.set_xlabel('Количество студентов')
plt.box(on=None)
```


    
![output_30_0](https://user-images.githubusercontent.com/73818584/134919840-76d23a77-53ad-4b09-b042-d176f45db128.png)
    


### Общие выводы:
       -Больше всего модулей в курсе "Анимация интерфейсов"(21), 
       меньше всего - "Веб-вёрстка для начинающих 2.0" и "PHP-разработчик с 0 до PRO"(по 8)
       
       -Наиболее интенсивный курс - "Java-разработчик c нуля", 
       наименее - "Java-разработчик c нуля"
       
       -Больше всего студентов на курсе "Интернет-маркетолог от Ingate",
       меньше всего - "SMM-маркетолог от А до Я"
       
       -Распределение возрастов по квартилям: 
       25% 25 лет
       50% 30 лет
       75% 35 лет
       
       -Самые в молодые курсы - "Веб-разработчик" и "Веб-дизайн"
       самые возрастные - "Excel базовый" и "Руководитель digital-проектов"
       
       -Наиболее востребованные сферы деятельности - "Design", "Development" и"Marketing"
      наименее - "Business" 

## 2. Расчет потенциальной нагрузки на преподавателей

1. **(p)** Рассчитать прирост студентов на каждом курсе в каждом месяце за всю историю (каждый месяц в диапазоне от марта 2016 до июля 2019 включительно). Считать дату начала прохождения курса студентом по дате начала первой домашней работы.
2. **(m)** На основании первого пункта построить line-graph с приростом студентов в каждом месяце для каждого курса. 15 графиков. Графики должны иметь заголовки, оси должны быть подписаны. Ticks нужно развернуть так, чтобы они были читаемы.
3. **(m)** На основании первого пункта построить line-graph с несколькими линиями, отражающими прирост студентов в каждом месяце для каждого курса. 15 линий на графике. Ticks нужно развернуть так, чтобы они были читаемы. График должен иметь заголовок. Ось, отражающая прирост, должна быть подписана. Линия для каждого курса должна иметь свой цвет (нужна легенда). Рамок у графика быть не должно ⭐
4. **(p)** Рассчитать количество прогрессов по выполнению домашних работ в каждом месяце за всю историю (каждый месяц в диапазоне от марта 2016 до июля 2019 включительно) для каждого курса. Учитывать, что выполнение домашнего задания может перетекать из одного месяца в другой (такие дз надо включать в общее число прогрессов для всех месяцев, которые покрывает срок выполнения этих дз)
5. **(m)** Построить line-graph по четвертому пункту. 15 графиков. Графики должны иметь заголовки, оси должны быть подписаны. Ticks нужно развернуть так, чтобы они были читаемы
6. **(m)** Построить один line-graph для всех курсов по четвертому пункту. 15 линий на графике. Ticks нужно развернуть так, чтобы они были читаемы. График должен иметь заголовок. Ось, отражающая количество прогрессов, должна быть подписана. Линия для каждого курса должна иметь свой цвет (нужна легенда). Рамок у графика быть не должно ⭐
7. На основании рассчитанных значений сделайте аналитический вывод (должен быть полным и покрывать все полученные выше метрики)

### Рассчитать прирост студентов на каждом курсе в каждом месяце за всю историю (каждый месяц в диапазоне от марта 2016 до июля 2019 включительно). Считать дату начала прохождения курса студентом по дате начала первой домашней работы.


```python
#Сшиваем датасет с расчетом по минимальному домашнему заданию
growth_df = merged_df[merged_df['is_homework']==True].groupby('title').agg(
    {'module_number' : 'min'}).reset_index().merge(merged_df, 
                                                   left_on=['title', 'module_number'],
                                                   right_on=['title', 'module_number'])

#Приводим даты в формат месяцев
growth_df[['start_date', 'finish_date']] = growth_df[[
    'start_date', 'finish_date']].astype('datetime64[M]')

#Группируем и агригируем данные по месяцам
growth_df = growth_df.groupby(
    ['title', 'start_date']).agg(
    {'finish_date':'count'}).unstack(level=0).fillna(0).stack().reset_index()

#Выводим результат  
growth_df
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>start_date</th>
      <th>title</th>
      <th>finish_date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-03-01</td>
      <td>Excel Базовый</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-03-01</td>
      <td>Java-разработчик</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-03-01</td>
      <td>Java-разработчик c нуля</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-03-01</td>
      <td>JavaScript с нуля</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-03-01</td>
      <td>PHP-разработчик с 0 до PRO. Часть 1</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>475</th>
      <td>2019-07-01</td>
      <td>Веб-дизайн Базовый</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>476</th>
      <td>2019-07-01</td>
      <td>Веб-дизайн с нуля 2.0</td>
      <td>765.0</td>
    </tr>
    <tr>
      <th>477</th>
      <td>2019-07-01</td>
      <td>Веб-разработчик</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>478</th>
      <td>2019-07-01</td>
      <td>Интернет-маркетолог от Ingate</td>
      <td>623.0</td>
    </tr>
    <tr>
      <th>479</th>
      <td>2019-07-01</td>
      <td>Руководитель digital-проектов</td>
      <td>71.0</td>
    </tr>
  </tbody>
</table>
<p>480 rows × 3 columns</p>
</div>




```python
# Определяем переменную названий курсов
titles = growth_df['title'].unique()
```

### На основании первого пункта построить line-graph с приростом студентов в каждом месяце для каждого курса. 15 графиков. Графики должны иметь заголовки, оси должны быть подписаны. Ticks нужно развернуть так, чтобы они были читаемы.


```python
# Определеяем полотно и диаграммы
fig, axes = plt.subplots(5, 3, figsize=(15, 20))

# Выводим диаграммы на полотно
for index, ax in enumerate(axes.flat):
    sns.lineplot(data=growth_df[growth_df['title']==titles[index]],
                 x='start_date', y='finish_date', ax=ax)
    
    # Подписи осей и настройка отображения
    ax.set_xlabel('Дата начала прохождения курса')
    ax.set_ylabel('Количество студентов')
    ax.set_title(titles[index])
    plt.setp(ax.get_xticklabels(), rotation=45)
    
#Автоматически подгоняем и выводим график 
fig.tight_layout()
plt.show()
```


    
![output_37_0](https://user-images.githubusercontent.com/73818584/134919869-300d8687-f2c7-47e6-822c-d235b9de3d7d.png)
    


### На основании первого пункта построить line-graph с несколькими линиями, отражающими прирост студентов в каждом месяце для каждого курса. 15 линий на графике. Ticks нужно развернуть так, чтобы они были читаемы. График должен иметь заголовок. Ось, отражающая прирост, должна быть подписана. Линия для каждого курса должна иметь свой цвет (нужна легенда). Рамок у графика быть не должно


```python
#Определеяем полотно и график
fig, axes = plt.subplots(figsize=(15, 10))

#Выбираем данные для визуализации
ax = sns.lineplot(data=growth_df, x='start_date', 
                  y='finish_date', hue='title')

# Подписи и настройка отображения
ax.set_title('Прирост студентов на курсах')
ax.set_xlabel('Дата начала прохождения курса')
ax.set_ylabel('Количество студентов')
plt.box(False)

#Автоматически подгоняем и выводим график 
fig.tight_layout()
plt.show()
```


    
![png](output_39_0.png)
    


### Рассчитать количество прогрессов по выполнению домашних работ в каждом месяце за всю историю (каждый месяц в диапазоне от марта 2016 до июля 2019 включительно) для каждого курса. Учитывать, что выполнение домашнего задания может перетекать из одного месяца в другой (такие дз надо включать в общее число прогрессов для всех месяцев, которые покрывает срок выполнения этих дз)


```python
# Оставляем в датасете только информацию о курсах, статусе и датам домашних работ
progress_df = merged_df.loc[(merged_df['status']=='done') &( 
                            merged_df['is_homework']==True),
                            ['title', 'start_date', 'finish_date']]

# Преобразовываем формат даты
progress_df['start_date'] = progress_df['start_date'].astype(
    'datetime64[M]')
progress_df['finish_date'] = progress_df['finish_date'].astype(
    'datetime64[M]') 


#Выводим результат       
progress_df = progress_df.groupby(['title','start_date']).agg(
    {'finish_date':'count'}).unstack(level=0).fillna(0).stack().reset_index()
```

### Построить line-graph по четвертому пункту. 15 графиков. Графики должны иметь заголовки, оси должны быть подписаны. Ticks нужно развернуть так, чтобы они были читаемы


```python
# Определеяем полотно и диаграммы
fig, axes = plt.subplots(5, 3, figsize=(15, 20))

# Выводим диаграммы на полотно
for index, ax in enumerate(axes.flat):
    data = progress_df[progress_df['title']==titles[index]]
    sns.lineplot(data=data, x='start_date', y='finish_date', ax=ax)
    
    # Подписываем оси и настраиваем отображение
    ax.set_xlabel('Дата выполнения домашнего задания')
    ax.set_ylabel('Количество выполненных заданий')
    ax.set_title(titles[index])
    plt.setp(ax.get_xticklabels(), rotation=45)
    
fig.tight_layout()
plt.show()
```


    
![output_43_0](https://user-images.githubusercontent.com/73818584/134919919-c5ac81a9-4135-4fb6-80c3-db02e3751783.png)
    


###  Построить один line-graph для всех курсов по четвертому пункту. 15 линий на графике. Ticks нужно развернуть так, чтобы они были читаемы. График должен иметь заголовок. Ось, отражающая количество прогрессов, должна быть подписана. Линия для каждого курса должна иметь свой цвет (нужна легенда). Рамок у графика быть не должно


```python
#Определяем полотно и график
fig, ax = plt.subplots(figsize=(12, 8))

#Строим график
sns.lineplot(data=progress_df, x='start_date',
             y='finish_date', hue='title')

# Подписи и настройка отображения
ax.set_title('Прогресс по выполнению домашних заданий')
ax.set_xlabel('Дата выполнения домашнего задания')
ax.set_ylabel('Количество выполненных заданий')
plt.box(False)
fig.tight_layout()
plt.show()
```


    
![output_45_0](https://user-images.githubusercontent.com/73818584/134919940-7ec4ad93-f527-438a-8efd-1471a3ebd27e.png)
    


### На основании рассчитанных значений сделайте аналитический вывод (должен быть полным и покрывать все полученные выше метрики)

       -Наиболее интенсивные в плане роста и общих показателей курсы 
           "Java разработчик",
           "Веб-верстка для начинающих 2.0",
           "Интернет-маркетолог от Ingate
       Наименее:
           "Веб-разработчик",
           "Анимация интерфейсов"
           
       -Самый резкий рост по приросту студентов имели курсы
          "Java-разработчик с нуля"
          "Веб-дизайн с нуля 2.0"
        
       -Самый большой спад прироста имели курсы
           "Веб-дизайн базовый"
        Спад, вероятно, обусловлен появлением расширенных программ по Веб-дизайну
            "Веб-дизайн с нуля 2.0"
        
        -Анализ прогресса по домашним заданиям не выявляет никаких аномалий в поведении кривых,
        наблюдается идентичность с поведением кривых по приросту студентов
        
        -Курсы с наибольшим количеством выполненных домашних заданий
            "Веб-дизайн с нуля 2.0",
            "Интернет-маркетолог от Ingate"
            
            
       
       

## 3. Выявление проблемных модулей

1. **(p)** Рассчитать минимальное, максимальное, среднее, медианное время прохождения каждого модуля (разность между временем начала и окончания выполнения домашней работы) для каждого курса. Если домашних заданий в модуле несколько, то считать разность между временем начала выполнения первой домашней работы и временем окончания выполнения последней домашней работы в модуле
2. **(m)** На основании первого пункта построить line-graph с медианным временем прохождения каждого модуля для каждого курса. 15 графиков. Графики должны иметь заголовки
3. **(p)**  Чтобы выявить сезонность, посчитать медианное время выполнения домашней работы по месяцам (12 месяцев, январь-декабрь) для каждого курса. 
4. **(m)** На основании третьего пункта построить line-graph, на который будут нанесены линии для каждого курса с медианным временем выполнения домашней работы по месяцам. 15 линий на графике. График должен иметь заголовок. Ось, отражающая время прохождения, должна быть подписана. Линия для каждого курса должна иметь свой цвет (нужна легенда). Рамок у графика быть не должно  ⭐
5. На основании рассчитанных значений сделайте аналитический вывод (должен быть полным и покрывать все полученные выше метрики)

### Рассчитать минимальное, максимальное, среднее, медианное время прохождения каждого модуля (разность между временем начала и окончания выполнения домашней работы) для каждого курса. Если домашних заданий в модуле несколько, то считать разность между временем начала выполнения первой домашней работы и временем окончания выполнения последней домашней работы в модуле





```python
#Создаём датасет для анализа модулей, чистим его от пропущенных значений
mod_df = merged_df[merged_df['is_homework']==True]\
[['title', 'module_title', 'start_date', 'finish_date']].dropna()

```


```python
#Преобразуем данные в удобный вид
mod_df[['finish_date','start_date']] = mod_df[['finish_date','start_date']].astype(
    'datetime64[D]')

#Cоздаём колонку с длительностью прохождения в днях
mod_df['complete_time'] = (mod_df['finish_date'] - mod_df['start_date']).dt.days


# Выводим сводную информацию о модулях
mod_df.groupby(
    ['title', 'module_title']
).agg(
    {'complete_time': [np.min, np.max, np.mean, np.median]})

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">complete_time</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>amin</th>
      <th>amax</th>
      <th>mean</th>
      <th>median</th>
    </tr>
    <tr>
      <th>title</th>
      <th>module_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Excel Базовый</th>
      <th>Визуализация данных Excel</th>
      <td>0</td>
      <td>175</td>
      <td>10.055000</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>Основной функционал Excel</th>
      <td>0</td>
      <td>184</td>
      <td>6.139738</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>Основной функционал Excel (продолжение)</th>
      <td>0</td>
      <td>185</td>
      <td>4.453202</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>Сводные таблицы Excel</th>
      <td>0</td>
      <td>239</td>
      <td>9.602151</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>Формулы и функции Excel. Более сложные формулы</th>
      <td>0</td>
      <td>176</td>
      <td>7.459259</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">Руководитель digital-проектов</th>
      <th>Решение факапов. Lean/TOC. Обзор.</th>
      <td>0</td>
      <td>212</td>
      <td>21.454545</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>Требовательность digital-продюсера</th>
      <td>0</td>
      <td>397</td>
      <td>17.762319</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>Управление временем</th>
      <td>0</td>
      <td>164</td>
      <td>7.883721</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>Управление дизайнерами. Разработка дизайна по scrum</th>
      <td>0</td>
      <td>199</td>
      <td>14.812500</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>Экологичный путь менеджера</th>
      <td>0</td>
      <td>246</td>
      <td>6.164286</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
<p>190 rows × 4 columns</p>
</div>




```python
#Убираем из датасет аномальные данные 
mod_df = mod_df[mod_df['complete_time'] >= 0 ]

# Выводим очищенную сводную информацию о модулях
mod_df.groupby(
    ['title', 'module_title']
).agg(
    {'complete_time': [np.min, np.max, np.mean, np.median]})

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">complete_time</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>amin</th>
      <th>amax</th>
      <th>mean</th>
      <th>median</th>
    </tr>
    <tr>
      <th>title</th>
      <th>module_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Excel Базовый</th>
      <th>Визуализация данных Excel</th>
      <td>0</td>
      <td>175</td>
      <td>10.055000</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>Основной функционал Excel</th>
      <td>0</td>
      <td>184</td>
      <td>6.139738</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>Основной функционал Excel (продолжение)</th>
      <td>0</td>
      <td>185</td>
      <td>4.453202</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>Сводные таблицы Excel</th>
      <td>0</td>
      <td>239</td>
      <td>9.602151</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>Формулы и функции Excel. Более сложные формулы</th>
      <td>0</td>
      <td>176</td>
      <td>7.459259</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">Руководитель digital-проектов</th>
      <th>Решение факапов. Lean/TOC. Обзор.</th>
      <td>0</td>
      <td>212</td>
      <td>21.454545</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>Требовательность digital-продюсера</th>
      <td>0</td>
      <td>397</td>
      <td>17.762319</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>Управление временем</th>
      <td>0</td>
      <td>164</td>
      <td>7.883721</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>Управление дизайнерами. Разработка дизайна по scrum</th>
      <td>0</td>
      <td>199</td>
      <td>14.812500</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>Экологичный путь менеджера</th>
      <td>0</td>
      <td>246</td>
      <td>6.164286</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
<p>190 rows × 4 columns</p>
</div>




```python
# Вычисляем медианное время прохождения для каждого модуля
medi_mod_df = mod_df.groupby(
    ['title', 'module_title']
).median(
).reset_index(
)
medi_mod_df

```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>module_title</th>
      <th>complete_time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Excel Базовый</td>
      <td>Визуализация данных Excel</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Excel Базовый</td>
      <td>Основной функционал Excel</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Excel Базовый</td>
      <td>Основной функционал Excel (продолжение)</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Excel Базовый</td>
      <td>Сводные таблицы Excel</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Excel Базовый</td>
      <td>Формулы и функции Excel. Более сложные формулы</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>185</th>
      <td>Руководитель digital-проектов</td>
      <td>Решение факапов. Lean/TOC. Обзор.</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Руководитель digital-проектов</td>
      <td>Требовательность digital-продюсера</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>187</th>
      <td>Руководитель digital-проектов</td>
      <td>Управление временем</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>188</th>
      <td>Руководитель digital-проектов</td>
      <td>Управление дизайнерами. Разработка дизайна по ...</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>189</th>
      <td>Руководитель digital-проектов</td>
      <td>Экологичный путь менеджера</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
<p>190 rows × 3 columns</p>
</div>



### На основании первого пункта построить line-graph с медианным временем прохождения каждого модуля для каждого курса. 15 графиков. Графики должны иметь заголовки


```python
#Здесь у нас может вылезти warning, для удобства сразу отфильруем их
import warnings
warnings.filterwarnings("ignore")
```


```python
#Размечаем полотно и графики
fig, axes = plt.subplots(5, 3, figsize=(17, 27))

#Для каждого курса выводим соответствующий график
for index, ax in enumerate(axes.flat):
    sns.lineplot(data=medi_mod_df[medi_mod_df['title'] == titles[index]],
                 x='module_title', y='complete_time', ax=ax)
    
    #Немного обрезаем названия модулей, для более компактной расстоновки
    ax.set_xticklabels(medi_mod_df.loc[medi_mod_df['title']==titles[index], 
     'module_title'].apply( lambda x: x[:12]), rotation=90)
    
    #Подписываем графики и оси координат
    ax.set_title(titles[index])
    ax.set_xlabel('Модуль курса')
    ax.set_ylabel('Медианное время')
    
# Ровняем графики на полотне и вызываем их
fig.tight_layout()
plt.show()
```


    
![output_56_0](https://user-images.githubusercontent.com/73818584/134919985-0dfe3e1d-acc3-4896-8ef5-6a10c89f3c20.png)
    


### Чтобы выявить сезонность, посчитать медианное время выполнения домашней работы по месяцам (12 месяцев, январь-декабрь) для каждого курса


```python
# Определение месяца завершения выполнения домашнего задания
mod_df['homework_month'] = mod_df['finish_date'].apply(lambda x: date(2020, x.month, 1))

# Вывод медианного времени выполнения домашней работы в разрезе месяцев
medi_mod_df = mod_df.pivot_table('complete_time', index='homework_month', columns='title', aggfunc=np.median)
medi_mod_df
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>title</th>
      <th>Excel Базовый</th>
      <th>Java-разработчик</th>
      <th>Java-разработчик c нуля</th>
      <th>JavaScript с нуля</th>
      <th>PHP-разработчик с 0 до PRO. Часть 1</th>
      <th>SMM-маркетолог от А до Я</th>
      <th>UX-дизайн</th>
      <th>Анимация интерфейсов</th>
      <th>Веб-вёрстка для начинающих 2.0</th>
      <th>Веб-дизайн PRO 2.0</th>
      <th>Веб-дизайн Базовый</th>
      <th>Веб-дизайн с нуля 2.0</th>
      <th>Веб-разработчик</th>
      <th>Интернет-маркетолог от Ingate</th>
      <th>Руководитель digital-проектов</th>
    </tr>
    <tr>
      <th>homework_month</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-01</th>
      <td>2.0</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>5.5</td>
      <td>9.0</td>
      <td>16.5</td>
      <td>10.0</td>
      <td>7.0</td>
      <td>10.0</td>
      <td>12.0</td>
      <td>11.5</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2020-02-01</th>
      <td>2.0</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>7.0</td>
      <td>17.0</td>
      <td>10.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2020-03-01</th>
      <td>3.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>8.0</td>
      <td>12.0</td>
      <td>10.0</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>7.0</td>
      <td>12.0</td>
      <td>5.0</td>
      <td>9.5</td>
      <td>6.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2020-04-01</th>
      <td>2.0</td>
      <td>9.0</td>
      <td>3.0</td>
      <td>5.0</td>
      <td>11.0</td>
      <td>9.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>10.5</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>8.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>2020-05-01</th>
      <td>3.0</td>
      <td>13.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>9.0</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>8.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>2020-06-01</th>
      <td>2.0</td>
      <td>12.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>14.5</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>5.0</td>
      <td>9.5</td>
      <td>5.0</td>
      <td>6.5</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>2020-07-01</th>
      <td>2.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>7.5</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>8.0</td>
      <td>8.0</td>
      <td>5.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>2020-08-01</th>
      <td>3.0</td>
      <td>11.0</td>
      <td>NaN</td>
      <td>8.0</td>
      <td>11.0</td>
      <td>NaN</td>
      <td>8.0</td>
      <td>3.5</td>
      <td>5.0</td>
      <td>24.0</td>
      <td>10.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>2020-09-01</th>
      <td>2.0</td>
      <td>11.0</td>
      <td>NaN</td>
      <td>10.0</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>24.0</td>
      <td>11.5</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>10.5</td>
    </tr>
    <tr>
      <th>2020-10-01</th>
      <td>2.0</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>8.0</td>
      <td>12.0</td>
      <td>5.0</td>
      <td>11.0</td>
      <td>21.0</td>
      <td>16.0</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>7.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2020-11-01</th>
      <td>2.0</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>16.0</td>
      <td>16.0</td>
      <td>11.0</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>11.0</td>
      <td>14.0</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>2020-12-01</th>
      <td>4.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>14.0</td>
      <td>10.0</td>
      <td>13.0</td>
      <td>7.0</td>
      <td>10.0</td>
      <td>20.0</td>
      <td>7.0</td>
      <td>8.5</td>
      <td>5.0</td>
      <td>7.0</td>
    </tr>
  </tbody>
</table>
</div>



### На основании третьего пункта построить line-graph, на который будут нанесены линии для каждого курса с медианным временем выполнения домашней работы по месяцам. 15 линий на графике. График должен иметь заголовок. Ось, отражающая время прохождения, должна быть подписана. Линия для каждого курса должна иметь свой цвет (нужна легенда). Рамок у графика быть не должно


```python
# Создание основных объектов графика
fig, ax = plt.subplots(figsize=(20, 10))

# Вывод графика на страницу
medi_mod_df.plot(ax=ax,title='Медианное время выполнения домашней работы в разрезе месяцев')

#Настраиваем отображение и выводим график
plt.tight_layout()
plt.box(False)
plt.show()
```


    
![output_60_0](https://user-images.githubusercontent.com/73818584/134920008-8a42c323-316a-490d-a45a-b0cb6a078813.png)
    


### На основании рассчитанных значений сделайте аналитический вывод (должен быть полным и покрывать все полученные выше метрики)

    -В данных наблюдаются аномальные данные в столбцах, содержащих время старта и финиша, возможно, имеет место сбои систем, отслеживающих процесс
    -Самый длительный по прохождению модуль среди курсов:
        "PHP-разработчик с 0 до PRO. Часть 1 - Курсовая работа" - 89 дней
    -Диаграмма сезонности показывает:
        Резкий спад времени выполнения в феврале у большинства курсов
        Последующий рост с пиками в июне, августе и до декабря

## 4. Расчет конверсии

1. **(p)** Посчитать конверсию перехода студентов из одного модуля в другой на каждом курсе. Формула: отношение количества студентов, приступивших к выполнению домашнего задания в этом модуле (если дз в модуле несколько, то считать по первому дз в модуле), к количеству студентов, сдавших задание в предыдущем модуле (если дз в модуле несколько, то считать по последнему дз в модуле).
2. **(m)** Постройте bar-chart, отражающий конверсию перехода студентов из одного модуля в другой на каждом курсе. График должен иметь заголовок. Ticks нужно развернуть так, чтобы они были читаемы
3. **(m)** Постройте горизонтальный (столбцы должны располагаться горизонтально) bar-chart, отражающий конверсию перехода студентов из одного модуля в другой на каждом курсе. 15 графиков. Графики должны иметь заголовки. Ticks должны содержать номер и название модуля. Цвет столбцов графиков должен содержать информацию о сфере, к которой относится курс (нужна легенда). Прозрачность должна стоять на отметке 0.1. На графики должна быть нанесена линия медианы конверсии для каждого курса. У медианы должен быть свой цвет. Рамок у графиков быть не должно ⭐
4. На основании рассчитанных значений сделайте аналитический вывод (должен быть полным и покрывать все полученные выше метрики)

### Посчитать конверсию перехода студентов из одного модуля в другой на каждом курсе. Формула: отношение количества студентов, приступивших к выполнению домашнего задания в этом модуле (если дз в модуле несколько, то считать по первому дз в модуле), к количеству студентов, сдавших задание в предыдущем модуле (если дз в модуле несколько, то считать по последнему дз в модуле)





```python
#Формируем датасет с группировкой по студенту с агрегацией дат домашних заданий
conversion_df = merged_df.loc[merged_df['is_homework']==True, 
    ['title', 'field', 'module_number', 
     'module_title', 'student_id', 'start_date', 'finish_date']
].groupby(
    ['field', 'title', 'module_number', 
     'module_title', 'student_id']
).agg(
{'start_date': np.min, 'finish_date': np.max}
)

```


```python
#Подсчитываем количество студентов начавших и завершивших модули
conversion_df = conversion_df.assign(
    student_start=conversion_df['start_date'].notnull(), 
    student_finish=conversion_df['finish_date'].notnull()
).groupby(
    ['title', 'field', 'module_number', 'module_title']
).aggregate(
    {'student_start': np.sum, 'student_finish': np.sum,
     'start_date': np.min, 'finish_date': np.max}
).sort_index().reset_index()

# Преобразовываем дату в формат по дням
conversion_df['start_date'] = conversion_df['start_date'
                                           ].astype('datetime64[D]')
conversion_df['finish_date'] = conversion_df['finish_date'
                                            ].astype('datetime64[D]')

# Расчитываем конверсии
for course in conversion_df['title'].unique():
    for index, module_number in conversion_df.loc[conversion_df['title']==course, 
                                                  'module_number'].items():
        
        # Определяем количество студентов, начавших обучение в текущем модуле
        student_start = conversion_df.loc[index, 'student_start']
        
        # Определение количества студентов, завершивших обучение в предыдущем модуле
        student_finish = conversion_df.loc[
            (conversion_df['title']==course) & (
                conversion_df['module_number'] < module_number), 
            'student_finish'].tail(1).min()
        
        # Подсчитываем конверсии текущего модуля
        conversion_df.loc[index, 'conversion'] = student_start / student_finish\
        if student_finish > 0 else 0

#Осматриваем получившийся датафрейм
conversion_df.head()        
```

### Постройте bar-chart, отражающий конверсию перехода студентов из одного модуля в другой на каждом курсе. График должен иметь заголовок. Ticks нужно развернуть так, чтобы они были читаемы


```python
#Размечаем полотно и график
fig, ax = plt.subplots(figsize=(30, 10))

#Строим график по выше полученному датасету
sns.barplot(data=conversion_df, x='module_title', y='conversion',
                                            hue='title', ci=False)

#Настройка отображения и подписей
ax.set_xlabel('Модули курса')
ax.set_ylabel('Конверсия')
plt.xticks(rotation=90)

#Выводим график
plt.show()
```


    
![output_68_0](https://user-images.githubusercontent.com/73818584/134920045-991e7fe7-93ca-49e6-98eb-60ffeb2bf541.png)
    


### Постройте горизонтальный (столбцы должны располагаться горизонтально) bar-chart, отражающий конверсию перехода студентов из одного модуля в другой на каждом курсе. 15 графиков. Графики должны иметь заголовки. Ticks должны содержать номер и название модуля. Цвет столбцов графиков должен содержать информацию о сфере, к которой относится курс (нужна легенда). Прозрачность должна стоять на отметке 0.1. На графики должна быть нанесена линия медианы конверсии для каждого курса. У медианы должен быть свой цвет. Рамок у графиков быть не должно


```python
import matplotlib.colors as colors

#Размечаем полотно и график
fig, ax = plt.subplots(15, 1, figsize=(30, 60))


# Создаём словарь сфера деятельности - цвет
colors = dict(zip(conversion_df['field'].unique(), 
                  colors.XKCD_COLORS))
#Создаём список курсов
courses_list = np.sort(conversion_df['title'].unique())

# Выводим диаграмм на страницу
for index, ax in enumerate(ax.flat):
    
    #Для удобства создаём датасет с данными только по нужному курсу
    chart_df = conversion_df[
        conversion_df['title']==courses_list[index]].reset_index(drop=True)
    
    # Выводим диаграммы на страницу
    chart_df.pivot_table(
        values='conversion', index=chart_df.index, 
        columns='field')
    sns.barplot(data=chart_df,
                 x='conversion', y='module_title', ax=ax, orient='h',
                ci=False, color=colors[
                    chart_df['field'].unique()[0]], alpha=0.1)
    
    # Размечаем линию медианы, и настраиваем отображение графика
    ax.set_title(titles[index])
    ax.vlines(chart_df['conversion'].median(
    ), ax.get_ylim()[0], ax.get_ylim()[1], color='r')
    
    ax.set_frame_on(False)
    ax.set_xlabel('Конверсия перехода')
    ax.set_ylabel('Модули курса')
    
    
#Подгоняем график 
fig.tight_layout()
  
```


    
![output_70_0](https://user-images.githubusercontent.com/73818584/134920089-ca1ee2eb-4bac-4a43-8b48-5f21099d4083.png)
    


### На основании рассчитанных значений сделайте аналитический вывод (должен быть полным и покрывать все полученные выше метрики)

    -Наибольшая положительная аномалия в конверсии наблюдается на курсе 
        "SMM-маркетолог от А до Я" в модуле "Продвижение в VK, FB, MyTarget", более 500%, это, вероятно,  модуль был добавлен с опозданием, соответственно, студенты выполняли его по прошествию некоторого времени c прохождения основной части, из за чего конверсия преобрела такие показатели
        
    -Наибольшая отрицательная аномалия наблюдается на курсе 
        "Анимация интерфейсов". На модуле "13. Анимация статичной концепции" конверсия достигаей нулевого  показателя, вероятно, у студентов возникли трудности с выполнением заданий в предыдущем модуле, InVisio Studio
        
    -В остальном, конверсия в остальных курсах колеблется без серьёзных аномалий 
