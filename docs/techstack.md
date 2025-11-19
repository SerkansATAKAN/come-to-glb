# 🏗️ GLB Ekosistemi – Teknoloji Yığını & Mimari

![Project Status](https://img.shields.io/badge/Status-Live-success)
![Platform](https://img.shields.io/badge/Platform-Web%20%7C%20Mobile%20%7C%20Desktop-blue)
![Workflow](https://img.shields.io/badge/Workflow-AI%20Native-purple)

**Portfolyom (GLB);** mobilya & iç mimari odaklı 3D modelleri, üretim planlama ihtiyaçlarını (nesting, teknik çizim) ve yapay zekâ destekli içerik üretimini tek çatı altında toplayan çok katmanlı hibrit bir projedir.

Bu dokümantasyon, projenin mevcut detaylı teknik mimarisini ve ileride gerçekleşecek **Supabase & Mobil Dönüşüm** yol haritasını içerir.

---

## 1. Web Platformu (GLB – 3D Model Gösterim)

Mobilya ve koltuk odaklı 3D modellerin yönetildiği, interaktif olarak önizlendiği ve mühendislik verilerinin sunulduğu web katmanıdır.

### 1.1. Frontend Teknolojileri
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white) ![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white) ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black) ![Three.js](https://img.shields.io/badge/Three.js-black?style=flat&logo=three.js&logoColor=white)

* **Vanilla HTML/CSS/JS:** Framework bağımlılığı olmayan (No-Framework), hafif ve hızlı yapı.
    * Modüler sayfa yapısı: `index.html`, `models.html`, `admin.html`.
    * Responsive tasarım: CSS Grid ve Flexbox ile mobil uyumlu kart yapıları.
    * Optimizasyon: `srcset` + `sizes` ile cihaz çözünürlüğüne göre (480px/960px) WebP thumbnail sunumu.
* **Three.js (WebGL):**
    * `.glb` / `.gltf` formatındaki sıkıştırılmış 3D varlıkların tarayıcıda render edilmesi.
    * OrbitControls ile zoom, pan ve döndürme yetenekleri.
    * PBR materyal ve ışıklandırma ayarları ile gerçekçi ürün sunumu.

### 1.2. Backend & Infrastructure (Mevcut Faz: Firebase)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=firebase&logoColor=black)

* **Firebase Hosting:** `firebase.json` üzerinden statik varlıklar ve agresif cache yönetimi.
* **Firebase Authentication:**
    * Role-Based Access Control (RBAC): Admin ve Kullanıcı ayrımı.
    * Guarded Routes: Kritik sayfalara yetkisiz erişimin engellenmesi.
* **Cloud Firestore (NoSQL):**
    * Model Metadata Yönetimi: Boyutlar (X,Y,Z), kategori ağacı.
    * Admin panelinden CRUD operasyonları.
* **Firebase Storage:**
    * `models/`: Optimize edilmiş GLB dosyaları.
    * `thumbnails/`: WebP formatında kapak görselleri.

### 1.3. Admin Paneli Özellikleri
* **Batch Upload:** Klasör seçimi ile toplu model yükleme.
* **Auto-Sanitization:** Dosya isimlerinden Türkçe karakter ve boşlukların otomatik temizlenmesi.
* **Deep Clean:** Bir model silindiğinde hem Storage'dan dosyanın hem de Firestore'dan kaydın eş zamanlı temizlenmesi.

---

## 2. AI Prompt & Otomasyon Stüdyosu

Görsel ve video üretim süreçleri için "insan doğruluklu" promptlar üreten otomasyon katmanıdır.

### 2.1. Arayüz & Veri Girişi
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=flat&logo=google-sheets&logoColor=white) ![Apps Script](https://img.shields.io/badge/Apps%20Script-4285F4?style=flat&logo=google&logoColor=white)

* **Google Sheets:** Stil kütüphanesi, kamera hareketleri ve renk paletleri için master veritabanı.
* **Google Apps Script (Web App):** Dinamik form arayüzü ile parametrelerin toplanması ve JSON payload oluşturulması.

### 2.2. n8n İş Akışı (Backend Logic & Gemini API)
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=flat&logo=n8n&logoColor=white) ![Gemini](https://img.shields.io/badge/AI-Google%20Gemini-4285F4)

Sistem, gelen isteği analiz eder, **Google Gemini API** ile zenginleştirir ve iki ana dala ayırır:

1.  **Video Dalı (`_mode == "video"`):**
    * Sinematik doğal dil anlatımı + JSON Timeline (`camera_action`, `lighting`).
    * *Hedef:* Google Veo, Runway ML, Pika Labs.
2.  **Görsel Dalı (`_mode == "image"`):**
    * Midjourney/DALL-E için optimize edilmiş, negatif promptları içeren detaylı tasvirler.

---

## 3. Offline 3D Dönüştürücü & Araçlar

Üretim yöneticilerinin internete ihtiyaç duymadan format dönüşümü yapmasını sağlayan masaüstü yazılımıdır.

### 3.1. Teknoloji Stack'i
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) ![Blender](https://img.shields.io/badge/Blender-E87D0D?style=flat&logo=blender&logoColor=white) ![Qt](https://img.shields.io/badge/PySide-41CD52?style=flat&logo=qt&logoColor=white)

* **Python 3.11 + PySide/PyQt:** Modern GUI tasarımı.
* **Blender (Headless Mode):** Arka planda çalışan dönüşüm motoru (GLB → FBX/OBJ/DXF).
* **PyInstaller:** Tek `.exe` dosyası olarak paketleme.

### 3.2. 3ds Max Entegrasyonu
* **MAXScript:** 3ds Max açılışında çalışan özel menü entegrasyonu ve GLB import pipeline'ı.

---

## 🚀 4. Genişleme ve Dönüşüm Fazı (Roadmap)

Projenin ilerleyen bölümlerinde, artan veri yükü ve ölçeklenme ihtiyaçları (depolama limitleri, kullanıcı sayısı) nedeniyle barındırma hizmeti **Firebase'den Supabase'e** taşınacak ve **Mobil Uygulama** entegrasyonu başlayacaktır.

### 4.1. Kullanılacak Yeni Teknolojiler & Kaynaklar
* 👉 **[Google AI / GPT Studio](https://aistudio.google.com)** – Kod ve mantık geliştirme
* 👉 **[Stitch Design System](https://stitch.withgoogle.com)** – UI Tasarım Sistemi
* 👉 **[Figma](https://www.figma.com)** – Arayüz Tasarımı
* 👉 **[Supabase](https://supabase.com)** – Backend & Veritabanı (Firebase Alternatifi)
* 👉 **[Dualite](https://dualite.dev)** – Figma to Code (React Native Dönüştürücü)

### 4.2. Dönüşüm Adımları
* **Dualite ile Mobil Uygulamayı Hayata Geçirme:**
    Stitch Design System ile oluşturduğumuz UI’yı, Dualite kullanarak React Native component’lerine dönüştürerek tamamen çalışan bir mobil arayüze taşıyacağım.
* **Supabase Entegrasyonu (Backend Geçişi):**
    Uygulamaya backend altyapısı ekleyerek kullanıcı kayıt (sign up), giriş (login) ve doğrulama gibi işlemleri Supabase üzerinde sorunsuz şekilde çalışır hale getireceğim.
* **Gerçek Zamanlı Veri Akışı:**
    Supabase’in `auth` + `database` (Realtime) yapısını kullanarak veri akışını hazırlayacak ve uygulamanın sonraki AI analiz bölümlerine altyapıyı kuracağım.
* **AI Analiz Mimarisinin Temelleri:**
    Kullanıcıya göre yetkiler (Row Level Security) ile ileride ücretli üyeliğe (SaaS) alt zemin hazırlayacağım.
* **Mobil Prototip → Gerçek Uygulama Geçişi:**
    Tasarımlarımızı (Dualite + React Native + Supabase Entegrasyonu) artık sadece bir prototip değil, “gerçek bir uygulama ekranı” olarak çalışır şekilde sisteme entegre edeceğim.

---

## 🔮 5. Ticari Vizyon: Mühendislik Veri Pazarı & Nesting

Uzun vadeli hedef, görsel model sunumunun ötesine geçmektir:

### 5.1. Dijital Varlık Pazarı (Asset Marketplace)
* **Çok Katmanlı Satış:**
    * *Tier 1 (Görsel):* Render ve oyun motorları için GLB/FBX.
    * *Tier 2 (Teknik):* Üreticiler için teknik çizimler (DXF/DWG).
* **Ödeme Altyapısı:** Global mikro ödemeler.

### 5.2. Otomatik Nesting & BOM
* Modelin parçalarını algılayıp plaka üzerine en az fire verecek şekilde yerleştirme (Nesting).
* "Satın Al" aşamasında Malzeme Listesi (BOM) sunumu.

### 5.3. AI Destekli "Mekan Giydirme"
* Kullanıcı odalarına yapay zeka ile sanal ürün yerleştirme (Virtual Staging).

---

## 📂 Proje Yapısı (Repo Structure)

```text
.
├── web-platform/           # (Mevcut) Firebase + Three.js Web Katmanı
├── mobile-app/             # (Gelecek) React Native + Supabase Katmanı
├── ai-automation/          # Google Apps Script + n8n + Gemini
├── offline-tools/          # Python + Blender Desktop Araçları
├── docs/                   # Teknik Dokümantasyon
│   ├── techstack.md        # (Şu an okuduğunuz dosya)
│   └── architecture.png
└── README.md
