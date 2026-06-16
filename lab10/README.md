# LAB-10

## MySQL İşlemleri (Transactions)

Veritabanı yönetim sistemlerinde **Transaction (İşlem)**, bir veya birden fazla SQL sorgusunun tek bir mantıksal iş birimi olarak gruplandırılmasıdır. Bir işlem içerisindeki tüm sorgular ya tamamen başarılı bir şekilde veritabanına uygulanır ya da herhangi bir hata durumunda tamamı iptal edilerek veritabanı işlem öncesindeki durumuna geri döndürülür.

İşlemler, veritabanının kararlılığını ve veri bütünlüğünü korumak için **ACID** prensiplerine dayanır:
- **Atomicity (Bütünlük/Bölünemezlik):** İşlemdeki tüm adımlar tek bir bütün sayılır. Ya hepsi gerçekleşir ya da hiçbiri gerçekleşmez.
- **Consistency (Tutarlılık):** İşlem öncesinde ve sonrasında veritabanı tanımlanmış tüm kurallara (kısıtlamalar, veri tipleri vb.) uygun kalmalıdır.
- **Isolation (Yalıtım):** Aynı anda çalışan farklı işlemler birbirlerinin ara durumlarını göremez veya etkileyemez.
- **Durability (Kalıcılık):** Başarıyla tamamlanmış (committed) bir işlemin sonuçları kalıcıdır ve sistem çökse dahi kaybolmaz.

---

### Genel İşlem Sözdizimi (Syntax)

MySQL'de bir işlemi yönetmek için kullanılan temel komutlar şunlardır:

#### 1. İşlemi Başlatma
Bir işlemi başlatmak ve otomatik kayıt (autocommit) modunu geçici olarak askıya almak için kullanılır:
```sql
START TRANSACTION;
-- veya alternatif olarak:
BEGIN;
```

#### 2. İşlemi Onaylama (Kalıcı Hale Getirme)
İşlem içerisinde yapılan tüm değişiklikleri veritabanına kalıcı olarak yazar ve işlemi sonlandırır:
```sql
COMMIT;
```
*Örnek Kullanım:*
```sql
START TRANSACTION;
UPDATE hesaplar SET bakiye = bakiye - 100 WHERE id = 1;
UPDATE hesaplar SET bakiye = bakiye + 100 WHERE id = 2;
COMMIT; -- Güncellemeler veritabanına kalıcı olarak kaydedilir.
```

#### 3. İşlemi İptal Etme (Geri Alma)
İşlem başlangıcından veya en son kayıt noktasından itibaren yapılan tüm değişiklikleri iptal ederek veritabanını işlem başlamadan önceki durumuna döndürür:
```sql
ROLLBACK;
```
*Örnek Kullanım:*
```sql
START TRANSACTION;
UPDATE hesaplar SET bakiye = bakiye - 100 WHERE id = 1;
UPDATE hesaplar SET bakiye = bakiye + 100 WHERE id = 2;
-- Bir hata oluştuğunu veya bakiyenin yetersiz olduğunu varsayalım:
ROLLBACK; -- Yapılan güncellemeler geri alınır, hesaplardaki paralar eski haline döner.
```

#### 4. Kontrol Noktası (Kayıt Noktası) Oluşturma
İşlem içerisinde daha sonra belirli bir yere geri dönebilmek için bir işaretleyici (milestone) tanımlar:
```sql
SAVEPOINT savepoint_adi;
```

#### 5. Kontrol Noktasına Geri Dönme
Tüm işlemi iptal etmek yerine, sadece belirlenen kontrol noktasına kadar olan değişiklikleri geri alır:
```sql
ROLLBACK TO savepoint_adi;
```

#### 6. Kontrol Noktasını Silme
Oluşturulan kontrol noktasını bellekten temizler:
```sql
RELEASE SAVEPOINT savepoint_adi;
```

> **Not:** MySQL varsayılan olarak `autocommit` (otomatik onaylama) modundadır. Yani çalıştırdığınız her bağımsız sorgu anında COMMIT edilir. Birden fazla sorguyu tek bir işlemde birleştirmek için `START TRANSACTION` komutu kullanılmalıdır.

---

## 1. Hazırlık ve Tablo Yapısı

Bu laboratuvar uygulamasında bir **Borsa (Stock Exchange - Emtia Değişimi)** senaryosu üzerinde çalışacağız. İki yatırımcının (traders) ellerindeki emtiaları (altın, gümüş vb.) takas etmesini simüle edeceğiz. Takas işleminin mantığı gereği, **bir tarafın transferi başarısız olursa diğer tarafın transferi de iptal edilmeli (roll-back)** ve veri tutarsızlığı önlenmelidir.

Öncelikle aşağıdaki tabloları oluşturunuz ve örnek verileri ekleyiniz:

```sql
-- Yatırımcılar tablosu
CREATE TABLE traders (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    balance DECIMAL(15, 2) NOT NULL
);

-- Yatırımcıların sahip olduğu emtialar (varlıklar) tablosu
CREATE TABLE assets (
    trader_id INT UNSIGNED NOT NULL,
    asset_name VARCHAR(20) NOT NULL,
    quantity DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (trader_id, asset_name),
    FOREIGN KEY (trader_id) REFERENCES traders(id) ON DELETE CASCADE
);

-- Başlangıç verilerini ekleyelim
INSERT INTO traders (name, balance) VALUES
('Ahmet', 10000.00),
('Mehmet', 5000.00);

INSERT INTO assets (trader_id, asset_name, quantity) VALUES
(1, 'Altın', 10.00),  -- Ahmet'in 10 birim Altın'ı var
(1, 'Gümüş', 0.00),   -- Ahmet'in hiç Gümüş'ü yok
(2, 'Altın', 0.00),   -- Mehmet'in hiç Altın'ı yok
(2, 'Gümüş', 100.00); -- Mehmet'in 100 birim Gümüş'ü var
```

---

## 2. Başarılı Emtia Takası (START TRANSACTION ve COMMIT)

Ahmet ile Mehmet kendi aralarında bir takas anlaşması yaparlar:
- **Ahmet**, Mehmet'e **2 birim Altın** verecektir.
- **Mehmet**, Ahmet'e **20 birim Gümüş** verecektir.
Bu takasın güvenli bir şekilde gerçekleşmesi için her iki güncelleme sorgusu da tek bir işlem bloğunda çalıştırılmalı ve başarıyla tamamlandıktan sonra onaylanmalıdır.

### Görev:
Aşağıdaki işlem bloğunu tamamlayarak takas işlemini gerçekleştiriniz ve değişiklikleri kalıcı hale getiriniz.

### Şablon:
Aşağıdaki şablondaki boşlukları uygun SQL ifadeleriyle doldurunuz:

```sql
-- 1. İşlemi başlatınız
____________________ ;

-- 2. Ahmet'in Altın miktarını 2 azaltınız
UPDATE assets 
SET quantity = quantity - ____ 
WHERE trader_id = ____ AND asset_name = 'Altın';

-- 3. Mehmet'in Altın miktarını 2 artırınız
UPDATE assets 
SET quantity = quantity + ____ 
WHERE trader_id = ____ AND asset_name = 'Altın';

-- 4. Mehmet'in Gümüş miktarını 20 azaltınız
UPDATE assets 
SET quantity = quantity - ____ 
WHERE trader_id = ____ AND asset_name = 'Gümüş';

-- 5. Ahmet'in Gümüş miktarını 20 artırınız
UPDATE assets 
SET quantity = quantity + ____ 
WHERE trader_id = ____ AND asset_name = 'Gümüş';

-- 6. İşlemi onaylayarak kalıcı hale getirin
____________________ ;
```

### Test Etme:
İşlem bloğunu çalıştırdıktan sonra aşağıdaki sorguyla varlıkları kontrol ediniz. Ahmet'in 8 Altın ve 20 Gümüş'ü, Mehmet'in ise 2 Altın ve 80 Gümüş'ü olduğunu doğrulayınız:
```sql
SELECT t.name, a.asset_name, a.quantity 
FROM traders t 
JOIN assets a ON t.id = a.trader_id;
```

---

## 3. Başarısız Takas ve Geri Alma (ROLLBACK)

Şimdi Ahmet ile Mehmet yeni bir takas gerçekleştirmek istesin:
- **Ahmet**, Mehmet'e **15 birim Altın** vermeyi taahhüt etsin.
- **Mehmet**, Ahmet'e **30 birim Gümüş** versin.

Ancak Ahmet'in elinde şu an sadece 8 birim Altın bulunmaktadır (Önceki başarılı işlem sonrası). Ahmet'in altın transferi yetersiz bakiye nedeniyle mantıksal olarak başarısız olmalı veya veritabanı kısıtlamalarını aşmamalıdır. Bir tarafın işlemi başarısız olduğunda, Mehmet'in gümüş transferi gerçekleşmiş olsa bile tüm işlem geri alınmalıdır (ROLLBACK).

### Görev:
İşlemi başlatıp güncelleme sorgularını çalıştırınız. Ahmet'in yeterli altını olmadığını fark ettiğinizde veya sistemde bir hata oluştuğunda yapılan değişiklikleri geri alacak komutu yazınız.

### Şablon:
Aşağıdaki şablondaki boşlukları doldurunuz:

```sql
-- 1. İşlemi başlatınız
____________________ ;

-- 2. Mehmet, Ahmet'e 30 birim Gümüş göndersin (Mehmet'in gümüşü azaltılır, Ahmet'inki artırılır)
UPDATE assets SET quantity = quantity - 30 WHERE trader_id = 2 AND asset_name = 'Gümüş';
UPDATE assets SET quantity = quantity + 30 WHERE trader_id = 1 AND asset_name = 'Gümüş';

-- 3. Ahmet, Mehmet'e 15 birim Altın göndersin
-- DİKKAT: Ahmet'in elinde sadece 8 Altın var! Bu güncelleme sonucunda Ahmet'in altını negatif (-7) olacaktır.
UPDATE assets SET quantity = quantity - 15 WHERE trader_id = 1 AND asset_name = 'Altın';
UPDATE assets SET quantity = quantity + 15 WHERE trader_id = 2 AND asset_name = 'Altın';

-- 4. Yetersiz bakiye durumunu fark ettiğimiz için tüm işlemleri iptal edip başlangıç durumuna geri sarın
____________________ ;
```

### Test Etme:
`ROLLBACK` komutundan sonra varlık tablosunu tekrar sorgulayınız. Mehmet'in gümüş miktarının 80'de, Ahmet'in altın miktarının ise 8'de (yani Bölüm 2 sonundaki değerlerde) kaldığını ve hiçbir değişikliğin yansımadığını gözlemleyiniz:
```sql
SELECT t.name, a.asset_name, a.quantity 
FROM traders t 
JOIN assets a ON t.id = a.trader_id;
```

---

## 4. Kayıt Noktaları Kullanımı (SAVEPOINT ve ROLLBACK TO)

Bazen büyük bir işlem bloğu içerisinde bazı adımların başarısız olması durumunda tüm işlemi en başa sarmak yerine, sadece belirli bir kontrol noktasına (Savepoint) geri dönmek isteyebiliriz.

Senaryomuz:
1. **İşlem Başlangıcı:** Ahmet, Mehmet'e **1 Altın** göndersin; Mehmet de Ahmet'e **10 Gümüş** göndersin. (Bu kısım sorunsuz).
2. **Kayıt Noktası (Savepoint):** Bu aşamada bir savepoint oluşturalım.
3. **İkinci Adım (Hatalı Girişim):** Ahmet daha sonra Mehmet'e **50 Altın** daha göndermeye çalışsın. Bu miktar Ahmet'in limitlerini aşacağı için bu alt adımı geri alalım ama ilk adımı bozmayalım.

### Görev:
Aşağıdaki şablonda belirtilen adımları takip ederek savepoint yapısını uygulayınız.

### Şablon:
Aşağıdaki şablondaki boşlukları doldurunuz:

```sql
-- 1. İşlemi başlatın
____________________ ;

-- 2. Birinci transfer (Ahmet 1 Altın gönderir, Mehmet alır)
UPDATE assets SET quantity = quantity - 1 WHERE trader_id = 1 AND asset_name = 'Altın';
UPDATE assets SET quantity = quantity + 1 WHERE trader_id = 2 AND asset_name = 'Altın';

-- 3. Birinci transfer (Mehmet 10 Gümüş gönderir, Ahmet alır)
UPDATE assets SET quantity = quantity - 10 WHERE trader_id = 2 AND asset_name = 'Gümüş';
UPDATE assets SET quantity = quantity + 10 WHERE trader_id = 1 AND asset_name = 'Gümüş';

-- 4. 'ilk_takas' adında bir kayıt noktası (SAVEPOINT) oluşturun
________________________________________ ;

-- 5. İkinci transfer girişimi (Ahmet Mehmet'e 50 Altın göndermeye çalışıyor - Yetersiz Bakiye Hatalı Adım!)
UPDATE assets SET quantity = quantity - 50 WHERE trader_id = 1 AND asset_name = 'Altın';
UPDATE assets SET quantity = quantity + 50 WHERE trader_id = 2 AND asset_name = 'Altın';

-- 6. Hatalı 50 Altın transferini iptal etmek için 'ilk_takas' savepoint noktasına geri dönün
________________________________________ ;

-- 7. İlk başarılı takası onaylayıp işlemi kalıcı olarak tamamlayın
____________________ ;
```

### Test Etme:
İşlem tamamlandıktan sonra verileri sorgulayınız. Ahmet'in 7 Altın ve 30 Gümüş'ü olduğunu, hatalı olan 50 Altınlık transfer girişiminin ise tamamen temizlendiğini doğrulayınız.

---

## Alıştırmalar (Süre: Yaklaşık 20 Dakika)

Aşağıdaki alıştırmaları şablon kullanmadan, yukarıdaki öğrendikleriniz doğrultusunda çözünüz.

1. **Alıştırma 1 (Bakiye ve Stok Kontrollü Güvenli İşlem):**
   Normal şartlarda SQL komutlarını elle çalıştırırken bakiyeyi gözümüzle kontrol edip `ROLLBACK` yazarız. Ancak bunu programatik veya SQL prosedürleri içerisinde yapmak için koşul ifadelerinden yararlanırız.
   * `traders` tablosundaki Ahmet'in parasını (`balance`) kullanarak Mehmet'ten 1000 TL değerinde emtia satın alacağını varsayalım.
   * Bir işlem başlatın. Ahmet'in bakiyesinden 1000 TL düşürün, Mehmet'in bakiyesine 1000 TL ekleyin.
   * Ardından Ahmet'in yeni bakiyesini kontrol edin. Eğer Ahmet'in bakiyesi negatif (`< 0`) duruma düştüyse işlemi `ROLLBACK` yapın, düşmediyse `COMMIT` yapın. Bu kontrolü sağlayan mantıksal SQL komut yapısını kurunuz.

2. **Alıştırma 2 (Autocommit Modunu Değiştirme):**
   * MySQL'de o anki oturum için `autocommit` ayarının durumunu sorgulayan SQL komutunu yazınız.
   * Otomatik onaylama modunu kapatmak (böylece her sorgunun otomatik commit edilmesini engellemek) için hangi komut kullanılır? Yazınız.

---

### Teslim Edilecek Kodlar Checklist

Ekampüs'e yükleyeceğiniz `.sql` dosyasında aşağıdaki SQL kodları yer almalıdır:

1. **Bölüm 2:** Başarılı emtia takası transaction kod bloğu (`START TRANSACTION` ... `COMMIT`)
2. **Bölüm 3:** Başarısız emtia takası ve geri alma kod bloğu (`START TRANSACTION` ... `ROLLBACK`)
3. **Bölüm 4:** Savepoint uygulaması kod bloğu (`START TRANSACTION` ... `SAVEPOINT` ... `ROLLBACK TO` ... `COMMIT`)
4. **Alıştırma 1:** Bakiye kontrollü mantıksal transaction kodları
5. **Alıştırma 2:** Autocommit sorgulama ve kapatma komutları

> Tüm SQL kodlarınızı Ekampüs'te açılmış olan alana süresi içerisinde .sql formatında yükleyiniz.
