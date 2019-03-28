
-- Create Table Products
CREATE TABLE Products (
    id INT UNSIGNED AUTO_INCREMENT,
    imagePath VARCHAR(75) NOT NULL UNIQUE,
    productName VARCHAR(45) NOT NULL UNIQUE,
    productDesc VARCHAR(200) NOT NULL,
    price DECIMAL(8,2) NOT NULL,
    PRIMARY KEY(id)
);

-- Create Table Services
CREATE TABLE Services (
    id INT UNSIGNED AUTO_INCREMENT,
    imagePath VARCHAR(75) NOT NULL UNIQUE,
    serviceName VARCHAR(45) NOT NULL UNIQUE,
    serviceDesc VARCHAR(200) NOT NULL,
    fee DECIMAL(8,2) NOT NULL,
    PRIMARY KEY(id)
);

-- Create Table Employees 
CREATE TABLE Employees (
    id INT UNSIGNED AUTO_INCREMENT,
    fname VARCHAR(50) NOT NULL,
    lname VARCHAR(50) NOT NULL,    
    PRIMARY KEY(id)
);

-- Create Table Equipments 
CREATE TABLE Equipments (
    id INT UNSIGNED AUTO_INCREMENT,
    equipName VARCHAR(45) NOT NULL UNIQUE,
    equipDesc VARCHAR(200) NOT NULL,
    PRIMARY KEY(id)
);

-- Create Table CustomerAccounts 
CREATE TABLE CustomerAccounts (
    id INT UNSIGNED AUTO_INCREMENT,
    email VARCHAR(100) NOT NULL UNIQUE,
    username VARCHAR(65) NOT NULL UNIQUE,
    password VARCHAR(60) NOT NULL,
    PRIMARY KEY(id)
);

-- Create Table CustomerAddresses 
CREATE TABLE CustomerAddresses (
    id INT UNSIGNED AUTO_INCREMENT,
    customerID INT UNSIGNED NOT NULL,
    fname VARCHAR(50) NOT NULL,
    lname VARCHAR(50) NOT NULL,
    address1 VARCHAR(200) NOT NULL,
    address2 VARCHAR(200),
    city VARCHAR(65) NOT NULL,
    state CHAR(2) NOT NULL,
    zipcode CHAR(5) NOT NULL,
    PRIMARY KEY(id),
    FOREIGN KEY(customerID) REFERENCES CustomerAccounts(id)
        ON DELETE CASCADE
);

-- Create Table CustomerBilling 
CREATE TABLE CustomerBilling (
    id INT UNSIGNED AUTO_INCREMENT,
    customerID INT UNSIGNED NOT NULL,
    addressID INT UNSIGNED NOT NULL,
    cardType VARCHAR(40) NOT NULL,
    cardNumber VARCHAR(24) NOT NULL,
    cardExpireMonth VARCHAR(2) NOT NULL,
    cardExpireYear VARCHAR(4) NOT NULL,
    PRIMARY KEY(id),
    FOREIGN KEY(customerID) REFERENCES CustomerAccounts(id)
            ON DELETE CASCADE,
    FOREIGN KEY(addressID) REFERENCES CustomerAddresses(id)
            ON DELETE CASCADE
);

-- Create Table CustomerOrders 
CREATE TABLE CustomerOrders (
    id INT UNSIGNED AUTO_INCREMENT,
    customerID INT UNSIGNED NOT NULL,
    addressID INT UNSIGNED NOT NULL,
    billingID INT UNSIGNED NOT NULL,
    dateOrdered DATE NOT NULL,    
    totalPrice DECIMAL(10,2) NOT NULL,    
    PRIMARY KEY(id),
    FOREIGN KEY(customerID) REFERENCES CustomerAccounts(id)
            ON DELETE CASCADE,
    FOREIGN KEY(addressID) REFERENCES CustomerAddress(id)
            ON DELETE CASCADE,
    FOREIGN KEY(billingID) REFERENCES CustomerBilling(id)
            ON DELETE CASCADE
);

-- Create Table ProdOrderDetails 
CREATE TABLE ProdOrderDetails (
    orderID INT UNSIGNED NOT NULL,
    prodID INT UNSIGNED NOT NULL,
    quantity INT(3) UNSIGNED NOT NULL,
    unitPrice DECIMAL(8,2) NOT NULL,
    subTotal DECIMAL(10,2) NOT NULL,
    FOREIGN KEY(orderID) REFERENCES CustomerOrders(id)
        ON DELETE CASCADE,
    FOREIGN KEY(prodID) REFERENCES Products(id)
            ON DELETE CASCADE
);

-- Create Table ServOrderDetails 
CREATE TABLE ServOrderDetails (
    orderID INT UNSIGNED NOT NULL,
    servID INT UNSIGNED NOT NULL,    
    scheduledDate DATE NOT NULL,    
    subTotal DECIMAL(10,2) NOT NULL,
    FOREIGN KEY(orderID) REFERENCES CustomerOrders(id)
        ON DELETE CASCADE,
    FOREIGN KEY(servID) REFERENCES Services(id)
            ON DELETE CASCADE
);