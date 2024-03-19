# LAB-04

# Örnek #1

## Customers tablosu:

| CustomerID |            CustomerName            |     ContactName    |            Address            |     City    | PostalCode | Country |
|:----------:|:----------------------------------:|:------------------:|:-----------------------------:|:-----------:|:----------:|:-------:|
|      1     |         Alfreds Futterkiste        |    Maria Anders    |         Obere Str. 57         |    Berlin   |    12209   | Germany |
|      2     | Ana Trujillo Emparedados y helados |    Ana Trujillo    | Avda. de la Constitución 2222 | México D.F. |    5021    |  Mexico |
|      3     |       Antonio Moreno Taquería      |   Antonio Moreno   |         Mataderos 2312        | México D.F. |    5023    |  Mexico |
|      4     |           Around the Horn          |    Thomas Hardy    |        120 Hanover Sq.        |    London   |   WA1 1DP  |    UK   |
|      5     |         Berglunds snabbköp         | Christina Berglund |         Berguvsvägen 8        |    Luleå    |  S-958 22  |  Sweden |


## Bu tabloyu sql komutları ile oluşturalım.

```mysql
CREATE TABLE Customers ( 
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

## Örnek tablo üzerinden işlemlerimizi gerçekleştirelim.

---

# SELECT
- SELECT ifadesi veritabanından veri seçmek için kullanılır.

`SELECT column1, column2, ... FROM table_name;`

> Burada column1, column2, ... veri seçmek istediğiniz tablonun alan adlarıdır. table_name, içinden veri seçmek istediğiniz tablonun adını temsil eder.

- Tüm sütunları seçmek için

`SELECT * FROM Customers;`

---

# WHERE
- WHERE ifadesi kayıtları filtrelemek için kullanılır. Yalnızca belirli bir koşulu karşılayan kayıtları çıkarmak için kullanılır.

`SELECT column1, column2, ... FROM table_name WHERE condition;`

> WHERE ifadesi yalnızca SELECT ile kullanılmaz, aynı zamanda UPDATE, DELETE vb. ifadelerinde de kullanılır! Bunlara daha sonraki derslerde değineceğiz.

- Örnek tablo için:

```
SELECT * FROM Customers WHERE CustomerID=1;
```

- sorgusu "Customers" tablosundan müşterileri seçerken, "CustomerID" alanının değeri 1 olanları getirir. Burada "CustomerID" alanı bir sayısal alan olduğu için tırnak işareti içine alınmamıştır. Çünkü SQL'de sayısal değerler tırnak işareti içine alınmaz. Ancak metinsel değerler (örneğin, müşteri isimleri gibi) tırnak işareti içinde belirtilir.

```
SELECT * FROM Customers
WHERE CustomerID > 80;
```

- Yukarıdaki sorgu "Customers" tablosundan müşterileri seçerken, "CustomerID" alanının değeri 80'den büyük olanları getirir.


## WHERE ile aşağıdaki operatörler kullanılabilir:

| Operator |                                                                Description                                                               |
|:--------:|:----------------------------------------------------------------------------------------------------------------------------------------:|
|     =    |                                    Eşittir. Belirli bir değere sahip olanları seçmek için kullanılır.                                    |
|     >    |                                Büyüktür. Belirli bir değerden daha büyük olanları seçmek için kullanılır.                                |
|     <    |                                Küçüktür. Belirli bir değerden daha küçük olanları seçmek için kullanılır.                                |
|    >=    |                           Büyük eşittir. Belirli bir değerden büyük veya eşit olanları seçmek için kullanılır.                           |
|    <=    |                           Küçük eşittir. Belirli bir değerden küçük veya eşit olanları seçmek için kullanılır.                           |
|    <>    | Eşit değil. Belirli bir değere sahip olmayanları seçmek için kullanılır. Not: Bazı SQL sürümlerinde bu operatör "!=" olarak yazılabilir. |
|  [BETWEEN](https://www.w3schools.com/mysql/mysql_between.asp) |                                           Belirli bir aralıkta olan nümerik veriye sahip satırları seçmek için kullanılır.                                          |
|   [LIKE](https://www.w3schools.com/mysql/mysql_like.asp)   |                                                Belirli bir alfanümerik deseni aramak için kullanılır.                                                |
|    [IN](https://www.w3schools.com/mysql/mysql_in.asp)    |                                    Bir sütun için birden fazla olası değeri belirtmek için kullanılır.                                   |


---


# GROUP BY
- GROUP BY ifadesi, aynı değerlere sahip satırları özet satırlara gruplar.
- Örneğin, bir müşteri tablosundaki müşterileri ülke sütununa göre gruplamak ve her ülkedeki müşteri sayısını bulmak için GROUP BY ifadesi kullanılabilir. Bu sayede, her ülkedeki müşteri sayısı gibi toplu veriler elde edilebilir.
- Sözdizimi (syntax) aşağıdaki gibidir:

`
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
`

- Örnek tablomuz için:

```
SELECT COUNT(CustomerID),Country
FROM Customers
GROUP BY Country;
```

- sorgusu her ülkedeki müşteri sayısını listeler.


```
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;
```

- Bu ifade, her ülkedeki müşteri sayısını yüksekten düşüğe doğru sıralayarak listeler.


---


# Örnek 2

## Products tablosu:

| ProductID |          ProductName         | SupplierID | CategoryID |         Unit        | Price |
|:---------:|:----------------------------:|:----------:|:----------:|:-------------------:|:-----:|
|     1     |             Chais            |      1     |      1     |  10 boxes x 20 bags |   18  |
|     2     |             Chang            |      1     |      1     |  24 - 12 oz bottles |   19  |
|     3     |         Aniseed Syrup        |      1     |      2     | 12 - 550 ml bottles |   10  |
|     4     | Chef Anton's Cajun Seasoning |      2     |      2     |    48 - 6 oz jars   |   22  |
|     5     |    Chef Anton's Gumbo Mix    |      2     |      2     |       36 boxes      | 21.35 |


- Bu tabloyu sql komutları ile oluşturalım:

```mysql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(255),
    SupplierID INT,
    CategoryID INT,
    Unit VARCHAR(255),
    Price DECIMAL(10, 2)
);
INSERT INTO Products (ProductID, ProductName, SupplierID, CategoryID, Unit, Price) VALUES
(1, 'Chais', 1, 1, '10 boxes x 20 bags', 18),
(2, 'Chang', 1, 1, '24 - 12 oz bottles', 19),
(3, 'Aniseed Syrup', 1, 2, '12 - 550 ml bottles', 10),
(4, 'Chef Anton''s Cajun Seasoning', 2, 2, '48 - 6 oz jars', 22),
(5, 'Chef Anton''s Gumbo Mix', 2, 2, '36 boxes', 21.35);
```

> Örnek tablo üzerinden işlemlerimizi gerçekleştirelim.


---


# ORDER BY

- ORDER BY ifadesi, sonuç kümesini artan veya azalan sırada sıralamak için kullanılır.

`
SELECT column1, column2, ... 
FROM table_name 
ORDER BY column1, column2, ... ASC|DESC;
`

- Örneğin ürünleri fiyata göre sıralamak için:

```mysql
SELECT * FROM Products
ORDER BY Price;
```

- komutu kullanılır. Aşağıdaki MySQL kodu aynı işlevi yerine getirir.

```mysql
SELECT * FROM Products
ORDER BY Price ASC;
```


## DESC (Azalan Sıralama):
- ORDER BY anahtar sözcüğü, kayıtları varsayılan olarak artan sırada sıralar. Kayıtları azalan düzende sıralamak için DESC anahtar sözcüğü kullanılır.
- Ürünleri en yüksek fiyattan en düşük fiyata doğru sıralamak için:

```mysql
SELECT * FROM Products
ORDER BY Price DESC;
```

- Aşağıdaki kod da aynı işlevi yerine getirir. Kodda yapılmış olan değişikliğe dikkat ediniz.

```mysql
SELECT * FROM Products
ORDER BY -Price;
```

## Alfabetik Sırayla:
- String değerleri için ORDER BY anahtar sözcüğü alfabetik olarak sıralanır.
- Ürünleri ProductName’e göre alfabetik olarak sıralamak için:

```mysql
SELECT * FROM Products
ORDER BY ProductName;
```

## Tersten Alfabetik Sırayla:
- Tabloyu alfabetik olarak tersten sıralamak için DESC kullanılır.
- Ürünleri ProductName’e göre ters sırada sıralamak için:

```mysql
SELECT * FROM Products
ORDER BY ProductName DESC;
```

> MySQL'de `ORDER BY column DESC` yerine `ORDER BY -column` kullanımı alfanümerik veriler için de çalışmaktadır.

## Çok Sütunlu ORDER BY:

```mysql
SELECT * FROM Customers
ORDER BY Country, CustomerName;
```
- komutu Örnek 1'deki "Customers" tablosundan tüm müşterileri seçer, ardından "Country" ve "CustomerName" sütunlarına göre sıralar. Bu, verilerin Country'ye göre sıralandığı ancak aynı Country'ye sahip satırlar varsa bu satırların kendi aralarında ise CustomerName'e göre sıralandığı anlamına gelir.
- Yani öncelikle "Ülke" sıralaması yapılır, aynı ülkeye sahip olan müşteriler bir araya getirilir. Ardından, aynı ülkeye sahip müşteriler arasında ise "MüşteriAdı"na göre alfabetik sıralama yapılır. Bu sayede, müşteriler hem ülke hem de müşteri adına göre düzenlenmiş bir sonuç kümesi elde edilir.

## Çok Sütunlu Sıralamada Hem ASC Hem DESC Kullanımı:

```mysql
SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;
```

- komutu Örnek 1'deki "Customers" tablosundaki tüm müşterileri, Country'e göre artan ve CustomerName sütununa göre azalan şekilde sıralayarak seçer.


---


# Alıştırma #1

- `Products` tablosundan fiyatı [18, 20] aralığında olan ürünleri çekiniz.
- `Customers` tablosundan ismi "B" harfi ile başlayan müşterileri çekiniz.

---

# Tartışma #1
- 1 milyon kayıt (record) içeren bir veritabanı tablosu düşününüz.
- İşlemcimiz 1 saniyede 1 milyon adet veri karşılaştırması yapabilmektedir.
- Elimizdeki bir sıralama algoritmasının karmaşıklığı O(n*logn)'dir. Yani n*logn adet karşılaştırma sonucunda diziyi sıralayabilmektedir.
  - *1 milyon verinin 1 sütuna göre sıralı bir şekilde çekilebilmesi için ***yaklaşık*** kaç saniye gerekir?*
  - *Bu işlemi hızlandırabilmek için ne tavsiye edersiniz? Veritabanı motoru nasıl geliştirilebilir?*
