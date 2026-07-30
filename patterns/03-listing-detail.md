# 3. İlan Detay Sayfası (Listing Detail Page)

Bu bölüm Airbnb'nin ilan detay sayfasını kapsıyor: fotoğraf galerisi, sticky/floating rezervasyon
kutusu, host bölümü, olanaklar listesi, açıklama, review özeti ve listesi, ev kuralları/iptal
politikası ifşası, konum/harita reveal mekaniği, benzer ilanlar carousel'i, paylaş/kaydet
aksiyonları ve mobil alt sabit CTA bar. Diğer bölümlerdeki gibi burada da "şu an tam olarak
böyle" değil, tekrar tekrar gözlemlenen/belgelenen yerleşik pattern'ler anlatılıyor; Airbnb
sürekli A/B test yapan canlı bir ürün olduğu için detaylar zamanla değişebilir.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: Airbnb'nin kendi yardım merkezi
sayfaları (konum gösterimi - help/article/2141, review puanlama sistemi - help/article/1257,
review sıralama/arama/etiket sistemi - help/article/13, Superhost gereksinimleri -
help/article/829), GoodUI'nin Airbnb kalp/kaydet ikonu yerleşimi A/B testi analizi, Appcues'un
Airbnb sticky rezervasyon widget'ı üzerine yazısı, eski bir Airbnb tasarımcısının kişisel
portföy sitesinde yayınladığı "fotoğraf görüntüleyici" (photo viewer) birleştirme vaka
çalışması (arlenmccluskey.com), ve Baymard Institute'ın hem "Image Gallery Overlay" örnek
sayfası hem de Airbnb vaka çalışması sayfası (ikisi de büyük ölçüde ücretli içerik arkasında
kilitli, sadece üst düzey başlık/sayaç görülebildi). Bazı maddeler (benzer ilanlar carousel'i,
ev kuralları paneli görsel yerleşimi) için birincil Airbnb kaynağına ulaşılamadı; bu maddeler
açıkça işaretlendi.

---

## 1. Fotoğraf galerisi: web'de grid önizleme + tam ekran "tüm fotoğrafları göster" modalı

**Ne olduğu:** Web'de ilan sayfasının en üstünde büyük bir ana fotoğraf ve onun yanında/altında
küçük bir mozaik (genelde 1 büyük + 4 küçük kare) halinde dizilmiş ek fotoğraflar gösterilir.
Mozaiğin sağ alt köşesindeki "Tüm fotoğrafları göster" butonuna tıklamak, ilanın tüm
fotoğraflarını (genelde kategori/oda başlıklarıyla gruplanmış) büyük, dikey kaydırılabilir bir
tam ekran modal/lightbox içinde açar.

**Nerede görülür:** Ağırlıklı olarak web. Mobil web'de de benzer bir mozaik küçültülmüş halde
görünüp dokunulduğunda tam ekran galeriye geçebiliyor, ama asıl "grid + modal" düzeni masaüstü
deneyiminin karakteristiği.

**UX gerekçesi:** Bir konaklama kararı neredeyse tamamen görsel kanıta dayanıyor; kullanıcı
mekânı fiziksel olarak göremediği için fotoğraflar "güven" ve "gerçeklik" sinyalinin birincil
taşıyıcısı oluyor. Grid önizleme, sayfayı ilk açan kullanıcıya hızlı bir genel izlenim (kaç oda,
genel stil, ışık durumu) verirken, "tüm fotoğrafları göster" modalı isteyen kullanıcıya
detaylı/odaya göre gruplanmış tam inceleme imkânı sunuyor; bu iki katmanlı yapı (özet + detay)
hem hız hem derinlik istemini aynı anda karşılıyor. arlenmccluskey.com'daki vaka çalışması,
Airbnb'nin eskiden ürün katmanına göre (Marketplace, Plus, Luxe, Hotels, Experiences) beş ayrı
fotoğraf görüntüleyici koduna sahip olduğunu ve bunları tek bir birleşik görüntüleyicide
topladığını anlatıyor; yatay fotoğraflar %40, dikey fotoğraflar %60 daha büyük gösterilebilir
hale gelmiş - bu, önceki tasarımda dikey fotoğrafların ekran alanı verimsiz kullanıldığı için
yatay fotoğrafların yaklaşık yarısı boyutunda göründüğü sorununu çözmek için yapılmış.

**Airbnb dışı bir uygulamaya uyarlama notu:** Fiziksel bir ürünü/mekânı satan her platformda
(emlak, araç, restoran, etkinlik mekânı) iki katmanlı galeri yaklaşımı (sayfa üstünde kompakt
mozaik önizleme + isteğe bağlı tam ekran detaylı galeri) uygulanabilir. Görsel oranı (yatay/
dikey) farklı olan içerikler karışık gösteriliyorsa, tek bir sabit en-boy oranına zorlamak yerine
her oranın kendi doğal boyutunda gösterilebileceği esnek bir grid sistemi tasarlamak, önemli bir
detay kaybını (ör. dikey çekilmiş fotoğrafların küçük görünmesi) önler.

**Kaynak / güven notu:** Kısmen doğrulandı. Fotoğraf görüntüleyici birleştirme projesinin
varlığı, beş ayrı eski görüntüleyici sorunu ve %40/%60 büyüme rakamı, doğrudan fetch edilen
arlenmccluskey.com vaka çalışmasından geliyor; ancak bu kaynak Airbnb'nin resmi bir yayını değil,
projede çalışmış olduğunu belirten bir tasarımcının kişisel portföyü - birebir doğru olduğunu
başka bir kaynaktan çapraz doğrulayamadım. Grid'in "1 büyük + 4 küçük" düzeni ve modalın oda
başlıklarına göre gruplanması, genel gözlem/eğitim verisinden geliyor; Baymard'ın ilgili "Image
Gallery Overlay" sayfasını fetch ettim ama içerik büyük ölçüde ücretli kilit arkasındaydı, sadece
"421/537 örnek" başlığı görülebildi → **kısmen doğrulanmadı**.

---

## 2. Mobilde tam ekran (full-bleed) kaydırılabilir fotoğraf carousel'i

**Ne olduğu:** Mobil uygulamada ilan sayfası açılır açılmaz, ekranın tüm genişliğini kaplayan bir
fotoğraf gösterilir; kullanıcı parmağıyla yatay kaydırarak (swipe) sıradaki fotoğrafa geçer.
Fotoğrafın altında/üstünde genelde nokta (dot) göstergesi veya "3/24" gibi bir sayaç bulunur; iki
kez dokunma (double tap) bazı platformlarda ayrı bir tam ekran fotoğraf tarayıcısını açar.

**Nerede görülür:** Mobil (hem native uygulama hem mobil web).

**UX gerekçesi:** Küçük ekranda grid düzeni pratik değil; tek fotoğrafı tam genişlikte gösterip
doğal swipe hareketiyle gezinmeyi sağlamak, dokunmatik ekranın en temel etkileşim biçimini
kullanıyor. arlenmccluskey.com vaka çalışmasındaki kullanıcı araştırmasına göre katılımcılar
"tam genişlikteki görsellerde, ok veya nokta gibi bir arayüz ipucu olmasa bile refleks olarak sağa
sola kaydırıyor" - yani swipe, mobilde ayrıca öğretilmesi gerekmeyen, örtük/keşfedilebilir bir
etkileşim. Bu da arayüzün sade kalmasına (fazladan ok butonu gerekmemesine) izin veriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Mobilde görsel ağırlıklı herhangi bir detay
sayfasında (ürün, ilan, portföy), grid yerine tam genişlikte swipe edilebilir tek görsel akışı
tercih etmek, ekstra gezinme UI'sine (ok butonları vb.) ihtiyacı azaltır; kullanıcılar swipe
hareketini zaten biliyor. Ancak sayfa üzerinde kaçıncı görselde olunduğunu gösteren bir nokta/
sayaç göstergesi eklemek, kullanıcının toplam içerik miktarını (ör. 24 fotoğraf) tahmin
edebilmesi için önemli.

**Kaynak / güven notu:** Kısmen doğrulandı. "Katılımcılar UI ipucu olmadan bile swipe ediyor"
gözlemi doğrudan arlenmccluskey.com vaka çalışmasından (fetch edilip okundu) geliyor. Nokta/sayaç
göstergesi ve double-tap ile ayrı tam ekran tarayıcı açılması detayı, WebSearch özetlerinde
ikincil/topluluk forumu kaynaklarından (community.withairbnb.com bir gönderisi) görüldü, birincil
kaynaktan tam doğrulanmadı → **kısmen doğrulanmadı**.

---

## 3. Sticky/floating rezervasyon kutusu (web): fiyat + tarih seçici + Reserve butonu

**Ne olduğu:** Web'de sayfanın sağ tarafında, ilan bilgileri (galeri, açıklama, host, olanaklar,
review'lar) aşağı doğru uzarken bile sabit kalan (ya da belirli bir noktaya kadar kullanıcıyla
birlikte kayan) bir rezervasyon kutusu: gecelik fiyat, tarih seçici (check-in/check-out), misafir
sayısı seçici ve büyük bir "Reserve"/"Rezervasyon yap" butonu içerir. Kullanıcı sayfada ne kadar
aşağı inerse insin bu kutu (ya da bir özeti) görünür kalır.

**Nerede görülür:** Web (masaüstü). Mobilde aynı işlevi alt sabit bir CTA bar üstleniyor (bkz.
madde 4).

**UX gerekçesi:** Appcues'un yazısına göre bu tasarım kararı bir "80/20" mantığına dayanıyor:
sitedeki etkileşimin büyük kısmı fiyat/tarih/rezervasyon gibi az sayıda kritik işlevde
yoğunlaşıyor, dolayısıyla bu işlevi her zaman erişilebilir tutmak mantıklı. Kullanıcı sayfada
fotoğraflara bakarken, host'u incelerken veya review okurken "acaba bu tarihlerde ne kadar
tutuyordu" diye yukarı geri kaydırmak zorunda kalmıyor; fiyat ve rezervasyon eylemi her zaman
göz önünde. Bu, dönüşüm hunisinin en kritik adımını (rezervasyon) sayfanın neresinde olursa
olsun bir tık uzaklıkta tutuyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Uzun, içerik ağırlıklı bir detay sayfasında (ürün
sayfası, ilan, bilet satışı, SaaS fiyatlandırma sayfası) tek bir birincil dönüşüm eylemi
(satın al, rezervasyon yap, abone ol) varsa, bu eylemi ve onunla ilgili temel parametreleri
(fiyat, miktar, tarih) sayfa boyunca sticky bir panelde tutmak, kullanıcının karar anında geri
kaydırma sürtünmesini ortadan kaldırır. Panelin taşımaması gereken şey: çok fazla ikincil bilgi
ekleyip asıl eylemi gölgelemek.

**Kaynak / güven notu:** Kısmen doğrulandı. "Sticky widget her zaman görünür kalıyor, bu 80/20
mantığına dayanıyor" argümanı doğrudan fetch edilen Appcues yazısından (goodux.appcues.com/blog/
airbnbs-sticky-widget) geliyor; ancak bu kaynak da Airbnb'nin resmi bir yayını değil, üçüncü
taraf bir "iyi UX örneği" blog yazısı, ve masaüstü/mobil ayrımını (masaüstünde sticky panel,
mobilde alt bar) ayrıca tartışmıyor - bu ayrım WebSearch özetlerinde başka (yine ikincil)
kaynaklardan geldi. Kutunun tam içeriği (fiyat + tarih + misafir sayacı + Reserve butonu sırası)
genel gözlem/eğitim verisinden → **kısmen doğrulanmadı**.

---

## 4. Mobil alt sabit CTA bar (bottom sticky bar)

**Ne olduğu:** Mobil uygulamada/mobil web'de sayfanın en altında, ekran boyunca sabit kalan ince
bir bar: solda toplam veya gecelik fiyat, sağda tek bir büyük "Reserve" (Rezervasyon yap)
butonu. Kullanıcı sayfada ne kadar kaydırırsa kaydırsın bu bar ekranda kalır; dokunulduğunda
tarih/misafir seçim akışı ya da doğrudan rezervasyon onay adımı açılır.

**Nerede görülür:** Mobil (hem native uygulama hem mobil web).

**UX gerekçesi:** Mobilde ekran alanı kısıtlı olduğu için web'deki gibi büyük bir sticky panel
sığdırmak mümkün değil; bunun yerine en kritik iki bilgi (fiyat, birincil eylem) tek satıra
indirgenmiş minimal bir bara sıkıştırılıyor. WebSearch özetlerinde geçen bir gözleme göre
"mobilde fiyat ekranın altına her zaman sabitlenmiş durumda" - bu, kullanıcının parmağının doğal
olarak ekranın alt kısmına yakın durduğu (bir elle kullanım / thumb zone) mobil ergonomisiyle de
örtüşüyor: birincil eylem, başparmağın en rahat ulaştığı bölgeye yerleştirilmiş oluyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Mobil detay sayfalarında (e-ticaret ürün sayfası,
bilet satışı, hizmet rezervasyonu) tek bir birincil eylem varsa, bunu ekranın en altında sabit,
tek satırlık, fiyat + buton ikilisine indirgenmiş bir bar olarak tutmak, hem thumb-zone
ergonomisine hem de mobildeki kısıtlı dikey alana uygun bir çözüm. Bar'a ikinci bir eylem (ör.
"karşılaştır" veya "favorile") eklemek çekici gelebilir ama bu, birincil eylemin görsel
ağırlığını zayıflatma riski taşır.

**Kaynak / güven notu:** Kısmen doğrulandı. "Mobilde fiyat ekranın altına her zaman sabitlenmiş"
ifadesi WebSearch sonuçlarında (bir topluluk/blog kaynağı özeti üzerinden) görüldü, birincil
Airbnb ekran görüntüsü/kaynağı fetch edilerek doğrulanmadı. Thumb-zone ergonomisi açıklaması
genel mobil UX literatüründen (eğitim verisi), Airbnb'ye özgü değil → **kısmen doğrulanmadı,
çoğunlukla eğitim verisinden**.

---

## 5. Host bölümü / "Meet your host" mini-profil

**Ne olduğu:** İlan sayfasında host'a ayrılmış bir bölüm: host'un profil fotoğrafı (genelde
yuvarlak), adı, varsa Superhost rozeti (madalyon/yıldız ikonu), host olarak kaç yıldır aktif
olduğu, toplam review sayısı/puanı, "response rate" (yanıt oranı) ve "response time" (yanıt
süresi) gibi metrikler ve bir "Host ile mesajlaş" ya da "Host'u tanı" (Meet your host) linki/
mini-kart yer alır.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Bir konaklama kararı, bir üründen çok bir insanla (ya da onun temsil ettiği
mekânla) güven ilişkisi kurmayı gerektiriyor; host'u somutlaştırmak (fotoğraf, isim, geçmiş
performans metrikleri) "bu parayı kime, hangi garantiyle veriyorum" sorusuna cevap veriyor.
Superhost rozetinin görünürlüğü özellikle önemli: Airbnb'nin kendi yardım merkezi sayfasına göre
Superhost olabilmek için bir host'un en az 10 rezervasyon (ya da toplam 100 gecelik 3
rezervasyon) tamamlamış olması, yeni mesaj ve rezervasyon taleplerinin **%90'ına 24 saat içinde**
yanıt vermesi, **%1'in altında** iptal oranına sahip olması ve **4.8 veya üzeri** genel puana
sahip olması gerekiyor - yani rozet, tek bir öznel izlenim değil, birden fazla nicel eşiği
birden geçmiş olmanın kısaltılmış bir görsel özeti. Response rate/time metrikleri de host'un
"sorularıma hızlı cevap verir mi" belirsizliğini rezervasyon öncesinde azaltıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** İnsan-insana güven gerektiren her marketplace'te
(freelancer platformu, ikinci el satış, hizmet pazaryeri) satıcı/hizmet sağlayıcının profilini
somutlaştırmak (fotoğraf, deneyim süresi, yanıt hızı, geçmiş performans rozeti) kritik bir güven
sinyali. Rozet sistemleri kurulurken Airbnb'deki gibi rozetin arkasında somut, nicel ve
denetlenebilir eşikler (belirli sayıda işlem, belirli bir yanıt oranı, belirli bir iptal oranı
altı) olması, rozetin keyfi/anlamsız bir "iyi satıcı" etiketine dönüşmesini engelliyor.

**Kaynak / güven notu:** Superhost eşikleri (10 rezervasyon/100 gece, %90 yanıt oranı, %1 iptal
oranı, 4.8 puan) **doğrulandı**: doğrudan fetch edilen Airbnb yardım merkezi sayfasından
(https://www.airbnb.com/help/article/829) alındı, birebir bu eşikler sayfada yazıyor. "Meet
your host" mini-kartının tam görsel yerleşimi (fotoğraf boyutu, kartın ilan sayfasındaki tam
konumu) ise genel gözlem/eğitim verisinden, birincil kaynaktan ekran görüntüsü üzerinden
doğrulanmadı → **kısmen doğrulandı: metrik eşikleri güçlü kaynaklı, görsel yerleşim
doğrulanmadı**.

---

## 6. Olanaklar (amenities) listesi + "X olanağın tümünü göster" modalı

**Ne olduğu:** İlan sayfasında "Bu yer ne sunuyor" (What this place offers) başlığı altında,
genelde en popüler/öne çıkan 8-10 olanağın (wifi, mutfak, ücretsiz otopark, klima vb.) ikonlarla
kısa bir liste halinde gösterilmesi; listenin altında "X olanağın tümünü göster" butonuna
dokunmak, kategorilere ayrılmış (banyo, yatak odası/çamaşır, eğlence, ısıtma/soğutma, güvenlik
vb.) tam listeyi bir modal/tam ekran panelde açar.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Bir ilanın onlarca (bazen ~150'ye yakın) olası olanağı olabilir; bunların
tamamını sayfanın ana akışında göstermek, kullanıcının asıl karar vermesi gereken bilgiyi
(fotoğraf, fiyat, konum) gürültüye boğar. Öne çıkan az sayıda olanağı ana sayfada göstermek,
gerisini isteğe bağlı bir modale taşımak, "kısa özet + isteğe bağlı derinlik" prensibinin bir
başka uygulaması: çoğu kullanıcı için birkaç olanak yeterli karar bilgisi verirken, detaycı
kullanıcı (ör. evcil hayvan dostu mu, jakuzi var mı) tam listeye tek dokunuşla ulaşabiliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Uzun özellik/özellik listesi olan her ürün detay
sayfasında (emlak ilanı, araç kiralama, elektronik ürün teknik özellikleri) aynı iki katmanlı
yaklaşım uygulanabilir: en sık aranan/karar belirleyici 6-10 özelliği ana sayfada göster, kalan
onlarca detayı kategorilere ayrılmış bir "tümünü gör" modaline taşı. Kategorilere ayırma
(ör. banyo/mutfak/güvenlik gibi gruplar) tam listenin taranabilir kalmasını sağlar.

**Kaynak / güven notu:** Kısmen doğrulandı. "Show all amenities" akışının var olduğu ve hostların
listeye ilk kurulumda kısa bir popüler olanaklar listesinden seçip yayından sonra ~150 olanağa
kadar genişletebildiği bilgisi WebSearch özetlerinde (Mobbin akış kataloğu ve Airbnb kaynak
merkezi sayfaları referans alınarak) görüldü; ilgili Mobbin akış sayfasını veya Airbnb kaynak
merkezi sayfasını doğrudan fetch edip tam metnini okumadım, sadece arama motoru özetini gördüm.
Kategorilere ayrılmış modal yapısı (banyo/yatak odası/güvenlik grupları) genel gözlem/eğitim
verisinden → **kısmen doğrulanmadı**.

---

## 7. Açıklama (description) "daha fazla göster" kısaltması

**Ne olduğu:** İlanın yazılı açıklaması, sayfa yüklendiğinde tam gösterilmez; belirli bir
karakter/satır sayısından sonra metin kesilir ("..." ile) ve altında bir "Daha fazla göster"
(Show more) linki/butonu belirir. Bu linke dokunmak açıklamanın tamamını (genelde bir modal
veya sayfa içi genişleme ile) gösterir.

**Nerede görülür:** İkisi de; mobilde kesme noktası web'e göre daha erken geliyor (daha az
karakterle kesiliyor).

**UX gerekçesi:** WebSearch sonuçlarına göre Airbnb ana açıklamayı 2022 Temmuz'unda 500 karaktere
sınırlamış; mobil uygulamada ise yaklaşık ilk 295 karakter görünür kalıyor, gerisi için
"Daha fazla göster"e dokunmak gerekiyor. Bu kısıtlama hem host'u öz/çekici bir özet yazmaya
zorluyor (bir "başlık" gibi düşünmesi teşvik ediliyor) hem de kullanıcının sayfayı ilk açtığında
uzun bir metin duvarıyla karşılaşıp asıl karar verici görsel/fiyat bilgisine geç ulaşmasını
engelliyor. Kısaltma + isteğe bağlı genişletme, "az bilgiyle hızlı tara, istersen derinleş"
prensibinin metin versiyonu.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcı tarafından yazılan uzunluğu değişken
metin içeren her detay sayfasında (ürün açıklaması, ilan metni, biyografi) sabit bir karakter
sınırıyla kısaltıp "daha fazla göster" linkiyle genişletme imkânı sunmak, sayfanın ilk taranışını
hızlı tutarken detaya ilgi duyan kullanıcıyı da mahrum bırakmıyor. Kısaltma noktasının cihaza göre
(mobilde daha erken) ayarlanması, küçük ekranda gereksiz kaydırmayı azaltır.

**Kaynak / güven notu:** Kısmen doğrulandı. "500 karakter sınırı" ve "mobilde ~295 karakter
görünür" rakamları WebSearch özetinde bir host-eğitimi/blog kaynağına (strassistance.com ve
benzeri ikincil host rehberi siteleri) atıfla görüldü; bu sayfaları doğrudan fetch edip birincil
metni okumadım, Airbnb'nin kendi kaynak merkezinden (resources/hosting-homes) de bu rakamı
doğrulayan bir sayfayı fetch etmedim. Kısaltma+genişletme davranışının kendisi (var olduğu)
makul güvenilir ama tam rakamlar ikincil kaynak → **kısmen doğrulanmadı**.

---

## 8. Review özeti: genel puan + kategori kırılımı

**Ne olduğu:** İlan sayfasında büyük bir genel yıldız puanı (ör. "4.92") ve toplam review
sayısının hemen altında/yanında, altı ayrı kategoriye bölünmüş bir puan dökümü: temizlik
(cleanliness), doğruluk (accuracy), check-in, iletişim (communication), konum (location) ve
değer (value). Her kategori genelde kendi yıldız/bar göstergesiyle ayrı ayrı listelenir.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Tek bir toplu puan ("4.9 yıldız") bilgi açısından zengin değildir; kullanıcı
"neden 4.9, hangi açıdan iyi" sorusuna cevap arar. Airbnb'nin kendi yardım merkezi sayfasına göre
**genel puan, altı kategori puanının bir ortalaması değildir; kendi başına ayrı bir kategoridir**
- yani bir misafir her kategoriye 5 verip yine de genel puana 4 verebilir. Bu ayrım önemli çünkü
kategori kırılımı, kullanıcıya kendi önceliğine göre (ör. "konum benim için her şeyden önemli")
filtrelenmiş bir okuma imkânı veriyor: genel puan yüksek olsa da "temizlik" düşükse bu, genel
puana bakarak fark edilemeyecek bir sinyal.

**Airbnb dışı bir uygulamaya uyarlama notu:** Çok boyutlu bir deneyimi (konaklama, restoran,
hizmet) tek bir puana indirgeyen her platformda, genel puanın yanına birkaç anlamlı alt kategori
puanı eklemek (ör. bir restoran için lezzet/servis/ortam/fiyat-performans), kullanıcının kendi
önceliğine göre karar vermesini kolaylaştırır. Genel puanın alt kategorilerin basit bir ortalaması
olmadığını açıkça belirtmek (ya da öyle tasarlamak), kullanıcının "puanlar tutarsız" hissine
kapılmasını önler.

**Kaynak / güven notu:** **Doğrulandı** - doğrudan fetch edilen Airbnb yardım merkezi sayfasından
(https://airbnb.com/help/article/1257): "Note that Overall rating is its own category (not an
average of the other categories)" ifadesi ve altı kategori (cleanliness, accuracy, check-in,
communication, location, value) birebir bu sayfada geçiyor. Kategori puanlarının sayfada tam
olarak nerede/hangi görsel formatta (yıldız mı bar mı) gösterildiği ise genel gözlemden, ekran
görüntüsü üzerinden doğrulanmadı.

---

## 9. Review listesi & filtreleme (sıralama, anahtar kelime arama, konu etiketleri)

**Ne olduğu:** Review özetinin altında, tekil review'ların listelendiği bir bölüm. Varsayılan
sıralama "en alakalı" (most relevant) review'ları öne çıkarıyor; kullanıcı bunu "en yeni",
"en yüksek puan" veya "en düşük puan" olarak değiştirebiliyor. Ayrıca bir arama kutusuyla
review metinleri içinde anahtar kelime aranabiliyor (ör. "wifi", "gürültü", "manzara"); sık
geçen konular otomatik olarak "konu etiketleri" (topic tags) halinde review listesinin üstünde
gösteriliyor, bir etikete dokunmak o konudan bahseden review'ları filtreleyip en yeniye göre
sıralıyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb'nin kendi yardım merkezi sayfasına göre varsayılan "alakalılık"
sıralaması review'un güncelliği, dili, yorumcunun yaşadığı ülke, review uzunluğu ve
kullanıcının arama filtreleri gibi birden fazla faktöre göre hesaplanıyor - yani sadece
kronolojik değil, o anki kullanıcıya en anlamlı gelecek review'ları öne çıkarmaya çalışan bir
sıralama. Anahtar kelime arama ve konu etiketleri, yüzlerce review'ı tek tek okumak zorunda
kalmadan kullanıcının kendi önceliğine (ör. "gürültülü mü", "internet hızlı mı") özel kanıt
bulmasını sağlıyor; bu, "review'ları tara" yükünü kullanıcıdan alıp arayüze devrediyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Çok sayıda kullanıcı yorumu biriken her platformda
(e-ticaret ürün review'ları, uygulama mağazası yorumları, iş yeri değerlendirmeleri), yüzlerce
yorumu ham kronolojik liste olarak sunmak yerine (a) anlamlı bir varsayılan sıralama
(alakalılık), (b) yeniden sıralama seçenekleri (en yeni/en yüksek/en düşük puan), ve (c) sık
geçen konuları otomatik çıkarıp etiketleyerek filtrelenebilir hale getirmek, kullanıcının kendi
sorusuna ("bu üründe X sorunu var mı") doğrudan cevap bulmasını hızlandırır.

**Kaynak / güven notu:** **Doğrulandı** - doğrudan fetch edilen Airbnb yardım merkezi sayfasından
(https://www.airbnb.com/help/article/13): "Reviews that are most relevant to you are shown first
by default", sıralama faktörleri (güncellik, dil, yorumcunun ülkesi, review uzunluğu, arama
filtreleri), sıralama seçenekleri (recency/highest rating/lowest rating), anahtar kelime arama
("search reviews using keywords (example: cleanliness, internet speed, location, etc.)") ve konu
etiketi seçilince review'ların "recency'e göre sıralanması" ifadeleri birebir bu sayfada geçiyor.

---

## 10. Ev kuralları / iptal politikası ifşa paterni

**Ne olduğu:** İlan sayfasında, rezervasyon yapılmadan önce görülebilen ayrı bir "Ev kuralları"
(House rules) ve "İptal politikası" (Cancellation policy) bölümü: check-in/check-out saatleri,
maksimum misafir sayısı, evcil hayvan/sigara/etkinlik kısıtlamaları gibi kurallar ile
esnek/orta/katı gibi iptal politikası kademeleri, rezervasyon onayından önce, genelde sayfanın
alt kısımlarında ama yine de ana akışta (ayrı bir gizli sayfaya gömülmeden) gösterilir.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb'nin kendi kaynak merkezi/yardım sayfalarına göre host'ların
uygulanabilir tüm ev kurallarını rezervasyon öncesinde ifşa etmesi bekleniyor ki misafir bilinçli
bir karar verebilsin; ev kuralları ayrıca ilan açıklamasında, rezervasyon sırasında gösterilen
şartlarda, otomatik misafir e-postalarında ve varış rehberinde tekrarlanıyor - yani tek seferlik
bir gösterim değil, karar/rezervasyon/varış akışının birden fazla noktasında tekrarlanan bir
ifşa modeli. Bu, "sürpriz kısıtlama" riskini (rezervasyon sonrası "meğer evcil hayvan
yasakmış" gibi anlaşmazlıkları) azaltıyor ve olası bir uyuşmazlıkta hem host hem misafir için
netlik sağlıyor. İptal politikasının rezervasyon öncesi net görünmesi ise finansal riski
(iptal ederse ne kadarını geri alır) önceden hesaplanabilir kılıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir sözleşme/rezervasyon içeren her platformda
(etkinlik bileti, kiralama, hizmet randevusu), kısıtlayıcı kuralları ve iptal/iade koşullarını
"küçük yazı" (fine print) olarak gizlemek yerine rezervasyon kararından önce ana akışta, ve
tekrar rezervasyon onayı sırasında göstermek, sonradan çıkan anlaşmazlıkları ve müşteri hizmetleri
yükünü azaltır. Kuralların birden fazla temas noktasında (ilan sayfası, onay ekranı, hatırlatma
e-postası) tekrarlanması, "okumadım" savunmasını da zayıflatır.

**Kaynak / güven notu:** Kısmen doğrulandı. "Ev kuralları ilan açıklamasında, rezervasyon
şartlarında, otomatik e-postalarda ve varış rehberinde tekrar gösteriliyor" bilgisi WebSearch
özetinde Airbnb'nin kendi yardım/kaynak merkezi sayfalarına (airbnb.com/help, airbnb.com/
resources/hosting-homes) atıfla görüldü, ancak bu spesifik sayfaları doğrudan fetch edip birebir
metnini okumadım. İlan sayfasındaki tam görsel yerleşim (hangi bölümün altında, hangi ikonlarla)
genel gözlem/eğitim verisinden → **kısmen doğrulanmadı**.

---

## 11. Konum/harita reveal: rezervasyondan önce yaklaşık, sonra kesin

**Ne olduğu:** İlan sayfasındaki harita, rezervasyon onaylanmadan önce host'un tam adresini değil,
host'un tercihine göre iki moddan birini gösterir: (a) "genel konum" modunda haritada gerçek
adresin etrafında yaklaşık yarım kilometre (bir mil'in altı) yarıçapında gölgeli/bulanık bir
daire; (b) "spesifik konum" modunda en yakın kavşağa işaret eden ama tam noktayı işaretlemeyen
bir pin. Rezervasyon onaylandıktan sonra ise tam harita konumu ve sokak adresi (ama daire/apartman
numarası hariç) misafire açılıyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Bu, host'un gizlilik/güvenliğini (tam adresin herkese, rezervasyon yapmamış
kişilere dahi açık olmasının yaratacağı riskleri, ör. davetsiz ziyaretler veya kötüye kullanım)
korurken, potansiyel misafirin karar vermesi için yeterli konum bağlamını (mahalle, bölgeye
yakınlık) hâlâ sunan kademeli bir bilgi ifşası (graduated disclosure) modeli. Kullanıcı, tam adresi
bilmeden de "bu bölge bana uyar mı" sorusuna cevap verebiliyor; rezervasyon onayı ise bir güven
eşiği (ödeme yapılmış, host tarafında da onaylanmış bir taahhüt) olarak, tam adresin
paylaşılmasını haklı kılan bir an olarak kullanılıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Fiziksel bir konumun (özel mülk, ev, ofis) tam
adresinin herkese açık olmasının güvenlik riski taşıdığı her platformda (kısa süreli kiralama,
özel etkinlik mekânı, hizmet randevusu evde), aynı kademeli ifşa modeli uygulanabilir: karar
öncesi yaklaşık/bulanık konum, rezervasyon/ödeme sonrası tam adres. Bu, hem satıcı gizliliğini
korur hem de "adres bilmeden nasıl karar vereceğim" itirazını, yeterli yakınlık bilgisiyle
(mahalle, yarıçap) çözer.

**Kaynak / güven notu:** **Doğrulandı** - doğrudan fetch edilen Airbnb yardım merkezi sayfasından
(https://www.airbnb.com/help/article/2141/how-will-my-listings-location-be-shown-on-the-map):
"specific location" modunda "a pin pointing to its exact location on the map but the numerical
street address will not be shared until the guest's reservation is confirmed" ve "general
location" modunda "a small circle surrounded by a shaded circle... within about a half mile
(less than 1 kilometer)" ifadeleri birebir bu sayfada geçiyor; ayrıca "your unit number will
never be shared with guests until they have a confirmed reservation" ifadesiyle daire/apartman
numarasının rezervasyon sonrasında bile ayrıca korunduğu doğrulandı.

---

## 12. Benzer/yakın ilanlar carousel'i (sayfa altı)

**Ne olduğu:** İlan sayfasının en altında, kullanıcının o an baktığı ilana benzer (aynı bölge,
benzer fiyat aralığı, benzer ev tipi) diğer ilanları küçük kartlar halinde yatay kaydırılabilir
bir carousel'de gösteren bir bölüm (genelde "Benzer evler" / "Bu bölgedeki diğer yerler" gibi bir
başlıkla).

**Nerede görülür:** İkisi de.

**UX gerekçesi (genel prensip olarak):** Kullanıcının o an baktığı belirli ilan (fiyat, tarih
uygunluğu, host'un tarzı gibi nedenlerle) uygun olmayabilir; sayfanın "dead end" (çıkmaz sokak)
olmasını önlemek için benzer alternatifleri aynı sayfada sunmak, kullanıcıyı arama sonuçlarına
geri dönüp yeniden filtrelemeye zorlamadan bir sonraki adımı sunuyor. Bu aynı zamanda platformun
kendi içinde kalma (bounce etmeme) süresini uzatan, e-ticaretteki "benzer/ilişkili ürünler"
paterniyle aynı mantığa dayanan bir keşif-devamlılığı mekanizması.

**Airbnb dışı bir uygulamaya uyarlama notu:** Her detay sayfasında (ürün, ilan, içerik), sayfanın
sonunu bir çıkmaz olarak bırakmak yerine "benzerlerini gör" tarzı bir bölümle bir sonraki adımı
önermek, kullanıcının bu ilan/ürün uygun olmadığında bile platformda kalmasını sağlar. Benzerlik
kriterinin (fiyat, konum, kategori) kullanıcıya açık/anlamlı olması, önerilerin alakasız
hissettirmemesi için önemli.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu paternin Airbnb ilan sayfasının
altında var olduğu genel gözlem/eğitim verisiyle biliniyor, ancak yapılan WebSearch aramaları
(hem genel hem "similar homes"/"other places to stay" başlıklı sorgular) bu spesifik bölümü
belgeleyen bir birincil ya da ikincil kaynağa ulaşamadı; arama sonuçları bunun yerine "Airbnb
alternatifleri" (rakip platformlar) hakkında makaleler döndürdü. Bu madde tamamen kendi
gözlemim/eğitim verim üzerinden yazıldı, hiçbir kaynakla doğrulanmadı.

---

## 13. Paylaş & kaydet (share & save) aksiyonlarının yerleşimi

**Ne olduğu:** İlan sayfasının en üstünde, genelde galerinin sağ üst köşesinde, iki ikincil eylem
butonu yan yana durur: bir paylaşım ikonu (linki kopyala, sosyal medyada/mesajda paylaş) ve bir
kalp/kaydet ikonu (ilanı bir wishlist'e ekle). Bu iki buton, ana rezervasyon eylemine göre görsel
olarak daha küçük/ikincil bir ağırlıkta tutulur.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Paylaşma ve kaydetme, "şimdi rezervasyon yap" kadar acil olmayan ama karar
sürecinin doğal bir parçası olan eylemler: kullanıcılar genelde bir ilanı hemen rezerve etmeden
önce başkalarına danışmak (paylaş) veya sonra karşılaştırmak üzere not almak (kaydet)
istiyor. Bu iki eylemi galerinin hemen üstünde, göze çarpan ama rezervasyon butonuyla
yarışmayan bir konumda tutmak, "karar öncesi araştırma" davranışını destekliyor. GoodUI'nin
fetch edilen bir A/B test analizine göre (bu analiz doğrudan ilan detay sayfası değil, arama
sonucu/listing kartı bağlamında yapılmış olsa da aynı prensibi gösteriyor) Airbnb, kalp/kaydet
ikonunun fotoğrafın tam üzerinde durduğu bir varyanttan, ikonu kartın sağ tarafına taşıyan bir
varyanta geçmiş; makalenin öne sürdüğü (ama Airbnb'nin resmi olarak doğrulamadığı) hipotez, bunun
hem fotoğrafı görsel olarak temiz tuttuğu hem de yanlışlıkla fotoğrafa dokunup kaydetme/kaydetmeme
gibi kaza sonucu tıklamaları azalttığı yönünde.

**Airbnb dışı bir uygulamaya uyarlama notu:** Birincil dönüşüm eylemi (satın al, rezervasyon yap)
olan her detay sayfasında, ikincil ama yine de değerli eylemleri (paylaş, favorile, karşılaştırmaya
ekle) görsel ağırlıkça daha hafif tutup ana eylemle karışmayacak, ama yine de kolay erişilebilir
bir konuma (genelde galeri/görsel alanının üst köşesi) yerleştirmek, iki farklı kullanıcı niyetini
(hemen karar ver vs. araştırmaya devam et) aynı sayfada çakışmadan destekler.

**Kaynak / güven notu:** Kısmen doğrulandı. Kalp/kaydet ikonunun fotoğraf üzerinden kartın sağ
tarafına taşınması yönündeki A/B test bilgisi doğrudan fetch edilen bir GoodUI yazısından geliyor
(https://goodui.org/leaks/airbnb-a-b-tests-and-detects-a-better-placement-for-saving-properties/);
ancak bu analiz **arama sonucu/listing kartı bağlamında** yapılmış (02 numaralı bölümde de bir
ölçüde ilgili olabilir), ilan detay sayfasının kendi üst barındaki paylaş/kaydet yerleşimine
birebir uygulanıp uygulanmadığı doğrulanmadı; ayrıca makalenin kendisi de öne sürdüğü nedenleri
"reverse engineered" (Airbnb'nin resmi açıklaması değil, yazarın tahmini) olarak nitelendiriyor.
İlan sayfası üst barındaki tam ikon yerleşimi genel gözlem/eğitim verisinden →
**kısmen doğrulanmadı, karma kaynak**.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip birincil/güçlü içerik olarak doğrulanan kaynaklar (Airbnb'nin kendi
  yardım merkezi sayfaları):** konum gösterimi (help/article/2141), review puanlama sistemi -
  genel puan ayrı kategori (help/article/1257), review sıralama/anahtar kelime arama/etiketler
  (help/article/13), Superhost gereksinimleri (help/article/829). Bu 4 kaynak, ilgili 13
  pattern'den 3'ünü (madde 8, 9, 11) ve 1 tanesini kısmen (madde 5) doğrudan ve güçlü şekilde
  destekliyor.
- **Fetch edilip okunan ama ikincil/üçüncü taraf olan kaynaklar:** Appcues'un sticky widget yazısı,
  GoodUI'nin kalp ikonu yerleşimi A/B test analizi, arlenmccluskey.com'daki eski bir Airbnb
  tasarımcısının fotoğraf görüntüleyici vaka çalışması. Bunlar gerçek ve fiilen okundu, ama
  Airbnb'nin resmi/birincil kaynağı değiller; ilgili maddelerde "kısmen doğrulandı" olarak
  işaretlendi.
- **Fetch edilip içeriği büyük ölçüde erişilemez/ücretli çıkan kaynaklar:** Baymard'ın Image
  Gallery Overlay örnek sayfası ve Airbnb vaka çalışması sayfası - sadece üst düzey başlık/sayaç
  görülebildi, asıl bulgular kilit arkasında.
- **Hiçbir kaynakla doğrulanamayan, tamamen eğitim verisinden yazılan madde:** benzer/yakın
  ilanlar carousel'i (madde 12) - yapılan aramalar konuyla ilgili birincil veya ikincil kaynak
  döndürmedi.
- Toplamda 13 pattern'den **3 tanesi** (madde 8, 9, 11) doğrudan Airbnb birincil kaynağıyla
  güçlü doğrulandı; **9 tanesi** kısmen doğrulandı (gerçek kaynak fetch edildi ama ikincil/
  üçüncü taraf ya da kısmi içerik); **1 tanesi** (madde 12) tamamen doğrulanmadı/eğitim
  verisinden. Canlı üründe sürekli A/B test yapıldığından, bu doküman "yerleşik/tekrar
  gözlemlenen pattern" çerçevesiyle okunmalı, "şu an birebir ekran görüntüsü" olarak değil.
