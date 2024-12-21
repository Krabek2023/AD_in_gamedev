# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Степанов Алексей Алексеевич
- РИ-230935
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
Научиться передавать в Unity данные из Google Sheets с помощью Python. Также научиться анализировать игровые переменные, описывать их изменение и поведение

## Задание 1
### Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.
Ход работы:
- Здоровье(hp), ее роль в игре отображать насколько игрок близок к поражению,чем ближе к 0 тем ближе к концу игры.
  Изменение переменной:
  - если игрок получил урон, то величина уменьшается
  - если игрок во время прокачки выбрал соотвествующие навыки, то они восполняют здоровье
  Диапазон допустимых значений переменной, от 0 и до указанного лимита.
- Схема экономической модели ресурса:
![схема](https://github.com/user-attachments/assets/aa549105-cc35-4176-a27d-f9463dfe6398)


## Задание 2
### ### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”. Средствами google-sheets визуализируйте данные в google-таблице (постройте график / диаграмму и пр.) для наглядного представления выбранной игровой величины. Опишите характер изменения этой величины, опишите недостатки в реализации этой величины (например, в игре может произойти условие наступления эксплойта) и предложите до 3-х вариантов модификации условий работы с переменной, чтобы сделать игровой опыт лучше.
Ход работы:
- Для начала нужно определить как меняется выбранная мной игровая переменная:
Здоровье игрока меняется в зависимсоти от получения им урона, это значит, что изменение этой переменной зависит от игрока и противников. Поскольку есть множество вариантов изменения этого параметра,я взял ситуацию когда игрок просто получает урон с некоторым промежутком времени.
- В выбранной ситуации график изменения переменной выглядит как прямая Hp= t*(-10) + 30 (В иных ситуациях график может иметь более сложное поведене):
![image](https://github.com/user-attachments/assets/a8a5ddf8-3a70-4f40-a34e-af0e9d3c0478)
Ссылка на [таблицу](https://docs.google.com/spreadsheets/d/16Y4BV7u3gdxiQMhBGlvTg5j-Drufmo0SzpH0QQx3q4w/edit?usp=sharing)
- Видно, что величина изменяется от 30 (лимит) до 0 (конец игры), уменьшение величины происходит когда игрок получает урон (в данном примере -10), увеличение происходит когда игрок использует "вампиризм", заканчивает волну. 
- В игре СПАСТИ РТФ взаимодействия с хп можно дополнить функционал этой переменной следющими идеями:
    - Добавить предмет который увеличивает текущее здоровье двуями способами, либо моментально, либо переодически
    - Возможность расходовать текущее здоровье в замен на какой ресурс (например патроны), возможно придется добавлять новые предметы чтобы не ломать логику              старого предмета с этим нововведением
    - Возможность прибавлять предел здоровья (вне прокачки) на определенный промежуток времени, например, для битвы с боссом


## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.
Ход работы:
- Для реализации данного задания я выделил правильно по которым описывается динамика изменения здоровья:
    - Если хп около лимита (в моем случае hp >= 30), то воспроизводится звук "хороший"
    - Если хп около среднего значения (в моем случае 30 > hp >= 20), то воспроизводится звук "средний"
    - Если хп около минимума (в моем случае hp <= 0), то воспроизводится звук "плохой"
- Для отображения этой динамики в Unity я создал компонент с таким скриптом:
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] >= 30 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
            Debug.Log("Проигрывается звук 'Horosho'");
        }
        if (dataSet["Mon_" + i.ToString()] < 30 & dataSet["Mon_" + i.ToString()] >= 20 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
            Debug.Log("Проигрывается звук 'Sredne'");
        }
        if (dataSet["Mon_" + i.ToString()] < 20 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
            Debug.Log("Проигрывается звук 'Ploho'");
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1-TBYwyDcrPz_f4pHD6Jv8I1gbH_UNFmD-HplJS98Nkk/values/Лист1?key=AIzaSyBFrX6sxQ8GY5gv57Az0AXnpU1rQKbjwJU");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[1]));
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
- Скрипт работает и в Unity изображена динамика изменения здоровья
![image](https://github.com/user-attachments/assets/efc763cb-6000-47a1-adb3-edb585248df3)
## Выводы

В ходе работы я научился ананлизировать работу экономической переменной здоровье.
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
