# EKA NDT - UI/UX Modernizasyon Planı

## 🎯 AMAÇ
Anasayfa.html'in görünümünü modernleştirmek. İşlevsellik korunacak, sadece CSS ve HTML yapısı iyileştirilecek.

##  KULLANILACAK SKILL'LER
- `ui-ux-pro-max` → Genel UI/UX prensipleri, layout, hiyerarşi
- `ui-styling` → Renk, tipografi, spacing, animasyon detayları

Her adımda ilgili skill'i `@skill` ile çağır.

---

## ⚡ TOKEN TASARRUFU KURALLARI (ÇOK ÖNEMLİ)

1. **ASLA tüm dosyayı yeniden yazma** — Sadece değişecek blokları değiştir
2. **Mevcut JS'e dokunma** — Sadece CSS ve HTML yapısı
3. **Her adımda tek bir bölüm** — Birden fazla şeyi aynı anda değiştirme
4. **Kısa rapor ver** — "✅ Tamamlandı" + ne değişti (max 3 cümle)
5. **Hata varsa geri al** — Bir önceki working state'e dön
6. **Gereksiz açıklama yazma** — Kod konuşsun

---

## 📋 ADIMLAR

### ADIM 1: Stil Temeli (Renk + Font + Reset)

**Hedef:** Modern renk paleti ve tipografi altyapısı

**Yapılacaklar:**
1. `<head>` içine Google Fonts ekle (Inter, 300-700 arası)
2. CSS değişkenleri tanımla (`:root`):
   - `--primary: #2563eb`
   - `--danger: #dc2626`
   - `--success: #10b981`
   - `--warning: #f59e0b`
   - `--bg: #f8fafc`
   - `--surface: #ffffff`
   - `--text: #0f172a`
   - `--text-muted: #64748b`
   - `--border: #e2e8f0`
3. `body` arka planını `--bg` yap
4. Font-family'yi `'Inter', system-ui, sans-serif` yap
5. `* { box-sizing: border-box; }` ekle

**Doğrulama:** Sayfa Inter fontuyla yüklenmeli, arka plan açık gri olmalı.

---

### ADIM 2: Navbar Modernizasyonu

**Hedef:** Üst menüyü modern dashboard navbar'ına çevir

**Yapılacaklar:**
1. `.top-nav` yüksekliğini 64px yap
2. Arka plan: `--text` (#0f172a)
3. Box-shadow: `0 1px 3px rgba(0,0,0,0.1)`
4. Logo boyutu: 24px, font-weight 800
5. Nav butonları:
   - Border-radius: 8px
   - Padding: 8px 16px
   - Hover: `background: rgba(255,255,255,0.1)`
   - Active: `background: var(--primary)`
6. Sağ butonlar (Giriş, PDF, Çıkış):
   - Aynı stil
   - Renk kodları: giriş=#3b82f6, PDF=#f59e0b, çıkış=#ef4444
7. Flex gap: 8px

**Doğrulama:** Navbar koyu, butonlar yuvarlak köşeli, hover efektleri çalışmalı.

---

### ADIM 3: Header Kart (Şirket Bilgileri)

**Hedef:** Üst bilgi alanını iki sütunlu modern karta çevir

**Yapılacaklar:**
1. `.header-info` → `display: grid; grid-template-columns: 1fr auto; gap: 24px; align-items: center;`
2. Sol taraf:
   - Logo: 32px, font-weight 800, color: var(--danger)
   - Alt yazı: 11px, color: var(--text-muted)
   - Ana başlık: 20px, font-weight 700, color: var(--text)
   - Alt başlık: 14px, font-weight 500, color: var(--text-muted)
3. Sağ taraf (Doküman bilgileri):
   - Küçük kart: background: var(--surface), border-radius: 8px, padding: 12px 16px
   - Box-shadow: `0 1px 2px rgba(0,0,0,0.05)`
   - Tablo yerine div satırları: `display: flex; justify-content: space-between; gap: 16px;`
   - Label: 11px, color: var(--text-muted), font-weight 500
   - Value: 12px, color: var(--text), font-weight 600
4. Alt border yerine padding-bottom + margin-bottom

**Doğrulama:** İki sütunlu düzen, sağda küçük bilgi kartı görünmeli.

---

### ADIM 4: Tarih Barı

**Hedef:** Tarih seçim alanını modern pill/badge görünümüne çevir

**Yapılacaklar:**
1. `.date-bar` arka plan: `linear-gradient(135deg, #eff6ff, #dbeafe)`
2. Border-radius: 12px
3. Margin: 16px 24px
4. Padding: 16px 24px
5. Box-shadow: `0 1px 2px rgba(0,0,0,0.05)`
6. Tarih input:
   - Border-radius: 6px
   - Border: 1px solid var(--border)
   - Padding: 6px 12px
   - Focus: `ring: 2px solid var(--primary)`
7. Gün adı badge:
   - Background: var(--primary)
   - Color: white
   - Padding: 4px 12px
   - Border-radius: 999px (pill)
   - Font-size: 12px, font-weight 600

**Doğrulama:** Tarih barı mavi gradient, gün adı pill badge olarak görünmeli.

---

### ADIM 5: İzinli Personel Kartı

**Hedef:** Sarı kutuyu modern uyarı kartına çevir

**Yapılacaklar:**
1. `.izinli-box`:
   - Background: #fef3c7
   - Border: 1px solid #fde68a
   - Border-radius: 12px
   - Padding: 20px
   - Margin: 16px 24px
2. Başlık: 14px, font-weight 600, color: #92400e
3. Tag'ler (`.izinli-tag`):
   - Background: #fbbf24
   - Color: #78350f
   - Padding: 4px 12px
   - Border-radius: 999px
   - Font-size: 12px, font-weight 500
   - Display: inline-flex, align-items: center, gap: 6px
4. Kaldır butonu (✕): cursor: pointer, hover: color: #dc2626
5. Select + Ekle butonu yan yana, modern form stili

**Doğrulama:** Tag'ler pill şeklinde, kaldır butonu hover'da kırmızı olmalı.

---

### ADIM 6: Tablo Header

**Hedef:** Tablo başlığını modern dashboard tablosuna çevir

**Yapılacaklar:**
1. `.task-table`:
   - Width: 100%
   - Border-collapse: separate (collapse yerine)
   - Border-spacing: 0
2. `th`:
   - Background: var(--text) (#0f172a)
   - Color: white
   - Padding: 12px 16px
   - Font-size: 11px
   - Font-weight: 600
   - Text-transform: uppercase
   - Letter-spacing: 0.5px
   - Position: sticky, top: 0, z-index: 10
3. İlk th (sol): border-radius: 8px 0 0 0
4. Son th (sağ): border-radius: 0 8px 0 0
5. Sub-header (ikinci satır): background: #1e293b (bir ton açık)

**Doğrulama:** Header koyu, sticky, köşeleri yuvarlak olmalı.

---

### ADIM 7: Tablo Satırları ve Hücreler

**Hedef:** Satır görünümünü ve hücre içlerini iyileştir

**Yapılacaklar:**
1. Satır renkleri (yumuşat):
   - `.row-green td`: background: #dcfce7
   - `.row-blue td`: background: #dbeafe
   - `.row-yellow td`: background: #fef3c7
   - `.row-gray td`: background: #f8fafc
2. Tüm satırlarda hover: `tr:hover td { background: #f1f5f9 !important; }`
3. `td`:
   - Padding: 10px 12px
   - Border-bottom: 1px solid var(--border)
   - Border-top: yok
4. Input'lar:
   - Border: 1px solid transparent
   - Background: transparent
   - Padding: 6px 10px
   - Border-radius: 6px
   - Focus: border-color: var(--primary), background: white
   - Width: 100%
5. Textarea (notlar): min-height: 36px, resize: vertical
6. Checkbox'lar:
   - Küçük badge görünümü tercih et (JS değişikliği gerektirirse ADIM 12'ye ertele)
   - Şimdilik sadece stil: transform: scale(1.1), cursor: pointer

**Doğrulama:** Satırlar yumuşak renkli, hover çalışmalı, input'lar odaklanınca border almalı.

---

### ADIM 8: Satır Aksiyonları ve İş No

**Hedef:** Sağdaki butonları ve iş numarasını modernleştir

**Yapılacaklar:**
1. İş numarası (`.row-num`):
   - Background: #e2e8f0
   - Color: var(--text)
   - Width: 28px, height: 28px
   - Border-radius: 50%
   - Display: inline-flex, align-items: center, justify-content: center
   - Font-size: 12px, font-weight: 600
2. Aksiyon butonları:
   - Width: 32px, height: 32px
   - Border-radius: 6px
   - Border: yok
   - Background: transparent
   - Hover: background: #f1f5f9
   - 🗑️ hover: background: #fee2e2, color: #dc2626
   - 🎨 hover: background: #e0e7ff
   - Font-size: 16px
   - Display: inline-flex, align-items: center, justify-content: center
3. Tooltip (opsiyonel, title attribute yeterli)

**Doğrulama:** İş no yuvarlak badge, butonlar hover'da renk değiştirmeli.

---

### ADIM 9: Yeni Satır Ekle Butonu

**Hedef:** Tablo altındaki ekleme butonunu modernleştir

**Yapılacaklar:**
1. `.btn-add-row`:
   - Background: white
   - Border: 2px dashed var(--border)
   - Color: var(--text-muted)
   - Border-radius: 8px
   - Padding: 12px 24px
   - Font-weight: 500
   - Width: auto (tam genişlik değil)
   - Display: inline-flex, align-items: center, gap: 8px
   - Margin: 16px 24px
   - Transition: all 0.2s ease
2. Hover:
   - Border-color: var(--primary)
   - Color: var(--primary)
   - Background: #eff6ff
3. İkon: + (Unicode veya SVG)

**Doğrulama:** Dashed border, hover'da mavi olmalı.

---

### ADIM 10: Ayarlar Sayfası - Sekmeler

**Hedef:** Ayarlar sekmelerini modern tab yapısına çevir

**Yapılacaklar:**
1. `.settings-tabs`:
   - Display: flex, gap: 4px
   - Border-bottom: 2px solid var(--border)
   - Padding-bottom: 0
   - Margin-bottom: 24px
2. Sekme butonları:
   - Background: transparent
   - Border: yok
   - Border-bottom: 2px solid transparent
   - Color: var(--text-muted)
   - Padding: 12px 16px
   - Font-weight: 500
   - Font-size: 14px
   - Cursor: pointer
   - Margin-bottom: -2px (border üstüne binmesi için)
3. Aktif sekme:
   - Color: var(--primary)
   - Border-bottom-color: var(--primary)
   - Font-weight: 600
4. Hover (pasif):
   - Color: var(--text)
   - Background: #f8fafc

**Doğrulama:** Sekmeler alt çizgili, aktif olan mavi olmalı.

---

### ADIM 11: Ayarlar Panel ve Form

**Hedef:** Ayarlar içerik panelini ve ekleme formlarını modernleştir

**Yapılacaklar:**
1. `.settings-panel`:
   - Background: var(--surface)
   - Border-radius: 12px
   - Box-shadow: `0 1px 3px rgba(0,0,0,0.1)`
   - Padding: 24px
2. Panel başlığı (`h3`):
   - Font-size: 18px
   - Font-weight: 700
   - Color: var(--text)
   - Border-bottom: yok (kaldır)
   - Margin-bottom: 20px
   - Padding-bottom: 0
3. Ekleme formu (`.add-form`):
   - Display: grid
   - Grid-template-columns: repeat(auto-fit, minmax(200px, 1fr))
   - Gap: 12px
   - Align-items: end
4. Form inputları:
   - Border: 1px solid var(--border)
   - Border-radius: 6px
   - Padding: 10px 12px
   - Font-size: 14px
   - Focus: border-color: var(--primary), box-shadow: 0 0 0 3px rgba(37,99,235,0.1)
5. Ekle butonu:
   - Background: var(--success)
   - Color: white
   - Border: yok
   - Border-radius: 6px
   - Padding: 10px 20px
   - Font-weight: 500
   - Hover: background: #059669

**Doğrulama:** Panel beyaz kart, form grid layout, input'lar focus'ta mavi ring almalı.

---

### ADIM 12: Ayarlar Liste Tablosu

**Hedef:** Ayarlar içindeki listeleme tablolarını modernleştir

**Yapılacaklar:**
1. `.list-table`:
   - Width: 100%
   - Border-collapse: separate
   - Border-spacing: 0
2. `th`:
   - Background: #f8fafc
   - Color: var(--text-muted)
   - Font-size: 11px
   - Font-weight: 600
   - Text-transform: uppercase
   - Letter-spacing: 0.5px
   - Padding: 10px 12px
   - Border-bottom: 2px solid var(--border)
3. `td`:
   - Padding: 12px
   - Border-bottom: 1px solid var(--border)
   - Font-size: 14px
4. Satır hover: `tr:hover td { background: #f8fafc; }`
5. Zebra: `tr:nth-child(even) td { background: #fafafa; }`
6. Sil butonu (`.del-btn`):
   - Background: transparent
   - Color: #ef4444
   - Border: 1px solid #fecaca
   - Border-radius: 6px
   - Padding: 4px 10px
   - Font-size: 12px
   - Hover: background: #ef4444, color: white

**Doğrulama:** Liste tablosu temiz, sil butonu hover'da kırmızı dolgu olmalı.

---

### ADIM 13: Login Modal

**Hedef:** Giriş modalını modern ve şık hale getir

**Yapılacaklar:**
1. `.modal-overlay`:
   - Background: rgba(15, 23, 42, 0.6)
   - Backdrop-filter: blur(4px)
2. `.modal`:
   - Background: white
   - Border-radius: 16px
   - Box-shadow: `0 20px 25px -5px rgba(0,0,0,0.1), 0 10px 10px -5px rgba(0,0,0,0.04)`
   - Padding: 32px
   - Width: 400px
   - Max-width: 90vw
3. Modal başlık (`h3`):
   - Font-size: 20px
   - Font-weight: 700
   - Text-align: center
   - Margin-bottom: 24px
   - Color: var(--text)
4. Input'lar:
   - Border: 1px solid var(--border)
   - Border-radius: 8px
   - Padding: 12px 16px
   - Font-size: 14px
   - Margin-bottom: 12px
   - Focus: border-color: var(--primary), box-shadow: 0 0 0 3px rgba(37,99,235,0.1)
5. Butonlar:
   - Border-radius: 8px
   - Padding: 12px
   - Font-weight: 500
   - Font-size: 14px
   - İptal: background: #f1f5f9, color: var(--text-muted)
   - Giriş: background: var(--primary), color: white
   - Hover: transform: translateY(-1px)
6. Hata mesajı:
   - Background: #fef2f2
   - Border: 1px solid #fecaca
   - Color: #dc2626
   - Border-radius: 6px
   - Padding: 10px 12px
   - Font-size: 13px
   - Text-align: center

**Doğrulama:** Modal yuvarlak köşeli, blur arka plan, input'lar focus'ta ring almalı.

---

### ADIM 14: Animasyonlar ve Geçişler

**Hedef:** Mikro etkileşimler ve sayfa geçişleri ekle

**Yapılacaklar:**
1. Global geçişler:
   ```css
   * { transition: background-color 0.2s ease, color 0.2s ease, border-color 0.2s ease; }