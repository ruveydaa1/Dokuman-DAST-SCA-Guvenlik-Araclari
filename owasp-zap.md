# OWASP ZAP

### OWASP ZAP Nedir?

**OWASP ZAP (Zed Attack Proxy)**, web uygulamalarının güvenlik açıklarını tespit etmek amacıyla kullanılan açık kaynaklı bir web uygulaması güvenlik test aracıdır.

ZAP, OWASP tarafından geliştirilen ve sürdürülen projelerden biridir. Web uygulaması ile kullanıcı arasındaki HTTP/HTTPS trafiğini proxy üzerinden geçirerek uygulamanın istek ve cevaplarının incelenmesine, değiştirilmesine ve güvenlik testlerinin gerçekleştirilmesine olanak sağlar.

ZAP, **DAST (Dynamic Application Security Testing)** kapsamında kullanılabilecek araçlardan biridir. DAST bir güvenlik test yaklaşımıyken ZAP, bu yaklaşımı uygulamak için kullanılabilecek araçlardan biridir.

### ZAP Nasıl Çalışır?

ZAP'ın temel çalışma mantığı bir **intercepting proxy** olarak hareket etmesine dayanır.

Normal bir web uygulaması iletişiminde:

**Kullanıcı → Web Uygulaması**

şeklinde bir iletişim gerçekleşirken ZAP kullanıldığında:

**Kullanıcı → ZAP → Web Uygulaması**

şeklinde bir yapı oluşur.

Bu sayede ZAP, uygulama ile istemci arasındaki HTTP/HTTPS isteklerini ve cevaplarını gözlemleyebilir.

ZAP üzerinden:

* HTTP/HTTPS istekleri görüntülenebilir.
* İstekler değiştirilebilir.
* Uygulamanın endpoint'leri keşfedilebilir.
* Güvenlik testleri gerçekleştirilebilir.
* Tespit edilen güvenlik problemleri raporlanabilir.

### ZAP'ın Temel Bileşenleri

ZAP içerisinde web uygulamalarının güvenlik testlerini gerçekleştirmek için çeşitli özellikler bulunmaktadır.

#### Proxy

ZAP'ın temel özelliklerinden biri proxy olarak çalışmasıdır.

Tarayıcı ZAP proxy'sine yönlendirildiğinde tarayıcı tarafından gönderilen HTTP/HTTPS istekleri ZAP üzerinden geçer. Bu istekler ZAP arayüzünden görüntülenebilir ve gerektiğinde değiştirilebilir.

Bu özellik özellikle uygulamanın nasıl iletişim kurduğunu anlamak ve manuel güvenlik testleri gerçekleştirmek için kullanılabilir.

#### Spider

**Spider**, web uygulamasının içerisinde bulunan sayfaları, endpoint'leri ve bağlantıları keşfetmek için kullanılan bir crawler özelliğidir.

Spider uygulamayı dolaşarak yeni URL'leri ve kaynakları keşfetmeye çalışır.

Örneğin:

```
/login
/users
/profile
/api/users
/api/products
```

gibi endpoint'lerin keşfedilmesine yardımcı olabilir.

Spider'ın temel amacı güvenlik açığı bulmak değil, uygulamanın saldırı yüzeyini keşfetmektir.

#### AJAX Spider

Modern web uygulamalarında JavaScript kullanımı oldukça yaygındır. Geleneksel crawler'lar JavaScript tarafından oluşturulan bazı içerikleri veya bağlantıları keşfetmekte yetersiz kalabilir.

**AJAX Spider**, JavaScript ağırlıklı web uygulamalarındaki içerikleri ve bağlantıları keşfetmek amacıyla kullanılabilir.

Bu nedenle özellikle SPA (Single Page Application) yapısına sahip modern web uygulamalarında faydalıdır.

### Passive Scan

**Passive Scan**, ZAP üzerinden geçen HTTP mesajlarını analiz eder ancak uygulamaya saldırı amaçlı yeni istekler göndermez.

Örneğin ZAP bir HTTP response'u incelerken:

* Güvenlik header'larının eksik olması
* Cookie güvenlik özellikleri
* Bilgi sızıntıları
* Bazı güvenli olmayan yapılandırmalar

gibi problemleri tespit edebilir.

Passive scan'in önemli avantajlarından biri uygulamanın davranışını değiştirmeden veya saldırı benzeri istekler göndermeden analiz gerçekleştirmesidir.

Bu nedenle uygulamanın normal kullanımı sırasında trafik analiz edilerek güvenlik kontrollerinin gerçekleştirilmesine olanak sağlar.

### Active Scan

**Active Scan**, uygulamaya özel olarak hazırlanmış test istekleri göndererek güvenlik açıklarını tespit etmeye çalışır.

Örneğin ZAP aşağıdaki gibi saldırı tekniklerini test etmek için özel girdiler oluşturabilir:

* SQL Injection
* Cross-Site Scripting (XSS)
* Path Traversal
* Command Injection
* Diğer yaygın web uygulaması zafiyetleri

Active Scan, uygulamanın davranışını değiştirebilecek veya veri oluşturabilecek saldırı benzeri istekler gönderebilir. Bu nedenle yalnızca **test edilmesi için yetki alınmış uygulamalarda** kullanılmalıdır.

Özellikle production ortamındaki gerçek sistemler üzerinde kontrolsüz active scan gerçekleştirmek uygun değildir.

### Passive Scan ve Active Scan Karşılaştırması

| Özellik                                   | Passive Scan   | Active Scan          |
| ----------------------------------------- | -------------- | -------------------- |
| Yeni saldırı istekleri gönderir mi?       | Hayır          | Evet                 |
| Mevcut trafik analiz edilir mi?           | Evet           | Evet                 |
| Uygulamanın davranışını etkileyebilir mi? | Düşük risk     | Evet                 |
| Güvenlik testi kapsamı                    | Daha sınırlı   | Daha geniş           |
| Kullanım                                  | Trafik analizi | Aktif güvenlik testi |

### API Güvenlik Testleri

ZAP yalnızca klasik web sayfalarının test edilmesiyle sınırlı değildir. API'lerin güvenlik testlerinde de kullanılabilir.

Özellikle REST API'lerde bulunan endpoint'ler ZAP tarafından keşfedilebilir veya OpenAPI tanımı gibi kaynaklar kullanılarak ZAP'a tanıtılabilir.

Örneğin bir uygulamada:

```
GET    /api/users
POST   /api/users
GET    /api/users/{id}
PUT    /api/users/{id}
DELETE /api/users/{id}
```

gibi endpoint'ler bulunuyorsa bu endpoint'ler üzerinden güvenlik testleri gerçekleştirilebilir.

Bu özellik backend tabanlı uygulamalar için önemlidir çünkü güvenlik açıklarının yalnızca frontend üzerinde değil, API katmanında da bulunması mümkündür.

### ZAP ile Basit Bir Test Akışı

Bir web uygulamasının ZAP ile test edilmesi genel olarak şu şekilde gerçekleştirilebilir:

1. Test edilecek web uygulaması çalıştırılır.
2. ZAP başlatılır.
3. Tarayıcının proxy ayarları ZAP'a yönlendirilir.
4. Uygulama tarayıcı üzerinden kullanılır.
5. ZAP uygulamanın HTTP/HTTPS trafiğini yakalar.
6. Spider veya AJAX Spider ile uygulamanın saldırı yüzeyi keşfedilir.
7. Passive Scan sonuçları incelenir.
8. Yetkili test ortamında Active Scan çalıştırılır.
9. Bulunan güvenlik açıkları incelenir.
10. Sonuçlar raporlanır.

Bu süreç sayesinde uygulamanın hem normal kullanım sırasında oluşturduğu trafik hem de aktif güvenlik testleri sonucunda elde edilen bulgular analiz edilebilir.

### ZAP'ın Avantajları

* Açık kaynaklıdır.
* Web uygulamalarının güvenlik testlerinde kullanılabilir.
* Proxy olarak çalışabilir.
* Otomatik tarama özellikleri sunar.
* Passive ve Active Scan desteğine sahiptir.
* Web uygulamalarının endpoint'lerini keşfetmeye yardımcı olur.
* API güvenlik testlerinde kullanılabilir.
* Manuel ve otomatik güvenlik testlerini birlikte destekler.
* CI/CD süreçlerine entegre edilebilir.

### ZAP'ın Sınırlamaları

ZAP otomatik olarak tüm güvenlik açıklarını tespit edemez.

Özellikle karmaşık iş mantığı, uygulamaya özgü yetkilendirme problemleri veya belirli kullanıcı akışlarına bağlı güvenlik açıklarının tespit edilmesi için manuel testler gerekebilir.

Ayrıca Active Scan saldırı benzeri istekler gönderdiği için test ortamının doğru şekilde hazırlanması önemlidir.

Bu nedenle ZAP, tek başına bütün güvenlik testlerinin yerine geçmek yerine **SAST, SCA ve manuel güvenlik testleriyle birlikte kullanılan bir DAST çözümü** olarak değerlendirilmelidir.

### ZAP'ın DevSecOps İçerisindeki Yeri

ZAP, DevSecOps süreçlerinde otomatik DAST kontrollerinin gerçekleştirilmesinde kullanılabilir.

Örneğin CI/CD pipeline içerisinde uygulama deploy edildikten sonra ZAP çalıştırılabilir:

```
Kod
 ↓
Build
 ↓
Deploy
 ↓
ZAP DAST Scan
 ↓
Sonuçların Analizi
 ↓
Başarılı / Başarısız
```

Belirlenen güvenlik politikalarına göre kritik seviyedeki bulgular pipeline'ın başarısız olmasına neden olabilir.

Bu sayede uygulamanın yalnızca kaynak kodu değil, **çalışan hali de** otomatik olarak güvenlik açısından kontrol edilmiş olur.

### ZAP ve SPM Sistemi

SPM içerisinde ZAP, DAST katmanında konumlandırılabilir.

Genel mimari:

```
UserManagement
      │
      │ Kafka
      ▼
ScannerService
      │
      ├── Semgrep
      │      └── SAST
      │
      ├── Trivy
      │      └── SCA / Vulnerability Scanning
      │
      └── OWASP ZAP
             └── DAST
```

Bu yapı sayesinde ScannerService farklı güvenlik tarama araçlarını merkezi bir servis üzerinden çalıştırabilir.

Semgrep kaynak kodu analiz ederken, Trivy uygulamanın kullandığı bileşenleri ve diğer desteklenen hedefleri tarayabilir. ZAP ise çalışan uygulamanın dışarıdan gözlemlenen davranışını test ederek farklı bir güvenlik katmanı sağlar.

Böylece SPM sistemi içerisinde **SAST, SCA ve DAST** kontrollerinin birlikte uygulanması mümkün hale gelir.
