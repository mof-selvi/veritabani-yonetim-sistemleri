# LAB-05

# LIMIT
- Bir SQL sorgusunun, `[belirlenmiş bir başlangıç noktasından itibaren]` belirli bir sayıdaki kayıtlar için gerçekleşmesini sağlar.

```mysql
SELECT 
    select_list
FROM
    table_name
WHERE
    condition
LIMIT {[offset,] row_count | row_count OFFSET offset};
```

> Not: daha ayrıntılı syntax yapısı için MySQL'in [Select Statements](https://dev.mysql.com/doc/refman/8.0/en/select.html) sayfasına bakınız.

- `offset` sayısı, sorgunun sol tarafından (SELECT'ten LIMIT'e kadar olan kısımdan) dönecek olan satırların baştan kaç tanesinin atlanacağını belirtir.
- Dönecek kayıtları ilk satırdan itibaren almak istiyorsak `offset` değeri 0 olmalıdır veya bu değer hiç belirtilmeyebilir. Belirtilmezse, bu değer varsayılan olarak 0'dır. OFFSET ifadesinden sonra da belirtilebilir.
- `row_count` sayısı, `offset` kadar satır atlandıktan sonra kaç adet satırın döndürüleceğini belirtir.


![image](https://github.com/mof-selvi/veritabani-yonetim-sistemleri/assets/58203457/783681dc-15e2-41ed-ad9c-715c55e56bd9)


## Tartışma #1
- LIMIT kullanımı hangi durumlarda işimize yarayabilir?
- LIMIT yerine WHERE kullanarak sınırlama yapamaz mıyız? Örneğin `WHERE ID>=11 AND ID<=20` ifadesi yeterli olmaz mıydı?
- LIMIT'in işlevini tüm verileri aldıktan sonra kullandığımız programda da yerine getirebiliriz. Bunu SQL sunucusu tarafında yapmanın avantajları neler olabilir?


---


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

# Alıştırma #1

- Customers tablosunu yukarıdaki kod ile tekrar oluşturunuz.
- Ülkenin Mexico olduğu satırlarda müşteri ismini 'Around the Horn' olarak güncelleyin.

# Alıştırma #2

- Customers tablosu üzerinden devam ediniz.
- CustomerID'nin 1 olduğu satırlarda müşteri ismini 'Satyam' ve ülkeyi 'USA' olarak güncelleyin.

# Alıştırma #3

- Bu alıştırma içinse yeni bir tablo oluşturacağız.
- Aşağıda gösterildiği gibi çalışanın id, ad, e-posta ve departman bilgilerini içeren gfg_employees adındaki bir tabloyu sql komutları ile oluşturalım.

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

## Alıştırma #3.1
- Rithvik isimli kayıtları siliniz.

## Alıştırma #3.2
- Departmanın "Development" olduğu satırları siliniz.

## Alıştırma #3.3
- Tablodaki tüm girişleri siliniz.


