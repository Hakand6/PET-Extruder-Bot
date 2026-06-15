 PET-Extruder-Bot: Akıllı PET Şişeden Filament Dönüştürücü

**PET-Extruder-Bot**, atık PET şişeleri hassas sıcaklık ve hız kontrolü ile 3B yazıcılar için yüksek kaliteli geri dönüştürülmüş filamente (rPET) dönüştüren, Arduino tabanlı bir otomasyon sistemidir. 

Bu proje; işlemci performansını optimize ederek **step motor hızını düşürmeden** eşzamanlı olarak LCD ekran güncelleme, NTC ile sıcaklık okuma, MOSFET tabanlı ısıtıcı kontrolü ve donanımsal güvenlik switchleri yönetimini gerçekleştiren gerçek zamanlı bir gömülü yazılım mimarisine sahiptir.

---

 Projenin Amacı (Project Purpose)

Bu projenin temel amacı; tüketim sonrası çevreye yük oluşturan **atık PET (Polietilen Tereftalat) şişeleri**, ekstrüzyon (eriyik filament imalatı) yöntemiyle yeniden işleyerek 3B yazıcılarlarda doğrudan kullanılabilir **yüksek kaliteli geri dönüştürülmüş filamete (rPET)** dönüştürmektir. 

Proje, hem çevresel sürdürülebilirliğe katkı sağlamayı hem de 3B yazıcı kullanıcıları için maliyeti sıfıra yakın bir ham madde üretim ekosistemi geliştirmeyi hedefler.

### Teknik ve Mühendislik Odaklı Amaçlar:
* **Asenkron İşlem Yönetimi:** Gömülü sistemlerde işlemciyi en çok yoran LCD ekran güncelleme ve analog sensör okuma gecikmelerini (`delay`), zamanlayıcı tabanlı (asenkron) mimariyle optimize ederek step motorun mikrosaniyelik adımlarını kesintisiz ve kararlı bir şekilde yürütmek.
* **Hassas Termal Kontrol:** Isı dalgalanmalarına karşı duyarlı olan PET malzemeyi, MOSFET ve NTC termistör geri beslemeli entre kontrol döngüsüyle tam **200°C**'de sabit tutarak erime kararlılığını sağlamak ve tıkanmaları önlemek.
* **Gerçek Zamanlı Telemetri ve Güvenlik:** Üretim esnasında kopma veya ham madde bitmesi gibi mekanik aksaklıkları donanımsal switchler vasıtasıyla anlık tespit edip sistemi güvenli moda almak; eşzamanlı olarak üretilen filamentin miktarını (metre/gram) gerçek zamanlı hesaplayarak kullanıcıya sunmak.

---

 Özellikler

* **Yüksek Hızlı ve Kararlı Motor Sürücü:** Sadece motor sürme kodlarındaki mikrosaniyelik (`150us`) tepki süresini koruyan, zamanlayıcı (timer/millis) tabanlı kesintisiz adım tetikleme mimarisi.
* **Hassas Sıcaklık Stabilizasyonu:** 100K NTC termistör vasıtasıyla okunan verileri dalgalanmaları önlemek adına filtreleyen ve MOSFET üzerinden hotend'i **200°C**'de sabit tutan kontrol döngüsü.
* **Akıllı Üretim Takibi (Kalibrasyonlu):** Gerçek üretim verilerine göre kalibre edilmiş (**33.5 dakikada 1.92 metre**), anlık hıza duyarlı dinamik metre ve gramaj hesaplayıcı (`1 metre = ~3.04 gram`).
* **Donanımsal Güvenlik ve Otomasyon:** * **Sistem Anahtarı (D7):** Tüm sistemi tek hamlede güvenli moda alan ana şalter kontrolü.
  * **Makaralı Filament Switch (D2):** Filament koptuğunda veya bittiğinde motoru milisaniyeler içinde kilitleyen "Hard Stop" mekanizması (Switch basılıyken sistem aktif, serbest kaldığında durur).
* **Anlık Telemetri Ekranı:** I2C LCD (16x2) üzerinden gerçek zamanlı sıcaklık, dinamik RPM (Devir/Dakika), üretilen toplam metre ve gramaj gösterimi.

---
 Donanım Bileşenleri

* **Mikrokontrolcü:** Arduino Nano / Uno
* **Motor & Sürücü:** NEMA 17 Step Motor & A4988 Sürücü (Full-Step Modu)
* **Isıtıcı Güç Bloğu:** IRFZ44N MOSFET & 12V Hotend (Isıtıcı Fişek)
* **Sensörler:** 100K NTC Termistör (Beta: 3950) & Makaralı Limit Switch
* **Arayüz & Kontrol:** I2C 16x2 LCD Ekran & 10K Potansiyometre
* **Güç Kaynağı:** 12V Güç Trafosu

---

 Devre Bağlantı Şeması

| Bileşen | Arduino Pini | Açıklama |
| :--- | :--- | :--- |
| **A4988 STEP** | D4 | Motor Adım Sinyali |
| **A4988 DIR** | D5 | Motor Yön Sinyali |
| **MOSFET Gate** | D11 | PWM Isıtıcı Tetikleme |
| **Limit Switch**| D2 | Filament Algılama (INPUT_PULLUP - NO/COM bağlantısı ile basılıyken çalışır) |
| **Sistem Switch**| D7 | Ana Şalter (INPUT_PULLUP - NC/COM bağlantısı ile kapalıyken çalışır) |
| **Potansiyometre**| A0 | Hız Ayarı Girişi |
| **NTC Termistör**| A1 | Sıcaklık Sensörü Girişi |
| **I2C LCD SDA** | A4 | Ekran Veri Hattı |
| **I2C LCD SCL** | A5 | Ekran Saat Sinyali |



---

 Kurulum ve Çalıştırma

1. Bu depoyu klonlayın:
   ```bash
   git clone [https://github.com/kullanici_adin/PET-Extruder-Bot.git](https://github.com/kullanici_adin/PET-Extruder-Bot.git)
