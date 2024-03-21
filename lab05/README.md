# LAB-05

# UPDATE
- UPDATE ifadesi bir tablodaki mevcut kayıtları değiştirmek için kullanılır.

```mysql
UPDATE table_name 
SET column1 = value1, column2 = value2, ... 
WHERE condition;
```

> NOT: Tablodaki kayıtları güncellerken UPDATE ifadesindeki WHERE deyimine dikkat edilmesi gerekir. WHERE deyimi hangi kayıtların güncellenmesi gerektiğini belirtir. WHERE deyimini atlarsanız tablodaki tüm kayıtlar güncellenecektir!

- Customers tablosu:

| CustomerID |            CustomerName            |     ContactName    |            Address            |     City    | PostalCode | Country |
|:----------:|:----------------------------------:|:------------------:|:-----------------------------:|:-----------:|:----------:|:-------:|
|      1     |         Alfreds Futterkiste        |    Maria Anders    |         Obere Str. 57         |    Berlin   |    12209   | Germany |
|      2     | Ana Trujillo Emparedados y helados |    Ana Trujillo    | Avda. de la Constitución 2222 | México D.F. |    5021    |  Mexico |
|      3     |       Antonio Moreno Taquería      |   Antonio Moreno   |         Mataderos 2312        | México D.F. |    5023    |  Mexico |
|      4     |           Around the Horn          |    Thomas Hardy    |        120 Hanover Sq.        |    London   |   WA1 1DP  |    UK   |
|      5     |         Berglunds snabbköp         | Christina Berglund |         Berguvsvägen 8        |    Luleå    |  S-958 22  |  Sweden |

- Bu tabloyu sql komutları ile oluşturalım.

```mysql
CREATE TABLE Customers( 
  CustomerID INT PRIMARY KEY, 
  CustomerName VARCHAR(255), 
  ContactName VARCHAR(255), 
  Address VARCHAR(255), 
  City VARCHAR(255), 
  PostalCode VARCHAR(10), 
  Country VARCHAR(255)
);

INSERT INTO Customers (CustomerID, CustomerName, ContactName, Address, City, PostalCode, Country) VALUES 
(1, 'Alfreds Futterkiste', 'Maria Anders', 'Obere Str. 57', 'Berlin', '12209', 'Germany'), 
(2, 'Ana Trujillo Emparedados y helados', 'Ana Trujillo', 'Avda. de la Constitución 2222', 'México D.F.', '5021', 'Mexico'), 
(3, 'Antonio Moreno Taquería', 'Antonio Moreno', 'Mataderos 2312', 'México D.F.', '5023', 'Mexico'), 
(4, 'Around the Horn', 'Thomas Hardy', '120 Hanover Sq.', 'London', 'WA1 1DP', 'UK'), 
(5, 'Berglunds snabbköp', 'Christina Berglund', 'Berguvsvägen 8', 'Luleå', 'S-958 22', 'Sweden');
```

- Aşağıdaki SQL ifadesi, ilk müşteriyi (CustomerID = 1) yeni bir `ilgili kişi` ve yeni bir `şehir` ile günceller.

```mysql
UPDATE Customers
SET ContactName= 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;
```

- `Customers` tablosundaki seçim artık şu şekilde görünecektir:

| CustomerID |            CustomerName            |     ContactName    |            Address            |     City    | PostalCode | Country |
|:----------:|:----------------------------------:|:------------------:|:-----------------------------:|:-----------:|:----------:|:-------:|
|      1     |   Alfreds FutterkisteFutterkiste   |   Alfred Schmidt   |         Obere Str. 57         |  Frankfurt  |    12209   | Germany |
|      2     | Ana Trujillo Emparedados y helados |    Ana Trujillo    | Avda. de la Constitución 2222 | México D.F. |    5021    |  Mexico |
|      3     |       Antonio Moreno Taquería      |   Antonio Moreno   |         Mataderos 2312        | México D.F. |    5023    |  Mexico |
|      4     |           Around the Horn          |    Thomas Hardy    |        120 Hanover Sq.        |    London   |   WA1 1DP  |    UK   |
|      5     |         Berglunds snabbköp         | Christina Berglund |         Berguvsvägen 8        |    Luleå    |  S-958 22  |  Sweden |


## Çoklu Satır Güncellemesi (UPDATE Multiple Records)
- WHERE, hangi kayıtların güncelleneceğini belirler. 
Aşağıdaki SQL ifadesi, ülkenin "Mexico" olduğu tüm kayıtlar için ContactName'i "Juan" olarak günceller.

```mysql
UPDATE Customers
SET ContactName='Juan'
WHERE Country='Mexico';
```

- `Customers` tablosundaki seçim artık şu şekilde görünecektir:

| CustomerID |            CustomerName            |     ContactName    |            Address            |     City    | PostalCode | Country |
|:----------:|:----------------------------------:|:------------------:|:-----------------------------:|:-----------:|:----------:|:-------:|
|      1     |   Alfreds FutterkisteFutterkiste   |   Alfred Schmidt   |         Obere Str. 57         |  Frankfurt  |    12209   | Germany |
|      2     | Ana Trujillo Emparedados y helados |        Juan        | Avda. de la Constitución 2222 | México D.F. |    5021    |  Mexico |
|      3     |       Antonio Moreno Taquería      |        Juan        |         Mataderos 2312        | México D.F. |    5023    |  Mexico |
|      4     |           Around the Horn          |    Thomas Hardy    |        120 Hanover Sq.        |    London   |   WA1 1DP  |    UK   |
|      5     |         Berglunds snabbköp         | Christina Berglund |         Berguvsvägen 8        |    Luleå    |  S-958 22  |  Sweden |


- WHERE deyimini kullanmazsak tüm kayıtlar güncellenecektir.

```mysql
UPDATE Customers
SET ContactName='Juan';
```

| CustomerID |            CustomerName            | ContactName |            Address            |     City    | PostalCode | Country |
|:----------:|:----------------------------------:|:-----------:|:-----------------------------:|:-----------:|:----------:|:-------:|
|      1     |   Alfreds FutterkisteFutterkiste   |     Juan    |         Obere Str. 57         |  Frankfurt  |    12209   | Germany |
|      2     | Ana Trujillo Emparedados y helados |     Juan    | Avda. de la Constitución 2222 | México D.F. |    5021    |  Mexico |
|      3     |       Antonio Moreno Taquería      |     Juan    |         Mataderos 2312        | México D.F. |    5023    |  Mexico |
|      4     |           Around the Horn          |     Juan    |        120 Hanover Sq.        |    London   |   WA1 1DP  |    UK   |
|      5     |         Berglunds snabbköp         |     Juan    |         Berguvsvägen 8        |    Luleå    |  S-958 22  |  Sweden |



---


# DELETE
- DELETE ifadesi tablodaki mevcut kayıtları silmek için kullanılır.

`DELETE FROM table_name WHERE condition;`

- Aşağıdaki SQL ifadesi "Alfreds Futterkiste" müşterisini "Customers" tablosundan siler:

```mysql
DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
```

- "Customers" tablosu artık şöyle görünür:

| CustomerID |            CustomerName            |     ContactName    |            Address            |     City    | PostalCode | Country |
|:----------:|:----------------------------------:|:------------------:|:-----------------------------:|:-----------:|:----------:|:-------:|
|      2     | Ana Trujillo Emparedados y helados |    Ana Trujillo    | Avda. de la Constitución 2222 | México D.F. |    5021    |  Mexico |
|      3     |       Antonio Moreno Taquería      |   Antonio Moreno   |         Mataderos 2312        | México D.F. |    5023    |  Mexico |
|      4     |           Around the Horn          |    Thomas Hardy    |        120 Hanover Sq.        |    London   |   WA1 1DP  |    UK   |
|      5     |         Berglunds snabbköp         | Christina Berglund |         Berguvsvägen 8        |    Luleå    |  S-958 22  |  Sweden |


## Tüm Kayıtları Silme
- Bir tablodaki tüm satırları, tabloyu silmeden silmek mümkündür. Bu, tablo yapısının, niteliklerinin ve dizinlerinin bozulmadan kalacağı anlamına gelir.

`DELETE FROM table_name;`

- Aşağıdaki SQL ifadesi, "Custormers" tablosundaki tüm satırları, tabloyu silmeden siler:

```mysql
DELETE FROM Customers;
```



---


# DROP TABLE

- Tabloyu tamamen silmek için DROP TABLE ifadesi kullanılır.
- Aşağıdaki SQL ifadesi, Müşteriler tablosunu siler:

```mysql
DROP TABLE Customers;
```


---

# ALIŞTIRMA #1

- Ülkenin Mexico olduğu satırlarda müşteri ismini 'Around the Horn' olarak güncelleyin.

# ALIŞTIRMA #2

- CustomerID'nin 1 olduğu satırlarda müşteri ismini 'Satyam' ve ülkeyi 'USA' olarak güncelleyin.

# ALIŞTIRMA #3

- Aşağıda gösterildiği gibi Çalışanın id, ad, e-posta ve departman olmak üzere kişisel ayrıntılarını içeren gfg_employees adında sql  komutlarını kullanarak bir tablo oluşturalım.

![image](https://github.com/mof-selvi/veritabani-yonetim-sistemleri/assets/58203457/e027dd26-0960-4682-97c2-75e860cbdd4f)

```mysql
CREATE TABLE gfg_employee ( 
id INT PRIMARY KEY, 
name VARCHAR (20) , 
email VARCHAR (25), 
department VARCHAR(20)
); 
INSERT INTO gfg_employee (id, name, email, department) VALUES 
(1, 'Jessie', 'jessie23@gmail.com', 'Development'), 
(2, 'Praveen', 'praveen_dagger@yahoo.com', 'HR'),
(3, 'Bisa', 'dragonBall@gmail.com', 'Sales'), 
(4, 'Rithvik', 'msvv@hotmail.com', 'IT'),
(5, 'Suraj', 'srjsunny@gmail.com', 'Quality Assurance'), 
(6, 'Om', 'OmShukla@yahoo.com', 'IT'), 
(7, 'Naruto', 'uzumaki@konoha.com', 'Development');
```

- Rithvik isimli kayıtları siliniz.

- Departmanın "Development" olduğu satırları siliniz.

- Tablodaki tüm girişleri siliniz.

