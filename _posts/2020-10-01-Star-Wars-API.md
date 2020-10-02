---
layout: post
title: Downloading Star Wars Data at Lightspeed
full-width: true
---

```python3
r = requests.get('https://swapi.dev/api/people/')
characters = r.json()
```

```python3
characters['results'][0]
```

```python3
characters['next']
```

```python3
# Create an initial request
r2 = requests.get("https://swapi.dev/api/people/")
people2 = r2.json()
# Create list to hold data
all_results = []
# Get first page of results
all_results = all_results + people2['results']
# While data['next'] isn't empty, let's download the next page, too
while people2['next'] is not None:
    print("Downloading", people2['next'])
    r2 = requests.get(people2['next'])
    people2 = r2.json()
    all_results = all_results + people2['results']
```

```python3
# Loop over the characters to extract all the birth years
birth_year = []
for i in all_results:
    birth_year.append((i['birth_year']))
```

```python3
def get_num(x):
    """
    Extracts a float from a string of the format "XX.XABY".
    
    Inputs:
        x (string): A string of the format "XX.XABY".
    Outputs:
        (float): A float corresponding to XX.X. Returns nan if the input is 'unknown'.
    """
    # Remove values of unknown
    if x != 'unknown':
        # Join together the numbers and periods to form a float
        return float(''.join(num for num in x if num.isdigit() or num == '.'))
    else:
        return math.nan
```
