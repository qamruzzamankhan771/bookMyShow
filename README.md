# bookMyShow

# Table of Contents
Introduction
Prerequisites
Entities with Attributes
Users
Theaters
Movies
Languages
Movie Casts
Theater Movies
Theater Seats
Seats Category Prices
Tickets
SQL Queries


Introduction<a name="introduction"></a>
This document outlines the database model for a Book My Show app clone. The database is designed to manage user information, theaters, movies, languages, movie casts, theater movies, theater seats, seat category prices, and tickets.

Prerequisites<a name="prerequisites"></a>
Before setting up the database, ensure the following prerequisites are met:

MySQL database installed.
DBeaver or MySQL Workbench for database access.



Entities with Attributes<a name="entities-with-attributes"></a>

Users<a name="users"></a>
Field	Type	Constraints
userId	Integer	Primary Key, Auto Increment, Not Null
userName	Varchar(255)	Not Null
phoneNumber	Varchar(255)	Not Null
email	Varchar(255)	Not Null


Theaters<a name="theaters"></a>
Field	Type	Constraints
theaterId	Integer	Primary Key, Auto Increment, Not Null
theaterName	Varchar(255)	Not Null
address	Varchar(255)	Not Null


Movies<a name="movies"></a>
Field	Type	Constraints
movieId	Integer	Primary Key, Auto Increment, Not Null
movieName	Varchar(255)	Not Null
ratings	Integer	
storyLine	Varchar(255)	
genre	Varchar(255)	


Languages<a name="languages"></a>
Field	Type	Constraints
languageId	Integer	Primary Key, Auto Increment, Not Null
movieId	Integer	Foreign Key, Not Null
languageName	Varchar(255)	Not Null


Movie Casts<a name="movie-casts"></a>
Field	Type	Constraints
id	Integer	Primary Key, Auto Increment, Not Null
movieId	Integer	Foreign Key, Not Null
name	Varchar(255)	Not Null
roleName	Varchar(255)	Not Null


Theater Movies<a name="theater-movies"></a>
Field	Type	Constraints
id	Integer	Primary Key, Auto Increment, Not Null
theaterId	Integer	Foreign Key, Not Null
moviesId	Integer	Foreign Key, Not Null
languageId	Integer	Foreign Key, Not Null
fromDate	DateTime	Not Null
tillDate	DateTime	Not Null
showtime	DateTime	Not Null


Theater Seats<a name="theater-seats"></a>
Field	Type	Constraints
id	Integer	Primary Key, Auto Increment, Not Null
theaterId	Integer	Foreign Key, Not Null
seatNo	Varchar(255)	Not Null
isActive	Boolean	Not Null


Seats Category Prices<a name="seats-category-prices"></a>
Field	Type	Constraints
id	Integer	Primary Key, Auto Increment, Not Null
theaterId	Integer	Foreign Key, Not Null
name	Varchar(255)	Not Null
price	Integer	Not Null


Tickets<a name="tickets"></a>
Field	Type	Constraints
ticketId	Integer	Primary Key, Auto Increment, Not Null
theaterMovieId	Integer	Foreign Key, Not Null
userId	Integer	Foreign Key, Not Null
seatCategoryId	Integer	Foreign Key, Not Null
seatNo	Integer	Not Null
code	DateTime	Not Null
validFrom	DateTime	Not Null
validTill	DateTime	Not Null


SQL Queries<a name="-queries"></a>

Create Database

CREATE DATABASE IF NOT EXISTS bookMyShow;
USE bookMyShow;


Create Tables

Users

CREATE TABLE Users (
    userId INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    userName VARCHAR(255) NOT NULL,
    phoneNumber VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL
);

Theaters

CREATE TABLE Theaters (
    theaterId INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    theaterName VARCHAR(255) NOT NULL,
    address VARCHAR(255) NOT NULL
);

Movies

CREATE TABLE Movies (
    movieId INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    movieName VARCHAR(255) NOT NULL,
    ratings INT,
    storyLine VARCHAR(255),
    genre VARCHAR(255)
);

Languages

CREATE TABLE Languages (
    languageId INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    movieId INT NOT NULL,
    languageName VARCHAR(255) NOT NULL,
    FOREIGN KEY (movieId) REFERENCES Movies(movieId)
);

Movie Casts

CREATE TABLE MovieCasts (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    movieId INT NOT NULL,
    name VARCHAR(255) NOT NULL,
    roleName VARCHAR(255) NOT NULL,
    FOREIGN KEY (movieId) REFERENCES Movies(movieId)
);

Theater Movies

CREATE TABLE TheaterMovies (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    theaterId INT NOT NULL,
    moviesId INT NOT NULL,
    languageId INT NOT NULL,
    fromDate DATETIME NOT NULL,
    tillDate DATETIME NOT NULL,
    showtime DATETIME NOT NULL,
    FOREIGN KEY (theaterId) REFERENCES Theaters(theaterId),
    FOREIGN KEY (moviesId) REFERENCES Movies(movieId),
    FOREIGN KEY (languageId) REFERENCES Languages(languageId)
);

Theater Seats

CREATE TABLE TheaterSeats (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    theaterId INT NOT NULL,
    seatNo VARCHAR(255) NOT NULL,
    isActive BOOLEAN NOT NULL,
    FOREIGN KEY (theaterId) REFERENCES Theaters(theaterId)
);

Seats Category Prices

CREATE TABLE SeatsCategoryPrices (
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    theaterId INT NOT NULL,
    name VARCHAR(255) NOT NULL,
    price INT NOT NULL,
    FOREIGN KEY (theaterId) REFERENCES Theaters(theaterId)
);

Tickets

CREATE TABLE Tickets (
    ticketId INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    theaterMovieId INT NOT NULL,
    userId INT NOT NULL,
    seatCategoryId INT NOT NULL,
    seatNo INT NOT NULL,
    code DATETIME NOT NULL,
    validFrom DATETIME NOT NULL,
    validTill DATETIME NOT NULL,
    FOREIGN KEY (theaterMovieId) REFERENCES TheaterMovies(id),
    FOREIGN KEY (userId) REFERENCES Users(userId),
    FOREIGN KEY (seatCategoryId) REFERENCES SeatsCategoryPrices(id)
);

Insert Sample Data

-- Insert sample data into Users table
INSERT INTO Users (userName, phoneNumber, email) VALUES
('John Doe', '1234567890', 'john.doe@example.com'),
('Jane Smith', '9876543210', 'jane.smith@example.com');

-- Insert sample data into Theaters table
INSERT INTO Theaters (theaterName, address) VALUES
('Cineplex Central', '123 Main St'),
('Silver Screen Plaza', '456 Broadway Ave');

-- Insert sample data into Movies table
INSERT INTO Movies (movieName, ratings, storyLine, genre) VALUES
('The Matrix', 8, 'A computer hacker learns about the true nature of his reality', 'Sci-Fi'),
('Inception', 9, 'A thief who enters the dreams of others to steal their secrets', 'Thriller');

-- Insert sample data into Languages table
INSERT INTO Languages (movieId, languageName) VALUES
(1, 'English'),
(2, 'English');

-- Insert sample data into Movie Casts table
INSERT INTO MovieCasts (movieId, name, roleName) VALUES
(1, 'Keanu Reeves', 'Neo'),
(2, 'Leonardo DiCaprio', 'Dom Cobb');

-- Insert sample data into Theater Movies table
INSERT INTO TheaterMovies (theaterId, moviesId, languageId, fromDate, tillDate, showtime) VALUES
(1, 1, 1, '2023-01-01 10:00:00', '2023-01-01 12:00:00', '2023-01-01 10:15:00'),
(2, 2, 1, '2023-01-02 14:00:00', '2023-01-02 16:00:00', '2023-01-02 14:30:00');

-- Insert sample data into Theater Seats table
INSERT INTO TheaterSeats (theaterId, seatNo, isActive) VALUES
(1, 'A1', true),
(1, 'A2', true),
(2, 'B1', true),
(2, 'B2', true);

-- Insert sample data into Seats Category Prices table
INSERT INTO SeatsCategoryPrices (theaterId, name, price) VALUES
(1, 'Regular', 10),
(2, 'Premium', 15);

-- Insert sample data into Tickets table
INSERT INTO Tickets (theaterMovieId, userId, seatCategoryId, seatNo, code, validFrom, validTill) VALUES
(1, 1, 1, 1, '2023-01-01 10:30:00', '2023-01-01 10:00:00', '2023-01-01 11:30:00'),
(2, 2, 2, 3, '2023-01-02 15:00:00', '2023-01-02 14:00:00', '2023-01-02 16:00:00');




-- query to list down all the shows on a given date at a given theatre along with their respective show timings


SELECT
    t.theaterName,
    m.movieName,
    tm.languageId,
    tm.fromDate,
    tm.tillDate,
    tm.showtime
FROM
    TheaterMovies tm
JOIN
    Theaters t ON tm.theaterId = t.theaterId
JOIN
    Movies m ON tm.moviesId = m.movieId
WHERE
    tm.theaterId = :theaterId
    AND DATE(tm.showtime) = '2023-01-01';
