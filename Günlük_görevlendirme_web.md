# EKA NDT - Günlük Görevlendirme Planı

## Proje Dokümantasyonu

**Son Güncelleme:** 2026-09-04
**Versiyon:** 2.0
**Teknoloji:** HTML5 + CSS3 + Vanilla JS + localStorage

---

## 1. Proje Özeti

EKA NDT için günlük görevlendirme planı oluşturma ve yönetme uygulaması. Tek sayfalık web uygulaması (SPA) olarak tasarlanmış, tüm veriler tarayıcı localStorageında saklanmaktadır.

### Temel Özellikler

- Günlük görev ekleme/düzenleme/silme
- Müşteri, personel, cihaz, araç yönetimi
- İzinli personel takibi
- Otomatik tamamlama (autocomplete)
- PDF çıktısı (html2pdf.js)
- Kullanıcı giriş sistemi
- Responsive tasarım (mobil uyumlu)
- Modern dashboard UI (Inter Font)

---

## 2. Proje Yapısı

```
Is_Plani/
├── Anasayfa.html              # Ana uygulama (tek dosya, ~800 satır)
├── README.md                  # Kullanım kılavuzu
├── .clinerules                # Cline AI geliştirme kuralları
├── .cline/skills/             # Cline skillleri (11 adet)
├── .agents/skills/            # Agent skillleri
├── .claude/skills/            # Claude skillleri
├── Günlük_görevlendirme_web.md # Bu dokümantasyon
└── skills-lock.json           # Skill versiyon kilidi
```

---

## 3. Mimari Yapı

### Tek Dosya Mimarisi

| Bölüm | Satır Aralığı | Açıklama |
|-------|--------------|----------|
| `<style>` | 1-203 | CSS stilleri (27 CSS değişkeni) |
| `<body>` (nav) | 207-219 | Üst navigasyon |
| `<body>` (main) | 221-267 | Ana sayfa (tablo, tarih, izinli) |
| `<body>` (settings) | 270-366 | Ayarlar sayfası (6 sekme) |
| `<body>` (modal) | 369-381 | Login modal |
| `<script>` | 383-800 | JavaScript (~420 satır) |

### JavaScript Modülleri

**Veri Katmanı:**
- `DB.get(key, def)` - localStoragedan oku
- `DB.set(key, val)` - localStoragedan yaz

**Yardımcı Fonksiyonlar:**
- `esc(s)` - XSS koruması (HTML escape)
- `val(id)` - Input değerini al
- `getDayName()` - Tarih → gün adı

**Navigasyon:**
- `showPage(page, btn)` - Sayfa değiştir
- `showSettingsTab(tab, btn)` - Ayarlar sekmesi değiştir

**Auth:**
- `updateAuthUI()` - Giriş/çıkış UI güncelle
- `showLogin()` / `hideLogin()` - Login modal
- `doLogin()` / `doLogout()` - Giriş/çıkış yap

**Ana Tablo:**
- `initDate()` - Tarih başlat
- `saveMainData()` - Tarih + görevleri kaydet
- `renderTasks()` - Görevleri tabloya yaz
- `addRow()` - Yeni satır ekle
- `deleteRow(btn)` - Satır sil
- `changeRowColor(btn)` - Satır rengi değiştir

**Otomatik Tamamlama:**
- `showAutocomplete(input, listType, field)` - Autocomplete göster
- `removeAutocomplete()` - Autocomplete kaldır

**İzinli Personel:**
- `renderIzinliMain()` - Ana sayfa izinli listesi
- `addIzinli()` - İzinli ekle (ana sayfa)
- `removeIzinli(i)` - İzinli kaldır

**Ayarlar CRUD:**
- `renderAllSettings()` - Tüm ayarları render et
- `render/add/delMusteri()` - Müşteri işlemleri
- `render/add/delPersonel()` - Personel işlemleri
- `render/add/delCihaz()` - Cihaz işlemleri
- `render/add/delArac()` - Araç işlemleri
- `populateIzinliSelects()` - İzinli selectleri doldur
- `renderKullanici/add/delKullanici()` - Kullanıcı yönetimi

**PDF Export:**
- `exportPDF()` - PDF oluştur ve indir

---

## 4. Veri Yapıları (localStorage)

### localStorage Anahtarları

| Anahtar | Tip | Açıklama |
|---------|-----|----------|
| `eka_users` | `[{user, pass}]` | Kullanıcı listesi |
| `eka_currentUser` | `string \| null` | Aktif kullanıcı |
| `eka_musteriler` | `[{ad, yetkili, telefon, adres}]` | Müşteri/firma listesi |
| `eka_personel` | `[{ad, gorev, telefon}]` | Personel listesi |
| `eka_cihazlar` | `[{ad, kod, durum}]` | Cihaz listesi |
| `eka_araclar` | `[{plaka, model, surucu}]` | Araç listesi |
| `eka_izinli` | `[string]` | İzinli personel adları |
| `eka_tasks` | `[task]` | Günlük görevler |
| `eka_taskDate` | `string (YYYY-MM-DD)` | Seçili tarih |

### Task Nesnesi

```javascript
{
  color: gray | green | blue | yellow,
  firma: string,
  saat: string,
  yer: string,
  musteriAd: string,
  musteriTel: string,
  aciklama: string,
  cihazlar: string,
  rt: boolean,
  ut: boolean,
  mt: boolean,
  pt: boolean,
  notlar: string
}
```

---

## 5. UI/UX Bileşenleri

### CSS Değişkenleri (27 adet)

```css
:root {
  --primary: #2563eb;
  --primary-light: #e0e7ff;
  --danger: #dc2626;
  --danger-light: #fee2e2;
  --danger-border: #fecaca;
  --success: #10b981;
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

### Sayfa Yapıları

**Navbar (`.top-nav`)**
- Sticky üst menü (64px yükseklik)
- Logo + navigasyon butonları + sağ aksiyonlar
- SVG ikonlar (emoji yok)
- Focus-visible ring desteği

**Header (`.header-info`)**
- Sadece PDFde görünür (`.print-only` sınıfı)
- İki sütunlu grid düzeni
- Şirket bilgileri + doküman meta verileri

**Tarih Barı (`.date-bar`)**
- Mavi gradient arka plan
- Tarih input + gün adı pill badge
- Border-radius: 12px

**İzinli Personel (`.izinli-box`)**
- Sarı tonlu kart
- Pill şekilli tagler
- Kaldır butonu (hoverda kırmızı)

**Görev Tablosu (`.task-table`)**
- Sticky header (koyu arka plan)
- Renkli satırlar (yeşil/mavi/sarı/gri)
- Hover efekti
- Inline editing (input, select, textarea, checkbox)
- Satır aksiyonları (renk değiştir, sil)

**Ayarlar Sayfası**
- 6 sekme (Müşteri, Personel, Cihaz, Araç, İzinli, Kullanıcı)
- Her sekme: ekleme formu + liste tablosu
- Grid layout form

**Login Modal**
- Blur arka plan (backdrop-filter)
- Yuvarlak köşeli (16px)
- Focus ringli inputlar

### Responsive Breakpoints

| Breakpoint | Değişiklikler |
|-----------|---------------|
| `max-width: 768px` | Nav gizilir, tek kolon layout |
| `max-width: 480px` | Daha küçük butonlar |

### Erişilebilirlik (A11y)

- `aria-label` tüm icon-only butonlarda
- `aria-hidden="true"` dekoratif SVGlerde
- `:focus-visible` ringler (2px outline)
- `@media (prefers-reduced-motion: reduce)` desteği
- Yüksek kontrast renkler (WCAG AA uyumlu)

---

## 6. Güvenlik

### XSS Koruması

```javascript
function esc(s) {
  return (s||)
    .replace(/"/g, &quot;)
    .replace(/</g, &lt;)
    .replace(/>/g, &gt;);
}
```

Tüm kullanıcı girişi `esc()` fonksiyonu ile temizlenir.

### Kullanıcı Doğrulama

- Basit kullanıcı adı/şifre kontrolü
- Şifre plaintext olarak saklanır (localStorage)
- Varsayılan: `admin / admin123`

---

## 7. Kullanım Kılavuzu

### Giriş

1. Sayfa yüklendiğinde login modal açılır
2. `admin` / `admin123` ile giriş yapın
3. Giriş sonrası tüm özellikler aktif olur

### Görev Ekleme

1. Tarih seçin (varsayılan: bugün)
2. Yeni Satır Ekle butonuna tıklayın
3. Tablo hücrelerini doldurun
4. Veriler otomatik kaydedilir

### Satır Rengi
- Gri → Yeşil → Mavi → Sarı → Gri (döngü)
- Rengi butonuna her tıklamada bir sonraki renk

### PDF Çıktısı

1. PDF butonuna tıklayın
2. Tarayıcı print dialog açılır
3. Save as PDF seçin

### Ayarlar

1. Ayarlar butonuna tıklayın
2. İlgili sekmeyi seçin
3. Form doldurup Ekle butonuna basın
4. Listeden silmek için Sil butonuna tıklayın

---

## 8. Geliştirici Notları

### Kodlama Standartları

- **Tek dosya:** `Anasayfa.html` (CSS ve JS inline)
- **İngilizce isimlendirme:** Türkçe değişken/fonksiyon isimleri kullanma
- **XSS koruması:** `esc()` fonksiyonu ile
- **Otomatik kayıt:** Her değişiklikte `DB.set()` çağrısı
- **CSS değişkenleri:** Hardcoded renk kullanma

### Yeni Özellik Ekleme

1. Veri yapısını `DB.get()` ile tanımla
2. HTMLde ilgili bileşeni oluştur
3. CSSde stil tanımla (CSS değişkenleri kullan)
4. JSde render/add/delete fonksiyonlarını yaz
5. `render*()` fonksiyonlarını güncelle

### Test

```bash
# Yerel sunucu
npx serve .

# Doğrudan aç
open Anasayfa.html
```

---

## 9. Varsayılan Veriler

### Müşteriler (11 kayıt)

| Firma | Yetkili | Telefon | Adres |
|-------|---------|---------|-------|
| YASMAK | Muhammet Bey | 0 536 410 36 26 | Hadımköy |
| SADANA | Aydın Karabulut | - | Hadımköy |
| ORBA | Orhan Barış | - | Silivri |
| ASLI BERK | Mesut Erkan | - | Bahçeşehir |
| AS-AZEM | Hamza Alp | - | Arnavutköy |
| ŞILAN | Sinan Ateş | - | Harmandere |
| ÖZGÜN İNŞAAT | Metin Yapa | - | Kaynarca |
| SEDF TERSANESİ | Mustafa Kolo | - | Tuzla |
| ZESOB | Soner Aksakal | - | EKA |
| GÜNERİ MAKİNA | ASA H.İrkil | - | ASA İnş. |
| ADA | Cemal Bey | - | Tuzla |
| SAĞLAM | Engin Kale | - | EKA |

### Personel (11 kayıt)

| Ad | Görev | Telefon |
|----|-------|---------|
| V.SEKMAN | RT | - |
| R.YAMANCI | RT | - |
| O.ALTUN | RT | - |
| T.VEZİR | RT | - |
| A.ŞENAL | UT/PT | - |
| Ç.AYDÖNER | UT | - |
| E.DADAY | UT/MT/PT | - |
| S.AYDOĞUŞ | RT | - |
| R.AKTİN | RT | - |
| B.İÇLEK | RT | - |
| H.ALTUN | RT | - |

---

## 10. Değişiklik Geçmişi (CHANGELOG)

### v2.0 - 2026-09-04
- Modern dashboard UI (Inter Font, CSS değişkenleri)
- 27 CSS değişkeni ile tema sistemi
- Emoji ikonlar → SVG ikonlar (20+ ikon)
- Responsive tasarım (768px + 480px breakpoint)
- Erişilebilirlik (aria-label, focus-visible, reduced-motion)
- Global transition optimize (sadece interaktif elemanlar)
- Font-size artırıldı (13px → 14px base)
- Tüm hardcoded renkler CSS değişkenine çevrildi
- `.print-only` sınıfı (header PDFye özel)
- CİHAZLAR sütunu eklendi
- `improve-codebase-architecture` skill kuruldu
- `find-skills` (vercel-labs) skill kuruldu
- `.clinerules` verimlilik kurallarıyla güncellendi

### v1.0 - 2026-09-03
- İlk sürüm
- Temel CRUD işlemleri
- localStorage veri saklama
- PDF export
- Kullanıcı giriş sistemi

---

## 11. Notlar

- Veriler tarayıcı localStorageında saklanır
- Farklı tarayıcılarda veri paylaşılmaz
- Veri yedekleme için düzenli PDF alın
- Gelecek: Backend entegrasyonu, çoklu kullanıcı, gerçek zamanlı senkronizasyon

---

**Dokümantasyon Son Güncelleme:** 2026-09-04
**Uygulama Versiyon:** 2.0
**Toplam Kod:** ~800 satır (HTML + CSS + JS)
