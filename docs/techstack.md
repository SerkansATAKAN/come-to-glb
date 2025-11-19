# 🏗️ GLB Ekosistemi – Teknoloji Yığını & Mimari

![Project Status](https://img.shields.io/badge/Status-Live-success)
![Platform](https://img.shields.io/badge/Platform-Web%20%7C%20Mobile%20%7C%20Desktop-blue)
![Workflow](https://img.shields.io/badge/Workflow-AI%20Native-purple)

**Portfolyom (GLB);** mobilya & iç mimari odaklı 3D modelleri, üretim planlama ihtiyaçlarını (nesting, teknik çizim) ve yapay zekâ destekli içerik üretimini tek çatı altında toplayan çok katmanlı hibrit bir projedir.

Bu dokümantasyon, projenin AI destekli geliştirme sürecini, mevcut teknik yapısını ve gelecekteki ticari dönüşüm hedeflerini açıklar:

1.  **Geliştirme Metodolojisi:** AI-Native İş Akışı
2.  **Web Platformu:** GLB 3D Model Platformu
3.  **AI Stüdyosu:** Prompt & Otomasyon
4.  **Desktop Tools:** Offline 3D Dönüştürücü
5.  **Gelecek Vizyonu:** Mühendislik Veri Pazarı & Nesting

---

## 1. Geliştirme Metodolojisi (AI-Native Workflow)

Proje, klasik kodlama yerine modern yapay zeka araçlarının orkestrasyonu ile geliştirilmektedir.

* **Prototipleme & UI:** Mobil altyapı, giriş ekranları ve temel yerleşimler **Bolt.new, Lovable, v0** veya **Dualite** kullanılarak hızla oluşturulur.
* **Versiyonlama:** Oluşturulan kod tabanı GitHub'a aktarılır.
* **Geliştirme & Derinleştirme:** Proje **Cursor, Trae** veya **Qoder** gibi AI destekli IDE'ler ile işlenerek backend bağlantıları ve karmaşık mantıklar eklenir.

---

## 2. Web Platformu (GLB – 3D Model Gösterim)

Mobilya ve koltuk odaklı 3D modellerin yönetildiği, interaktif olarak önizlendiği ve mühendislik verilerinin sunulduğu web katmanıdır.

### 2.1. Frontend & Tasarım Teknolojileri
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white) ![Stitch](https://img.shields.io/badge/Design-Stitch-pink) ![Three.js](https://img.shields.io/badge/Three.js-black?style=flat&logo=three.js&logoColor=white)

* **Stitch (Design System):** Tasarım tutarlılığı ve UI bileşenleri için Stitch altyapısı kullanılır.
* **Vanilla HTML/CSS/JS:** Framework bağımlılığı olmayan (No-Framework), hafif yapı. (Bolt/Lovable çıktıları optimize edilerek kullanılır).
    * Modüler sayfa yapısı: `index.html`, `models.html`, `admin.html`.
    * Responsive tasarım: CSS Grid ve Flexbox ile mobil uyumlu kart yapıları.
    * Optimizasyon: `srcset` + `sizes` ile cihaz çözünürlüğüne göre (480px/960px) WebP thumbnail sunumu.
* **Three.js (WebGL):**
    * `.glb` / `.gltf` formatındaki sıkıştırılmış 3D varlıkların tarayıcıda render edilmesi.
    * OrbitControls ile zoom, pan ve döndürme yetenekleri.
    * PBR materyal ve ışıklandırma ayarları ile gerçekçi ürün sunumu.

### 2.2. Backend & Infrastructure (Firebase)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=firebase&logoColor=black)

* **Firebase Hosting:** `firebase.json` üzerinden statik varlıklar ve agresif cache yönetimi.
* **Firebase Authentication:**
    * Bolt/Lovable ile entegre edilmiş giriş altyapısı.
    * Role-Based Access Control (RBAC): Admin ve Kullanıcı ayrımı.
    * Guarded Routes: Kritik sayfalara yetkisiz erişimin engellenmesi.
* **Cloud Firestore (NoSQL):**
    * Model Metadata Yönetimi: Boyutlar (X,Y,Z), kategori ağacı.
    * Admin panelinden CRUD operasyonları.
* **Firebase Storage:**
    * `models/`: Optimize edilmiş GLB dosyaları.
    * `thumbnails/`: WebP formatında kapak görselleri.

### 2.3. Admin Paneli Özellikleri
* **Batch Upload:** Klasör seçimi ile toplu model yükleme.
* **Auto-Sanitization:** Dosya isimlerinden Türkçe karakter ve boşlukların otomatik temizlenmesi.
* **Deep Clean:** Bir model silindiğinde hem Storage'dan dosyanın hem de Firestore'dan kaydın eş zamanlı temizlenmesi.

---

## 3. AI Prompt & Otomasyon Stüdyosu

Görsel ve video üretim süreçleri için "insan doğruluklu" promptlar üreten otomasyon katmanıdır.

### 3.1. Arayüz & Veri Girişi
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=flat&logo=google-sheets&logoColor=white) ![Apps Script](https://img.shields.io/badge/Apps%20Script-4285F4?style=flat&logo=google&logoColor=white)

* **Google Sheets:** Stil kütüphanesi, kamera hareketleri ve renk paletleri için master veritabanı.
* **Google Apps Script (Web App):** Dinamik form arayüzü ile parametrelerin toplanması ve JSON payload oluşturulması.

### 3.2. Backend Logic & AI Entegrasyonu
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=flat&logo=n8n&logoColor=white) ![Gemini](https://img.shields.io/badge/AI-Google%20Gemini-4285F4)

* **Google Gemini API:**
    * Kullanıcıdan gelen ham isteklerin anlamlandırılması.
    * Sahne ve atmosfer betimlemelerinin zenginleştirilmesi.
* **n8n İş Akışı:**
    Sistem, gelen isteği analiz eder ve iki ana dala ayırır:
    1.  **Video Dalı (`_mode == "video"`):**
        * Sinematik doğal dil anlatımı + JSON Timeline (`camera_action`, `lighting`).
        * *Hedef:* Google Veo, Runway ML, Pika Labs.
    2.  **Görsel Dalı (`_mode == "image"`):**
        * Midjourney/DALL-E için optimize edilmiş, negatif promptları içeren detaylı tasvirler.

---

## 4. Offline 3D Dönüştürücü & Araçlar

Üretim yöneticilerinin internete ihtiyaç duymadan format dönüşümü yapmasını sağlayan masaüstü yazılımıdır.

### 4.1. Teknoloji Stack'i
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) ![Blender](https://img.shields.io/badge/Blender-E87D0D?style=flat&logo=blender&logoColor=white) ![Qt](https://img.shields.io/badge/PySide-41CD52?style=flat&logo=qt&logoColor=white)

* **Python 3.11 + PySide/PyQt:** Modern GUI tasarımı.
* **Blender (Headless Mode):** Arka planda çalışan dönüşüm motoru (GLB → FBX/OBJ/DXF).
* **PyInstaller:** Tek `.exe` dosyası olarak paketleme.

### 4.2. 3ds Max Entegrasyonu
* **MAXScript:** 3ds Max açılışında çalışan özel menü entegrasyonu ve GLB import pipeline'ı.

---

## 🔮 5. Yol Haritası: Mühendislik Veri Pazarı & Nesting

Bu projenin uzun vadeli hedefi, sadece görsel model sunmak değil, **üretime hazır mühendislik verilerini** de pazarlamaktır.

### 5.1. Dijital Varlık Pazarı (Asset Marketplace)
* **Çok Katmanlı Satış:**
    * *Tier 1 (Görsel):* Sadece render ve oyun motorları için GLB/FBX satışı.
    * *Tier 2 (Teknik):* Üreticiler için teknik çizimler (DXF/DWG) ve ölçülendirilmiş şemalar.
* **Ödeme Altyapısı:** Stripe veya Iyzico entegrasyonu ile global mikro ödemeler.

### 5.2. Otomatik Nesting & BOM (Malzeme Listesi)
* Modelin parçalarını (örneğin bir koltuğun iskelet parçaları) algılayıp, plaka üzerine en az fire verecek şekilde yerleştirme (Nesting).
* Kullanıcıya "Satın Al" aşamasında **Kesim Listesi (Cutlist)** ve **Malzeme Maliyet Raporu (BOM)** sunulması.

### 5.3. AI Destekli "Mekan Giydirme"
* Müşterinin kendi odasının fotoğrafını yükleyip, GLB sistemindeki mobilyaları yapay zeka ile odaya yerleştirmesi (Virtual Staging).

---

## 📂 Proje Yapısı (Repo Structure)

```text
.
├── web-platform/           # Firebase, HTML, JS, Three.js dosyaları
│   ├── public/
│   └── firebase.json
├── ai-automation/          # Google Apps Script kodları ve n8n şemaları
│   ├── apps-script/
│   └── n8n-workflows/
├── offline-tools/          # Python masaüstü dönüştürücü kaynak kodları
│   ├── src/
│   └── build_scripts/
├── docs/                   # Teknik dokümantasyon
│   ├── techstack.md        # (Şu an okuduğunuz dosya)
│   └── architecture.png
└── README.md
