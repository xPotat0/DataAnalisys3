# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]



Отчет по лабораторной работе #3 выполнил:
- Исмагилов Денис Рустамович
- РИ210945
Отметка о выполнении заданий (заполняется студентом):


| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |


знак "*" - задание выполнено; знак "#" - задание не выполнено;


Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.




## Цель работы
Ознакомиться с технологией работы связки Python – Google Sheets - Unity


## Задание 1
 - Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity. 

## Ход работы:
- Создадим новый 3D проект в unity. После этого перейдём во вкладку Window - Package Manager. В появившемся окне нажмём на "+" - Add package from disk... - и выберем пакеты JSON, который мы заранее установили(Все вспомогательные файлы могут быть найдены в текущем репозитории). Установим пакеты из папок:
    - ml-agents-release_19 / com,unity.ml-agents / package.json
    - ml-agents-release_19 / com,unity.ml-agents.extensions / package.json
![1](https://user-images.githubusercontent.com/106258306/196661186-abd97982-c960-4827-9ef1-b756fbc86e57.png)
- После успешной установки можно закрыть окно управления пакетами. 
- Если они были установлены верно, то во вкладке Component добавится ML agent.
![2](https://user-images.githubusercontent.com/106258306/196662350-faccacde-4a5e-4f7b-b9f3-280b69bbe1d7.png)
- Теперь запустим командную строку Anaconda в режиме Администратора.
- В строке напишем следующую команду:
"conda create -n MLAgent python=3.10.6"
- Если всё было выполнено верно, то появится следующие сообщение
![3](https://user-images.githubusercontent.com/106258306/196669654-49eab959-7717-4889-be61-bc1b3aeaf44d.png)
- Напишем команду "y". Теперь в консоли покажутся следующие две команды: активация и дезактивация MLAgnet. Активируем MLAgent соответствующей командой
- conda activate MLagnet
![4](https://user-images.githubusercontent.com/106258306/196670864-da9b7098-1bf6-4f40-9d26-c1aa0c39aa1a.png)
- Теперь установим в python необходимые элементы. Для этого введём команду: "pip install mlagents==0.28.0"
- После установки будет выведено сообщение о успешно установленных пакетах.



## Вывод:

