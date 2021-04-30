## Setup

1. Open **Visual Studio**.
2. At the start screen select `Continue without code`.
3. Open the `View` menu and select `SQL Server Object Explorer`.
4. Expand the node for your SQL Server instance.
5. Right-click on "Databases" and select `Add new database`.
6. Name the new database `MusicHistory`.
7. Right click the `MusicHistory` database and choose `New Query`.
8. Run [this SQL script](./assets/musichistory.sqlserver.sql) to create tables in your database and insert some seed data.
9. Create a new query window You will be writing your SQL statements in this new, blank query window.

## Instructions

1. Using the **SQL Server Object Explorer** in Visual Studio, examine the tables, columns, and foreign keys of the database.
2. Using the `dbdiagram.io` site, create an ERD for the database.

For each of the following exercises, provide the appropriate query. Yes, even the ones that are expressed in the form of questions. Everything from class and the references listed above is fair game.

1. Query all of the entries in the Genre table
```sql
SELECT * FROM Genre
```

2. Query all the entries in the Artist table and order by the artist's name. HINT: use the ORDER BY keywords
```sql
SELECT * FROM Artist ORDER BY ArtistName
```

3. Write a SELECT query that lists all the songs in the Song table and include the Artist name
```sql
SELECT * FROM Song s LEFT JOIN Artist a ON s.ArtistId = a.Id
```

4. Write a SELECT query that lists all the Artists that have a Pop Album
```sql
SELECT Artist.ArtistName, Genre.Label FROM Album JOIN Artist ON Album.ArtistId = Artist.Id JOIN Genre on Album.GenreId = Genre.Id WHERE Genre.Label = 'Pop'
```

5. Write a SELECT query that lists all the Artists that have a Jazz or Rock Album
```sql
SELECT Artist.ArtistName, Genre.Label FROM Album JOIN Artist ON Album.ArtistId = Artist.Id JOIN Genre on Album.GenreId = Genre.Id WHERE Genre.Label = 'Jazz' OR Genre.Label = 'Rock'
```

6. Write a SELECT statement that lists the Albums with no songs
```sql
SELECT a.Title FROM Album a LEFT JOIN Song s ON a.Id = s.AlbumId WHERE s.AlbumId IS NULL
```

7 Using the INSERT statement, add one of your favorite artists to the Artist table.
```sql
INSERT INTO Artist (ArtistName, YearEstablished) VALUES ('Leviathan', '2021')
```

8. Using the INSERT statement, add one, or more, albums by your artist to the Album table.
```sql
INSERT INTO Album (Title, ReleaseDate, AlbumLength, Label, ArtistId, GenreId) VALUES ('Number One Victory Royale', 2021, 110, 'TikTok', 28, 7)
```

9. Using the INSERT statement, add some songs that are on that album to the Song table.
```sql
INSERT INTO Song (Title, SongLength, ReleaseDate, GenreId, ArtistId, AlbumId) VALUES ('Number One Victory Royale', 110, '02/01/2021', 7, 28, 23)
```

10. Write a SELECT query that provides the song titles, album title, and artist name for all of the data you just entered in. Use the LEFT JOIN keyword sequence to connect the tables, and the WHERE keyword to filter the results to the album and artist you added.
```sql
SELECT a.ArtistName, s.Title, m.Title FROM Artist a LEFT JOIN Song s ON s.ArtistId = a.Id LEFT JOIN Album m ON m.ArtistID = a.Id WHERE a.Id=28;
```
Reminder: Direction of join matters. Try the following statements and see the difference in results.
```sql
SELECT a.Title, s.Title FROM Album a LEFT JOIN Song s ON s.AlbumId = a.Id;
SELECT a.Title, s.Title FROM Album a RIGHT JOIN Song s ON s.AlbumId = a.Id;
SELECT a.Title, s.Title FROM Song s LEFT JOIN Album a ON s.AlbumId = a.Id;
```

11. Write a SELECT statement to display how many songs exist for each album. You'll need to use the COUNT() function and the GROUP BY keyword sequence.
```sql
SELECT a.Title, COUNT(a.Title) FROM Album a LEFT JOIN Song s ON a.Id = s.AlbumId WHERE s.AlbumId IS NOT NULL GROUP BY a.Title
```

12. Write a SELECT statement to display how many songs exist for each artist. You'll need to use the COUNT() function and the GROUP BY keyword sequence.
```sql
SELECT a.ArtistName, COUNT(a.ArtistName) FROM Artist a LEFT JOIN Song s ON a.Id = s.ArtistId WHERE s.ArtistId IS NOT NULL GROUP BY a.ArtistName
```

13. Write a SELECT statement to display how many songs exist for each genre. You'll need to use the COUNT() function and the GROUP BY keyword sequence.
```sql
SELECT g.Label, COUNT(g.Label) FROM Genre g LEFT JOIN Song s ON g.Id = s.GenreId WHERE s.GenreId IS NOT NULL GROUP BY g.Label
```

14. Write a SELECT query that lists the Artists that have put out records on more than one record label. Hint: When using GROUP BY instead of using a WHERE clause, use the HAVING keyword
```sql
SELECT a.ArtistName FROM Artist a LEFT JOIN Album m ON a.Id = m.ArtistId WHERE m.ArtistId IS NOT NULL GROUP BY ArtistName HAVING COUNT(DISTINCT m.Label) > 1
```

15. Using MAX() function, write a select statement to find the album with the longest duration. The result should display the album title and the duration.
```sql
SELECT Title, AlbumLength FROM Album WHERE AlbumLength = (SELECT MAX(AlbumLength) FROM Album)
```

16. Using MAX() function, write a select statement to find the song with the longest duration. The result should display the song title and the duration.
```sql
SELECT Title, SongLength FROM Song WHERE SongLength = (SELECT MAX(SongLength) FROM Song)
```

17. Modify the previous query to also display the title of the album.
```sql
SELECT s.Title, a.Title, s.SongLength FROM Song s LEFT JOIN Album a ON s.albumID = a.Id WHERE SongLength = (SELECT MAX(SongLength) FROM Song)
```
