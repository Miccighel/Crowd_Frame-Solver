# Specifiche di massima del servizio REST di interazione con il solver v. 1.1

La docker image è disponibile sul docker registry `opthub.uniud.it` con nome `item-assignment-rest:latest`. Dev'essere scaricata con `docker pull` e dev’essere creato un container. Una volta fatto, il container fornisce il suo servizio attraverso il protocollo `http` che è esposto porta interna `18080`. 

Tutti gli endpoint restituiscono contenuti di tipo `application/json` e accettano solo contenuti dello stesso tipo.

## Endpoint `/`

L'endpoint principale è accessibile solo attraverso il metodo http `GET` e restituisce uno stato del sistema che comprende la lista dei runners disponibili e i task eseguiti e in esecuzione.

Un esempio di output restituito è il seguente che indica che sono disponibili i seguenti 7 runner.

```json
{
  "neighborhoods":[],
  "runners":["/runner/BSA",
             "/runner/IA_ChangeHillClimbing",
             "/runner/IA_ChangeSimulatedAnnealing",
             "/runner/IA_ChangeSteepestDescent",
             "/runner/IA_SwapSteepestDescent",
             "/runner/SSA",
             "/runner/SW_HC"],
  "started":"2021-03-30T20:22:13Z",
  "tasks":null,
  "tester_id":"IA_REST",
  "version":"1.1",
  "workers":{ "number":11, "solution_time":0, "tasks_run":0 }
}
```

## Endpoint `/runner/<runner_name>`

L'endpoint consente di interagire con il runner il cui nome è specificato di seguito. In particolare, esso risponde ai seguenti metodi http:

#### `GET`

Restituisce una lista di parameteri del runner. Solo per documentazione. 

#### `POST`

Invia una richiesta di soluzione al runner. Richiede che l’input sia passato in un file json nel seguente formato:

```json
{
  "id": "toy",
  "min_item_repetitions": 3,
  "min_item_quality_level": 120,
  "categories": [
    {
      "id": "relevance",
      "levels": [
        "not_relevant",
        "marginally_relevant",
        "relevant",
        "highly_relevant"
      ],
      "worker_assignments": 1
    },
    {
      "id": "political_orientation",
      "levels": [
        "democratic",
        "republican"
      ],
      "worker_assignments": 2
    }
  ],
  "properties": [
    {
      "id": "gender",
      "levels": [
        "male",
        "female"
      ],
      "item_assignments": 1
    },
    {
      "id": "political_orientation",
      "levels": [
        "democratic",
        "republican"
      ],
      "item_assignments": 1
    }
  ],
  "items": [
    {
      "id": "I0",
      "categories": [
        {
          "id": "relevance",
          "level": "not_relevant"
        },
        {
          "id": "political_orientation",
          "level": "democratic"
        }
      ]
    },
    {
      "id": "I1",
      "categories": [
        {
          "id": "relevance",
          "level": "not_relevant"
        },
        {
          "id": "political_orientation",
          "level": "republican"
        }
      ]
    },
    {
      "id": "I2",
      "categories": [
        {
          "id": "relevance",
          "level": "marginally_relevant"
        },
        {
          "id": "political_orientation",
          "level": "democratic"
        }
      ]
    },
    {
      "id": "I3",
      "categories": [
        {
          "id": "relevance",
          "level": "marginally_relevant"
        },
        {
          "id": "political_orientation",
          "level": "republican"
        }
      ]
    },
    {
      "id": "I4",
      "categories": [
        {
          "id": "relevance",
          "level": "relevant"
        },
        {
          "id": "political_orientation",
          "level": "democratic"
        }
      ]
    },
    {
      "id": "I5",
      "categories": [
        {
          "id": "relevance",
          "level": "relevant"
        },
        {
          "id": "political_orientation",
          "level": "republican"
        }
      ]
    },
    {
      "id": "I6",
      "categories": [
        {
          "id": "relevance",
          "level": "highly_relevant"
        },
        {
          "id": "political_orientation",
          "level": "democratic"
        }
      ]
    },
    {
      "id": "I7",
      "categories": [
        {
          "id": "relevance",
          "level": "highly_relevant"
        },
        {
          "id": "political_orientation",
          "level": "republican"
        }
      ]
    }
  ],
  "workers": [
    {
      "id": "W0",
      "expertise": 56,
      "properties": [
        {
          "id": "gender",
          "level": "male"
        },
        {
          "id": "political_orientation",
          "level": "democratic"
        }
      ]
    },
    {
      "id": "W1",
      "expertise": 9,
      "properties": [
        {
          "id": "gender",
          "level": "male"
        },
        {
          "id": "political_orientation",
          "level": "republican"
        }
      ]
    },
    {
      "id": "W2",
      "expertise": 84,
      "properties": [
        {
          "id": "gender",
          "level": "male"
        },
        {
          "id": "political_orientation",
          "level": "republican"
        }
      ]
    },
    {
      "id": "W3",
      "expertise": 45,
      "properties": [
        {
          "id": "gender",
          "level": "female"
        },
        {
          "id": "political_orientation",
          "level": "democratic"
        }
      ]
    },
    {
      "id": "W4",
      "expertise": 82,
      "properties": [
        {
          "id": "gender",
          "level": "female"
        },
        {
          "id": "political_orientation",
          "level": "democratic"
        }
      ]
    },
    {
      "id": "W5",
      "expertise": 53,
      "properties": [
        {
          "id": "gender",
          "level": "female"
        },
        {
          "id": "political_orientation",
          "level": "republican"
        }
      ]
    },
      {
      "id": "W6",
      "expertise": 20,
      "properties": [
        {
          "id": "gender",
          "level": "female"
        },
        {
          "id": "political_orientation",
          "level": "republican"
        }
      ]
      }
  ]
}
```

In cui i tre campi sono conformi ai formati già concordati.

La risposta, in caso non vi siano errori, è un'oggetto json simile al seguente:

```json
{
  "submitted":"2021-03-30T20:51:34Z",
  "task_id":"8488109602186256044",
  "url":"/running/8488109602186256044"
}
```

contenente la `url` alla quale possiamo fare riferimento per conoscere lo stato della risoluzione.

## Endpoint `/running/<run_id>`

Gestisce l'esecuzione del runner il cui `run_id` è passato di seguito. In particolare, esso risponde ai seguenti metodi http:

#### `GET`

L'endpoint consente di interagire con il runner in esecuzione, il cui `run_id` è quello restituito dalla richiesta di esecuzione (`POST` all'endpoint precedente).

La risposta è un json simile al seguente:

```json
{
  "cost": {
    "components": {
      "IA_CategoryWorkerAssignments": {
        "cost": 0,
        "hard": true,
        "weight": 1000
      },
      "IA_MinimumItemQualityLevel": {
        "cost": 209,
        "hard": false,
        "weight": 1
      },
      "IA_PropertyItemAssignments": {
        "cost": 0,
        "hard": false,
        "weight": 1
      }
    },
    "objective": 209,
    "total": 209,
    "violations": 0
  },
  "finished": false,
  "instance_url": "/instance/8488109602186256044",
  "runner": "BSA",
  "running": true,
  "started": "2021-03-30T20:51:34Z",
  "submitted": "2021-03-30T20:51:34Z",
  "task_id": "8488109602186256044"
}
```

contenente le indicazioni delle componenti di costo della miglior soluzione fino a questo momento, nel caso in cui il runner sia ancora in esecuzione `finished = false`, altrimenti della soluzione trovata se `finished = true`.

#### `DELETE`

Se ancora in esecuzione, richiede la cancellazione dell'esecuzione del runner (e di conseguenza di tutti i suoi risultati intermedi).

## Endpoint `/solution/<run_id>`

L'endpoint consente di accedere alla soluzione relativa al `run_id` passato come parametro. In particolare, esso risponde ai seguenti metodi http:

#### `GET`

Restituisce il json della soluzione. Essa avrà un riferimento all'istanza della quale è soluzione nel formato concordato, ad esempio:

```json
{
    "cost": {
        "components": {
            "IA_CategoryWorkerAssignments": {
                "cost": 0,
                "hard": true,
                "weight": 1000
            },
            "IA_MinimumItemQualityLevel": {
                "cost": 186,
                "hard": false,
                "weight": 1
            },
            "IA_PropertyItemAssignments": {
                "cost": 0,
                "hard": false,
                "weight": 1
            }
        },
        "objective": 186,
        "total": 186,
        "violations": 0
    },
    "finished": false,
    "runner": "BSA",
    "running": true,
    "solution": {
        "Instance_id": "test-C1-P1-102",
        "Used_workers": 85,
        "Workers": [
            {
                "Assignments": [
                    "I6",
                    "I73",
                    "I44",
                    "I21",
                    "I4",
                    "I71"
                ],
                "Id": "W0"
            },
            {
                "Assignments": [
                    "I72",
                    "I67",
                    "I50",
                    "I15",
                    "I64",
                    "I89"
                ],
                "Id": "W1"
            },
            {
                "Assignments": [
                    "I6",
                    "I37",
                    "I86",
                    "I81",
                    "I58",
                    "I59"
                ],
                "Id": "W2"
            },
            {
                "Assignments": [
                    "I30",
                    "I1",
                    "I92",
                    "I15",
                    "I22",
                    "I11"
                ],
                "Id": "W3"
            },
            {
                "Assignments": [
                    "I18",
                    "I1",
                    "I32",
                    "I75",
                    "I4",
                    "I77"
                ],
                "Id": "W4"
            },
            {
                "Assignments": [
                    "I42",
                    "I25",
                    "I74",
                    "I15",
                    "I70",
                    "I77"
                ],
                "Id": "W5"
            },
            {
                "Assignments": [
                    "I30",
                    "I49",
                    "I56",
                    "I21",
                    "I28",
                    "I23"
                ],
                "Id": "W6"
            },
            {
                "Assignments": [
                    "I24",
                    "I61",
                    "I68",
                    "I51",
                    "I82",
                    "I17"
                ],
                "Id": "W7"
            },
            {
                "Assignments": [
                    "I60",
                    "I97",
                    "I56",
                    "I33",
                    "I76",
                    "I53"
                ],
                "Id": "W8"
            },
            {
                "Assignments": [
                    "I48",
                    "I1",
                    "I20",
                    "I33",
                    "I52",
                    "I95"
                ],
                "Id": "W9"
            },
            {
                "Assignments": [
                    "I48",
                    "I49",
                    "I86",
                    "I99",
                    "I52",
                    "I101"
                ],
                "Id": "W10"
            },
            {
                "Assignments": [
                    "I18",
                    "I19",
                    "I98",
                    "I33",
                    "I94",
                    "I89"
                ],
                "Id": "W11"
            },
            {
                "Assignments": [
                    "I24",
                    "I1",
                    "I56",
                    "I21",
                    "I100",
                    "I83"
                ],
                "Id": "W12"
            },
            {
                "Assignments": [
                    "I30",
                    "I79",
                    "I26",
                    "I27",
                    "I88",
                    "I11"
                ],
                "Id": "W13"
            },
            {
                "Assignments": [
                    "I96",
                    "I55",
                    "I80",
                    "I45",
                    "I16",
                    "I95"
                ],
                "Id": "W14"
            },
            {
                "Assignments": [
                    "I84",
                    "I85",
                    "I86",
                    "I99",
                    "I40",
                    "I41"
                ],
                "Id": "W15"
            },
            {
                "Assignments": [
                    "I36",
                    "I73",
                    "I74",
                    "I63",
                    "I64",
                    "I65"
                ],
                "Id": "W16"
            },
            {
                "Assignments": [
                    "I18",
                    "I79",
                    "I50",
                    "I3",
                    "I88",
                    "I5"
                ],
                "Id": "W17"
            },
            {
                "Assignments": [
                    "I54",
                    "I13",
                    "I68",
                    "I33",
                    "I34",
                    "I59"
                ],
                "Id": "W18"
            },
            {
                "Assignments": [
                    "I0",
                    "I91",
                    "I86",
                    "I87",
                    "I40",
                    "I29"
                ],
                "Id": "W19"
            },
            {
                "Assignments": [
                    "I24",
                    "I73",
                    "I8",
                    "I99",
                    "I94",
                    "I29"
                ],
                "Id": "W20"
            },
            {
                "Assignments": [
                    "I78",
                    "I61",
                    "I92",
                    "I57",
                    "I22",
                    "I23"
                ],
                "Id": "W21"
            },
            {
                "Assignments": [
                    "I0",
                    "I79",
                    "I92",
                    "I63",
                    "I34",
                    "I95"
                ],
                "Id": "W22"
            },
            {
                "Assignments": [
                    "I48",
                    "I7",
                    "I14",
                    "I15",
                    "I34",
                    "I23"
                ],
                "Id": "W23"
            },
            {
                "Assignments": [
                    "I66",
                    "I67",
                    "I32",
                    "I57",
                    "I94",
                    "I83"
                ],
                "Id": "W24"
            },
            {
                "Assignments": [
                    "I0",
                    "I13",
                    "I38",
                    "I3",
                    "I52",
                    "I11"
                ],
                "Id": "W25"
            },
            {
                "Assignments": [
                    "I18",
                    "I43",
                    "I8",
                    "I3",
                    "I46",
                    "I35"
                ],
                "Id": "W26"
            },
            {
                "Assignments": [
                    "I78",
                    "I13",
                    "I26",
                    "I75",
                    "I22",
                    "I11"
                ],
                "Id": "W27"
            },
            {
                "Assignments": [
                    "I66",
                    "I91",
                    "I86",
                    "I9",
                    "I10",
                    "I101"
                ],
                "Id": "W28"
            },
            {
                "Assignments": [
                    "I60",
                    "I43",
                    "I68",
                    "I39",
                    "I4",
                    "I71"
                ],
                "Id": "W29"
            },
            {
                "Assignments": [
                    "I96",
                    "I91",
                    "I32",
                    "I63",
                    "I64",
                    "I83"
                ],
                "Id": "W30"
            },
            {
                "Assignments": [
                    "I72",
                    "I85",
                    "I98",
                    "I9",
                    "I28",
                    "I17"
                ],
                "Id": "W31"
            },
            {
                "Assignments": [
                    "I18",
                    "I61",
                    "I62",
                    "I81",
                    "I58",
                    "I59"
                ],
                "Id": "W32"
            },
            {
                "Assignments": [
                    "I12",
                    "I85",
                    "I32",
                    "I21",
                    "I40",
                    "I53"
                ],
                "Id": "W33"
            },
            {
                "Assignments": [
                    "I12",
                    "I49",
                    "I80",
                    "I27",
                    "I28",
                    "I65"
                ],
                "Id": "W34"
            },
            {
                "Assignments": [
                    "I24",
                    "I19",
                    "I20",
                    "I93",
                    "I52",
                    "I53"
                ],
                "Id": "W35"
            },
            {
                "Assignments": [
                    "I60",
                    "I31",
                    "I92",
                    "I93",
                    "I88",
                    "I5"
                ],
                "Id": "W36"
            },
            {
                "Assignments": [
                    "I36",
                    "I25",
                    "I44",
                    "I63",
                    "I70",
                    "I41"
                ],
                "Id": "W37"
            },
            {
                "Assignments": [
                    "I72",
                    "I79",
                    "I20",
                    "I51",
                    "I88",
                    "I17"
                ],
                "Id": "W38"
            },
            {
                "Assignments": [
                    "I84",
                    "I67",
                    "I68",
                    "I57",
                    "I64",
                    "I59"
                ],
                "Id": "W39"
            },
            {
                "Assignments": [
                    "I54",
                    "I19",
                    "I14",
                    "I93",
                    "I76",
                    "I47"
                ],
                "Id": "W40"
            },
            {
                "Assignments": [
                    "I84",
                    "I31",
                    "I92",
                    "I9",
                    "I4",
                    "I17"
                ],
                "Id": "W41"
            },
            {
                "Assignments": [
                    "I12",
                    "I67",
                    "I80",
                    "I75",
                    "I10",
                    "I89"
                ],
                "Id": "W42"
            },
            {
                "Assignments": [
                    "I60",
                    "I97",
                    "I26",
                    "I93",
                    "I94",
                    "I29"
                ],
                "Id": "W43"
            },
            {
                "Assignments": [
                    "I66",
                    "I97",
                    "I20",
                    "I27",
                    "I82",
                    "I89"
                ],
                "Id": "W44"
            },
            {
                "Assignments": [
                    "I42",
                    "I43",
                    "I56",
                    "I51",
                    "I10",
                    "I23"
                ],
                "Id": "W45"
            },
            {
                "Assignments": [
                    "I72",
                    "I13",
                    "I80",
                    "I45",
                    "I10",
                    "I47"
                ],
                "Id": "W46"
            },
            {
                "Assignments": [
                    "I54",
                    "I61",
                    "I2",
                    "I57",
                    "I82",
                    "I5"
                ],
                "Id": "W47"
            },
            {
                "Assignments": [
                    "I96",
                    "I85",
                    "I8",
                    "I45",
                    "I88",
                    "I77"
                ],
                "Id": "W48"
            },
            {
                "Assignments": [
                    "I96",
                    "I79",
                    "I98",
                    "I69",
                    "I46",
                    "I47"
                ],
                "Id": "W49"
            },
            {
                "Assignments": [
                    "I90",
                    "I7",
                    "I50",
                    "I63",
                    "I22",
                    "I89"
                ],
                "Id": "W50"
            },
            {
                "Assignments": [
                    "I90",
                    "I7",
                    "I62",
                    "I81",
                    "I34",
                    "I77"
                ],
                "Id": "W51"
            },
            {
                "Assignments": [
                    "I0",
                    "I37",
                    "I14",
                    "I39",
                    "I28",
                    "I101"
                ],
                "Id": "W52"
            },
            {
                "Assignments": [
                    "I12",
                    "I25",
                    "I8",
                    "I15",
                    "I76",
                    "I23"
                ],
                "Id": "W53"
            },
            {
                "Assignments": [
                    "I6",
                    "I37",
                    "I2",
                    "I45",
                    "I76",
                    "I101"
                ],
                "Id": "W54"
            },
            {
                "Assignments": [
                    "I90",
                    "I31",
                    "I74",
                    "I3",
                    "I58",
                    "I35"
                ],
                "Id": "W55"
            },
            {
                "Assignments": [
                    "I30",
                    "I37",
                    "I68",
                    "I9",
                    "I4",
                    "I11"
                ],
                "Id": "W56"
            },
            {
                "Assignments": [
                    "I42",
                    "I55",
                    "I50",
                    "I87",
                    "I100",
                    "I65"
                ],
                "Id": "W57"
            },
            {
                "Assignments": [
                    "I60",
                    "I55",
                    "I26",
                    "I93",
                    "I16",
                    "I35"
                ],
                "Id": "W58"
            },
            {
                "Assignments": [
                    "I66",
                    "I19",
                    "I74",
                    "I69",
                    "I64",
                    "I77"
                ],
                "Id": "W59"
            },
            {
                "Assignments": [
                    "I0",
                    "I97",
                    "I14",
                    "I45",
                    "I16",
                    "I83"
                ],
                "Id": "W60"
            },
            {
                "Assignments": [
                    "I48",
                    "I19",
                    "I44",
                    "I39",
                    "I82",
                    "I83"
                ],
                "Id": "W61"
            },
            {
                "Assignments": [
                    "I30",
                    "I49",
                    "I38",
                    "I69",
                    "I100",
                    "I41"
                ],
                "Id": "W62"
            },
            {
                "Assignments": [
                    "I24",
                    "I49",
                    "I38",
                    "I21",
                    "I28",
                    "I35"
                ],
                "Id": "W63"
            },
            {
                "Assignments": [
                    "I90",
                    "I25",
                    "I62",
                    "I81",
                    "I40",
                    "I17"
                ],
                "Id": "W64"
            },
            {
                "Assignments": [
                    "I84",
                    "I37",
                    "I98",
                    "I87",
                    "I34",
                    "I47"
                ],
                "Id": "W65"
            },
            {
                "Assignments": [
                    "I36",
                    "I25",
                    "I62",
                    "I99",
                    "I22",
                    "I95"
                ],
                "Id": "W66"
            },
            {
                "Assignments": [
                    "I72",
                    "I73",
                    "I20",
                    "I81",
                    "I40",
                    "I95"
                ],
                "Id": "W67"
            },
            {
                "Assignments": [
                    "I78",
                    "I55",
                    "I8",
                    "I39",
                    "I100",
                    "I47"
                ],
                "Id": "W68"
            },
            {
                "Assignments": [
                    "I90",
                    "I43",
                    "I2",
                    "I27",
                    "I70",
                    "I71"
                ],
                "Id": "W69"
            },
            {
                "Assignments": [
                    "I96",
                    "I97",
                    "I38",
                    "I69",
                    "I16",
                    "I5"
                ],
                "Id": "W70"
            },
            {
                "Assignments": [
                    "I54",
                    "I1",
                    "I50",
                    "I87",
                    "I10",
                    "I101"
                ],
                "Id": "W71"
            },
            {
                "Assignments": [
                    "I6",
                    "I73",
                    "I98",
                    "I3",
                    "I58",
                    "I29"
                ],
                "Id": "W72"
            },
            {
                "Assignments": [
                    "I48",
                    "I85",
                    "I44",
                    "I69",
                    "I46",
                    "I65"
                ],
                "Id": "W73"
            },
            {
                "Assignments": [
                    "I42",
                    "I91",
                    "I62",
                    "I75",
                    "I70",
                    "I41"
                ],
                "Id": "W74"
            },
            {
                "Assignments": [
                    "I66",
                    "I7",
                    "I44",
                    "I87",
                    "I100",
                    "I29"
                ],
                "Id": "W75"
            },
            {
                "Assignments": [
                    "I6",
                    "I31",
                    "I56",
                    "I9",
                    "I16",
                    "I71"
                ],
                "Id": "W76"
            },
            {
                "Assignments": [
                    "I36",
                    "I55",
                    "I26",
                    "I51",
                    "I46",
                    "I53"
                ],
                "Id": "W77"
            },
            {
                "Assignments": [
                    "I54",
                    "I7",
                    "I2",
                    "I99",
                    "I82",
                    "I5"
                ],
                "Id": "W78"
            },
            {
                "Assignments": [
                    "I78",
                    "I31",
                    "I38",
                    "I33",
                    "I52",
                    "I71"
                ],
                "Id": "W79"
            },
            {
                "Assignments": [
                    "I42",
                    "I13",
                    "I2",
                    "I51",
                    "I76",
                    "I59"
                ],
                "Id": "W80"
            },
            {
                "Assignments": [
                    "I12",
                    "I61",
                    "I32",
                    "I57",
                    "I46",
                    "I65"
                ],
                "Id": "W81"
            },
            {
                "Assignments": [
                    "I78",
                    "I91",
                    "I14",
                    "I39",
                    "I58",
                    "I35"
                ],
                "Id": "W82"
            },
            {
                "Assignments": [
                    "I36",
                    "I43",
                    "I80",
                    "I75",
                    "I70",
                    "I41"
                ],
                "Id": "W83"
            },
            {
                "Assignments": [
                    "I84",
                    "I67",
                    "I74",
                    "I27",
                    "I94",
                    "I53"
                ],
                "Id": "W84"
            }
        ]
    },
    "started": "2021-03-31T13:19:57Z",
    "submitted": "2021-03-31T13:19:57Z",
    "task_id": "8488109602186256044"
}
```

#### `DELETE`

Rimuove la soluzione dal pool di soluzioni.

## Endpoint `/evaluate/`

L'endpoint consente la valutazione di una soluzione in termini di costo.

#### `POST`

Accetta un file json analogo a quello passato all'endpoint `/runner`, effettua il ricalcolo dei costi.

Restituisce la soluzione nel formato dell'endpoint `/solution` con in aggiunta un campo `issues`, che contiene eventualmente una lista di problemi riscontrati nella lettura o nella conversione.

## Endpoint `/stats/`

L'endpoint restituisce statistiche sull'utilizzo di memoria e cpu, se disponibili.

#### `GET`

Invoca la funzionalità e restituisce le statistiche con il seguente formato:

```json
{
  "statistics": [
    { "time": <timestamp>, "cpu": <cpu_load>, "memory": <memory_load> }, ...
  ]
}
```

## Endpoint `/neighborhood/<neighborhood_name>`

L’endpoint estende le funzionalità dell’API REST consentendo di effettuare le operazioni descritte in seguito.

### `/neighborhood/<neighborhood_name>/best-move`

#### `POST`

Invoca la funzionalità per la generazione della miglior mossa possibile nel vicinato specificato a partire dalla soluzione passata come *payload* `initial_solution` con il formato consueto degli altri endpoint.

La funzionalità cerca la migliore mossa possibile a partire dalla soluzione inviata e restituisce:

1. una descrizione della mossa e alcune statistiche sulla stessa (in particolare il suo costo) disponibili in un campo `"move"`
2. la nuova soluzione a seguito dell’esecuzione della mossa;
3. il costo della nuova soluzione.

### `/neighborhood/<neighborhood_name>/make-move`

#### `POST`

Invoca la funzionalità per l'esecuzione di una mossa specificata nel *payload* e appartenente al vicinato specificato a partire dalla soluzione passata come `initial_solution` con il formato consueto degli altri endpoint.

La funzionalità esegue la mossa e restituisce:

1. una descrizione della mossa e alcune statistiche sulla stessa (in particolare il suo costo) disponibili in un campo `"move"`
2. la nuova soluzione a seguito dell’esecuzione della mossa;
3. il costo della nuova soluzione.

## ⚠️ Nessun neighborhood esposto per ora