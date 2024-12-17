CREATE DATABASE INVENTORY_MANAGEMENT_SYSTEM;

CREATE TABLE Products(

    ProductID INT PRIMARY KEY IDENTITY(1,1), 
    Name NVARCHAR(100) NOT NULL,     
    SKU NVARCHAR(50) UNIQUE NOT NULL,     
    Category NVARCHAR(50) NULL,            
    Quantity INT DEFAULT 0,              
    UnitPrice DECIMAL(10,2) NULL,        
    Barcode NVARCHAR(50) NULL,           
    CreatedAt DATETIME DEFAULT GETDATE(),   
    UpdatedAt DATETIME DEFAULT GETDATE()    
);
CREATE TABLE Suppliers (
    SupplierID INT PRIMARY KEY IDENTITY(1,1),
    SupplierName NVARCHAR(100) NOT NULL,      
    ContactName NVARCHAR(100) NULL,          
    Phone NVARCHAR(15) NULL,               
    Email NVARCHAR(100) NULL,              
    Address NVARCHAR(255) NULL              
);
CREATE TABLE PurchaseOrders (
    PurchaseOrderID INT PRIMARY KEY IDENTITY(1,1),     
    SupplierID INT FOREIGN KEY REFERENCES Suppliers(SupplierID), 
    OrderDate DATETIME DEFAULT GETDATE(),              
    Status NVARCHAR(20) CHECK (Status IN ('Pending', 'Completed', 'Cancelled')), 
    TotalAmount DECIMAL(10, 2) NULL                 
);
CREATE TABLE PurchaseOrderDetails (
    PODetailID INT PRIMARY KEY IDENTITY(1,1), 
    PurchaseOrderID INT FOREIGN KEY REFERENCES PurchaseOrders(PurchaseOrderID), 
    ProductID INT FOREIGN KEY REFERENCES Products(ProductID), 
    Quantity INT NOT NULL, 
    UnitPrice DECIMAL(10,2) NOT NULL 
);
CREATE TABLE SalesOrders (
    SalesOrderID INT PRIMARY KEY IDENTITY(1,1), 
    CustomerName NVARCHAR(100) NULL, 
    OrderDate DATETIME DEFAULT GETDATE(), 
    Status NVARCHAR(20) CHECK (Status IN ('Pending', 'Shipped', 'Cancelled')), 
    TotalAmount DECIMAL(10,2) NULL 
);
CREATE TABLE SalesOrderDetails (
    SODetailID INT PRIMARY KEY IDENTITY(1,1), 
    SalesOrderID INT FOREIGN KEY REFERENCES SalesOrders(SalesOrderID), 
    ProductID INT FOREIGN KEY REFERENCES Products(ProductID), 
    Quantity INT NOT NULL, 
    UnitPrice DECIMAL(10,2) NOT NULL 
);
CREATE TABLE StockMovements (
    MovementID INT PRIMARY KEY IDENTITY(1,1),
    ProductID INT FOREIGN KEY REFERENCES Products(ProductID),
    MovementType NVARCHAR(20) CHECK (MovementType IN ('IN', 'OUT', 'ADJUSTMENT')),
    Quantity INT NOT NULL,
    MovementDate DATETIME DEFAULT GETDATE(),
    Description NVARCHAR(255) NULL
);
CREATE TABLE Users (
    UserID INT PRIMARY KEY IDENTITY(1,1),
    Username NVARCHAR(50) UNIQUE NOT NULL,
    PasswordHash NVARCHAR(255) NOT NULL,
    Role NVARCHAR(20) CHECK (Role IN ('Admin', 'Manager', 'Staff')),
    CreatedAt DATETIME DEFAULT GETDATE()
);
CREATE TABLE AuditLogs (
    LogID INT PRIMARY KEY IDENTITY(1,1),
    UserID INT FOREIGN KEY REFERENCES Users(UserID),
    Action NVARCHAR(100) NOT NULL,
    TableAffected NVARCHAR(50) NULL,
    ActionTime DATETIME DEFAULT GETDATE(),
    Description NVARCHAR(255) NULL
);
CREATE TABLE Categories (
    CategoryID INT PRIMARY KEY IDENTITY(1,1),
    CategoryName NVARCHAR(100) UNIQUE NOT NULL,
    Description NVARCHAR(255)
);

