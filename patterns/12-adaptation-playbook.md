# 12. Airbnb Dışı Bir Uygulamaya Uyarlama Rehberi

Bu bölüm bir sentezdir, önceki 11 bölümde tek tek belgelenen 133 pattern'i tekrar etmez. Amaç,
o pattern'lerin arkasında tekrar tekrar ortaya çıkan az sayıda üst-ilkeyi çıkarmak, bunları
somut bir uygulama checklist'ine dönüştürmek, Airbnb'yi kör kopyalamanın nerede yanlış olacağını
dürüstçe işaretlemek ve son olarak 12 bölümün doğrulama kalitesini tek bir yerde özetlemektir.
Referanslar `bkz. patterns/0X` biçiminde, ilgili bölümdeki madde numarasıyla birlikte veriliyor.

---

## 1. Genel felsefe: tekrar eden meta-prensipler

### 1.1. Arama, hamburger menünün yerini alan ana navigasyon omurgasıdır

Airbnb'nin IA'sının en belirgin kararı, üç çizgili bir menü yerine büyük, kapsül biçimli bir arama
çubuğunu ekranın en görünür, en büyük bileşeni yapmasıdır. Bu tek başına bir bileşen tercihi değil,
"kullanıcı niyeti oturumların büyük çoğunluğunda aynı ve öngörülebilir" varsayımına dayanan bir
IA felsefesidir: birincil eylem gizli bir menünün arkasına değil, sayfanın en göze çarpan yerine
konur (bkz. patterns/07 madde 1). Bunun doğal sonucu, klasik bir breadcrumb'a da ihtiyaç
kalmamasıdır çünkü konum bilgisi bir hiyerarşi seviyesi değil, aramanın bir parametresidir
(bkz. patterns/07 madde 9). Aynı felsefe arama girişinin kendisinde de görülür: segmentli tek
bir pil (Nereye/Ne zaman/Kim), kategori şeridi, esnek arama modları, hepsi "önce arama kutusunu
doldur" zorunluluğunu farklı seviyelerde yumuşatan aynı ilkenin uzantılarıdır (bkz. patterns/01
madde 1, 2, 3).

### 1.2. Fotoğraf/içerik önde, arayüz kromu geride ve cimri

Airbnb'nin marka rengi Rausch neredeyse hiç kullanılmaz; sadece birincil CTA, arama vurgusu ve
kalp ikonu gibi az sayıda "asıl eylem" noktasında görünür, geri kalan her şey nötr gri/siyah/beyaz
kalır (bkz. patterns/08 madde 2). Bunun gerekçesi tesadüfi değil: gerçek karar materyali (mekânın
fotoğrafı) arayüz süslemesiyle rekabet etmemelidir (bkz. patterns/08 madde 7). Aynı ilke ilan
kartı anatomisinde (fotoğraf kartın neredeyse tamamı, meta bilgi ince bir şerit, bkz. patterns/02
madde 1) ve galeri tasarımında (grid önizleme + isteğe bağlı tam ekran derinlik, bkz. patterns/03
madde 1) tekrar eder. Buton hiyerarşisi de aynı "cimri kullan" mantığına uyar: bir ekranda sadece
tek bir dolu/vurgulu buton olur, geri kalanı metin/outline'dır (bkz. patterns/08 madde 10).

### 1.3. Progressive disclosure: yaklaşık/kısa önce, kesin/tam sonra

Bu ilke projede en sık tekrar eden kalıptır ve en az üç farklı biçimde görülür. Birincisi, bilgi
yoğunluğu: açıklama metni 500 karakterde kesilir, "daha fazla göster" isteğe bağlıdır
(bkz. patterns/03 madde 7); olanaklar listesinde önce 8-10 öne çıkan, sonra "tümünü göster" modalı
gelir (bkz. patterns/03 madde 6). İkincisi, coğrafi/kimlik hassasiyeti: rezervasyon öncesi
yaklaşık/bulanık konum, rezervasyon sonrası tam adres (bkz. patterns/03 madde 11); bu aynı mantık
trust tarafında da geçerlidir, kimlik doğrulaması host/misafir ilişkisinin güven eşiğine göre
kademelidir (bkz. patterns/05 madde 3). Üçüncüsü, filtre/karar karmaşıklığı: hızlı erişim çipleri
önce, "tüm filtreler" paneli isteğe bağlı sonra (bkz. patterns/01 madde 7, 8). Ortak payda,
kullanıcıya baştan tüm karmaşıklığı yüklememek, ama her zaman bir "daha fazlasını iste" kapısı
bırakmaktır.

### 1.4. Web/mobil: anlamda parite, mekanikte kasıtlı farklılaşma

Hiçbir bölüm "web ve mobil aynı olsun" demiyor; tam tersi, aynı işlevin platforma özgü en doğal
etkileşim biçimiyle yeniden yorumlanması tekrar eden bir örüntü. Rezervasyon kutusu web'de sticky
bir panel, mobilde ince bir alt bar olur (bkz. patterns/03 madde 3, 4); filtre/tarih/misafir seçimi
web'de dropdown, mobilde bottom sheet olur (bkz. patterns/07 madde 7); fotoğraf gezinme web'de
hover+ok, mobilde swipe olur (bkz. patterns/02 madde 2, patterns/03 madde 2). Mobile-native
katmanda bu farklılaşma daha da derinleşir: biyometrik oturum açma, native harita SDK'sı + WebView
yedeği, native share sheet, haptic geri bildirim, hepsi "platformun kendi konvansiyonuna güven,
onu yeniden icat etme" ilkesinin uzantılarıdır (bkz. patterns/11 madde 2, 3, 5, 8, 14). Airbnb'nin
React Native'i benimseyip sonra terk etmesi bu ilkenin mimari kanıtıdır: platformlar arası
tutarlılık paylaşılan koddan değil, ortak bir tasarım dilinden (DLS) gelebiliyor
(bkz. patterns/11 madde 1, patterns/08 madde 5).

### 1.5. Güven sinyalleri küçük, ikincil, ama nicel ve denetlenebilir eşiklere dayalı

Superhost ve Guest Favorite rozetleri görsel olarak mütevazıdır (küçük bir madalyon, bir pill),
ama arkalarında somut, ölçülebilir eşikler durur: %90 yanıt oranı, %1 iptal oranı, 4.8+ puan gibi
(bkz. patterns/05 madde 1, 2). Rozetin gücü onun gösterişinden değil, düzenli aralıklarla yeniden
hesaplanmasından gelir, bu da onu "bir kere kazanılan sonsuz ayrıcalık" olmaktan çıkarıp "sürekli
hak edilmesi gereken statü" yapar. Aynı ilke kimlik doğrulama rozetinde (bkz. patterns/05 madde 3)
ve host yanıt oranı gösteriminde (bkz. patterns/05 madde 5) tekrarlanır. Kritik olan şudur: güven
sinyalleri gürültü değil, seyrek ve anlamlı tutulur, tıpkı sekme çubuğundaki bildirim
noktalarının sadece zaman-duyarlı içerikte kullanılıp keşif sekmelerinde hiç kullanılmaması gibi
(bkz. patterns/07 madde 11).

### 1.6. Erken doğrulama, hata sonrası kurtarmadan daha güçlüdür

Takvim arayüzü, host'un minimum/maksimum konaklama kuralına uymayan tarihleri baştan tıklanamaz
gösterir; kullanıcı geçersiz bir aralığı seçip sonra bir hata mesajıyla geri gönderilmez
(bkz. patterns/04 madde 1). Aynı mantık form doğrulamasında (alan terk edildiğinde kontrol,
düzeltilirken anlık geri bildirim, bkz. patterns/09 madde 8) ve "sıfır sonuç" durumunda tekrar
eder: bir filtrenin 0 sonuca düşüreceğini seçim yapılmadan önce sinyallemek, sonradan "sonuç
bulunamadı" ekranıyla kurtarmaya çalışmaktan daha güçlü bir tasarım kararı olarak öne çıkıyor
(bkz. patterns/09 madde 3). "Önce önle" ilkesi her yerde "sonra kurtar" ilkesinden üstün tutuluyor.

### 1.7. Otomasyon + insan kontrolü dengesi: susma, kötü öneriden iyidir

Airbnb'nin öneri araçları iki farklı özerklik seviyesi sunar: Smart Pricing tam otomasyon (sistem
karar verir, host sadece sınır koyar), Price Tips yarı-otomasyon (sistem önerir, host karar verir)
(bkz. patterns/10 madde 10). Price Tips'in yeterli veri yoksa hiç görünmemesi kayda değer: sistem
gürültülü/çelişkili bir öneri sunmak yerine susmayı tercih eder. Aynı denge kimlik doğrulamada da
var, otomatik yüz eşleştirme başarısız olursa kullanıcı doğrudan reddedilmez, bir insan inceleme
ekibine düşer (bkz. patterns/11 madde 4); zamanlanmış otomatik host mesajlarında da host'a son
anda atlama/düzenleme hakkı tanınır (bkz. patterns/06 madde 6). Otomasyon hep bir insan
müdahale/geri çekilme kapısıyla birlikte tasarlanıyor.

### 1.8. Kişiselleştirme ve bildirim şeffaf, kapatılabilir bir hizmet olarak sunulur

Airbnb kişiselleştirmenin hangi sinyalleri (gezinme geçmişi, geçmiş rezervasyon, benzer kullanıcı
profilleri) kullandığını kendi yardım merkezinde açıkça listeler ve kullanıcıya bunu (kısmen)
kapatma anahtarı verir (bkz. patterns/10 madde 2, 4). Bildirimler de tek bir aç/kapa değil, kanal
(push/e-posta/SMS) x kategori (Mesajlar/Hatırlatmalar/Promosyon/Hesap) matrisi olarak sunulur;
bazı kategoriler (güvenlik, hesap) bilinçli olarak kapatılamaz bırakılır (bkz. patterns/06 madde
8). Kişiselleştirme "gizli gözetim" değil "beyan edilmiş, sınırları çizilmiş bir hizmet" olarak
konumlandırılıyor.

### 1.9. Hiçbir sayfa çıkmaz sokak olmamalı

İlan detay sayfasının altında benzer ilanlar carousel'i (bkz. patterns/03 madde 12), boş wishlist/
trips durumunda somut bir "şimdi ara" CTA'sı (bkz. patterns/09 madde 4, 6), sıfır sonuç ekranında
filtre gevşetme önerisi (bkz. patterns/09 madde 3), hepsi aynı ilkenin farklı yüzeylerdeki
tekrarıdır: kullanıcı bir dead-end'e geldiğinde platform onu her zaman bir sonraki somut adıma
yönlendirir, asla "burada hiçbir şey yok" diyip bırakmaz.

---

## 2. Uygulama checklist'i

Aşağıdaki liste, bir ürünü sıfırdan bu pattern'ler üzerine kurarken izlenebilecek kaba bir sıra.
Her madde ilgili pattern dosyasına referans veriyor; kendi ürününüzün ihtiyaçlarına göre
atlanabilir/yeniden sıralanabilir, bu bir zorunlu şablon değil bir kontrol listesidir.

**Keşif**
1. Ana ekranın en büyük, en görünür bileşenini arama/başlangıç eylemi yapın; hamburger menüyü
   birincil navigasyon aracı olarak kullanmayın (patterns/07 #1).
2. Çok parametreli aramayı (konum+tarih+kişi sayısı gibi) tek bir segmentli pil'e toplayın, her
   segmenti kendi mikro-etkileşimiyle (takvim, sayaç, arama kutusu) çözün (patterns/01 #1).
3. Net bir niyeti olmayan kullanıcılar için bir kategori/ilham şeridi ve "esnek arama" modu ekleyin
   (patterns/01 #2, #3).
4. Arama kutusu boşken bile popüler/kişiselleştirilmiş öneriler gösterin, "sıfır sorgu durumu"nu
   boş bırakmayın (patterns/01 #4).
5. Harita tabanlı bir keşifse, haritayı ayrı bir sekmeye gizlemeyin; web'de split-view, mobilde
   kolay erişilebilir bir toggle/bottom sheet kullanın; harita ve liste iki yönlü senkron olsun
   (patterns/01 #9, #10).

**Liste/Grid görünümü**
6. Kart şablonunu (görsel + rozet + aksiyon ikonu + başlık + fiyat) tüm kategori varyasyonlarında
   sabit sırada tutun (patterns/02 #1).
7. Kapak görselinin ikinci/üçüncü karesini kartın kendisinde gezilebilir yapın; gezinme
   kontrollerini kartın ana link alanından ayırın (stopPropagation) (patterns/02 #2).
8. Kaydet/favorile eylemi için tek başına bir ikon yeterli olmayabilir; ilk kullanımda kısa bir
   metin etiketi/coach-mark ekleyin (patterns/02 #4).
9. Ek ücret/vergi ekleyen bir üründe, nihai fiyata en yakın rakamı mümkün olduğunca erken (liste
   aşamasında) gösterin, checkout'a kadar saklamayın (patterns/02 #5).
10. Minimum review eşiği belirlemeden ortalama puan göstermeyin; eşik altını "Yeni" rozetiyle
    işaretleyin (patterns/02 #7).
11. Görsel yükleme 1-10 saniye arasıysa skeleton+shimmer kullanın; 1 saniyenin altında hiç
    kullanmayın (patterns/02 #9, patterns/09 #1).
12. Uzun listelerde infinite scroll'a girişmeden önce liste sanallaştırmasını (sadece görüneni
    render etme) çözün, aksi halde performans/geri-dönüş-konumu sorunları çıkar (patterns/02 #10).

**Detay sayfası**
13. İki katmanlı galeri kurun: kompakt önizleme + isteğe bağlı tam ekran derinlik; farklı en-boy
    oranlarını tek bir sabit orana zorlamayın (patterns/03 #1).
14. Tek bir birincil dönüşüm eylemi varsa (satın al/rezerve et), onu sayfa boyunca sticky tutun
    (web'de panel, mobilde alt bar); panele ikincil bilgi yığmayın (patterns/03 #3, #4).
15. Satıcı/host'u somutlaştırın: fotoğraf, deneyim süresi, yanıt hızı, nicel rozet
    (patterns/03 #5, patterns/05 #1).
16. Uzun özellik listelerini (olanaklar gibi) öne çıkan az sayıda + "tümünü gör" modeliyle sunun
    (patterns/03 #6).
17. Kullanıcı yazılı içeriğini (açıklama, yorum, biyografi) sabit bir uzunlukta kesip "daha fazla
    göster" ile genişletin (patterns/03 #7).
18. Çok boyutlu bir deneyimi tek puana indirmeyin; genel puanın yanına birkaç alt kategori puanı
    ekleyin ve bunun ortalama olmadığını belirtin (patterns/03 #8).
19. Review listesinde anlamlı bir varsayılan sıralama + yeniden sıralama + konu etiketleriyle
    filtreleme sunun (patterns/03 #9).
20. Sayfanın sonunu çıkmaz bırakmayın; benzer/ilgili içerik önerisi ekleyin (patterns/03 #12).

**Transaction/checkout**
21. Uygun olmayan tarih/miktar seçimlerini takvim/formda baştan görsel olarak pasifleştirin,
    hata mesajına bırakmayın (patterns/04 #1).
22. Farklı kısıtlama mantığına tabi katılımcı tiplerini (yetişkin/çocuk/evcil hayvan gibi) ayrı
    stepper satırlarına bölün (patterns/04 #2).
23. Arz tarafının onayına ihtiyaç olup olmadığını en baştan, buton metniyle ayırın ("hemen
    rezerve et" vs. "talep gönder"); bunu ödeme ekranına kadar belirsiz bırakmayın (patterns/04 #3).
24. Ek/zorunlu ücretleri kalem kalem gösterin, checkout'un son adımına saklamayın (patterns/04 #4).
25. Yüksek tutarlı/uzun-vadeli satın almalarda bölünmüş ödeme seçeneği (şimdi az, kalan sonra)
    sunmayı değerlendirin, ama koşullarını (minimum tutar, minimum süre) şeffaf tutun
    (patterns/04 #5).
26. Ödemeden hemen önce bir "gözden geçir ve onayla" özet ekranı koyun; bu ekranı kısa tutun
    (patterns/04 #7).
27. Talep bekleme durumunu net bir "beklemede" göstergesiyle sunun ve süreye bir zaman kutusu
    (24 saat gibi) koyun; ücretlendirmeyi onaydan sonraya erteleyin (patterns/04 #9).
28. Ödeme sonrası kalıcı bir "siparişlerim/rezervasyonlarım" merkezine yönlendirin, sadece
    "başarılı" mesajıyla bırakmayın (patterns/04 #10).
29. İptal/iade koşullarını tek seferlik değil, karar öncesi + ödeme anı + iptal öncesi önizleme
    olmak üzere üç noktada tekrarlayın (patterns/04 #11).

**Güven katmanı**
30. Satıcı/hizmet sağlayıcı rozetlerini somut, denetlenebilir, düzenli aralıklarla yeniden
    hesaplanan nicel eşiklere bağlayın (patterns/05 #1, #2).
31. Kimlik doğrulamasını yüksek riskli akışlarda isteğe bağlı değil zorunlu bir kapı yapın;
    toplanan hassas veriyi (kimlik, selfie) diğer kullanıcılara göstermeyeceğinizi açıkça belirtin
    (patterns/05 #3).
32. Review'ı sadece doğrulanmış işlem sahiplerine açın; satıcıya herkese açık, tek seferlik bir
    yanıt hakkı tanıyın (patterns/05 #4).
33. Platform dışı ödeme/iletişim riskini soyut bir uyarı yerine somut dolandırıcılık işaretleri
    listesiyle anlatın; platform dışına çıkmanın kaybettireceği korumayı açıkça belirtin
    (patterns/05 #7).
34. Fiziksel bir mekân/üçüncü taraf mülkü söz konusuysa gözetim cihazı ifşasını zorunlu,
    yapılandırılmış bir alanda toplayın (patterns/05 #9).
35. Parasal anlaşmazlık riski taşıyan taleplerin genel mesajlaşmadan ayrı, izlenebilir bir kanaldan
    geçmesini sağlayın (patterns/05 #11).

**Mesajlaşma/post-transaction**
36. Rezervasyon/işlem öncesi bir soru-cevap kanalı açın; bunu taahhütten önceki belirsizliği
    azaltmak için kullanın (patterns/06 #1).
37. Mesajlaşmaya sabit bir hız sınırı koyun (saatlik/günlük tavan); düzenleme/geri çekme
    penceresini kısa tutun (patterns/06 #2).
38. Birden fazla tarafın koordine olması gerekiyorsa mesajlaşmayı kişi kimliği yerine işlem/
    rezervasyon kimliği etrafında gruplayın (patterns/06 #3).
39. Dosya/medya paylaşımını işlem onayından sonraki döneme kısıtlayın (patterns/06 #4).
40. Tekrar eden, zamanlaması kritik iletişimi (hatırlatma, yönerge) otomatikleştirin, ama otomatik
    mesajları thread içinde açıkça etiketleyin (patterns/06 #6).
41. Yanıt hızı için resmi bir üst sınır tanımlayın (ör. 24 saat), ama beklentiyi olumlu
    istatistiklerle ("çoğu kullanıcı çok daha hızlı yanıtlıyor") kalibre edin (patterns/06 #10).

**Navigasyon kabuğu**
42. Birincil (sık, her oturumda tekrarlanan) eylemleri her zaman görünür tutun; ikincil (seyrek,
    hesap/ayar) işlevleri bir dropdown/menüde toplayın (patterns/07 #4).
43. Beş veya daha az üst düzey bölümünüz varsa sabit bir alt tab bar kullanın, ikon+etiket
    kombinasyonunu tercih edin (patterns/07 #5).
44. Kullanıcı iki büyük ölçüde örtüşmeyen role sahipse (alıcı/satıcı gibi), bunları ayrı "mod"lar
    olarak tanımlayıp aralarında bilinçli bir geçiş eylemi isteyin (patterns/07 #6).
45. Kısa, tek-amaçlı seçimleri (filtre, tarih, miktar) bottom sheet'te tutun; uzun/gezinilebilir
    içeriği tam ekran sayfaya taşıyın (patterns/07 #7).
46. Bildirimden/e-postadan gelen tıklamayı jenerik ana ekrana değil, doğrudan ilgili ekrana
    yönlendirin (deep linking) (patterns/07 #10).

**Görsel sistem**
47. Marka rengini sadece birincil eylem/aktif durum/kaydetme gibi az sayıda noktada kullanın,
    dekoratif olarak kullanmayın (patterns/08 #2).
48. Görsel-ağırlıklı bir üründe arayüz kromunu bilinçli nötr tutup en büyük görsel alanı gerçek
    ürün fotoğrafına ayırın (patterns/08 #7).
49. Bir tasarım sistemi kurarken önce adlandırılmış, tartışılabilir ilkeler (Unified/Universal/
    Iconic/Conversational tarzı) tanımlayın, sonra bileşen kütüphanesine geçin (patterns/08 #5).
50. Bir ekranda tek bir dolu/vurgulu buton bırakın, geri kalan eylemleri metin/outline'a indirin
    (patterns/08 #10).

**Durumlar/feedback**
51. Form hatalarını alan terk edildiğinde (blur) tetikleyin, düzeltme anında (tuş vuruşunda)
    kaldırın; erken/anlık uyarı vermeyin (patterns/09 #8).
52. Düşük riskli/geri alınabilir eylemler için kalıcı bir onay ekranı değil, kısa bir toast/
    snackbar kullanın (patterns/09 #10).
53. İki uçlu (başlangıç/bitiş) bir seçicide her iki ucun da simetrik bir geçersiz-durum davranışı
    olsun; sadece bir yönü ele alıp diğerini belirsiz bırakmayın (patterns/09 #9).
54. Boş durumları (wishlist, gelen kutusu, geçmiş) jenerik "hiçbir şey yok" yazısıyla değil,
    özelliğin değerini ima eden bir görsel + somut CTA ile tasarlayın (patterns/09 #4, #6).
55. "Gerçekten boş" ile "henüz yüklenmedi/senkronize olmadı" durumlarını görsel olarak kesin
    biçimde ayırın (patterns/09 #5).

**Kişiselleştirme**
56. Kişiselleştirme sinyallerini uzun-vadeli eğilim ve kısa-vadeli/güncel niyet olarak ayrı
    modelleyin, sadece birine güvenmeyin (patterns/10 #2).
57. Kaydet/favorile gibi düşük-sürtünmeli eylemleri örtük bir tercih sinyali olarak öneri motoruna
    geri besleyin (patterns/10 #4).
58. Hangi sinyallerin kullanıldığını kullanıcıya açık bir sayfada listeleyin ve en azından
    kısmi bir kapatma seçeneği sunun (patterns/10 #2, #4).
59. Fiyatı otomatik değiştiren bir araç (tam otomasyon) ile sadece öneren bir araç (yarı-otomasyon)
    ikisini yan yana sunmayı değerlendirin; yetersiz veri varsa öneriyi hiç göstermeyin
    (patterns/10 #10).

**Mobil-native katman**
60. Sık kullanılan bir uygulamada parola yerine cihazın biyometrik donanımına bağlı oturum açma
    sunun; bunu "güvenilir cihaz" kavramıyla sınırlı tutup yeni cihazda güçlü kanala (SMS/PIN)
    dönün (patterns/11 #2, #3).
61. Harita SDK'sına tek bir sağlayıcıda kilitlenmeyin; performanslı ama evrensel olmayan bir
    native SDK + daha yavaş ama her cihazda çalışan bir yedek (WebView gibi) kurun
    (patterns/11 #5).
62. Kimlik/durum doğrulaması gereken akışlarda galeriden dosya seçmeyi kapatıp sadece canlı kamera
    çekimini kabul edin (patterns/11 #4, #9).
63. Push bildirim izni gibi "tek atışlık" sistem promptlarını uygulama açılışında değil, kullanıcı
    faydayı deneyimlediği bir bağlamda tetikleyin (patterns/11 #10).
64. Mağaza değerlendirme istemini olumlu bir deneyimin hemen ardından tetikleyin, hata/şikayet
    anından hemen sonra asla göstermeyin (patterns/11 #13).
65. Tinder tarzı ikili swipe-karar modelini yalnızca düşük riskli, tek değişkenli kararlar için
    düşünün; çok değişkenli, yüksek riskli kararlarda (konaklama, araç, iş ilanı gibi) sektör
    genelinde başarısız olduğu gözlemleniyor (patterns/11 #7).

---

## 3. Nerede Airbnb'yi kopyalamamak lazım

Bu proje boyunca tekrar eden bir uyarı şuydu: Airbnb sürekli A/B test yapan, sık sık sessizce
değişen canlı bir üründür; burada "pattern" demek "şu an birebir böyle" değil "tekrar gözlemlenen,
yerleşik" demektir. Bu temkin, hangi pattern'lerin kopyalanmaya değer olduğu kadar, hangilerinin
Airbnb'ye özgü koşullardan doğduğu ve başka bir bağlama taşınırsa yanlış olacağı sorusu için de
geçerli.

**Güven sinyali ağırlığı, düşük-riskli ürünlerde gürültüye dönüşür.** Airbnb'nin Superhost/Guest
Favorite/kimlik doğrulama/AirCover yığını, iki yabancının parayı önden verip fiziksel bir mekânı
paylaşmasını gerektiren, yüksek maliyetli ve geri alınamaz bir işlemin ürünüdür (bkz. patterns/05).
Bir görev yönetimi aracı, bir düşük-bedelli e-ticaret sitesi ya da içerik tüketim uygulaması aynı
yoğunlukta rozet/doğrulama/sigorta katmanı kurarsa, bu güven üretmez, sadece kullanıcının önemsiz
gördüğü işlemi gereksiz ağırlaştırır. Güven sinyalinin dozu, işlemin gerçek riskiyle orantılı
olmalı; Airbnb'nin dozu kendi bağlamına göre kalibre edilmiştir, evrensel bir "doğru miktar" değil.

**Review/puan enflasyonu, kopyalanacak bir pattern değil, bir uyarı hikâyesidir.** Airbnb'nin
kendi review sistemi, puanları hem host performans eşiklerine (Superhost 4.8+) hem arama
sıralamasına doğrudan bağladığı için 4 yıldızı fiilen bir "başarısızlık" haline getirmiş, bu da
puanların skalanın en üstünde sıkışmasına (yaygın gözleme göre listelerin çoğu 4.5 üzerinde)
yol açmıştır (bkz. patterns/05 madde 12). Bir puanlama sistemi kurarken bu mimari hatayı
tekrarlamamak gerekir: puanı doğrudan bir ceza/eşik mekanizmasına bağlamak, puanın ayırt edici
gücünü baştan öldürür. Airbnb'nin kendi çözümü (kategori kırılımı + üst-dilim rozeti) bir tamir
girişimidir, kaynaktaki hatanın kendisi değil.

**Bölünmüş ödeme ve "review and pay" gibi ağır checkout mekanikleri, düşük tutarlı/anlık işlemlerde
gereksiz sürtünmedir.** "Şimdi az öde, kalanını sonra öde" seçeneği yüksek tutarlı, check-in tarihi
uzak rezervasyonlar için tasarlanmıştır (bkz. patterns/04 madde 5); bir kahve siparişi ya da düşük
bedelli bir dijital ürün için bu mekaniği taşımak, karar sürecine gereksiz bir adım ekler.
Benzer şekilde çok adımlı "gözden geçir ve onayla" ekranı yüksek riskli/çok parametreli bir kararda
anlamlıdır (bkz. patterns/04 madde 7); tek tıkla tamamlanan basit bir işlemde bu ekran sadece
sürtünme katar.

**Instant Book / Request to Book ikiliği, sadece arz tarafının gerçekten manuel onay ihtiyacı
olan pazaryerlerinde anlamlıdır.** Bu ayrım (bkz. patterns/04 madde 3) host'un takvimini/riskini
kontrol etme ihtiyacından doğar; arzın standart, stok bazlı ve host onayına ihtiyaç duymadığı bir
üründe (ör. bir e-ticaret sepeti) bu dallanmayı taklit etmek gereksiz bir karmaşıklıktır.

**Mesaj hız sınırlaması (24 saatte 25 mesaj) ve platform-dışı iletişim uyarısı, yabancılar arası
işlem riski olan pazaryerlerine özgüdür.** Bu kısıtlar (bkz. patterns/06 madde 2, patterns/05
madde 7) taciz/dolandırıcılık riskine karşı tasarlanmıştır; kullanıcıların zaten birbirini tanıdığı
veya düşük riskli bir bağlamda (ör. bir ekip içi mesajlaşma aracı) bu sert limitleri kopyalamak,
gerçek bir ihtiyaca değil, ithal edilmiş bir korkuya hizmet eder.

**"Kategoriler" ızgarasının 2025'te büyük ölçüde kaldırılması (bkz. patterns/10 madde 1),** büyük
bir lansmanla tanıtılan bir keşif özelliğinin bile şirketin iş önceliği değiştiğinde (burada yeni
gelir kollarına, Experiences/Services'e ana sayfa alanı açma ihtiyacı) sessizce geri çekilebileceğinin
somut kanıtıdır. Buradaki ders özelliğin kendisi değil, "şu an tam olarak böyle çalışıyor" diye
kopyalanacak bir referansın yayın tarihi belirtilmeden asla kesin bir gerçek gibi sunulmaması
gerektiğidir; bu doküman genelinde defalarca tekrarlanan bir uyarının en somut örneğidir.

**React Native'i benimseyip terk etme hikâyesi (bkz. patterns/11 madde 1),** cross-platform bir
framework'ün "iki kere değil bir kere yaz" vaadinin, ölçek büyüdükçe "üç kod tabanını (native iOS,
native Android, RN köprüsü) bakımda tutma" riskine dönüşebileceğinin bir uyarısıdır; küçük bir
ekip için bu hâlâ doğru bir başlangıç noktası olabilir, "Airbnb sonunda terk etti, o zaman hiç
kullanmayalım" diye okunmamalı ama "ne kadarı gerçekten paylaşılıyor" sorusunu erken ve düzenli
sormak gerektiği şeklinde okunmalı.

**Dark mode'un native uygulamalarda resmi olarak sunulmaması (bkz. patterns/08 madde 11),**
bir "pattern" değil muhtemelen bir eksikliktir; kullanıcı forumlarında yıllardır tekrarlanan
talep, bunun bilinçli bir tasarım tercihinden çok bir öncelik/karmaşıklık sorunu (fotoğraf
kontrastının koyu zeminde yeniden düşünülmesi gerekliliği) olabileceğine işaret ediyor. Bu, başka
bir ürünün "Airbnb da yapmıyor, biz de yapmayalım" diye referans göstermemesi gereken bir boşluk.

**Tinder-tarzı swipe-karar modelinin Airbnb'de bulunmaması (bkz. patterns/11 madde 7)** kendi
başına öğretici: sektördeki benzer denemeler (kiralama uygulamalarında ilan swipe'ı) kullanıcı
testlerinde başarısız olmuş, çünkü yüksek riskli/çok değişkenli bir karar tek bir fotoğrafa
bakarak ikili bir "beğen/geç" kararına indirgenemiyor. Bu pattern'in kopyalanmaması gerektiği
durumun tersi de doğru: düşük riskli, tek değişkenli kararlarda (bir haberi geç, bir fotoğrafı
beğen) swipe modeli gayet işe yarayabilir; bağlamı karıştırmamak gerekir.

---

## 4. Doğrulama durumu özeti

Aşağıdaki tablo her bölümün kendi "genel gözlem: kaynak kalitesi özeti" bölümünden ve
`PROGRESS.md`'deki notlardan derlenmiştir. "Güçlü/birincil" sütunu, Airbnb'nin kendi yardım
merkezi, mühendislik blogu veya açık kaynak deposundan doğrudan fetch edilip doğrulanan madde
sayısını gösterir; bu doğrulama Airbnb'nin ürününün *şu an* böyle çalıştığını değil, bir noktada
Airbnb'nin kendi ağzından böyle tarif edildiğini garanti eder.

| # | Bölüm | Toplam madde | Güçlü/birincil | Kısmen doğrulanan | Doğrulanmamış/eğitim verisi | Not |
|---|-------|:---:|:---:|:---:|:---:|-------|
| 01 | Keşif & Arama | 12 | 3 | ~6 | ~3 | Baymard split-view ve Airbnb Engineering blogu güçlü kaynaklardı |
| 02 | İlan Kartı & Grid | 12 | 3 | ~7 | ~2 | Fiyat toggle, wishlist akışı, infinity.js birincil kaynaklıydı |
| 03 | İlan Detay | 13 | 3 | 9 | 1 | Review sistemi ve konum reveal Airbnb yardım merkeziyle güçlü doğrulandı |
| 04 | Rezervasyon/Checkout | 11 | 5 | 5 | 1 | Bölümdeki en güçlü kaynaklı gruplardan biri, 10 yardım merkezi sayfası fiilen fetch edildi |
| 05 | Güven & Güvenlik | 12 | 6 | 5 | 1 | Projenin en güçlü kaynaklı bölümlerinden biri; profil eksiksizliği zayıf kaldı |
| 06 | Mesajlaşma & Bildirimler | 12 | 6 | 5 | 1 | 8 yardım merkezi sayfası + 2 mühendislik yazısı fiilen fetch edildi |
| 07 | Navigasyon & IA | 12 | 2 | 6 | 5 | Projenin en zayıf birincil kaynaklı bölümlerinden biri |
| 08 | Görsel Tasarım Sistemi | 12 | 4 | 4 | 4 | Renk/spacing/radius/buton piksel detayları resmi kaynaksız, üçüncü taraf token sitelerinden |
| 09 | Durumlar & Feedback | 14 | 3 (+4 genel endüstri) | ~2 | ~5 | Airbnb'ye özgü olmayan NN/g ve Baymard kaynakları güçlü ama şirkete özgü değil |
| 10 | Kişiselleştirme & Öneri | 11 | 6 | 2 | 3 | En riskli alan: Airbnb sık ve sessizce değiştiriyor (Kategoriler 2025'te kaldırıldı) |
| 11 | Mobile-Only Pattern'ler | 14 | 5 | 4 | 6 | Native detaylar (widget, haptic, share sheet) resmi kaynaklarda hiç belgelenmiyor |

Genel okuma: 04, 05 ve 06 numaralı bölümler (rezervasyon/checkout, güven/güvenlik, mesajlaşma)
Airbnb'nin kendi yardım merkezinde en çok doğrudan belgelenen alanlar olduğu için bu projenin en
güvenilir kısımları. 07 (navigasyon/IA), 08 (görsel tasarım sistemi) ve 09 (durumlar/feedback) ise
en zayıf birincil kaynak tabanına sahip; bunun nedeni bu konuların Airbnb'nin resmi dokümantasyon
kanallarında (yardım merkezi, mühendislik blogu) nadiren doğrudan ele alınması, bu yüzden birçok
piksel-seviyesi/etkileşim detayı üçüncü taraf "design token extractor" siteleri, topluluk forumları
veya genel endüstri araştırmasından (Baymard, NN/g gibi, Airbnb'ye özgü olmayan ama güçlü otorite
kaynaklardan) geliyor. 10 numaralı bölüm (kişiselleştirme) özel bir uyarı gerektiriyor: burada hem
en taze/en güçlü kaynaklar (2026 tarihli bir Airbnb Engineering yazısı dahil) hem de en riskli
alan (hızlı ve sessiz özellik değişimleri, çoğu kez erişilemeyen news.airbnb.com sayfaları) bir
arada bulunuyor.

Ortak, tekrarlayan bir kısıt: `news.airbnb.com` ve `airbnb.com/resources/*` gibi basın/pazarlama
sayfaları araştırma boyunca sistematik olarak 403 Forbidden ile engellendi; bu yüzden en güncel
özellik duyuruları (Kategoriler'in kaldırılması, 2025-2026 AI özellikleri, toplam fiyat gösterimi
haberleri) çoğunlukla yalnızca WebSearch özetleriyle sınırlı kaldı, birincil metinleri doğrudan
okunamadı. Bu doküman seti bu yüzden "Airbnb şu an ekranında birebir böyle görünüyor" diye değil,
"tekrar tekrar gözlemlenen, kısmen ya da tam belgelenen yerleşik pattern" çerçevesiyle okunmalı;
her maddenin kendi dosyasındaki "kaynak/güven notu" bölümü, o maddeye ne kadar güvenilebileceğinin
tek gerçek referansıdır.
