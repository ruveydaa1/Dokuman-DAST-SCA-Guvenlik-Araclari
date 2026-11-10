# SCA

### SCA Nedir?

**SCA (Software Composition Analysis)**, bir yazılım projesinde kullanılan üçüncü parti ve açık kaynaklı bileşenlerin güvenlik açısından analiz edilmesini sağlayan bir güvenlik yaklaşımıdır.

Modern yazılım projelerinde uygulamanın tamamının geliştirici tarafından yazılması yerine birçok hazır kütüphane, framework ve paket kullanılmaktadır. Bu bağımlılıklar geliştirme sürecini hızlandırırken beraberinde güvenlik risklerini de getirebilir.

Bir uygulamada kullanılan bir kütüphanenin bilinen bir güvenlik açığına sahip olması, bu kütüphaneyi kullanan uygulamanın da risk altında olmasına neden olabilir.

SCA'nın temel amacı, uygulamada kullanılan bu bileşenleri tespit etmek ve bilinen güvenlik açıkları açısından analiz etmektir.

### SCA Neden Gereklidir?

Bir yazılım projesinde kullanılan bağımlılıkların tamamını manuel olarak takip etmek oldukça zor olabilir.

Örneğin bir uygulamada doğrudan kullanılan birkaç paket bulunabilir:

```
Application
 ├── Package A
 ├── Package B
 └── Package C
```

Ancak bu paketlerin kendi bağımlılıkları da olabilir:

```
Application
 ├── Package A
 │    ├── Package D
 │    └── Package E
 │
 ├── Package B
 │    └── Package F
 │
 └── Package C
```

Bu nedenle uygulamada doğrudan kullanılmayan ancak başka bir paketin bağımlılığı olarak gelen bileşenlerde de güvenlik açığı bulunabilir.

SCA araçları bu bağımlılıkları analiz ederek güvenlik açısından risk oluşturabilecek bileşenlerin belirlenmesine yardımcı olur.

### Direct ve Transitive Dependency

SCA'yı anlamak için dependency türlerini ayırmak önemlidir.

#### Direct Dependency

Uygulamanın doğrudan eklediği bağımlılıktır.

Örneğin bir .NET projesinde:

```xml
<PackageReference Include="SomePackage" Version="1.0.0" />
```

şeklinde tanımlanan paket bir **direct dependency** olarak değerlendirilebilir.

#### Transitive Dependency

Uygulamanın doğrudan eklemediği ancak kullandığı başka bir paketin bağımlılığı nedeniyle projeye dahil olan bileşendir.

Örneğin:

```
Application
     │
     ▼
Package A
     │
     ▼
Package B
```

Burada Application doğrudan Package A'yı kullanırken Package B, Package A'nın bağımlılığı olarak projeye dahil olabilir.

Transitive dependency'lerin manuel olarak fark edilmesi zor olabileceği için SCA araçları bu bileşenlerin tespit edilmesinde önemli rol oynar.

### SCA ile Neler Tespit Edilebilir?

SCA araçlarının özelliklerine göre değişmekle birlikte temel olarak aşağıdaki güvenlik problemleri araştırılabilir:

* Bilinen güvenlik açıklarına sahip bağımlılıklar
* Güncel olmayan ve güvenlik açısından risk taşıyan paket sürümleri
* Vulnerable transitive dependencies
* Kullanılan açık kaynak bileşenlerinin envanteri
* Bazı araçlarda lisans problemleri
* Bazı araçlarda kullanılmaması gereken veya riskli bileşenler

SCA sonucunda bir dependency'nin belirli bir güvenlik açığıyla ilişkili olduğu tespit edilebilir.

### CVE Nedir?

**CVE (Common Vulnerabilities and Exposures)**, kamuya açık olarak bilinen güvenlik açıklarına verilen standart kimlik numarasıdır.

Bir güvenlik açığı CVE veritabanında kayıtlıysa benzersiz bir CVE kimliği bulunabilir.

Örneğin:

```
CVE-2024-XXXX
```

şeklindeki bir ifade belirli bir güvenlik açığını temsil eder.

SCA araçları kullanılan dependency'leri bilinen vulnerability kayıtlarıyla karşılaştırarak projede bulunan bileşenlerin bilinen CVE'lerden etkilenip etkilenmediğini belirlemeye çalışır.

### CVSS Nedir?

**CVSS (Common Vulnerability Scoring System)**, bir güvenlik açığının teknik ciddiyetini değerlendirmek için kullanılan puanlama sistemidir.

CVSS puanı genellikle güvenlik açıklarının önceliklendirilmesine yardımcı olur.

Genel olarak:

| CVSS Skoru | Genel Seviye |
| ---------- | ------------ |
| 0.0        | None         |
| 0.1 – 3.9  | Low          |
| 4.0 – 6.9  | Medium       |
| 7.0 – 8.9  | High         |
| 9.0 – 10.0 | Critical     |

Bir SCA taramasında örneğin bir dependency için **Critical** seviyesinde bir vulnerability bulunduğunda bu bulgu diğer düşük seviyeli bulgulara göre daha öncelikli ele alınabilir.

CVSS bir güvenlik açığının teknik ciddiyetini ifade eder; gerçek risk değerlendirmesinde uygulamanın bağlamı ve açığın sistem üzerindeki etkisi de dikkate alınmalıdır.

### SCA ve SBOM İlişkisi

**SBOM (Software Bill of Materials)**, bir yazılım ürününün hangi bileşenlerden oluştuğunu gösteren yazılım bileşen envanteridir.

SCA ile SBOM birbirinin yerine geçen kavramlar değildir.

SCA:

> "Kullandığım bileşenlerde bilinen bir güvenlik açığı var mı?"

sorusuna cevap vermeye odaklanırken,

SBOM:

> "Bu yazılım hangi bileşenlerden oluşuyor?"

sorusuna cevap veren bir envanter sağlar.

Örneğin bir SBOM içerisinde:

```
Application
 ├── Newtonsoft.Json 13.x
 ├── Serilog 4.x
 ├── Package X 2.x
 └── Package Y 1.x
```

gibi bileşenler listelenebilir.

SCA aracı bu bileşenleri vulnerability kaynaklarıyla karşılaştırarak güvenlik açıklarını tespit edebilir.

Bu nedenle SBOM, SCA süreçlerinde kullanılan önemli bir veri kaynağı ve yazılım tedarik zinciri görünürlüğü sağlayan bir yapı olarak değerlendirilebilir.

### SCA ve SAST Arasındaki Fark

<table data-search="false"><thead><tr><th>Özellik</th><th>SAST</th><th>SCA</th></tr></thead><tbody><tr><td>Açılım</td><td>Static Application Security Testing</td><td>Software Composition Analysis</td></tr><tr><td>Odak noktası</td><td>Kaynak kod</td><td>Dependencies / Components</td></tr><tr><td>Kendi yazdığımız kodu inceler mi?</td><td>Evet</td><td>Temel amacı değildir</td></tr><tr><td>Üçüncü parti paketleri analiz eder mi?</td><td>Temel amacı değildir</td><td>Evet</td></tr><tr><td>CVE tespiti</td><td>Temel amacı değildir</td><td>Evet</td></tr><tr><td>SBOM ile ilişkisi</td><td>Dolaylı</td><td>Güçlü</td></tr><tr><td>Örnek araç</td><td>Semgrep</td><td>Trivy</td></tr></tbody></table>

Örneğin Semgrep uygulamanın kendi kaynak kodunda güvenlik açısından problem oluşturabilecek bir kod yapısını tespit etmeye çalışırken, SCA aracı uygulamanın kullandığı bir dependency'nin bilinen bir CVE'ye sahip olup olmadığını araştırabilir.

### SCA ve DAST Arasındaki Fark

| Özellik                           | SCA             | DAST             |
| --------------------------------- | --------------- | ---------------- |
| Odak noktası                      | Dependencies    | Çalışan uygulama |
| Uygulamanın çalışması gerekir mi? | Hayır           | Evet             |
| Kaynak kod gerekir mi?            | Her zaman değil | Hayır            |
| CVE'li dependency bulma           | Evet            | Hayır            |
| Runtime davranışını test etme     | Hayır           | Evet             |
| Örnek araç                        | Trivy           | OWASP ZAP        |

Bu iki yöntem farklı güvenlik katmanlarını kontrol eder.

Örneğin bir uygulamada güvenlik açığı bulunan bir npm veya NuGet paketi kullanılıyorsa SCA bunu tespit edebilir. Ancak uygulamanın runtime sırasında XSS gibi bir güvenlik açığı üretip üretmediğini değerlendirmek için DAST kullanılabilir.

### SCA'nın Avantajları

* Üçüncü parti bağımlılıkların güvenlik açısından kontrol edilmesini sağlar.
* Bilinen CVE'lerin tespit edilmesine yardımcı olur.
* Transitive dependency'lerin görünür hale getirilmesini sağlar.
* Büyük projelerde dependency yönetimini kolaylaştırır.
* Yazılım tedarik zinciri güvenliğinin artırılmasına yardımcı olur.
* CI/CD pipeline'larına entegre edilebilir.
* SBOM süreçlerini destekleyebilir.
* Güvenlik açıklarının önem seviyesine göre önceliklendirilmesine yardımcı olur.

### SCA'nın Sınırlamaları

SCA'nın temel odağı kullanılan bileşenlerin ve bağımlılıkların analizidir. Bu nedenle uygulamanın kendi kaynak kodunda bulunan tüm güvenlik problemlerini tespit etmesi beklenmemelidir.

Örneğin uygulamanın kendi kodunda SQL Injection oluşturan bir kod bulunuyorsa bu problemin tespiti için SAST veya DAST gibi başka güvenlik testlerine ihtiyaç duyulabilir.

Ayrıca bir dependency'nin bilinen bir CVE'ye sahip olması her zaman uygulamanın doğrudan istismar edilebilir olduğu anlamına gelmez. Güvenlik açığının uygulamada gerçekten kullanılıp kullanılmadığı ve sistem üzerindeki etkisi ayrıca değerlendirilmelidir.

Bu nedenle SCA sonuçları otomatik olarak tek başına "uygulama güvensizdir" şeklinde yorumlanmamalıdır.

### SCA'nın DevSecOps İçerisindeki Yeri

SCA, yazılım geliştirme sürecinin erken aşamalarından itibaren uygulanabilir.

Örneğin CI/CD pipeline içerisinde dependency kontrolü gerçekleştirilebilir:

```
Kod
 ↓
Dependency Detection
 ↓
SCA Scan
 ↓
Vulnerability Analysis
 ↓
Severity Check
 ↓
Build / Deployment
```

Belirlenen güvenlik politikasına göre örneğin Critical seviyesinde bir vulnerability tespit edildiğinde build işleminin başarısız olması sağlanabilir.

Bu yaklaşım, güvenlik açığı bulunan bağımlılıkların production ortamına ulaşmadan önce tespit edilmesine yardımcı olur.

### SPM Sisteminde SCA

SPM sisteminde SCA, uygulamaların kullandığı bağımlılıkların güvenlik açısından kontrol edilmesi için kullanılabilir.

Mevcut mimaride ScannerService içerisine SCA yeteneği eklenmesi planlanabilir.

Genel yapı:

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

Bu yapı sayesinde farklı güvenlik analizleri tek bir ScannerService üzerinden gerçekleştirilebilir.

Bir sonraki bölümde **Trivy** incelenerek SCA ve vulnerability scanning kapsamında nasıl kullanılabileceği açıklanacaktır.
