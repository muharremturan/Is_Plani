# EKA NDT - Günlük Görevlendirme Planı

## Proje Dokümantasyonu

**Son Güncelleme:** 2026-09-05
**Versiyon:** 4.5
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
- `saveTasksFromTable()` - Tabloyu localStorage'a yaz
- `renderTasks()` - Görevleri tabloya yaz
- `addRow()` - Yeni satır ekle
- `deleteRow(btn)` - Satır sil
- `changeRowColor(btn)` - Satır rengi değiştir

**Görevler (tarih bazlı):**
- `getTasksForDate(date)` - Belirli bir tarihe ait görev listesi; eski global dizi kalıntısı gelirse tarihe bağlayıp tarih bazlı yapıya çevirir
- `setTasksForDate(date, list)` - Belirli bir tarihe görev listesi yazar

**Otomatik Tamamlama:**
- `showAutocomplete(input, listType, field)` - Autocomplete göster
- `removeAutocomplete()` - Autocomplete kaldır

**İzinli Personel (tarih bazlı):**
- `getSelectedDate()` - Aktif/seçili tarihi döndürür
- `getIzinliForDate(date)` - Belirli bir tarihe ait izinli listesi
- `setIzinliForDate(date, list)` - Belirli bir tarihe izinli listesi yazar
- `renderIzinliMain()` - Ana sayfa izinli listesi
- `addIzinli()` - İzinli ekle (ana sayfa)
- `removeIzinli(i)` - İzinli kaldır (seçili tarih)

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
| `eka_izinli` | `{date: [string]}` | Tarihe göre izinli personel adları |
| `eka_tasks`       | `{date: [task]}` | Tarihe göre günlük görevler (her gün ayrıdır) |
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
  arac: string,      // "PLAKA - MODEL" (ayarlar > araçlar listesinden)
  cihazlar: string,
  personel: string,  // personel adı (ayarlar > personel listesinden)
  tests: string[],   // örn: ["RT","UT","MT","PT"]
  notlar: string
}
```

---

## 5. UI/UX Bileşenleri

### CSS Değişkenleri (27 adet)

```css
:root {
  --primary: #e40a20;
  --primary-hover: #b80019;
  --primary-light: #ffe5e5;
  --primary-bg: #ffebee;
  --danger: #b91c1c;
  --danger-hover: #991b1b;
  --danger-light: #fef2f2;
  --danger-border: #fecaca;
  --success: #1a8b49;
  --success-hover: #166f3b;
  --success-light: #dcf2e0;
  --warning: #f7c51e;
  --warning-hover: #d9a00;
  --warning-light: #fef9e6;
  --bg: #f5f5f5;
  --surface: #ffffff;
  --surface-hover: #efefef;
  --text: #222222;
  --text-light: #333333;
  --text-muted: #595959;
  --text-subtle: #888888;
  --border: #e0e0e0;
  --border-light: #eeeeee;
  --border-dark: #d4d4d4;
  --nav-bg: #18191b;
  --nav-text: #f1f1f1;
  --nav-link: #cccccc;
  --row-green: #dcf2e0;
  --row-blue: #e0e7ff;
  --row-gray: #fafafa;
  --row-yellow: #fef9e6;
  --row-hover: #eeeeee;
  --izinli-bg: #fff1f1;
  --izinli-border: #e40a20;
  --izinli-title: #b91c1c;
  --izinli-tag-bg: #e40a20;
  --izinli-tag-text: #ffffff;
  --date-bg: linear-gradient(135deg, #ffe5e5, #ffebee);
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
- Kırmızı tonlu gradient arka plan
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

### v4.9 - 2026-09-05 (CSS Bütünlük + Premium Sunum Düzeltmeleri)
- **Tanımsız CSS değişken referansları giderildi** (gerçek görünürlük hatası düzeldi):
  - Tablo başlığı (`.grid-header`) kullanıyordu: `--accent-primary`, hover ve light/bg tintleri `:root` ta artık tanımlı (EKA kırmızısı)
  - Backward-compat alias eklendi: `--border`, `--border-light`, `--surface-hover`, `--text-subtle`, `--gray-tint` → mevcut tokenlara bağlandı
- **Eksik noktalı virgül düzeltmeleri (18 yer):** `color: var(--text-primary)` sonrası `;` eksikti → sonraki tüm deklarasyonlar kırılıyordu
- **Animasyon çakışması çözüldü:** `.main-container` scroll-reveal `opacity:0` ile tablo başlığını gizliyordu; artık `fadeInMain` keyframe (0.4s ease-out) kullanıyor
- **Premium hover fizik:** `.top-nav` ve `.add-form` butonları için `::after` parlama overlay'i + `:active` scale (magnetic touch)
- **Doğrulama:** CSS 220/220 blok dengeli, tanımsız değişken kalmadı (otomatik script ile doğrulandı)
### v2.0 - 2026-09-04
### v5.0 - 2026-09-05 (Araç Sütunu Eklendi)
- **Personel sütununun sağına ARAÇ sütunu eklendi** (çoklu seçim dropdown)
- Veri ayarlardaki araç listesinden (`eka_araclar`) çekiliyor, "plaka - model" biçiminde gösteriliyor
- Grid 11 → **12 sütun** oldu (desktop + print şablonları güncellendi)
- `eka_tasks` veri yapısına `arac` alanı eklendi (string, virgülle ayrılmış)
- `renderTasks`, `saveTasksFromTable`, "anlamlı satır" filtresi `arac` ile güncellendi
- Sıralama: NO | Firma | Saat | Adres | Yetkili | Tel | Cihazlar | Testler | Personel | **ARAÇ** | Notlar | İşlem
- **v5.0.1 bug fix:** Dropdown portal yönteminde seçilen öğeler satıra eklenmiyordu — seçili liste `ms` yerine taşınmış `body` üzerindeki dropdown'tan (`dd`) toplanacak şekilde düzeltildi
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

### v3.1 - 2026-09-04
- **CSS Grid Dönüşümü:** `<table>` yapısı tamamen CSS Grid'e çevrildi
  - `.task-grid` container (display: grid, 11 sütun)
  - `.grid-header` (display: contents, sticky başlıklar)
  - `.grid-row` (display: contents, satırlar)
  - `.grid-cell` (esnek hücreler)
  - `grid-template-columns: 40px 1fr 60px 1fr 1fr 90px 1fr 1fr 130px 1fr 80px`
- Tüm JavaScript fonksiyonları güncellendi:
  - `renderTasks()` → div.grid-row oluşturma
  - `saveTasksFromTable()` → .grid-row seçicileri
  - `deleteRow()`, `changeRowColor()`, `copyRow()` → .grid-row seçicileri
  - `showAutocomplete()` → .grid-row seçicileri
- Medya sorguları güncellendi (.task-table → .grid-cell)
- `changeRowColor()` bug fix: `grid-row` class'ı korunuyor
- Kopyala butonu eklendi (İşlem sütununda)

### v3.2 - 2026-09-04 (UX/UI Düzeltmeleri)
- **Grid Yapısı Kararlı Hale Getirildi:** `display: contents` kaldırıldı
  - `.grid-header` ve her `.grid-row` ayrı `display: grid` (paylaşılan sütun şablonu)
  - Sticky başlık güvenilir çalışıyor (transform edilen stacking sorunu giderildi)
- **Satır Taşması Çözüldü:** `.table-wrapper` eklendi
  - `overflow-x: auto` ile yatay kaydırma
  - Sabit genişlik yerine `min-width: 1360px` (küçük ekranlarda yatay scroll)
  - `grid-template-columns`: `45px 190px 70px 150px 150px 100px 190px 120px 140px 150px 95px`
- **Menü/Dropdown Sorunu Çözüldü (alt sütuna kayması):**
  - Test multi-select dropdown z-index: 200 → 9999
  - Auto-complete list z-index: 100 → 9999
  - Modal overlay z-index: 1000 → 10000
  - Dropdown açıkken üstte kalması için `.test-multiselect.open { z-index: 9999 }`
- **Hücre Yerleşimi:** `.grid-cell` artık `display: flex` (dikey ortalama), kenarlıklar düzeltildi
- **Print Stili:** Yazdırmada yatay scroll kaldırıldı, esnekli 1fr kolonlar, header düz
- **Görsel Geri Bildirim (Feedback):**
  - Hover'da satır vurgulama
  - Buton hover'da scale + renk, active durumda scale küçülme
  - Input hover/focus odak durumları
- **Erişilebilir Dokunma Alanı:** `.row-actions` butonları radius 8px, hover renkleri

### v3.3 - 2026-09-04 (Kompakt Satır + Tam Ekran Sığdırma)
- **Kolonlar esnek yapıldı (ekrana tam sığar):**
  - `min-width: 1360px` kaldırıldı, `.task-grid { width: 100% }`
  - `grid-template-columns: 38px 1.3fr 60px 1fr 1fr 78px 1.5fr 1fr 120px 1.1fr 66px`
  - Sabit (İş No, Saat, Tel, Testler, İşlem) + esnek `fr` kolonlar (Firma, Yer, Ad, Açıklama, Cihaz, Notlar)
  - Yatay scroll kaldırıldı; tablo artık görünür genişliğe tam yayılıyor
- **Satır aralıkları kompaktlaştırıldı:**
  - Hücre padding: `var(--space-sm)` → `2px 4px` (mobilde `1px 3px`)
  - Input padding: `3px 6px`, trigger `min-height` 1.6rem
  - `space-sm` → kompakt bootstrap'lar
- **Yazı uzadıkça aşağı genişler:**
  - `grid-cell` `align-items: stretch` + input'lar `align-self: center`
  - Açıklama alanı `text` input → `textarea` (rows=1) çevrildi, çok satır destekleniyor
  - Notlar textarea `height:auto` + `overflow:hidden` (kontenjana göre büyür)
  - Test etiketleri `flex-wrap: wrap` (çok test seçilince alt satıra sarar, satır büyür)
  - `.test-multiselect-trigger` `flex-wrap: wrap`

### v4.0 - 2026-09-04 (Tarih Bazlı İzinli Personel + Silme Düzeltmesi)
- **İzinli personel artık tarih bazlı:** `eka_izinli` array → `{date: [ad, ...]}` obje
  - Eski genel liste otomatik mevcut tarihe taşınıyor (migration)
- **Yeni yardımcı fonksiyonlar:** `getSelectedDate()`, `getIzinliForDate(date)`, `setIzinliForDate(date, list)`
- **Silme düzeltmesi:** `removeIzinli(i)` artık seçili günün listesinden kaldırıyor
  - Geçersiz index koruması (`i < 0 || i >= length` dönüş)
  - Ana sayfadaki ✕ ikonu giriş durumundan bağımsız gösteriliyor (eskiden yalnızca girişli kullanıcıda görünüyordu)
  - ✕ ikonuna `title`, `role=button`, `aria-label` erişilebilirlik etiketleri
- **Tarih değişim entegrasyonu:**
  - `saveMainData()` tarih değiştiğinde `renderIzinliMain()` ile o günün izinlilerini yüklüyor
  - `showPage('main')` dönüşünde de yenileniyor
- `addIzinli`, `addIzinliFromSettings`, `renderIzinliSettings` fonksiyonları tarih bazlı güncellendi

### v4.1 - 2026-09-04 (Nesne / Satır Hizalama Düzeltmesi)
- **Kök neden:** `.grid-cell` `align-items: stretch` kullanıyordu → girdi elemanları hücreye göre geriliyor, hücreler farklı yükseklikte olduğundan elemanlar farklı boyutta görünüyordu
- `.grid-cell` artık `align-items: center` (dikey ortalama, taşma yok)
- **Ortak eleman yüksekliği (32px):** text/time/select input → `height: 32px`
- **Esnek elemanlar (`min-height:32px`):** textarea ve test-trigger (`height:auto`) içerik uzadıkça aşağı büyür — önceki istekle uyumlu
- `.row-num` ve `row-actions button` 28px'e sabitlendi (satır hiz ilişkisi)
- textarea `padding-top:6px` + `max-height:200px` + `overflow:hidden`
- Tüm öğeler `box-sizing: border-box`, `.notes-area` `min-height: 32px`

### v4.2 - 2026-09-04 (İşlem sütununda tek ⋮ menü)
- İşlem sütununda 3 ayrı simge (renk/kopyala/sil) kaldırıldı; yerine **tek dikey ⋮ (kebab) butonu** kondu
- Butona tıklanınca **aşağı açılan menü** görüntüleniyor:
  - Renk değiştir, Kopyala, Sil
- **Yapı:** `.row-menu` (relative) + `.row-menu-btn` (⋮) + `.row-menu-dropdown` (absolute, right:0, z-index:9999)
- **Kapanma davranışları:**
  - Dışa tıklayınca `closeMenus()`
  - `Escape` tuşu ile kapanır
  - Başka bir menü açılınca önceki kapanır (`aria-expanded` güncellenir)
- Aynı anda yalnızca bir menü açık kalabilir
- Print'te `.row-menu` gizleniyor (`no-print` + print kuralı)
- Mevcut `changeRowColor`, `copyRow`, `deleteRow` fonksiyonları `.grid-row` buldukları için aynen çalışıyor

### v4.5 - 2026-09-05 (İşler izinlerin mantığıyla: her gün tamamen bağımsız)
- **Talep:** "İzinler sadece kaydedildiği gün çıkıyor ama işler tüm günlerde aynı görünüyor; işlerin mantığı izinler gibi olsun."
- **Yeni yardımcılar (izin helper'larıyla birebir aynı pattern):**
  - `getTasksForDate(date)` - sadece o güne ait görev listesini döner; eski global dizi kalıntısı gelirse onu ilk erişimde o tarihe bağlayıp tarih bazlı yapıya çevirir
  - `setTasksForDate(date, list)` - sadece o güne ait listeyi yazar
- Eski kod veri yapısına göre defalarca `if (Array.isArray(allTasks))` dallandırıyordu; tüm iş okuma/yazma noktaları bu helper'lara indirgendi:
  - `saveTasksFromTable()`, `renderTasks()`, `addRow()`, `deleteRow()`, `copyRow()`, `confirmCopy()`
- **Davranış değişikliği:** Boş günde varsayılan boş satır artık **veriye kaydedilmiyor**; yalnızca ekranda tek boş satır gösteriliyor. Kalıcı satır "Yeni Satır Ekle" ile eklenir. Böylece boş bir güne bakıldığında `eka_tasks` içine gereksiz kayıt yazılmaz.
- Doğrulama: Node simülasyonu ile iki farklı günün listesinin bağımsız olduğu ve bir günü sildiğinde diğer günün etkilenmediği teyit edildi.

### v4.6 - 2026-09-05 (EKA marka renk paleti - ekandt.com.tr)
- Palet `https://www.ekandt.com.tr` sitesinden alındı (color.css `red.css` ile `--primary` #e40a20 marka kırmızısı; site gri/yişil/sarı tonları).
- `:root` değişken değerleri yenilendi (`Anasayfa.html` `<style>`):
  - `--primary` = `#e40a20`, `--primary-hover` = `#b80019`, `--primary-light` = `#ffe5e5`, `--primary-bg` = `#ffebee`
  - `--danger` = `#b91c1c` (primary'den ayrı tutuldu), `--success` = `#1a8b49` (EKA yeşili), `--warning` = `#f7c51e`
  - Nötr gri palet EKA tonlarına yönlendirildi (`--bg #f5f5f5`, `--text #222222`, `--border #e0e0e0`, ...)
  - `--nav-bg` = `#18191b`, `--nav-text`/`--nav-link` beyaz/gri (site header tonu)
  - `--izinli-*` renkleri kırmızı/marka tonlarına alındı
  - `--date-bg` gradienti kırmızı tonlara çevrildi
- Yapı değişmedi: hâlâ CSS değişkenleri + sınıflar. `.clinerules` "CSS değişkenleri hardcode kullanma" kuralına uygun.
- **Görsel etki:** Nav/menü koyu gri, logo ve vurgu renkleri EKA kırmızısı, focus-outline mavi → kırmızı.

### v4.4 - 2026-09-05 (Her gün ayrı sayfa garantisi / veri tamiri)
- **Teşhis:** "Tüm günlerde aynı tablo görünüyor / bir satır silinince her yerde siliniyor" sorununun kök nedeni backend değil; tarayıcının uygulamanın **eski (global tek dizi)** sürümünü önbellekten çalıştırmasıydı. LocalStorage tarih bazlı mimariyi zaten destekliyor; Node simülasyonu ile de doğrulandı (D1'in D2'den silme işlemi etkilenmediği görüldü).
- **Önbellek önleme:** `<head>`'e `Cache-Control: no-cache, no-store, must-revalidate`, `Pragma: no-cache`, `Expires: 0` meta etiketleri eklendi (tarayıcı artık eski dosyayı kullanmayacak).
- **`normalizeInitialData()` yeni fonksiyon eklendi (açılışta çalışır):**
  - Eski global `eka_tasks` dizisini tarih bazlı `{date: [task]}` yapıya taşır (mevcut tarihe bağlar).
  - Tarih bazlı yapıdaki **her günün dizisini derin kopyalar** → iki günün aynı diziye referans vermesi (paylaşılan dizi) imkânsız hale gelir.
  - Eski global `eka_izinli` dizisini tarih bazlı yapıya taşır.
- **`eka_modelVersion = '3.1'`** anahtarı eklendi (yeni sürümün yüklendiğini doğrulamak için).
- **Kullanıcıya not:** Sorun devam ediyorsa `Ctrl+Shift+R` (F5) ile sayfayı tam yenileyin; düzelmezse tarayıcının site verilerini temizleyin.

### v4.3 - 2026-09-04 (Kompakt Sütunlar + Tutarlı Sayfa Hizalaması)
- **Sütun genişlikleri daraltıldı** (tablo artık sayfaya sığıyor):
  - `38px 1.3fr 60px 1fr 1fr 78px 1.5fr 1fr 120px 1.1fr 66px` → `32px 1fr 55px 1fr 1fr 72px 1.4fr 1fr 108px 1fr 60px`
  - Sabit sütunlar ~362px → ~317px (İş No 32, Saat 55, Tel 72, Testler 108, İşlem 60)
- **Yatay hizalama tutarlılığı giderildi (farklı noktada bitme sorunu):**
  - Sorun: `.table-wrapper` yatay margin 0 (tam genişlik), `.date-bar`/`.izinli-box` 1.5rem → tablo diğerlerinden daha geniş, farklı kenarda bitiyordu
  - Çözüm: `.table-wrapper { margin: var(--space-lg) var(--space-xl) }` → artık date-bar ve izinli-box ile **aynı 1.5rem yatay hizada** başlıyor/bitiyor
  - Mobilde (768px) tablo da `0.75rem 1rem` ile diğer bölümlerle aynı hizada
  - Print'te `margin: 0 !important` (yazdırma temiz)

### v3.0 - 2026-09-04
- **ADIM 1:** Tarih bazlı veri mimarisi (localStorage obje yapısı)
- Migration sistemi (eski veriler otomatik taşındı)
- Tarih değişimi ile dinamik tablo güncelleme
- İzinli personel silme fonksiyonu UI'a bağlandı
- **ADIM 2:** Modern CSS Grid tasarımı
- Soft renk paleti (Shadcn UI stili)
- Esnek sütun genişlikleri (1fr, sabit)
- Modern border-radius, box-shadow, spacing
- **ADIM 3:** Testler modülü
- Ayarlar'a "Testler" sekmesi eklendi (eka_testler)
- RT/UT/MT/PT checkbox'ları yerine multi-select dropdown
- Arama yapılabilen çoklu seçim
- **ADIM 4:** Dinamik ComboBox ve görev kopyalama
- Firmalar, adresler, yetkili, cihazlar için datalist desteği
- Hem seçilebilir hem yazılabilir form alanları
- "Kopyala" butonu (tarih seçimi ile)
- Başarı modalı ile geri bildirim
- Dosya boyutu: 1228 satır

### v2.0 - 2026-09-04
- UI/UX modernizasyonu (14 adım)
- 27 CSS değişkeni
- 20 SVG ikon (emoji kaldırıldı)
- Responsive tasarım (768px + 480px)
- Erişilebilirlik (aria-label, focus-visible, reduced-motion)
- `.print-only` sınıfı
- CİHAZLAR sütunu

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

**Dokümantasyon Son Güncelleme:** 2026-09-05
**Uygulama Versiyon:** 5.0
**Toplam Kod:** ~1245 satır (HTML + CSS + JS)
