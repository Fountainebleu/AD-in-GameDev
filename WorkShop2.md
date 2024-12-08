# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Гайдукевич Евгений Олегович
- РИ230948
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
Научиться передавать в Unity данные из Google Sheets с помощью Python. Проанализировать одну из игровых переменных в игре СПАСТИ РТФ: Выживание.

## Задание 1
### Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.
Ход работы:
- Монеты играют ключевую роль в игре, потому что это ресурс, за который игрок может купить патроны и оружие.
- Увеличение количества монет в инвентаре происходит после того, как игрок убил противника и подобрал на месте его смерти монеты. Также можно увеличить количества монет подбирая аирдропы. Трата денег происходит в магазине, где игрок может купить патроны и оружие. Есть также перки связанные с деньгами.
- Монеты могут быть в диапазоне от 0 (когда у игрока нет денег) до неограниченного количества.
- Экономическая Модель игры состоит из крана, инвентаря, преобразователя и трубы. Кран генерирует монеты за убийство врагов и спавнит аирдропы. Инвенрать хранит монет, которые собрал игрок. Преобразователь - магазин, где деньги конвертируются в полезные ресурсы (оружие, патроны). Труба забирает монеты игрока, которые он потратил в магазине.


## Задание 2
### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”. Средствами google-sheets визуализируйте данные в google-таблице (постройте график / диаграмму и пр.) для наглядного представления выбранной игровой величины. Опишите характер изменения этой величины, опишите недостатки в реализации этой величины (например, в игре может произойти условие наступления эксплойта) и предложите до 3-х вариантов модификации условий работы с переменной, чтобы сделать игровой опыт лучше.

- Скриипт на питоне
  
```py

import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascience-443817-9cf2fe80dccd.json')
sh = gc.open("UnitySheets")
price = np.random.randint(0, 2000, 11)
mon = list(range(1,11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        temtInf = ((price[i-1]-price[i-2])/price[i-2])*100
        temtInf = str(temtInf)
        temtInf = temtInf.replace('.',',')
        sh.sheet1.update(('A' + str(i)), [[str(i)]])
        sh.sheet1.update(('B' + str(i)), [[str(price[i-1])]])
        sh.sheet1.update(('C' + str(i)), [[str(temtInf)]])
        print(temtInf)

```
- Ссылка на таблицу https://docs.google.com/spreadsheets/d/1-vJK6hDf-80F_CZ5Hg67bnSOqQuGhA3SqHiq_2yItb8/edit?usp=sharing
- Характер изменения: количество монет резко изменяется от одного периода к другому. Есть как периоды значительного прироста (например, в 6 и 8 интервалах), так и периоды крупных потерь (5 и 11 интервалы).
- недостатки в реализации:
- 1) Отсутствие предсказуемости: непредсказуемые резкие скачки и падения затрудняют контроль игрока над своими ресурсами и могут вызывать фрустрацию.
  2) резмерные колебания: сильные положительные или отрицательные изменения (например, в 6 и 11 интервалах) нарушают баланс игры. Игрок может слишком быстро обогатиться (что снижает вызов игры) или потерять все ресурсы, что может демотивировать.
  3) Риск эксплойтов: резкий прирост монет в 6 интервале может быть связан с эксплуатацией определённых игровых механик. Например, если есть механика "фарма", игрок мог обнаружить способ быстро накапливать монеты без усилий.
- Предложения по модификации условий работы с переменной: установить максимальные значения прироста или убытка монет за единицу времени, добавить новые перки, связанные с фармом монет, увеличение стоимости товара за каждую покупку.


## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.

- Создаем пустой объект, добавляем ему Audio Source и пишем скрипт.

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip purchase;
    public AudioClip bigPurchase;
    public AudioClip earnCoins;
    public AudioClip earnManyCoins;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;
    
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] <= -50 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBigPurchase());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        if (dataSet["Mon_" + i.ToString()] < 0 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioPurchase());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        if (dataSet["Mon_" + i.ToString()] > 0 & dataSet["Mon_" + i.ToString()] < 50 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioEarnCoins());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        if (dataSet["Mon_" + i.ToString()] > 50 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioEarnManyCoins());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1-vJK6hDf-80F_CZ5Hg67bnSOqQuGhA3SqHiq_2yItb8/values/Лист1?key=AIzaSyBxzmnI3uN2FtmmAua9BUPbOyV5Vy4j8a8");
        yield return curResp.SendWebRequest();
        string rawResp = curResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioPurchase()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = purchase;
        selectAudio.Play();
        yield return new WaitForSeconds(2);
        statusStart = false;
        i++;
    }
    
    IEnumerator PlaySelectAudioBigPurchase()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = bigPurchase;
        selectAudio.Play();
        yield return new WaitForSeconds(2);
        statusStart = false;
        i++;
    }
    
    IEnumerator PlaySelectAudioEarnCoins()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = earnCoins;
        selectAudio.Play();
        yield return new WaitForSeconds(2);
        statusStart = false;
        i++;
    }
    
    IEnumerator PlaySelectAudioEarnManyCoins()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = earnManyCoins;
        selectAudio.Play();
        yield return new WaitForSeconds(2);
        statusStart = false;
        i++;
    }
}

```
- Скачиваем звуки и добавляем их к нашему скрипту


## Выводы

Научились передавать в Unity данные из Google Sheets с помощью Python и работать с ними. Также проанализировал игровую валюту из игры и предложил пути развития этой переменной.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
