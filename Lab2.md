# АНАЛИЗ ДАННЫХ В РАЗРАБОТКЕ ИГР
## Нужно больше золота
Отчет по лабораторной работе #2 выполнил:
- Григорьев Артем Евгеньевич
- РИ230914
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы


## Задание 1
### Описание игровой переменной здоровье в игре СПАСТИ РТФ: Выживание

#### Игровая роль:

- Представляет собой текущее здоровье игрока: Это значение напрямую отражает, сколько урона игрок может получить, прежде чем умрет. 
- Определяет выживаемость: Чем выше здоровье, тем дольше игрок может выдерживать атаки зомби.
- Влияет на урон: Зомби наносят урон, уменьшая переменную здоровья.
- Влияет на регенерацию: Если игрок не получал урона в течение определенного времени, его здоровье постепенно восстанавливается, предлагая пассивную механику восстановления.
- Усиляется вампиризмом: Прокачанный вампиризм позволяет игроку восстанавливать процент здоровья, когда он наносит урон зомби, создавая самоподдерживающийся цикл.

#### Условия изменения:

- Уменьшается:
  * При получении урона от атак зомби. 
  * Величина урона определяется силой зомби.
- Увеличивается:
  * Регенерация: Медленно увеличивается с течением времени, если игрок не получал урона в последнее время.
  * Вампиризм: Получает процент здоровья обратно за каждое очко урона, нанесенное зомби.
  * Повышение уровня: Игрок может прокачивать свое максимальное здоровье, увеличивая общее количество здоровья, которым он может обладать.

#### Допустимые значения:

- Минимум: 0 (это означает смерть; игра заканчивается).
- Максимум: Определяется максимальным уровнем здоровья игрока.
  * Базовое значение: 30.
  * Увеличивается с прокачкой: Каждый уровень максимального здоровья увеличивает максимальное значение на 10, позволяя игроку выдерживать больше урона по мере продвижения.

#### Примеры:

- Начальное здоровье: 30
- После получения 20 единиц урона: 10
- После 5 секунд без урона, с регенерацией: 11
- После удара по зомби с 1% вампиризма: 10,5-14
- После повышения максимального уровня здоровья на 3 уровня: 60

#### Ключевые моменты:

- Баланс: Механика переменной здоровья должна быть тщательно сбалансирована, чтобы обеспечить сложный, но справедливый игровой процесс. Скорость нанесения урона, регенерации и вампиризма должна создавать увлекательный и захватывающий бой, не будучи при этом чрезмерно наказуемым или тривиальным.
- Прогрессия: Система здоровья должна побуждать игроков повышать уровень и инвестировать в свои статы, вознаграждая их за стратегический геймплей.

Схема экономической модели здоровья в игре СПАСТИ РТФ: Выживание

![image](https://github.com/user-attachments/assets/1e5c9916-b252-43a6-a264-3f74c67a829a)

## Задание 2
### С помощью скрипта на языке Python заполнитm google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”.

#### Скрипт, который заполняет таблицу данными, описывающими здоровье в игре “СПАСТИ РТФ:Выживание”

```py
from googleapiclient.discovery import build
from google.oauth2.service_account import Credentials

# Your Google Sheet URL (e.g., "https://docs.google.com/spreadsheets/d/SHEET_ID/edit#gid=0")
SHEET_URL = "https://sheets.googleapis.com/v4/spreadsheets/1syVSAh8Z6N3TBxKxDxMuvgsFo4jwlBLr2ZgBslkR-qw/values/Лист1?key=AIzaSyC2DxxKqhCcKlgS_MPVH2MfIuHgs_BDtDM"

# Extract the Sheet ID from the URL
SHEET_ID = SHEET_URL.split("/")[5]

# Define the health variable data
health_data = [
  {
    "Variable": "Condition",
    "Change": "Change",
    "Amount": "Amount",
    "Change Conditions": [
      {
        "Condition": "Taking damage from zombie attacks",
        "Change": "Decreases",
        "Amount": "10"
      },
      {
        "Condition": "Taking damage from boss attacks",
        "Change": "Decreases",
        "Amount": "35"
      },
      {
        "Condition": "Regeneration (5 seconds without damage)",
        "Change": "Increases",
        "Amount": "1 health point every 3 seconds"
      },
      {
        "Condition": "Vampirism (zombie damage)",
        "Change": "Increases",
        "Amount": "1% of damage dealt"
      },
      {
        "Condition": "Leveling up maximum health",
        "Change": "Increases",
        "Amount": "+10 maximum health per level"
      }
    ]
  }
]

health_data_visualization = [
  ['',''],
  ['Example', 'Regen Health','Example', 'Vamp Health'],
  ['Start health', 30,'Start health', 30],
  ['After zombie attack', 20,'After zombie attack', 20],
  ['After regeneration', 21, 'After vampirism (default damage)', 21],
  ['','','',22],
  ['','','',23],
  ['',22,'',24],
  ['','','',25],
  ['','','',26],
  ['',23,'',27],
  ['','','',28],
  ['','','',29],
  ['',24,'',30],
  ['',''],
  ['',''],
  ['',25]
]


# Create the data for the spreadsheet
body = {
 'values': [
  # First row
  [health_data[0]['Variable'], health_data[0]['Change'], health_data[0]['Amount']],

  # Subsequent rows from 'Change Conditions'
  *[[condition['Condition'], condition['Change'], condition['Amount']]
    for condition in health_data[0]['Change Conditions']]
 ] + health_data_visualization[:]
}



# Create the service object
creds = Credentials.from_service_account_file(r'C:\Users\amnix\UrFU\Lab2\unitydatascience-438521-26281ac6b69a.json')
service = build('sheets', 'v4', credentials=creds)

# Update the spreadsheet
result = service.spreadsheets().values().update(
  spreadsheetId=SHEET_ID,
  range='F2:L50', # Target range: from F2 to H6 (inclusive)
  valueInputOption='RAW',
  body=body
).execute()
```

#### Таблица и диаграмма, описывающими здоровье в игре “СПАСТИ РТФ:Выживание”

https://docs.google.com/spreadsheets/d/1syVSAh8Z6N3TBxKxDxMuvgsFo4jwlBLr2ZgBslkR-qw/edit?hl=ru&gid=0#gid=0

![image](https://github.com/user-attachments/assets/3fc3d8a6-8a27-44bc-9a61-0afa8f9afa64)

![image](https://github.com/user-attachments/assets/40e3d478-f4c2-4fbf-874e-6363f6bb5714)


#### Изменение здоровья в игре имеет гибридный характер, сочетая в себе:

- Дискретные изменения: Здоровье восстанавливается фиксированными порциями по 1 HP в 3 секунды.
- Восстановление в зависимости от времени: Регенерация ограничена кулдауном, основанным на последнем получении игроком урона.
- Переменная скорость: Вампиризма зависит от скорости нанесения урона, которая может меняться в зависимости от оружия и хп зомби/количество зомби.

#### Недостатки в реализации здоровья:
  
- Регенерация здоровья хорошая механика, но слишком долгая. Бегать кругами от зомби ожидая восстановления очередной единицы здоровья в течении долгого времени довольно утомительно.
- Вампиризм слишком сильный - если удар по ноге или в тело хилит 0.5 - 1 хп, то удар в голову сразу все 4 (в качестве оружия рассматривается нож).
- В качестве эксплойта можно сказать, что с ножа ты бьешь не одного противника а сплешем в определенном радиусе. Сплеш, непосредственно, является нанесением урона, из-за чего вампиризм хилит огромные цифры здоровья. А собрать толпу зомби вместе и бегать вокруг практически не получая урона довольно простая задача, что тоже по сути эксплойт.

#### Предложение модификации здоровья:
  
- Регенерацию здоровья можно сделать раз в 1.5 секунды вместо трех, так хотя бы будет видно, что оно восстанавливается).
- Уменшить вампризим от удара по голове до 3.
- Уменшить урон сплеша с ножа на 50%.
  
## Задание 3
### Скрипт для изменения воспроизведение звуковых файлов в зависимости от значения очков здоровья

```С#

using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class HealthController : MonoBehaviour
{
    public AudioSource healthSource; 
    public AudioClip damageSound;
    public AudioClip deathSound;

    private int maxHealth;
    private int currentHealth;

    private List<int> hpList = new List<int>() { 30, 20, 10, 0, 30 }; // Примерные значения хп
    private int currentHpIndex = 0; 
    private float nextSoundTime = 0f; 

    void Start()
    {
        maxHealth = 30;
        currentHealth = maxHealth;
        healthSource = GetComponent<AudioSource>(); 
    }

    void Update()
    {
        if (currentHpIndex < hpList.Count)
        {
            currentHealth = hpList[currentHpIndex]; 

            if (Time.time >= nextSoundTime)
            {
                if (currentHealth == maxHealth)
                {
                    healthSource.PlayOneShot(fullHealthSound);
                }
                else if (currentHealth > 0)
                {
                    healthSource.PlayOneShot(damageSound);
                }
                else if (currentHealth == 0)
                {
                    healthSource.PlayOneShot(deathSound);
                }

                nextSoundTime = Time.time + 5f; 
                currentHpIndex++; 
            }
        }
    }
}
```

## Выводы



## Powered by

**Grigoriev**
