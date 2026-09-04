# EKA NDT - Günlük Görevlendirme Planı
## Dokümantasyon v1.0 — Haziran 2026

---

## 📋 İÇİNDEKİLER

1. [Proje Genel Bakış](#1-proje-genel-bakış)
2. [Teknoloji Yığını](#2-teknoloji-yığını)
3. [Dosya Yapısı](#3-dosya-yapısı)
4. [Veri Yapıları](#4-veri-yapıları)
5. [CSS Tasarım Sistemi](#5-css-tasarım-sistemi)
6. [HTML Yapısı](#6-html-yapısı)
7. [JavaScript Mimarisi](#7-javascript-mimarisi)
8. [Sayfa Akışı](#8-sayfa-akışı)
9. [PDF Export Sistemi](#9-pdf-export-sistemi)
10. [Responsive Tasarım](#10-responsive-tasarım)
11. [Erişilebilirlik](#11-erişilebilirlik)
12. [Kurulum ve Çalıştırma](#12-kurulum-ve-çalıştırma)
13. [Bakım ve Güncelleme Rehberi](#13-bakım-ve-güncelleme-rehberi)
14. [Genişletilebilirlik](#14-genişletilebilirlik)

---

## 1. Proje Genel Bakış

### 1.1 Tanım
EKA NDT Günlük Görevlendirme Planı, tek sayfalık bir web uygulaması (SPA) olup, günlük iş görevlerinin planlanmasını, takip edilmesini ve PDF formatında çıktı alınmasını sağlar.

### 1.2 Amaç
- Kaynaktır NDT (Tahribatsız Test) mühendislik firması için günlük görevlendirme planı oluşturmak
- Müşteri, personel, cihaz ve araç verilerini yönetmek
- İş programını tablo halinde düzenlemek
- PDF formatında resmi belge çıktısı almak

### 1.3 Kullanıcılar
- **Admin kullanıcısı**: Tüm özelliklere erişim (admin/admin123)
- **Kayıtlı kullanıcılar**: Veri girişi ve düzenleme
- **Anonim kullanıcılar**: Sadece görüntüleme (izinli personel hariç)

---

## 2. Teknoloji Yığını

| Teknoloji | Versiyon | Amaç |
|-----------|----------|------|
| **HTML5** | - | Sayfa yapısı |
| **CSS3** | - | Stil ve düzen |
| **JavaScript (Vanilla)** | ES6+ | İş mantığı |
| **Inter Font** | Google Fonts | Tipografi (300-800 weight) |
| **localStorage** | - | Veri kalıcılığı |
| **SVG Icons** | Inline | İkonlar (emoji yok) |

### Kullanılan Kütüphaneler
- **Yok** — Tamamen vanilla HTML/CSS/JS

### Önemli Kısıtlamalar
- Tek dosya (`Anasayfa.html`)
- Sunucu gerektirmez (statik dosya)
- Tüm veri localStorage'da saklanır (şimdilik)
- html2pdf.js CDN ile PDF export (opsiyonel)

---

## 3. Dosya Yapısı

```
Is_Plani/
├── Anasayfa.html                      ← Ana uygulama (tek dosya, ~800+ satır)
├── README.md                          ← Proje açıklaması
├── Günlük_görevlendirme_web.md        ← Bu dosya (dokümantasyon)
├── .clinerules                        ← Cline AI geliştirme kuralları
├── skills-lock.json                   ← Skill kilitleme dosyası
├── .cline/skills/                     ← Cline özel skill'leri (10+ skill)
├── .agents/skills/                    ← Agent skill'leri
├── .claude/skills/                    ← Claude skill'leri
└── .gitkeep                           ← Boş klasör takibi
```

### Anasayfa.html İç Yapısı

```
Anasayfa.html
├── <head>
│   ├── Google Fonts (Inter)
│   └── <style> — Tüm CSS (200+ satır)
│       ├── :root (CSS değişkenleri — 27 token)
│       ├── Reset & Global stiller
│       ├── .top-nav (navbar)
│       ├── .header-info (PDF header)
│       ├── .date-bar (tarih seçici)
│       ├── .izinli-box (izinli personel)
│       ├── .task-table (ana tablo)
│       ├── .settings-* (ayarlar)
│       ├── .modal-* (login modal)
│       ├── @media print (yazdırma)
│       ├── @media (max-width: 768px) (tablet)
│       └── @media (max-width: 480px) (mobil)
├── <body>
│   ├── <nav class="top-nav">
│   ├── <div id="page-main"> (ana sayfa)
│   ├── <div id="page-settings"> (ayarlar)
│   ├── <div id="loginModal"> (giriş modal)
│   └── <script> — Tüm JS (400+ satır)
│       ├── DB objesi (localStorage)
│       ├── Varsayılan veriler
│       ├── Yardımcı fonksiyonlar (esc, val, getDayName)
│       ├── Navigasyon (showPage, showSettingsTab)
│       ├── Auth (updateAuthUI, showLogin, doLogin, doLogout)
│       ├── Ana tablo (renderTasks, addRow, deleteRow, changeRowColor)
│       ├── Otomatik tamamlama (showAutocomplete)
│       ├── İzinli personel (renderIzinliMain, addIzinli)
│       ├── Ayarlar CRUD (render/add/del fonksiyonları)
│       ├── PDF export (exportPDF)
│       └── Başlangıç (DOMContentLoaded)
```
# EKA NDT - Günlük Görevlendirme Planı Web Uygulaması

## 📋 Proje Genel Bakış

| Özellik | Değer |
|---------|-------|
| **Proje Adı** | EKA NDT Günlük Görevlendirme Planı |
| **Versiyon** | 2.0 |
| **Teknoloji** | Tek dosya HTML (CSS + JS inline) |
| **Veri Saklama** | localStorage |
| **Dil** | Türkçe arayüz, İngilizce kod |
| **PDF Export** | html2pdf.js (CDN) |
| **Font** | Google Fonts - Inter (300-800) |
| **Responsive** | 768px ve 480px breakpoint'leri |

---

## 🏗️ Mimari Yapı

### Tek Dosya Yapığı (Single Page Application)

```
Anasayfa.html
├── <style> — Tüm CSS (203 satır)
│   ├── :root — CSS değişkenleri (27 token)
│   ├── Global stiller
│   ├── Bileşen stilleri
│   ├── @media print
│   ├── @media (prefers-reduced-motion)
│   └── @media (max-width: 768px / 480px)
├── <body> — HTML yapısı
│   ├── <nav> — Üst navigasyon
│   ├── #page-main — Ana sayfa
│   │   ├── .header-info (print-only)
│   │   ├── .date-bar — Tarih seçimi
│   │   ├── .izinli-box — İzinli personel
│   │   └── .task-table — Ana görev tablosu
│   ├── #page-settings — Ayarlar sayfası
│   │   ├── .settings-tabs — Sekme navigasyonu
│   │   └── 6 × .settings-panel — Her varlık için panel
│   └── #loginModal — Giriş modalı
└── <script> — JavaScript (416 satır)
    ├── DB — Veri katmanı
    ├── Yardımcı fonksiyonlar
    ├── Navigasyon
    ├── Auth (kimlik doğrulama)
    ├── Ana tablo CRUD
    ├── Otomatik tamamlama
    ├── İzinli personel yönetimi
    ├── Ayarlar CRUD
    └── PDF export
```

---

## 💾 Veri Yapıları (localStorage)

### localStorage Anahtarları

| Anahtar | Tip | Açıklama | Varsayılan |
|---------|-----|----------|------------|
| `eka_users` | `[{user, pass}]` | Kullanıcı listesi | `[{user:'admin', pass:'admin123'}]` |
| `eka_currentUser` | `string \| null` | Aktif kullanıcı | `null` |
| `eka_musteriler` | `[{ad, yetkili, telefon, adres}]` | Müşteri/firma listesi | 10 kayıtlı örnek veri |
| `eka_personel` | `[{ad, gorev, telefon}]` | Personel listesi | 11 kayıtlı örnek veri |
| `eka_cihazlar` | `[{ad, kod, durum}]` | Cihaz listesi | `[]` |
| `eka_araclar` | `[{plaka, model, surucu}]` | Araç listesi | `[]` |
| `eka_izinli` | `[string]` | İzinli personel isimleri | `[]` |
| `eka_tasks` | `[taskObj]` | Günlük görevler | 15 örnek görev |
| `eka_taskDate` | `string (YYYY-MM-DD)` | Aktif tarih | Bugün |

### Task Nesnesi

```javascript
{
  color: 'gray' | 'green' | 'blue' | 'yellow',  // Satır rengi
  firma: string,           // Firma adı
  saat: string,            // HH:MM formatında saat
  yer: string,             // İş yapılacak yer
  musteriAd: string,       // Müşteri yetkili kişi
  musteriTel: string,      // Müşteri telefonu
  aciklama: string,        // İş açıklaması
  cihazlar: string,        // Kullanılan cihazlar
  rt: boolean,             // Radografik Test
  ut: boolean,             // Ultrasonik Test
  mt: boolean,             // Manyetik Partikül Test
  pt: boolean,             // Penetrant Test
  notlar: string           // Notlar (textarea)
}
```

---

## 🎨 Tasarım Sistemi (Design Tokens)

### CSS Değişkenleri (27 token)

```css
:root {
  /* Ana Renkler */
  --primary: #2563eb;          // Mavi - Ana aksiyon rengi
  --primary-light: #e0e7ff;    // Açık mavi - Hover/arka plan
  --danger: #dc2626;           // Kırmızı - Hata/silme
  --danger-light: #fee2e2;     // Açık kırmızı - Sil hover
  --danger-border: #fecaca;    // Kımsırı - Sil buton border
  --success: #10b981;          // Yeşil - Başarı/onay
  --success-hover: #059669;    // Koyu yeşil - Ekle hover
  --warning: #f59e0b;          // Sarı - Uyarı/PDF

  // Arka Plan ve Yüzey
  --bg: #f8fafc;               // Açık gri - Sayfa arka planı
  --surface: #ffffff;          // Beyaz - Kart yüzeyi

  // Metin
  --text: #0f172a;             // Koyu lacivert - Ana metin
  --text-light: #1e293b;       // Açık lacivert - Alt header
  --text-muted: #64748b;       // Gri - İkincil metin
  --border: #e2e8f0;           // Açık gri - Border
  --nav-link: #cbd5e1;         // Soluk beyaz - Nav link

  // Tablo Satırları
  --row-green: #dcfce7;        // Yeşil satır
  --row-blue: #dbeafe;         // Mavi satır
  --row-gray: #f8fafc;         // Gri satır
  --row-yellow: #fef3c7;       // Sarı satır
  --row-hover: #f1f5f9;        // Hover rengi

  // İzinli Personel
  --izinli-bg: #fef3c7;        // Sarı arka plan
  --izinli-border: #fde68a;    // Sarı border
  --izinli-title: #92400e;     // Koyu sarı başlık
  --izinli-tag-bg: #fbbf24;    // Tag arka planı
  --izinli-tag-text: #78350f;  // Tag metni

  // Tarih Barı
  --date-bg-start: #eff6ff;    // Gradient başlangıç
  --date-bg-end: #dbeafe;      // Gradient bitiş
}
```

### Tipografi

| Özellik | Değer |
|---------|-------|
| **Font Ailesi** | `'Inter', system-ui, -apple-system, sans-serif` |
| **Temel Boyut** | `14px` (html), `1rem` (body) |
| **Satır Yüksekliği** | `1.5` |
| **Font Ağırlıkları** | 300, 400, 500, 600, 700, 800 |
| **Anti-aliasing** | `-webkit-font-smoothing: antialiased` |

### Border Radius Sistemi

| Değer | Kullanım |
|-------|----------|
| `6px` | Input'lar, butonlar, küçük elemanlar |
| `8px` | Butonlar, kartlar, tablo köşeleri |
| `12px` | Kartlar, paneller, tarih barı |
| `16px` | Modal |
| `999px` | Pill badge'ler, gün adı |

### Box Shadow Sistemi

| Değer | Kullanım |
|-------|----------|
| `0 1px 2px rgba(0,0,0,0.05)` | Doc card |
| `0 1px 3px rgba(0,0,0,0.1)` | Navbar, panel |
| `0 4px 12px rgba(0,0,0,0.15)` | Hover elevation |
| `0 20px 25px -5px rgba(0,0,0,0.1)` | Modal |

---

## 🔧 JavaScript Fonksiyonları

### Veri Katmanı

```javascript
const DB = {
  get(key, def) { /* localStorage'dan oku, parse et, hata varsa default döndür */ },
  set(key, val) { /* localStorage'a yaz, JSON.stringify ile */ }
};
```

### Yardımcı Fonksiyonlar

| Fonksiyon | Açıklama |
|-----------|----------|
| `esc(s)` | XSS koruması: `"` → `&quot;`, `<` → `&lt;`, `>` → `&gt;` |
| `val(id)` | Element değerini trim ile al |
| `getDayName(dateStr)` | Tarih string'inden gün adını hesapla (Pazar-Pazartesi-...) |

### Navigasyon

| Fonksiyon | Açıklama |
|-----------|----------|
| `showPage(page, btn)` | Sayfa göster/gizler (`main` veya `settings`) |
| `showSettingsTab(tab, btn)` | Ayarlar sekmesini değiştirir |

### Kimlik Doğrulama (Auth)

| Fonksiyon | Açıklama |
|-----------|----------|
| `updateAuthUI()` | Giriş/Çıkış butonlarını ve user badge'ini günceller |
| `showLogin()` | Login modalını gösterir, input'ları temizler, focus atar |
| `hideLogin()` | Login modalını gizler |
| `doLogin()` | Kullanıcı adı/şifre kontrolü, başarılıysa currentUser'a kaydeder |
| `doLogout()` | currentUser'ı null yapar, UI günceller |

### Ana Tablo CRUD

| Fonksiyon | Açıklama |
|-----------|----------|
| `initDate()` | Sayfa yüklenince tarih input'unu ve gün adını ayarla |
| `saveMainData()` | Tarih değiştiğinde çalışır, tarihi kaydeder, task'ları kaydeder |
| `saveTasksFromTable()` | Tablodaki tüm satırları oku, tasks array'ine kaydet |
| `renderTasks()` | tasks array'ini tabloya render et (boşsa 15 varsayılan görev ekle) |
| `addRow()` | Yeni boş task ekle, tabloyu yeniden render et |
| `deleteRow(btn)` | Satırı sil, tasks array'inden çıkar, render et |
| `changeRowColor(btn)` | Satır rengini değiştir (gray→green→blue→yellow→gray) |

### Otomatik Tamamlama

| Fonksiyon | Açıklama |
|-----------|----------|
| `showAutocomplete(input, listType, field)` | Input focus'landığında eşleşen verileri göster |
| `removeAutocomplete()` | Autocomplete listesini kaldır |

**Özellik:** Firma adı seçilince otomatik olarak yetkili, adres ve telefon alanları dolar.

### İzinli Personel

| Fonksiyon | Açıklama |
|-----------|----------|
| `renderIzinliMain()` | Ana sayfadaki izinli personel kutusunu render et |
| `addIzinli()` | Seçilen personeli izinli listesine ekle |
| `removeIzinli(i)` | İzinli personeli listeden çıkar |

### Ayarlar CRUD (6 Varlık)

| Varlık | Render | Add | Delete | Silme Sonrası |
|--------|--------|-----|--------|---------------|
| Müşteri | `renderMusteri()` | `addMusteri()` | `delMusteri(i)` | `renderMusteri()` |
| Personel | `renderPersonel()` | `addPersonel()` | `delPersonel(i)` | `renderPersonel()` + `populateIzinliSelects()` |
| Cihaz | `renderCihaz()` | `addCihaz()` | `delCihaz(i)` | `renderCihaz()` |
| Araç | `renderArac()` | `addArac()` | `delArac(i)` | `renderArac()` |
| İzinli | `renderIzinliSettings()` | `addIzinliFromSettings()` | `removeIzinli(i)` | `renderIzinliSettings()` |
| Kullanıcı | `renderKullanici()` | `addKullanici()` | `delKullanici(i)` | `renderKullanici()` |

### PDF Export

```javascript
function exportPDF() {
  // 1. .print-only elemanları göster
  // 2. html2pdf.js ile PDF oluştur
  // 3. Dosya adı: EKA_NDT_YYYY-MM-DD.pdf
  // 4. .print-only elemanları gizle
}
```

---

## 📱 Responsive Tasarım

### Breakpoint'ler

| Breakpoint | Genişlik | Değişiklikler |
|------------|----------|---------------|
| `> 768px` | Desktop | Tam görünüm, yan yana layout |
| `≤ 768px` | Tablet | Nav-linkler gizilir, tek sütun header, küçük padding |
| `≤ 480px` | Mobil | Daha küçük butonlar, user-badge gizilir, küçük font |

### Mobil Uyumlu Özellikler

- **Navbar:** 56px yükseklik, nav-linkler gizli
- **Header:** Tek sütun (`grid-template-columns: 1fr`)
- **Tarih Barı:** `flex-wrap: wrap`
- **Tablo:** Küçük padding (`0.5rem 0.375rem`)
- **Butonlar:** Tam genişlik (`width: calc(100% - 2rem)`)
- **Form:** Tek sütun grid (`grid-template-columns: 1fr`)

---

## ♿ Erişilebilirlik (Accessibility)

### ARIA Etiketleri

| Eleman | Özellik |
|--------|---------|
| Nav butonları | `aria-label="Ayarlar"`, `aria-label="Giriş"`, `aria-label="PDF İndir"` |
| Tablo butonları | `aria-label="Renk değiştir"`, `aria-label="Sil"` |
| SVG ikonlar | `aria-hidden="true"` (dekoratif) |

### Focus Yönetimi

```css
button:focus-visible,
input:focus-visible,
select:focus-visible,
textarea:focus-visible {
  outline: 2px solid var(--primary);
  outline-offset: 2px;
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    transition: none !important;
    animation: none !important;
  }
}
```

---

## 🖨️ PDF Export Mekanizması

### Akış

1. Kullanıcı "PDF" butonuna tıklar
2. `exportPDF()` fonksiyonu çalışır
3. `.print-only` elemanları `display: flex` yapılır (header bilgileri görünür)
4. `html2pdf.js` kütüphanesi `#printableArea`'dan PDF oluşturur
5. Dosya adı: `EKA_NDT_YYYY-MM-DD.pdf`
6. `.print-only` elemanları tekrar gizlenir

### PDF'te Görünen/Gizli Elemanlar

| Görünür | Gizli |
|---------|-------|
| Header bilgileri (şirket, doküman no) | Navbar |
| Tarih barı | Yeni Satır Ekle butonu |
| İzinli personel | İşlem butonları (sil, renk) |
| Ana tablo | Footer |
| | Settings sayfası |

---

## 🔐 Güvenlik

### XSS Koruması

```javascript
function esc(s) {
  return (s||'')
    .replace(/"/g, '&quot;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;');
}
```

Tüm kullanıcı girişi `esc()` fonksiyonu ile temizlenir.

### Kimlik Doğrulama

- Kullanıcı adı ve şifre `eka_users` listesinde karşılaştırılır
- Başarılı giriş `eka_currentUser`'a kaydedilir
- Sayfa yenilendiğinde `currentUser` `localStorage`'tan okunur
- İzinli personel işlemleri için giriş zorunlu

---

## 🚀 Kurulum ve Çalıştırma

### Yöntem 1: Doğrudan Açma

```bash
open Anasayfa.html
```

### Yöntem 2: Yerel Sunucu

```bash
npx serve .
# veya
python3 -m http.server 8000
```

### Varsayılan Giriş Bilgileri

| Kullanıcı Adı | Şifre |
|---------------|-------|
| `admin` | `admin123` |

---

## 🛠️ Bakım ve Genişletme

### Yeni Varlık Ekleme

1. **Veri yapısı ekle:** `DB.get('yeni_varlik', [])` kontrolü ekle
2. **localStorage anahtarı:** `eka_yeni_varlik` formatında
3. **HTML panel:** `.settings-panel` içine yeni div ekle
4. **Sekme butonu:** `.settings-tabs` içine yeni button ekle
5. **CRUD fonksiyonları:** `renderYeni()`, `addYeni()`, `delYeni()` ekle
6. **CSS:** `.list-table` zaten kullanılabilir

### Yeni CSS Değişkeni Ekleme

```css
:root {
  --yeni-degisken: deger;
}
```

### PDF'e Yeni Alan Ekleme

1. HTML'de `<div class="print-only">` içine ekle
2. CSS'te `@media print` içinde görünür olduğundan emin ol

### Renk Değişikliği

Tüm renkler CSS değişkenleri ile tanımlıdır. `:root` bloğundan değiştirin.

---

## 📁 Dosya Yapısı

```
Is_Plani/
├── Anasayfa.html                      (816 satır - Ana uygulama)
├── Günlük_görevlendirme_web.md        (Bu dokümantasyon)
├── README.md                          (Kullanım kılavuzu)
├── .clinerules                        (Proje kuralları)
├── .cline/skills/                     (11 skill)
├── .agents/skills/                    (4 skill)
├── .claude/skills/                    (7 skill)
└── skills-lock.json                   (Skill versiyonları)
```

---

## 📝 Versiyon Geçmişi

| Versiyon | Tarih | Değişiklikler |
|----------|-------|---------------|
| 1.0 | 2026-09-03 | İlk sürüm, temel CRUD |
| 2.0 | 2026-09-04 | UI/UX modernizasyonu, SVG ikonlar, responsive, erişilebilirlik |

---

## 🔄 Güncelleme Talimatları

Bu dokümantasyon, sistemde yapılan her değişiklikte güncellenmelidir:

1. **Yeni fonksiyon eklendiğinde:** İlgili bölüme fonksiyon adını ve açıklamasını ekle
2. **Yeni CSS değişkeni ekleğinde:** "CSS Değişkenleri" tablosuna ekle
3. **Yeni localStorage anahtarı ekleğinde:** "localStorage Anahtarları" tablosuna ekle
4. **Yeni varlık ekleğinde:** Hem veri yapısını hem CRUD bölümünü güncelle
5. **Responsive breakpoint değiğiğinde:** "Responsive Tasarım" bölümünü güncelle
6. **PDF davranışı değiştiğinde:** "PDF Export" bölümünü güncelle

## 4. Veri Yapıları

Tüm veri `localStorage`'a `eka_` prefix'i ile kaydedilir.

### 4.1 eka_users
```js
[{ user: 'admin', pass: 'admin123' }]
```
**Amaç**: Kimlik doğrulama. Varsayılan: `admin/admin123`.

### 4.2 eka_currentUser
```js
string | null
```
**Amaç**: Oturum açıkken kullanıcı adı.

### 4.3 eka_musteriler
```js
[{ ad: 'YASMAK', yetkili: 'Muhammet Bey', telefon: '0 536 410 36 26', adres: 'Hadımköy' }]
```

### 4.4 eka_personel
```js
[{ ad: 'V.SEKMAN', gorev: 'RT', telefon: '' }]
```

### 4.5 eka_cihazlar
```js
[{ ad: 'Olympus OmniScan', kod: 'OS-001', durum: 'Aktif' }]
```

### 4.6 eka_araclar
```js
[{ plaka: '34 ABC 123', model: 'Ford Transit', surucu: 'Mustafa' }]
```

### 4.7 eka_izinli
```js
['V.SEKMAN', 'R.YAMANCI']
```

### 4.8 eka_tasks
```js
[{ color: 'green', firma: 'YASMAK', saat: '17:00', yer: 'HADIMKÖY', musteriAd: 'MUHAMMET BEY', musteriTel: '0 536 410 36 26', aciklama: '30 "', cihazlar: '', rt: true, ut: false, mt: false, pt: false, notlar: 'D2510 - 34 GCB 589' }]
```

### 4.9 eka_taskDate
```js
'2026-06-04'
```

---

## 5. CSS Tasarım Sistemi

### 5.1 Renk Değişkenleri (:root)

```css
:root {
  --primary: #2563eb;
  --primary-light: #e0e7ff;
  --danger: #dc2626;
  --danger-light: #fee2e2;
  --danger-border: #fecaca;
  --success: #10b981;
  --success-hover: #059669;
  --warning: #f59e0b;
  --bg: #f8fafc;
  --surface: #ffffff;
  --text: #0f172a;
  --text-light: #1e293b;
  --text-muted: #64748b;
  --border: #e2e8f0;
  --nav-link: #cbd5e1;
  --row-green: #dcfce7;
  --row-blue: #dbeafe;
  --row-gray: #f8fafc;
  --row-yellow: #fef3c7;
  --row-hover: #f1f5f9;
  --izinli-bg: #fef3c7;
  --izinli-border: #fde68a;
  --izinli-title: #92400e;
  --izinli-tag-bg: #fbbf24;
  --izinli-tag-text: #78350f;
  --date-bg-start: #eff6ff;
  --date-bg-end: #dbeafe;
}
```

### 5.2 Tipografi
- **Font**: `'Inter', system-ui, -apple-system, sans-serif`
- **Base size**: `14px` (html), `1rem` (body)
- **Weight range**: 300, 400, 500, 600, 700, 800

### 5.3 Spacing Sistemi
- `0.25rem` = 4px
- `0.5rem` = 8px
- `0.75rem` = 12px
- `1rem` = 14px
- `1.5rem` = 21px

### 5.4 Border Radius
- `6px` — input, button
- `8px` — butonlar, kartlar
- `12px` — paneller, modal
- `16px` — modal
- `999px` — pill badge'ler

---

## 6. HTML Yapısı

### 6.1 Navbar (`<nav class="top-nav">`)
- Sticky navbar, koyu arka plan (`--text`)
- Logo: "EKA" (kırmızı) + "NDT INSPECTION QUALITY" (beyaz)
- Nav links: "Ana Sayfa" + "Ayarlar"
- Right actions: user badge, Giriş/Çıkış, PDF butonu

### 6.2 Ana Sayfa (`<div id="page-main">`)
- `.header-info.print-only` — PDF'de görünür (şirket bilgileri)
- `.date-bar` — Tarih seçici + gün adı badge
- `.izinli-box` — İzinli personel kartı
- `table.task-table` — Ana görev tablosu
- `.btn-add-row` — Yeni satır ekleme butonu

### 6.3 Ayarlar Sayfası (`<div id="page-settings">`)
- 6 sekme: Müşteri, Personel, Cihaz, Araç, İzinli, Kullanıcı
- Her panel: başlık + form + tablo

### 6.4 Login Modal (`#loginModal`)
- Backdrop blur, yuvarlak köşeli
- Kullanıcı adı + şifre input
- İptal + Giriş Yap butonları

---

## 7. JavaScript Mimarisi

### 7.1 Veri Katmanı (DB)
```js
const DB = {
  get(key, def) { /* localStorage.getItem('eka_'+key) */ },
  set(key, val) { /* localStorage.setItem('eka_'+key, JSON.stringify(val)) */ }
};
```

### 7.2 Varsayılan Veriler
- **users**: `[{user:'admin', pass:'admin123'}]`
- **musteriler**: 10+ kayıt
- **personel**: 11 kayıt
- **tasks**: İlk açılışta 15 örnek satır

### 7.3 Fonksiyon Listesi

#### Navigasyon
| Fonksiyon | Açıklama |
|-----------|----------|
| `showPage(page, btn)` | Sayfa değiştirir |
| `showSettingsTab(tab, btn)` | Ayarlar sekmesi değiştirir |

#### Auth
| Fonksiyon | Açıklama |
|-----------|----------|
| `updateAuthUI()` | Giriş/Çıkış butonlarını gösterir/gizler |
| `showLogin()` | Login modalını açar |
| `hideLogin()` | Login modalını kapatır |
| `doLogin()` | Kullanıcı adı/şifre kontrolü |
| `doLogout()` | Çıkış yapar |

#### Ana Tablo
| Fonksiyon | Açıklama |
|-----------|----------|
| `initDate()` | Tarih alanını yükler |
| `saveMainData()` | Tarih + task'ları kaydeder |
| `saveTasksFromTable()` | Tablodaki input'ları okur |
| `renderTasks()` | Tasks array'ini tabloya yazar |
| `addRow()` | Yeni boş satır ekler |
| `deleteRow(btn)` | Satırı siler |
| `changeRowColor(btn)` | Rengi değiştirir |

#### İzinli Personel
| Fonksiyon | Açıklama |
|-----------|----------|
| `renderIzinliMain()` | Ana sayfadaki izinli kartını günceller |
| `addIzinli()` | Personeli izinli listesine ekler |
| `removeIzinli(i)` | İzni kaldırır |

#### Ayarlar CRUD
Her veri tipi için: `renderXxx()`, `addXxx()`, `delXxx(i)`
---

## 📞 İletişim ve Destek

Bu uygulama EKA NDT Gözetim Müh. San. Tic. Ltd. Şti. iç kullanımı için geliştirilmiştir.

**Son Güncelleme:** 2026-09-04
**Dokümantasyon Versiyonu:** 1.0

---

## 8. Sayfa Akışı

### 8.1 İlk Açılış
1. HTML/CSS yüklenir
2. DB objesi oluşturulur
3. Varsayılan veriler kontrol edilir
4. currentUser kontrol edilir
5. Tarih, tasks, izinli personel yüklenir
6. Sayfa kullanıma hazır

### 8.2 Kullanıcı Girişi
1. "Giriş" butonuna tıkla
2. Modal açılır
3. Kullanıcı adı + şifre gir
4. Doğrulama başarılı → currentUser set edilir
5. Butonlar güncellenir

### 8.3 PDF Çıktısı
1. "PDF" butonuna tıkla
2. `.print-only` elemanları gösterilir
3. `.no-print` elemanları gizlenir
4. html2pdf.js ile PDF oluşturulur
5. Elemanlar eski haline döner

---

## 9. PDF Export Sistemi

| Sınıf | Ekran | PDF |
|-------|-------|-----|
| `.no-print` | Görünür | Gizli |
| `.print-only` | Gizli | Görünür |

**PDF'te görünenler**: Şirket bilgileri, tarih, tablo, izinli personel
**PDF'te gizlenenler**: Navbar, butonlar, işlem sütunu

---

## 10. Responsive Tasarım

| Breakpoint | Genişlik | Hedef |
|------------|----------|-------|
| Desktop | > 768px | Varsayılan |
| Tablet | ≤ 768px | 768px |
| Mobil | ≤ 480px | 480px |

**Mobil değişiklikler**: Navbar küçülür, header tek sütun, tablo padding küçülür

---

## 11. Erişilebilirlik

- `aria-label` — Tüm icon-only butonlarda
- `aria-hidden="true"` — Dekoratif SVG ikonlarda
- `focus-visible` — Tüm interaktif elemanlarda
- `@media (prefers-reduced-motion)` — Animasyon kaldırma
- Yüksek kontrast — WCAG AA uyumlu

---

## 12. Kurulum ve Çalıştırma

```bash
# Doğrudan aç
open Anasayfa.html

# Yerel sunucu
npx serve .
# → http://localhost:3000
```

**Varsayılan giriş**: `admin` / `admin123`

---

## 13. Bakım ve Güncelleme Rehberi

### Yeni Alan Ekleme
1. Veri yapısını güncelle (varsayılan veriler)
2. Tablo başlığına ekle
3. Tablo satırına ekle (template)
4. `saveTasksFromTable()` fonksiyonuna ekle
5. `addRow()` fonksiyonuna ekle

### Yeni Ayar Paneli Ekleme
1. Veri yapısı ekle
2. Sekme butonu ekle
3. Panel HTML'i ekle
4. JS fonksiyonları ekle (render/add/del)
5. `renderAllSettings()` fonksiyonuna ekle

### Dokümantasyon Güncelleme
Her değişiklikte bu dosyayı güncelle:
- Yeni alan: Bölüm 4 + 7
- Yeni panel: Bölüm 6 + 7
- CSS değişikliği: Bölüm 5
- Yeni fonksiyon: Bölüm 7.3

---

## 14. Genişletilebilirlik

| Öncelik | İyileştirme |
|---------|-------------|
| 🔴 Yüksek | Backend entegrasyonu |
| 🔴 Yüksek | Kullanıcı rolleri |
| 🟡 Orta | Tarih aralığı |
| 🟡 Orta | Filtreleme |
| 🟢 Düşük | Karanlık tema |
| 🟢 Düşük | CSV import/export |

### Backend Geçişi
`DB` objesini değiştirerek API'ye geçiş yapılabilir.
### Backend Geçişi
`DB` objesini değiştirerek API'ye geçiş yapılabilir.

---

## 📞 İletişim ve Destek

Bu uygulama EKA NDT Gözetim Müh. San. Tic. Ltd. Şti. iç kullanımı için geliştirilmiştir.

**Son Güncelleme:** 2026-09-04
**Dokümantasyon Versiyonu:** 1.0
