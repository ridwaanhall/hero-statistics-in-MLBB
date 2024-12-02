# MLBB Hero Winrate and Counter

## Overview

This module provides classes to interact with an API that filters hero statistics based on various criteria. It includes the `FilterAPI` class for making API requests and the `FilterFactory` class to generate specific filter configurations. Additionally, it provides a function to create a table of hero data.

## Classes

### FilterAPI

A class to handle fetching data from a specified URL with given payload.

#### Methods

- **`__init__(self, url: str, payload: Dict)`**: Initializes the FilterAPI with the specified URL and payload.
  - `url`: The API endpoint to fetch data from.
  - `payload`: The payload to send in the POST request.

- **`fetch_data(self) -> Optional[Dict]`**: Sends a POST request to the URL with the payload and returns the JSON response.
  - Returns: JSON response as a dictionary if successful, otherwise `None`.

### FilterFactory

A class to create `FilterAPI` objects based on predefined filters.

#### Constants

- **`URLS`**: Dictionary mapping day filters to API URLs.
- **`RANKS`**: Dictionary mapping rank filters to their corresponding filter criteria.

#### Static Methods

- **`create_filter(day_filter: str, rank_filter: str, page_size: int, page_index: int, sorts_field: str, sorts_order: str) -> Optional[FilterAPI]`**: Creates a `FilterAPI` object based on provided filter parameters.
  - `day_filter`: The day filter (e.g., "1", "3", "7", "15", "30").
  - `rank_filter`: The rank filter (e.g., "all", "epic", "legend", "mythic", "honor", "glory").
  - `page_size`: The number of records per page.
  - `page_index`: The page index to retrieve.
  - `sorts_field`: The field to sort by (e.g., "main_hero_appearance_rate", "main_hero_win_rate", "main_hero_ban_rate").
  - `sorts_order`: The sort order ("asc" or "desc").
  - Returns: A `FilterAPI` object or `None` if the filters are invalid.

## Functions

### `create_hero_table(data: Dict, rate_filter: Optional[Dict[str, float]] = None) -> PrettyTable`

Creates a table of hero data from the API response.

- `data`: The JSON response data from the API.
- `rate_filter`: Optional dictionary to filter heroes based on pick rate, win rate, or ban rate.
- Returns: A `PrettyTable` object with hero statistics.

## Example Usage

### Payload

```python
def main():
    day_filter = "7"  # 1 day, 3 days, 7 days, 15 days, 30 days
    rank_filter = "honor"  # all, epic, legend, mythic, honor, glory
    page_size = 126  # 1 - 126
    page_index = 1
    type_rate, rate = "win_rate", 0
    sorts_field, sorts_order = "main_heroid", "desc"

    filter_factory = FilterFactory.create_filter(day_filter, rank_filter, page_size, page_index, sorts_field, sorts_order)

    if filter_factory:
        data = filter_factory.fetch_data()
        if data:
            table = create_hero_table(data, {type_rate: rate})
            print(table)
        else:
            print("Failed to retrieve data.")
    else:
        print("Invalid filter type.")

if __name__ == "__main__":
    main()
```

### Output

| Hero ID | Hero Name | Pick Rate | Win Rate | Ban Rate | Counter Hero |
|---------|-----------|-----------|----------|----------|--------------|
| 126 | Suyou | 0.75% | 48.91% | 46.08% | Baxia,Karina,Sun,Belerick,Lunox |
| 125 | Zhuxin | 0.65% | 51.51% | 73.54% | Lolita,Alice,Barats,Aulus,Chang'e |
| 124 | Chip | 0.19% | 47.97% | 15.18% | Silvanna,Helcurt,Kimmy,Zilong,Eudora |
| 123 | Cici | 0.56% | 48.89% | 0.62% | Balmond,Hylos,Thamuz,Aldous,Akai |
| 122 | Nolan | 0.90% | 47.16% | 5.99% | Estes,Bane,Hanzo,Floryn,Aulus |
| 121 | Ixia | 0.53% | 50.66% | 0.38% | Estes,Lolita,Rafaela,Minotaur,Popol and Kupa |
| 120 | Arlott | 0.58% | 47.35% | 0.67% | X.Borg,Cici,Zhuxin,Esmeralda,Ruby |
| 119 | Novaria | 0.22% | 45.26% | 0.10% | Barats,Carmilla,Yve,Ixia,Atlas |
| 118 | Joy | 0.58% | 47.70% | 39.17% | X.Borg,Lolita,Lylia,Atlas,Grock |
| 117 | Fredrinn | 0.24% | 48.79% | 0.26% | Eudora,Alucard,Saber,Silvanna,Masha |
| 116 | Julian | 0.92% | 51.51% | 3.79% | Faramis,Esmeralda,Angela,Estes,X.Borg |
| 115 | Xavier | 1.23% | 50.36% | 0.78% | Gloo,Barats,Belerick,Baxia,Hylos |
| 114 | Melissa | 0.93% | 53.81% | 1.58% | Lolita,Sun,Alucard,Popol and Kupa,Karina |
| 113 | Yin | 0.87% | 51.03% | 6.16% | Floryn,Estes,Angela,Diggie,Hanzo |
| 112 | Floryn | 0.45% | 54.60% | 2.83% | X.Borg,Alice,Faramis,Lesley,Diggie |
| 111 | Edith | 0.52% | 52.02% | 0.26% | Eudora,Odette,Johnson,Claude,Faramis |
| 110 | Valentina | 0.26% | 42.88% | 0.15% | Atlas,Carmilla,X.Borg,Lolita,Grock |
| 109 | Aamon | 0.49% | 50.17% | 0.60% | Cici,Grock,Zilong,Diggie,Uranus |
| 108 | Aulus | 0.23% | 52.44% | 0.09% | Sun,Faramis,Lolita,Aldous,Fanny |
| 107 | Natan | 0.60% | 48.51% | 0.59% | Sun,Atlas,Baxia,Uranus,Johnson |
| 106 | Phoveus | 0.84% | 51.05% | 28.69% | Wanwan,Valentina,Harith,Ruby,Benedetta |
| 105 | Beatrix | 0.81% | 46.28% | 0.50% | Estes,Barats,Ixia,Miya,Minotaur |
| 3 | Saber | 0.80% | 51.05% | 64.98% | Natalia,Nolan,Benedetta,Lancelot,Xavier |
| 2 | Balmond | 0.32% | 44.18% | 0.14% | Barats,Eudora,Saber,Fredrinn,Martis |
| 1 | Miya | 2.95% | 54.56% | 30.23% | Saber,Masha,Selena,Guinevere,Jawhead |

# Hero Best With, Strong Against, Weak Against

This module provides classes to interact with an API that filters hero statistics based on various criteria. It includes the `HeroFilterAPI` class for making API requests and the `HeroFilterFactory` class to generate specific filter configurations. Additionally, it provides a function to create a table of hero data.

## Classes

### HeroFilterAPI

A class to handle fetching data from a specified URL with a given payload.

#### Methods

- **`__init__(self, url: str, payload: Dict)`**: Initializes the HeroFilterAPI with the specified URL and payload.
  - `url`: The API endpoint to fetch data from.
  - `payload`: The payload to send in the POST request.

- **`fetch_data(self) -> Optional[Dict]`**: Sends a POST request to the URL with the payload and returns the JSON response.
  - Returns: JSON response as a dictionary if successful, otherwise `None`.

### HeroFilterFactory

A class to create `HeroFilterAPI` objects based on predefined filters.

#### Static Methods

- **`hero_create_filter(sortid_values: List[int], roadsort_values: List[int], page_size: int, page_index: int, sorts_field: str, sorts_order: str) -> FilterAPI`**: Creates a `FilterAPI` object based on provided filter parameters.
  - `sortid_values`: List of integers representing sort IDs.
  - `roadsort_values`: List of integers representing roadsort values.
  - `page_size`: The number of records per page.
  - `page_index`: The page index to retrieve.
  - `sorts_field`: The field to sort by.
  - `sorts_order`: The sort order ("asc" or "desc").
  - Returns: A `FilterAPI` object.

## Functions

### `hero_create_hero_table(data: Dict) -> PrettyTable`

Creates a table of hero data from the API response.

- `data`: The JSON response data from the API.
- Returns: A `PrettyTable` object with hero statistics.

### `hero_main()`

Main function to create a filter, fetch data, and print a table of hero statistics.

## Example Usage

### Payload

```python
def hero_main():
    sortid_values = [1, 2, 3, 4, 5, 6]
    roadsort_values = [1, 2, 3, 4, 5]
    page_size = 126
    page_index = 1
    sorts_field, sorts_order = "heroid", "desc"

    hero_filter_factory = HeroFilterFactory.hero_create_filter(sortid_values, roadsort_values, page_size, page_index, sorts_field, sorts_order)

    data = hero_filter_factory.fetch_data()
    if data:
        table = hero_create_hero_table(data)
        print(table)
    else:
        print("Failed to retrieve data.")

if __name__ == "__main__":
    hero_main()
```

### Output

+---------+----------------+-------------------------------+-----------------------------------------+----------------------------------------+----------------+---------------------+-------------------+
| Hero ID | Hero Name      |           Best With           |              Strong Against             |              Weak Against              | Desc Best With | Desc Strong Against | Desc Weak Against |
+---------+----------------+-------------------------------+-----------------------------------------+----------------------------------------+----------------+---------------------+-------------------+
|     126 | Suyou          |        Rafaela, Tigreal       |               Ixia, Xavier              |            Melissa, Badang             | [14, 6]        | [121, 115]          | [114, 77]         |
|     125 | Zhuxin         |     Cici, Kimmy, Belerick     |         Cyclops, Popol and Kupa         |              Saber, Fanny              | [123, 71, 70]  | [33, 94]            | [3, 17]           |
|      61 | Chang'e        |  Gatotkaca, Tigreal, Unknown  |     Eudora, Odette, Unknown, Unknown    |    Hanzo, Harley, Unknown, Unknown     | [41, 6, 0]     | [15, 46, 0, 0]      | [69, 42, 0, 0]    |
|     104 | Gloo           |     Eudora, Saber, Unknown    |   Cecilion, Chang'e, Unknown, Unknown   |    Karrie, Lesley, Unknown, Unknown    | [15, 3, 0]     | [91, 61, 0, 0]      | [40, 53, 0, 0]    |
|       2 | Balmond        |    Angela, Rafaela, Unknown   |          Layla, Eudora, Unknown         |        Lesley, Irithel, Unknown        | [55, 14, 0]    | [18, 15, 0]         | [53, 43, 0]       |
|       1 | Miya           |   Tigreal, Belerick, Unknown  |            Saber, Eudora, Sun           |          Pharsa, Yve, Unknown          | [6, 70, 0]     | [3, 15, 27]         | [52, 101, 0]      |
|       4 | Alice          |   Balmond, Dyrroth, Unknown   |        Cecilion, Chang'e, Unknown       |         Baxia, Martis, Unknown         | [2, 85, 0]     | [91, 61, 0]         | [87, 58, 0]       |
|       5 | Nana           |    Rafaela, Franco, Unknown   |         Eudora, Kadita, Unknown         |        Zilong, Natalia, Unknown        | [14, 10, 0]    | [15, 75, 0]         | [16, 24, 0]       |
|       6 | Tigreal        |    Vexana, Odette, Unknown    |          Zilong, Freya, Unknown         |        Chang'e, Zhask, Unknown         | [38, 46, 0]    | [16, 22, 0]         | [61, 50, 0]       |
|       7 | Alucard        |   Gatotkaca, Xavier, Unknown  |          Harley, Aamon, Unknown         |        Franco, Johnson, Unknown        | [41, 115, 0]   | [42, 109, 0]        | [10, 32, 0]       |
|       8 | Karina         |   Tigreal, Faramis, Unknown   |          Layla, Hanabi, Unknown         |         Akai, Khufra, Unknown          | [6, 76, 0]     | [18, 60, 0]         | [9, 78, 0]        |
|       9 | Akai           |     Saber, Eudora, Unknown    |         Eudora, Odette, Unknown         |         Nana, Diggie, Unknown          | [3, 15, 0]     | [15, 46, 0]         | [5, 48, 0]        |
|      12 | Bruno          |         Franco, Khufra        |               Hanabi, Miya              |          Gatotkaca, Fredrinn           | [10, 78]       | [60, 1]             | [41, 117]         |
|      11 | Bane           |        Tigreal, Luo Yi        |             Hylos, Belerick             |              Vale, Xavier              | [6, 96]        | [49, 70]            | [66, 115]         |
|      13 | Clint          |        Rafaela, Angela        |               Moskov, Miya              |            Melissa, Karrie             | [14, 55]       | [31, 1]             | [114, 40]         |
|      17 | Fanny          |      Minotaur, Gatotkaca      |             Karrie, Irithel             |              Kaja, Khufra              | [19, 41]       | [40, 43]            | [62, 78]          |
|      18 | Layla          |      Minsitthar, Belerick     |            Tigreal, Belerick            |              Karina, Ling              | [74, 70]       | [6, 70]             | [8, 84]           |
|      19 | Minotaur       |        Granger, Melissa       |             Paquito, Julian             |             Layla, Lesley              | [79, 114]      | [103, 116]          | [18, 53]          |
|      20 | Lolita         |         Hanabi, Layla         |              Kimmy, Cyclops             |             Ling, Paquito              | [60, 18]       | [71, 33]            | [84, 103]         |
|      22 | Freya          |      Belerick, Gatotkaca      |               Uranus, Gloo              |           Guinevere, Martis            | [70, 41]       | [59, 104]           | [80, 58]          |
|      23 | Gord           |        Hylos, Belerick        |             Barats, Fredrinn            |            Fanny, Lancelot             | [49, 70]       | [99, 117]           | [17, 47]          |
|      25 | Kagura         |         Atlas, Tigreal        |              Layla, Eudora              |              Joy, Martis               | [93, 6]        | [18, 15]            | [118, 58]         |
|      26 | Chou           |        Angela, Rafaela        |              Terizla, Alpha             |             Lesley, Pharsa             | [55, 14]       | [82, 28]            | [53, 52]          |
|      27 | Sun            |   Belerick, Tigreal, Unknown  |   Franco, Gatotkaca, Unknown, Unknown   |    Ruby, Unknown, Unknown, Unknown     | [70, 6, 0]     | [10, 41, 0, 0]      | [29, 0, 0, 0]     |
|      28 | Alpha          |    Vexana, Selena, Unknown    |      Layla, Ixia, Unknown, Unknown      |     Chou, Gusion, Unknown, Unknown     | [38, 63, 0]    | [18, 121, 0, 0]     | [26, 56, 0, 0]    |
|      29 | Ruby           |    Pharsa, Kadita, Unknown    |   Aldous, Guinevere, Unknown, Unknown   |   Wanwan, Irithel, Unknown, Unknown    | [52, 75, 0]    | [64, 80, 0, 0]      | [89, 43, 0, 0]    |
|      30 | Yi Sun-shin    |    Alpha, Khaleed, Unknown    |    Uranus, Terizla, Unknown, Unknown    |     Franco, Kaja, Unknown, Unknown     | [28, 98, 0]    | [59, 82, 0, 0]      | [10, 62, 0, 0]    |
|      31 | Moskov         |  Fredrinn, Gatotkaca, Unknown |    Thamuz, Balmond, Unknown, Unknown    |  Lancelot, Natalia, Unknown, Unknown   | [117, 41, 0]   | [72, 2, 0, 0]       | [47, 24, 0, 0]    |
|      32 | Johnson        |    Odette, Kadita, Unknown    |    Alucard, Zilong, Unknown, Unknown    |    Harith, Kagura, Unknown, Unknown    | [46, 75, 0]    | [7, 16, 0, 0]       | [73, 25, 0, 0]    |
|      34 | Estes          |    Lesley, Hanabi, Unknown    |      Valir, Alice, Unknown, Unknown     |    Saber, Eudora, Unknown, Unknown     | [53, 60, 0]    | [57, 4, 0, 0]       | [3, 15, 0, 0]     |
|      35 | Hilda          |    Rafaela, Franco, Unknown   |     Zilong, Layla, Unknown, Unknown     |    Thamuz, Uranus, Unknown, Unknown    | [14, 10, 0]    | [16, 18, 0, 0]      | [72, 59, 0, 0]    |
|      37 | Lapu-Lapu      |    Tigreal, Atlas, Unknown    |     Layla, Eudora, Unknown, Unknown     |    Lesley, Moskov, Unknown, Unknown    | [6, 93, 0]     | [18, 15, 0, 0]      | [53, 31, 0, 0]    |
|      38 | Vexana         |    Atlas, Beatrix, Unknown    |     Layla, Irithel, Xavier, Unknown     |     Saber, Aamon, Karina, Unknown      | [93, 105, 0]   | [18, 43, 115, 0]    | [3, 109, 8, 0]    |
|      39 | Roger          |    Pharsa, Vexana, Unknown    |     Hanabi, Odette, Unknown, Unknown    |  Minsitthar, Khufra, Unknown, Unknown  | [52, 38, 0]    | [60, 46, 0, 0]      | [74, 78, 0, 0]    |
|      40 | Karrie         |     Saber, Aamon, Unknown     |   Tigreal, Fredrinn, Unknown, Unknown   |      Xavier, Layla, Nana, Unknown      | [3, 109, 0]    | [6, 117, 0, 0]      | [115, 18, 5, 0]   |
|      41 | Gatotkaca      |     Kagura, Vale, Unknown     | Layla, Popol and Kupa, Unknown, Unknown |    Diggie, Wanwan, Unknown, Unknown    | [25, 66, 0]    | [18, 94, 0, 0]      | [48, 89, 0, 0]    |
|      42 | Harley         |     Akai, Khufra, Unknown     |     Cecilion, Ixia, Unknown, Unknown    |    Lolita, Uranus, Unknown, Unknown    | [9, 78, 0]     | [91, 121, 0, 0]     | [20, 59, 0, 0]    |
|      43 | Irithel        |     Valir, Hylos, Unknown     |      Miya, Alpha, Unknown, Unknown      |   Badang, Silvanna, Unknown, Unknown   | [57, 49, 0]    | [1, 28, 0, 0]       | [77, 90, 0, 0]    |
|      44 | Grock          |    Moskov, Barats, Unknown    |   Terizla, Lapu-Lapu, Unknown, Unknown  |  Novaria, Valentina, Unknown, Unknown  | [31, 99, 0]    | [82, 37, 0, 0]      | [119, 110, 0, 0]  |
|      45 | Argus          |    Angela, Rafaela, Unknown   |      Gord, Aurora, Unknown, Unknown     |  Gatotkaca, Tigreal, Unknown, Unknown  | [55, 14, 0]    | [23, 36, 0, 0]      | [41, 6, 0, 0]     |
|      46 | Odette         |   Johnson, Jawhead, Unknown   |     Layla, Irithel, Unknown, Unknown    |   Minotaur, Edith, Unknown, Unknown    | [32, 54, 0]    | [18, 43, 0, 0]      | [19, 111, 0, 0]   |
|      47 | Lancelot       |    Atlas, Tigreal, Unknown    |    Cecilion, Pharsa, Unknown, Unknown   | Minsitthar, Phoveus, Unknown, Unknown  | [93, 6, 0]     | [91, 52, 0, 0]      | [74, 106, 0, 0]   |
|      48 | Diggie         |      Layla, Miya, Unknown     |     Atlas, Lolita, Unknown, Unknown     |     Ling, Aamon, Unknown, Unknown      | [18, 1, 0]     | [93, 20, 0, 0]      | [84, 109, 0, 0]   |
|      50 | Zhask          |   Belerick, Lolita, Unknown   |    Aldous, Paquito, Unknown, Unknown    |   Pharsa, Cecilion, Unknown, Unknown   | [70, 20, 0]    | [64, 103, 0, 0]     | [52, 91, 0, 0]    |
|      51 | Helcurt        |  Tigreal, Esmeralda, Terizla  |      Layla, Chang'e, Vale, Unknown      | Fredrinn, Belerick, Gatotkaca, Unknown | [6, 81, 82]    | [18, 61, 66, 0]     | [117, 70, 41, 0]  |
|      52 | Pharsa         |     Atlas, Franco, Unknown    |      Layla, Hanabi, Xavier, Unknown     |  Franco, Zilong, Minsitthar, Unknown   | [93, 10, 0]    | [18, 60, 115, 0]    | [10, 16, 74, 0]   |
|      53 | Lesley         |    Minotaur, Baxia, Unknown   |  Gatotkaca, Fredrinn, Unknown, Unknown  |    Ling, Lancelot, Unknown, Unknown    | [19, 87, 0]    | [41, 117, 0, 0]     | [84, 47, 0, 0]    |
|      54 | Jawhead        |   Odette, Belerick, Unknown   |     Saber, Eudora, Unknown, Unknown     |    Edith, Thamuz, Unknown, Unknown     | [46, 70, 0]    | [3, 15, 0, 0]       | [111, 72, 0, 0]   |
|      55 | Angela         |    Lancelot, Aamon, Unknown   |    Uranus, Belerick, Unknown, Unknown   |     Ling, Harley, Unknown, Unknown     | [47, 109, 0]   | [59, 70, 0, 0]      | [84, 42, 0, 0]    |
|      56 | Gusion         |    Vexana, Pharsa, Unknown    |     Layla, Hanabi, Unknown, Unknown     |     Franco, Kaja, Unknown, Unknown     | [38, 52, 0]    | [18, 60, 0, 0]      | [10, 62, 0, 0]    |
|      58 | Martis         |    Alpha, Khaleed, Unknown    |     Layla, Odette, Unknown, Unknown     |   Thamuz, Fredrinn, Unknown, Unknown   | [28, 98, 0]    | [18, 46, 0, 0]      | [72, 117, 0, 0]   |
|      59 | Uranus         |    Beatrix, Hanabi, Unknown   |    Tigreal, Franco, Unknown, Unknown    |    Layla, Lesley, Unknown, Unknown     | [105, 60, 0]   | [6, 10, 0, 0]       | [18, 53, 0, 0]    |
|      60 | Hanabi         |    Carmilla, Atlas, Unknown   |   Tigreal, Belerick, Unknown, Unknown   |   Lancelot, Zilong, Unknown, Unknown   | [92, 93, 0]    | [6, 70, 0, 0]       | [47, 16, 0, 0]    |
|      62 | Kaja           |     Eudora, Aamon, Unknown    |    Fanny, Lancelot, Unknown, Unknown    |    Layla, Pharsa, Unknown, Unknown     | [15, 109, 0]   | [17, 47, 0, 0]      | [18, 52, 0, 0]    |
|      63 | Selena         |    Xavier, Beatrix, Unknown   |      Gord, Layla, Unknown, Unknown      |    Hanabi, Wanwan, Unknown, Unknown    | [115, 105, 0]  | [23, 18, 0, 0]      | [60, 89, 0, 0]    |
|      64 | Aldous         |    Balmond, Bruno, Unknown    |    Zilong, Leomord, Unknown, Unknown    |   Martis, Dyrroth, Unknown, Unknown    | [2, 12, 0]     | [16, 67, 0, 0]      | [58, 85, 0, 0]    |
|      65 | Claude         |    Angela, Faramis, Unknown   |     Vexana, Floryn, Unknown, Unknown    |    Eudora, Franco, Unknown, Unknown    | [55, 76, 0]    | [38, 112, 0, 0]     | [15, 10, 0, 0]    |
|      66 | Vale           |   Lolita, Belerick, Unknown   |     Terizla, Hilda, Unknown, Unknown    |    Hayabusa, Ling, Unknown, Unknown    | [20, 70, 0]    | [82, 35, 0, 0]      | [21, 84, 0, 0]    |
|      67 | Leomord        |    Pharsa, Vexana, Unknown    |      Odette, Pharsa, Yve, Cecilion      |      Phoveus, Franco, Akai, Nana       | [52, 38, 0]    | [46, 52, 101, 91]   | [106, 10, 9, 5]   |
|      68 | Lunox          |   Franco, Minotaur, Unknown   |       Yve, Lylia, Unknown, Unknown      |     Nolan, Ling, Unknown, Unknown      | [10, 19, 0]    | [101, 86, 0, 0]     | [122, 84, 0, 0]   |
|      69 | Hanzo          |    Atlas, Tigreal, Unknown    |      Xavier, Ixia, Unknown, Unknown     |     Ling, Fanny, Unknown, Unknown      | [93, 6, 0]     | [115, 121, 0, 0]    | [84, 17, 0, 0]    |
|      70 | Belerick       |      Natan, Miya, Unknown     |     Lancelot, Ling, Unknown, Unknown    |    Valir, Xavier, Unknown, Unknown     | [107, 1, 0]    | [47, 84, 0, 0]      | [57, 115, 0, 0]   |
|      71 | Kimmy          |    Rafaela, Angela, Unknown   |    Hylos, Carmilla, Unknown, Unknown    |    Natalia, Aamon, Unknown, Unknown    | [14, 55, 0]    | [49, 92, 0, 0]      | [24, 109, 0, 0]   |
|      72 | Thamuz         |    Angela, Rafaela, Unknown   |      Zilong, Sun, Unknown, Unknown      |   Karrie, Irithel, Unknown, Unknown    | [55, 14, 0]    | [16, 27, 0, 0]      | [40, 43, 0, 0]    |
|      73 | Harith         | Minsitthar, Minotaur, Unknown |   Belerick, Johnson, Unknown, Unknown   |   Novaria, Pharsa, Unknown, Unknown    | [74, 19, 0]    | [70, 32, 0, 0]      | [119, 52, 0, 0]   |
|      75 | Kadita         |    Atlas, Tigreal, Unknown    |     Aurora, Eudora, Unknown, Unknown    |    Clint, Beatrix, Unknown, Unknown    | [93, 6, 0]     | [36, 15, 0, 0]      | [13, 105, 0, 0]   |
|      76 | Faramis        |    Leomord, Martis, Unknown   |     Saber, Eudora, Unknown, Unknown     |   Novaria, Pharsa, Unknown, Unknown    | [67, 58, 0]    | [3, 15, 0, 0]       | [119, 52, 0, 0]   |
|      77 | Badang         |    Tigreal, Luo Yi, Unknown   |     Layla, Odette, Unknown, Unknown     |     Wanwan, Ling, Unknown, Unknown     | [6, 96, 0]     | [18, 46, 0, 0]      | [89, 84, 0, 0]    |
|      78 | Khufra         |    Eudora, Pharsa, Unknown    |     Joy, Lancelot, Unknown, Unknown     |    Fredrinn, Gloo, Unknown, Unknown    | [15, 52, 0]    | [118, 47, 0, 0]     | [117, 104, 0, 0]  |
|      79 | Granger        |     Franco, Kaja, Unknown     |   Chang'e, Cecilion, Unknown, Unknown   |   Saber, Hayabusa, Unknown, Unknown    | [10, 62, 0]    | [61, 91, 0, 0]      | [3, 21, 0, 0]     |
|      80 | Guinevere      |     Franco, Hylos, Unknown    |    Zilong, Silvanna, Unknown, Unknown   |     Badang, Chou, Unknown, Unknown     | [10, 49, 0]    | [16, 90, 0, 0]      | [77, 26, 0, 0]    |
|      81 | Esmeralda      |    Atlas, Tigreal, Unknown    |      Akai, Lolita, Unknown, Unknown     |    Karrie, Lesley, Unknown, Unknown    | [93, 6, 0]     | [9, 20, 0, 0]       | [40, 53, 0, 0]    |
|      82 | Terizla        |   Gatotkaca, Atlas, Unknown   |      Zilong, Sun, Unknown, Unknown      |    Arlott, Lesley, Unknown, Unknown    | [41, 93, 0]    | [16, 27, 0, 0]      | [120, 53, 0, 0]   |
|      84 | Ling           |    Angela, Khufra, Unknown    |     Layla, Hanabi, Unknown, Unknown     |   Eudora, Jawhead, Unknown, Unknown    | [55, 78, 0]    | [18, 60, 0, 0]      | [15, 54, 0, 0]    |
|      85 | Dyrroth        |     Franco, Kaja, Unknown     |      Gord, Hanabi, Unknown, Unknown     |   Vexana, Belerick, Unknown, Unknown   | [10, 62, 0]    | [23, 60, 0, 0]      | [38, 70, 0, 0]    |
|      86 | Lylia          |   Minotaur, Franco, Unknown   |    Carmilla, Hylos, Unknown, Unknown    |     Nolan, Ling, Unknown, Unknown      | [19, 10, 0]    | [92, 49, 0, 0]      | [122, 84, 0, 0]   |
|      87 | Baxia          |     Thamuz, Alpha, Unknown    |   Chang'e, Silvanna, Unknown, Unknown   |    Karrie, Lesley, Unknown, Unknown    | [72, 28, 0]    | [61, 90, 0, 0]      | [40, 53, 0, 0]    |
|      88 | Masha          |    Angela, Rafaela, Unknown   |      Ixia, Layla, Unknown, Unknown      |     Cici, Thamuz, Unknown, Unknown     | [55, 14, 0]    | [121, 18, 0, 0]     | [123, 72, 0, 0]   |
|      89 | Wanwan         |   Franco, Belerick, Unknown   |    Fredrinn, Luo Yi, Unknown, Unknown   |    Lancelot, Gord, Unknown, Unknown    | [10, 70, 0]    | [117, 96, 0, 0]     | [47, 23, 0, 0]    |
|      90 | Silvanna       |    Atlas, Tigreal, Unknown    |      Layla, Valir, Unknown, Unknown     |   Martis, Unknown, Unknown, Unknown    | [93, 6, 0]     | [18, 57, 0, 0]      | [58, 0, 0, 0]     |
|      91 | Cecilion       |  Carmilla, Minotaur, Unknown  |    Balmond, Terizla, Unknown, Unknown   |     Ling, Fanny, Unknown, Unknown      | [92, 19, 0]    | [2, 82, 0, 0]       | [84, 17, 0, 0]    |
|      92 | Carmilla       |   Cecilion, Aurora, Unknown   |   Dyrroth, Lapu-Lapu, Unknown, Unknown  |    Hanabi, Wanwan, Unknown, Unknown    | [91, 36, 0]    | [85, 37, 0, 0]      | [60, 89, 0, 0]    |
|      93 | Atlas          |      Vale, Ixia, Unknown      |     Vexana, Eudora, Unknown, Unknown    |    Valir, Moskov, Unknown, Unknown     | [66, 121, 0]   | [38, 15, 0, 0]      | [57, 31, 0, 0]    |
|      94 | Popol and Kupa |    Alpha, Khaleed, Unknown    |     Lesley, Brody, Unknown, Unknown     |    Gusion, Karina, Unknown, Unknown    | [28, 98, 0]    | [53, 100, 0, 0]     | [56, 8, 0, 0]     |
|      95 | Yu Zhong       |    Vexana, Xavier, Unknown    |     Aulus, Zilong, Unknown, Unknown     |    Lesley, Karrie, Unknown, Unknown    | [38, 115, 0]   | [108, 16, 0, 0]     | [53, 40, 0, 0]    |
|      96 | Luo Yi         |     Aamon, Karina, Unknown    |      Eudora, Gord, Unknown, Unknown     |   Uranus, Fredrinn, Unknown, Unknown   | [109, 8, 0]    | [15, 23, 0, 0]      | [59, 117, 0, 0]   |
|      97 | Benedetta      |  Johnson, Gatotkaca, Unknown  |   Jawhead, Guinevere, Unknown, Unknown  | Minsitthar, Phoveus, Unknown, Unknown  | [32, 41, 0]    | [54, 80, 0, 0]      | [74, 106, 0, 0]   |
|      98 | Khaleed        |     Roger, Karina, Unknown    |    Cyclops, Xavier, Unknown, Unknown    |     X.Borg, Chou, Unknown, Unknown     | [39, 8, 0]     | [33, 115, 0, 0]     | [83, 26, 0, 0]    |
|      99 | Barats         |   Mathilda, Angela, Unknown   |    Belerick, Lolita, Unknown, Unknown   |     Akai, Moskov, Unknown, Unknown     | [102, 55, 0]   | [70, 20, 0, 0]      | [9, 31, 0, 0]     |
|     100 | Brody          |   Hylos, Minsitthar, Unknown  |       Aamon, Yin, Unknown, Unknown      |    Martis, Wanwan, Unknown, Unknown    | [49, 74, 0]    | [109, 113, 0, 0]    | [58, 89, 0, 0]    |
|     101 | Yve            |   Minotaur, Gatotkaca, Atlas  |         Fredrinn, Barats, Aulus         |           Yin, Saber, Karina           | [19, 41, 93]   | [117, 99, 108]      | [113, 3, 8]       |
|      74 | Minsitthar     |    Melissa, Hanabi, Unknown   |   Benedetta, Arlott, Unknown, Unknown   |    Zilong, Aulus, Unknown, Unknown     | [114, 60, 0]   | [97, 120, 0, 0]     | [16, 108, 0, 0]   |
|     102 | Mathilda       |     Saber, Eudora, Unknown    |       Gord, Vale, Unknown, Unknown      |  Franco, Minsitthar, Unknown, Unknown  | [3, 15, 0]     | [23, 66, 0, 0]      | [10, 74, 0, 0]    |
|     105 | Beatrix        |     Khufra, Atlas, Unknown    |    Hylos, Belerick, Unknown, Unknown    |     Eudora, Ling, Unknown, Unknown     | [78, 93, 0]    | [49, 70, 0, 0]      | [15, 84, 0, 0]    |
|     106 | Phoveus        |  Gatotkaca, Minotaur, Tigreal |           Wanwan, Arlott, Chou          |         Aurora, Valir, Vexana          | [41, 19, 6]    | [89, 120, 26]       | [36, 57, 38]      |
|     107 | Natan          |    Johnson, Alpha, Unknown    |       Tigreal, Gatotkaca, Unknown       |         Saber, Eudora, Unknown         | [32, 28, 0]    | [6, 41, 0]          | [3, 15, 0]        |
|     108 | Aulus          |      Angela, Yve, Unknown     |            Hanabi, Yve, Gord            |        Franco, Wanwan, Unknown         | [55, 101, 0]   | [60, 101, 23]       | [10, 89, 0]       |
|     109 | Aamon          |    Rafaela, Floryn, Unknown   |        Xavier, Cecilion, Unknown        |       Balmond, Fredrinn, Unknown       | [14, 112, 0]   | [115, 91, 0]        | [2, 117, 0]       |
|     111 | Edith          |    Minotaur, Atlas, Unknown   |          Zilong, Aulus, Unknown         |        Hanabi, Wanwan, Unknown         | [19, 93, 0]    | [16, 108, 0]        | [60, 89, 0]       |
|     110 | Valentina      |    Carmilla, Atlas, Unknown   |         Hanabi, Faramis, Unknown        |        Hayabusa, Natalia, Hanzo        | [92, 93, 0]    | [60, 76, 0]         | [21, 24, 69]      |
|     112 | Floryn         |    Terizla, Thamuz, Unknown   |         Uranus, Lolita, Unknown         |         Karina, Saber, Unknown         | [82, 72, 0]    | [59, 20, 0]         | [8, 3, 0]         |
|     113 | Yin            |   Mathilda, Rafaela, Unknown  |         Layla, Cecilion, Unknown        |       Fredrinn, Badang, Unknown        | [102, 14, 0]   | [18, 91, 0]         | [117, 77, 0]      |
|     114 | Melissa        |   Tigreal, Minotaur, Unknown  |           Sun, Zilong, Unknown          |         Xavier, Nana, Unknown          | [6, 19, 0]     | [27, 16, 0]         | [115, 5, 0]       |
|     115 | Xavier         |    Carmilla, Atlas, Unknown   |         Hanabi, Faramis, Unknown        |      Benedetta, Hayabusa, Natalia      | [92, 93, 0]    | [60, 76, 0]         | [97, 21, 24]      |
|     116 | Julian         |    Angela, Diggie, Unknown    |        Cecilion, Aldous, Unknown        |              Joy, Harith               | [55, 48, 0]    | [91, 64, 0]         | [118, 73]         |
|     117 | Fredrinn       |    Angela, Floryn, Unknown    |          Vale, Eudora, Unknown          |             Karrie, Lesley             | [55, 112, 0]   | [66, 15, 0]         | [40, 53]          |
|     118 | Joy            |     Angela, Hylos, Unknown    |         Xavier, Vexana, Unknown         |           Minsitthar, Khufra           | [55, 49, 0]    | [115, 38, 0]        | [74, 78]          |
|     120 | Arlott         |   Gatotkaca, Atlas, Unknown   |           Sun, Zilong, Unknown          |            Thamuz, Fredrinn            | [41, 93, 0]    | [27, 16, 0]         | [72, 117]         |
|     119 | Novaria        |    Beatrix, Layla, Unknown    |         Eudora, Odette, Unknown         |            Saber, Hayabusa             | [105, 18, 0]   | [15, 46, 0]         | [3, 21]           |
|     121 | Ixia           |    Fredrinn, Bane, Unknown    |         Fredrinn, Bane, Unknown         |              Saber, Aamon              | [117, 11, 0]   | [117, 11, 0]        | [3, 109]          |
|     124 | Chip           |     Alpha, Hanabi, Kadita     |           Vexana, Layla, Ixia           |            Barats, Tigreal             | [28, 60, 75]   | [38, 18, 121]       | [99, 6]           |
|     123 | Cici           |    Angela, Floryn, Unknown    |       Fredrinn, Minotaur, Unknown       |              Saber, Kaja               | [55, 112, 0]   | [117, 19, 0]        | [3, 62]           |
|       3 | Saber          |    Kadita, Cyclops, Unknown   |          Layla, Eudora, Unknown         |        Estes, Faramis, Unknown         | [75, 33, 0]    | [18, 15, 0]         | [34, 76, 0]       |
|      83 | X.Borg         |    Karina, Martis, Unknown    |     Terizla, Aulus, Unknown, Unknown    |   Martis, Dyrroth, Unknown, Unknown    | [8, 58, 0]     | [82, 108, 0, 0]     | [58, 85, 0, 0]    |
|      36 | Aurora         |      Ixia, Freya, Karrie      |    Leomord, Thamuz, Terizla, Unknown    |    Ling, Kadita, Guinevere, Unknown    | [121, 22, 40]  | [67, 72, 82, 0]     | [84, 75, 80, 0]   |
|      49 | Hylos          |  Terizla, Minsitthar, Unknown |      Layla, Gord, Unknown, Unknown      |    Karrie, Lesley, Unknown, Unknown    | [82, 74, 0]    | [18, 23, 0, 0]      | [40, 53, 0, 0]    |
|      57 | Valir          |   Khaleed, Yu Zhong, Unknown  |   Balmond, Fredrinn, Unknown, Unknown   |   Lancelot, Fanny, Unknown, Unknown    | [98, 95, 0]    | [2, 117, 0, 0]      | [47, 17, 0, 0]    |
|      10 | Franco         |      Vale, Karina, Brody      |            Vale, Estes, Layla           |         Wanwan, Fanny, Badang          | [66, 8, 100]   | [66, 34, 18]        | [89, 17, 77]      |
|      14 | Rafaela        |        Freya, Lapu-Lapu       |              Masha, Barats              |             Ling, Hayabusa             | [22, 37]       | [88, 99]            | [84, 21]          |
|      15 | Eudora         |        Johnson, Jawhead       |              Wanwan, Moskov             |       Fredrinn, Unknown, Unknown       | [32, 54]       | [89, 31]            | [117, 0, 0]       |
|      16 | Zilong         |         Pharsa, Hanabi        |               Layla, Lylia              |              Franco, Nana              | [52, 60]       | [18, 86]            | [10, 5]           |
|      21 | Hayabusa       |        Lapu-Lapu, Alpha       |            Chang'e, Cecilion            |              Franco, Kaja              | [37, 28]       | [61, 91]            | [10, 62]          |
|      24 | Natalia        |      Yi Sun-shin, Vexana      |             Novaria, Irithel            |            Minsitthar, Kaja            | [30, 38]       | [119, 43]           | [74, 62]          |
|      33 | Cyclops        | Belerick, Minsitthar, Unknown |     Terizla, Alpha, Unknown, Unknown    |   Lancelot, Alucard, Kagura, Unknown   | [70, 74, 0]    | [82, 28, 0, 0]      | [47, 7, 25, 0]    |
|     103 | Paquito        |    Angela, Rafaela, Unknown   |      Gord, Layla, Unknown, Unknown      |     Franco, Kaja, Unknown, Unknown     | [55, 14, 0]    | [23, 18, 0, 0]      | [10, 62, 0, 0]    |
|     122 | Nolan          |  Tigreal, Gatotkaca, Unknown  |         Hanabi, Eudora, Unknown         |            Thamuz, Fredrinn            | [6, 41, 0]     | [60, 15, 0]         | [72, 117]         |
+---------+----------------+-------------------------------+-----------------------------------------+----------------------------------------+----------------+---------------------+-------------------+
