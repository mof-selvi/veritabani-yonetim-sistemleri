# LAB-09

## MySQL Tetikleyiciler (Triggers)

Tetikleyiciler (Triggers), veritabanındaki belirli bir tabloda bir olay gerçekleştiğinde (veri ekleme - `INSERT`, güncelleme - `UPDATE` veya silme - `DELETE`) otomatik olarak çalışan SQL kod bloklarıdır.

### Genel Tetikleyici Sözdizimi (Syntax)

MySQL'de bir tetikleyici oluşturmak için kullanılan genel şablon aşağıdaki gibidir:

```sql
DELIMITER //

CREATE TRIGGER tetikleyici_adi
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON tablo_adi
FOR EACH ROW
BEGIN
    -- Tetiklendiğinde çalışacak SQL komutları buraya yazılır
END //

DELIMITER ;
```

- **DELIMITER:** SQL komut sonlandırıcısını geçici olarak `//` yapar. Tetikleyici gövdesindeki `;` işaretlerinin komutu erken bitirmesini önler.
- **{BEFORE | AFTER}:** Tetikleyicinin olaydan önce mi yoksa sonra mı çalışacağını belirler.
- **{INSERT | UPDATE | DELETE}:** Tetikleyiciyi harekete geçirecek olayı belirler.
- **NEW ve OLD Değişkenleri:**
  - `NEW.sütun_adı`: Eklenen/güncellenen yeni satırdaki verileri temsil eder (`INSERT` ve `UPDATE`).
  - `OLD.sütun_adı`: Silinen/güncellenen eski satırdaki verileri temsil eder (`UPDATE` ve `DELETE`).

---

## 1. Hazırlık ve Tablo Yapısı

Bu laboratuvar uygulamasında bir e-ticaret veritabanı simülasyonu üzerinde çalışacağız. Öncelikle aşağıdaki tabloları oluşturunuz ve örnek verileri ekleyiniz:

```sql
-- Ürünler tablosu
CREATE TABLE products (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    stock_quantity INT NOT NULL,
    price DECIMAL(10, 2) NOT NULL
);

-- Siparişler tablosu
CREATE TABLE orders (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    product_id INT UNSIGNED NOT NULL,
    quantity INT NOT NULL,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);

-- Fiyat geçmişi takip tablosu
CREATE TABLE price_history (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    product_id INT UNSIGNED NOT NULL,
    old_price DECIMAL(10, 2),
    new_price DECIMAL(10, 2),
    change_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);

-- Sistem log (günlük) tablosu
CREATE TABLE audit_log (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    action_type VARCHAR(50),
    log_message TEXT,
    action_date DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Başlangıç verilerini ekleyelim
INSERT INTO products (name, stock_quantity, price) VALUES
('laptop', 50, 15000.00),
('mouse', 150, 250.00),
('keyboard', 80, 450.00);
```

---

## 2. BEFORE INSERT Tetikleyicisi (Veri Doğrulama ve Düzeltme)

`BEFORE INSERT` tetikleyicileri, veri veritabanına kaydedilmeden hemen önce çalışır. Bu sayede girilen verileri değiştirebilir veya düzeltebiliriz.

### Görev:
Yeni bir ürün eklenirken (`INSERT`);
1. Ürün isminin ilk harfini büyük (`UPPER`), kalanını küçük harf (`LOWER`) yapınız.
2. Girilen fiyat (`price`) negatif ise otomatik olarak `0.00` yapınız.

### Şablon:
Aşağıdaki şablondaki boşlukları doldurarak tetikleyiciyi oluşturunuz:

```sql
DELIMITER //

CREATE TRIGGER before_product_insert
BEFORE INSERT ON products
FOR EACH ROW
BEGIN
    -- 1. Ürün isminin ilk harfini büyütme ve kalanını küçük yapma kodunu yazınız:
    -- İpucu: CONCAT, UPPER, SUBSTRING fonksiyonlarını ve NEW.name değişkenini kullanınız.
    
    SET NEW.name = __________________________________________________ ;
    
    -- 2. Fiyat kontrolü kodunu yazınız:
    -- İpucu: IF NEW.price < 0 THEN ... END IF; yapısını kullanınız.
    
    __________________________________________________________________
    __________________________________________________________________
    __________________________________________________________________
    
END //

DELIMITER ;
```

### Test Etme:
Tetikleyiciyi yazıp çalıştırdıktan sonra aşağıdaki komutla test ediniz:
```sql
INSERT INTO products (name, stock_quantity, price) VALUES ('headphone', 30, -50.00);

-- Eklenen veriyi kontrol edelim. İsmin 'Headphone' ve fiyatın 0.00 olduğunu gözlemleyiniz.
SELECT * FROM products;
```

---

## 3. AFTER INSERT Tetikleyicisi (Stok Güncelleme ve Hata Fırlatma)

`AFTER INSERT` tetikleyicileri, veri tabloya başarıyla eklendikten sonra çalışır. Başka tablolardaki ilişkili verileri güncellemek için idealdir.

### Görev:
Bir müşteri sipariş verdiğinde (`orders` tablosuna kayıt eklendiğinde);
1. Sipariş edilen miktar (`quantity`) kadar ürün stoğu (`products` tablosundaki `stock_quantity`) düşürülmelidir.
2. Eğer stokta yeterli ürün yoksa sipariş iptal edilmeli ve `SIGNAL SQLSTATE '45000'` kullanılarak hata mesajı fırlatılmalıdır.

### Şablon:
Aşağıdaki şablondaki boşlukları doldurarak tetikleyiciyi oluşturunuz:

```sql
DELIMITER //

CREATE TRIGGER after_order_insert
AFTER INSERT ON orders
FOR EACH ROW
BEGIN
    DECLARE current_stock INT;
    
    -- Ürünün güncel stok miktarını current_stock değişkenine atayınız:
    SELECT stock_quantity INTO current_stock
    FROM products
    WHERE id = ____________________ ;
    
    -- Stok yeterli mi kontrol ediniz:
    IF current_stock < ____________________ THEN
        -- Hata fırlatma komutunu yazınız:
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Hata: Yetersiz stok! Sipariş tamamlanamadı.';
    ELSE
        -- Stoğu güncelleme (azaltma) komutunu yazınız:
        UPDATE products
        SET stock_quantity = ____________________
        WHERE id = ____________________ ;
    END IF;
END //

DELIMITER ;
```

### Test Etme:
1. **Başarılı Sipariş Testi:**
   ```sql
   INSERT INTO orders (product_id, quantity) VALUES (1, 5);
   
   -- Stoğun 45'e düştüğünü kontrol ediniz:
   SELECT * FROM products WHERE id = 1;
   ```

2. **Başarısız Sipariş Testi (Yetersiz Stok):**
   ```sql
   -- Laptop stoğu 45 kalmış olmalıdır. 100 adet sipariş vermeyi deneyiniz ve hata aldığınızı gözlemleyiniz:
   INSERT INTO orders (product_id, quantity) VALUES (1, 100);
   ```

---

## 4. AFTER UPDATE Tetikleyicisi (Fiyat Değişikliği Loglama)

`AFTER UPDATE` tetikleyicileri, bir satır güncellendikten sonra çalışır. Veri değişim günlüklerini (audit/log) tutmak için sıkça kullanılır.

### Görev:
Bir ürünün fiyatı güncellendiğinde, eski fiyatı ve yeni fiyatı `price_history` tablosuna otomatik olarak kaydetmek istiyoruz. Ancak, fiyat değişmediyse boş yere log kaydı oluşturulmamalıdır.

### Şablon:
Aşağıdaki şablondaki boşlukları doldurarak tetikleyiciyi oluşturunuz:

```sql
DELIMITER //

CREATE TRIGGER after_product_update
AFTER UPDATE ON products
FOR EACH ROW
BEGIN
    -- Yalnızca fiyat değiştiğinde tetiklenmesi için koşulu yazınız:
    -- İpucu: OLD.price ile NEW.price karşılaştırılmalıdır.
    IF ____________________________________ THEN
        -- price_history tablosuna ekleme yapınız:
        INSERT INTO price_history (product_id, old_price, new_price)
        VALUES ( ____________________ , ____________________ , ____________________ );
    END IF;
END //

DELIMITER ;
```

### Test Etme:
1. **Fiyatı Güncelleyelim:**
   ```sql
   UPDATE products SET price = 16200.00 WHERE id = 1;
   ```

2. **Geçmişi ve Ürünü Kontrol Edelim. Yeni kaydın geçmiş tablosuna eklendiğini gözlemleyiniz:**
   ```sql
   SELECT * FROM price_history;
   ```

---

## 5. AFTER DELETE Tetikleyicisi (Denetim Loglama)

`AFTER DELETE` tetikleyicileri, bir kayıt tablodan tamamen silindikten sonra çalışır.

### Görev:
Bir ürün sistemden silindiğinde (`products` tablosundan silindiğinde), bu silme işlemini `audit_log` tablosuna silinen ürünün ID ve isim bilgisiyle birlikte kaydeden tetikleyiciyi yazınız.

### Şablon:
Aşağıdaki şablondaki boşlukları doldurarak tetikleyiciyi oluşturunuz:

```sql
DELIMITER //

CREATE TRIGGER after_product_delete
AFTER DELETE ON products
FOR EACH ROW
BEGIN
    -- audit_log tablosuna log kaydı ekleyen kodu yazınız:
    -- İpucu: OLD.id ve OLD.name değerlerini CONCAT ile birleştirerek log_message alanına kaydediniz.
    INSERT INTO audit_log (action_type, log_message)
    VALUES ('DELETE', __________________________________________________ );
END //

DELIMITER ;
```

### Test Etme:
1. **Bir Ürünü Silelim:**
   ```sql
   -- 2. adımda eklediğimiz Headphone ürününü silelim (id değeri tablonuzda ne ise onu yazınız):
   DELETE FROM products WHERE name = 'Headphone';
   ```

2. **Log Tablosunu Kontrol Edelim. Silme işlemine dair mesajın log tablosuna eklenip eklenmediğini inceleyiniz:**
   ```sql
   SELECT * FROM audit_log;
   ```

---

## Alıştırmalar (Süre: Yaklaşık 20 Dakika)

Aşağıdaki alıştırmaları sıfırdan şablon kullanmadan tek başınıza çözmeye çalışınız.

1. **Alıştırma 1 (BEFORE INSERT - Sipariş Miktarı Doğrulama):**
   Bir müşteri sipariş oluştururken (`orders` tablosuna ekleme yapılırken), sipariş miktarı (`quantity`) sıfır veya negatif girilemesin. Eğer kullanıcı hatalı (<= 0) bir sipariş miktarı girerse `SIGNAL SQLSTATE` kullanarak bir hata mesajı döndürünüz ve siparişin veritabanına eklenmesini engelleyiniz.

2. **Alıştırma 2 (Tetikleyici Listeleme ve Silme):**
   - Veritabanınızda tanımlı tüm tetikleyicileri listelemek için hangi SQL komutunu kullanırsınız?
   - Yazdığınız `before_product_insert` tetikleyicisini silmek (drop) için hangi SQL komutunu kullanırsınız?

---

### Checklist

Yazmanızın beklendiği toplam **7 adet SQL kod bloğu/komutu** vardır:

1. **`before_product_insert`** tetikleyici kodu (Bölüm 2)
2. **`after_order_insert`** tetikleyici kodu (Bölüm 3)
3. **`after_product_update`** tetikleyici kodu (Bölüm 4)
4. **`after_product_delete`** tetikleyici kodu (Bölüm 5)
5. **Alıştırma 1** için hazırladığınız tetikleyici kodu (BEFORE INSERT on orders)
6. **Alıştırma 2** - Tetikleyicileri listeleme komutu
7. **Alıştırma 2** - `before_product_insert` tetikleyicisini silme (drop) komutu

> SQL kodlarınızı test ettikten sonra çalıştığını gösteren PhpMyAdmin/MySQL Workbench ekran görüntülerini (tetikleyici öncesi ve sonrası ilgili tabloların satırlarını) Ekampüs'te açılmış olan alana süresi içerisinde .png formatında yükleyiniz.


