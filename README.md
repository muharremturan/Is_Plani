# EKA NDT - Günlük Görevlendirme Planı

## Kısa Açıklama

EKA NDT şirketine ait tek sayfalık (SPA) web uygulamasıdır. Günlük iş programı, müşteri, personel, cihaz ve araç yönetimi sağlar. Tüm veriler tarayıcıdaki `localStorage` içinde saklanır. Arayüz Türkçe'dir ve PDF çıktısı alabilmek üzere tasarlanmıştır.

## Kurulum Adımları

1. Projeyi çalıştırın (herhangi bir build adımı gerekmez):
   - `index.html` dosyasını doğrudan bir web tarayıcısında açın, **veya**
2. Alternatif olarak yerel bir HTTP sunucusu kullanın:
   ```bash
   npx serve .
   ```
   veya
   ```bash
   python3 -m http.server 8000
   ```
3. Tarayıcınızı `http://localhost:8000` adresine yönlendirin.

> Not: `localStorage` kullandığından, uygulama dosya protokolüyle (`file://`) da çalışabilir. Tarayıcı güvenlik ayarlarınız localStorage erişimine izin vermelidir.

## Kullanım Kılavuzu

- **Giriş:** Kullanıcı adı ve şifre ile oturum açın.
- **Ana Sayfa:** Günlük görev listesini görüntüleyin, yeni görev ekleyin, düzenleyin veya silin.
- **Veri Yönetimi:** Müşteri, personel, cihaz ve araç bilgilerini ekleyip düzenleyebilirsiniz.
- **PDF Çıktısı:** `.no-print` sınıfına sahip elemanlar PDF'de gizlenir, `.print-only` sınıfına sahip elemanlar sadece PDF'de görünür. Tarayıcı yazdırma fonksiyonunu veya PDF butonunu kullanarak PDF alabilirsiniz.
- **Otomatik Kayıt:** Tüm değişiklikler otomatik olarak `localStorage`'a kaydedilir.

## Varsayılan Giriş Bilgileri

| Kullanıcı Adı | Şifre     |
|---------------|-----------|
| `admin`       | `admin123`|

## Veri Yapıları (localStorage)

| Anahtar          | Tip / Yapı |
|------------------|------------|
| `eka_users`       | `[{user, pass}]` |
| `eka_currentUser` | `string \| null` |
| `eka_musteriler`  | `[{ad, yetkili, telefon, adres}]` |
| `eka_personel`    | `[{ad, gorev, telefon}]` |
| `eka_cihazlar`    | `[{ad, kod, durum}]` |
| `eka_araclar`     | `[{plaka, model, surucu}]` |
| `eka_izinli`      | `[string]` |
| `eka_tasks`       | `[{color, firma, saat, yer, musteriAd, musteriTel, aciklama, cihazlar, rt, ut, mt, pt, notlar}]` |
| `eka_taskDate`    | `string` (YYYY-MM-DD) |

## Geliştirme Kuralları

`.clinerules` dosyasında detaylı kurallar bulunmaktadır. Lütfen bu kurallara uyun:

1. `index.html` dışına çıkmayın, yeni dosya oluşturmayın.
2. Mevcut pattern'i koruyun.
3. Geriye uyumluluğu bozmayın.
4. Türkçe hata mesajları kullanın.
5. Her değişiklik sonrası test edilebilir olduğundan emin olun.