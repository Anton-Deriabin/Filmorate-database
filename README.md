1. Список друзей пользователя
Получить список всех друзей пользователя по id:

SELECT u.id AS friend_id, u.name AS friend_name, f.is_approved
FROM friends f
JOIN user u ON u.id = f.reciever
WHERE f.sender = 1; -- Укажите id пользователя

2. Список фильмов с их жанрами
Получить список фильмов и связанных с ними жанров:

SELECT f.name AS film_name, g.name AS genre_name
FROM film f
JOIN film_genres fg ON f.id = fg.film_id
JOIN genres g ON fg.genre_id = g.id;

3. Фильмы по определённому жанру
Получить список фильмов, относящихся к жанру "Action":

SELECT f.name AS film_name
FROM film f
JOIN film_genres fg ON f.id = fg.film_id
JOIN genres g ON fg.genre_id = g.id
WHERE g.name = 'Action';

4. Самые популярные фильмы
Получить список фильмов с количеством лайков, отсортированный по популярности:

SELECT f.name AS film_name, COUNT(fl.user_id) AS likes_count
FROM film f
LEFT JOIN film_likes fl ON f.id = fl.film_id
GROUP BY f.id, f.name
ORDER BY likes_count DESC;

5. Пользователи, которые поставили лайк определённому фильму
Получить список пользователей, которые поставили лайк фильму с ID 1:

SELECT u.name AS user_name
FROM film_likes fl
JOIN user u ON fl.user_id = u.id
WHERE fl.film_id = 1;

6. Фильмы с их рейтингами
Получить список фильмов с их возрастным рейтингом:

SELECT f.name AS film_name, r.name AS rating_name
FROM film f
JOIN ratings r ON f.rating_id = r.id;

7. Список всех жанров с количеством фильмов
Получить список всех жанров и количество фильмов в каждом жанре:

SELECT g.name AS genre_name, COUNT(fg.film_id) AS film_count
FROM genres g
LEFT JOIN film_genres fg ON g.id = fg.genre_id
GROUP BY g.id, g.name
ORDER BY film_count DESC;

8. Список всех фильмов, которые понравились друзьям пользователя
Получить список фильмов, которые лайкнули друзья пользователя с ID 1:

SELECT DISTINCT f.name AS film_name
FROM friends fr
JOIN film_likes fl ON fr.reciever = fl.user_id
JOIN film f ON fl.film_id = f.id
WHERE fr.sender = 1 AND fr.is_approved = true;

9. Пользователи, у которых нет друзей
Получить список пользователей, которые не добавили никого в друзья:

SELECT u.id, u.name
FROM user u
LEFT JOIN friends f ON u.id = f.sender
WHERE f.sender IS NULL;

10. Фильмы, которые не имеют жанров
Получить список фильмов, которые не связаны ни с одним жанром:

SELECT f.name AS film_name
FROM film f
LEFT JOIN film_genres fg ON f.id = fg.film_id
WHERE fg.genre_id IS NULL;
