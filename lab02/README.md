- # MySQL'de Tablo Oluşturma

Tablolar CREATE TABLE komutu ile oluşturulur. Örnek bir tablo oluşturma kodu:
```
CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
);
```
> Tablo ve sütun isimleri için snake case, pascal case veya camel case kullanılabilir.
> Yukarıdaki tabloda "Persons" tablo ismi; PersonID, LastName, FirstName, Address ve City ise bu tabloda bulunacak sütunların isimleridir.

- # MySQL'de Veri Türleri
SQL'de genel olarak 3 veri türü kategorisi bulunmaktadır: metin (string), sayısal (numeric) ve tarih-zaman (date-time).

- ## String Veri Türleri

| Veri Tipi Türleri           | Açıklama |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CHAR(size)                  | SABİT uzunluktaki bir dize (harf, rakam ve özel karakterler içerebilir). "size" parametresi sütun uzunluğunu karakter cinsinden belirtir - 0 ila 255 arasında olabilir. Varsayılan değer 1'dir.                                                          |
| VARCHAR(size)               | DEĞİŞKEN uzunluktaki bir dize (harf, rakam ve özel karakterler içerebilir). "size" parametresi maksimum sütun uzunluğunu karakter cinsinden belirtir - 0 ila 65535 arasında olabilir.                                                                    |
| BINARY(size)                | CHAR() ile aynıdır, ancak ikili bayt dizilerini depolar. "size" parametresi sütun uzunluğunu bayt cinsinden belirtir. Varsayılan değer 1'dir.                                                                                                            |
| VARBINARY(size)             | VARCHAR() ile aynıdır, ancak ikili bayt dizilerini depolar. "size" parametresi maksimum sütun uzunluğunu bayt cinsinden belirtir.                                                                                                                        |
| TINYBLOB                    | BLOB'lar için (Binary Large OBjects). Maksimum uzunluk: 255 bayt                                                                                                                                                                                         |
| TINYTEXT                    | En fazla 255 karakter uzunluğunda bir dizeyi tutar.                                                                                                                                                                                                      |
| TEXT(size)                  | En fazla 65,535 bayt uzunluğunda bir dizeyi tutar.                                                                                                                                                                                                       |
| BLOB(size)                  | BLOB'lar için (Binary Large OBjects). En fazla 65,535 bayt veri tutar.                                                                                                                                                                                   |
| MEDIUMTEXT                  | En fazla 16,777,215 karakter uzunluğunda bir dizeyi tutar.                                                                                                                                                                                               |
| MEDIUMBLOB                  | BLOB'lar için (Binary Large OBjects). En fazla 16,777,215 bayt veri tutar.                                                                                                                                                                               |
| LONGTEXT                    | En fazla 4,294,967,295 karakter uzunluğunda bir dizeyi tutar.                                                                                                                                                                                            |
| LONGBLOB                    | BLOB'lar için (Binary Large OBjects). En fazla 4,294,967,295 bayt veri tutar.                                                                                                                                                                            |
| ENUM(val1, val2, val3, ...) | Yalnızca bir değeri olabilen bir dize nesnesi, olası değerler listesinden seçilen. Bir ENUM listesine en fazla 65535 değer ekleyebilirsiniz. Liste içinde olmayan bir değer eklenirse, boş bir değer eklenir. Değerler girdiğiniz sıraya göre sıralanır. |
| SET(val1, val2, val3, ...)  | 0 veya daha fazla değeri olabilen bir dize nesnesi, olası değerler listesinden seçilen. Bir SET listesine en fazla 64 değer ekleyebilirsiniz.                                                                                                            |

- ## Numeric Veri Türleri

| Veri Tipi Türleri           | Açıklama |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BIT(size)                 | Bir bit değer tipi. Her bir değer için bit sayısı "size" ile belirtilir. "size" parametresi 1 ila 64 arasında bir değeri tutabilir. "size" için varsayılan değer 1'dir.                                                                                                                    |
| TINYINT(size)             | Çok küçük bir tamsayı. İmzalı aralık -128 ile 127 arasındadır. İmzasız aralık ise 0 ile 255 arasındadır. "size" parametresi maksimum görüntü genişliğini belirtir (ki bu 255'tir).                                                                                                         |
| BOOL                      | Sıfır, yanlış olarak kabul edilir; sıfır olmayan değerler ise doğru olarak kabul edilir.                                                                                                                                                                                                   |
| BOOLEAN                   | BOOL ile eşdeğerdir.                                                                                                                                                                                                                                                                       |
| SMALLINT(size)            | Küçük bir tamsayı. İmzalı aralık -32768 ile 32767 arasındadır. İmzasız aralık ise 0 ile 65535 arasındadır. "size" parametresi maksimum görüntü genişliğini belirtir (ki bu 255'tir).                                                                                                       |
| MEDIUMINT(size)           | Orta büyüklükte bir tamsayı. İmzalı aralık -8388608 ile 8388607 arasındadır. İmzasız aralık ise 0 ile 16777215 arasındadır. "size" parametresi maksimum görüntü genişliğini belirtir (ki bu 255'tir).                                                                                      |
| INT(size)                 | Orta büyüklükte bir tamsayı. İmzalı aralık -2147483648 ile 2147483647 arasındadır. İmzasız aralık ise 0 ile 4294967295 arasındadır. "size" parametresi maksimum görüntü genişliğini belirtir (ki bu 255'tir).                                                                              |
| INTEGER(size)             | INT(size) ile eşdeğerdir.                                                                                                                                                                                                                                                                  |
| BIGINT(size)              | Büyük bir tamsayı. İmzalı aralık -9223372036854775808 ile 9223372036854775807 arasındadır. İmzasız aralık ise 0 ile 18446744073709551615 arasındadır. "size" parametresi maksimum görüntü genişliğini belirtir (ki bu 255'tir).                                                            |
| FLOAT(size, d)            | Bir kayan nokta sayısı. Toplam basamak sayısı "size" ile belirtilir. Ondalık noktasından sonraki basamak sayısı "d" parametresi ile belirtilir. Bu sözdizimi MySQL 8.0.17'de kullanımdan kaldırılmış olup, gelecekteki MySQL sürümlerinde kaldırılacaktır.                                 |
| FLOAT(p)                  | Bir kayan nokta sayısı. MySQL, sonuç veri türü için FLOAT veya DOUBLE'ı kullanıp kullanmamayı belirlemek için "p" değerini kullanır. "p" 0 ile 24 arasındaysa, veri tipi FLOAT() olur. "p" 25 ile 53 arasındaysa, veri tipi DOUBLE() olur.                                                 |
| DOUBLE(size, d)           | Normal boyutlu bir kayan nokta sayısı. Toplam basamak sayısı "size" ile belirtilir. Ondalık noktasından sonraki basamak sayısı "d" parametresi ile belirtilir.                                                                                                                             |
| DOUBLE PRECISION(size, d) |                                                                                                                                                                                                                                                                                            |
| DECIMAL(size, d)          | Tam bir sabit ondalık sayı. Toplam basamak sayısı "size" ile belirtilir. Ondalık noktasından sonraki basamak sayısı "d" parametresi ile belirtilir. "size" için maksimum sayı 65'tir. "d" için maksimum sayı 30'dur. "size" için varsayılan değer 10'dur. "d" için varsayılan değer 0'dır. |
| DEC(size, d)              | DECIMAL(size,d) ile eşdeğerdir.                                                                                                                                                                                                                                                            |

- ## Tarih-Zaman Veri Türleri

| Veri Tipi Türleri           | Açıklama |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DATE           | Bir tarih. Biçim: YYYY-AA-GG. Desteklenen aralık, '1000-01-01' ile '9999-12-31' arasındadır.                                                                                                                                                                                                                                                                                                          |
| DATETIME(fsp)  | Tarih ve saat kombinasyonu. Biçim: YYYY-AA-GG ss:dd:ss. Desteklenen aralık, '1000-01-01 00:00:00' ile '9999-12-31 23:59:59' arasındadır. Otomatik başlatma ve güncellemeleri elde etmek için sütun tanımına DEFAULT ve ON UPDATE eklemek, mevcut tarih ve saat değerine otomatik başlatma ve güncelleme sağlar.                                                                                       |
| TIMESTAMP(fsp) | Bir zaman damgası. TIMESTAMP değerleri, Unix epokasından ('1970-01-01 00:00:00' UTC) itibaren geçen saniye sayısı olarak depolanır. Biçim: YYYY-AA-GG ss:dd:ss. Desteklenen aralık, '1970-01-01 00:00:01' UTC ile '2038-01-09 03:14:07' UTC arasındadır. Otomatik başlatma ve güncellemeleri elde etmek için sütun tanımında DEFAULT CURRENT_TIMESTAMP ve ON UPDATE CURRENT_TIMESTAMP kullanılabilir. |
| TIME(fsp)      | Bir zaman. Biçim: ss:dd:ss. Desteklenen aralık, '-838:59:59' ile '838:59:59' arasındadır.                                                                                                                                                                                                                                                                                                             |
| YEAR           | Dört haneli bir yıl. Dört haneli biçimde izin verilen değerler: 1901 ila 2155 ve 0000. MySQL 8.0, iki haneli biçimde yılı desteklemez.                                                                                                                                                                                                                                                                |



[Kaynak 1](https://www.w3schools.com/mysql/mysql_datatypes.asp)
[Kaynak 2](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)

- ## Alıştırma #1
