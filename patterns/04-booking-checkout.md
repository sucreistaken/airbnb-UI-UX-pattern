# 4. Rezervasyon / Checkout Akışı (Booking & Checkout Flow)

Bu bölüm, kullanıcının bir ilanı görüntülemekten fiili rezervasyona geçtiği tüm akışı kapsıyor:
tarih aralığı seçici (takvim arayüzü, minimum/maksimum konaklama, kapalı tarihler), misafir
sayacı (yetişkin/çocuk/bebek/evcil hayvan), Instant Book (anında rezervasyon) ile Request to
Book (rezervasyon talebi) ayrımı, fiyat dökümü (gecelik ücret x gece sayısı, temizlik ücreti,
hizmet ücreti, vergiler, "vergiler hariç toplam" anahtarının checkout bağlamındaki hali), ödeme
yöntemi seçimi ve bölünmüş ödeme ("şimdi bir kısmını, kalanını sonra öde"), trip protection/ek
sigorta upsell yerleşimi, "review and pay" (gözden geçir ve öde) son onay ekranı, ödeme
bilgisi girildikten sonra tarih değiştirme akışı, host onayı bekleme durumu (request-to-book
için) ve rezervasyon sonrası onay ekranı ile Trips (Seyahatlerim) sayfasına devir, son olarak
iptal politikasının checkout'ta yeniden onaylanması. Diğer bölümlerde olduğu gibi burada da "şu
an tam olarak böyle" değil, tekrar tekrar gözlemlenen/belgelenen yerleşik pattern'ler anlatılıyor;
Airbnb sürekli A/B test yapan canlı bir ürün olduğu için buton metinleri, sıralama ve yüzdeler
zamanla değişebilir.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: Airbnb'nin kendi yardım merkezi
sayfalarından on tanesi (Instant Book/Request to Book ayrımı - help/article/85, Instant Book'un
işleyişi - help/article/1510, rezervasyon ayarlarını özelleştirme/min-max konaklama -
help/article/484, ev için iptal politikaları - help/article/475, iptal öncesi/sonrası iade
tutarını bulma - help/article/311, misafir olarak çocukların/bebeklerin sayılması -
help/article/433, misafir olarak rezervasyon durumunu bulma - help/article/234, planlı/bölünmüş
ödemeler - help/article/2143, ev rezervasyonunun tarihlerini değiştirme - help/article/913, ve
ABD misafirleri için seyahat sigortası/ek ürünler - help/article/3443). Bunlara ek olarak
WebSearch üzerinden (doğrudan fetch edilemeden, arama motoru özeti üzerinden) toplam fiyat
("before taxes"/vergiler hariç toplam) gösterge değişikliğiyle ilgili haber kaynakları
(TechCrunch, Skift, Rental Scale-Up, resmi news.airbnb.com duyurusu) ve Baymard Institute'ın
tarih seçici örnekleri sayfası tarandı; news.airbnb.com ve skift.com sayfaları doğrudan fetch
denemesinde HTTP 403 ile engellendi, bu yüzden bu iki kaynak "ikincil özet, birincil metin
okunamadı" olarak işaretlendi. Bazı maddeler (review-and-pay ekranının tam görsel yerleşimi,
takvimde kapalı/uygun olmayan tarihlerin tam görsel ifadesi, rezervasyon sonrası onay ekranının
animasyon/görsel detayları) için birincil Airbnb kaynağına ulaşılamadı; bu maddeler açıkça
işaretlendi.

---

## 1. Tarih aralığı seçici: takvim UI'si, minimum/maksimum konaklama, kapalı tarihler

**Ne olduğu:** Kullanıcı check-in ve check-out tarihlerini, iki aylık bir görünüm sunan (yan yana
iki takvim) bir takvim bileşeninde seçiyor. Kullanılabilir tarihler aktif/tıklanabilir, host'un
zaten dolu olan veya host tarafından kapatılmış (blocked) tarihleri gri/soluk ve tıklanamaz
gösteriliyor. Host'un ayarladığı minimum konaklama süresinden (ör. "3 gece minimum") kısa bir
aralık seçilmeye çalışıldığında, arayüz bunu engelliyor veya uyarıyor; maksimum konaklama süresi
de (host ayarlamazsa varsayılan olarak 31 gece) benzer şekilde bir üst sınır koyuyor.

**Nerede görülür:** İkisi de. Web'de takvim genelde sayfa içinde bir dropdown/popover olarak
açılıyor; mobilde tam ekran bir bottom sheet/modal halinde, tarihler üstte büyük başlık olarak
seçilirken sayfanın geri kalanı takvime bırakılıyor.

**UX gerekçesi:** Bir konaklama rezervasyonunun en temel iki parametresi (hangi tarihler,
kaç gece) yanlış veya belirsiz olursa tüm sonraki adımlar (fiyat, uygunluk, host onayı) anlamsız
hale geliyor; bu yüzden takvim arayüzünün kullanılamayan tarihleri en baştan görsel olarak devre
dışı bırakması kritik. Airbnb'nin kendi yardım merkezi sayfasına göre host'lar minimum/maksimum
gece sayısını genel olarak ayarlayabildiği gibi, haftanın belirli günleri (ör. Cuma/Cumartesi
check-in için 2 gece minimum) veya belirli mevsimler/tarihler için özel kurallar da
tanımlayabiliyor; ayrıca host'un istediği "önceden haber verme süresi" (advance notice, ör.
en az 1-7 gün önceden rezervasyon) otomatik olarak takvimde tampon (buffer) günler olarak
kapatılıyor. Bu, kullanıcının uygun olmayan bir tarihi seçip sayfanın ilerleyen adımında
("bu tarihler müsait değil" hatasıyla) geri gönderilmesi yerine, hatayı en erken noktada (takvim
etkileşiminin kendisinde) engellemesini sağlıyor - bir tür "erken doğrulama" (early validation)
prensibi.

**Airbnb dışı bir uygulamaya uyarlama notu:** Randevu/rezervasyon/kiralama içeren her platformda
(otel, araç kiralama, etkinlik bileti, klinik randevusu), uygun olmayan tarihleri (dolu, kapalı,
minimum/maksimum süre dışı) takvimde en baştan görsel olarak pasifleştirmek, kullanıcının
sonradan geri dönüp düzeltmesi gereken bir hata mesajı akışından çok daha az sürtünmeli.
Minimum/maksimum kısıtlamalar varsa, kullanıcı geçersiz bir aralığı seçmeye çalıştığında sessizce
reddetmek yerine "bu ilan en az 3 gece için rezerve edilebilir" gibi açık bir mikro-metinle
nedeni belirtmek, kullanıcının "neden seçemiyorum" kafa karışıklığını önler.

**Kaynak / güven notu:** Kısmen doğrulandı. Minimum/maksimum konaklama ayarlarının var olduğu,
31 gecelik varsayılan maksimumun, haftanın günlerine göre özel kuralların ve "önceden haber
verme süresi"nin takvimde otomatik tampon günü olarak kapandığı bilgisi doğrudan fetch edilen
Airbnb yardım merkezi sayfasından (https://www.airbnb.com/help/article/484) geliyor. Ancak bu
sayfa host tarafının ayar ekranını anlatıyor; misafir tarafında takvimin tam görsel dili (gri
tonu, tıklanamayan tarihlerin üzerine gelince çıkan tooltip metni gibi detaylar) genel gözlem/
eğitim verisinden geliyor, ekran görüntüsü üzerinden doğrulanmadı. İki aylık yan yana takvim
görünümü ve mobildeki tam ekran bottom sheet davranışı da eğitim verisinden, birincil kaynakla
teyit edilmedi.

---

## 2. Misafir sayacı: yetişkin / çocuk / bebek / evcil hayvan stepper'ı

**Ne olduğu:** Bir "Misafirler" (Guests) alanına dokunulduğunda açılan bir panelde dört ayrı
satır bulunuyor: Yetişkinler (Adults), Çocuklar (Children), Bebekler (Infants) ve varsa Evcil
hayvanlar (Pets); her satırın yanında bir eksi/artı (stepper) kontrolü var. Bebekler ve evcil
hayvanlar, genelde ilanın izin verdiği maksimum misafir sayısına dahil edilmiyor ama yine de ayrı
sayılıyor ve ilana göre kısıtlanabiliyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb'nin kendi yardım merkezi sayfasına göre 2 yaşın altındaki bebekler
host'un maksimum misafir kapasitesine dahil edilmiyor (sayılmıyor), ama çocuklar bu kapasiteye
dahil ediliyor ve bir host, toplam misafir sayısı (çocuklar dahil) kendi maksimum kapasitesini
aşarsa rezervasyonu reddedebiliyor. Bu ayrım önemli çünkü tek bir "kaç kişi" alanı, farklı
kısıtlama mantıklarına (yatak/oda kapasitesi vs. bebek yatağı/ekstra ücret gibi) sahip misafir
tiplerini birbirine karıştırırdı; ayrı satırlar hem host'un doğru kapasiteyi hesaplamasını hem de
misafirin "bebeğim de sayılıyor mu, ekstra ücret var mı" belirsizliğini baştan çözüyor. Ayrıca
aynı kaynağa göre bir ilan çocuk/bebek için uygun değilse Instant Book devre dışı kalıyor ve
rezervasyon otomatik olarak host onayı gerektiren bir talebe dönüşüyor - yani misafir sayacındaki
seçim, sayfanın ilerisindeki "anında rezervasyon mu, onay bekleyen talep mi" dallanmasını
doğrudan etkiliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kapasiteye bağlı her rezervasyon sisteminde (uçuş,
restoran, etkinlik bileti), farklı fiyatlandırma/kapasite kuralına tabi misafir tiplerini
(yetişkin, çocuk, bebek, evcil hayvan/ekstra eşya) tek bir sayı alanına sıkıştırmak yerine ayrı
steper satırlarına bölmek, hem arka planda doğru kapasite/fiyat hesaplamasını hem de kullanıcının
"bu kategori sayılıyor mu" sorusuna görsel olarak baştan cevap vermesini sağlar. Bir kategori
(ör. bebek) özel bir onay/politika dallanması tetikliyorsa, bunun stepper'ın kendisinde (ör. bir
bilgi ikonuyla) belirtilmesi, sonraki adımda sürpriz bir "host onayı gerekiyor" mesajıyla
karşılaşmayı önler.

**Kaynak / güven notu:** Kısmen doğrulandı. Bebeklerin (2 yaş altı) maksimum kapasiteye dahil
edilmediği, çocukların dahil edildiği ve çocuk/bebek için uygun olmayan ilanlarda Instant Book'un
devre dışı kalıp rezervasyonun otomatik olarak talebe dönüştüğü bilgisi doğrudan fetch edilen
Airbnb yardım merkezi sayfasından (https://www.airbnb.com/help/article/433) geliyor. Evcil hayvan
satırının varlığı ve stepper'ın tam görsel/sıra düzeni (yetişkin-çocuk-bebek-evcil hayvan sırası,
panelin açılma şekli) genel gözlem/eğitim verisinden, birincil kaynaktan ekran görüntüsü
üzerinden doğrulanmadı.

---

## 3. Instant Book vs. Request to Book: ayrım ve mesajlaşma

**Ne olduğu:** Bazı ilanlarda tarih ve misafir seçildikten sonra buton "Confirm and pay" (Onayla
ve öde) yazıyor ve ödeme tamamlandığı an rezervasyon kesinleşiyor (Instant Book). Diğer
ilanlarda buton "Request to book" (Rezervasyon talep et) yazıyor; misafir ödeme bilgisini girip
talebi gönderiyor ama host'un talebi onaylamasına kadar rezervasyon kesinleşmiyor ve
misafirden herhangi bir ücret tahsil edilmiyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb'nin kendi yardım merkezi sayfalarına göre Instant Book ilanları
"host onayı gerektirmiyor", misafir sadece tarih seçip anında rezervasyon yapabiliyor; Request
to Book'ta ise host genelde 24 saat içinde yanıt veriyor, host kabul ederse misafirden ücret
tahsil ediliyor, host reddederse veya yanıt vermezse misafirden hiç ücret alınmıyor ve misafir
başka bir yer için serbestçe rezervasyon yapabiliyor (Hindistan'da farklı bir kural var: talep
anında tahsilat yapılıyor, reddedilirse 15 gün içinde tam iade ediliyor). Bu ayrımın arayüzde
farklı buton metniyle (Confirm and pay / Request to book) baştan işaretlenmesi, kullanıcının
"ödeme yaptığım an yer benim mi oluyor, yoksa host'un onayını mı bekliyorum" belirsizliğini,
ödeme ekranına gelmeden önce çözüyor - bu, finansal bir taahhüdün kesinliğini önceden netleştiren
kritik bir beklenti yönetimi (expectation setting) mekanizması. Ayrıca aynı kaynağa göre, check-
in'e 48 saatten az kalmışsa ve misafirin varış saati host'un check-in penceresi dışındaysa,
normalde Instant Book olan bir ilan bile otomatik olarak Request to Book'a dönüşüyor; bu da
sistemin, riskli/son dakika senaryolarında host'a otomatik bir kontrol noktası eklediğini
gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Arz tarafının (satıcı, hizmet sağlayıcı, ev sahibi)
her rezervasyonu manuel onaylamak isteyebildiği her iki taraflı pazaryerinde (freelancer
platformu, araç paylaşımı, özel ders rezervasyonu), "anında onay" ve "onay bekleyen talep"
akışlarını tek bir CTA'ya gizlemek yerine buton metniyle en baştan ayırmak (ör. "Şimdi Rezerve
Et" vs. "Talep Gönder"), kullanıcının ödeme anındaki taahhüt kesinliğini yanlış anlamasını
önler. Riskli/kenar durumlarda (son dakika, alışılmadık saat) anında onay akışını otomatik
olarak manuel onaya düşürmek, arz tarafına bir güvenlik supabı bırakır.

**Kaynak / güven notu:** Doğrulandı. Instant Book'ta host onayı gerekmediği, Request to Book'ta
host'un genelde 24 saat içinde yanıt verdiği, reddedilme/yanıtsızlık durumunda ücret alınmadığı,
Hindistan'daki farklı kural ve check-in'e 48 saatten az kalan/check-in penceresi dışı taleplerde
Instant Book'un otomatik olarak Request'e dönüşmesi, doğrudan fetch edilen iki Airbnb yardım
merkezi sayfasından (https://www.airbnb.com/help/article/85 ve
https://www.airbnb.com/help/article/1510) birebir doğrulandı. Buton metinlerinin tam ifadesi
("Confirm and pay" / "Request to book") de bu sayfalarda geçiyor.

---

## 4. Fiyat dökümü: gecelik ücret x gece, temizlik ücreti, hizmet ücreti, vergiler, "vergiler
hariç toplam" anahtarı

**Ne olduğu:** Rezervasyon kutusunda/checkout ekranında fiyat, tek bir toplam rakam olarak değil,
alt alta sıralanmış kalemler halinde gösteriliyor: gecelik ücret x gece sayısı, temizlik ücreti
(cleaning fee), Airbnb hizmet ücreti (service fee) ve varsa vergiler (taxes); bunların toplamı
en altta kalın yazıyla veriliyor. Ayrı olarak, arama sonuçları/ilan sayfası/checkout genelinde
kullanıcının açıp kapatabildiği bir "toplam fiyatı göster" anahtarı (toggle) var; açıldığında
kullanıcı arama/liste aşamasından itibaren vergiler hariç ama diğer tüm ücretleri içeren
"all-in" bir toplam görüyor, kapalıyken sadece gecelik/temel ücret görünüyor ve tam döküm
checkout'a kadar gizli kalıyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Fiyat şeffaflığı, konaklama rezervasyonlarında en sık şikayet edilen konulardan
biri: kullanıcı ilanı ucuz sanıp checkout'a geldiğinde temizlik/hizmet ücretleriyle toplamın
belirgin şekilde arttığını görünce "sürpriz ücret" (drip pricing) hissi yaşıyor, bu da sosyal
medyada viral şikayetlere ve terk edilen sepetlere yol açıyor. Kalem kalem döküm göstermek,
kullanıcının "neden bu kadar tuttu" sorusuna somut cevap veriyor ve her ücretin ne için
alındığını (temizlik, platform hizmeti, vergi) ayırt etmesini sağlıyor. "Toplam fiyatı göster"
anahtarının ayrıca arama/liste aşamasına taşınması, sürpriz hissini checkout'a kadar ertelemek
yerine kullanıcının karar sürecinin en başında (hangi ilana bakacağını seçerken) gerçek maliyeti
görmesini sağlıyor; bu değişikliğin arka planda arama sıralama algoritmasına da yansıtıldığı
(yüksek kaliteli ve en iyi toplam fiyatlı ilanların öne çıkarılması) bildiriliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Taban fiyata ek zorunlu ücretler (hizmet ücreti,
işlem ücreti, temizlik/hazırlık ücreti, vergi) ekleyen her platformda (bilet satışı, teslimat,
kiralama), bu ücretleri checkout'un son adımına saklamak yerine mümkün olduğunca erken (ideal
olarak arama/liste aşamasında, en azından ilan detayında) kalem kalem göstermek, "drip pricing"
tepkisini ve sepet terkini azaltır. Kullanıcıya isteğe bağlı bir "tüm ücretler dahil toplamı
göster" anahtarı sunmak, hem şeffaflığı artırıyor hem de bunu istemeyen (ör. sadece gecelik
ücreti karşılaştırmak isteyen) kullanıcıyı da rahatsız etmiyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Toplam fiyat gösterim anahtarının Aralık 2022'de ABD
dahil bazı bölgelerde kullanıma sunulduğu, varsayılan olarak kapalı geldiği, açıldığında
"vergiler hariç ama diğer ücretleri içeren" bir toplamın arama sonuçları/harita/filtre/ilan
sayfasında gösterildiği ve arama sıralama algoritmasının toplam fiyatı önceliklendirdiği bilgisi,
WebSearch üzerinden görülen TechCrunch, Yahoo Finance ve Rental Scale-Up haberlerinin
özetlerinden geliyor; bu kaynakları doğrudan fetch etmedim (TechCrunch/Yahoo Finance sayfalarını
fetch etmedim, news.airbnb.com'daki resmi duyuru sayfası ve skift.com sayfası doğrudan fetch
denemesinde HTTP 403 ile engellendi). Checkout ekranındaki kalem kalem döküm sırası (gecelik x
gece, temizlik, hizmet ücreti, vergi sırası) genel gözlem/eğitim verisinden →
**kısmen doğrulanmadı, çoğunlukla ikincil kaynak özeti**.

---

## 5. Ödeme yöntemi seçimi & bölünmüş ödeme ("şimdi bir kısmını, kalanını sonra öde")

**Ne olduğu:** Checkout'ta bir ödeme yöntemi seçme alanı (kredi/banka kartı, PayPal, Airbnb
kredisi, bazı ülkelerde yerel yöntemler) bulunuyor; uygun rezervasyonlarda bunun yanında bir
"Ödeme planı" (Payment Plan) seçeneği çıkıyor: "Şimdi bir kısmını öde, kalanını sonra öde"
(Pay Part Now, Part Later) veya bazı durumlarda "Şimdi hiç ödeme, sonra tamamını öde" (Reserve
Now, Pay Later). Toplam tutarın ne kadarının hemen, ne kadarının check-in öncesi belirli bir
tarihte tahsil edileceği checkout ekranında açıkça gösteriliyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb'nin kendi yardım merkezi sayfasına göre bu bölünmüş ödeme seçeneği
belirli koşullara bağlı: check-in'e en az 14 gün olması, konaklamanın 28 geceden kısa olması,
kredi/banka kartı, hediye kartı, Airbnb kredisi, PayPal veya ABD banka hesabı gibi belirli ödeme
yöntemleriyle ödenmesi ve kupon/kredi sonrası kalan tutarın en az 50 USD olması gerekiyor;
ödemenin nasıl bölüneceği (genelde ~%50 baştan) rezervasyona ve ilanın iptal politikasına göre
hesaplanıp checkout'ta gösteriliyor, kullanıcı bunu kendisi değiştiremiyor. Bu özellik özellikle
büyük/uzun vadeli rezervasyonlarda ödeme sürtünmesini azaltıyor: kullanıcı büyük bir toplam
tutarı görüp vazgeçmek yerine, daha küçük bir ilk ödemeyle taahhüde girip kalan tutarı check-in
yaklaşırken (daha planı netleşmişken) ödüyor. Bu, finansal riski zaman içinde yaymak isteyen ve
uzun vadeli planlayan kullanıcı segmentini (yüksek değerli, uzun süre önceden rezervasyon yapan
misafirler) hedefleyen bir dönüşüm optimizasyonu.

**Airbnb dışı bir uygulamaya uyarlama notu:** Yüksek tutarlı ve check-in/teslimat tarihi ileride
olan her rezervasyon türünde (tur paketi, etkinlik bileti, düğün/organizasyon hizmeti), tam
tutarı tek seferde tahsil etmek yerine "şimdi küçük bir kısmını, kalanını yaklaşırken öde"
seçeneği sunmak, büyük harcama kararının anlık sürtünmesini azaltıp erken taahhüdü teşvik eder.
Bu seçeneğin şeffaf koşullara (minimum tutar, minimum önceden rezervasyon süresi) bağlanması ve
bölünmüş tutarların checkout'ta net gösterilmesi, kullanıcının "ikinci ödeme ne zaman/ne kadar
çekilecek" belirsizliğini önler.

**Kaynak / güven notu:** Doğrulandı. Bölünmüş ödemenin (Pay Part Now Part Later) ve "hiç ödeme
yapmadan, tamamını sonra öde" (Reserve Now Pay Later) seçeneklerinin var olduğu, uygunluk
koşulları (14 gün önceden, 28 geceden kısa konaklama, belirli ödeme yöntemleri, kupon sonrası en
az 50 USD kalan tutar), bölünmenin rezervasyona/iptal politikasına göre hesaplanıp checkout'ta
gösterildiği ve ikinci ödemeden 3 gün önce hatırlatma e-postası gönderildiği bilgisi doğrudan
fetch edilen Airbnb yardım merkezi sayfasından (https://www.airbnb.com/help/article/2143) birebir
doğrulandı. "~%50 baştan" oranı sayfada "in most cases" ifadesiyle genel bir eğilim olarak
geçiyor, kesin/sabit bir kural olarak değil.

---

## 6. Trip protection / ek sigorta upsell yerleşimi

**Ne olduğu:** Uygun rezervasyonlarda checkout ekranında, ana rezervasyon akışının bir parçası
olarak (ayrı bir sayfaya gitmeden) isteğe bağlı bir "seyahat sigortası" (travel insurance) ekleme
seçeneği çıkıyor; kullanıcı bunu checkout sırasında ya da rezervasyon tamamlandıktan sonra
onay e-postasındaki veya rezervasyon detay sayfasındaki bir "Add to your trip" (Seyahatine ekle)
butonuyla da ekleyebiliyor.

**Nerede görülür:** İkisi de (ABD'de ikamet eden misafirler için; bölgeye göre farklılık
gösterebiliyor).

**UX gerekçesi:** Airbnb'nin kendi yardım merkezi sayfasına göre bu seyahat sigortası ürünü
Airbnb'nin herkese ücretsiz sunduğu AirCover korumasından ayrı, Generali tarafından
sigortalanan ve Airbnb Insurance Agency LLC tarafından satılan, ayrıca ücretli bir üründür; kapsamı
öncelikle "seyahat iptali" (hastalık, uçuş iptali gibi kapsanan bir nedenle rezervasyonu iptal
etmek zorunda kalırsan rezervasyon bedelinin %100'üne kadarının geri ödenmesi) etrafında kurulu.
Bu upsell'in ana rezervasyon akışının içine (ayrı bir sayfaya yönlendirmeden) yerleştirilmesi ve
rezervasyon sonrasında da "Add to your trip" ile tekrar erişilebilir olması, kullanıcının karar
verme baskısını azaltıyor: checkout anında eklemezse bile fikrini değiştirip sonradan ekleyebilme
imkânı bırakıyor, bu da satın alma kararını zorlamak yerine kolaylaştırıyor. Son dakika
rezervasyonlarının bu ürüne uygun olmaması (eligibility kısıtlaması), sigortanın işlevsel
mantığıyla (iptal riskine karşı önceden koruma) tutarlı bir tasarım kararı.

**Airbnb dışı bir uygulamaya uyarlama notu:** Ana hizmetin yanına isteğe bağlı bir sigorta/koruma
ürünü eklemek isteyen her rezervasyon platformunda (uçuş, tur, etkinlik bileti), bu upsell'i ana
akışı bozmayan, atlanabilir ama göz ardı edilemeyecek kadar görünür bir noktaya (checkout'un bir
adımı olarak) yerleştirmek ve rezervasyon sonrasında da (onay e-postası/rezervasyon detay
sayfası üzerinden) tekrar erişilebilir kılmak, kullanıcıyı zorlamadan dönüşümü artırır. Ürünün
temel/ücretsiz korumadan (varsa) net şekilde ayrıştırılması, kullanıcının "zaten ücretsiz koruma
var mıydı, bu neden ayrıca ücretli" karışıklığını önler.

**Kaynak / güven notu:** Doğrulandı. Seyahat sigortasının AirCover'dan ayrı, ücretli ve Generali
tarafından sigortalanan bir ürün olduğu, checkout sayfasında veya rezervasyon sonrası "Add to
your trip" butonuyla eklenebildiği, sadece ABD'de ikamet eden uygun misafirlere sunulduğu ve son
dakika rezervasyonlarının uygun olmayabileceği bilgisi doğrudan fetch edilen Airbnb yardım
merkezi sayfasından (https://www.airbnb.com/help/article/3443) birebir doğrulandı. Upsell'in
checkout ekranındaki tam görsel konumu (fiyat dökümünün üstünde mi altında mı gösterildiği) genel
gözlem/eğitim verisinden, ekran görüntüsü üzerinden doğrulanmadı.

---

## 7. "Review and pay" (gözden geçir ve öde) son onay ekranı

**Ne olduğu:** Ödeme bilgisi girilmeden hemen önceki son adımda, kullanıcının o ana kadar
seçtiği her şeyin (ilan özeti/fotoğrafı, tarihler, misafir sayısı, fiyat dökümü, iptal politikası
özeti, host'un temel kuralları) tek bir ekranda toplandığı bir "gözden geçir" görünümü sunuluyor;
bu ekranın en altında ödeme yöntemi seçimi ve nihai "Confirm and pay" / "Request to book" butonu
yer alıyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Çok adımlı bir karar sürecinin (tarih seç, misafir seç, ilan detaylarını
incele, fiyatı gör) sonunda, ödeme gibi geri dönüşü zor bir eylemden hemen önce tüm seçimleri tek
bir ekranda özetlemek, kullanıcıya son bir "her şey doğru mu" kontrol noktası veriyor; bu hem
yanlışlıkla yanlış tarihle/misafir sayısıyla rezervasyon yapılmasını önlüyor hem de finansal
taahhüdün (toplam tutar, iptal koşulları) son kez, dikkatin dağılmadığı bir noktada net şekilde
görülmesini sağlıyor. Bu tür "özet + onay" ekranları, e-ticaretteki "siparişi gözden geçir"
adımıyla aynı işlevsel mantığa dayanıyor: karmaşık, çok parçalı bir kararı tek bir taahhüt anına
indirgemek.

**Airbnb dışı bir uygulamaya uyarlama notu:** Birden fazla adımda toplanan seçimlerin (tarih,
miktar, seçenek, fiyat) nihai bir ödeme eylemine bağlandığı her akışta (e-ticaret sepeti, bilet
satışı, abonelik kaydı), ödemeden hemen önce tüm seçimlerin tek bir özet ekranında toplanması,
kullanıcı hatalarını (yanlış tarih/miktar) son anda yakalama şansı verir ve finansal/politik
taahhüdün (iptal koşulları gibi) son kez net görülmesini sağlar. Bu ekranın aşırı uzun/karmaşık
olmaması, aksi halde kullanıcının "gözden geçirmeden" hızlıca aşağı kaydırıp geçmesine yol açar.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu ekranın var olduğu ve genel
işlevi (tarih/misafir/fiyat/politika özeti + ödeme adımı) genel ürün gözlemi/eğitim verisiyle
biliniyor; yapılan WebSearch aramaları ("review and pay" screen confirmation) bu spesifik ekranı
birincil ya da güçlü ikincil bir kaynakla belgeleyen bir sonuç döndürmedi, sonuçlar bunun yerine
iptal politikası tiers'ı gibi komşu konulara yönlendirdi. Ekranın tam görsel yerleşimi ve
bölüm sırası tamamen kendi gözlemim/eğitim verim üzerinden yazıldı, hiçbir kaynakla
doğrulanmadı.

---

## 8. Ödeme bilgisi girildikten sonra tarih değiştirme akışı (edit-dates-after-payment)

**Ne olduğu:** Rezervasyon tamamlandıktan sonra misafir fikrini değiştirip tarihleri
değiştirmek isterse, bunu "Trips" (Seyahatlerim) sayfasından ilgili rezervasyonu açıp "Change
reservation" (Rezervasyonu değiştir) butonuyla yeni tarihleri seçip host'a bir değişiklik talebi
gönderiyor; host bu talebi onaylamak veya reddetmek zorunda. Maliyet artarsa fark genelde host
onayladığı an (bazı ödeme yöntemlerinde 48 saate kadar ek süreyle) tahsil ediliyor; maliyet
azalırsa fark iadesi orijinal ödeme yöntemine genelde 15 gün içinde yapılıyor. Instant Book
rezervasyonlarında, check-out'tan en az 72 saat önce yapılan bir uzatma talebi host onayı
beklemeden anında onaylanabiliyor.

**Nerede görülür:** İkisi de (akış web ve mobil uygulamada aynı temel adımları izliyor).

**UX gerekçesi:** Ödeme tamamlandıktan sonra bir rezervasyon "donmuş" (immutable) bir kayıt değil;
seyahat planları sıkça değişiyor, bu yüzden tarihleri sonradan değiştirebilme imkânı, misafirin
tüm rezervasyonu iptal edip yeniden baştan yapmak zorunda kalmasını (ki bu hem iptal
politikasına takılıp para kaybı riski hem de host için gereksiz bir iptal kaydı anlamına
gelirdi) önlüyor. Değişikliğin yine host onayına tabi olması (Instant Book uzatmaları hariç),
host'un takviminde zaten onaylanmış bir rezervasyonun koşullarının tek taraflı değişmemesini
garanti ediyor - bu, host'un kendi takvim kontrolünü korurken misafire esneklik sağlayan bir
denge. Maliyet farkının şeffaf gösterilmesi (eski toplam vs. yeni toplam), misafirin "acaba ek
ücret çıkacak mı" belirsizliğini onaylamadan önce çözüyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Ödeme sonrası değişikliğe açık her rezervasyon
sisteminde (otel, kiralama, randevu), misafirin/müşterinin tüm rezervasyonu iptal edip yeniden
yapmasını gerektirmeyen bir "tarih/detay değiştir" akışı sunmak, gereksiz iptal-yeniden rezervasyon
sürtünmesini ve olası iptal cezası riskini ortadan kaldırır. Değişikliğin karşı tarafın (host,
hizmet sağlayıcı) onayına tabi tutulması, arz tarafının kendi programının kontrolünü kaybetmemesini
sağlar; fiyat farkının açıkça (eski/yeni karşılaştırması) gösterilmesi ise sürpriz ek ücret
hissini önler.

**Kaynak / güven notu:** Doğrulandı. Tarih değiştirme akışının Trips sayfası üzerinden
"Change reservation" ile başladığı, host onayı gerektirdiği, maliyet artışının genelde host
onayı anında (bazı ödeme yöntemlerinde 48 saate kadar ek süreyle) tahsil edildiği, azalışta
iadenin ~15 gün içinde yapıldığı ve Instant Book rezervasyonlarında check-out'tan en az 72 saat
önceki uzatmaların host onayı beklemeden geçebildiği bilgisi doğrudan fetch edilen Airbnb yardım
merkezi sayfasından (https://www.airbnb.com/help/article/913) birebir doğrulandı.

---

## 9. Host onayı bekleme durumu (request-to-book pending state)

**Ne olduğu:** Request to Book ile gönderilen bir talep, host yanıt verene kadar "beklemede"
(pending) bir durumda kalıyor; misafir bu durumu Messages (Mesajlar) bölümündeki host'la olan
mesaj dizisinde veya Trips sayfasında görebiliyor. Host talebi onaylarsa misafire e-posta, SMS
(ayarlıysa) ve push bildirimi gönderiliyor ve bir rezervasyon onay kodu (confirmation code)
oluşuyor; host reddederse veya 24 saat içinde yanıt vermezse misafirden ücret alınmıyor ve
misafir serbestçe başka bir yer arayabiliyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Bir talebin sonucu belirsizken (host yanıt vermeden önce) kullanıcının bu
belirsizliği net bir "beklemede" durumuyla görebilmesi, "acaba rezervasyonum oldu mu, param
çekildi mi" kaygısını azaltıyor; ücretin ancak onaydan sonra çekilmesi, bu bekleme süresince
finansal risk taşımadığını garanti ediyor. Onay/red durumunun birden fazla kanaldan (e-posta,
SMS, push) bildirilmesi, kullanıcının uygulamayı sürekli açıp kontrol etmek zorunda kalmadan
sonucu öğrenmesini sağlıyor - bu, "durumu aktif olarak takip etme" yükünü kullanıcıdan alıp
bildirim sistemine devrediyor. 24 saatlik netlik (yanıt gelmezse otomatik olarak ücretsiz iptal)
ise belirsizliğin süresiz uzamasını engelleyen bir zaman kutusu (timebox).

**Airbnb dışı bir uygulamaya uyarlama notu:** Arz tarafının onayına bağlı her talep akışında
(iş başvurusu, özel ders talebi, hizmet rezervasyonu), talebin "beklemede" durumunu kullanıcıya
net bir durum göstergesiyle sunmak ve ücretlendirmeyi onaydan sonraya ertelemek, bekleme
süresince kullanıcının finansal ve bilgi kaygısını azaltır. Yanıt için üst bir zaman sınırı
(ör. 24 saat) koyup bu süre dolduğunda otomatik/sonuçsuz kapatma tanımlamak, belirsizliğin
süresiz asılı kalmasını önler ve kullanıcıya "başka seçeneğe geçebilirim" netliği verir.

**Kaynak / güven notu:** Doğrulandı. Rezervasyon durumunun Messages bölümündeki mesaj dizisi
üzerinden görüldüğü, host'un 24 saat içinde yanıt verdiği, onay durumunda e-posta+SMS+push
bildirimi ve bir onay kodu oluştuğu, red/yanıtsızlık durumunda ücret alınmadığı bilgisi doğrudan
fetch edilen Airbnb yardım merkezi sayfasından (https://www.airbnb.com/help/article/234) ve
tekrar https://www.airbnb.com/help/article/85 sayfasından doğrulandı. Bekleme durumunun ekranda
tam olarak hangi görsel dille (ör. sarı bir rozet, "Pending" etiketi) gösterildiği ise
kaynakta açıkça anlatılmıyor, genel gözlem/eğitim verisinden.

---

## 10. Rezervasyon sonrası onay ekranı & Trips (Seyahatlerim) sayfasına devir

**Ne olduğu:** Rezervasyon tamamlandığında (Instant Book'ta anında, Request to Book'ta host
onayından sonra) misafire bir onay e-postası gönderiliyor; uygulama/site içinde de bir onay
ekranı gösteriliyor ve rezervasyon artık kullanıcının hesabındaki "Trips" (Seyahatlerim)
bölümünde bir kayıt olarak beliriyor. Trips sayfasından kullanıcı rezervasyon durumunu,
onay kodunu, host'la mesajlaşmayı ve (varsa) rezervasyona bağlı ek hizmetleri (araç servisi,
market teslimatı gibi) tek bir yerden takip edebiliyor; itinerary (seyahat programı) vize
başvurusu gibi amaçlarla indirilip yazdırılabiliyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Checkout akışının bitiş noktası, kullanıcıyı sadece "işlem başarılı" bilgisiyle
bırakmak yerine, rezervasyonu kalıcı ve her an geri dönülebilir bir yere (Trips sayfası)
taşıyarak "bundan sonra ne olacak" sorusuna da cevap veriyor: rezervasyon durumu nerede takip
edilecek, host'la nasıl iletişime geçilecek, iptal/değişiklik nereden yapılacak. Bu, tek seferlik
bir işlem onayından, sürekli erişilebilir bir "seyahat merkezi"ne geçişi temsil ediyor; e-posta
onayının ayrıca kalıcı bir kanal (posta kutusu) üzerinden de erişilebilir olması, kullanıcının
uygulamaya bağımlı kalmadan rezervasyon kanıtına ulaşabilmesini sağlıyor. İtinerary'nin vize
başvurusu gibi resmi amaçlarla indirilebilir/yazdırılabilir olması, rezervasyonun sadece bir
uygulama-içi kayıt değil, resmi bir belge işlevi de görebildiğini gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Herhangi bir rezervasyon/satın alma akışının
sonunda kullanıcıyı sadece bir "başarılı" mesajıyla bırakmak yerine, işlemi kalıcı ve merkezi bir
"siparişlerim/seyahatlerim" bölümüne yönlendirmek, kullanıcının sonraki adımları (durumu takip
etme, iptal/değiştirme, satıcıyla iletişim) nereden yapacağını baştan netleştirir. Onay
bilgisinin hem uygulama içinde hem e-posta gibi kalıcı bir kanalda tekrarlanması, kullanıcının
kanıta her koşulda (uygulamayı silmiş olsa bile) erişebilmesini sağlar.

**Kaynak / güven notu:** Kısmen doğrulandı. Rezervasyon onayının e-posta ile bildirildiği,
rezervasyonun Trips bölümünde bulunabildiği, itinerary'nin vize başvurusu gibi amaçlarla
indirilip yazdırılabildiği ve misafirin konfirmasyon kodunu Messages veya Trips üzerinden
bulabildiği bilgisi, WebSearch üzerinden görülen birden fazla Airbnb yardım merkezi başlığından
(itinerary indirme/yazdırma - help/article/2672, rezervasyon detaylarını yazdırma -
help/article/2639, rezervasyon durumu bulma - help/article/234) geliyor; bu sayfaların hepsini
tek tek fetch edip birebir metnini okumadım, sadece arama motoru özetlerini ve daha önce fetch
edilen article/234'ün ilgili kısmını gördüm. Onay ekranının kendisinin görsel dili (ör.
kutlama animasyonu, onay ikonu) tamamen eğitim verisinden → **kısmen doğrulanmadı**.

---

## 11. İptal politikasının checkout'ta yeniden onaylanması

**Ne olduğu:** İptal politikası, ilan sayfasında bir kez gösterildikten sonra checkout akışında
(ödemeden önce) tekrar, genelde politikanın adıyla (Esnek/Flexible, Orta/Moderate, Sınırlı/
Limited, Katı/Firm gibi) ve varsa iade edilebilir standart oran ile indirimli iade edilemez
(non-refundable) oran arasında bir seçimle birlikte gösteriliyor. Standart politikaların
tamamına, rezervasyon en az 7 gün önceden onaylanmışsa, onaydan sonraki 24 saat içinde ücretsiz
iptal hakkı tanıyan bir "24 saatlik iptal penceresi" ekleniyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb'nin kendi yardım merkezi kaynaklarına göre standart kısa dönem
politikaları dört kademeden oluşuyor: Esnek (check-in'e 24 saat kalana kadar tam iade), Orta
(5 gün kalana kadar tam iade), Sınırlı (14 gün öncesine tam iade, 7-14 gün arası %50 iade) ve
Katı (30 gün öncesine tam iade, 7-30 gün arası %50 iade); bunlara ek olarak tüm standart
politikalarda, rezervasyon check-in'den en az 7 gün önce onaylanmışsa onaydan sonraki 24 saat
içinde vergiler dahil tam iade hakkı tanınıyor. Bu politikanın rezervasyon kararının en son
adımında (ödemeden hemen önce) tekrar gösterilmesi, kullanıcının "acaba fikrim değişirse ne
kadarını geri alırım" finansal riskini, parayı harcamadan önce son kez netleştiriyor; ayrıca
misafirler iptal etmeden önce Trips sayfasından "Cancel reservation" akışına girip iade tutarının
detaylı dökümünü, iptali kesinleştirmeden önizleme olarak görebiliyor. Bu, iptal politikasının
tek seferlik bir ifşa değil, karar (ilan sayfası), taahhüt (checkout) ve olası pişmanlık (iptal
öncesi önizleme) olmak üzere üç ayrı noktada tekrarlanan bir güven mekanizması olduğunu
gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** İptal/iade koşulları olan her rezervasyon veya
satın alma akışında (bilet, kiralama, abonelik), bu koşulları sadece küçük punto bir "şartlar ve
koşullar" linkinde değil, hem karar öncesinde (ürün sayfasında) hem ödeme anında (checkout'ta)
hem de olası bir iptal talebinden hemen önce (iade tutarını önizleyerek) tekrar göstermek, "bunu
bilmiyordum" anlaşmazlıklarını azaltır ve kullanıcının finansal riskini her aşamada yeniden
değerlendirmesine imkân tanır. Standart bir "ilk X saat içinde ücretsiz iptal" penceresi eklemek,
dürtüsel bir rezervasyon sonrası pişmanlığı ücretsiz telafi eden düşük maliyetli bir güven
sinyali olabilir.

**Kaynak / güven notu:** Kısmen doğrulandı. Dört standart iptal politikası kademesinin (Esnek,
Orta, Sınırlı, Katı) tam oranları ve 24 saatlik ücretsiz iptal penceresinin (check-in'den en az
7 gün önce onaylanmış rezervasyonlar için, vergiler dahil tam iade) koşulları doğrudan fetch
edilen Airbnb yardım merkezi sayfasından (https://www.airbnb.com/help/article/475) birebir
doğrulandı; iptal öncesi iade tutarını Trips üzerinden önizleyebilme akışı da doğrudan fetch
edilen başka bir yardım merkezi sayfasından (https://www.airbnb.com/help/article/311)
doğrulandı. Ancak iptal politikasının checkout ekranında **tam olarak nerede/hangi görsel
formatta** (ayrı bir bölüm mü, fiyat dökümünün altında bir satır mı) gösterildiği bu iki kaynakta
da açıkça belirtilmiyor; bu detay genel gözlem/eğitim verisinden → **kısmen doğrulanmadı**.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip birincil/güçlü içerik olarak doğrulanan kaynaklar (Airbnb'nin kendi
  yardım merkezi sayfaları):** Instant Book/Request to Book ayrımı (help/article/85, /1510),
  rezervasyon ayarları/min-max konaklama (help/article/484), iptal politikası kademeleri
  (help/article/475), iptal öncesi iade önizleme (help/article/311), misafir sayacında çocuk/
  bebek sayımı (help/article/433), rezervasyon durumu/host onayı bekleme (help/article/234),
  bölünmüş/planlı ödemeler (help/article/2143), tarih değiştirme akışı (help/article/913) ve
  seyahat sigortası upsell'i (help/article/3443). Bu 10 sayfa, 11 pattern'den 4 tanesini (madde
  3, 5, 6, 8, 9 - toplam 5) doğrudan ve güçlü şekilde, 3 tanesini (madde 1, 2, 11) kısmen
  destekliyor.
- **Doğrudan fetch edilemeyip yalnızca WebSearch özeti üzerinden görülen kaynaklar:** toplam
  fiyat/"vergiler hariç toplam" anahtarı haberleri (TechCrunch, Yahoo Finance, Rental
  Scale-Up); bu konudaki resmi news.airbnb.com duyurusu ve skift.com makalesi doğrudan fetch
  denemesinde HTTP 403 ile engellendi. Trips/itinerary indirme başlıkları da (help/article/2672,
  /2639) yalnızca arama özetinden görüldü, birebir fetch edilmedi.
- **Hiçbir kaynakla doğrulanamayan, tamamen eğitim verisinden yazılan madde:** "review and pay"
  son onay ekranının kendisi (madde 7) - yapılan aramalar bu spesifik ekranı belgeleyen bir
  birincil ya da ikincil kaynak döndürmedi, sonuçlar komşu konulara (iptal politikası) yönlendi.
- Toplamda 11 pattern'den **5 tanesi** (madde 3, 5, 6, 8, 9) doğrudan Airbnb birincil kaynağıyla
  güçlü doğrulandı; **5 tanesi** (madde 1, 2, 4, 10, 11) kısmen doğrulandı (gerçek kaynak fetch
  edildi/görüldü ama ikincil kaynak, kısmi içerik veya görsel detay eksik); **1 tanesi** (madde
  7) tamamen doğrulanmadı/eğitim verisinden. Canlı üründe sürekli A/B test yapıldığından, bu
  doküman "yerleşik/tekrar gözlemlenen pattern" çerçevesiyle okunmalı, "şu an birebir ekran
  görüntüsü" olarak değil.
