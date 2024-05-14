# LAB-07

<details>
    <summary>Temel Bilgiler</summary>

## UNION Operatörü
- SQL UNION operatörü, iki veya daha fazla SELECT ifadesinin sonuç kümelerini birleştirmek için kullanılır.
- UNION içerisindeki her SELECT ifadesinin aynı sayıda sütuna sahip olması gerekir.
- Sütunların benzer veri tiplerine sahip olması gerekir. Her SELECT ifadesindeki sütunlar aynı sırada olmalıdır.

Syntax:
```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

## UNION ALL Operatörü
- `UNION` operatörü varsayılan olarak sadece benzersiz (tekrarsız) değerleri seçer. Tekrar eden değerlerin de seçilmesine izin vermek için `UNION ALL` kullanılır.


## Toplama Fonksiyonları

- Bir toplama fonksiyonu, bir değer kümesi üzerinde bir hesaplama yapar ve tek bir değer döndürür.

- Örnek fonksiyonlar:
    - MIN() : seçilen sütundaki en küçük değeri döndürür
    - MAX() : seçilen sütundaki en büyük değeri döndürür
    - COUNT() : seçim sonucu dönen listedeki satırların sayısını döndürür
    - SUM() : seçilen sütundaki değerlerin toplamını döndürür
    - AVG() : seçilen sütundaki değerlerin ortalamasını döndürür

- Kullanım ve dönen sonuç örnekleri:

---
---

```sql
SELECT SUM(Quantity) FROM OrderDetails;
```

| SUM(Quantity) |
| ------------  |
|      51317    |

---
---

```sql
SELECT COUNT(*) FROM OrderDetails;
```

|   COUNT(*)    |
| ------------  |
|      2155     |

---
---

```sql
SELECT MAX(Quantity) AS TheBiggest FROM OrderDetails;
```

|   TheBiggest  |
| ------------  |
|      130      |

---
---

</details>

<details>
    <summary>Etkinlik</summary>

## Alıştırma #1

> Dikkat! Bu alıştırmalarda yazacağınız tüm SQL kodları saklamanız ve yükleme alanından göndermeniz gerekecektir. Kodlarınızı not ettiğinizden emin olmak ve kaybetmemek sizin sorumluluğunuzdadır.

- Yeni bir veritabanı oluşturunuz.
- Aşağıdaki tabloların yapısını oluşturan SQL kodu yazınız.
- Aşağıdaki tablolardaki verileri, oluşturduğunuz MySQL tablolarına yerleştiriniz.

#### Tablo 1: `Calisanlar`

| ID | Name     | City       | Salary |
|----|----------|------------|--------|
| 1  | John     | New York   | 50000  |
| 2  | Sarah    | Los Angeles| 55000  |
| 3  | Michael  | Chicago    | 60000  |
| 4  | Emily    | Houston    | 48000  |
| 5  | David    | Atlanta    | 62000  |

#### Tablo 2: `Musteriler`

| ID | Name     | City       | Budget |
|----|----------|------------|--------|
| 1  | Alice    | Houston    | 10000  |
| 2  | Bob      | Dallas     | 15000  |
| 3  | Charlie  | Boston     | 12000  |
| 4  | Diana    | Seattle    | 9000   |
| 5  | Emily    | San Francisco | 11000 |

> Bu iki tablodan biri çalışanları diğeri ise müşterileri temsil etmektedir. Her iki tabloda da `ID` dışında `Name` (isim), `City` (şehir) sütunları ve bir sayısal değer sütunu (`Salary` veya `Budget`) bulunmaktadır.


- Her iki tablodaki tüm şehirleri tekrarsız olarak listeleyen SQL kodunu yazınız.
- Her iki tablodaki tüm kişi isimlerini tekrarlı verileri de dahil ederek listeleyen SQL kodunu yazınız.
- En yüksek bütçeyi getiren SQL sorgusunu yazınız.
- En düşük maaşı getiren SQL sorgusunu yazınız.
- Ortalama maaşı getiren SQL sorgusunu yazınız.


</details>
