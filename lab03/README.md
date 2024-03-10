# LAB-03

- # MySQL'de Tabloya Veri Ekleme

---

- ## 1) Basit Bir Tabloya Veri Ekleme
- Aşağıdaki kod ile bir tablo oluşturunuz.
```
CREATE TABLE ogrenciler (
    ogrenci_id INT,
    ad VARCHAR(50),
    soyad VARCHAR(50),
    bolum VARCHAR(50),
    not_ortalamasi FLOAT
);
```

- SQL'de veri ekleme komutu olan INSERT INTO için syntax aşağıdaki gibidir. Değişken kısımlar {...} şeklinde küme parantezleri ile belirtilmiştir.
```
INSERT INTO {TABLO ADI} ({1. SÜTUN İSMİ}, {2. SÜTUN İSMİ}, ...) VALUES ({1. SÜTUN VERİSİ}, {2. SÜTUN VERİSİ}, ...);
```

- Yukarıda oluşturduğunuz tabloya aşağıdaki kod ile bir satırlık veri ekleyiniz.
```
INSERT INTO ogrenciler (ogrenci_id, ad, soyad, bolum, not_ortalamasi) VALUES (1, 'Ahmet', 'Yilmaz', 'Bilgisayar Muhendisligi', 85.5);
```

- İki SQL komutu aralarında ; olacak şekilde art arda yazılarak da birlikte çalıştırılabilir. (Bu özellik bazı web programlama sistemleri tarafından direkt kabul edilmeyebilir. Bkz. SQL Injection)
```
-- İkinci satır
INSERT INTO ogrenciler (ogrenci_id, ad, soyad, bolum, not_ortalamasi)
VALUES (2, 'Mehmet', 'Kaya', 'Elektrik Elektronik Muhendisligi', 78.2);

-- Üçüncü satır
INSERT INTO ogrenciler (ogrenci_id, ad, soyad, bolum, not_ortalamasi)
VALUES (3, 'Ayse', 'Demir', 'Makine Muhendisligi', 92.0);
```
> SQL'de -- karakterleri ile başlayan satır yorum satırı olur.


---



- ## 2) PRIMARY KEY İçeren Bir Tabloya Veri Ekleme
- Aşağıdaki kod ile yeni bir tablo oluşturunuz.
```
CREATE TABLE ogrenciler2 (
    ogrenci_id INT PRIMARY KEY,
    ad VARCHAR(50),
    soyad VARCHAR(50),
    bolum VARCHAR(50),
    not_ortalamasi FLOAT
);
```

- Aşağıdaki gibi bir kod ile 3 satırlık veriyi aynı anda bir tabloya ekleyebilirsiniz.
```
INSERT INTO ogrenciler2 (ogrenci_id, ad, soyad, bolum, not_ortalamasi)
VALUES (1, 'Ahmet', 'Yilmaz', 'Bilgisayar Muhendisligi', 85.5),
       (2, 'Mehmet', 'Kaya', 'Elektrik Elektronik Muhendisligi', 78.2),
       (3, 'Ayse', 'Demir', 'Makine Muhendisligi', 92.0);
```


---


- ## Alıştırma #1
- "ogrenciler2" tablosuna daha önce eklenmiş satırlardan bir tanesi ile aynı "ogrenci_id" değerine sahip yeni bir satır eklemeye çalışın.


---


- ## 3) PRIMARY KEY & AUTO_INCREMENT İçeren Bir Tabloya Veri Ekleme
- Aşağıdaki kod ile yeni bir tablo daha oluşturunuz.
```
CREATE TABLE ogrenciler3 (
    ogrenci_id INT AUTO_INCREMENT PRIMARY KEY,
    ad VARCHAR(50),
    soyad VARCHAR(50),
    bolum VARCHAR(50),
    not_ortalamasi FLOAT
);
```

- Tablonun ogrenci_id satırına dikkat ediniz.
- Aşağıdaki kod ile 3 satır elemesi yapınız.
```
INSERT INTO ogrenciler3 (ad, soyad, bolum, not_ortalamasi)
VALUES ('Ahmet', 'Yilmaz', 'Bilgisayar Muhendisligi', 85.5),
       ('Mehmet', 'Kaya', 'Elektrik Elektronik Muhendisligi', 78.2),
       ('Ayse', 'Demir', 'Makine Muhendisligi', 92.0);
```
- Bu sefer "ogrenci_id" değerleri vermediğimize dikkat ediniz.
- Yönetim panelinden eklenen satırlar için "ogrenci_id" değerleri olup olmadığını kontrol ediniz.


> Not: ID'leri 100'den başlatmak için tablonun AUTO_INCREMENT değerini aşağıdaki gibi değiştirebiliriz.
```
CREATE TABLE ogrenciler3 (
    ogrenci_id INT AUTO_INCREMENT PRIMARY KEY,
    ad VARCHAR(50),
    soyad VARCHAR(50),
    bolum VARCHAR(50),
    not_ortalamasi FLOAT
) AUTO_INCREMENT = 100;
```

---


- ## Alıştırma #2
- Kendi PRIMARY KEY (birincil anahtar) ve AUTO_INCREMENT (otomatik artırma) içeren tablonuzu ID'ler 1000'den başlayacak şekilde oluşturunuz.
- PRIMARY KEY olmayan, AUTO_INCREMENT özelliği olan bir sütuna sahip yeni bir tablo oluşturmaya çalışınız.
- Nümerik veri tipinde olmayan bir sütunu PRIMARY KEY ve AUTO_INCREMENT kullanarak yeni bir tabloda oluşturmaya çalışınız.


---

- # Not
- MySQL varsayılan olarak nümerik veri tipindeki sütunları işaretli (signed) olarak oluşturur. Bu, o sütuna negatif değerler de verebileceğimiz anlamına gelir.
- Birincil anahtarlı sütunlar genellikle 1'den başlar. Bu yüzden ID değerleri için negatif değerlere ihtiyacımız yoktur. Bu sebeple ID sütunlarında UNSIGNED ifadesini kullanarak veri türünün kapsadığı tüm alanı kullanılabilir yaparız.
- Yüksek sayıda veri eklemesi yapılacağını beklediğimiz tablolar için birincil anahtarlı sütunda genellikle BIGINT UNSIGNED veri tipi seçeriz.

```
...
  ID BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
...
```
