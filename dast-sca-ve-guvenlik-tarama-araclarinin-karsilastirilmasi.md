# DAST, SCA ve Güvenlik Tarama Araçlarının Karşılaştırılması

### Genel Bakış

DAST ve SCA, yazılım güvenliğinin farklı katmanlarını kontrol eden iki farklı güvenlik yaklaşımıdır.

**DAST**, uygulama çalışırken ortaya çıkabilecek güvenlik problemlerini tespit etmeye odaklanırken, **SCA** uygulamada kullanılan üçüncü parti bileşenlerin ve bağımlılıkların güvenlik durumunu analiz eder.

Bu yaklaşımları uygulamak için farklı araçlardan yararlanılabilir. Bu çalışmada incelenen temel araçlar **OWASP ZAP** ve **Trivy**'dir.

### SAST, SCA ve DAST Karşılaştırması

<table data-search="false"><thead><tr><th>Özellik</th><th>SAST</th><th>SCA</th><th>DAST</th></tr></thead><tbody><tr><td>Açılım</td><td>Static Application Security Testing</td><td>Software Composition Analysis</td><td>Dynamic Application Security Testing</td></tr><tr><td>Temel odak</td><td>Kaynak kod</td><td>Dependencies / Components</td><td>Çalışan uygulama</td></tr><tr><td>Uygulamanın çalışması gerekir mi?</td><td>Hayır</td><td>Hayır</td><td>Evet</td></tr><tr><td>Kaynak koduna ihtiyaç</td><td>Genellikle evet</td><td>Her zaman değil</td><td>Hayır</td></tr><tr><td>Üçüncü parti dependency analizi</td><td>Temel amacı değil</td><td>Evet</td><td>Hayır</td></tr><tr><td>Runtime testi</td><td>Hayır</td><td>Hayır</td><td>Evet</td></tr><tr><td>CVE tespiti</td><td>Temel amacı değil</td><td>Evet</td><td>Temel amacı değil</td></tr><tr><td>Örnek araç</td><td>Semgrep</td><td>Trivy</td><td>OWASP ZAP</td></tr></tbody></table>

Bu üç yaklaşım farklı güvenlik problemlerini hedeflediği için birbirlerinin alternatifi değildir.

Bir uygulamanın güvenliğini daha kapsamlı değerlendirebilmek için bu yöntemlerin birlikte kullanılması daha etkili bir yaklaşım sağlar.

### Semgrep, Trivy ve ZAP Karşılaştırması

| Araç          | Kategori                    | İncelediği Alan                        | Örnek Kullanım                           |
| ------------- | --------------------------- | -------------------------------------- | ---------------------------------------- |
| **Semgrep**   | SAST                        | Kaynak kod                             | Güvensiz kod kalıplarının tespiti        |
| **Trivy**     | SCA + Vulnerability Scanner | Dependency, image, repository, IaC vb. | CVE ve dependency taraması               |
| **OWASP ZAP** | DAST                        | Çalışan web uygulaması / API           | XSS, SQL Injection vb. güvenlik testleri |

#### Semgrep

Semgrep kaynak kod üzerinde analiz gerçekleştirir.

Örneğin geliştiricinin yazdığı kod içerisinde güvenli olmayan bir fonksiyon kullanımı veya belirli bir güvenlik açığına yol açabilecek kod kalıbı bulunuyorsa Semgrep bunu tespit edebilir.

#### Trivy

Trivy daha çok kullanılan bileşenlerin ve çeşitli deployment/infrastructure hedeflerinin güvenlik açısından incelenmesine odaklanır.

Örneğin bir dependency'nin bilinen CVE'si bulunuyorsa veya bir container image içerisinde güvenlik açığı bulunan bir paket varsa Trivy bunu tespit edebilir.

Bunun yanında IaC, secret, license ve SBOM gibi farklı alanlarda da kullanılabilir.

#### OWASP ZAP

ZAP çalışan uygulama üzerinde dinamik güvenlik testleri gerçekleştirir.

Örneğin uygulamanın belirli bir endpoint'ine özel test girdileri göndererek XSS veya SQL Injection gibi runtime sırasında ortaya çıkabilecek güvenlik problemlerini araştırabilir.

### Aynı Uygulamada Üç Aracın Kullanılması

Bir uygulama için örnek bir güvenlik kontrolü şu şekilde gerçekleştirilebilir:

```
                         Uygulama
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
           Semgrep        Trivy          ZAP
              │             │             │
             SAST          SCA           DAST
              │             │             │
              ▼             ▼             ▼
          Kaynak kod     Dependencies   Runtime
                         / Image / IaC   davranışı
              │             │             │
              └─────────────┼─────────────┘
                            ▼
                     Güvenlik Raporu
```

Bu yapı sayesinde uygulamanın farklı katmanları ayrı ayrı kontrol edilebilir.

Örneğin aynı proje üzerinde:

* Semgrep kaynak kodunda bir güvenlik problemi bulabilir.
* Trivy kullanılan bir dependency'nin CVE'ye sahip olduğunu tespit edebilir.
* ZAP çalışan API üzerinde bir güvenlik açığı tespit edebilir.

Bu bulguların tamamı farklı güvenlik katmanlarına ait olsa da aynı uygulamanın genel güvenlik durumunu değerlendirmeye katkı sağlar.

### Hangi Durumda Hangi Araç Kullanılmalı?

<table data-search="false"><thead><tr><th>İhtiyaç</th><th>Kullanılabilecek yaklaşım / araç</th></tr></thead><tbody><tr><td>Kaynak kod güvenliği</td><td>SAST / Semgrep</td></tr><tr><td>Dependency güvenliği</td><td>SCA / Trivy</td></tr><tr><td>Container image güvenliği</td><td>Trivy</td></tr><tr><td>IaC güvenliği</td><td>Trivy</td></tr><tr><td>Secret tespiti</td><td>Trivy</td></tr><tr><td>SBOM oluşturma</td><td>Trivy</td></tr><tr><td>Çalışan web uygulamasının güvenliği</td><td>DAST / OWASP ZAP</td></tr><tr><td>API güvenlik testi</td><td>OWASP ZAP</td></tr><tr><td>Runtime güvenlik problemleri</td><td>DAST / OWASP ZAP</td></tr></tbody></table>

Buradaki seçim uygulamanın ihtiyaçlarına göre yapılmalıdır. Örneğin yalnızca dependency güvenliğinin kontrol edilmesi gerekiyorsa Trivy'nin SCA özelliklerinden yararlanılabilir. Çalışan bir web uygulamasının güvenlik testine ihtiyaç varsa ZAP gibi bir DAST aracına ihtiyaç duyulur.

### Sonuç

DAST, SCA ve SAST farklı güvenlik problemlerini hedefleyen tamamlayıcı güvenlik yaklaşımlarıdır.

**SAST**, geliştirilen kaynak kodunu analiz eder.

**SCA**, uygulamada kullanılan üçüncü parti bileşenleri ve dependency'leri analiz eder.

**DAST**, çalışan uygulamayı dışarıdan test eder.

Bu nedenle güvenli bir yazılım geliştirme sürecinde yalnızca tek bir yönteme güvenmek yerine farklı güvenlik analizlerinin birlikte kullanılması daha kapsamlı bir güvenlik kontrolü sağlar.
