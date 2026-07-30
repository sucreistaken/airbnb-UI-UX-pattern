# Airbnb UI/UX Pattern Kütüphanesi - Tasarım

Tarih: 2026-07-30
Durum: Onaylandı

## Amaç

Airbnb'nin web sitesi ve mobil uygulamasındaki UI/UX pattern'lerini uçtan uca çıkarıp,
ileride konusu Airbnb ile ilgili olsun ya da olmasın başka bir uygulamada kullanıcı
deneyimini bu pattern'ler üzerine kurabilmek için referans alınacak bir doküman seti
üretmek. Bu aşamada kod/app yazılmıyor; hedef tamamen araştırma ve dokümantasyon.

## Kapsam

Aşağıdaki bölümler hem web hem mobil ayrımı gözetilerek çıkarılacak:

1. `discovery-search` - Keşif & Arama (arama çubuğu, filtreler, harita, kategori sekmeleri)
2. `listing-card-browse` - İlan Kartı & Liste/Grid Görünümü (skeleton, wishlist kalp, fiyat gösterimi)
3. `listing-detail` - İlan Detay Sayfası (galeri, host bilgisi, review özeti, sticky rezervasyon kutusu)
4. `booking-checkout` - Rezervasyon / Checkout Akışı (tarih seçici, misafir sayacı, fiyat döküm, ödeme adımları)
5. `trust-safety-signals` - Güven & Güvenlik Sinyalleri (superhost, doğrulama, review sistemi)
6. `messaging-notifications` - Mesajlaşma & Bildirimler
7. `navigation-ia` - Navigasyon & Bilgi Mimarisi (web: header/mega menü; mobil: alt tab bar, bottom sheet, gesture)
8. `visual-design-system` - Görsel Tasarım Sistemi (tipografi, renk, spacing, ikon dili, motion/micro-interaction)
9. `states-feedback` - Boş/Hata/Yükleme Durumları & Feedback
10. `personalization-recommendations` - Kişiselleştirme & Öneri Kartları
11. `mobile-specific-patterns` - Mobile-only pattern'ler (gesture, haptic, native bottom sheet derinliği) - diğer
    bölümlerde web/mobil ayrımı olsa da, mobilin gölgede kalmaması için ayrı bir sentez bölümü.
12. `adaptation-playbook` - Airbnb dışı bir uygulamaya uyarlama rehberi (genel prensiplerin özeti, kapanış bölümü)

Her pattern maddesi şu alanları içerecek:

- Ne olduğu (kısa tanım)
- Nerede görülür (web / mobil / ikisi de)
- UX gerekçesi (neden işe yarıyor)
- Airbnb dışı bir uygulamaya uyarlama notu
- Kaynak / güven notu (gerçek kaynaktan mı doğrulandı, yoksa eğitim verisinden gelen doğrulanmamış bir gözlem mi)

## Yöntem

- WebSearch/WebFetch ile gerçek kaynaklar taranacak: Airbnb Design (airbnb.design), NN/g ve
  UX Collective gibi yerlerdeki teardown/vaka analizleri, Airbnb'nin kendi blog/mühendislik
  yazıları (DLS - Design Language System dahil).
- Doğrulanamayan gözlemler kesinlik iddia etmeden "doğrulanmadı / eğitim verisinden" olarak
  işaretlenecek. Tek kaynaktan genel kural çıkarılmayacak.
- Canlı ürün her zaman değişebileceğinden (Airbnb sürekli A/B test yapan bir ürün), pattern'ler
  "şu an tam olarak böyle" değil "yerleşik/tekrar gözlemlenen pattern" çerçevesiyle yazılacak.

## Çıktı Yapısı

```
patterns/
  00-INDEX.md              - tüm bölümlere link + kısa özet
  01-discovery-search.md
  02-listing-card-browse.md
  03-listing-detail.md
  04-booking-checkout.md
  05-trust-safety-signals.md
  06-messaging-notifications.md
  07-navigation-ia.md
  08-visual-design-system.md
  09-states-feedback.md
  10-personalization-recommendations.md
  11-mobile-specific-patterns.md
  12-adaptation-playbook.md
PROGRESS.md                 - hangi bölüm hangi durumda (todo/in-progress/done), loop takibi için
```

## Süreç

- Bu proje bağımsız bir git deposu (`~/Desktop/Projects/airbnb-ux-patterns`).
- `/loop` ile kendi kendine iterasyon: her turda `PROGRESS.md`'den bir sonraki eksik/yetersiz
  bölüm seçilecek, araştırılıp doldurulacak/derinleştirilecek, commit atılacak, sonra bir
  sonraki tura geçilecek.
- Tüm bölümler "done" olana kadar devam edilecek (loop-until-dry); son turda genel bir
  tutarlılık/eksik tarama turu yapılıp `adaptation-playbook.md` ile kapatılacak.
- Bu doküman implementasyon planı yerine geçer; ayrı bir writing-plans adımına gerek yok
  çünkü çıktı kod değil, doğrudan bu dokümantasyon setidir.
