# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]



Отчет по лабораторной работе #3 выполнил:
- Исмагилов Денис Рустамович
- РИ210945
Отметка о выполнении заданий (заполняется студентом):


| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
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
"
```python
conda create -n MLAgent python=3.10.6
```
- Если всё было выполнено верно, то появится следующие сообщение
![3](https://user-images.githubusercontent.com/106258306/196669654-49eab959-7717-4889-be61-bc1b3aeaf44d.png)
- Напишем команду "y". Теперь в консоли покажутся следующие две команды: активация и дезактивация MLAgnet. 
Активируем MLAgent соответствующей командой:
```python
conda activate MLagent
```

![4](https://user-images.githubusercontent.com/106258306/196670864-da9b7098-1bf6-4f40-9d26-c1aa0c39aa1a.png)

- Теперь установим в python необходимые элементы. 
- Введём команду:
```python
pip install mlagents==0.28.0
```
- Введём команду:
```python
pip install "pip install torch~=1.7.1 -f https://download.pytorch.org/whl/torch_stable.html
```
- После установки будет выведено сообщение о успешно установленных пакетах.
- Перейдём в unity. Скопируем путь местонахождения проекта. После этого выполним команду:
```python
"cd C:\Users\111\MLAtest"
```
- Создадим в сцене панель и куб. Назовём их соответственно Floor и Target. Для создания объекта в unity нужно нажать ПКМ в обозреватель(он находится слева от сцены), выбрать пункт Create 3D и выбрать нужный объект.
- Зададим кубу координаты 3, 0.5, 3. Чтобы это сделать выделим куб и справа от сцены, в меню настройки, введём параметры position.

![5](https://user-images.githubusercontent.com/106258306/196682175-b347fdb3-f517-45c5-8447-99c09fd026c7.png)

- Создадим сферу и назовём её RollerAgent. Зададим её вторую координату 0,5.
- Добавим в её новый компонент, выделив сферу и нажав на панели справа кнопку Add component. Компонент называется rigitbody.
- После проведённых манипуляций сцена будет выглядеть следющим образом

![6](https://user-images.githubusercontent.com/106258306/196683730-2d89caeb-6da8-433a-9a59-7380e56f6f03.png)

- Создадим Folder и назовём его material. Для этого на панели, снизу от сцены, нажмём ПКМ - create - Folder.
- Перейдем в папку и создадим в ней несколько новых материалов. Для этого нажмём ПКМ - Create - Material

![7](https://user-images.githubusercontent.com/106258306/196684272-9592fabe-e7b6-48d4-8fee-715c7412d495.png)

- Первый материал назовём RollerAgent. Зададим ему красный цвет.
- Второй материал назовём Target. Зададим ему зелёный цвет.
- Задать цвет можно выбрав материал и нажав на цветную полосу справа от сцены.
- Применим цвета к объектам на сцене. "Перетащим" материал на объект. Имя материала соответствует имени объекта

![8](https://user-images.githubusercontent.com/106258306/196685398-bc792f67-29b9-4eb7-9f3d-e18b17c6bc33.png)

- Создадим пустой объект и назовём его TargetArea. "Перетащим" объекты Floor, Target, RollerAgent в пустой объект.

![9](https://user-images.githubusercontent.com/106258306/196686505-93a99624-7986-41c0-a5fd-03a332ec4a5d.png)

- Создадим новый Folder и назовём его Scripts.
- Выберем RollerAgent и добавим в него новый компанент. Введем "RollerAgent" и в появившемся списке выберем Script.

![10](https://user-images.githubusercontent.com/106258306/196688335-a0c0adbd-8a05-437e-add6-e1978e3a270e.png)

##### Откроём скрипт. Напишем код, который будет
- Задавать направление движения шара к цели
- Задавать случайную координату цели

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }
        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionsBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionsBuffers.ContinuousActions[0];
        controlSignal.z = actionsBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if(this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}

```

- У шара, в компоненте скрипт, появился экземпляр Target. Перетащим туда наш куб.

![11](https://user-images.githubusercontent.com/106258306/196693581-5b690d16-e626-4ac1-a92f-da38a52be834.png)

- Добавим шару новый компонент Desicion Requester. Изменим параметр Decision perioud на 10
- Добавим шару новый компонент Behaviour Parameters. Изменим параметр Behaviour name на "RollerBall", Space size на 8, Continuous action на 2
- Перейдём в папку с проектом unity. Создадим файл rollerball_config.yaml. Это можно сделать создав текстовый файл и переименовав его. Откроем файл и введём следующие

```yaml
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

- Перейдём в командную строку и введём команду
```python
mlagents-learn rollerball_config.yaml --run-id=RollerBall --force
```

- После выполнения мы увидем в консоли следующие

![12](https://user-images.githubusercontent.com/106258306/197208392-951dbe13-fda8-4435-b3cf-1e9e3f9d9fa1.png)

- Теперь запустим сцену в Unity. Шар начнёт быстро двигаться, а когда достигнет цели, то всё начнётся заново, но местоположение куба будет другим.
- Подождём, пока не пройдёт несколько сотен итераций. Остановим сцену. В консоли будет выведенно сообщение о том, что результаты были скопированы.
- Создадим в папке Assets новый folder и перетщим туда наш объект TargetArea. Поставим на сцену ещё два объекта TargetArea.

![13](https://user-images.githubusercontent.com/106258306/197209399-e22f86f0-72fa-4f0d-a4b5-7ba3dda391e4.png)

- Теперь запустим программу
```python
mlagents-learn rollerball_config.yaml --run-id=RollerBall --resume
```
- Теперь, когда зон стало больше, стало выполняться большие кол-во операций.
- Увеличим кол-во зон до 9. Возобновим запись и запустим сцену.

![14](https://user-images.githubusercontent.com/106258306/197211120-ea30f792-c785-469b-9826-5524472ebc2b.png)

- Количество операций снова увеличилось. Увеличим кол-во зон до 27. Возобновим запись и запустим сцену.

![15](https://user-images.githubusercontent.com/106258306/197211504-d06f5752-2ce8-4e12-bd7d-ead597430c92.png)

- Остановим сцену. Зайдём в папку с проектом, перейдём в папку result и возьмём оттуда файл с названием RollerBall.onnx. Скопируем его.

![16](https://user-images.githubusercontent.com/106258306/197223245-4726e636-3a25-4a87-9f4d-dc36239c7791.png)

- Вставим файл в папку Assets нашего проекта. Перейдём в Unity. Уберём все зоны, кроме одной(Удалить или выбрать ненужную зону и в правом меню убрать галочку с и инспектора).

![17](https://user-images.githubusercontent.com/106258306/197224140-57a54861-b270-40fa-9d43-f51d41ff2b83.png)

- Выберем в оставшейся зоне RollerAgent. В компоненте Behaviour Parameters установим настройку Model. Перетащим наш файл RollerBall.onnx, который мы скопировали ранее в папку assets, и перетащим его в Model.
-  Установим настройку Inference Device = CPU.
-  Установим настройку Behavior Type = Inference Only.

![18](https://user-images.githubusercontent.com/106258306/197225215-0f513d76-8809-41d6-9449-18bc9e75384b.png)

- Запустим проект. Теперь шар катается в зоне пола(Floor) и меняет траекторию в зависимости от места, где оказался куб(Target).
#### Мы реализовали машинное обучение

## Задание 2
- Описать назначение строк в файле roller_agent.yaml
### Ход работы:
### Рассмотрим параметры в файле
|Название|Описание|
|-----|-----|
|trainer_type|Задаёт тип используемого тренера. По умолчанию "ppo". Возможны варианты "ppo", "sac", "poca"|
|hyperparameters->batch_size|Количество опытов в каждой итерации градиентного спуска. Это всегда должно быть в несколько раз меньше, чемbuffer_size . Если вы используете непрерывные действия, это значение должно быть большим (порядка 1000 с). Если вы используете только дискретные действия, это значение должно быть меньше (порядка 10 с). *Типовой диапазон: (непрерывный - PPO): 512- 5120; (непрерывный - SAC): 128- 1024; (Дискретный, PPO и SAC): 32- 512*|
|hyperparameters->buffer_size|*(по умолчанию = 10240для PPO и 50000для SAC)*PPO: количество опытов, которые необходимо собрать перед обновлением модели политики. Соответствует тому, сколько опыта должно быть собрано, прежде чем мы будем изучать или обновлять модель. Это должно быть в несколько раз больше, чемbatch_size . Обычно большее buffer_sizeзначение соответствует более стабильным обновлениям обучения. SAC: максимальный размер буфера опыта — примерно в тысячи раз больше, чем ваши эпизоды, чтобы SAC мог учиться как на старом, так и на новом опыте. *Типовой диапазон: PPO: 2048- 409600; САК: 50000-1000000*|
|hyperparameters->learning_rate|*(по умолчанию = 3e-4)* Начальная скорость обучения для градиентного спуска. Соответствует силе каждого шага обновления градиентного спуска. Обычно это значение следует уменьшать, если обучение нестабильно, а вознаграждение не увеличивается постоянно. *Типовой диапазон: 1e-5-1e-3*|
|hyperparameters->beta|*(по умолчанию = 5.0e-3)* Сила регуляризации энтропии, которая делает политику «более случайной». Это гарантирует, что агенты должным образом исследуют пространство действия во время обучения. Увеличение этого параметра обеспечит выполнение большего количества случайных действий. Это должно быть скорректировано таким образом, чтобы энтропия (измеряемая с помощью TensorBoard) медленно уменьшалась вместе с увеличением вознаграждения. Если энтропия падает слишком быстро, увеличьте бета. Если энтропия падает слишком медленно, уменьшите beta. *Типовой диапазон: 1e-4-1e-2*|
|hyperparameters->epsilon|*(по умолчанию = 0.2)* Влияет на скорость изменения политики во время обучения. Соответствует допустимому порогу расхождения между старой и новой политикой при обновлении градиентного спуска. Установка небольшого значения этого параметра приведет к более стабильным обновлениям, но также замедлит процесс обучения. *Типовой диапазон: 0.1-0.3*|
|hyperparameters->lambd|*(по умолчанию = 0.95)* Параметр регуляризации (лямбда), используемый при расчете обобщенной оценки преимущества ( GAE ). Это можно рассматривать как то, насколько агент полагается на свою текущую оценку стоимости при вычислении обновленной оценки стоимости. Низкие значения соответствуют большему полаганию на текущую оценку ценности (что может быть высоким смещением), а высокие значения соответствуют большему полаганию на фактические вознаграждения, полученные в среде (что может быть высокой дисперсией). Параметр обеспечивает компромисс между ними, и правильное значение может привести к более стабильному процессу обучения. *Типовой диапазон: 0.9-0.95*|
|hyperparameters->num_epoch|*(по умолчанию = 3)* Количество проходов через буфер опыта при выполнении оптимизации градиентного спуска. Чем больше размер партии, тем больше это допустимо. Уменьшение этого параметра обеспечит более стабильные обновления за счет более медленного обучения. *Типовой диапазон: 3-10*|
|hyperparameters->learning_rate_schedule|*(по умолчанию = linearдля PPO и constantдля SAC)* Определяет, как скорость обучения изменяется с течением времени. Для PPO мы рекомендуем снижать скорость обучения до значения max_steps, чтобы обучение сходилось более стабильно. Однако в некоторых случаях (например, обучение в течение неизвестного времени) эту функцию можно отключить. Для SAC мы рекомендуем поддерживать скорость обучения постоянной, чтобы агент мог продолжать обучение до тех пор, пока его функция Q не сойдется естественным образом. linearскорость обучения уменьшается линейно, достигая 0 на max_steps, сохраняя при constantэтом скорость обучения постоянной для всего тренировочного прогона.|
|network_settings->normalize|*(по умолчанию = false)* Применяется ли нормализация к входным данным векторных наблюдений. Эта нормализация основана на скользящем среднем и дисперсии векторного наблюдения. Нормализация может быть полезна в случаях со сложными задачами непрерывного управления, но может быть вредна для более простых задач дискретного управления.|
|network_settings->hidden_units|*(по умолчанию = 128)* Количество единиц в скрытых слоях нейронной сети. Соответствуют количеству единиц в каждом полносвязном слое нейронной сети. Для простых задач, где правильное действие представляет собой простую комбинацию входных данных наблюдения, это значение должно быть небольшим. Для задач, где действие представляет собой очень сложное взаимодействие между переменными наблюдения, это значение должно быть больше. *Типовой диапазон: 32-512*|
|network_settings->num_layers|*(по умолчанию = 2)* Количество скрытых слоев в нейронной сети. Соответствует количеству скрытых слоев после ввода наблюдения или после кодирования CNN визуального наблюдения. Для простых задач меньше слоев, скорее всего, будут обучать быстрее и эффективнее. Для более сложных задач управления может потребоваться больше слоев. *Типовой диапазон: 1-3*|
|reward_signals|Позволяет задавать настройки как для внешних (т. е. основанных на среде), так и для внутренних сигналов вознаграждения (например, любопытство и GAIL). Каждый сигнал вознаграждения должен определять как минимум два параметра strength и gamma, в дополнение к любым гиперпараметрам, специфичным для класса. Обратите внимание: чтобы удалить сигнал вознаграждения, вы должны полностью удалить его запись из файла reward_signals. По крайней мере, один сигнал вознаграждения должен оставаться определенным в любое время. Предоставьте следующие конфигурации для разработки сигнала вознаграждения для вашего тренировочного прогона. |
|reward_signals->extrinsic->gamma|*(по умолчанию = 0.99)* Фактор скидки для будущих вознаграждений, поступающих из окружающей среды. Это можно рассматривать как то, как далеко в будущем агент должен заботиться о возможных вознаграждениях. В ситуациях, когда агент должен действовать в настоящем, чтобы подготовиться к вознаграждению в отдаленном будущем, это значение должно быть большим. В случаях, когда вознаграждение является более немедленным, оно может быть меньше. Должен быть строго меньше 1. *Типичный диапазон: 0.8-0.995*|
|reward_signals->extrinsic->strength|*(по умолчанию = 1.0)* Коэффициент, на который умножается вознаграждение, данное средой. Типичные диапазоны будут варьироваться в зависимости от сигнала вознаграждения. *Типичный диапазон:1.00*|
|max_steps|(по умолчанию = 500000) Общее количество шагов (т. е. собранных наблюдений и предпринятых действий), которые необходимо выполнить в среде (или во всех средах при параллельном использовании нескольких) перед завершением процесса обучения. Если в вашей среде есть несколько агентов с одинаковым именем поведения, все шаги, предпринятые этими агентами, будут max_stepsучитываться в одном и том же счетчике. *Типовой диапазон: 5e5-1e7*|
|time_horizon|*(по умолчанию = 64*) Сколько шагов опыта необходимо собрать для каждого агента, прежде чем добавить его в буфер опыта. Когда этот предел достигается до конца эпизода, оценка значения используется для прогнозирования общего ожидаемого вознаграждения из текущего состояния агента. Таким образом, этот параметр является компромиссом между менее предвзятой, но более высокой оценкой дисперсии (длинный временной горизонт) и более предвзятой, но менее разнообразной оценкой (короткий временной горизонт). В тех случаях, когда в эпизоде есть частые награды или эпизоды непомерно велики, более идеальным может быть меньшее количество. Это число должно быть достаточно большим, чтобы охватить все важные действия в последовательности действий агента. *Типовой диапазон: 32-2048*|
|summary_freq|*(по умолчанию = 50000)* Количество опытов, которое необходимо собрать перед созданием и отображением статистики обучения. Это определяет детализацию графиков в Tensorboard.|

### Рассмотрим компоненты, которые мы добавили для нашей сферы
|Название Компонента|Описание|
|-----|-----|
|Desicion Requester|Компонент DecisionRequester автоматически запрашивает решения для экземпляра агента через регулярные промежутки времени.Компонент DecisionRequester предоставляет удобный и гибкий способ запуска процесса принятия решения агентом. Без DecisionRequester реализация вашего агента должна вручную вызывать функцию RequestDecision(). У компонента есть поле "DesicionPeriod", которое означает через какое количество шагов агент будет запрашивать решение.|
|Behaviors Parameters|Компонент для настройки поведения экземпляра агента и свойств мозга. Во время выполнения этот компонент генерирует объекты политики агента в соответствии с настройками, указанными в редакторе.|

## Задание 3
- Доработать сцену и обучить ML-Agent таким образом, чтобы шар перемещался между двумя кубами разного цвета. Кубы должны, как и в первом задании, случайно изменять координаты на плоскости. 
### Ход работы:
- Переименуем первый, зелёный куб. Назовём его "Target_1"
- Создадим новый куб и поместим его над плоскоостью Floor. Зададим ему координаты 3.0, 0.5, -3.0 и имя "Target_2".

![21](https://user-images.githubusercontent.com/106258306/197496639-bffcd6eb-db1d-4694-b81e-9c3254298b29.png)

- Создадим новый материал синего цвета и "перетащим" его на новый куб.
- Релузьтатом должен стать следующий вид сцены:

![19](https://user-images.githubusercontent.com/106258306/197496792-813dcb0b-b840-4e11-af16-abae6b2d79ba.png)

### Модернизируем наш скрипт у шара.
- Создадим новый объект с названием Target_2.
- Добавим случайную генерацию позиции Target_2
- Добавим цикл While, который будет отдалять кубы друг от друга, если они находятся слишком близко.
- Добавим в наблюдатель сенсора текущую позицию Target_2.
- Изменим условия получения награды на точку между Target_1 и Target_2

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target_1;
    public Transform Target_2;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target_1.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
        Target_2.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
        while(Vector3.Distance(Target_1.localPosition, Target_2.localPosition) <= 2)
        {
            Target_1.localPosition = new Vector3(Target_1.position.x+0.5f, 0.5f, Target_1.position.z+0.5f);
            Target_2.localPosition = new Vector3(Target_2.position.x-0.5f, 0.5f, Target_2.position.z-0.5f);
        }
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target_1.localPosition);
        sensor.AddObservation(Target_2.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget_1 = Vector3.Distance(this.transform.localPosition, (Target_1.localPosition - Target_2.localPosition)/2);

        if(distanceToTarget_1 < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```

## Вывод:
Что есть игровой баланс? Как его достичь? Прежде всего попытаемся дать определение, что такое игровой баланс и как его могут воспринимать разные игроки.
Стоит начать с того, что **игрового баланса нет**.Но это не значит, что он не существует. Нельзя дать точное определение, которое было бы применимо ко всем ситуациям, но есть гипотезы, мифы, эмпирические методы и способы, как он может быть обнаружен. Посмтрим на схему:

![image](https://user-images.githubusercontent.com/106258306/197353726-ead42da8-1ede-4ef3-872a-3708bff5d67d.png)

Баланс - это не набор цифр, которые каким-то образом должны уравняться. Баланс есть попытка сделать игру более цельной и играбельной. Для одиночных игр баланс - это вызовы во времени, для многопользовательских игр - преимущества с той или иной стороны, для стратегических игр - не является ли одна стратегия во всём лучше других, можно ли победить её. Игровой баланс - это, прежде всего, достижение верхней точки узкого коридора, **фана**. Если игра будет слишком просто, она будет скучной, если сложной - приносить боль. 
Баланс имеет несколько видов:
- Сила - Отношение объектов друг к другу с позиции силы.
- Время - Время возникновения игровых событий, Время развития игровых сущностей
- Пространство - Равные возможности передвижения для врагов
- Экономика - Определение ценности объектов в игре
- Сложность - Возможность ощущать попеременно преимущество и отставание

Для того, чтобы грамотно настраивать баланс - необходимо постоянно тестировать, собирать информацию, изменять и снова тестировать игру. Это всё занимает очень много времени. И тут нам может помочь машинное обучение. Оно способно быстро тестировать изменения и выводить статистику по тому, насколько изменилась ситуация в игре. Это сэкономит разработчикам большое количество время. 
