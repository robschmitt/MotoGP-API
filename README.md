# MotoGP API
## Introduction

The MotoGP API can be broadly divided into two sections:

- **Results API**: Delivers data focused on results, such as practice and race sessions, classifications, grid positions, and more.
- **Broadcast API**: Provides information about events, riders, and seasons.

### API Calls
The MotoGP API is a restful API with different endpoints which return JSON metadata about events, seasons, riders, classifications and so on.

### Base Address
The base address of the MotoGP API is `https://api.motogp.pulselive.com/motogp/v1`

### Requests
Data resources are accessed via standard HTTP requests in UTF-8 format. The API is read only, therefore the only HTTP verb in use is `GET`.

## Results API

### Live Timing
Retrieve a timing feed during live sessions.

`GET` `/timing-gateway/livetiming-lite`

#### Response
```json
  "head": {
    "championship_id": "3",
    "category": "MotoGP",
    "circuit_id": "6",
    "circuit_name": "Autodromo Internazionale del Mugello",
    "global_event_id": "",
    "event_id": "606",
    "event_tv_name": "Mugello MotoGP Official Test",
    "event_shortname": "IT1",
    "date": "03/06/2024",
    "datet": 20240603,
    "datst": 20240603,
    "num_laps": 0,
    "gmt": "",
    "trsid": 1,
    "session_id": "1",
    "session_type": 4,
    "session_name": "Session 1",
    "session_shortname": "FP1",
    "duration": "288000000",
    "remaining": "0",
    "session_status_id": "F",
    "session_status_name": "F",
    "date_formated": "10:00 - 03/06/2024",
    "url": null
  },
  "rider": {
    "1": {
      "order": 1,
      "rider_id": 7646,
      "status_name": "CL",
      "status_id": "1",
      "rider_number": "33",
      "color": "ff6600",
      "text_color": "ffffff",
      "pos": 1,
      "rider_shortname": "Binder",
      "rider_name": "Brad",
      "rider_surname": "BINDER",
      "lap_time": "1'47.617",
      "num_lap": 5,
      "last_lap_time": "1'57.459",
      "last_lap": 17,
      "trac_status": "B",
      "team_name": "Red Bull KTM Factory Racing",
      "bike_name": "KTM",
      "gap_first": "0.000",
      "gap_prev": "0.000",
      "on_pit": true
    },
    ...
  }
}
```

### Get Seasons
Retrieve a list of MotoGP seasons.

`GET` `/results/seasons`

#### Response
```json
[
  {
    "id": "db8dc197-c7b2-4c1b-b3a4-6dc534c023ef",
    "name": null,
    "year": 2023,
    "current": true
  },
  {
    "id": "db8dc197-c7b2-4c1b-b3a4-6dc534c014ef",
    "name": null,
    "year": 2022,
    "current": false
  }
  ...
]
```

### Get Events
Retrieve a list of MotoGP events for a given season.

`GET` `/results/events?seasonUuid={id}&isFinished={isFinished}`

`id` string **required**\
The season UUID.

`isFinished` boolean\
Retrieve only past events when `true`. Valid values are `true`, `false`.

#### Response
```json
[
  {
    "country": {
      "iso": "PT",
      "name": "Portugal",
      "region_iso": ""
    },
    "event_files": {
      ...
    },
    "circuit": {
      "name": "Autódromo Internacional do Algarve",
      ...
    },
    "sponsored_name": "Grande Prémio Tissot de Portugal",
    "toad_api_uuid": "4b70d9b1-3f31-4521-9ec1-9d18cd67a32a",
	...
  }
]
```

#### Note
The value of `toad_api_uuid` is a reference to the same event in the Broadcast API.

### Get Season’s Categories
Retrieves a list of categories for a given season.

`GET` `/results/categories?seasonUuid={id}`

`id` string **required**\
The season ID.

#### Response
```json
[
  {
    "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
    "name": "MotoGP™",
    "legacy_id": 3
  },
  {
    "id": "549640b8-fd9c-4245-acfd-60e4bc38b25c",
    "name": "Moto2™",
    "legacy_id": 2
  },
  {
    "id": "954f7e65-2ef2-4423-b949-4961cc603e45",
    "name": "Moto3™",
    "legacy_id": 1
  },
  {
    "id": "3bda82ba-0445-440c-9375-cd48d777bd95",
    "name": "MotoE™",
    "legacy_id": 19
  }
]
```

### Get Event’s Categories
Retrieves a list of categories for a given event.

`GET` `/results/categories?eventUuid={id}`

`id` string **required**\
The event ID.

#### Response
```json
[
  {
    "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
    "name": "MotoGP™",
    "legacy_id": 3
  },
  {
    "id": "549640b8-fd9c-4245-acfd-60e4bc38b25c",
    "name": "Moto2™",
    "legacy_id": 2
  },
  {
    "id": "954f7e65-2ef2-4423-b949-4961cc603e45",
    "name": "Moto3™",
    "legacy_id": 1
  },
  {
    "id": "3bda82ba-0445-440c-9375-cd48d777bd95",
    "name": "MotoE™",
    "legacy_id": 19
  }
]
```

### Get Sessions
Retrieve sessions for a given event.

`GET` `/results/sessions?eventUuid={eventId}&categoryUuid={categoryId}`

`eventId` string **required**\
The event ID.

`categoryId` string **required**\
The category ID.

#### Response
```json
[
  {
    "date": "2023-03-24T10:45:00+00:00",
    "number": 1,
    "condition": {
      "track": "Dry",
      "air": "17º",
      "humidity": "72%",
      "ground": "21º",
      "weather": "Cloudy"
    },
    "circuit": "Autódromo Internacional do Algarve",
    "session_files": {
      ...
    },
    "id": "cb7655d9-387b-4247-9bbe-a067bbe484ff",
    "type": "P",
    "category": {
      "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
      "legacy_id": 3,
      "name": "MotoGP™"
    },
    "event": {
      ...
    },
    "status": "Official"
  },
  ...
]
```

### Get Session
Retrieve a single session

`GET` `/results/sessions/{id}`

`id` string **required**\
The session ID.

#### Response
```json

  "date": "2023-03-24T10:45:00+00:00",
  "number": 1,
  "condition": {
    "track": "Dry",
    "air": "17º",
    "humidity": "72%",
    "ground": "21º",
    "weather": "Cloudy"
  },
  "circuit": "Autódromo Internacional do Algarve",
  "session_files": {
    ...
  },
  "id": "cb7655d9-387b-4247-9bbe-a067bbe484ff",
  "type": "P",
  "category": {
    "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
    "legacy_id": 3,
    "name": "MotoGP™"
  },
  "event": {
    ...
  },
  "status": "Official"
}
```

### Get Classification
Retrieve the classification for a single session.

`GET` `/results/session/{id}/classification?seasonYear={seasonYear}&test={isTest}`

`id` string **required**\
The session ID.

`seasonYear` integer\
The season year. Valid values are `2023`, `2024`, etc. Passing this value seems to have no effect, but may possibly be used for disambiguation where required.

`isTest` boolean\
Whether the current session is for an official test (as opposed to a normal race weekend). Valid values are `true`, `false`.

#### Response
```json
{
  "classification": [
    {
      "id": "372fcfe7-a9e9-4ebc-817e-67ce7300204e",
      "position": 1,
      "rider": {
        "id": "e7e4c72e-952e-4f64-8e8a-a47ff590fade",
        "full_name": "Alex Marquez",
        "country": {
          "iso": "ES",
          "name": "Spain",
          "region_iso": ""
        },
        "legacy_id": 8173,
        "number": 73,
        "riders_api_uuid": "41195f0f-9817-4a4d-913e-c1fbbb351d9b"
      },
      "team": {
        "id": "c3c7604e-d1d6-46c0-955f-b53214e0812c",
        "name": "Gresini Racing MotoGP",
        "legacy_id": 77,
        "season": {
          "id": "db8dc197-c7b2-4c1b-b3a4-6dc534c023ef",
          "year": 2023,
          "current": false
        }
      },
      "constructor": {
        "id": "b9d93efb-3cd0-4681-9de4-c412a866d568",
        "name": "Ducati",
        "legacy_id": 110
      },
      "best_lap": {
        "number": 12,
        "time": "01:38.782"
      },
      "total_laps": 15,
      "top_speed": 339.6,
      "gap": {
        "first": "0.000",
        "prev": "0.000"
      },
      "status": "INSTND"
    },
	...
  ],
  "file": "https://resources.motogp.com/files/results/2023/POR/MotoGP/P1/Classification.pdf",
  "files": null
}
```

### Get Entry List
Retrieve the riders partaking in an event.

`GET` `/event/{eventId}/entry?categoryUuid={categoryId}`

`eventId` string **required**\
The event ID.

`categoryId` string **required**\
The category ID.

#### Response
```json
{
  "entry": [
    {
      "id": "5b510f5b-cf29-46b7-832f-1eb96c900188",
      "number": 1,
      "rider": {
        "id": "9cb55304-0ac1-401c-beb6-1a4f445018a4",
        "full_name": "Francesco Bagnaia",
        "country": {
          "iso": "IT",
          "name": "Italy",
          "region_iso": ""
        },
        "legacy_id": 8273,
        "number": 1,
        "rider_api_uuid": "66b78301-5826-4986-b11e-fa68a7bd77a7"
      },
      "team": {
        "id": "52ce0374-e215-4d1c-ae94-062ea3cb45ac",
        "name": "Ducati Lenovo Team",
        "legacy_id": 75,
        "season": {
          "id": "db8dc197-c7b2-4c1b-b3a4-6dc534c023ef",
          "year": 2023,
          "current": false
        }
      },
      "team_name": "Ducati Lenovo Team",
      "constructor": {
        "id": "b9d93efb-3cd0-4681-9de4-c412a866d568",
        "name": "Ducati",
        "legacy_id": 110
      },
      "wildcard": false,
      "replacement": null,
      "replaced": false,
      "rookieOfTheYear": false,
      "independentTeamRider": false
    },
	...
  ],
  "file": "https://resources.motogp.com/files/results/2023/FRA/MotoGP/Entry.pdf"
}
```

### Get Grid Positions
Retrieve the grid positions for a single event.

`GET` `/results/event/{eventId}/category/{categoryId}/grid`

`eventId` string **required**\
The event ID.

`categoryId` string **required**\
The category ID.

#### Response
```json
[
  {
    "rider": {
      "id": "f55b433d-38b8-4d1d-bb3a-a709c82a0260",
      "full_name": "Marc Marquez",
      "country": {
        "iso": "ES",
        "name": "Spain",
        "region_iso": ""
      },
      "legacy_id": 7444,
      "riders_api_uuid": "23e50438-a657-4fb0-a190-3262b5472f29"
    },
    "category": {
      "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
      "legacy_id": 3,
      "name": "MotoGP™"
    },
    "event": {
      ...
    },
    "qualifying_position": 1,
    "qualifying_time": "01:37.226"
  },
  ...
]
```

### Get Standings
Retrieve the rider standings.

`GET` `/results/standings?seasonUuid={seasonId}&categoryUuid={categoryId}`

`seasonId` string **required**\
The season ID.

`categoryId` string **required**\
The category ID.

#### Response
```json
{
  "file": "https://resources.motogp.com/files/results/2023/VAL/Moto2/RAC/worldstanding.pdf",
  "classification": [
    {
      "id": "88fb6e29-1a24-4ccd-b47f-619f5dd2ab77",
      "position": 1,
      "rider": {
        "id": "a6108385-dd4c-490d-b02e-542ed0275f79",
        "full_name": "Pedro Acosta",
        "country": {
          "iso": "ES",
          "name": "Spain",
          "region_iso": ""
        },
        "legacy_id": 8658,
        "number": 37,
        "riders_api_uuid": "ea39a0af-95d3-4a37-81a7-f332efdb9216"
      },
      "team": {
        "id": "57a76f99-caf6-4020-88d3-c8532c0b7af0",
        "name": "Red Bull KTM Ajo",
        "legacy_id": 49,
        "season": {
          "id": "db8dc197-c7b2-4c1b-b3a4-6dc534c023ef",
          "year": 2023,
          "current": false
        }
      },
      "constructor": {
        "id": "40e1073c-1f6a-44c3-8320-cd6eb811ac71",
        "name": "Kalex",
        "legacy_id": 447
      },
      "session": "RAC",
      "points": 332.5
    },
    ...
  ]
  "xmlFile": "https://resources.motogp.com/files/results/2023/VAL/Moto2/RAC/worldstanding.XML"
}
```

### Get Standings Files
Retrieve the official files relating to the standings.

`GET` `/results/standings/files?seasonUuid={seasonId}&categoryUuid={categoryId}`

`seasonId` string **required**\
The season ID.

`categoryId` string **required**\
The category ID.

#### Response
```json
{
  "riders_results": {
    "podiums": "https://resources.motogp.com/files/results/2023/VAL/Podiums.pdf",
    "pole_positions": "https://resources.motogp.com/files/results/2023/VAL/PolePositions.pdf",
    "nations_statistics": "https://resources.motogp.com/files/results/2023/VAL/table2.pdf",
    "riders_all_time": "https://resources.motogp.com/files/results/2023/VAL/table4.pdf"
  },
  "riders_championship": {
    "independent_team_rider": "",
    "rookie_of_the_year": "https://resources.motogp.com/files/results/2023/VAL/Moto2/RAC/rookieirtacup.pdf",
    "season_statistics": "https://resources.motogp.com/files/results/2023/VAL/Moto2/table1.pdf",
    "bmw_award": ""
  }
}
```

### Get Rider qualifying standings (BMW Award)
Retrieve the qualifying standings.

`GET` `/results/standings/bmwaward?seasonUuid={seasonId}`

`seasonId` string **required**\
The season ID.

#### Response
```json

  "file": "https://resources.motogp.com/files/results/2023/VAL/MotoGP/BMW_Award.pdf",
  "classification": [
    {
      "id": "90d81326-4f7d-4165-b165-1fedb3beced2",
      "position": 1,
      "rider": {
        "id": "9cb55304-0ac1-401c-beb6-1a4f445018a4",
        "full_name": "Francesco Bagnaia",
        "country": {
          "iso": "IT",
          "name": "Italy",
          "region_iso": ""
        },
        "legacy_id": 8273,
        "number": 1,
        "riders_api_uuid": "66b78301-5826-4986-b11e-fa68a7bd77a7"
      },
      "team": {
        "id": "52ce0374-e215-4d1c-ae94-062ea3cb45ac",
        "name": "Ducati Lenovo Team",
        "legacy_id": 75,
        "season": {
          "id": "db8dc197-c7b2-4c1b-b3a4-6dc534c023ef",
          "year": 2023,
          "current": false
        }
      },
      "constructor": {
        "id": "b9d93efb-3cd0-4681-9de4-c412a866d568",
        "name": "Ducati",
        "legacy_id": 110
      },
      "countryShortName": "ITA",
      "points": 369,
      "event": {
        ...
      }
    },
	...
  ]
}
```

## Broadcast API

### Season categories
Retrieve all categories for a single season

`GET` `/categories?seasonYear={seasonYear}`

`seasonYear` integer **required**\
The season year. Valid values are `2023`, `2024`, etc.

#### Response
```json
[
  {
    "id": "737ab122-76e1-4081-bedb-334caaa18c70",
    "name": "MotoGP",
    "legacy_id": 3
  },
  {
    "id": "ea854a67-73a4-4a28-ac77-d67b3b2a530a",
    "name": "Moto2",
    "legacy_id": 2
  },
  {
    "id": "1ab203aa-e292-4842-8bed-971911357af1",
    "name": "Moto3",
    "legacy_id": 1
  },
  {
    "id": "cf196668-f900-4116-af79-810b91828a37",
    "name": "MotoE",
    "legacy_id": 19
  }
]
```

### Get Events
Retrieves the broadcast events.

`GET` `/events/seasonYear={seasonYear}`

`seasonYear` integer **required**\
The season year. Valid values are `2023`, `2024`, etc.

#### Response
```json
[
  {
    "timing_id": 1000,
    "event_categories": [
      {
        "category_id": "93888447-8746-4161-882c-e08a1d48447e",
        "category_timing_id": 3,
        "timing_id": 601,
        "sequence": 1
      }
    ],
    "country": "ES",
    "circuit": {
      "id": "c5e48a7a-42d3-4e5d-8a40-2efc77eb36b6",
      "name": "Circuit Ricardo Tormo",
      "iso_code": "ES",
      "country": "Spain",
      "region": "VC",
      "city": "Cheste",
      "postal_code": "46380",
      "address": "Salida, Autovía del Este, 334, 46380 Madrid, Valencia, Spain",
      "lat": "39.4848428",
      "lng": "-0.6265042",
      "place_id": "ChIJn1iBxLHwYA0R9ErxCUURNkQ",
      "constructed": 1999,
      "designer": "Antonio Risueño Galindo",
      "active": true,
      "timing_ids": [
        {
          "business_unit": "MGP",
          "id": 77
        },
        {
          "business_unit": "RKC",
          "id": 77
        },
        {
          "business_unit": "CEV",
          "id": 77
        },
        {
          "business_unit": "ATC",
          "id": 77
        }
      ],
      "track": {
        ...
      },
      "circuit_descriptions": [
          ...
      ],
      "user_location": {
        "lat": "39.487719",
        "lng": "-0.628912",
        "radius": 2000
      }
    },
    "kind": "TEST",
    "broadcasts": [
      {
        "id": "c09e76c7-0d9a-4a91-a30d-4737e0188591",
        "shortname": "FP1",
        "name": "Free Practice",
        "date_start": "2023-11-28T10:00:00+0100",
        "date_end": "2023-11-28T18:00:00+0100",
        "remain": -17291893,
        "type": "SESSION",
        "kind": "PRACTICE",
        "status": "FINISHED",
        "progressive": 1,
        "has_timing": true,
        "has_live": true,
        "has_report": false,
        "has_results": true,
        "has_on_demand": true,
        "is_live": false,
        "is_live_timing": false,
        "live": false,
        "category": {
          "id": "93888447-8746-4161-882c-e08a1d48447e",
          "acronym": "MGP",
          "name": "MotoGP",
          "active": true,
          "timing_id": 3,
          "priority": 1
        },
        "gp_day": 1,
        "timing_id": 1
      }
    ],
    "date_end": "2023-11-28T01:00:00+01:00",
    "time_zone": "EUROPE/MADRID",
    "type": "SPORT",
    "shortname": "VC1",
    "business_unit": {
      "id": "d6ced16c-c25e-4f61-bfa5-4683679ff873",
      "name": "MotoGP™",
      "acronym": "MGP"
    },
    "url": "Valencia MotoGP™ Official Test ",
    "sequence": 1,
    "schedule": {
      "options": [
        {
          "date": 1701129600,
          "dateStart": "2023-11-28T10:00:00+0100",
          "name": "Tuesday",
          "day": 28,
          "month": "November",
          "day_suffix": "th",
          "gp_day": 1
        }
      ],
      "selected_day": 1
    },
    "date_start": "2023-11-28T01:00:00+01:00",
    "urls": [
      
    ],
    "assets": [
      ...
    ],
    "name": "Valencia MotoGP™ Official Test ",
    "season": {
      "id": "b17bec50-9780-4845-a6fd-f6212d05276c",
      "year": 2024,
      "current": true
    },
    "id": "fc73530a-7d68-4a25-8d86-9cb932a3bca9",
    "categories": [
      {
        "id": "93888447-8746-4161-882c-e08a1d48447e",
        "acronym": "MGP",
        "name": "MotoGP",
        "active": true,
        "timing_id": 3,
        "priority": 1
      }
    ],
    "status": "FINISHED",
    "hashtag": "#ValenciaTest"
  },
  ...
]
```

### Get Event
Retrieve a single broadcast event

`GET` `/events/{id}`

`id` string **required**\
The broadcast event ID.

#### Response
```json
{
  "event_categories": [
    ...
  ],
  "country": "AR",
  "circuit": {
    ...
  },
  "type": "SPORT",
  "urls": [
    ...
  ],
  "assets": [
    ...
  ],
  "season": {
    "id": "b38a3bd8-8bab-47b4-b1f4-a41bd03b9a25",
    "year": 2023,
    "current": false
  },
  "id": "688005f3-5ce3-482d-83ec-c05c8d8eb844",
  "categories": [
    {
      "id": "93888447-8746-4161-882c-e08a1d48447e",
      "acronym": "MGP",
      "name": "MotoGP",
      "active": true,
      "timing_id": 3,
      "priority": 1
    },
    {
      "id": "bc2b0143-1bfb-4ad0-9501-da2e474e3ea7",
      "acronym": "MT2",
      "name": "Moto2",
      "active": true,
      "timing_id": 2,
      "priority": 2
    },
    {
      "id": "7b0adf61-0a93-4e3d-a7ef-1fee93c2591f",
      "acronym": "MT3",
      "name": "Moto3",
      "active": true,
      "timing_id": 1,
      "priority": 3
    }
  ],
  "hashtag": "#ArgentinaGP",
  "timing_id": 2,
  "kind": "GP",
  "broadcasts": [
    {
      "id": "4f5093d8-7345-4939-9030-52f3a6f36c02",
      "shortname": "SHOW",
      "name": "GearUP",
      "date_start": "2023-03-30T11:45:00-0300",
      "date_end": "2023-03-30T12:00:00-0300",
      "remain": -38267263,
      "type": "MEDIA",
      "kind": "PRESS",
      "status": "FINISHED",
      "progressive": 11,
      "has_timing": false,
      "has_live": false,
      "has_report": false,
      "has_results": false,
      "has_on_demand": false,
      "is_live": false,
      "is_live_timing": false,
      "live": false,
      "category": {
        "id": "93888447-8746-4161-882c-e08a1d48447e",
        "acronym": "MGP",
        "name": "MotoGP",
        "active": true,
        "timing_id": 3,
        "priority": 1
      },
      "gp_day": 0,
      "timing_id": 110
    },
    ...
  ],
  "date_end": "2023-04-01T21:00:00-03:00",
  "time_zone": "AMERICA/CORDOBA",
  "shortname": "ARG",
  "business_unit": {
    "id": "d6ced16c-c25e-4f61-bfa5-4683679ff873",
    "name": "MotoGP™",
    "acronym": "MGP"
  },
  "url": "Argentina",
  "results-api-circuit-uuid": "536d88a0-b7b9-4312-a5f1-f196e8228991",
  "sequence": 2,
  "schedule": {
    "options": [
      {
        "date": 1680134400,
        "dateStart": "2023-03-30T13:15:00-0300",
        "name": "Thursday",
        "day": 30,
        "month": "March",
        "day_suffix": "th",
        "gp_day": 0
      },
      {
        "date": 1680220800,
        "dateStart": "2023-03-31T15:00:00-0300",
        "name": "Friday",
        "day": 31,
        "month": "March",
        "day_suffix": "st",
        "gp_day": 1
      },
      {
        "date": 1680307200,
        "dateStart": "2023-04-01T17:00:00-0300",
        "name": "Saturday",
        "day": 1,
        "month": "April",
        "day_suffix": "st",
        "gp_day": 2
      },
      {
        "date": 1680393600,
        "dateStart": "2023-04-02T15:45:00-0300",
        "name": "Sunday",
        "day": 2,
        "month": "April",
        "day_suffix": "nd",
        "gp_day": 3
      }
    ],
    "selected_day": 3
  },
  "date_start": "2023-03-30T21:00:00-03:00",
  "results-api-event-uuid": "7dfe8c6b-d700-41d7-a7e8-fde4069b8c05",
  "name": "Gran Premio Michelin® de la República Argentina",
  "status": "FINISHED"
}
```

### Get Riders
Retrieve all riders across all categories in the current season.

`GET` `/riders`

#### Response
```json
[
  {
    "current_career_step": {
      "season": 2024,
      "number": 1,
      "sponsored_team": "Ducati Lenovo Team",
      "team": {
        "id": "892fff2f-7402-4fbd-99fb-5fd567d8a80c",
        "constructor": {
          "id": "38af1078-e2f1-4399-811c-1e98cf6f6150",
          "name": "Ducati",
          "legacy_id": 110
        },
        "name": "Ducati Lenovo Team",
        "legacy_id": 15,
        "color": "#cc0000",
        "text_color": "#ffffff",
        "picture": "...",
        "published": true
      },
      "category": {
        "id": "737ab122-76e1-4081-bedb-334caaa18c70",
        "name": "MotoGP",
        "legacy_id": 3
      },
      "in_grid": true,
      "short_nickname": "FB1",
      "current": true,
      "pictures": {
        "profile": {
          "main": "...",
          "secondary": null
        },
        "bike": {
          "main": "...",
          "secondary": null
        },
        "helmet": {
          "main": "...",
          "secondary": null
        },
        "number": "...",
        "portrait": "..."
      },
      "type": "Official"
    },
    "country": {
      "iso": "IT",
      "name": "Italy",
      "flag": "https://photos.motogp.com/countries/flags/iso2/IT.svg"
    },
    "birth_city": "Torino",
    "surname": "Bagnaia",
    "birth_date": "1997-01-14",
    "name": "Francesco",
    "nickname": null,
    "legacy_id": 8273,
    "id": "66b78301-5826-4986-b11e-fa68a7bd77a7",
    "years_old": 27,
    "published": true
  },
  ...
]
```

### Get Rider
Retrieve a single rider.

`GET` `/riders/{id}`

`id` string **required**\
The rider ID.

#### Response
```json
{
  "country": {
    "iso": "ES",
    "name": "Spain",
    "flag": "https://photos.motogp.com/countries/flags/iso2/ES.svg"
  },
  "career": [
    {
      "season": 2024,
      "number": 42,
      "sponsored_team": "Monster Energy Yamaha MotoGP™",
      "team": {
        "id": "141b6f0f-7e53-4d27-9bdb-0ea8fba7e842",
        "constructor": {
          "id": "f2e835be-7fab-4782-a26b-de3d583d132c",
          "name": "Yamaha",
          "legacy_id": 3
        },
        "name": "Monster Energy Yamaha MotoGP™",
        "legacy_id": 19,
        "color": "#0c368c",
        "text_color": "#ffffff",
        "picture": "...",
        "published": true
      },
      "category": {
        "id": "737ab122-76e1-4081-bedb-334caaa18c70",
        "name": "MotoGP",
        "legacy_id": 3
      },
      "in_grid": true,
      "short_nickname": "AR42",
      "current": true,
      "pictures": {
        "profile": {
          "main": "...",
          "secondary": "..."
        },
        "bike": {
          "main": "...",
          "secondary": null
        },
        "helmet": {
          "main": "...",
          "secondary": null
        },
        "number": "...",
        "portrait": "..."
      },
      "type": "Official"
    },
    ...
  ],
  "birth_city": "Barcelona",
  "legend": false,
  "birth_date": "1995-12-08",
  "merchandise_url": "...",
  "biography": {
    ...
  },
  "published": true,
  "legend_picture": null,
  "wildcard": false,
  "injured": false,
  "surname": "Rins",
  "name": "Alex",
  "nickname": null,
  "physical_attributes": {
    "height": 176,
    "weight": 68
  },
  "legacy_id": 8150,
  "id": "04bf0ce4-5062-44fc-9745-ec85a8d8f8d3",
  "banned": false
}
```

### Get Rider Statistics
Retrieve rider statistics.

`GET` `/riders/{legacyId}/stats`

`legacyId` integer **required**\
The Rider legacy ID.

#### Response
```json
{
  "first_grand_prix": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "event": {
        "id": "c68ece2b-fc82-42b1-a1fb-6a0f4af01d64",
        "name": "GRAND PRIX OF QATAR",
        "sponsored_name": "Commercial Bank Grand Prix of Qatar",
        "short_name": "QAT",
        "test": false,
        "season": "2015",
        ...
      }
    },
    ...
  ],
  "podiums": {
    "categories": [
      {
        "category": {
          "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
          "legacy_id": 3,
          "name": "MotoGP™"
        },
        "count": 35
      },
      ...
    ],
    "total": 75
  },
  "last_wins": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "event": {
        "id": "c9d2b49e-2b65-4975-9e4a-80d5429cc1b2",
        "name": "GRAND PRIX OF THE AMERICAS",
        "sponsored_name": "Red Bull Grand Prix of The Americas",
        "short_name": "AME",
        "test": false,
        "season": "2024",
        ...
      }
    },
    {
      "category": {
        "id": "549640b8-fd9c-4245-acfd-60e4bc38b25c",
        "legacy_id": 2,
        "name": "Moto2™"
      },
      "event": {
        "id": "1b869fa9-da9e-4ee5-90c3-fd3e171ad5ae",
        "name": "MALAYSIAN MOTORCYCLE GRAND PRIX",
        "sponsored_name": "Shell Advance Malaysian Motorcycle GP",
        "short_name": "MAL",
        "test": false,
        "season": "2014",
        ...
      }
    },
    ...
  ],
  "third_positions": {
    "categories": [
      {
        "category": {
          "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
          "legacy_id": 3,
          "name": "MotoGP™"
        },
        "count": 14
      },
      ...
    ],
    "total": 22
  },
  "poles": {
    "categories": [
      {
        "category": {
          "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
          "legacy_id": 3,
          "name": "MotoGP™"
        },
        "count": 15
      },
      ...
    ],
    "total": 26
  },
  "first_podiums": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "event": {
        "id": "0af4a3a6-99f5-4aaf-bffa-3ace311df40d",
        "name": "GRAND PRIX DE FRANCE",
        "sponsored_name": "Monster Energy Grand Prix de France",
        "short_name": "FRA",
        "test": false,
        "season": "2016",
        ...
      }
    },
    ...
  ],
  "second_positions": {
    "categories": [
      {
        "category": {
          "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
          "legacy_id": 3,
          "name": "MotoGP™"
        },
        "count": 11
      },
      ...
    ],
    "total": 27
  },
  "world_championship_wins": {
    "categories": [
      {
        "category": {
          "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
          "legacy_id": 3,
          "name": "MotoGP™"
        },
        "count": 0
      },
      ...
    ],
    "total": 1
  },
  "best_positions": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "count": 1
    },
    ...
  ],
  "best_grid_positions": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "count": 1
    },
    ...
  ],
  "first_grand_prix_victories": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "event": {
        "id": "00a3ae01-6c18-4d92-ba15-b3a9bb89e660",
        "name": "BRITISH GRAND PRIX",
        "sponsored_name": "Octo British Grand Prix",
        "short_name": "GBR",
        "test": false,
        "season": "2016",
        ...
      }
    },
    ...
  ],
  "race_fastest_laps": {
    "categories": [
      {
        "category": {
          "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
          "legacy_id": 3,
          "name": "MotoGP™"
        },
        "count": 12
      },
      ...
    ],
    "total": 24
  },
  "best_qualify_positions": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "count": 1
    },
    ...
  ],
  "grand_prix_victories": {
    "categories": [
      {
        "category": {
          "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
          "legacy_id": 3,
          "name": "MotoGP™"
        },
        "count": 10
      },
      ...
    ],
    "total": 26
  },
  "all_races": {
    "categories": [
      {
        "category": {
          "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
          "legacy_id": 3,
          "name": "MotoGP™"
        },
        "count": 167
      },
      ...
    ],
    "total": 235
  },
  "first_race_fastest_lap": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "event": {
        "id": "4e410e6b-75ef-4180-ad4b-87669009245c",
        "name": "GRAN PREMI DE CATALUNYA",
        "sponsored_name": "Gran Premi Monster Energy de Catalunya",
        "short_name": "CAT",
        "test": false,
        "season": "2016",
        ...
      }
    },
    ...
  ],
  "first_pole_positions": [
    {
      "category": {
        "id": "e8c110ad-64aa-4e8e-8a86-f2f152f6a942",
        "legacy_id": 3,
        "name": "MotoGP™"
      },
      "event": {
        "id": "d1eb5ec0-d5f2-451f-8eba-34cf83ff725d",
        "name": "GRAND PRIX OF QATAR",
        "sponsored_name": "Grand Prix of Qatar",
        "short_name": "QAT",
        "test": false,
        "season": "2017",
        ...
      }
    },
    ...
  ]
}
```

### Get Rider Statistics by season
Retrieve rider statistics, summarised by season.

`GET` `/riders/{legacyId}/statistics`

`legacyId` integer **required**\
The Rider legacy ID.

#### Response
```json
[
  {
    "season": "2023",
    "category": "MotoGP\u2122",
    "constructor": "KTM",
    "starts": 12,
    "first_position": 0,
    "second_position": 2,
    "third_position": 1,
    "podiums": 3,
    "poles": 0,
    "points": 173,
    "position": 4
  },
  {
    "season": "2022",
    "category": "MotoGP\u2122",
    "constructor": "KTM",
    "starts": 20,
    "first_position": 0,
    "second_position": 3,
    "third_position": 0,
    "podiums": 3,
    "poles": 0,
    "points": 188,
    "position": 6
  },
  ...
]
```

### Get Teams
Retrieve the teams.

`GET` `/teams/categoryUuid={categoryId}&seasonYear={seasonYear}`

`categoryId` string **required**\
The broadcast category ID.

`seasonYear` integer **required**\
The season year. Valid values are `2023`, `2024`, etc.

#### Note
This is the only known way to get all riders from all seasons, as the `/riders` endpoint only has the riders from the current season.

#### Response
```json
[
  {
    "color": "#5cb33a",
    "riders": [
      {
        "id": "71df6f0d-51c3-4cdb-9f5c-51939e6f33f2",
        "name": "Maverick",
        "surname": "Viñales",
        ...
        "years_old": 28,
        "published": true,
        "legacy_id": 7409
      },
      {
        "id": "a1311c17-1099-4893-bba0-347bf43226a4",
        "name": "Lorenzo",
        "surname": "Savadori",
        ...
        "years_old": 30,
        "published": true,
        "legacy_id": 7246
      },
      {
        "id": "a2f51450-cb43-4d32-8eef-bda9ebb435ed",
        "name": "Aleix",
        "surname": "Espargaro",
        ...
        "years_old": 33,
        "published": true,
        "legacy_id": 6854
      }
    ],
    "constructor": {
      "id": "21fbe597-b587-42e1-b571-a06af5cc2487",
      "name": "Aprilia",
      "legacy_id": 7
    },
    "name": "Aprilia Racing",
    "legacy_id": 21,
    "id": "11d18b37-baba-400a-80c2-f8ddf040f97e",
    "text_color": "#ffffff",
    "published": true,
    "picture": "https://photos.motogp.com/teams/1/1/11d18b37-baba-400a-80c2-f8ddf040f97e/FrontalBike_0008_41-Aleix-Espargaro,-Bike_aleix.png"
  },
  ...
]
```
