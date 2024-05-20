# LAB-08

## JOIN - Tablo Birleştirme

- Kullanıcılar ve siparişler tablolarını oluşturunuz:
```sql
CREATE TABLE users (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100)
);

CREATE TABLE orders (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    user_id INT UNSIGNED,
    product_name VARCHAR(100),
    quantity INT,
    total_price DECIMAL(10, 2),
    FOREIGN KEY (user_id) REFERENCES users(id)
);

INSERT INTO users (username, email) VALUES
('alice', 'alice@example.com'),
('bob', 'bob@example.com'),
('charlie', 'charlie@example.com'),
('david', 'david@example.com');

INSERT INTO orders (user_id, product_name, quantity, total_price) VALUES
(1, 'Laptop', 2, 2000.00),
(1, 'Mouse', 1, 20.00),
(2, 'Keyboard', 1, 50.00),
(3, 'Monitor', 1, 300.00),
(3, 'Headphones', 1, 100.00);

```

## Kullanıcılar ve Siparişleri

- Aşağıdaki sorgu kullanıcıları ve siparişleri aynı anda çeker:
```sql
SELECT *
FROM users
JOIN orders ON users.id = orders.user_id;
```

> Hiçbir siparişi olmayan `david` adlı kullanıcının listelenmediğine dikkat ediniz.

- MySQL'de `JOIN` ve `INNER JOIN` birbirlerinin yerine kullanılabilir. Aşağıdaki sorgu aynı şeyi yapar:
```sql
SELECT *
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

> Hiçbir siparişi olmayan `david` adlı kullanıcının listelenmediğine dikkat ediniz.

## Left/Right Join
- Aşağıdaki sorguları sırasıyla deneyerek sonuçları gözlemleyiniz:
```sql
SELECT *
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

```sql
SELECT *
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

## Sütun Seçimleri
- Belirli sütunları seçmek için nokta notasyonu `tablo_adı.sütun_adı` şeklinde kullanılabilir.
```sql
SELECT users.username, orders.product_name
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

## Takma Ad
- Bir tabloya ait birden fazla sütun seçimi yapılması gerekiyorsa tablo adını tekrar yazmak yerine kısaltmak kullanışlı olabilir.
```sql
SELECT U.username, U.email, O.product_name
FROM users U
RIGHT JOIN orders O ON U.id = O.user_id;
```

> Dikkat: bir tablo için takma ad belirlendikten sonra tekrar tablo adı kullanılmamalıdır.

> Takma ad tek harften oluşmak zorunda değildir. Tek veya iki harfden oluşan takma adlar sıklıkla kullanılmaktadır.

- Aşağıdaki sorgu da bir önceki ile aynı işlemi yapmaktadır. MySQL'de takma ad kullanımında `AS` kullanımı opsiyoneldir.
```sql
SELECT U.username, U.email, O.product_name
FROM users AS U
RIGHT JOIN orders AS O ON U.id = O.user_id;
```

> Not: `JOIN` kullanabilmek için `foreign key`, `primary key` veya `index` tanımlamaları yapmak şart değildir. Fakat bu anahtarları tanımlamak performansı önemli ölçüde artırır ve veri bütünlüğünü sağlar. Çok sayıda veri ekleme ve silme durumlarının ardından kontrolümüz dışında bir yerlerde veri kalmış olmasını istemeyiz.

## M:N İlişkili Tablolarda Birleştirme

- Öğrencileri, dersleri ve öğrencilerin aldığı dersleri veritabanımıza yerleştirelim:
```sql

CREATE TABLE students (
    student_id INT UNSIGNED PRIMARY KEY,
    student_name VARCHAR(50)
);
INSERT INTO students (student_id, student_name) VALUES
    (1, 'Ahmet Yılmaz'),
    (2, 'Ayşe Demir'),
    (3, 'Mehmet Şahin'),
    (4, 'Zeynep Kaya'),
    (5, 'Ali Veli');


CREATE TABLE courses (
    course_id INT UNSIGNED PRIMARY KEY,
    course_name VARCHAR(50)
);
INSERT INTO courses (course_id, course_name) VALUES
    (1, 'Matematik'),
    (2, 'Fizik'),
    (3, 'Kimya'),
    (4, 'VTYS'),
    (5, 'Biyoloji');


CREATE TABLE student_courses (
    student_id INT UNSIGNED,
    course_id INT UNSIGNED,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);
INSERT INTO student_courses (student_id, course_id) VALUES
    (1, 1),
    (1, 4),
    (2, 2),
    (3, 3),
    (3, 4);

```

- Ara tabloların bulunduğu durumda birden fazla `JOIN` ifadesi kullanmamız gerekecektir.

- Aşağıdaki sorgu, öğrencisi olan dersleri ve bu derslerin öğrencilerini listeleyecektir. `JOIN` kullanıldığı için öğrencisi olmayan dersler veya ders almayan öğrenciler listede bulunmayacaktır.

```sql
SELECT * 
FROM courses AS C 
JOIN student_courses AS SC ON SC.course_id=C.course_id
JOIN students AS S ON S.student_id=SC.student_id;
```

- Aşağıdaki sorgu ise aynı verileri çeker fakat sütunlar sıralanmaya öğrenciler tablosundan başlar.

```sql
SELECT * 
FROM students AS S
JOIN student_courses AS SC ON S.student_id=SC.student_id
JOIN courses AS C ON SC.course_id=C.course_id;
```

## Alıştırmalar
- Tüm öğrencileri (ders almayanlar da dahil), aldıkları dersler ile eşleşmiş şekilde, sütun tekrarları olmadan listeleyiniz.

    **Beklenen çıktı**

    | student_id | student_name | course_id | course_name |
    |------------|--------------|-----------|-------------|
    |          1 | Ahmet Yılmaz |         1 | Matematik   |
    |          1 | Ahmet Yılmaz |         4 | VTYS        |
    |          2 | Ayşe Demir   |         2 | Fizik       |
    |          3 | Mehmet Şahin |         3 | Kimya       |
    |          3 | Mehmet Şahin |         4 | VTYS        |
    |          4 | Zeynep Kaya  |      NULL | NULL        |
    |          5 | Ali Veli     |      NULL | NULL        |


- Tüm dersleri (alınmamış olanlar da dahil), tüm öğrenciler (ders almayanlar da dahil) ile eşleşmiş şekilde, sütun tekrarları olmadan listeleyiniz.

    **Beklenen çıktı**

    | student_id | student_name | course_id | course_name |
    |------------|--------------|-----------|-------------|
    |          1 | Ahmet Yılmaz |         1 | Matematik   |
    |          1 | Ahmet Yılmaz |         4 | VTYS        |
    |          2 | Ayşe Demir   |         2 | Fizik       |
    |          3 | Mehmet Şahin |         3 | Kimya       |
    |          3 | Mehmet Şahin |         4 | VTYS        |
    |          4 | Zeynep Kaya  |      NULL | NULL        |
    |          5 | Ali Veli     |      NULL | NULL        |
    |       NULL | NULL         |         5 | Biyoloji    |

> İpucu: önceki konulardan hatırlamanız gereken yerler bulunmaktadır.

> Tüm SQL kodlarınızı Ekampüs'te açılmış olan alana süresi içerisinde kopyalayıp yapıştırınız.
