## SPRINT 1
----------------------

CREATE DATABASE LibraryDB;

## Create authors table

CREATE TABLE authors (
  id INT PRIMARY KEY,
  name TEXT NOT NULL,
  nationality TEXT,
  birth_year INT,
  death_year INT
);


## Create books table

CREATE TABLE books (
  id INT PRIMARY KEY,
  title TEXT NOT NULL,
  author_id INT NOT NULL,
  genres TEXT[] DEFAULT ARRAY[]::TEXT[],
  published_year INT,
  available BOOLEAN DEFAULT TRUE,
  CONSTRAINT fk_author
    FOREIGN KEY (author_id)
    REFERENCES authors(id)
    ON DELETE RESTRICT
);


## Create patrons table

CREATE TABLE patrons (
  id INT PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  borrowed_books INT[] DEFAULT ARRAY[]::INT[]
);

## SPRINT 2 (Insert the sample rows)
-----------------------------------------------------------

## Authors 
INSERT INTO authors (id, name, nationality, birth_year, death_year) VALUES
(1, 'George Orwell', 'British', 1903, 1950),
(2, 'Harper Lee', 'American', 1926, 2016),
(3, 'F. Scott Fitzgerald', 'American', 1896, 1940),
(4, 'Aldous Huxley', 'British', 1894, 1963),
(5, 'J.D. Salinger', 'American', 1919, 2010),
(6, 'Herman Melville', 'American', 1819, 1891),
(7, 'Jane Austen', 'British', 1775, 1817),
(8, 'Leo Tolstoy', 'Russian', 1828, 1910),
(9, 'Fyodor Dostoevsky', 'Russian', 1821, 1881),
(10, 'J.R.R. Tolkien', 'British', 1892, 1973);

## Books 
INSERT INTO books (id, title, author_id, genres, published_year, available) VALUES
(1, '1984', 1, ARRAY['Dystopian', 'Political Fiction'], 1949, TRUE),
(2, 'To Kill a Mockingbird', 2, ARRAY['Southern Gothic', 'Bildungsroman'], 1960, TRUE),
(3, 'The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
(4, 'Brave New World', 4, ARRAY['Dystopian', 'Science Fiction'], 1932, TRUE),
(5, 'The Catcher in the Rye', 5, ARRAY['Realist Novel', 'Bildungsroman'], 1951, TRUE),
(6, 'Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
(7, 'Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
(8, 'War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
(9, 'Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
(10, 'The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);

## Patrons 
INSERT INTO patrons (id, name, email, borrowed_books) VALUES
(1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
(2, 'Bob Smith', 'bob@example.com', ARRAY[1,2]),
(3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
(4, 'David Brown', 'david@example.com', ARRAY[3]),
(5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
(6, 'Frank Moore', 'frank@example.com', ARRAY[4,5]),
(7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
(8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
(9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
(10, 'Jack Anderson', 'jack@example.com', ARRAY[7,8]);

## SPRINT 3(READ OPERAIONS)
---------------------------------
## get all books

SELECT * FROM books;

## Get books by title 

SELECT * from books
WHERE title = '1984';

## Get book by ID

SELECT b.*, a.name AS author_name
FROM books b
JOIN authors a ON b.author_id = a.id
WHERE b.id = 1;

## Get books by author ID

SELECT * FROM books WHERE author_id = 1;

## Get books by authorName

SELECT b.*, a.name AS author_name
FROM books b
JOIN authors a ON b.author_id = a.id
WHERE LOWER(a.name) LIKE LOWER('%orwell%');

## Get all available books

SELECT b.*, a.name AS author_name
FROM books b
JOIN authors a ON b.author_id = a.id
WHERE b.available = TRUE;

## SPRINT 4(UPDATE OPERATIONS)
------------------------------
## Mark a book as borrowed (set available = false)

UPDATE books
SET available = FALSE
WHERE id = 1;

## Mark a book as returned (set available = true)

UPDATE books
SET available = TRUE
WHERE id = 1;

## Add a new genre to an existing book

UPDATE books
SET genres = array_append(genres, 'Classic')
WHERE id = 3
  AND NOT ('Classic' = ANY(genres));


## Add borrowed book to patrons record


UPDATE patrons
SET borrowed_books = array_append(borrowed_books, 1)
WHERE id = 2
  AND NOT (1 = ANY(borrowed_books));



## Remove returned book from patron records

UPDATE books
SET available = FALSE
WHERE id = 1;


UPDATE patrons
SET borrowed_books = array_remove(borrowed_books, 1)
WHERE id = 2;

UPDATE books
SET available = TRUE
WHERE id = 1;

## SPRINT 5(DELETE OPERATIONS)
-----------------------------------

## delete a  book by title

DELETE FROM books
WHERE title = 'The Hobbit';

## Delete an author by ID

DELETE FROM authors WHERE id = 10;

## SPRINT 6 (ADVANCED QUERIES)
-------------------------------------
## Find books published after 1950

SELECT * FROM books WHERE published_year > 1950 ORDER BY published_year;

## Find all american authors

SELECT * FROM authors WHERE LOWER(nationality) = 'american';

## Set all books as available

UPDATE books SET available = TRUE;

## Find all books that are available AND published after 1950

SELECT b.*, a.name AS author_name
FROM books b
JOIN authors a ON b.author_id = a.id
WHERE b.available = TRUE AND b.published_year > 1950;

## Find authors whose names contain "George" 

SELECT * FROM authors WHERE LOWER(name) LIKE '%george%';

## Increment the published year 1869 by 1

UPDATE books
SET published_year = published_year + 1
WHERE published_year = 1869;

## Bulk update example: add genre 'Classic' to all books published before 1950

UPDATE books
SET genres = array_cat(genres, ARRAY['Classic'])
WHERE published_year < 1950
  AND NOT ('Classic' = ANY(genres));

## find books where genre includes 'Dystopian'

  SELECT * FROM books WHERE 'Dystopian' = ANY(genres);



## SPRINT 7
---------------

# Library Management System 

## Description

A simple Library Management System implemented in PostgreSQL. Stores authors, books, and patrons. Supports CRUD operations and advanced queries.

## Setup
1. Open   pgAdmin.
2. Create the database:
   - psql: CREATE DATABASE LibraryDB;

















