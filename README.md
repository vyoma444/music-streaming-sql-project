# Music Streaming Database Project

Mini PostgreSQL project using:

- SELECT
- WHERE
- JOIN
- LEFT JOIN
- GROUP BY
- HAVING
- Subqueries

## Tables
- users
- artists
- songs
- plays

## Features
- Find top played songs
- Find inactive users
- Analyze song play statistics



CREATE TABLE users (
id SERIAL PRIMARY KEY,
name VARCHAR(50),
country VARCHAR(100),
joined_date DATE
);

CREATE TABLE artists(
id SERIAL PRIMARY KEY,
name VARCHAR(50),
genre VARCHAR(100)
);

CREATE TABLE songs (
    id SERIAL PRIMARY KEY,
    title VARCHAR(100),
    artist_id INT REFERENCES artists(id),
    duration_sec INT
);


CREATE TABLE plays (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id),
    song_id INT REFERENCES songs(id),
    played_at TIMESTAMP
);



INSERT INTO users (name, country, joined_date) VALUES
('Alice', 'USA', '2023-01-10'),
('Bob', 'India', '2023-02-15'),
('Charlie', 'UK', '2023-03-20'),
('David', 'Canada', '2023-04-05'),
('Eve', 'Germany', '2023-05-01');

INSERT INTO artists (name, genre) VALUES
('Taylor Swift', 'Pop'),
('Ed Sheeran', 'Pop'),
('Drake', 'Hip-Hop'),
('Adele', 'Pop'),
('Arijit Singh', 'Bollywood');

INSERT INTO songs (title, artist_id, duration_sec) VALUES
('Love Story', 1, 230),
('Shape of You', 2, 240),
('God Plan', 3, 198),
('Hello', 4, 300),
('Tum Hi Ho', 5, 260);

INSERT INTO plays (user_id, song_id, played_at) VALUES
(1, 6, NOW()),
(1, 7, NOW()),
(2, 7, NOW()),
(2, 8, NOW()),
(4, 9, NOW()),
(1, 6, NOW()),
(2, 6, NOW());

SELECT * FROM users
SELECT * FROM artists

SELECT * FROM plays

SELECT s.title, a.name AS artist_name, COUNT(p.id) AS play_count
FROM songs s
JOIN artists a ON s.artist_id = a.id
JOIN plays p ON s.id = p.song_id
GROUP BY s.title, a.name
ORDER BY play_count DESC
LIMIT 3; 


SELECT u.name, u.country
FROM users u
LEFT JOIN plays p ON u.id = p.user_id
WHERE p.id is NULL;


SELECT s.title
FROM songs s
JOIN artists a ON s.artist_id = a.id
LEFT JOIN plays p ON s.id = p.song_id
WHERE a.genre = 'Pop'
GROUP BY s.id, s.title
HAVING COUNT(p.id) > (
    SELECT AVG(play_count)
    FROM (
        SELECT COUNT(*) AS play_count
        FROM plays
        GROUP BY song_id
    ) AS sub
);





# Music Streaming Database Project

Mini PostgreSQL project using:

- SELECT
- WHERE
- JOIN
- LEFT JOIN
- GROUP BY
- HAVING
- Subqueries

## Tables
- users
- artists
- songs
- plays

## Features
- Find top played songs
- Find inactive users
- Analyze song play statistics
