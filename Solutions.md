
##Black Diamond: 

- Get all tracks on the playlist(s) called Music and show their name, genre name, album name, and artist name.
At least 5 joins.

**Solution**

SELECT Playlist.Name, Genre.Name, Album.Title, Artist.Name
FROM Playlist 
JOIN PlaylistTrack ON Playlist.PlaylistId = PlayListTrack.PlaylistId
JOIN Track ON Track.TrackId = PlaylistTrack.TrackId
JOIN Genre on Genre.GenreId = Track.GenreId
JOIN Album ON Album.AlbumId = Track.AlbumId
JOIN Artist ON Artist.ArtistId = Album.ArtistId
WHERE Playlist.Name = 'Music'




##eCommerce Simulation: 

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT 
)

INSERT INTO users (name, email) VALUES ('Betty', 'Betty@gmail.com'),
('Wilma', 'Wilma@gmail.com'), ('Fred', 'Fred@gmail.com')


CREATE TABLE products (
    id SERIAL PRIMARY KEY, 
    name TEXT, 
    price INTEGER
)

INSERT INTO products (name, price) VALUES ('sweater', 12),
('socks', 5), ('pants', 20)


CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    orderNum INTEGER,
    productId INTEGER REFERENCES products (id)
)

INSERT INTO orders (orderNum, productId) VALUES (1, 1), (1, 2), (2,3), (2,1), (3, 3), (3, 2)




////Get all products for the first order:

SELECT name, price, orders.orderNum, orders.productId
FROM products
JOIN orders ON orders.productId = products.id
Where orders.orderNum = 1



////Get all orders:

SELECT ordernum, productid, products.name, products.price
FROM orders
JOIN products ON orders.productId = products.id

<!-- SELECT * FROM orders -->


////Get the total cost of an order ( sum the price of all products on an order ).

SELECT sum(products.price)
FROM orders
JOIN products ON orders.productId = products.id
WHERE ordernum = 1


////Add a foreign key reference from Orders to Users.

ALTER TABLE orders
ADD COLUMN userId INTEGER REFERENCES users (id)


////Update the Orders table to link a user to each order.

UPDATE orders SET userId = 1 WHERE orderNum = 1;
UPDATE orders SET userId = 2 WHERE orderNum = 3;
UPDATE orders SET userId = 3 WHERE orderNum = 2;

<!-- INSERT INTO orders (orderNum, productId, userId) VALUES (1, 1, 1), (1, 2, 2), (2, 3, 3), (2, 1, 2), (3, 3, 2), (3, 2, 3) --> 


////Get all orders for a user:

SELECT *
FROM orders
JOIN users ON users.id = orders.userId
WHERE users.Id = 1


<!-- Select * from orders where userId = 1 -->

////Get how many orders each user has:

SELECT count(orderNum)
FROM orders
JOIN users ON users.id = orders.userId
WHERE users.Id = 2



##Black Diamond: 

////Get the total amount on all orders for each user:

SELECT sum(price)
FROM orders
JOIN products ON products.id = orders.userId