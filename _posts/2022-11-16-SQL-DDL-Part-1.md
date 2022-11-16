---
layout: single
title: "SQL Data Definition Language - Part 1"
output: html_document
tags: [sql]
comments: false
author_profile: true
toc: true
toc_sticky: true
excerpt: "Introduction to using SQL data definition language to create tables with relationships."
---

{{ page.excerpt }}

### Description
An introduction to data definition language by creating tables and its relationships based on the scenario.

---------------------------
### Problems

#### 1. DDL from a Logical Data Model & Data Dictionary Specification.  
Create the tables and relationships based on the following logical data model and data dictionary.
```
CREATE TABLE A1B_Band 
(
	BandId	INT PRIMARY KEY IDENTITY (1,1),
	BandName VARCHAR(25) NULL,
	FormationDate DATETIME NULL,
)
CREATE TABLE A1B_Album 
(
	AlbumId INT PRIMARY KEY IDENTITY (1,1),
	AlbumTitle VARCHAR(30) NULL,
	ReleaseDate DATETIME NULL,
	ProductionCost DECIMAL(10,2) NULL,
	BandId INT FOREIGN KEY REFERENCES A1B_Band(BandId),
)
```
![SQL1](\assets\images\sql3\1.png){: .align-center} 

---

#### 2. DDL from a Conceptual Data Model 
Create the required tables and relationships based on the following conceptual data model, making any necessary adjustments or additions to implement it as a 3NF logical and physical model. Make reasonable assumptions about data types for each field, and be sure to include a field that can store the player’s performance (e.g., winnings in $) for each tournament.
```
CREATE TABLE A1B_City
(
	CityId INT PRIMARY KEY IDENTITY (1,1),
	City NVARCHAR(50) NULL,
)

CREATE TABLE A1B_State
(
	StateId INT PRIMARY KEY IDENTITY (1,1),
	State NVARCHAR(50) NULL,
)

CREATE TABLE A1B_Tournament
(
	TournamentId INT PRIMARY KEY IDENTITY (1,1),
	TournamentType NVARCHAR(50) NULL,
	StartDate DATETIME NULL,
	EndDate DATETIME NULL,
	TotalPrizeMoney INT NULL,
	CityId INT FOREIGN KEY REFERENCES A1B_City(CityId),
	StateId INT FOREIGN KEY REFERENCES A1B_State(StateId),
)

CREATE TABLE A1B_Player
(
	PlayerId INT PRIMARY KEY IDENTITY(1,1),
	FirstName VARCHAR(50) NULL,
	LastName VARCHAR (50) NULL,
	Email NVARCHAR(50),
	PhoneNumber NVARCHAR(50) NULL,
)

CREATE TABLE A1B_Perfomance
(
	PerformanceId INT PRIMARY KEY IDENTITY (1,1),
	SkillRating DECIMAL(8,2),
	PrizeReceived DECIMAL(8,2),
	TournamentId INT FOREIGN KEY REFERENCES A1B_Tournament(TournamentId),
	PlayerId INT FOREIGN KEY REFERENCES A1B_Player(PlayerId),
)

```
![SQL2](\assets\images\sql3\2.png){: .align-center}  

---

#### 3. DDL from a Narrative Case  
Create the required tables and relationships based on the following requirements narrative.
We need a better system to track our product inventory across locations. Right now, we’re tracking everything on a spreadsheet, but it’s getting too big to manage and is difficult to share. We want to keep it simple for now, and maybe invest in a big ERP system in the future. We need to track the basic information about each location (there’s a code we use, the location, and the type of facility that we have there). As far as the products, we just want to track what it is, how we measure it, and how many units we have in stock currently at each location with the last count date (no need for inventory history, but if you want to put that in the design, I suppose that’s ok).
```
CREATE TABLE A1B_Location
(
	LocId INT PRIMARY KEY IDENTITY (1,1),
	Location NVARCHAR(50) NULL,
	LocationType NVARCHAR(50) NULL,
)

CREATE TABLE A1B_Product
(
	ProductId INT PRIMARY KEY IDENTITY (1,1),
	DescriptionChar NVARCHAR(50) NULL,
	UnitMeasurement NVARCHAR(50) NULL,
)

CREATE TABLE A1B_ProductLocation
(
	ProductLocId INT PRIMARY KEY IDENTITY (1,1),
	Inventory INT NULL,
	CountDate DATE NULL,
	ProductId INT FOREIGN KEY REFERENCES A1B_Product(ProductId),
	LocId INT FOREIGN KEY REFERENCES A1B_Location(LocId),
)
```
![SQL3](\assets\images\sql3\3.png){: .align-center} 

