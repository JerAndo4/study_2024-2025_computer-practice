---
## Front matter
title: "Моделирование сетей передачи данных"
subtitle: "Отчёт по лабораторной работе №3: Измерение и тестирование пропускной способности сети. Воспроизводимый эксперимент"
author: "Кармацкий Никита Сергеевич"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---
# Цель работы

Основной целью работы является знакомство с инструментом для измерения пропускной способности сети в режиме реального времени — iPerf3, а также получение навыков проведения воспроизводимого эксперимента по измерению пропускной способности моделируемой сети в среде Mininet.

# Выполнение лабораторной работы

1. С помощью API Mininet создаем простейшую топологию сети, состоящую из двух хостов и коммутатора с назначенной по умолчанию Mininet сетью 10.0.0.0/8. В каталоге /work/lab_iperf3 для работы над проектом создаем подкаталог lab_iperf3_topo и скопируем в него файл с примером скрипта mininet/examples/emptynet.py, описывающего стандартную простую топологию сети Mininet. (рис. [-@fig:001]):

![Создание подкаталога и копирование файлов](image/1.PNG){ #fig:001 width=100% height=75% }

2. Изучим содержание скрипта lab_iperf3_topo.py (рис. [-@fig:002]):

![Скрипт lab_iperf3_topo.py](image/2.PNG){ #fig:002 width=100% height=75% }

3. Запустим скрипт создания топологии lab_iperf3_topo.py. После отработки скрипта просмотрим 
элементы топологии и завершим работу mininet (рис. [-@fig:003]):

![Запуск скрипта и просмотр элементов топологии](image/3.PNG){ #fig:003 width=100% height=75% }

4. Следующим шагом внесём в скрипт lab_iperf3_topo.py изменение, позволяющее вывести на экран  информацию о хосте h1, а именно имя хоста, его IP-адрес, MAC-адрес. Для этого после строки, задающей старт работы сети, добавим нужную строку(рис. [-@fig:004]):

![Редактирование скрипта для просмотра информации по хосту h1](image/4.PNG){ #fig:004 width=100% height=75% }

5. Проверим корректность отработки изменённого скрипта (рис. [-@fig:005]):

![Проверка работоспособности скрипта](image/5.PNG){ #fig:005 width=100% height=75% }

6. Затем изменим скрипт lab_iperf3_topo.py так, чтобы на экран выводилась информация об имени, IP-адресе и MAC-адресе обоих хостов сети и проверим корректность отработки изменённого скрипта (рис. [-@fig:006] - рис. [-@fig:007]):

![Изменение скрипта для просмотра инофрмации по двум хостам](image/6.PNG){ #fig:006 width=100% height=75% }

![Просмотр результатов работы скрипта](image/7.PNG){ #fig:007 width=100% height=75% }

7. Mininet предоставляет функции ограничения производительности и изоляции с помощью классов СPULimitedHost и TCLink. Добавим в скрипт настройки параметров производительности. Для начала сделаем копию скрипта lab_iperf3_topo.py (рис. [-@fig:008]):

![Копирование скрипта lab_iperf3_topo.py](image/8.PNG){ #fig:008 width=100% height=30% }

8. В начале скрипта lab_iperf3_topo2.py добавим записи об импорте классов CPULimitedHost и TCLink.  Далее изменим строку описания сети, указав на использование ограничения производительности и изоляции. Следующим шагом изменим функцию задания параметров виртуального хоста h1, указав, что ему будет выделено 50% от общих ресурсов процессора системы. Аналогичным образом для хоста h2 зададим долю выделения ресурсов процессора в 45%. В конце изменим функцию параметров соединения между хостом h1 и коммутатором s3 (рис. [-@fig:009]):

![Изменение скрипта: Добавляем импорт новых классов, меняем строку описание сети, задаем новые параметры для хоста h1 и h2, меняем соелиение между хостом h1 и коммутатором s3 ](image/9.PNG){ #fig:009 width=100% height=75% }

9. Запускаем скрипт на отработку (рис. [-@fig:010]):

![Запуск скрипта и результаты работы](image/10.PNG){ #fig:010 width=100% height=75% }

10. Перед завершением лабораторной работы, построим графики по проводимому эксперименту. Для этого сделаем копию скрипта lab_iperf3_topo2.py и поместим его в подкаталог iperf(рис. [-@fig:011]):

![Создаем новый подкаталог и копируем наш скрипт lab_iperf_topo2.py](image/11.PNG){ #fig:011 width=100% height=75% }

11. В начале скрипта lab_iperf3.py добавим запись об импорте time и изменим код в скрипте так, чтобы (рис. [-@fig:012]):
-  на хостах не было ограничения по использованию ресурсов процессора;
- каналы между хостами и коммутатором были по 100 Мбит/с с задержкой 75 мс, без потерь, без использования ограничителей пропускной
способности и максимального размера очереди
- После функции старта сети опишим запуск на хосте h2 сервера iPerf3, а на хосте h1 запуск с задержкой в 10 секунд клиента iPerf3 с экспортом результатов в JSON-файл, закомментируем строки, отвечающие за запуск CLI-интерфейса

![Добавление в скрипт библиотеки time, изменение в работе хостов, настройка каналов между коммутатором и хостами, добавление функции записи сервера iperf3ан хосте 2 и запуска клиента через 10 секунд на хосте 1, запись результатов в файл](image/12.PNG){ #fig:012 width=100% height=75% }

12. Запускаем на отработку скрипт (рис. [-@fig:013]):

![Запуск скрипта lab_iperf3.py на отработку](image/13.PNG){ #fig:013 width=100% height=75% }

13. Строим графики по получившимся резулттатм работы скрипта, а так же создание Makefile для проведения эксперимента (рис. [-@fig:014]):

![Постройка графиков и создание makefile](image/14.PNG){ #fig:014 width=100% height=40% }

14. Напишем скрипт для запуска эксперимента, постройки графиков и удаление файлов из каталога с результатами (рис. [-@fig:015]):

![Скрипт в Makefile](image/15.PNG){ #fig:015 width=100% height=75% }

15. Проверка корректности работы скрипта Makefile(рис. [-@fig:016]):

![Проверка корректности работы скрипта Makefile](image/16.PNG){ #fig:016 width=100% height=75% }

# Вывод

В ходе выполнения лабораторной работы познакомились с инструментом для измерения пропускной способности 
сети в режиме реального времени — iPerf3, а также получили навыки проведения воспроизводимого 
эксперимента по измерению пропускной способности моделируемой сети в среде Mininet

# Список литературы. Библиография

[1] Mininet: https://mininet.org/