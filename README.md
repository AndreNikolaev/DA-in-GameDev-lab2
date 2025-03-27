# Анализ данных в разработке игр
Отчет по лабораторной работе #2 выполнил(а):
- Николаев Андрей Ильич
- НМТ-233319

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


## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.

Я выбрал переменную HP (health points/очки здоровья)

Роль в игре: HP представляют собой физическое состояние персонажа и его способность выдерживать урон. Это, пожалуй, самая важная переменная для выживания, поскольку при достижении 0 HP персонаж обычно умирает.

Условия изменения: 
- Уменьшение: Получение урона: Основной источник уменьшения HP - это атаки врагов и другие физические повреждения.
- Восстановление: Здоровье может быть восполнено, собирая необходимые артефакты на поле или прокачивая перки, по мере прохождения.
  
Диапазон допустимых значений: В данной игре: СПАСТИ РТФ: Выживание диапазон значений здоровья ограничен, но может быть увеличен за счет того, что по мере прохождения волн, возможен выбор такого перка на вампиризм или увеличение максимального здоровья, либо покупку здоровья за монеты, получаемые за убийство врагов.

Схема экономической модели в игре "СПАСТИ РТФ: Выживание":

Здоровье в данной игре имеет ограниченный запас и уменьшается по мере нанесения ударов врагом по игроку и восстановливается при лечении.
![image](https://github.com/user-attachments/assets/839c1c4b-d356-4161-914f-273d7ac51a5e)




## Задание 2
### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”. Средствами google-sheets визуализируйте данные в google-таблице (постройте график / диаграмму и пр.) для наглядного представления выбранной игровой величины. Опишите характер изменения этой величины, опишите недостатки в реализации этой величины (например, в игре может произойти условие наступления эксплойта) и предложите до 3-х вариантов модификации условий работы с переменной, чтобы сделать игровой опыт лучше

 С помощью Python и Jupyter lab, я сделал таблицу, в которой показывается количество HP и его составляющие.
 ![image](https://github.com/user-attachments/assets/feb0a87d-9ae7-4089-bbd8-c2f3be5fa521)
![image](https://github.com/user-attachments/assets/eb898b6f-adf6-4ad1-bc6a-b69d617f9e04)

```py
function firstfunc() {
  import gspread
import numpy as np
gc = gspread.service_account('unitydatasciense-445509-fac94f705ae6.json')
sh = gc.open("WorkSheet2")
# Генерация случайных значений для здоровья (health) и урона (damage)
initial_health = np.random.randint(0, 30, 10)
damage_per_turn = np.random.randint(1, 15, 10)
time_points = list(range(0, 10))

index = 1 

while index <= len(time_points):
    current_health = initial_health[index - 1] - damage_per_turn[index - 1]
    status = "Живой" if current_health > 0 else "Мертвый"
    
    
    current_health = str(current_health).replace('.', ',')

   
    sh.sheet1.update(range_name=f'A{index}', values=[[index]])  
    sh.sheet1.update(range_name=f'B{index}', values=[[int(initial_health[index - 1])]])  # Начальное здоровье
    sh.sheet1.update(range_name=f'C{index}', values=[[int(current_health)]])  # Текущее здоровье
    sh.sheet1.update(range_name=f'D{index}', values=[[int(damage_per_turn[index - 1])]])  # Урон
    sh.sheet1.update(range_name=f'E{index}', values=[[status]])  

    print(current_health, status)

    index += 1
}
```
## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewEmptyCSharpScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }
    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://docs.google.com/spreadsheets/d/10Mj86hQt35x9YtkF8c-Su1DDxIGTMjjj5Vg1xl4MWxM/values/Лист1?key=AIzaSyDUPzM91xk_c6d8NNXn164e0rr2kB-MF5g");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```

## Выводы
Я научился взаимодействовать с Google Sheets, API Google таблиц, научился взаимодействовать с данными в API и Unity, разработал простейшую программу, связанную со здоровьем игрока.

## Powered by
Николаев Андрей Ильич
