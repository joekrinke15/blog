---
layout: post
title: Downloading Star Wars Data at Lightspeed
image: https://vignette.wikia.nocookie.net/starwars/images/c/cc/Star-wars-logo-new-tall.jpg/revision/latest?cb=20190313021755
full-width: true
---
In this post I will be using the Star Wars API (SWAPI) to learn more about some of the classic characters from Star Wars. I'll be using the requests package to access the data. The first step of accessing the data is making a request. Once you have the data you've requested from the API you can get it in JSON format by using the json method.

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
Interestingly enough, the API only provides us with 10 characters worth of information. Each time we call requests.get() we only get information from the current page we are looking at. Luckily, we can use 'next' to move on to the next page of data. You can see below that passing in next takes us to page 2.

```python3
characters['next']
```
We can use the next feature in a while loop to continually get more data as long as the next page exists. This will allow us to access all the data over multiple pages. 
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

Now let's determine who is the oldest character in the Star Wars universe. Star Wars uses a pretty interesting way to measure the ages of people (and aliens/robots). Everything is dated as BBY or ABY, which stands for "Before Battle of Yavin" or "After Battle of Yavin" -this is evidently the battle in which the first death star was destroyed. Older people would have higher BBY values (6000BBY), whereas younger people would have higher ABY values (6000ABY). This dataset doesn't have anyone born in BBY, which makes this problem a bit simpler. We can begin by getting a list of the birth years of each character.

```python3
# Loop over the characters to extract all the birth years
birth_year = []
for i in all_results:
    birth_year.append((i['birth_year']))
```
The birth years are in a string format with BBY at the end. We need to create a function to get the year values from the string.

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
Now that we have our function we can use map to apply it to each element in the list. From there, all we have to do is get the index of the highest value and use it to find the oldest character. 

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
The oldest character is Yoda!

<p align="center">
<img src ='https://cdn2.lamag.com/wp-content/uploads/sites/6/2019/12/baby-yoda-lucasfilm-1068x711.jpg'/>
  <em>Yoda looks a lot younger here.</em>
</p>

I'm embarassed to admit I'm not sure what movies he appears in, as I haven't actually seen most of them. Let's write some code to find out.

```python3
# Try to look at Yoda's movie appearances 
oldest_character['films']
```
It seems that, when you try to access the films, it just links to more pages of data. We'll need to use the API to get the names of the films.

```python3
# Create a list to hold the titles of the films
film_list = []
for i in oldest_character['films']: # Iterate over each film in the list
    film = requests.get(i) # Create a response for each film
    film_list.append(film.json()['title']) # Get the json version of the data and access the title. Append the title of each film to the list.
    # Print the films Yoda has been in 
print(film_list)
```
Yoda has been in The Empire Strikes Back, Return of the Jedi, The Phantom Menace, Attack of the Clones, and Revenge of the Sith. Maybe this will inspire me to actually watch all the Star Wars movies!
