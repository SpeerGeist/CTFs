# Mr-Worldwide - 200 points
Cryptography

## Description:
> A musician left us a message. What's it mean?

## Hint:
> None.

## Files:
message.txt:
> picoCTF{(35.028309, 135.753082)(46.469391, 30.740883)(39.758949, -84.191605)(41.015137, 28.979530)(24.466667, 54.366669)(3.140853, 101.693207)_(9.005401, 38.763611)(-3.989038, -79.203560)(52.377956, 4.897070)(41.085651, -73.858467)(57.790001, -152.407227)(31.205753, 29.924526)}


## Solving:

With this we can easily just search for each coordinate in maps but for fun I wrote a python script to take the message and output the flag (this was longer but it works).
```c
#! usr/bin/env Python3

import re
import reverse_geocoder as rg
from collections import OrderedDict

# Opening message and stripping unwanted letters and punctuation
with open('message.txt', 'r') as file:
    Locations = file.read()
Locations = re.sub("[a-zA-Z]", '', Locations)
Locations = Locations.strip('{}')
Locations = re.sub("_", '', Locations)
Locations = re.sub("\(", '', Locations)
Locations = Locations.split(')')

# splitting the list in to individual longitude and latitude lists
coordsList = [item.split(', ') for item in Locations]
lat = []
lon = []

def latr(lat, coordsList):
    for i in range(len(coordsList)-1):
        lat.append(coordsList[i][0])
    return lat

def lonr(lon, coordsList):
    for i in range(len(coordsList)-1):
        lon.append(coordsList[i][1])
    return lon

latr(lat, coordsList)
lonr(lon, coordsList)
names = []
firstLetters = []

# Reverse Geocode search and return city name in to names list.
for i in range(len(lat)):
    coords = []
    x = (float(lat[i]), float(lon[i]))
    coords.append(x)
    result = rg.search(coords[0])
    names.append(result[0]['name'])
    coords.pop()

# Get first letter of city names and join in to string for flag
for i in range(len(names)):
    firstLetters.append(names[i][0])
def flag(firstLetters):
    firstLetters = ''.join(firstLetters)
    print('Your flag is:')
    print('picoCTF{' + firstLetters + '}')
flag(firstLetters)
```
This outputs the flag:
> 'picoCTF{KODEAK_ALASKA}'
An issue arose with the 4th coordinate that returned 'Eminoenue' an inner-city to Istanbul. Maybe just poor choice in selection of coordinates?


### Flag: 

```c
picoCTF{KODIAK_ALASKA}
```
