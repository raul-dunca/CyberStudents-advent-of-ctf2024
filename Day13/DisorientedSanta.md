<img src="https://github.com/raul-dunca/assets/blob/main/.images_CyberStudents-advent-of-ctf2024/day13_description.png">

For this challenge, I used [overpass turbo](https://overpass-turbo.eu/). I played with different queries but did not manage to find the location only after the second hint. My initial approach was to search only in the 20 countries which actually use euros, but that is not actually needed, since the charge can be specified. My final query was:
```sql
[out:json][timeout:1000];

node["tourism"="museum"]["charge"="3 EUR"]->.museums;
node["amenity"="library"]->.libraries;
node.museums(around.libraries:1000);

out body;
>;
out skel qt;
```

This yields around 10 results, but only one contains `"museum": "history"`, that is `Haxahüs – La maison des sorcières`.

`csd{48.205,7.365}`
