# 15_Buton_Basma_Suresi_Kadar_LED_Yakma (Time Measurement)

Bu proje, **STM32F407-Discovery** kartı üzerinde butona ne kadar süre basıldığını ölçen ve bu süreyi hafızasına alıp, LED'i o süre kadar tekrar yakan bir "Kayıt/Oynat" uygulamasıdır.

Bu depo, `HAL_Delay` (kör bekleme) yerine `HAL_GetTick()` fonksiyonunun kullanılarak **geçen sürenin (elapsed time)** nasıl hesaplanacağını gösterir.

> **🔗 Yeni Kavram: `HAL_GetTick()`**
>
> Önceki projelerde zamanlama için sadece `HAL_Delay()` kullandık.
> * `HAL_GetTick()`: Mikrodenetleyiciye enerji verildiği andan itibaren geçen süreyi **milisaniye (ms)** cinsinden verir (Sistem saati).
> * **Süre Hesabı:** `Bitiş Zamanı - Başlangıç Zamanı = Geçen Süre` formülü ile yapılır.

---

### 🎯 Proje Senaryosu

Sistem bir döngü içinde şu adımları izler:

1.  **Kayıt Başlangıcı (Butona Basıldı):**
    * LED yanar (Geri bildirim için).
    * Sistem o anki zamanı (`baslangic`) kaydeder.
2.  **Kayıt Süreci (Basılı Tutuluyor):**
    * Kod, kullanıcı elini butondan çekene kadar `while` döngüsü içinde bekler.
3.  **Kayıt Bitişi (Buton Bırakıldı):**
    * LED söner.
    * Şu anki zamandan başlangıç zamanı çıkarılarak **toplam basılma süresi** (`basilan_sure`) hesaplanır.
4.  **Bekleme:**
    * Karışıklığı önlemek için sistem 3 saniye boyunca hiçbir şey yapmadan bekler (LED sönük).
5.  **Oynatma (Playback):**
    * LED tekrar yanar.
    * Hesaplanan `basilan_sure` kadar yanık kalır (Yani butona ne kadar bastıysanız, LED o kadar süre yanar).
    * Süre bitince söner.

---

### ⚙️ Pull-Up Konfigürasyonu

Projenin düzgün çalışması için `.ioc` dosyasında buton pininin (`PA0`) **Pull-Up** olarak ayarlanması gereklidir.

* **Pin:** `PA0` -> `GPIO_Input`
* **Resistor:** `Pull-up`

<img width="843" height="644" alt="image" src="https://github.com/user-attachments/assets/a5bccc60-b813-4f18-9e9a-a4f0fd3519bf" />
---

### 🛠️ Gerekli Donanım

* **1x** STM32F407-Discovery Geliştirme Kartı
* **1x** LED
* **1x** 220 Ohm Direnç
* **1x** Push-Button
* **Breadboard ve Jumper Kablolar**

---

### 🔌 Devre Şeması

Buton bağlantısı **Pull-Up** mantığına göre (GND'ye) yapılmalıdır.

| Bileşen | STM32 Pini | Bağlantı Detayı |
| :--- | :--- | :--- |
| **Buton** | `PA0` | Bir bacak **PA0**, diğer bacak **GND** |
| **LED** | `PA1` | Anot -> **PA1**, Katot -> Direnç -> **GND** |

<img width="346" height="480" alt="image" src="https://github.com/user-attachments/assets/5b2998e0-3e4e-4f1a-84cc-8264f9fee38a" />


---

### 💻 Kod Bloğu

<img width="1408" height="796" alt="image" src="https://github.com/user-attachments/assets/6dfb6a3b-ad60-4182-a55c-b4da84bd33c6" />
