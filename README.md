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
