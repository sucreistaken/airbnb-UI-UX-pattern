# 1. Keşif & Arama (Discovery & Search)

Bu bölüm Airbnb'nin arama girişi, kategori/keşif katmanı, filtreler, harita entegrasyonu ve
sonuç durumlarını kapsar. Airbnb sürekli A/B test yapan canlı bir ürün olduğu için burada
"şu an tam olarak böyle" değil, tekrar tekrar gözlemlenmiş/belgelenmiş yerleşik pattern'ler
anlatılıyor. Her madde kaynağıyla birlikte veriliyor; kaynağı fiilen açıp okuyamadığım
gözlemler açıkça "doğrulanmadı, eğitim verisinden" diye işaretlendi.

Araştırma sırasında fiilen açılıp okunan kaynaklar: Baymard Institute (split-view araştırması,
Airbnb vaka çalışması sayfası, autocomplete örnek sayfası), Google Design'ın Airbnb yazısı,
Airbnb Engineering (Medium/airbnb-engineering) blog yazısı "Recommending travel destinations
to help users explore", ve harita sıralaması üzerine üçüncü taraf bir teknik özet (techscoop
substack). Bunların dışındaki bazı ikincil Medium/UX Collective yazıları sadece arama
sonucu snippet'i üzerinden görüldü, tam metin fetch edilemedi (redirect/404/403 hataları) -
bu durumlar ilgili maddede ayrıca belirtildi.

---

## 1. Arama çubuğu / giriş noktası (pill search bar)

**Ne olduğu:** Nereye (Where), Ne zaman (Check-in/Check-out), Kim (Who/guest sayısı) gibi
farklı arama parametrelerini tek bir hap (pill) şeklinde, yan yana dizilmiş segmentler halinde
gösteren birleşik arama çubuğu. Segmentlerden birine dokunmak/tıklamak sadece o alana özel bir
alt-panel (tarih takvimi, misafir sayacı, konum arama kutusu) açar; tüm bar tek bir "arama"
eylemi gibi hissettirir.

**Nerede görülür:** İkisi de. Web'de sayfa üstünde yatay, kompakt bir bar; mobilde tam ekran
genişliğinde, dokunulunca tam ekran arama akışına geçen bir giriş noktası olarak.

**UX gerekçesi:** Çok adımlı/çok parametreli bir sorguyu (konum + tarih + kişi sayısı) tek bir
görsel birim haline getirerek bilişsel yükü azaltır; kullanıcı "3 ayrı form alanı doldurmak"
yerine "tek bir arama kutusuyla konuşuyormuş" gibi hisseder. Segmentli yapı, her adımı kendi
bağlamında (takvim, sayaç, otomatik tamamlama) en uygun UI ile çözmeyi mümkün kılar.

**Airbnb dışı bir uygulamaya uyarlama notu:** Çok parametreli arama gerektiren herhangi bir
ürün (uçuş, etkinlik bileti, restoran rezervasyonu, araç kiralama) aynı "segmentli tek bar"
yaklaşımını kullanabilir: parametreleri ayrı formlara dağıtmak yerine tek bir yatay/pill bar
üzerinde sıralamak, her segmenti kendi mikro-etkileşimiyle (takvim, sayaç, arama kutusu)
açmak.

**Kaynak / güven notu:** Segmentlerin "Where / Check in / Check out / Who" şeklinde adlandığı
ve pill biçiminin bir tasarım sistemi bileşeni olarak ele alındığı, WebSearch sonucu olarak
görülen bir ikincil kaynaktan (Superdesign blog, airbnb tasarım sistemi dökümü) geliyor; bu
sayfayı tam metin fetch etmedim, sadece arama snippet'i üzerinden okudum. Genel yapı (tek bar,
segmentli alanlar) yaygın olarak bilinen ve birden fazla ikincil kaynakta tutarlı bir gözlem,
ama birincil Airbnb kaynağından doğrulanmadı → **doğrulanmadı, eğitim verisinden + ikincil
kaynak snippet'i**.

---

## 2. Kategori sekmeleri / "Icons" satırı (kategori bazlı keşif)

**Ne olduğu:** Arama çubuğunun hemen altında yatay kaydırılabilir bir ikon+etiket şeridi
("Icons", "Amazing views", "Tiny homes", "OMG!", "Trending" vb.); bunlardan birine dokunmak
konum/tarih girmeden, doğrudan o kategoriye ait ilanları filtreler. Seçili kategori altı çizili
(underline) bir aktif durum göstergesiyle vurgulanır.

**Nerede görülür:** İkisi de; web'de anasayfa hemen altında, mobilde ana sekmenin üstünde
yatay scrollable strip olarak.

**UX gerekçesi:** Kullanıcının net bir arama niyeti olmadan (henüz nereye gideceğini bilmeyen,
"ilham arayan" ziyaretçi) keşfe başlamasını sağlar; klasik "önce konum yaz" adımını atlayarak
paradox-of-choice etkisini azaltır. İkon şeridi az yer kaplarken çok sayıda giriş noktası sunar
ve "aktif altı çizili sekme" göstergesi sistemi sade/dikkat dağıtmayan tutar.

**Airbnb dışı bir uygulamaya uyarlama notu:** E-ticaret, içerik keşfi veya herhangi bir
marketplace uygulamasında, kullanıcı henüz ne aradığını tam bilmiyorsa arama kutusunun hemen
altına "ilham/kategori" şeridi eklemek dönüşüm öncesi terk oranını azaltabilir. Özellikle yeni
kullanıcılar veya "browsing" modundaki kullanıcılar için arama kutusuna alternatif bir giriş
noktası olarak değerlidir.

**Kaynak / güven notu:** "Icons" kategorisinin varlığı ve kategori bazlı keşfin genel mantığı
(paradoks-of-choice azaltma, curated categories) WebSearch sonuçlarında birden fazla ikincil
kaynakta (case study/teardown yazıları) tutarlı şekilde geçiyor, ancak airbnb.design'dan
birincil kaynak fetch edilemedi (arama sonuçları esasen Medium/ikincil kaynaklardı). "Icons"
kategorisinde wishlist kalbi yerine paylaşım butonu gösterildiği iddiası da yalnızca bir
ikincil kaynak snippet'inden geliyor → **kısmen doğrulanmadı, ikincil kaynak snippet'i +
eğitim verisi**.

---

## 3. Esnek arama: "I'm Flexible" / Flexible Dates / Flexible Destinations

**Ne olduğu:** Kullanıcı net bir tarih veya konum belirlemeden arama yapabildiği üç ayrı
esnek-arama modu: (a) **Flexible Dates** - "hafta sonu", "bir haftalık", "aylık" gibi kaba
zaman aralıklarıyla arama; (b) **Flexible Destinations/"I'm Flexible"** - belirli bir yer
yerine "tekne", "tiny home", "kubbe ev" gibi benzersiz ilan kategorilerine göre, kullanıcının
konumuna en yakından başlayarak öneri listesi sunma.

**Nerede görülür:** İkisi de; web ve mobil uygulamada arama akışının bir parçası olarak.

**UX gerekçesi:** Kullanıcıların önemli bir kısmı "belirli bir yere gitmek" değil "ilginç bir
yerde tatil yapmak" istiyor; bu modlar zorunlu (destinasyon/tarih girmeden arama yapılamaz)
kısıtı kaldırarak keşif odaklı kullanıcıları da sisteme dahil ediyor. Airbnb'nin kendi haber
duyurularına göre bu özellik 500 milyonun üzerinde esnek arama üretmiş ve rezervasyonların
yaklaşık 1/20'si esnek arama üzerinden yapılmış; ayrıca talebi popüler turistik bölgelerden
dağıtarak arz-talep dengesine de katkı sağladığı belirtiliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Zorunlu parametre girişi olan her arama akışında
("belirli tarih girmeden arama yapamazsın" gibi) bir "esnek mod" sunmak, kullanıcı niyetinin
net olmadığı erken keşif aşamasındaki terk oranını azaltabilir. Örn. bir etkinlik/konser
uygulamasında "bu ay içinde herhangi bir gün" veya "bana yakın herhangi bir yer" modu.

**Kaynak / güven notu:** Kısmen doğrulandı. "500 milyon esnek arama" ve "1/20 rezervasyon"
rakamları WebSearch sonuçlarında Airbnb'nin kendi haber sitesine (news.airbnb.com) atıfla
görüldü, ancak ilgili news.airbnb.com sayfasını doğrudan fetch etmeye çalıştığımda 403
(erişim engeli) hatası aldım - yani rakamları birincil kaynaktan kendim doğrulayamadım, sadece
arama motoru özetinden gördüm. Özelliğin var olduğu ve nasıl çalıştığı (flexible dates/
destinations mekaniği) WebSearch snippet'lerinde tutarlı → **kısmen doğrulanmadı: özelliğin
varlığı makul güvenilir, sayısal istatistikler doğrudan doğrulanamadı**.

---

## 4. Arama kutusuna dokununca gelen öneriler (autosuggest / önceden öneri)

**Ne olduğu:** Kullanıcı arama kutusuna henüz hiçbir şey yazmadan, sadece kutuya
dokunduğunda/tıkladığında bile birden fazla şehir/bölge önerisinin listelenmesi (ör. kullanıcı
konumuna yakın popüler destinasyonlar, geçmiş davranışa göre tahmin edilen ilgi alanları).

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb'nin kendi mühendislik ekibinin yazdığı blog yazısına göre, net bir
destinasyonu olmayan "keşif amaçlı" kullanıcılar platformu daha az sıklıkla ziyaret ediyor ve
yakın gelecekte rezervasyon yapma olasılıkları daha düşük; bu kullanıcılar genelde "Fransa"
gibi geniş bir bölge yazıp ilham arıyor. Autosuggest, bu kullanıcılara arama kutusuna
dokunur dokunmaz somut şehir önerileri sunarak erken karar verme sürtünmesini azaltıyor.
Yazıya göre bu özellik özellikle İngilizce konuşulmayan bölgelerde test edilmiş ve rezervasyon
artışı sağlamış.

**Airbnb dışı bir uygulamaya uyarlama notu:** Arama kutusu boşken bile "önerilen" veya
"trend olan" seçenekleri göstermek, özellikle yeni/keşif modundaki kullanıcılar için faydalı
bir kalıp. Herhangi bir arama/keşif deneyiminde (iş ilanları, e-ticaret, seyahat) "sıfır sorgu
durumu" (zero-query state) boş bırakılmamalı; en azından popüler/kişiselleştirilmiş öneriler
konulmalı.

**Kaynak / güven notu:** **Doğrulandı** - Airbnb Engineering blog yazısı "Recommending travel
destinations to help users explore" (medium.com/airbnb-engineering) fiilen fetch edilip
okundu: https://medium.com/airbnb-engineering/recommending-travel-destinations-to-help-users-explore-5fa7a81654fb
İçerik: "When users click on the search bar, multiple city recommendations are presented"
ifadesi doğrudan bu kaynakta geçiyor; transformer tabanlı bir öneri modeli, kullanıcı
davranışını (arama/görüntüleme/rezervasyon geçmişi) dil modellemesindeki gibi token dizisi
olarak işliyor.

---

## 5. Yazarken otomatik tamamlama (autocomplete)

**Ne olduğu:** Kullanıcı destinasyon kutusuna yazmaya başladığında anlık olarak eşleşen
yer/şehir/bölge önerilerinin bir açılır liste halinde gösterilmesi; klavye ok tuşlarıyla
gezinme ve Enter ile seçim desteklenir.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Kullanıcının doğru yazım/terminolojiyi bilmesine gerek kalmadan doğru
sonuca ulaşmasını sağlar, yazım hatalarını tolere eder ve arama kapsamını (şehir mi, mahalle
mi, ülke mi) netleştirmeye yardımcı olur - bu genel autocomplete UX prensibi arama literatüründe
(Baymard, NN/g tarzı kaynaklarda) yaygın olarak belgelenir.

**Airbnb dışı bir uygulamaya uyarlama notu:** Konum/isim/kategori bazlı her arama kutusunda
autocomplete standart bir gereklilik olarak düşünülmeli; özellikle çok-dilli/global kullanıcı
tabanı olan ürünlerde yazım hatalarını tolere etmek ve doğru kapsamı (şehir/bölge/POI)
netleştirmek kritik.

**Kaynak / güven notu:** Kısmen doğrulandı. Baymard Institute'ın "Autocomplete Suggestions"
örnek galerisinde Airbnb'ye özel bir sayfa olduğu doğrulandı (https://baymard.com/ecommerce-design-examples/34-autocomplete-suggestions/10344-airbnb
- fetch edildi), ancak sayfanın asıl içeriği (dropdown'da tam olarak neyin göründüğü, sıralama
mantığı) ücretli/kilit içerik olduğu için görülemedi, sadece "örnek #841/1130" başlığı
okunabildi. Genel autocomplete davranışı (ok tuşlarıyla gezinme, Enter ile seçim) WebSearch
özetlerinden geldi, birincil kaynaktan tam doğrulanamadı → **kısmen doğrulanmadı**.

---

## 6. Son aramalar (recent searches)

**Ne olduğu:** Kullanıcının daha önce yaptığı aramaların (konum + tarih kombinasyonu) arama
kutusunun altında veya anasayfada kısayol olarak tekrar sunulması; bir dokunuşla önceki
sorguya geri dönme imkânı.

**Nerede görülür:** İkisi de (varsayım - bkz. güven notu).

**UX gerekçesi (genel prensip olarak):** Kullanıcıların çoğu bir rezervasyon kararını tek
oturumda vermez; birden fazla ziyarette aynı aramaya dönerler. Son aramaları öne çıkarmak
tekrar yazma/filtreleme maliyetini ortadan kaldırır ve çok-oturumlu karar verme sürecini
destekler.

**Airbnb dışı bir uygulamaya uyarlama notu:** Karar döngüsü uzun olan her üründe (emlak,
araba, iş ilanı, seyahat) son aramaları anasayfaya/arama girişine yakın bir yere koymak,
kullanıcının kaldığı yerden devam etmesini kolaylaştırır.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Airbnb yardım merkezinde
"son aramaları temizleme" ile ilgili video/içerik başlıklarının var olduğunu WebSearch'te
gördüm (bu, özelliğin bir şekilde var olduğuna işaret ediyor), ancak fetch ettiğim
airbnb.com/help/article/3893 sayfası bu konuyla ilgili değil çıktı (geçmiş seyahatleri
paylaşma hakkındaydı) ve özelliğin tam olarak nerede/nasıl göründüğünü (anasayfada mı, arama
kutusu altında mı) doğrulayan bir birincil kaynağa ulaşamadım. Bu maddeyi büyük ölçüde genel
mobil/arama UX bilgisi ve dolaylı işaretlerden yazdım - kesin Airbnb detayı olarak
okunmamalı.

---

## 7. Hızlı erişim filtre çubuğu (price / dates / property type toggle çipleri)

**Ne olduğu:** Sonuç sayfasının üstünde, en sık kullanılan filtrelerin (fiyat aralığı,
tarihler, ilan tipi) ayrı ayrı buton/çip olarak sabit durduğu yatay bir şerit; "Filters"
butonu ise geri kalan her şeyi kapsayan tam panele açılır.

**Nerede görülür:** İkisi de; web'de sonuç sayfası üstünde yatay bar, mobilde yine yatay
scrollable çip şeridi.

**UX gerekçesi:** Baymard Institute'ın büyük ölçekli seyahat/konaklama testlerinde
katılımcıların uzun sonuç listelerini daraltmak için rutin olarak popüler filtreleri
başlangıç noktası olarak kullandığı gözlemlenmiş. En sık kullanılan filtreleri öne çıkarmak,
kullanıcının "her filtreyi görmek için panel açması" gerekliliğini ortadan kaldırıp en yaygın
daraltma eylemlerini tek dokunuşa indiriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Filtre sayısı fazla olan her listeleme/arama
ürününde (e-ticaret, iş ilanı, emlak) en sık kullanılan 3-5 filtreyi ayrı çip/buton olarak öne
çıkarıp, geri kalanını "Tüm filtreler" gibi tek bir ikincil panelde toplamak, Baymard'ın "önemli
filtreleri öne çıkarma" bulgusuyla örtüşen bir tasarım kararı.

**Kaynak / güven notu:** Kısmen doğrulandı. Baymard'ın genel bulgusu (kullanıcıların popüler
filtreleri başlangıç noktası olarak kullanması) WebSearch özetinde "Baymard Institute'ın
Travel Accommodations araştırması" atfıyla görüldü; Baymard'ın ilgili blog başlığı
("Consider Promoting Important Filters - 61% Don't") arama sonuçlarında listelendi ama bu
spesifik sayfayı ayrıca fetch etmedim. Airbnb'ye özgü çip/buton düzeni WebSearch snippet'lerinden
(ikincil kaynaklar) geliyor → **kısmen doğrulanmadı**.

---

## 8. "Daha fazla filtre" tam ekran paneli (more filters sheet)

**Ne olduğu:** "Filters" butonuna dokunulduğunda açılan, ilan tipi, oda/yatak sayısı, fiyat
aralığı (çift uçlu slider), olanaklar (amenities), erişilebilirlik seçenekleri gibi onlarca
alt filtreyi kategori başlıkları altında gruplayan tam ekran (mobil) veya büyük modal (web)
panel. Panelin altında genelde canlı güncellenen bir "X sonucu göster" butonu bulunur.

**Nerede görülür:** İkisi de; mobilde tam ekran overlay, web'de büyük bir modal/dropdown
panel olarak.

**UX gerekçesi:** Baymard'ın Airbnb vaka çalışmasında filtre paneli "karmaşıklığı yönetmede
usta işi" (masterclass in managing complexity) olarak tarif ediliyor: checkbox, slider,
toggle çip ve net kategori gruplamaları kullanarak onlarca seçeneği bunaltıcı olmadan sunuyor.
Filtreleri tek bir yoğun listede değil, mantıksal kategoriler (yer tipi, fiyat, olanaklar,
erişilebilirlik) altında gruplamak tarama (scanning) yükünü azaltıyor. Panelin altındaki
canlı sonuç sayacı, kullanıcının "kaç seçenek kaldığını" göndermeden önce görmesini sağlayarak
sıfır-sonuç riskini azaltıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Filtre sayısı 10'u aşan her üründe filtreleri tek
uzun bir listede değil, mantıksal kategoriler altında (2-3 filtre/grup) gruplamak ve panelin
altına canlı güncellenen bir "N sonuç göster" butonu koymak, kullanıcının filtre uygularken
"boşa" gitme riskini azaltır.

**Kaynak / güven notu:** Kısmen doğrulandı. Baymard'ın Airbnb vaka çalışması sayfası
(https://baymard.com/ux-benchmark/case-studies/airbnb) fetch edildi; sayfa filtreleme için
desktop'ta "4 iyi pratik / 2 iyileştirme fırsatı" işareti, mobil web'de "6/3" işareti olduğunu
gösteriyor, ancak asıl detaylı bulgular ücretli erişim arkasında kilitli, sadece özet
sayaçlar görülebildi. "Masterclass in managing complexity" ifadesi ve slider/toggle/checkbox
detayları WebSearch özetlerinden (ikincil kaynak) geldi, Baymard'ın kendi tam metninden değil
→ **kısmen doğrulanmadı**.

---

## 9. Harita entegrasyonu: split-view (web) ve tam ekran harita (mobil)

**Ne olduğu:** Web'de sonuç listesiyle haritanın yan yana (split-view) gösterilmesi; mobilde
liste/harita arasında bir toggle/tab ile geçiş yapılması ya da haritanın altında yarı-şeffaf
bir liste sheet'i olarak sürüklenebilir şekilde belirmesi.

**Nerede görülür:** İkisi de, ama uygulanış biçimi farklı: web = yan yana split-view,
mobil = toggle/tab veya bottom-sheet üzerinden geçiş.

**UX gerekçesi:** Baymard Institute'ın konaklama sitelerinde yaptığı büyük ölçekli
kullanılabilirlik testine göre kullanıcıların **%95'i** split-view düzenlerinde haritayla
etkileşime giriyor ve haritayı aramayı belirli bir alana odaklamak için kullanıyor. Aynı
araştırmaya göre test edilen sitelerin **%70'i** varsayılan olarak sadece liste görünümü
sunuyor; bu durumda kullanıcıların **%65'i** haritayı hiç bulup kullanamıyor. Split-view'i
overlay/modal içine gömmek ise kullanıcıların sıralama/filtre seçeneklerini gözden
kaçırmasına ve terk etmesine yol açan bir tuzak olarak belirlenmiş.

**Airbnb dışı bir uygulamaya uyarlama notu:** Konum bazlı karar verilen her üründe (emlak,
restoran, etkinlik, araç kiralama) haritayı ayrı bir sekmeye/linke gizlemek yerine
varsayılan görünümde listeyle yan yana (web) veya kolay erişilebilir bir toggle ile (mobil)
sunmak, kullanıcıların konum bağlamını kaybetmeden karar vermesini sağlar. Filtre/sıralama
kontrollerinin harita görünümünde de erişilebilir kalmasına özellikle dikkat edilmeli.

**Kaynak / güven notu:** **Doğrulandı** - Baymard Institute'ın "The Optimal Layout for Hotel
& Property Rental Search Results & 3 Pitfalls to Avoid" makalesi fiilen fetch edilip okundu:
https://baymard.com/blog/accommodations-split-view
Yukarıdaki %95, %70, %65 rakamları ve overlay/vertical sidebar/vertical reklam tuzakları bu
sayfadan doğrudan alındı. Not: bu araştırma genel olarak "seyahat/konaklama siteleri"
kategorisi için yapılmış, sadece Airbnb'ye özel değil - Airbnb'nin kendisinin split-view
kullandığı ayrıca (Baymard'ın Airbnb vaka çalışması sayfasından) teyit edildi ama bu makale
Airbnb'yi tek başına konu almıyor.

---

## 10. Harita-liste senkronizasyonu ve "bu alanda arama yap" davranışı

**Ne olduğu:** Kullanıcı haritayı kaydırdığında/yakınlaştırdığında liste sonuçlarının o
coğrafi alana göre güncellenmesi (ya da güncelleme için "Search as I move the map" / "Redo
search in map" tarzı bir onay butonunun belirmesi); ayrıca harita üzerinde tüm ilanları değil,
en üst sıralı 30-50 ilanı büyük pin, geri kalanını küçük "mini-pin" nokta olarak gösteren
kademeli yoğunluk stratejisi.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Üçüncü taraf bir teknik özete göre Airbnb, haritada 200'den fazla pin
göstermek yerine en üst sıralı 30-50 ilanı belirgin pin, geri kalanını küçük noktalar olarak
göstermeye geçmiş; gerekçe "çok fazla pin, harika bir yer bulmayı aslında zorlaştırabilir"
şeklinde özetleniyor. Liste ve harita modlarının farklı sıralama mantığı gerektirdiği de
belirtiliyor: liste modu sıralı/hiyerarşik iken harita modu mekânsal ve hiyerarşisiz;
bu yüzden harita için ayrı bir ML sıralama modeli (coğrafi dikkat ve görünürlük odaklı)
eğitilmiş. Baymard'ın ayrı bir bulgusuna göre ise bazı rakip sitelerde (Expedia) kullanıcıların
**%75'i** haritada yakınlaştırıp alanı daralttıktan sonra liste görünümüne dönünce bu
daraltmanın yansımadığını, yani harita-liste senkronizasyonunun eksik olduğu durumların ciddi
bir kullanılabilirlik sorunu yarattığını gösteriyor - bu da senkronizasyonun neden kritik
olduğunu negatif örnekle kanıtlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Harita tabanlı herhangi bir arama deneyiminde
(emlak, restoran, iş ilanı) liste ve harita arasındaki senkronizasyonun iki yönlü olması
şart: hem harita hareketi listeyi güncellemeli, hem de listedeki bir öğeye dokunmak haritada
karşılığını göstermeli. Yoğun coğrafi alanlarda tüm sonuçları eşit ağırlıkta pin olarak
göstermek yerine, en alakalı sonuçları öne çıkarıp geri kalanını görsel olarak "sessize alma"
(mini-pin/nokta) stratejisi, harita kalabalığını azaltır.

**Kaynak / güven notu:** Kısmen doğrulandı. Mini-pin/30-50 ilan stratejisi ve ayrı harita ML
modeli bilgisi, fiilen fetch edilen bir üçüncü taraf teknik özet yazısından geliyor
(https://techscoop.substack.com/p/how-airbnb-made-map-search-smarter) - bu, Airbnb'nin
birincil kaynağı değil, konuyu yorumlayan bir bülten; iddiaların orijinal Airbnb
mühendislik/tasarım kaynağına kadar inip inmediğini doğrulayamadım. Expedia'daki
%75 senkronizasyon sorunu rakamı ise Baymard'ın split-view makalesinden (yukarıdaki #9 ile
aynı kaynak) doğrudan doğrulandı, ama bu Airbnb'ye değil Expedia'ya ait bir bulgu, genel bir
uyarı olarak aktarılıyor. "Search as I move the map" butonunun Airbnb'de birebir bu adla var
olduğunu doğrudan birincil kaynaktan teyit edemedim → **kısmen doğrulanmadı, karma kaynak**.

---

## 11. Sonuç yok / boş arama durumu (no results state)

**Ne olduğu:** Kullanıcının filtre kombinasyonu hiçbir ilanla eşleşmediğinde gösterilen ekran:
boş bir sayfa yerine "bu kriterlere uygun sonuç yok" mesajı ve filtreleri gevşetme/temizleme
için net bir aksiyon (ör. "tüm filtreleri kaldır" butonu).

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb sınırlı arz üzerine kurulu bir pazaryeri olduğu için bazı filtre
kombinasyonları kaçınılmaz olarak sıfır sonuç veriyor. Boş bir ekran kullanıcıyı ne
yapacağını bilemez halde bırakırken, net bir mesaj + "filtreleri kaldır" aksiyonu kullanıcıyı
hemen bir sonraki adıma yönlendiriyor ve arama hunisinde tam bir çıkmaz (dead end) oluşmasını
engelliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Filtrelenebilir her arama/listeleme ürününde
sıfır sonuç durumu için mutlaka (a) net bir "neden sonuç yok" mesajı, (b) filtreleri tek
tıkla gevşetme/temizleme aksiyonu, (c) mümkünse yakın/benzer alternatif öneriler sunulmalı.
Bu, genel bir "boş durum" (empty state) prensibi olarak arama UX literatüründe yaygın kabul
görüyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Airbnb'nin bu durumu "kriterlere uygun sonuç yok"
mesajı + filtre temizleme aksiyonuyla çözdüğü bilgisi WebSearch özetlerinde ikincil kaynaklara
atıfla geçiyor (birebir Airbnb ekran görüntüsü/birincil kaynak fetch edilemedi); "sınırlı arz
nedeniyle sıfır sonuç kaçınılmaz" çerçevesi de aynı ikincil kaynaktan. Genel boş-durum
prensipleri (spelling correction, ilgili içerik linkleri, filtre gevşetme) UX best-practice
literatüründen (Toptal, Eleken, UXPin gibi genel empty-state kaynakları) geliyor, Airbnb'ye
özel değil → **kısmen doğrulanmadı, çoğunlukla genel UX prensibi + ikincil kaynak**.

---

## 12. Kategori bazlı bağlama duyarlı aksiyon (Icons kategorisinde paylaş vs. kaydet)

**Ne olduğu:** Aynı ilan kartı bileşeninin, hangi kategoride görüntülendiğine göre farklı
birincil aksiyon göstermesi: standart kategorilerde kart üzerinde kalp/wishlist ikonu varken,
"Icons" (sıra dışı/ikonik mülkler) kategorisinde bu ikonun yerini bir paylaşım (share) butonu
alıyor.

**Nerede görülür:** İkisi de (ağırlıklı olarak mobil uygulamada gözlemlenmiş).

**UX gerekçesi:** Airbnb'nin "Icons" kategorisindeki mülkler (olağanüstü/viral nitelikli
evler) kullanıcılar tarafından tipik olarak "ileride rezervasyon yapmak üzere kaydetmek"
yerine "arkadaşlarla paylaşmak" amacıyla görüntüleniyor; bu varsayıma dayanarak birincil
aksiyon butonu context'e göre değiştiriliyor. Bu, "aynı bileşen her yerde aynı davranmalı"
genel kuralının bilinçli bir istisnası: kullanıcı niyetinin kategoriye göre sistematik olarak
değiştiği durumlarda bileşen davranışını da niyete göre uyarlamak.

**Airbnb dışı bir uygulamaya uyarlama notu:** İçerik/ürün kartı bileşeni olan her uygulamada,
belirli bir alt-kategorideki içeriklerin kullanım amacı (satın alma/kaydetme vs. paylaşma/
gösterme) sistematik olarak farklıysa, kart üzerindeki birincil aksiyonu o kategoriye özel
olarak değiştirmek düşünülebilir - ama bu istisna az sayıda, gerçekten farklı davranışlı
kategoriyle sınırlı tutulmalı, aksi halde bileşen tutarlılığı bozulur.

**Kaynak / güven notu:** **Doğrulanmadı, ikincil kaynak snippet'i.** Bu bilgi yalnızca bir
WebSearch özetinde (Superdesign blog / airbnb tasarım sistemi dökümü snippet'i) geçti; ilgili
sayfayı fetch edip birincil metni okumadım, Airbnb'nin kendi ekranında bunu doğrudan
gözlemlemedim. Makul ve iddia kendi içinde tutarlı, ama tek ikincil kaynağa dayandığı için
temkinli okunmalı.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip okunan ve içerik doğrulanan kaynaklar:** Baymard Institute split-view
  makalesi (accommodations-split-view), Baymard Airbnb vaka çalışması sayfası (üst düzey
  özet), Baymard autocomplete örnek sayfası (başlık düzeyinde), Google Design'ın Airbnb
  yazısı, Airbnb Engineering blog yazısı "Recommending travel destinations to help users
  explore", techscoop substack'in harita sıralaması özeti.
- **Fetch girişimi başarısız olan kaynaklar:** uxdesign.cc winter redesign yazısı (redirect
  zinciri çözülemedi), TechCrunch 2018 arama revizyonu yazısı (404), news.airbnb.com flexible
  search duyurusu (403), airbnb.com/help/article/3893 (ilgisiz içerik çıktı).
- Yukarıdaki 12 pattern'den **3 tanesi** (madde 4, 9, ve kısmen 10) doğrudan fetch edilen
  birincil/güçlü kaynaklarla doğrulandı; geri kalanı ya "kısmen doğrulanmadı" (WebSearch
  özeti/ikincil kaynak snippet'i var ama tam metin okunamadı) ya da "doğrulanmadı, eğitim
  verisinden" olarak işaretlendi. Canlı üründe A/B testler nedeniyle detaylar değişebilir;
  bu doküman "yerleşik/tekrar gözlemlenen pattern" çerçevesiyle okunmalı.
