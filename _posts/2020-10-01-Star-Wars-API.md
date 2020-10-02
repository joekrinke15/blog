---
layout: post
title: Downloading Star Wars Data at Lightspeed
full-width: true
---
In this post I will be using the Star Wars API (SWAPI) to learn more about some of the classic characters from Star Wars. I'll be using the requests package to access the data. The first step of accessing the data is making a request. Once you have the data you've requested from the API you can begin using it. Data is commonly sent in JSON format. 
```python3
# Make a request
r = requests.get('https://swapi.dev/api/people/')

# Get the data in json format
characters = r.json()
```
Now that we have some of the data in JSON format we can see what an individual record looks like. 

```python3
# Look at a record
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

```python3
# Apply the function to the list using mapping
cleaned_list = list(map(get_num, birth_year))
# Get the index corresponding to the oldest character
max_index = cleaned_list.index(max(cleaned_list))
# Use the index to get the oldest character's information
oldest_character = all_results[max_index]
# Print the result
print(oldest_character['name'], oldest_character['birth_year'])
```

```python3
# Try to look at Yoda's movie appearances 
oldest_character['films']
```

```python3
# Create a list to hold the titles of the films
film_list = []
for i in oldest_character['films']: # Iterate over each film in the list
    film = requests.get(i) # Create a response for each film
    film_list.append(film.json()['title']) # Get the json version of the data and access the title. Append the title of each film to the list.
    # Print the films Yoda has been in 
print(film_list)
```
