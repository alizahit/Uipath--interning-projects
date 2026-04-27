# 🌍 Global Pazar Araştırması ve Fizibilite Otomasyonu

![UiPath](https://img.shields.io/badge/UiPath-Advanced-blue?style=for-the-badge&logo=uipath)
![API](https://img.shields.io/badge/API-Integration-green?style=for-the-badge&logo=postman)
![AI](https://img.shields.io/badge/AI-Gemini_Flash-orange?style=for-the-badge&logo=google-gemini)

## 📖 Proje Özeti
Bu proje, global bir perakende şirketinin yeni pazar giriş stratejilerini desteklemek amacıyla geliştirilmiş bir **UiPath** otomasyonudur. Otomasyon; demografik verileri ve seyahat maliyetlerini otomatik olarak toplar, ardından bir **Yapay Zeka (LLM)** kullanarak bu pazarların potansiyelini puanlar.

## 🎓 Öğrenme Hedeflerim
Bu projeyi geliştirirken aşağıdaki teknik yetkinlikleri kazanmayı ve süreçlerime entegre etmeyi hedefledim:
* **🌐 REST API & HTTP Request:** Uygulamalar arası veri transferi için HTTP protokollerini kullanmayı ve JSON yanıtlarını ayrıştırmayı öğrenmek. 
* **🤖 AI (LLM) Integration:** RPA süreçlerine yapay zeka modellerini (Gemini) entegre ederek "karar verme" mekanizmalarını otomatize etmek. 
* **📊 Dynamic Web Automation:** Farklı web sitelerinden (Wikipedia, Skyscanner) dinamik ve karmaşık veri çekme (scraping) yöntemlerini kavramak. 

---

## 🛠️ Modüller ve İşlem Akışı

### 🔍 1. CountryDataExtract.xaml (Veri Toplama)
* Vikipedi üzerinden güncel ülke, başkent ve nüfus verilerini çeker.
* İlgili lokasyonları filtreleyerek ana veri kümesini oluşturur. 

### 📝 2. createExcel.xaml (Rapor Hazırlığı)
* Süreç sonunda üretilecek olan `FizibiliteRaporu.xlsx` dosyasını ve gerekli sütun başlıklarını oluşturur. 

### 🧠 3. AiPerformer.xaml (Yapay Zeka Değerlendirmesi)
* Toplanan verileri **Gemini 2.5 Flash Lite** modeline HTTP isteği ile gönderir. 
* Yapay zekanın pazar için verdiği puanı (1-10) alarak fizibilite raporunu tamamlar.

### ✈️ 4. flyTicket.xaml (Maliyet Analizi)
* Skyscanner üzerinden belirlenen tarih ve kişi sayısına göre uçuş araması yapar. 
* "En ucuz" bilet fiyatını **Regex** kullanarak çeker ve toplam maliyeti hesaplayarak rapora işler. 

---

## 🚀 Kurulum Notları
* `Data/Config.xlsx` dosyasındaki URL ve tarih parametrelerini güncelleyin. 
* `AiPerformer.xaml` içindeki HTTP Request kısmına kendi Gemini API anahtarınızı eklemeyi unutmayın.
* Süreci `Main.xaml` üzerinden başlatarak sonuçları `Data/Output` klasöründe görüntüleyebilirsiniz.
