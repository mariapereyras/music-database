# Music database 

For this project I wanted to practice how to create tables and then obtain information from then by querying the data to answer some questions. 

First I created a table for `bands` with the columns `id` and `name` for each band, we have 8 bands in total for this table:

`CREATE TABLE bands(
id SERIAL PRIMARY KEY, 
name VARCHAR(255) NOT NULL);`

Second, I created a table for `albums` with the columns `id` as a primary key that can auto increment, `name` which could not be null for each album name, `release_year` and `band_id` which associated this table to the `bands` table as a foreign key. 

`CREATE TABLE albums(
id SERIAL PRIMARY KEY, 
name VARCHAR(255) NOT NULL,
release_year INTEGER,
band_id SERIAL, 
CONSTRAINT fk_band
FOREIGN KEY (band_id) REFERENCES bands (id));`


Third, I created a table for `songs` with the columns `id`, `name` which could not be null for each song name, `length` for the length of each song and `album_id` which associated this table to the `albums` table as a foreign key. 

`CREATE TABLE songs(
id SERIAL PRIMARY KEY,
name VARCHAR NOT NULL, 
length FLOAT NOT NULL,
album_id INTEGER NOT NULL,
CONSTRAINT fx_album
FOREIGN KEY (album_id) REFERENCES albums (id));`

After creating the tables, I incerted the data for them from https://raw.githubusercontent.com/WebDevSimplified/Learn-SQL/master/data.sql

Then moving on to the queries, the questions I wanted to answer from these tables using postgresql where the following: 

*Select only the Names of all the Bands*

`SELECT name FROM bands;`

![alt text](https://github.com/mariapereyras/music-database/blob/main/screenshots/1.png "Hello")

*Select the Oldest Album*

`SELECT name,release_year FROM albums
ORDER BY release_year 
FETCH FIRST 1 row ONLY;`

![alt text](https://github.com/mariapereyras/music-database/blob/main/screenshots/2.png "Hello")


*Get all Bands that have Albums*

`SELECT b.name, albums.name FROM bands as b
JOIN albums ON b.id = albums.band_id;`

![alt text](https://github.com/mariapereyras/music-database/blob/main/screenshots/3.png "Hello")


*Get all Bands that have No Albums*
`SELECT b.id, b.name FROM bands AS b
LEFT JOIN albums ON b.id = albums.band_id
GROUP BY b.id, albums.band_id
HAVING count(albums.id) = 0`

![alt text](https://github.com/mariapereyras/music-database/blob/main/screenshots/4.png "Hello")

*Get the Longest Album*

`SELECT sum(length) as total_length,albums.name , albums.id FROM songs
JOIN albums ON songs.album_id = albums.id
GROUP BY albums.id 
ORDER BY total_length DESC
FETCH FIRST 1 row ONLY;`

![alt text](https://github.com/mariapereyras/music-database/blob/main/screenshots/5.png "Hello")

*Update the Release Year of the Album with no Release Year*

`UPDATE albums SET release_year = 2000 WHERE ID = 4;`

*Insert a record for your favorite Band and one of their Albums*

`INSERT INTO bands VALUES ( 8, 'camila');`

*Delete the Band and Album you added in*

`DELETE FROM bands WHERE id = 8;`

*Get the Average Length of all Songs*

`SELECT AVG(length) FROM songs;`

![alt text](https://github.com/mariapereyras/music-database/blob/main/screenshots/6.png "Hello")

*Select the longest Song off each Album*

`SELECT albums.name, MAX (length) as longest_song FROM songs 
JOIN albums on albums.id = songs.album_id
GROUP BY albums.name
ORDER BY longest_song DESC;`

![alt text](https://github.com/mariapereyras/music-database/blob/main/screenshots/7.png "Hello")

*Get the number of Songs for each Band*

`SELECT COUNT (album_id), bands.name  FROM albums
JOIN songs on albums.id = songs.album_id
JOIN bands on bands.id = albums.band_id
GROUP BY bands.name;`

![alt text](https://github.com/mariapereyras/music-database/blob/main/screenshots/8.png "Hello")
