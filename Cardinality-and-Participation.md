### **Cardinality and Participation in Database Design**

Cardinality and participation are two fundamental concepts in **Entity-Relationship (ER) modeling** that define the relationship between entities in a database.

---

### **1. Cardinality**

Cardinality refers to the **number of instances** of an entity that can be associated with instances of another entity in a relationship. It defines the **maximum** number of times an entity can be related to another entity.

#### **Types of Cardinality:**

1. **One-to-One (1:1)**

   - Each entity in set A is related to at most one entity in set B, and vice versa.
   - Example: A **passport** is assigned to only one **person**, and each person has only one passport.

2. **One-to-Many (1:M)**

   - One entity in set A can be associated with multiple entities in set B, but each entity in B is associated with only one in A.
   - Example: A **teacher** can teach multiple **courses**, but each course is taught by only one teacher.

3. **Many-to-Many (M:N)**
   - Multiple entities in set A can be related to multiple entities in set B.
   - Example: **Students** can enroll in multiple **courses**, and each course can have multiple students.

---

### **2. Participation**

Participation specifies whether **all** instances of an entity must be involved in a relationship or not. It defines the **minimum** number of times an entity must be related to another entity.

#### **Types of Participation:**

1. **Total Participation**

   - Every instance of an entity **must** participate in the relationship.
   - Represented by a **double line** in an ER diagram.
   - Example: Every **employee** must be assigned to a **department**.

2. **Partial Participation**
   - Some instances of an entity may **not** be related to another entity.
   - Represented by a **single line** in an ER diagram.
   - Example: Not all **customers** may have placed an **order**.

---

### **Cardinality vs. Participation**

| Feature        | Cardinality                                                               | Participation                                            |
| -------------- | ------------------------------------------------------------------------- | -------------------------------------------------------- |
| Definition     | Maximum number of entity instances that can participate in a relationship | Minimum number of entity instances that must participate |
| Types          | 1:1, 1:M, M:N                                                             | Total, Partial                                           |
| Representation | Numbers (e.g., 1, M, N)                                                   | Single or double lines                                   |
| Example        | A teacher teaches multiple students (1:M)                                 | Every employee must be assigned to a department (Total)  |

---

### **E-R Diagram Design for Musicana Records Database**

This problem requires designing an **Entity-Relationship (ER) diagram** for managing musicians, albums, instruments, and songs.

---

## **Step 1: Identify Entities and Attributes**

### **Entities and Their Attributes**

1. **Musician**

   - **Musician_ID** (Primary Key)
   - **Name**
   - **Street**
   - **City**
   - **Phone_Number**

2. **Instrument**

   - **Instrument_Name** (Primary Key)
   - **Musical_Key**

3. **Album**

   - **Album_ID** (Primary Key)
   - **Title**
   - **Copyright_Date**
   - **Producer_ID** (Foreign Key from Musician)

4. **Song**

   - **Song_ID** (Primary Key)
   - **Title**
   - **Author**
   - **Album_ID** (Foreign Key from Album)

5. **Plays (Associative Entity)**

   - **Musician_ID** (Foreign Key from Musician)
   - **Instrument_Name** (Foreign Key from Instrument)

6. **Performs (Associative Entity)**
   - **Musician_ID** (Foreign Key from Musician)
   - **Song_ID** (Foreign Key from Song)

---

## **Step 2: Define Relationships and Cardinality**

1. **Musician ↔ Instrument** (**M:N**)

   - A musician can play multiple instruments, and an instrument can be played by multiple musicians.
   - Implemented using the **Plays** relationship.

2. **Album ↔ Song** (**1:M**)

   - Each album has multiple songs, and each song belongs to exactly **one album**.

3. **Song ↔ Musician (Performs Relationship)** (**M:N**)

   - A song can be performed by multiple musicians, and a musician can perform multiple songs.
   - Implemented using the **Performs** relationship.

4. **Musician ↔ Album (Producer Role)** (**1:M**)
   - Each album has **exactly one musician** as its producer.
   - A musician may produce multiple albums.
   - Implemented by storing **Producer_ID** in the **Album** table.

---

## **Step 3: Database Schema (Tables)**

```sql
CREATE TABLE Musician (
    Musician_ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Street VARCHAR(100),
    City VARCHAR(50),
    Phone_Number VARCHAR(20)
);

CREATE TABLE Instrument (
    Instrument_Name VARCHAR(50) PRIMARY KEY,
    Musical_Key VARCHAR(10)
);

CREATE TABLE Album (
    Album_ID INT PRIMARY KEY,
    Title VARCHAR(100),
    Copyright_Date YEAR,
    Producer_ID INT,
    FOREIGN KEY (Producer_ID) REFERENCES Musician(Musician_ID)
);

CREATE TABLE Song (
    Song_ID INT PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Album_ID INT NOT NULL,
    FOREIGN KEY (Album_ID) REFERENCES Album(Album_ID)
);

CREATE TABLE Plays (
    Musician_ID INT,
    Instrument_Name VARCHAR(50),
    PRIMARY KEY (Musician_ID, Instrument_Name),
    FOREIGN KEY (Musician_ID) REFERENCES Musician(Musician_ID),
    FOREIGN KEY (Instrument_Name) REFERENCES Instrument(Instrument_Name)
);

CREATE TABLE Performs (
    Musician_ID INT,
    Song_ID INT,
    PRIMARY KEY (Musician_ID, Song_ID),
    FOREIGN KEY (Musician_ID) REFERENCES Musician(Musician_ID),
    FOREIGN KEY (Song_ID) REFERENCES Song(Song_ID)
);
```

---

## **Step 4: Participation Constraints**

- **Total Participation**:

  - Every **song must belong to one album** → **Enforced by `NOT NULL` on `Album_ID` in `Song` table**.
  - Every **album must have a producer** → **Enforced by `NOT NULL` on `Producer_ID` in `Album` table**.

- **Partial Participation**:
  - A musician **may or may not play instruments** → No constraint on `Plays` table.
  - A musician **may or may not perform a song** → No constraint on `Performs` table.

---

## **Step 5: Summary of Relationships and Constraints**

| Relationship                | Cardinality | Participation                             |
| --------------------------- | ----------- | ----------------------------------------- |
| Musician ↔ Instrument       | **M:N**     | Partial                                   |
| Musician ↔ Song (Performs)  | **M:N**     | Partial                                   |
| Album ↔ Song                | **1:M**     | Total (Each song must belong to an album) |
| Musician ↔ Album (Producer) | **1:M**     | Total (Each album must have a producer)   |

---

### **E-R Diagram Design for Real Estate Firm**

This problem requires designing an **Entity-Relationship (ER) diagram** to model the real estate firm’s database.

---

## **Step 1: Identify Entities and Attributes**

### **Entities**

1. **SalesOffice**

   - Office_Number (Primary Key)
   - Location

2. **Employee**

   - Employee_ID (Primary Key)
   - Employee_Name
   - Office_Number (Foreign Key from SalesOffice)

3. **Property**

   - Property_ID (Primary Key)
   - Address
   - City
   - State
   - Zip_Code
   - Office_Number (Foreign Key from SalesOffice)

4. **Owner**

   - Owner_ID (Primary Key)
   - Owner_Name

5. **Owns (Associative Entity)**
   - Owner_ID (Foreign Key from Owner)
   - Property_ID (Foreign Key from Property)
   - Percent_Owned (Stores percentage of ownership)

---

## **Step 2: Define Relationships and Cardinality**

1. **SalesOffice ↔ Employee** (**1:M**)

   - A sales office can have multiple employees, but an employee must belong to only one office.

2. **SalesOffice ↔ Manager (Employee as Manager Role)** (**1:1**)

   - Each sales office has one **manager**, and an employee cannot manage multiple offices.

3. **SalesOffice ↔ Property** (**1:M**)

   - Each property must be listed under one sales office, but an office can list multiple properties.

4. **Owner ↔ Property** (**M:N**)
   - A property can have multiple owners, and an owner can own multiple properties.
   - **Percentage Ownership** is tracked in the **Owns** relationship.

---

## **Step 3: Database Schema (Tables)**

```sql
CREATE TABLE SalesOffice (
    Office_Number INT PRIMARY KEY,
    Location VARCHAR(255)
);

CREATE TABLE Employee (
    Employee_ID INT PRIMARY KEY,
    Employee_Name VARCHAR(100),
    Office_Number INT,
    FOREIGN KEY (Office_Number) REFERENCES SalesOffice(Office_Number)
);

CREATE TABLE Property (
    Property_ID INT PRIMARY KEY,
    Address VARCHAR(255),
    City VARCHAR(100),
    State VARCHAR(50),
    Zip_Code VARCHAR(10),
    Office_Number INT NOT NULL,
    FOREIGN KEY (Office_Number) REFERENCES SalesOffice(Office_Number)
);

CREATE TABLE Owner (
    Owner_ID INT PRIMARY KEY,
    Owner_Name VARCHAR(100)
);

CREATE TABLE Owns (
    Owner_ID INT,
    Property_ID INT,
    Percent_Owned DECIMAL(5,2),
    PRIMARY KEY (Owner_ID, Property_ID),
    FOREIGN KEY (Owner_ID) REFERENCES Owner(Owner_ID),
    FOREIGN KEY (Property_ID) REFERENCES Property(Property_ID)
);
```

---

## **Step 4: Participation Constraints**

- **Total Participation**:

  - Each **property must be listed with one sales office** → **Enforced by `NOT NULL` on `Office_Number` in `Property` table**.
  - Each **sales office must have a manager** → **Can be enforced by adding a Manager_ID in `SalesOffice` referencing `Employee_ID`**.

- **Partial Participation**:
  - A sales office **may or may not** have properties listed.
  - An employee **may or may not** manage an office.
  - A property **may have zero or more owners**.

---

## **Step 5: Summary of Relationships and Constraints**

| Relationship                     | Cardinality | Participation                               |
| -------------------------------- | ----------- | ------------------------------------------- |
| SalesOffice ↔ Employee           | **1:M**     | Total for Employee, Partial for SalesOffice |
| SalesOffice ↔ Manager (Employee) | **1:1**     | Total (Each office must have a manager)     |
| SalesOffice ↔ Property           | **1:M**     | Total for Property, Partial for SalesOffice |
| Owner ↔ Property                 | **M:N**     | Partial                                     |

---

### **E-R Diagram Design for General Hospital System**

This problem requires designing an **Entity-Relationship (ER) diagram** for managing hospital wards, patients, consultants, nurses, and drugs.

---

## **Step 1: Identify Entities and Attributes**

### **Entities and Their Attributes**

1. **Ward**

   - **Ward_ID** (Primary Key)
   - **Name**

2. **Patient**

   - **Patient_ID** (Primary Key)
   - **Name**
   - **Date_Of_Birth**
   - **Ward_ID** (Foreign Key from Ward)

3. **Consultant**

   - **Consultant_ID** (Primary Key)
   - **Name**

4. **Examines (Associative Entity)**

   - **Patient_ID** (Foreign Key from Patient)
   - **Consultant_ID** (Foreign Key from Consultant)

5. **Nurse**

   - **Nurse_ID** (Primary Key)
   - **Name**
   - **Number**
   - **Address**
   - **Ward_ID** (Foreign Key from Ward)

6. **Drug**

   - **Drug_Code** (Primary Key)
   - **Recommended_Dosage**
   - **Brand_Name**

7. **Administers (Associative Entity)**
   - **Nurse_ID** (Foreign Key from Nurse)
   - **Patient_ID** (Foreign Key from Patient)
   - **Drug_Code** (Foreign Key from Drug)
   - **Dosage**
   - **Date_Time** (Timestamp of administration)

---

## **Step 2: Define Relationships and Cardinality**

1. **Ward ↔ Patient** (**1:M**)

   - A ward can host multiple patients, but each patient belongs to exactly one ward.

2. **Patient ↔ Consultant** (**M:N**)

   - Each patient is assigned one primary consultant but may be examined by multiple consultants.
   - A consultant may be assigned to multiple patients.

3. **Ward ↔ Nurse** (**1:M**)

   - Each ward has multiple nurses, but each nurse must be assigned to only one ward.

4. **Ward ↔ Supervisor Nurse** (**1:1**)

   - Each ward has exactly **one supervising nurse**, and a nurse cannot supervise multiple wards.

5. **Nurse ↔ Patient ↔ Drug (Administers Relationship)** (**M:N**)
   - A nurse administers drugs to multiple patients, and a patient may receive multiple drugs.
   - **Dosage and Date_Time** are recorded for each administration.

---

## **Step 3: Database Schema (Tables)**

```sql
CREATE TABLE Ward (
    Ward_ID INT PRIMARY KEY,
    Name VARCHAR(100)
);

CREATE TABLE Patient (
    Patient_ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Date_Of_Birth DATE,
    Ward_ID INT NOT NULL,
    FOREIGN KEY (Ward_ID) REFERENCES Ward(Ward_ID)
);

CREATE TABLE Consultant (
    Consultant_ID INT PRIMARY KEY,
    Name VARCHAR(100)
);

CREATE TABLE Examines (
    Patient_ID INT,
    Consultant_ID INT,
    PRIMARY KEY (Patient_ID, Consultant_ID),
    FOREIGN KEY (Patient_ID) REFERENCES Patient(Patient_ID),
    FOREIGN KEY (Consultant_ID) REFERENCES Consultant(Consultant_ID)
);

CREATE TABLE Nurse (
    Nurse_ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Number VARCHAR(20),
    Address VARCHAR(255),
    Ward_ID INT NOT NULL,
    FOREIGN KEY (Ward_ID) REFERENCES Ward(Ward_ID)
);

CREATE TABLE Drug (
    Drug_Code VARCHAR(20) PRIMARY KEY,
    Recommended_Dosage VARCHAR(50),
    Brand_Name VARCHAR(100)
);

CREATE TABLE Administers (
    Nurse_ID INT,
    Patient_ID INT,
    Drug_Code VARCHAR(20),
    Dosage VARCHAR(50),
    Date_Time DATETIME,
    PRIMARY KEY (Nurse_ID, Patient_ID, Drug_Code, Date_Time),
    FOREIGN KEY (Nurse_ID) REFERENCES Nurse(Nurse_ID),
    FOREIGN KEY (Patient_ID) REFERENCES Patient(Patient_ID),
    FOREIGN KEY (Drug_Code) REFERENCES Drug(Drug_Code)
);
```

---

## **Step 4: Participation Constraints**

- **Total Participation**:

  - Every **patient must be hosted by a ward** → **Enforced by `NOT NULL` on `Ward_ID` in `Patient`**.
  - Every **nurse must serve in one ward** → **Enforced by `NOT NULL` on `Ward_ID` in `Nurse`**.
  - Every **ward must have a supervising nurse** → **Enforced by adding a `Supervisor_Nurse_ID` foreign key in `Ward`**.

- **Partial Participation**:
  - A consultant **may not be assigned to a patient**.
  - A patient **may not receive any drugs**.
  - A nurse **may not administer drugs**.

---

## **Step 5: Summary of Relationships and Constraints**

| Relationship            | Cardinality | Participation                       |
| ----------------------- | ----------- | ----------------------------------- |
| Ward ↔ Patient          | **1:M**     | Total for Patient, Partial for Ward |
| Patient ↔ Consultant    | **M:N**     | Partial                             |
| Ward ↔ Nurse            | **1:M**     | Total for Nurse, Partial for Ward   |
| Ward ↔ Supervisor Nurse | **1:1**     | Total                               |
| Nurse ↔ Patient ↔ Drug  | **M:N**     | Partial                             |

---

### **E-R Diagram Design for Airline Management System**

This problem requires designing an **Entity-Relationship (ER) diagram** for managing airline companies, employees, aircraft, routes, crew, and transactions.

---

## **Step 1: Identify Entities and Attributes**

### **Entities and Their Attributes**

1. **Airline**

   - **Airline_ID** (Primary Key)
   - **Name**
   - **Address**
   - **Contact_Person**
   - **Phone_Number**

2. **Employee**

   - **Employee_ID** (Primary Key)
   - **Name**
   - **Address**
   - **Birthday (Day, Month, Year)**
   - **Gender**
   - **Position**
   - **Qualifications**
   - **Airline_ID** (Foreign Key from Airline)

3. **Aircraft**

   - **Aircraft_ID** (Primary Key)
   - **Model**
   - **Capacity**
   - **Airline_ID** (Foreign Key from Airline)

4. **Route**

   - **Route_ID** (Primary Key)
   - **Origin**
   - **Destination**
   - **Distance**
   - **Classification** (e.g., Domestic, International)

5. **Assigned_To (Associative Entity)**

   - **Aircraft_ID** (Foreign Key from Aircraft)
   - **Route_ID** (Foreign Key from Route)
   - **Num_Passengers**
   - **Price_Per_Passenger**
   - **Departure_Date_Time**
   - **Arrival_Date_Time**
   - **Travel_Duration**

6. **Crew**

   - **Crew_ID** (Primary Key)
   - **Major_Pilot**
   - **Assistant_Pilot**
   - **Hostess1**
   - **Hostess2**
   - **Aircraft_ID** (Foreign Key from Aircraft)

7. **Transaction**
   - **Transaction_ID** (Primary Key)
   - **Transaction_Date**
   - **Description**
   - **Amount**
   - **Transaction_Type** (Buy/Sell)
   - **Airline_ID** (Foreign Key from Airline)

---

## **Step 2: Define Relationships and Cardinality**

1. **Airline ↔ Employee** (**1:M**)

   - Each airline has multiple employees, but each employee works for only one airline.

2. **Airline ↔ Aircraft** (**1:M**)

   - Each airline owns multiple aircraft, but each aircraft belongs to one airline.

3. **Aircraft ↔ Route** (**M:N**)

   - An aircraft can operate on multiple routes, and each route may have multiple aircraft assigned.
   - Implemented using the **Assigned_To** relationship.

4. **Aircraft ↔ Crew** (**1:1**)

   - Each aircraft has **one assigned crew**, and each crew is assigned to **one aircraft**.

5. **Airline ↔ Transaction** (**1:M**)
   - Each airline has multiple transactions, but each transaction belongs to one airline.

---

## **Step 3: Database Schema (Tables)**

```sql
CREATE TABLE Airline (
    Airline_ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Address VARCHAR(255),
    Contact_Person VARCHAR(100),
    Phone_Number VARCHAR(20)
);

CREATE TABLE Employee (
    Employee_ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Address VARCHAR(255),
    Birthday DATE,
    Gender VARCHAR(10),
    Position VARCHAR(100),
    Qualifications TEXT,
    Airline_ID INT,
    FOREIGN KEY (Airline_ID) REFERENCES Airline(Airline_ID)
);

CREATE TABLE Aircraft (
    Aircraft_ID INT PRIMARY KEY,
    Model VARCHAR(100),
    Capacity INT,
    Airline_ID INT,
    FOREIGN KEY (Airline_ID) REFERENCES Airline(Airline_ID)
);

CREATE TABLE Route (
    Route_ID INT PRIMARY KEY,
    Origin VARCHAR(100),
    Destination VARCHAR(100),
    Distance INT,
    Classification VARCHAR(50)
);

CREATE TABLE Assigned_To (
    Aircraft_ID INT,
    Route_ID INT,
    Num_Passengers INT,
    Price_Per_Passenger DECIMAL(10,2),
    Departure_Date_Time DATETIME,
    Arrival_Date_Time DATETIME,
    Travel_Duration TIME,
    PRIMARY KEY (Aircraft_ID, Route_ID),
    FOREIGN KEY (Aircraft_ID) REFERENCES Aircraft(Aircraft_ID),
    FOREIGN KEY (Route_ID) REFERENCES Route(Route_ID)
);

CREATE TABLE Crew (
    Crew_ID INT PRIMARY KEY,
    Major_Pilot VARCHAR(100),
    Assistant_Pilot VARCHAR(100),
    Hostess1 VARCHAR(100),
    Hostess2 VARCHAR(100),
    Aircraft_ID INT UNIQUE,
    FOREIGN KEY (Aircraft_ID) REFERENCES Aircraft(Aircraft_ID)
);

CREATE TABLE Transaction (
    Transaction_ID INT PRIMARY KEY,
    Transaction_Date DATE,
    Description TEXT,
    Amount DECIMAL(15,2),
    Transaction_Type ENUM('Buy', 'Sell'),
    Airline_ID INT,
    FOREIGN KEY (Airline_ID) REFERENCES Airline(Airline_ID)
);
```

---

## **Step 4: Participation Constraints**

- **Total Participation**:

  - Each **aircraft must belong to one airline** → **Enforced by `NOT NULL` on `Airline_ID` in `Aircraft`**.
  - Each **crew must be assigned to one aircraft** → **Enforced by `UNIQUE` constraint on `Aircraft_ID` in `Crew`**.
  - Each **transaction must belong to an airline** → **Enforced by `NOT NULL` on `Airline_ID` in `Transaction`**.

- **Partial Participation**:
  - A route **may not have aircraft assigned**.
  - An airline **may not have transactions recorded**.
  - An employee **may or may not exist in a specific position**.

---

## **Step 5: Summary of Relationships and Constraints**

| Relationship          | Cardinality | Participation                           |
| --------------------- | ----------- | --------------------------------------- |
| Airline ↔ Employee    | **1:M**     | Total for Employee, Partial for Airline |
| Airline ↔ Aircraft    | **1:M**     | Total for Aircraft, Partial for Airline |
| Aircraft ↔ Route      | **M:N**     | Partial                                 |
| Aircraft ↔ Crew       | **1:1**     | Total                                   |
| Airline ↔ Transaction | **1:M**     | Partial                                 |

---
