# Taylor Swift Spotify Analysis 

## Questions and Answers

Author: Marcia Hawkins-Day  
email: marciahd@umich.edu  


**1. During what time span, did Taylor’s songs reached peak popularity? When did her songs reach lowest popularity?**

````sql
SELECT 
	YEAR(a.date) AS year, MAX(s.popularity) AS max_popularity, AVG(s.popularity) AS mean_popularity
FROM 
	songs s
LEFT JOIN albums a on s.release_date_id = a.id
GROUP BY 
    YEAR(a.date);
````
**Results:**
year     |max_popularity| mean_popularity|
---------------|--------|----------------|
2006 |  59|           50.1333|
2010 |  64|           49.7273|
2012 |  72|           60.5000|
2014 |  82|           54.4211|
2017 |  78|           71.8667|
2019 |  80|           72.1111|
2020 |  65|           62.6471|
2021 |  76|           65.5349|


**2. Songs like All Too Well (the 10 minute version) are quite popular with many of her fans. Many of the office Swifties love Tayor Swift because of her song story-telling abilities; this often results in songs which are longer in length. Please describe the relationship between song duration and popularity.**


````sql
SELECT 
    name AS songe_name, popularity, (length/60000) AS lenght_in_minutes
FROM
    songs
ORDER BY
    popularity 
LIMIT 10;
````
**Results:**
song_name     |popularity| length_in_minutes|
---------------|--------|----------------|
I Wish You Would - Voice Memo |  0|           1.7856|
Blank Space - Voice Memo |  0|   2.1864
I Know Places - Voice Memo |  0|           3.6056|
Back to December |  43|           4.8840|
Never Grow UP |  44|           4.8413|
Innocent |  44| 5.0378|
Mine - POP Mix | 45| 3.8424|
A Perfectly Good Heart |  46| 3.6691|
Long Live| 47| 5.2993|
Tied Together with a Smile| 47| 4.1351|


````sql
SELECT name as song_name, popularity, (length/60000) AS lenght_in_minutes
FROM
    songs
ORDER BY
    popularity 
DESC LIMIT 10;
````

**Results:**
song_name     |popularity| length_in_minutes|
---------------|--------|----------------|
Blank Space | 82| 3.8638
Lover| 80 | 3.6884
Shake It Off| 80| 3.6533
You Need To Calm Down| 78 | 2.8560
Delicate| 78| 3.8709
Look What You Made Me Do| 77| 3.5309
Cruel Summer| 77| 2.9738
Me! (feat. Brendon Urie of Panic! At The Disco)| 77| 3.2167 
Paper Rings| 76| 3.7067
Getaway Car| 76| 3.8938| 3.8938

````sql
SELECT 
    a.name, AVG(s.popularity) AS mean_popularity, SUM((s.length/60000)) AS total_minutes
FROM 
    songs s
LEFT JOIN 
    albums a on a.id = s.album_id
GROUP BY 
    a.name;
````

**Results:**
name     |mean_popularity| total_minutes|
---------------|--------|----------------|
Taylor Swift| 50.1333| 53.4928
Speak Now (Deluxe Package)| 49.7273| 101.1889
Red (Deluxe Edition)| 60.5000| 90.6748
1989 (Deluxe)| 54.4211| 68.7610
reputation| 71.8667| 55.7551
Lover| 72.1111| 61.8566
folkmore (deluxe version)| 62.6471| 67.1401
evermore (deluxe version)| 65.4706| 69.0815
Fearless (Taylor's Version)| 65.5769| 106.5719


**3. Some critics have argued that Taylor Swift’s popularity has increased since her transition from the country to pop music genre, does the data support this? There is a divide among office Swifties about which genre resulted in more popular music. Is Taylor’s most popular music from a the country or pop (or alternative) genre?** 

````sql
SELECT 
    YEAR(a.date) AS year, MAX(s.popularity) AS max_popularity,
FROM songs s
LEFT JOIN 
    albums a on s.release_date_id = a.id
GROUP BY 
    YEAR(a.date);
````

**Results:**
year     |max_popularity| mean_popularity|
---------------|--------|----------------|
2006| 59| 50.1333
2010| 64| 49.7273
2012| 72| 60.5000
2014| 82| 54.4211
2017| 78| 71.8667
2019| 80| 72.1111
2020| 65| 62.6471
2021| 76| 65.5349


````sql
SELECT 
    g.name, MAX(s.popularity) AS max_popularity, AVG(s.popularity) AS mean_popularity
FROM songs s 
LEFT JOIN genres g on s.genre_id = g.id
GROUP BY g.name;
````

**Results:**
year     |max_popularity| mean_popularity|
---------------|--------|----------------|
country| 76| 57.4353
pop| 82| 65.5769
alternative| 72| 64.0588

**4. Overall, what attribute(s) makes for the most popular and least popular Taylor Swift music?**

````sql
SELECT 
    s.name as song_name, s.popularity, g.name AS genre FROM songs s
LEFT JOIN genres g on s.genre_id = g.id
ORDER BY 
    s.popularity 
DESC LIMIT 10;
````
**Results:**
song_name  | popularity| genre |
---------------|--------|----------------|
Blank Space | 82| pop
Lover| 80| pop
Shake It Off| 80| pop
You Need To Calm Down| 78| pop
Delicate| 78| pop
Look What You Made Me Do| 77| pop
Cruel Summer| 77| pop
ME! (feat. Brendon Urie of Panic! At The Disco)| 77| pop
Paper Rings| 76| pop
Getaway Car| 76| pop


````sql
SELECT  
    s.name as song_name, s.popularity, g.name AS genre 
FROM songs s
LEFT JOIN genres g on s.genre_id = g.id
ORDER BY 
    s.popularity 
LIMIT 10;
````

**Results:**
song_name  | popularity| genre |
---------------|--------|----------------|
I Wish You Would - Voice Memo |  0|           pop|
Blank Space - Voice Memo |  0|   pop|
I Know Places - Voice Memo |  0| pop|
Back To December |  43|   country|
Never Grow UP |  44|    country|
Innocent |  44| country|
Mine - POP Mix | 45| country|
A Perfectly Good Heart |  46| country|
Long Live| 47| country|
Tied Together with a Smile| 47| country|


````sql
SELECT  
    s.name as song_name, s.popularity, a.name AS album FROM songs s
LEFT JOIN albums a on s.album_id = a.id
ORDER BY 
    s.popularity 
DESC LIMIT 10;
````

**Results:**
song_name  | popularity| album |
---------------|--------|----------------|
Blank Space | 82| 1989 (Deluxe)
Lover| 80| Lover
Shake It Off| 80| 1989 (Deluxe)
You Need To Calm Down| 78| Lover
Delicate| 78| reputation
Look What You Made Me Do| 77| reputation
Cruel Summer| 77| Lover
ME! (feat. Brendon Urie of Panic! At The Disco)| 77| Lover
Paper Rings| 76| Lover
Getaway Car| 76| reputation


````sql
SELECT  
    s.name as song_name, s.popularity, a.name AS album 
FROM songs s
LEFT JOIN albums a on s.album_id = a.id
ORDER BY 
    s.popularity 
LIMIT 10;
````

**Results:**
song_name  | popularity| album|
---------------|--------|----------------|
I Wish You Would - Voice Memo |  0|           1989 (Deluxe)|
Blank Space - Voice Memo |  0|  1989 (Deluxe)|
I Know Places - Voice Memo |  0| 1989 (Deluxe)|
Back to December |  43|   Speak Now (Deluxe Package)|
Never Grow UP |  44|    Speak Now (Deluxe Package)|
Innocent |  44| Speak Now (Deluxe Package)|
Mine - POP Mix | 45| Speak Now (Deluxe Package)|
A Perfectly Good Heart |  46| Taylor Swift|
Long Live| 47| Speak Now (Deluxe Package)|
Tied Together with a Smile| 47| Taylor Swift|

````sql
SELECT 
    name AS song_name, popularity, (length/60000) AS minutes, danceability, acousticness, energy, tempo
FROM songs
ORDER BY 
    popularity 
DESC LIMIT 10;

````

**Results:**
song_name  | popularity| minutes | danceability | acousticness| energy| tempo
--------------|--------|---------|----|----|----|----|
Blank Space | 82| 3.8638| 0.76| 0.103| 0.703| 95.997
Lover| 80|3.6884| 0.359| 0.492| 0.543| 68.534
Shake It Off| 80| 3.6533| 0.647| 0.0647| 0.8| 160.078
You Need To Calm Down| 78| 2.8560| 0.771| 0.00929| 0.671| 85.026
Delicate| 78| 3.8709| 0.75| 0.216| 0.404| 95.045
Look What You Made Me Do| 77| 3.5309| 0.766| 0.204| 0.709| 128.07
Cruel Summer| 77| 2.9738| 0.552| 0.117| 0.702| 169.994
ME! (feat. Brendon Urie of Panic! At The Disco)| 77| 3.2167| 0.61| 0.033| 0.83| 182.162
Paper Rings| 76| 3.7067| 0.811| 0.0129| 0.719| 103.979
Getaway Car| 76| 3.8938| 0.562| 0.00465| 0.689| 172.054

````sql
SELECT 
    name AS song_name, popularity, (length/60000) AS minutes, danceability, acousticness, energy, tempo
FROM songs
ORDER BY 
    popularity 
LIMIT 10;
````

**Results:**
song_name  | popularity| minutes | danceability | acousticness| energy| tempo
--------------|--------|---------|----|----|----|----|
I Wish You Would - Voice Memo |  0|  1.7856| 0.781| 0.717| 0.357| 118.317
Blank Space - Voice Memo |  0|  2.1864| 0.675| 0.801| 0.234| 127.296
I Know Places - Voice Memo |  0| 3.6056| 0.592| 0.829| 0.128| 78.828
Back to December |  43| 4.8840| 0.525| 0.113| 0.676| 141.95
Never Grow UP |  44| 4.8413| 0.715| 0.829| 0.308| 124.899
Innocent |  44| 5.0378| 0.553| 0.202| 0.604| 133.989
Mine - POP Mix | 45| 3.8424| 0.696| 0.00461| 0.768| 121.05
A Perfectly Good Heart |  46| 3.6691| 0.483| 0.00349| 0.751| 156.092
Long Live| 47| 5.2993| 0.412| 0.0426| 0.682| 203.959
Tied Together with a Smile| 47| 4.1351| 0.479| 0.525| 0.578| 146.165


````sql
SELECT 
    a.name AS album, AVG(s.popularity) AS mean_popularity, ROUND(AVG((s.length/60000)), 4) AS mean_song_length_in_minutes,
     ROUND(AVG(s.danceability), 4) AS mean_danceability, ROUND(AVG(s.acousticness), 4) AS mean_acousticness
FROM songs s
LEFT JOIN albums a on s.album_id = a.id
GROUP BY 
    a.name
ORDER BY 
    AVG(s.popularity) 
DESC LIMIT 10;
````

**Results:**
album  | mean_popularity| mean_song_length_in_minutes | mean_danceability | mean_acousticness
--------------|--------|---------|----|----|
Lover| 72.1111| 3.4365| 0.6582| 0.3337
reputation| 71.8667| 3.7178| 0.6579| 0.1385
Fearless (Taylor's Version)| 65.5769| 4.0978| 0.551| 0.2141
evermore (deluxe version) | 65.4706| 4.0636| 0.5268| 0.7941
folkmore (deluxe version)| 62.6471| 3.9494| 0.5419| 0.7176
Red (Deluxe Edition)| 60.5000| 4.1216| 0.6334| 0.1488| 
1989 (Deluxe)| 54.4211| 3.6190| 0.6332| 0.2446| 
Taylor Swift| 50.1333| 3.5662| 0.5453| 0.183
Speak Now (Deluxe Package)| 49.7273| 4.5995| 0.559| 0.2265



````sql
SELECT a.name AS album, AVG(s.popularity) AS mean_popularity, ROUND(AVG(s.energy), 4) AS mean_energy,
      ROUND(AVG(s.tempo), 4) AS mean_tempo
FROM songs s
LEFT JOIN albums a on s.album_id = a.id
GROUP BY a.name
ORDER BY 
    AVG(s.popularity) 
DESC LIMIT 10;
````

**Results:**
album  | mean_popularity| mean_energy| mean_tempo|
--------------|--------|---------|----|
Lover| 72.1111| 0.5452| 119.9727
reputation| 71.8667| 0.5829| 127.5401
Fearless (Taylor's Version)| 65.5769| 0.6391| 131.2372 
evermore (deluxe version) | 65.4706| 0.4941| 120.7073
folkmore (deluxe version)| 62.6471| 0.4158| 119.8844
Red (Deluxe Edition)| 60.5000| 0.6008| 110.2965
1989 (Deluxe)| 54.4211| 0.6248| 127.0331
Taylor Swift| 50.1333| 0.6643| 126.0538
Speak Now (Deluxe Package)| 49.7273| 0.6594| 132.8357

