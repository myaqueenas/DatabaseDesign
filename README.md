# DatabaseDesign
## A conceptual model of data used by Cars-4-U sales staff, A logical model for a future relational database
### Youngjin Noh

#### 1. Introduction

Imagine a fictitious car sales company called Cars-4-U has approached you wanting a database to support their showroom staff. To begin with, they require a very short report containing just:
 
·       A conceptual model of data used by Cars-4-U sales staff
·       A logical model for a future relational database
They require no introduction or explanation text, just these two models.

They have kindly written a description of the data used by sales staff - a “case”. As with all written cases for designing a database, there is information which is absolutely central but also information which is missing, ambiguous (‘confusing’) and redundant (‘not needed’).
 
### 2. Description of the case
 
Cars-4-U buys and sells cars that have been previously owned. The company currently has several text-based and computerised systems that manage data related to the company, including customer records and car information. However, much of the current data are duplicated and distributed across the company, and the various systems do not communicate with each other. Cars-4-U needs a relational database to support and record details of automobile sales and purchases by the company’s sales staff.
 
The sales staff members are known by their first name and surname. They all work in the same Cars-4-U showroom (on Pennistone Road, Sheffield, S2 4RT) although the company may open new showrooms at different locations in the future. At that point, sales staff may work at various showrooms depending on staff availability. There would always be at least one sales staff member at a showroom. Sales staff members have at least one phone number, often including a landline telephone number and a mobile phone number. They also have an email address.
 
The database needs to store each sales person’s purchase and sale of cars each month since their salary is based on their monthly net profit (basically, the total price of all cars sold minus the total of all original purchase prices). Taxes are currently dealt with by a separate system and the database does not need to consider this.
 
Sales staff members either buy cars directly from a seller or when a customer part exchange occurs. That is, when a car owned by a customer is sold to Cars-4-U to reduce the price of the car being purchased (i.e. price paid by customer is the purchase price of the ‘new’ vehicle minus amount received in part-exchange for their ‘old’ vehicle). Cars are not sold for less than their original purchase price and part-exchange cars are never bought for more than the price of the ‘new’ car being purchased. Although it’s possible for a car to return to the showroom after being previously sold to a customer, this is not common and the sales staff would treat the returning car like a different car.
 
The type of purchase (e.g. bought from a seller or part-exchange), the date the car was purchased, the amount that the car was ‘bought for’, the amount the car was ‘advertised at’ in the showroom and the amount the car was eventually ‘sold for’ needs to be captured. Please note that the advertised price of a car might be reduced if Cars-4-U if the car remains in the showroom or forecourt too long or Cars-4-U has a ‘Sale’. A history of advertised prices for each car needs to be kept.
 
The sales staff members need access to information about each car for sale. This includes the date each car was first registered, its unique registration number (e.g. B5 PHS), its age (current date minus registration date, mileage (number of miles or km covered since new), and service history. The service history is a list of dates when the car was serviced and the mileage at that date. Details about the number of previous owners also needs to be kept. Finally, although not needed at the moment, the showroom cars are stored/displayed at, needs to be known. This might change if a car is moved between showrooms.
 
Sales staff members need access to details about each car in stock, including the make (e.g. Ford, Toyota), the model (e.g. Focus or Auris), the engine size (e.g. 1.6 litres or 1600cc), the fuel type (e.g. petrol, diesel or hybrid), the transmission type (e.g. manual or automatic) and the body type (e.g. hatchback or saloon).

The details of customers who buy cars, sellers who sell cars and potential customers who visit the showroom need to be stored. Details have to include their title (e.g. Mr., Mrs. or Miss.), forename, surname and address if they sell, purchase or part-exchange a vehicle. Otherise, if they are just visiting a showroom, sales staff might try to capture their first name and/or surname, the date of the visit and some notes. They might make further repeated entries if the customer repeatly visits (which they often do) or they might try finding the customer’s details and edit their notes. Other information which might be collected includes customer contact details.
 
### 3. Deliverables
 
The report for Cars-4-U should contain just two sections:
 
#### Section I: Conceptual Model
 
This section of the report should include just one entity-relationship (E-R) diagram using UML class notation used throughout most of INF6050 and representing the conceptual model of the Cars-4-U case. This conceptual model should include entity properties and identifiers. The conceptual data models should represent the Cars-4-U case as faithfully and accurately as possible.

A useful free tool for generating UML diagrams is UMLET (http://www.umlet.com/), however you are free to produce the UML diagram however you wish. The diagram should not be hand-drawn and be easily readable on a computer screen. 
 
![Figure](https://raw.githubusercontent.com/myaqueenas/DatabaseDesign/main/UML.bmp)

#### Section II: Logical Model

Based on conceptual model you should provide a set of relations that represents your logical data model.

STRONG table
CAR (REGISTRATION_NUMBER, REGISTRATION_DATE, MILEAGE, SERVICE_HISTORY, MANUFACTURER, MODEL, DISPACEMENT, FUEL_TYPE, TRANSMISSION_TYPE, BODY_TYPE, COLOUR, STAFF_NUMBER, SHOWROOM_NUMBER)
PRIMARY_KEY REGISTRATION_NUMBER
CONSTRAINT MODEL NOT NULL
FOREIGN_KEY STAFF_NUMBER NOT NULL REFERENCES STAFF
FOREIGN_KEY CUSTOMER_NUMBER NOT NULL UNIQUE REFERENCES CUSTOMER
FOREIGN_KEY SHOWROOM_NUMBER NOT NULL REFERENCES SHOWROOM	

CUSTOMER (CUSTOMER_NUMBER, NAME, ADDRESS, EMAIL_ADDRESS, DATE_OF_BIRTH, PHONE)
PRIMARY_KEY CUSTOMER_NUMBER
CONSTRAINT SURNAME NOT NULL

STAFF (STAFF_NUMBER, NAME, EMAIL_ADDRESS, DATE_OF_BIRTH, PHONE, SHOWROOM_NUMBER)
PRIMARY_KEY STAFF_NUMBER
CONSTRAINT SURNAME NOT NULL
FOREIGN_KEY SHOWROOM_NUMBER NOT NULL REFERENCES SHOWROOM

SHOWROOM (SHOWROOM_NUMBER, SHOWROOM_NAME)
PRIMARY_KEY SHOWROOM_NUMBER
CONSTRAINT SHOWROOM_NAME UNIQUE

LINKER table
BUYS (CUSTOMER_NUMBER, REGISTRATION_NUMBER, BOUGHT_DATE)
PRIMARY_KEY CUSTOMER_NUMBER, REGISTRATION_NUMBER, BOUGHT_DATE
FOREIGN_KEY CUSTOMER_NUMBER REFERENCES CUSTOMER
FOREIGN_KEY REGISTRATION_NUMBER REFERENCES CAR

SELLS (CUSTOMER_NUMBER, REGISTRATION_NUMBER, SOLD_DATE)
PRIMARY_KEY CUSTOMER_NUMBER, REGISTRATION_NUMBER, SOLD_DATE
FOREIGN_KEY CUSTOMER_NUMBER REFERENCES CUSTOMER
FOREIGN_KEY REGISTRATION_NUMBER REFERENCES CAR

PURCAHES (STAFF_NUMBER, REGISTRATION_NUMBER, PURCHASED_DATE)
PRIMARY_KEY STAFF_NUMBER, REGISTRATION_NUMBER, PURCHASED_DATE
FOREIGN_KEY STAFF_NUMBER REFERENCES STAFF
FOREIGN_KEY REGISTRATION_NUMBER REFERENCES CAR

SELLS (STAFF_NUMBER, REGISTRATION_NUMBER, SOLD_DATE)
PRIMARY_KEY STAFF_NUMBER, REGISTRATION_NUMBER, SOLD_DATE
FOREIGN_KEY STAFF_NUMBER REFERENCES STAFF
FOREIGN_KEY REGISTRATION_NUMBER REFERENCES CAR

BASED AT (STAFF_NUMBER, SHOWROOM_NUMBER)
PRIMARY_KEY STAFF_NUMBER, SHOWROOM_NUMBER
FOREIGN_KEY STAFF_NUMBER REFERENCES STAFF
FOREIGN_KEY SHOWROOM_NUMBER REFERENCES SHOWROOM

STORED AT (REGISTRATION_NUMBER, SHOWROOM_NUMBER, STORED_DATE)
PRIMARY_KEY REGISTRATION_NUMBER, SHOWROOM_NUMBER, STORED_DATE
FOREIGN_KEY REGISTRATION_NUMBER REFERENCES CAR
FOREIGN_KEY SHOWROOM_NUMBER REFERENCES SHOWROOM

WEAK table
NAME(TITLE, SURNAME, FIRSTNAME, CUSTOMER_NUMBER, STAFF_NUMBER, NAME_TYPE)
PRIMARY_KEY CUSTOMER_NUMBER, STAFF_NUMBER, NAME_TYPE 
FOREIGN_KEY CUSTOMER_NUMBER NOT NULL REFERENCES CUSTOMER
FOREIGN_KEY STAFF_NUMBER NOT NULL REFERENCES STAFF
FOREIGN_KEY NAME_TYPE NOT NULL REFERENCES NAME_TYPE_LOOKUP

PHONE (PHONE_NUMBER, PHONE_TYPE, CUSTOMER_NO, STAFF_NO)
PRIMARY_KEY PHONE_NUMBER
FOREIGN_KEY CUSTOMER_NUMBER NOT NULL REFERENCES CUSTOMER
FOREIGN_KEY STAFF_NUMBER NOT NULL REFERENCES STAFF
FOREIGN_KEY PHONE_TYPE NOT NULL REFERENCES PHONE_TYPE_LOOKUP

ADDRESS (CUSTOMER_NUMBER, ADDRESS_TYPE, HOUSE_NAME, ADDRESS_LINE_1, ADDRESS_LINE_2, POST_CODE)
PRIMARY_KEY CUSTOMER_NUMBER, ADDRESS_TYPE 
FOREIGN_KEY CUSTOMER_NUMBER NOT NULL REFERENCES CUSTOMER
FOREIGN_KEY ADDERSS_TYPE NOT NULL REFERENCES ADDRESS_TYPE_LOOKUP

DATE_OF_BIRTH (DAY, MONTH, YEAR, CUSTOMER_NUMBER, STAFF_NUMBER, DATE_OF_BIRTH_TYPE)
PRIMARY_KEY CUSTOMER_NUMBER, STAFF_NUMBER, DATE_OF_BIRTH_TYPE 
FOREIGN_KEY CUSTOMER_NUMBER NOT NULL REFERENCES CUSTOMER
FOREIGN_KEY STAFF_NUMBER NOT NULL REFERENCES STAFF
FOREIGN_KEY DATE_OF_BIRTH_TYPE NOT NULL REFERENCES DATE_OF_BIRTH_TYPE _LOOKUP

LOOK-UP table
NAME_TYPE_LOOKUP (NAME_TYPE, NAME_TYPE_DESCRIPTION)
PRIMARY_KEY NAME_TYPE

PHONE_TYPE_LOOKUP (PHONE_TYPE, PHONE_TYPE_DESCRIPTION)
PRIMARY_KEY PHONE_TYPE

ADDRESS_TYPE_LOOKUP (ADDRESS_TYPE, ADDRESS_TYPE_DESCRIPTION)
PRIMARY_KEY ADDRESS_TYPE

DATE_OF_BIRTH_TYPE_LOOKUP (DATE_OF_BIRTH_TYPE, DATE_OF_BIRTH_TYPE_DESCRIPTION)
PRIMARY_KEY DATE_OF_BIRTH_TYPE
