# DAST

### DAST Nedir?

**DAST (Dynamic Application Security Testing)**, bir uygulama çalışır durumdayken güvenlik açıklarını tespit etmek amacıyla gerçekleştirilen dinamik uygulama güvenlik testidir.

DAST yaklaşımında uygulamanın kaynak kodu doğrudan incelenmez. Bunun yerine çalışan uygulama ile dışarıdan etkileşime geçilir ve uygulamanın farklı girdiler karşısındaki davranışı analiz edilir. Bu nedenle DAST, **black-box (kara kutu) güvenlik testi** yaklaşımı olarak değerlendirilir.

DAST araçları, uygulamanın web arayüzü veya API'leri üzerinden istekler göndererek potansiyel güvenlik açıklarını araştırır. Bu sayede yalnızca kaynak kodunda görülebilen problemlerden ziyade, uygulamanın çalışma anındaki davranışından kaynaklanan güvenlik sorunları da tespit edilebilir.

### DAST Nasıl Çalışır?

DAST sürecinde temel olarak aşağıdaki adımlar gerçekleştirilir:

1. Uygulama çalışır durumda olacak şekilde başlatılır.
2. DAST aracı uygulamanın erişilebilir endpoint'lerini ve kaynaklarını keşfeder.
3. Uygulamaya farklı HTTP istekleri ve test girdileri gönderilir.
4. Uygulamanın verdiği cevaplar analiz edilir.
5. Güvenlik açısından şüpheli davranışlar tespit edilir.
6. Tespit edilen bulgular raporlanır.

DAST araçları uygulamaya gerçek saldırı tekniklerini veya saldırı benzeri test girdilerini uygulayabildiğinden, testlerin kontrollü ve yetkili ortamlarda gerçekleştirilmesi gerekir.

### DAST ile Tespit Edilebilecek Güvenlik Problemleri

DAST, özellikle çalışan web uygulamalarında ortaya çıkan güvenlik problemlerinin tespit edilmesinde kullanılır.

Örnek olarak:

* **Cross-Site Scripting (XSS)**
* **SQL Injection**
* Girdi ve çıktı doğrulama problemleri
* Authentication problemleri
* Bazı server ve uygulama konfigürasyon hataları
* Güvenlik başlıklarıyla ilgili problemler
* Uygulamanın beklenmeyen girdiler karşısındaki güvenlik sorunları

OWASP, DAST araçlarının özellikle input/output validation, authentication ve server configuration gibi alanlarda güvenlik problemlerinin tespit edilmesine yardımcı olabileceğini belirtmektedir.

### DAST'ın Avantajları

#### Kaynak Koduna İhtiyaç Duymaz

DAST, uygulamayı dışarıdan test ettiği için kaynak koduna erişim gerektirmez. Bu nedenle farklı teknoloji ve framework'lerle geliştirilmiş uygulamalarda kullanılabilir.

#### Gerçek Çalışma Ortamını Test Edebilir

Uygulamanın runtime davranışı incelendiği için yalnızca kod seviyesinde görülemeyen bazı güvenlik problemlerinin ortaya çıkarılmasına yardımcı olur.

#### Otomatikleştirilebilir

DAST taramaları CI/CD süreçlerine veya belirli zaman aralıklarında gerçekleştirilen güvenlik kontrollerine dahil edilebilir.

#### Web Uygulamaları ve API'lerde Kullanılabilir

DAST araçları web uygulamalarının yanı sıra uygun yapılandırmalarla API endpoint'lerinin de güvenlik açısından test edilmesinde kullanılabilir.

### DAST'ın Sınırlamaları

DAST tek başına uygulamadaki tüm güvenlik problemlerini tespit edemez.

Örneğin iş mantığı hataları, bazı yetkilendirme problemleri ve karmaşık uygulama davranışları otomatik DAST taramalarının dışında kalabilir. OWASP ZAP dokümantasyonu da otomatik active scan işlemlerinin tüm güvenlik açıklarını bulamayacağını ve özellikle mantıksal güvenlik problemleri için manuel güvenlik testlerinin gerekli olabileceğini belirtmektedir.

Bu nedenle DAST'ın **SAST, SCA ve manuel penetration testing gibi diğer güvenlik testleriyle birlikte kullanılması** daha kapsamlı bir güvenlik yaklaşımı sağlar.

### DAST ve SAST Arasındaki Fark

<table data-search="false"><thead><tr><th>Özellik</th><th>SAST</th><th>DAST</th></tr></thead><tbody><tr><td>Açılım</td><td>Static Application Security Testing</td><td>Dynamic Application Security Testing</td></tr><tr><td>Test edilen yapı</td><td>Kaynak kod</td><td>Çalışan uygulama</td></tr><tr><td>Kaynak kodu gerekir mi?</td><td>Genellikle evet</td><td>Hayır</td></tr><tr><td>Yaklaşım</td><td>White-box</td><td>Black-box</td></tr><tr><td>Uygulama çalışıyor mu?</td><td>Gerekmez</td><td>Evet</td></tr><tr><td>Örnek araç</td><td>Semgrep</td><td>OWASP ZAP</td></tr><tr><td>Örnek bulgular</td><td>Güvensiz kod kullanımı</td><td>XSS, SQL Injection vb.</td></tr></tbody></table>

### DAST'ın DevSecOps İçerisindeki Yeri

DAST, uygulamanın geliştirme ve dağıtım süreçlerinde güvenlik kontrollerinin bir parçası olarak kullanılabilir.

Örneğin bir CI/CD sürecinde uygulama deploy edildikten sonra DAST taraması çalıştırılabilir. Tarama sonucunda kritik seviyede güvenlik açıkları tespit edilirse deployment sürecinin durdurulması veya güvenlik ekibine bildirim gönderilmesi sağlanabilir.

Bu yaklaşım sayesinde güvenlik testleri yalnızca geliştirme sürecinin sonunda yapılan manuel kontroller olmaktan çıkarılarak DevSecOps sürecinin sürekli bir parçası haline getirilebilir.

### DAST Araçları

DAST gerçekleştirmek için farklı araçlar kullanılabilir. Bunlardan biri **OWASP ZAP (Zed Attack Proxy)**'dir.

OWASP ZAP, web uygulamalarının güvenlik testlerini gerçekleştirmek amacıyla kullanılan açık kaynaklı bir güvenlik aracıdır ve DAST kapsamında değerlendirilebilir. Bir sonraki bölümde ZAP'ın çalışma mantığı, passive scan ve active scan özellikleri ayrıntılı olarak incelenecektir.
