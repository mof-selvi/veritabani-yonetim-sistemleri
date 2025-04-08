# LAB-06

# MySQL ve Index'ler

# Alıştırma #1
- [Employees Installation](https://dev.mysql.com/doc/employee/en/employees-installation.html) sayfasındaki yönergeleri izleyerek `employees` veritabanını kendi cihazınıza aktarınız.
  - [datacharmer/test_db](https://github.com/datacharmer/test_db) reposunu indirip dosyaları bir klasöre çıkarınız.
  - `employees.sql` dosyasını gördüğünüz dizine giriniz.
  - Dosya gezgininin adres çubuğuna `cmd` yazın ya da `cmd` programını `Başlat` menüsünden açıp `cd` komutu ile SQL dosyalarını çıakrdığınız klasöre geçiş yapınız.
  - Cihazınızdaki MySQL kurulumunun dizinini bulun. Xampp kullanıyorsanız bu `.../xampp/mysql/bin/mysql.exe` şeklinde olacaktır.
  - `mysql.exe` programının tam yolunu (absolute path) komut istemi penceresine yapıştırarak aşağıdakine benzer şekilde aktarım komutunu yazınız.
  
  Windows için:
  ```shell
  C:\...\xampp\mysql\bin\mysql.exe -u root -p < employees.sql
  ```

  Linux için:
  ```shell
  /usr/bin/.../xampp/mysql/bin/mysql -u root -p < employees.sql
  ```
  
  - `INFO` ve `LOADING` kelimeleri içeren bir çıktı alıyorsanız ve henüz yeni komut satırı yazamıyorsanız buraya kadar doğru işlem yaptınız demektir. Yeni komut giriş satırı gelene kadar bekleyiniz.
  - İşlem sonunda `employees` adında bir veritabanınız olacak. PhpMyAdmin ile kontrol ediniz.

- `employees` veritabanındaki yine `employees` adındaki tablonun yapısını ve içerisindeki bazı verileri inceleyiniz.

## Dikkat
> Bu aşamadan itibaren, çalıştıracağınız **tüm SQL kodlarını** ve bu kodların ***bazılarının çalıştırılmasının ardından PhpMyAdmin tarafından sorgu sonucunun üstünde verilen `Sorgu ... saniye sürdü.` biçimindeki cümlede bulunan süreleri*** not etmeniz gerekecektir.
> Sorgu komutunu ve süresini not almadığınız işlemleri tekrar doğru bir şekilde yapabilmek için veritabanını kaldırıp alıştırmaya baştan başlamanız gerekebilir. Dikkat etmek sizin sorumluluğunuzdadır.


# Alıştırma #2

- 31 Ocak 1999 tarihinden sonra işe alınan ilk 150 kişiyi listeleyen SQL sorgusunu yazınız. (SQL sorgusunu not alınız.)
- 31 Ocak 1999 tarihinden sonra işe alınan ikinci 150 kişiyi listeleyen SQL sorgusunu yazınız. (SQL sorgusunu not alınız.)

> Not: tarih/zaman türlerindeki veriler de aynen nümerik veriler gibi karşılaştırılabilir. Dikkat edilmesi gereken nokta; tarihler '...' arasında yazılmalı ve değerler büyük zaman diliminden küçük zaman dilimine doğru verilmelidir. \
> Doğru tarih/zaman değerlerine örnekler (YYYY-MM-DD hh:mm:ss);
> - '2024-12-31'
> - '2024-12-31 16:00'
> - '2024-12-31 16:35:47'

# Alıştırma #3

- Aşağıdaki kod ile bir `players` tablosu oluşturunuz.

```mysql
CREATE TABLE players (
  id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(30) NOT NULL,
  city VARCHAR(20) NOT NULL,
  score INT UNSIGNED NOT NULL DEFAULT 0
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

- [Populate Dummy Data for MySQL](https://raw.githubusercontent.com/kedarvj/mysql-random-data-generator/master/populate.sql) adresinde bulunan MySQL kodlarının tümünü seçip `SQL` sekmesinde çalıştırınız.

- Tekrar `SQL` sekmesine gelip aşağıdaki kod ile tabloya rastgele veriler ekletiniz:

```mysql
call populate('VERİTABANI ADI','players',100000,'N');
```

- Yukarıdaki kodu en az 1 milyon kayıt oluşana kadar tekrar tekrar çalıştırınız.

> Kopyalanan kodlar MySQL sunucusunun çalıştırabileceği bir prosedür (fonksiyon) oluşturur.
> Daha sonra bu `populate` isimli prosedürün çalıştırılmasıyla çok sayıda satırın veritabanına eklenmesi kolaylaşmış olur. \
> 100000 kaydın eklenmesi yaklaşık 1-1.5 dk sürebilir.

- Alternatif olarak [Drive](https://drive.google.com/file/d/1jLJw5aDnb7kcDGpztXBBO7AV1vIXLK2m/view?usp=sharing) bağlantısındaki hazır SQL dosyasını indirip içe aktarabilirsiniz.

- a) İsminde `b` ve `t` karakterlerinin yanyana olduğu oyuncuların sayısını sorgulayınız. (Sorgunuzu ve hemen altına çalışma süresini saniye cinsinden not ediniz.) Bir sorgunun sonuç sayısını almak için sorguyu

```mysql
SELECT COUNT(*) FROM ...
```

şeklinde yazabilirsiniz.

- b) İsmi `bt` ile başlayan oyuncuların sayısını sorgulayınız. (Sorgunuzu ve hemen altına çalışma süresini saniye cinsinden not ediniz.)

- İsim sütunu için bir `index` oluşturunuz. (SQL komutunu ve çalışma süresini alt alta not alınız.) `index` oluşturma komutu için syntax aşağıdaki gibidir (TABLO_ADI ve SUTUN_ADI sırasıyla tablo ve sütun adlarını belirtir. İndex'lerin isimleri genellikle `idx_` önekiyle başlar ve herhangi bir isim verilebilir);

```mysql
CREATE INDEX idx_SUTUN_ADI ON TABLO_ADI (SUTUN_ADI);
```

> Not: çok sayıda satırın bulunduğu bir tabloda bir ya da daha fazla sütun için index eklerken veri sayısına bağlı olarak bir süre beklememiz gerekebilir.

- Yukarıdaki a ve b şıklarındaki işlemleri tekrarlayınız. Sorgularınızı ve çalışma sürelerini not almayı unutmayınız.

- Aşağıdaki soruların her birini birkaç cümleyle yanıtlayarak yorumlarınızı belirtiniz. Cümlelerinizi, sorgularınızı not aldığınız metnin altına devam ederek yazabilirsiniz.

    - İndex oluşturduktan sonra iki sorgu için de sürelerde belirgin bir değişiklik gözlemlediniz mi? Gözlemlerinizin sebebi ne olabilir?
    - İndex varken, b şıkkındaki sorgunun çalışma süresi ile a şıkkındaki sorgunun çalışma süresi arasında dikkat çeken bir fark var mıdır? Yorumunuzu olası sebepleri belirterek ifade ediniz.



# Gönderim

Şu ana kadar notlarınız 6 adet SQL sorgusu, 4 adet çalışma süresi ve 2 kısa yorum paragrafı içeriyor olmalı. 

Aldığınız notları metin halinde Ekampüs sayfasındaki ilgili yerden göndermeyi unutmayınız.


> Not: PhpMyAdmin, sorgular sonucunda dönen satırları biçimlendirip bize gösterir. Arkaplanda çalışan başka işlemler de bulunmaktadır. Bu yüzden bize gösterdiği süreler tam tamına SQL sorgu süresi olmayabilir. Ayrıca MySQL vb pek çok veritabanı yönetim sistemi, sorguları optimize etmek için önbellek kullanır. Böylece sorgular beklediğimizden daha hızlı çalışabilir. Bu etkinlik, teorik bilgileri destekleyebilecek olan ampirik gözlemler yapabilme amacıyla hazırlanmıştır.

