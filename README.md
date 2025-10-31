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
