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
- Напишем команду "y". Теперь в консоли покажутся следующие две команды: активация и дезактивация MLAgnet. 
Активируем MLAgent соответствующей командой: conda activate MLagent

![4](https://user-images.githubusercontent.com/106258306/196670864-da9b7098-1bf6-4f40-9d26-c1aa0c39aa1a.png)

- Теперь установим в python необходимые элементы. 
- Введём команду: "pip install mlagents==0.28.0"
- Введём команду: "pip install "pip install torch~=1.7.1 -f https://download.pytorch.org/whl/torch_stable.html"
- После установки будет выведено сообщение о успешно установленных пакетах.
- Перейдём в unity. Скопируем путь местонахождения проекта. После этого выполним команду: "cd C:\Users\111\MLAtest"
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
mlagents-learn rollerball_config.yaml --run-id=RollerBall --force

- После выполнения мы увидем в консоли следующие

![12](https://user-images.githubusercontent.com/106258306/197208392-951dbe13-fda8-4435-b3cf-1e9e3f9d9fa1.png)

- Теперь запустим сцену в Unity. Шар начнёт быстро двигаться, а когда достигнет цели, то всё начнётся заново, но местоположение куба будет другим.
- Подождём, пока не пройдёт несколько сотен итераций. Остановим сцену. В консоли будет выведенно сообщение о том, что результаты были скопированы.
- Создадим в папке Assets новый folder и перетщим туда наш объект TargetArea. Поставим на сцену ещё два объекта TargetArea.

![13](https://user-images.githubusercontent.com/106258306/197209399-e22f86f0-72fa-4f0d-a4b5-7ba3dda391e4.png)

- Теперь запустим программу mlagents-learn rollerball_config.yaml --run-id=RollerBall --resume
- Теперь, когда зон стало больше, стало выполняться большие кол-во операций.
- Увеличим кол-во зон до 9. Возобновим запись и запустим сцену.

![14](https://user-images.githubusercontent.com/106258306/197211120-ea30f792-c785-469b-9826-5524472ebc2b.png)

- Количество операций снова увеличилось. Увеличим кол-во зон до 27. Возобновим запись и запустим сцену.

![15](https://user-images.githubusercontent.com/106258306/197211504-d06f5752-2ce8-4e12-bd7d-ead597430c92.png)

-Остановим сцену.

## Вывод:

