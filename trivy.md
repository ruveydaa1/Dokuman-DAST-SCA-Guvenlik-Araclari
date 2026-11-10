# TRİVY

### Trivy Nedir?

**Trivy**, açık kaynaklı bir güvenlik tarama aracıdır. Yazılım projelerinde, container image'larında, dosya sistemlerinde, Git repository'lerinde ve farklı altyapı yapılandırmalarında güvenlik problemlerinin tespit edilmesi amacıyla kullanılabilir.

Trivy'nin önemli özelliklerinden biri, tek bir güvenlik alanına odaklanmak yerine farklı tarama türlerini desteklemesidir.

Trivy ile temel olarak:

* Vulnerability scanning
* Dependency scanning
* Container image scanning
* Filesystem scanning
* Git repository scanning
* IaC misconfiguration scanning
* Secret scanning
* License scanning
* SBOM oluşturma ve SBOM tarama

gibi işlemler gerçekleştirilebilir.

Bu nedenle Trivy'yi yalnızca bir **SCA aracı** olarak tanımlamak yerine, **SCA dahil olmak üzere farklı güvenlik taramalarını gerçekleştirebilen kapsamlı bir güvenlik tarama aracı** olarak değerlendirmek daha doğrudur.

### Trivy'nin SCA ile İlişkisi

Trivy'nin SCA kapsamında kullanılmasının temel amacı, projede kullanılan yazılım bileşenlerinin bilinen güvenlik açıklarına karşı kontrol edilmesidir.

Örneğin bir projede:

```
Application
 ├── Package A
 ├── Package B
 └── Package C
```

şeklinde bağımlılıklar bulunuyorsa Trivy bu bileşenleri analiz ederek bilinen vulnerability kayıtlarıyla karşılaştırabilir.

Eğer kullanılan bir dependency'nin güvenlik açığı bulunuyorsa Trivy bunu bulgu olarak raporlayabilir.

Örneğin sonuçlarda genel olarak:

```
Package       Version     Severity    Vulnerability
-----------------------------------------------------
Package-A     1.2.3       HIGH        CVE-XXXX-XXXX
Package-B     4.5.6       MEDIUM      CVE-XXXX-XXXX
```

gibi bilgiler görülebilir.

Bu özellik nedeniyle Trivy, SCA süreçlerinde kullanılabilecek araçlardan biridir.

### Trivy Vulnerability Scanning

Trivy'nin temel özelliklerinden biri **vulnerability scanning** özelliğidir.

Vulnerability scanning ile işletim sistemi paketlerinde veya uygulamanın kullandığı yazılım bileşenlerinde bilinen güvenlik açıkları araştırılabilir.

Trivy'nin desteklediği hedef ve ekosisteme göre farklı paket yöneticileri ve programlama dili bağımlılıkları analiz edilebilir.

Örneğin bir container image içerisinde bulunan işletim sistemi paketleri veya uygulama dependency'leri vulnerability açısından incelenebilir.

Temel bir container image taraması:

```bash
trivy image nginx:latest
```

şeklinde gerçekleştirilebilir.

Bu komut `nginx:latest` container image'ını vulnerability açısından tarar.

### Filesystem Scanning

Trivy yalnızca container image'larını değil, bir dosya sistemini veya proje klasörünü de tarayabilir.

Bir projenin bulunduğu klasörde:

```bash
trivy fs .
```

komutu kullanılabilir.

Bu tarama sırasında proje içerisinde bulunan dependency bilgileri incelenerek bilinen güvenlik açıkları araştırılabilir.

Bu özellik özellikle kaynak kodunun bulunduğu repository'lerin güvenlik açısından kontrol edilmesinde kullanılabilir.

### Git Repository Scanning

Trivy Git repository'lerini de tarayabilir.

Bu sayede repository içerisinde bulunan:

* Dependency vulnerability'leri
* Secret'lar
* Misconfiguration'lar
* Diğer desteklenen güvenlik problemleri

kontrol edilebilir.

Örneğin uzaktaki bir repository:

```bash
trivy repo https://github.com/example/project
```

şeklinde taranabilir.

Bu özellik SPM gibi repository tabanlı güvenlik taraması gerçekleştiren sistemler açısından önemlidir.

### Container Image Scanning

Container teknolojilerinin yaygınlaşmasıyla birlikte Docker image'larının güvenliği önemli hale gelmiştir.

Bir container image içerisinde:

* İşletim sistemi paketleri
* Programlama dili paketleri
* Üçüncü parti kütüphaneler
* Uygulama dosyaları

bulunabilir.

Bu bileşenlerden herhangi birinin bilinen güvenlik açığı bulunması container image'ını riske sokabilir.

Trivy ile image taranarak bu vulnerability'ler tespit edilebilir.

Örneğin:

```bash
trivy image myapp:latest
```

komutu ile `myapp:latest` image'ı taranabilir.

Bu özellik Trivy'nin SCA kapsamının ötesine geçen önemli yeteneklerinden biridir.

### IaC Misconfiguration Scanning

Trivy yalnızca vulnerability taraması yapmaz. **Infrastructure as Code (IaC)** dosyalarında bulunan güvenlik yapılandırma problemlerini de tespit edebilir.

Örneğin:

* Terraform
* Kubernetes
* Dockerfile
* Cloud yapılandırmaları

gibi dosyalarda güvenlik açısından hatalı yapılandırmalar bulunabilir.

Bir klasördeki IaC dosyalarını taramak için:

```bash
trivy config .
```

komutu kullanılabilir.

Bu sayede yalnızca kullanılan dependency'ler değil, uygulamanın altyapısının güvenli şekilde yapılandırılıp yapılandırılmadığı da kontrol edilebilir.

### Secret Scanning

Kaynak kod veya repository içerisinde yanlışlıkla:

* API key
* Access token
* Password
* Private key
* Diğer gizli bilgiler

bulundurulması ciddi güvenlik riskleri oluşturabilir.

Trivy secret scanning özelliği ile kaynaklarda bulunabilecek gizli bilgilerin tespit edilmesine yardımcı olabilir.

Örneğin bir repository içerisinde yanlışlıkla:

```
API_KEY=xxxxxxxxxxxxxxxx
```

gibi bir değer commit edilmişse secret scanning bunu tespit edebilir.

Bu özellik SCA'nın doğrudan bir parçası değildir ancak Trivy'nin kapsamının neden yalnızca SCA ile sınırlı olmadığını gösteren özelliklerden biridir.

### License Scanning

Açık kaynaklı paketler farklı lisanslarla dağıtılabilir.

Bazı projelerde kullanılan açık kaynak bileşenlerinin lisanslarının kontrol edilmesi de önemli olabilir.

Trivy, desteklediği hedeflerde kullanılan bileşenlerin lisans bilgilerini inceleyerek lisans uyumluluğu ve politika kontrollerine yardımcı olabilir.

Bu özellik özellikle kurumsal projelerde kullanılan üçüncü parti bileşenlerin yönetimi açısından önemlidir.

### SBOM Oluşturma

Trivy'nin önemli özelliklerinden biri de **SBOM (Software Bill of Materials)** oluşturabilmesidir.

SBOM, bir yazılımın hangi bileşenlerden oluştuğunu gösteren bir envanterdir.

Trivy kullanılarak SBOM farklı standartlarda oluşturulabilir.

Örneğin CycloneDX formatında bir SBOM oluşturulabilir:

```bash
trivy image --format cyclonedx --output sbom.json myapp:latest
```

Bu SBOM daha sonra yazılım bileşenlerinin takip edilmesi ve güvenlik analizlerinde kullanılabilir.

SBOM sayesinde bir uygulamada hangi dependency'lerin veya paketlerin bulunduğu daha görünür hale gelir.

### SBOM ile Vulnerability Scanning

Trivy yalnızca SBOM oluşturmakla kalmaz, SBOM üzerinden vulnerability taraması yapılmasına da olanak sağlayabilir.

Genel süreç:

```
Application / Container
          │
          ▼
     SBOM Creation
          │
          ▼
   Component Inventory
          │
          ▼
 Vulnerability Analysis
          │
          ▼
      Scan Results
```

Bu yapı özellikle yazılım tedarik zincirinin izlenmesi açısından önemlidir.

### Trivy Çıktıları

Trivy tarama sonuçlarında genel olarak:

* Package adı
* Installed version
* Fixed version
* Vulnerability ID
* Severity
* Vulnerability açıklaması

gibi bilgiler bulunabilir.

Örneğin:

```
Package        Installed    Fixed       Severity
-------------------------------------------------
Package-A      1.0.0        1.0.2       HIGH
Package-B      2.1.0        2.2.0       CRITICAL
```

şeklinde bir sonuç elde edilebilir.

**Fixed Version** bilgisi mevcutsa geliştirici hangi sürüme yükseltme yaparak ilgili vulnerability'nin giderilebileceği konusunda fikir sahibi olabilir.

### Trivy'nin Avantajları

* Açık kaynaklıdır.
* Kullanımı kolay CLI sunar.
* Container image'larını tarayabilir.
* Dependency vulnerability'lerini tespit edebilir.
* Repository ve filesystem taraması yapabilir.
* IaC misconfiguration'larını kontrol edebilir.
* Secret scanning yapabilir.
* SBOM oluşturabilir.
* SBOM üzerinden vulnerability taraması gerçekleştirebilir.
* CI/CD süreçlerine entegre edilebilir.
* Farklı güvenlik kontrollerini tek araç altında birleştirebilir.

### Trivy'nin Sınırlamaları

Trivy geniş bir tarama kapsamına sahip olsa da tek başına tüm güvenlik problemlerini tespit etmesi beklenmemelidir.

Örneğin uygulamanın kendi kaynak kodunda bulunan karmaşık bir authorization problemi için SAST veya manuel güvenlik testi gerekebilir.

Benzer şekilde çalışan bir web uygulamasındaki runtime zafiyetlerinin tespit edilmesi için DAST araçları kullanılmalıdır.

Bu nedenle Trivy'nin SPM içerisinde Semgrep veya ZAP'ın yerine geçmesi yerine, bu araçları tamamlayan bir güvenlik tarama bileşeni olarak değerlendirilmesi daha doğru olacaktır.

### Trivy ve Diğer Araçların Farkı

| Araç          | Tür                                       | Temel Odak                               |
| ------------- | ----------------------------------------- | ---------------------------------------- |
| **Semgrep**   | SAST                                      | Kaynak kod analizi                       |
| **OWASP ZAP** | DAST                                      | Çalışan web uygulaması                   |
| **Trivy**     | SCA + Vulnerability / IaC / Secret / SBOM | Dependency, image, repository ve altyapı |
| **SBOM**      | Envanter                                  | Yazılım bileşenlerinin listelenmesi      |

Burada önemli olan nokta, bu kavramların birbirinin alternatifi olmamasıdır.

Semgrep kaynak kodunu analiz eder.

ZAP çalışan uygulamayı test eder.

Trivy ise kullanılan bileşenleri ve desteklediği diğer hedefleri güvenlik açısından tarar.

SBOM ise yazılımın hangi bileşenlerden oluştuğunu gösteren bir envanter sağlar.

### Trivy'nin SPM Sistemindeki Yeri

Mevcut SPM mimarisinde Trivy, ScannerService içerisinde ayrı bir tarama motoru olarak konumlandırılabilir.

Önerilen yapı:

```
                     UserManagement
                           │
                           │ Kafka
                           ▼
                    ┌───────────────┐
                    │ ScannerService │
                    └───────┬───────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
          Semgrep         Trivy          ZAP
             │              │              │
            SAST        SCA / Vuln        DAST
                         / IaC /
                         Secret /
                          SBOM
```

Bu yapıda ScannerService tarama işlemlerinin merkezi noktası olarak görev yaparken, her araç kendi sorumlu olduğu güvenlik analizini gerçekleştirir.

Örneğin:

**Semgrep**\
→ Kaynak kodundaki güvenlik problemlerini araştırır.

**Trivy**\
→ Dependency, container, repository, IaC, secret ve SBOM gibi alanlarda tarama gerçekleştirir.

**OWASP ZAP**\
→ Çalışan web uygulamasını ve API'leri DAST kapsamında test eder.

Bu yaklaşım SPM sisteminin farklı güvenlik katmanlarını tek bir mimari altında birleştirmesine olanak sağlar.
