# come-to-glb
# 🏗️ GLB Ekosistemi (Portfolyom)

![Status](https://img.shields.io/badge/Status-Live-success)
![Platform](https://img.shields.io/badge/Platform-Web%20%7C%20Mobile%20%7C%20Desktop-blue)
![Stack](https://img.shields.io/badge/Stack-Firebase%20%7C%20Supabase%20%7C%20Redis%20%7C%20React%20Native-orange)
![AI Tools](https://img.shields.io/badge/AI-Gemini%20%7C%20Dualite%20%7C%20Stitch-purple)

**Portfolyom (GLB)**; mobilya & iç mimari odaklı 3D modelleri, üretim planlama ihtiyaçlarını (mühendislik çizimleri, nesting) ve yapay zekâ destekli içerik üretimini tek çatı altında toplayan hibrit bir ekosistemdir.

Bu repo, aşağıdaki sistemlerin üst düzey mimarisini, dokümantasyonunu ve **gelecekteki mobil dönüşüm planını** içerir:
* Web tabanlı 3D model platformu
* AI prompt & otomasyon stüdyosu
* Offline 3D dönüştürücü masaüstü aracı
* Mobil uygulama ve backend dönüşüm mimarisi

> 📚 **Detaylı teknoloji yığını ve migrasyon planı için:** [**`docs/techstack.md`**](docs/techstack.md)

---

## 🔍 Ne Çözüyor?
* 3D koltuk / mobilya modelleri için **merkezi bir GLB kütüphanesi** sunar.
* Üretim tarafı için **ölçü, boyut ve mühendislik verisini** görünür kılar.
* Veo, Runway, Pika, Midjourney, DALL·E gibi sistemler için **yüksek doğruluklu video/görsel prompt’ları** üretir.
* Tamamen offline çalışan bir masaüstü araç ile **GLB → FBX/OBJ/DXF** dönüşümünü tek tıkla yapar.

---

## 🧩 Ana Bileşenler

### 1. Web Platformu – GLB 3D Model Kütüphanesi
* **Vanilla HTML/CSS/JS + Three.js:**
    * `index.html`, `models.html`, `admin.html` ile modüler yapı.
    * GLB/glTF modellerin tarayıcıda interaktif önizlemesi.
* **Firebase (Hosting, Auth, Firestore, Storage):**
    * Rol bazlı yetkilendirme (Admin / Kullanıcı).
    * Firestore’da model metadata yönetimi (ölçüler, kategori, thumbnail).
    * Storage’da GLB + WebP thumbnail pipeline (480/960px, `srcset` desteği).
* **Admin Paneli:**
    * Klasörden toplu model yükleme.
    * Otomatik isim düzeltme (Türkçe karakter & boşluk temizleme).
    * Model silindiğinde Storage + Firestore’dan tam temizlik (Deep Clean).

### 2. AI Stüdyosu – Prompt & Otomasyon
* **Google Sheets + Apps Script Web App:**
    * Türkçe arayüz: Video/Görsel modu, stil seçimi, kamera hareketi, atmosfer vb.
    * Form verisini normalize edip n8n webhook’una JSON olarak gönderir.
* **n8n Workflow & Gemini API:**
    * `_mode` alanına göre iki dal:
        * `video`: Doğal dil açıklama + zaman çizelgesi JSON (timeline, kamera, ışık, altyazı).
        * `image`: Midjourney/DALL·E için stil bazlı detaylı prompt + negatif prompt.
    * Çıktıyı tek bir `prose` (HTML) alanında geri döndürür; arayüz bu alanı doğrudan gösterir.

### 3. Offline 3D Dönüştürücü – Masaüstü Araç
* **Python + PySide/PyQt:** Klasör / dosya seçimi, tablo görünümü, karanlık tema, ilerleme çubuğu.
* **Blender (Headless):** GLB → FBX/OBJ/DXF dönüşümünü arka planda yürütür.
* **PyInstaller:** Tek `.exe` paket (konsolsuz, sessiz çalışma).

### 4. Genişleme ve Dönüşüm (Mobile & Supabase)
Projenin ilerleyen fazlarında artan veri yükü ve mobil ihtiyaçlar için şu teknolojiler devreye alınacaktır:
* **Dualite & Stitch:** Stitch Design System ile hazırlanan UI'ın, Dualite kullanılarak **React Native** bileşenlerine dönüştürülmesi ve gerçek mobil uygulamaya geçiş.
* **Supabase Entegrasyonu:** Kullanıcı yönetimi (Auth) ve gerçek zamanlı veri akışının (Realtime DB) Firebase'den Supabase altyapısına taşınması.
* * **Redis (Caching):** Sık erişilen 3D model verileri ve AI yanıtları için yüksek performanslı önbellekleme (Caching) katmanı.
* **AI Analiz Mimarisi:** Ücretli üyelik ve analiz raporları için ölçeklenebilir backend yapısının kurulması.

---

## 🧭 Yol Haritası (Özet)

> *Detaylı roadmap için bakınız: **[`docs/techstack.md` → Bölüm 4 ve 5](docs/techstack.md)**.*

* **Kısa Vadede:** Web arayüzünde gelişmiş filtreler, AI stüdyosunda "Tek Tık Reels" şablonları.
* **Orta Vadede:**
    * GLB’den otomatik 2D teknik çizim/DXF üretimi.
    * **Dualite** ile mobil uygulama prototipinden gerçek uygulamaya geçiş.
    * **Supabase + Redis** ile yüksek performanslı ve gerçek zamanlı veri altyapısı.
* **Uzun Vadede:** GLB platformunu üreticiler için bir **3D model marketplace** haline getirmek ve ERP entegrasyonları.

---

## 📂 Klasör Yapısı

Bu repo şu anda mimari ve teknoloji yığını odaklıdır. Kodlar mantıksal olarak aşağıdaki gibi ayrıştırılmıştır:

```text
.
├── web-platform/           # (Mevcut) Firebase + Three.js tabanlı GLB web katmanı
│   ├── public/
│   └── firebase.json
├── ai-automation/          # Apps Script + n8n akışları
│   ├── apps-script/
│   └── n8n-workflows/
├── offline-tools/          # Python + Blender dönüştürücü
│   ├── src/
│   └── build_scripts/
├── mobile-app/             # (Gelecek) React Native + Supabase katmanı
├── docs/                   # Teknik dokümanlar
│   ├── techstack.md
│   └── architecture.png
└── README.md
