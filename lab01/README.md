# MySQL / MariaDB kurulumu
* Bölümümüzce verilen Web Tabanlı Programlama dersi için gerekli olan yazılım paketleri içerisinde de bulunduğundan, bu dersimizde [XAMPP](https://www.apachefriends.org/tr/index.html) içerisindeki MySQL kurulumu kullanılacaktır.
* XAMPP içerisindeki DBMS yazılımı aslen MariaDB'dir fakat bundan sonra kendisinden MySQL olarak bahsedilecektir.
* Birden fazla MySQL servisi kurmak mümkündür fakat aynı anda birden fazla uygulama tek portu kullanamayacağı için hepsine farklı port tanımlamak gerekecektir.
* Uygulama derslerimizde veritabanlarımızı görüntülemek için PhpMyAdmin ve MySQL Workbench gibi yazılımlar kullanıyor olacağız.

# Veritabanı görüntüleme araçları
* MySQL'i başlatmak için xampp-control.exe programını açınız ve MySQL servisi çalıştırınız.
* PhpMyAdmin kullanmak için Apache servisini de çalıştırmamız gerekecektir. Eğer MySQL Workbench kullanacaksanız bu gerekmez.
* PhpMyAdmin için tarayıcınızı açınız ve [http://localhost/phpmyadmin](http://localhost/phpmyadmin) veya [http://127.0.0.1/phpmyadmin](http://127.0.0.1/phpmyadmin) adresine gidiniz.
  Dikkat
  > Eğer sayfayı görüntüleyemiyorsanız Apache portunuzu XAMPP programından kontrol ediniz. 80 haricinde bir port görüyorsanız o portu adresteki localhost ifadesinden sonra ":" kullanarak belirtmelisiniz.
  > Örneğin, portunuz 8080 ise: [http://localhost:8080/phpmyadmin](http://localhost:8080/phpmyadmin)
  > 80 portunu kullanan başka bir servis olduğunda ya o servisi durdurmak ya da Apache için farklı bir port kullanmak gerekir.
  > Bu gibi durumlarda XAMPP içerisinde Config > Apache (httpd.conf) ayarlarına giderek "Listen" ile başlayan satırı bulup boş bir port numarası giriniz.
  MySQL'i farklı bir port ile başlatmak için:
  > XAMPP içerisinde Config > my.ini seçeneğinden MySQL ayar dosyasını açınız.
  > "port=3306" şeklinde olan satırları uygun bir port numarası ile düzeltip dosyayı kaydederek kapatınız.
  > MySQL'i yeniden çalıştırmayı denediğinizde, port boşsa, servis sorunsuz bir şekilde açılacaktır.
  Dikkat
  > MySQL'in varsayılan portunu değiştirirseniz bunu PhpMyAdmin ayar dosyasında da belirtmeniz gerekebilir.
  > XAMPP/phpMyAdmin/config.inc.php dosyasını açınız ve
  > $cfg['Servers'][$i]['host'] = '127.0.0.1';
  > şeklinde görülen satırı
  > $cfg['Servers'][$i]['host'] = '127.0.0.1:3307';
  > şeklinde port numarasını ekleyerek değiştiriniz.
* MySQL Workbench ile, yeni bir bağlantı oluştururken XAMPP ile gelen MySQL'in portunu ve bağlantı bilgilerini vererek de aynı veritabanına bağlanabilirsiniz.

# Örnek bir veritabanı incelemesi
* [mysqlsampledatabase.sql](mysqlsampledatabase.sql) dosyasını bilgisayarınıza indirin.
* Kullandığınız veritabanı yönetim uygulaması üzerinden yeni bir veritabanı oluşturun.
* İndirdiğiniz dosyadaki verileri; ister SQL dosyasının içeriğini kopyalayarak, ister SQL dosyasını doğrudan import ederek veritabanınıza kayıt edin.
* Tablo isimlerini ve yapılarını inceleyiniz.
* Veritabanı kayıtlarını görüntülemeyi deneyiniz.
* Veritabanı şemasını ve tablolar arasındaki bağlantıları görüntüleyip yorumlayınız.

---

* Sonraki uygulamalarımızda bu veritabanındaki kodları kendimiz yazabiliyor olacağız. Bir sonraki uygulama dersine kadar gerekli yazılımları bilgisayarlarınıza yükleyiniz.
* Hata alırsanız yardım isteyebilirsiniz. Yardım için lütfen dersimizin Telegram grubunu veya e-posta yolunu tercih ediniz.
