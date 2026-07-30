# 5. Güven & Güvenlik Sinyalleri (Trust & Safety UX Signals)

Bu bölüm Airbnb'nin, iki yabancının (host ve misafir) parayı önden verip fiziksel bir mekânı
paylaşmasını gerektiren bir modelde nasıl "güven" ürettiğini kapsıyor: Superhost ve Guest
Favorite rozetleri, kimlik doğrulama (Verified ID) akışı, review'ların gerçekliğini ve
host'un review'lara yanıt verebilmesini sağlayan mekanizmalar, host yanıt oranı/süresi
gösterimi, AirCover garanti mesajlaşması, platform dışı ödemeye karşı uyarı sistemi, profil
eksiksizliği sinyalleri, güvenlik kamerası/gürültü monitörü ifşası, ayrımcılık karşıtı politika
temas noktaları, uyuşmazlık çözümü giriş noktaları ve son olarak Airbnb'nin bilinen bir
zayıflığı olan yıldız puanlama enflasyonu. Diğer bölümlerdeki gibi burada da "şu an tam olarak
böyle" değil, tekrar tekrar gözlemlenen/belgelenen yerleşik pattern'ler anlatılıyor; Airbnb
sürekli değişen canlı bir ürün olduğu için (ör. güvenlik kamerası politikası 2024'te köklü
şekilde değişti) detaylar zamanla farklılaşabilir.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: Airbnb'nin kendi yardım merkezi
sayfalarından Guest Favorite rozeti kriterleri (help/article/3496), kimlik doğrulama süreci
(help/article/1237), AirCover for Hosts kapsamı (help/article/3733), platform dışında ödeme
uyarısı (help/article/199), güvenlik kamerası/gürültü monitörü politikası (help/article/3061),
host yanıt oranı/süresi hesaplama ve gösterimi (help/article/430), review'lara host yanıtı
verme süreci (help/article/32) ve Nondiscrimination Policy / Community Commitment
(help/article/1405); ayrıca akademik bir makale (arXiv 1701.01645, Airbnb review'larında pozitif
yanlılık/inflation üzerine). Nondiscrimination güncellemesini anlatan news.airbnb.com sayfası
403 ile engellendi, sadece WebSearch özeti kullanılabildi. Superhost'un tam sayısal eşikleri
(help/article/829) bu projenin 03 numaralı bölümünde zaten doğrudan fetch edilip doğrulanmıştı;
burada tekrar fetch edilmedi, o bulguya referans veriliyor.

---

## 1. Superhost rozeti: nicel eşiklere dayalı performans rozeti

**Ne olduğu:** Host'un profil fotoğrafının yanında, arama sonuçlarında ve ilan sayfasında beliren
madalyon şeklinde bir rozet ("Superhost"). Bu bölümün 03 numaralı dosyasında zaten doğrudan
Airbnb yardım merkezinden (help/article/829) doğrulanan eşiklere göre: host'un en az 10
rezervasyon (ya da toplam 100 gecelik en az 3 rezervasyon) tamamlamış olması, yeni mesaj ve
rezervasyon taleplerinin en az %90'ına 24 saat içinde yanıt vermesi, %1'in altında iptal oranına
sahip olması ve genel puanının 4.8 veya üzeri olması gerekiyor. Bu bölümde ayrıca doğrudan fetch
edilen help/article/430'a göre Superhost için yanıt oranı hesaplaması, ilan sayfasındaki güncel
"response rate" göstergesinden biraz farklı bir yöntemle, "geçmiş 12 ay boyunca her yeni host/
misafir mesaj dizisine verilen ilk yanıt" temelinde ve üç ayda bir (quarterly) değerlendiriliyor.

**Nerede görülür:** İkisi de (web arama sonucu kartları, ilan sayfası host bölümü, mobil
uygulamada aynı yerler).

**UX gerekçesi:** Bir rozetin güven üretebilmesi için keyfi olmaması, denetlenebilir ve tekrar
ölçülen nicel eşiklere dayanması gerekiyor; aksi halde rozet sadece kozmetik bir "iyi host"
etiketine döner ve zamanla anlamını yitirir. Superhost'un çeyreklik yeniden değerlendirilmesi
(host eşiklerin altına düşerse rozeti kaybedebilir) rozetin "bir kere kazanılır, sonsuza kadar
kalır" değil, sürekli hak edilmesi gereken bir statü olduğunu ima ediyor; bu da misafire "şu an
geçerli" bir güven sinyali veriyor, geçmişte bir kere iyi performans göstermiş ama şimdi
düşmüş bir hostun eski itibarını taşımasını önlüyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Satıcı/hizmet sağlayıcı performans rozeti kurulan
her marketplace'te (freelancer platformu, e-ticaret satıcı rozetleri, sürücü/kurye
uygulamaları), rozetin arkasındaki eşiklerin (a) somut ve ölçülebilir olması, (b) düzenli
aralıklarla yeniden değerlendirilmesi, rozetin uzun vadede güvenilir kalmasını sağlar. Yanıt
oranı hesaplamasında "sadece ilk yanıt" gibi bir kural koymak (takip mesajlarının sayılmaması),
metriğin oyun/manipülasyon edilmesini de zorlaştırır.

**Kaynak / güven notu:** Superhost'un 10 rezervasyon/100 gece, %90 yanıt oranı, %1 iptal, 4.8
puan eşikleri **doğrulandı** (bu projenin 03-listing-detail.md dosyasında help/article/829'dan
doğrudan fetch edilerek zaten teyit edilmişti). Superhost'a özgü yanıt oranı hesaplamasının
"ilk yanıt, geçmiş 12 ay, üç ayda bir değerlendirme" detayı da **doğrulandı**: doğrudan fetch
edilen help/article/430'da "based on your first reply to each new host/guest message thread
over the past 12 months (assessed quarterly)" ifadesi birebir geçiyor.

---

## 2. Guest Favorite rozeti: Superhost'tan bağımsız, ilan-bazlı ikinci bir güven katmanı

**Ne olduğu:** Arama sonuçlarında ve ilan sayfasında beliren, kalp/yıldız temalı ayrı bir
"Guest Favorite" (Misafirlerin Favorisi) rozeti. Doğrudan fetch edilen help/article/3496'ya göre
bu rozetin kazanılması için host'un: en az 5 review'a sahip olması, "mükemmel review'lar"
(check-in, temizlik, doğruluk, host iletişimi, konum ve değer kategorilerinde yüksek puanlar)
alması, host iptalleri ve kalite kaynaklı müşteri hizmeti sorunlarında ortalama %1'lik "üstün bir
güvenilirlik kaydı"na sahip olması ve misafirlerle iletişimin platform üzerinden yürütülmüş
olması gerekiyor.

**Nerede görülür:** İkisi de; arama sonuçlarında ilan kartının üzerinde, ilan sayfasında ise hem
kendi rozeti hem (varsa) Superhost rozetiyle birlikte gösteriliyor.

**UX gerekçesi:** Guest Favorite ve Superhost, kavramsal olarak farklı şeyleri ölçüyor: Superhost
host'un genel operasyonel güvenilirliğini (yanıt hızı, iptal oranı, deneyim), Guest Favorite ise
doğrudan misafir memnuniyetinin review içeriğine yansımasını ölçüyor. Doğrudan fetch edilen
kaynağa göre değerlendirme **her ilan için ayrı ayrı** yapılıyor, yani aynı hostun bir ilanı
Guest Favorite rozetine sahipken diğeri olmayabilir; bu, rozetin "host kişisi" değil "bu belirli
konaklama deneyimi" hakkında bir sinyal olduğunu netleştiriyor. İki rozetin birlikte var
olabilmesi (bir host hem Superhost hem bir ilanı Guest Favorite olabilir) katmanlı bir güven
hiyerarşisi kuruyor: biri "bu hostla çalışmak güvenli", diğeri "bu spesifik yer gerçekten
sevildi".

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir marketplace'te hem satıcı-seviyesinde hem
ürün/ilan-seviyesinde ayrı güven sinyalleri tutmak (ör. "güvenilir satıcı" rozeti + "en çok
beğenilen ürün" rozeti), tek bir kümülatif rozetin veremeyeceği bir ayrım sağlar: bir satıcının
genel güvenilirliği yüksek olsa da belirli bir ürünü/ilanı zayıf olabilir, ya da tam tersi.
Rozetlerin ayrı hesaplanması, kullanıcıya "kime güveniyorum" ile "bu spesifik şeye güveniyorum"
sorularını ayrı ayrı cevaplama imkânı veriyor.

**Kaynak / güven notu:** **Doğrulandı**: eligibility kriterleri (5+ review, altı kategoride
yüksek puan, %1 iptal/kalite sorunu eşiği, platform içi iletişim şartı), Superhost'la ilişkisi
("iki rozet birbirinden bağımsız, ikisi de gösterilebilir, Superhost kriterleri değişmedi") ve
ilan-bazlı değerlendirme ifadesi, doğrudan fetch edilen https://airbnb.com/help/article/3496
sayfasından birebir alındı. Rozetin arama sonucu kartındaki tam piksel/ikon yerleşimi ise genel
gözlemden, ekran görüntüsü üzerinden ayrıca doğrulanmadı.

---

## 3. Kimlik doğrulama (Verified ID) rozeti ve akışı

**Ne olduğu:** Kullanıcının profilinde, profil fotoğrafının yanında beliren kırmızı bir onay
işaretli rozet ve profilde "Identity verified" linki. Doğrudan fetch edilen help/article/1237'ye
göre doğrulama süreci; yasal isim, adres gibi bilgilerin girilmesi, bir devlet kimlik belgesinin
(ehliyet, pasaport, kimlik kartı) fotoğrafının yüklenmesi ve buna ek bir selfie çekilmesinden
oluşuyor; selfie "profilde diğer kullanıcılara gösterilmiyor". Doğrulama tipik olarak 1 saatten
kısa sürede tamamlanıyor. Rozet, kullanıcının doğrulamayı ilk tamamladığı ay/yılı da gösteriyor.

**Nerede görülür:** İkisi de. Guest için checkout sırasında (rezervasyon tamamlanmadan önce),
host için ilk kez ilan oluştururken, co-host için davet kabul ederken zorunlu tutuluyor.

**UX gerekçesi:** Fiziksel bir mülkü/parayı paylaşmayı gerektiren bir modelde en temel güven
sorusu "bu gerçekten var olan, iddia ettiği kişi mi" sorusu; doğrulamayı rezervasyon/ilan
oluşturma akışının **zorunlu** bir adımı yapmak (isteğe bağlı bir "profilini doğrula" önerisi
değil), bu soruyu platform genelinde varsayılan olarak kapatıyor. Selfie'nin diğer kullanıcılara
hiç gösterilmemesi, doğrulama sürecinin kendisinin (kimin gerçek olduğunun teyit edilmesi) ile
sosyal görünürlüğün (profil fotoğrafı) ayrı tutulduğunu gösteriyor; bu, kullanıcıların
doğrulama için ekstra bir hassas veri (selfie) paylaşmaya daha az çekinerek razı olmasını
sağlıyor çünkü bu verinin herkese açık profilde son bulmayacağını biliyorlar.

**Airbnb dışı bir uygulamaya uyarlama notu:** Güvenin özellikle kritik olduğu (yüksek finansal
risk, fiziksel buluşma içeren) her iki taraflı marketplace'te (araç paylaşımı, ikinci el yüksek
değerli eşya satışı, hizmet randevusu), kimlik doğrulamasını isteğe bağlı bir "profil rozeti"
olarak değil, ana akışın (ilk ilan verme, ilk rezervasyon tamamlama) zorunlu bir kapısı olarak
tasarlamak katılımı büyük ölçüde artırır. Doğrulama için toplanan hassas belgelerin
(kimlik, selfie) diğer kullanıcılara hiç gösterilmeyeceğini açıkça belirtmek, kullanıcı
tarafındaki gizlilik kaygısını azaltır.

**Kaynak / güven notu:** **Doğrulandı**: doğrulama için istenen bilgiler (yasal isim/adres,
devlet kimliği fotoğrafı, selfie), selfie'nin profilde gösterilmediği, doğrulamanın host/co-host/
misafir için ne zaman zorunlu olduğu, 1 saatten kısa işlem süresi ve rozetin "ay/yıl" gösterdiği
bilgileri doğrudan fetch edilen https://www.airbnb.com/help/article/1237 sayfasından geliyor.
Rozetin tam görsel biçimi ("kırmızı onay işareti, profil fotoğrafının yanında") da aynı sayfada
belirtiliyor ancak bunun güncel tasarım dilinde (DLS) tam olarak nasıl render edildiği ekran
görüntüsüyle ayrıca doğrulanmadı.

---

## 4. Review authenticity sinyalleri: sadece tamamlanmış konaklamadan gelen review + host'un public yanıtı

**Ne olduğu:** İki ayrı ama birbiriyle ilişkili mekanizma. Birincisi, Airbnb'nin review sistemi
yalnızca gerçekten tamamlanmış bir rezervasyonu olan misafirlerin o ilana review bırakabilmesine
izin veriyor (bu projenin 03 numaralı bölümünde zaten doğrulanan help/article/13'teki review
sıralama/arama sistemi de bu temel varsayım üzerine kurulu). İkincisi, host'lar kendilerine
yazılan herhangi bir review'a **herkese açık, tek seferlik bir yanıt** yazabiliyor: doğrudan
fetch edilen help/article/32'ye göre host, Profil > Reviews > "Reviews about you" yolunu izleyip
ilgili review'ın altındaki "Leave public response" ile yanıt yazıyor; bu yanıt "hemen
yayınlanıyor" ve review'ın hemen altında, gelecekteki host ve misafirlerin göreceği şekilde
kalıcı olarak görünüyor.

**Nerede görülür:** İkisi de görüntüleme tarafında (yayınlanan yanıt herkese, her platformda
görünür); yanıt yazma akışı ise doğrudan fetch edilen kaynağa göre 2024 itibarıyla **sadece
masaüstü/mobil web'den** yapılabiliyor, native mobil uygulamadan host review'a yanıt yazamıyor.

**UX gerekçesi:** "Sadece gerçek konaklamadan review" kuralı, review sisteminin en temel
güvenilirlik temelini oluşturuyor: rakip bir hostun ya da hiç konaklamamış birinin sahte olumsuz
(veya olumlu) review yazmasını yapısal olarak engelliyor. Host'un public yanıt hakkı ise farklı
bir sorunu çözüyor: tek taraflı bir review, anlaşmazlık durumunda (ör. misafirin kural ihlali,
yanlış anlaşılma) sadece misafirin versiyonunu kalıcı olarak sergiler; host'a yanıt hakkı
tanımak, gelecekteki okuyucuya "bu review'ın iki tarafı var" sinyalini veriyor ve review'ın
tek taraflı bir mahkumiyet gibi okunmasını yumuşatıyor. Yanıtın "hemen yayınlanması" (host'un
düzenleyip onaylatması gerekmemesi) sürtünmesiz bir mekanizma sağlıyor, ama bu aynı zamanda
host'un öfkeli/savunmacı bir yanıt yazıp geri alamamasının riskini de taşıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcı review'larının biriktiği her platformda
(e-ticaret, hizmet pazaryeri, iş değerlendirme siteleri), review'ı sadece doğrulanmış işlem
sahiplerine açmak sahte review riskini yapısal olarak azaltır; satıcıya/hizmet sağlayıcıya
herkese açık, kalıcı bir yanıt hakkı tanımak ise anlaşmazlık durumlarında tek taraflı bir
anlatının kalıcılaşmasını önler. Yanıtın anında yayınlanması kullanıcı deneyimini hızlandırırken,
"gönder" öncesi kısa bir önizleme/geri alma penceresi eklemek (Airbnb'nin şu an sunmadığı
görünen bir özellik) savunmacı/pişman olunacak yanıtların önüne geçebilir.

**Kaynak / güven notu:** Host'un public yanıt akışı, adım adım yolu, yanıtın anında ve kalıcı
yayınlanması **doğrulandı**: doğrudan fetch edilen https://www.airbnb.com/help/article/32
sayfasından birebir alındı; ayrıca "mobil uygulamadan yanıt yazılamıyor, sadece mobil web'den"
detayı WebSearch özetinde ikincil bir kaynaktan (host rehberi sitesi, "as of July 2024" notuyla)
görüldü, bu spesifik alt-detay birincil kaynaktan tam doğrulanmadı → **kısmen doğrulanmadı**.
"Sadece tamamlanmış konaklamadan review" kuralının kendisi, bu projenin 03 numaralı bölümünde
zaten help/article/13 ve 1257'den dolaylı olarak destekleniyor (review sisteminin rezervasyon
temelli çalıştığı örtük varsayımı), ancak bu spesifik cümle ("sadece gerçek konaklayanlar review
bırakabilir") birebir alıntılanarak ayrıca doğrulanmadı → **kısmen doğrulanmadı, dolaylı
kaynak**.

---

## 5. Host yanıt oranı / yanıt süresi gösterimi

**Ne olduğu:** İlan sayfasının alt kısmında (doğrudan fetch edilen kaynağa göre "her ilan
sayfasının en altında") host'un ne kadar hızlı yanıt verdiğini gösteren bir metin: "yanıt oranı"
(response rate, geçmiş 30 gündeki yeni sorulara/rezervasyon taleplerine 24 saat içinde yanıt
verme yüzdesi) ve "yanıt süresi" (response time, geçmiş 30 gündeki tüm yeni mesajlara ortalama
yanıt verme hızı, ör. "1 saatten az", "birkaç saat içinde").

**Nerede görülür:** İkisi de; ilan sayfasının alt kısmında, genelde host bölümüyle birlikte veya
ona yakın.

**UX gerekçesi:** Rezervasyon öncesi bir misafirin en somut belirsizliklerinden biri "acil bir
sorum olursa ya da check-in'de bir sorun çıkarsa host bana ne kadar sürede döner" sorusu. Bu
metriği ham bir sayı/yüzde olarak göstermek, host'un iletişim kalitesini kullanıcının kendi
öznel izlenimine (ör. "profil fotoğrafı güven verici görünüyor mu") değil, geçmiş 30 günlük
gerçek davranışsal veriye dayandırıyor. Doğrudan fetch edilen kaynağa göre bu metrik hem arama
sıralamasını hem Superhost uygunluğunu etkiliyor, yani sadece pasif bir bilgi değil, host'u hızlı
yanıt vermeye teşvik eden bir davranışsal geri bildirim döngüsünün parçası; "sadece ilk yanıt
sayılır, takip mesajları sayılmaz" kuralı da metriğin manipülasyona (ör. host'un sürekli kısa
otomatik mesajlarla "yanıt vermiş" gibi görünmesi) karşı bir miktar dayanıklı olmasını sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Alıcı-satıcı/misafir-host iletişiminin kritik
olduğu her marketplace'te, satıcının/hizmet sağlayıcının yanıt hızını geçmiş davranışsal veriye
dayalı somut bir metrik olarak (öznel bir "iyi iletişimci" etiketi değil) göstermek, alıcının
riskini önceden azaltır. Metriğin arama sıralamasını da etkilemesi, satıcıları bu davranışı
sürdürmeye teşvik eden bir ikinci katman oluşturuyor; ancak metriğin nasıl hesaplandığının
(ör. "sadece ilk yanıt sayılır") açık olması, satıcıların metriği manipüle etmeye çalışmasını
zorlaştırmak için önemli.

**Kaynak / güven notu:** **Doğrulandı**: yanıt oranı/süresinin ilan sayfasının en altında
gösterildiği, hesaplama yöntemi (24 saat içinde yanıt, geçmiş 30 gün, sadece ilk yanıtın
sayılması), arama sıralamasına ve Superhost uygunluğuna etkisi, doğrudan fetch edilen
https://www.airbnb.com/help/article/430 sayfasından birebir alındı.

---

## 6. AirCover / garanti mesajlaşması

**Ne olduğu:** Airbnb'nin host ve misafirlere sunduğu birleşik bir koruma paketinin markalı adı:
"AirCover". Doğrudan fetch edilen help/article/3733'e göre host'lar için bu paket; misafir kimlik
doğrulaması, rezervasyon taraması (screening), 3 milyon dolara kadar host hasar koruması (host
damage protection), 1 milyon dolarlık host sorumluluk sigortası, Experiences/Services için ayrı
1 milyon dolarlık sorumluluk sigortası ve 24 saat güvenlik hattını kapsıyor. Misafirler için ayrı
bir "AirCover for guests" paketi ise host rezervasyonu iptal ederse, check-in yapılamazsa veya
ilan tarif edildiğinden ciddi şekilde farklıysa ve host durumu düzeltemiyorsa (ya da misafir
kendini güvensiz hissediyorsa) destek sağlıyor.

**Nerede görülür:** İkisi de; ancak doğrudan fetch edilen kaynak host tarafına odaklı bir yardım
merkezi sayfası olduğu için, bu paketin rezervasyon/checkout akışında (ör. ödeme ekranında bir
banner/rozet olarak) veya ilan sayfasında tam olarak nerede ve nasıl görsel olarak
mesajlandığına dair bilgi bu kaynakta yer almıyor.

**UX gerekçesi:** Karşı taraf hakkında (host'un evi gerçekten var mı, tarif edildiği gibi mi;
misafir eve zarar verir mi) tam bilgiye sahip olmadan para transferi yapmak, klasik bir
"asimetrik bilgi" güven sorunu. AirCover, bu riski platformun kendisinin sigorta/garanti olarak
üstlenmesi: misafire "bu ilan yanlışsa/host ortadan kaybolursa yalnız kalmayacaksın", host'a
"misafir evine zarar verirse tek başına kalmayacaksın" güvencesi vererek, iki tarafın da
karşılıklı olarak birbirine güvenmek yerine platforma güvenmesini sağlıyor; bu, marketplace'in
kendisinin bir güven aracısı (trust broker) rolü üstlenmesinin somut bir örneği.

**Airbnb dışı bir uygulamaya uyarlama notu:** Yüksek riskli, geri alınamaz işlemler içeren her
iki taraflı marketplace'te (kısa süreli kiralama, ikinci el yüksek değerli eşya, freelance
hizmet), platformun kendisinin bir garanti/sigorta katmanı sunması, iki tarafın birbirine
doğrudan güvenmesi gereken yükü azaltıp bu güveni platforma taşıyor. Böyle bir paketi tek,
akılda kalıcı bir markalı isim (AirCover gibi) altında birleştirmek, dağınık/parça parça
koruma maddelerini kullanıcının anlayabileceği tek bir "güvencem var" hissine dönüştürüyor.

**Kaynak / güven notu:** Kısmen doğrulandı. AirCover for Hosts'un kapsamı ve tam dolar
rakamları ($3M hasar koruması, $1M sorumluluk sigortası, $1M Experiences sorumluluk sigortası,
24 saat güvenlik hattı) **doğrulandı**, doğrudan fetch edilen
https://www.airbnb.com/help/article/3733 sayfasından birebir alındı. AirCover for guests'in
kapsamı da WebSearch özetinde ikincil olarak (help/article/3227 başlığından) görüldü ama bu
sayfa ayrıca fetch edilmedi → **kısmen doğrulanmadı**. Paketin rezervasyon/checkout akışında
veya ilan sayfasında tam olarak nasıl ve nerede görsel olarak sunulduğu (banner, rozet, ayrı bir
"AirCover ile korunuyorsunuz" bölümü gibi) hiçbir kaynaktan doğrulanmadı, tamamen genel
gözlem/eğitim verisinden → **doğrulanmadı, eğitim verisinden**.

---

## 7. Platform içi güvenli ödeme + "asla platformun dışında ödeme yapma/iletişim kurma" uyarısı

**Ne olduğu:** Airbnb, ödeme ve iletişimin rezervasyon onaylanana kadar tamamen kendi platformu
üzerinden yürütülmesini politika olarak zorunlu kılıyor ve bunun dışına çıkan taleplere karşı
kullanıcıyı aktif olarak uyarıyor. Doğrudan fetch edilen help/article/199'a göre bir host'un
misafirden rezervasyon öncesi platform dışında ödeme yapmasını veya iletişim kurmasını istemesi
"Airbnb politikasına aykırı"; platform dışında ödeme yapan bir misafir AirCover korumasını
kaybediyor. Sayfa ayrıca somut bir "dolandırıcılık uyarı işaretleri" listesi veriyor: banka/havale
transferi talepleri, kasa çeki (cashier's check), para havalesi (money order) ve PDF/kağıt fatura
istekleri.

**Nerede görülür:** İkisi de; bu uyarı hem yardım merkezi sayfası olarak hem de (WebSearch
özetlerine göre, doğrudan fetch edilerek teyit edilmedi) mesajlaşma arayüzünde platform dışı bir
bağlantı/telefon numarası/e-posta paylaşılmaya çalışıldığında otomatik olarak tetiklenen bir
uyarı/engelleme mekanizması şeklinde uygulanıyor olabilir.

**UX gerekçesi:** Bir dolandırıcının en yaygın stratejisi, kurbanı güvenli/denetimli bir kanaldan
(platform içi ödeme, ki burada Airbnb işlemi izleyebiliyor, anlaşmazlıkta müdahale edebiliyor)
denetimsiz bir kanala (banka havalesi, nakit) çekmek. Airbnb'nin bunu sadece "yapmayın" diye
pasif bir öneri olarak değil, somut ve tanınabilir uyarı işaretleri (kasa çeki, para havalesi
gibi spesifik ödeme yöntemleri) listeleyerek anlatması, kullanıcıya "şüpheli" kavramını soyut
bırakmak yerine denetlenebilir bir kontrol listesi veriyor. Ayrıca platform dışı ödemenin somut
bir bedeli olduğunu (AirCover korumasının kaybı) açıkça belirtmek, kullanıcıya "bunu yaparsam
neyi riske atıyorum" sorusuna dolaylı bir korku yerine net bir cevap veriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Ödemenin platform içinde tutulmasının hem
dolandırıcılık riskini azalttığı hem de platformun komisyon/güven modelini koruduğu her
marketplace'te (freelance platformları, ikinci el satış siteleri, hizmet pazaryerleri), kullanıcı
uyarısını soyut bir politika cümlesi olarak değil, somut/tanınabilir dolandırıcılık işaretleri
(spesifik ödeme yöntemleri, tipik cümle kalıpları) listesi şeklinde sunmak daha etkili. Platform
dışına çıkmanın kaybettireceği somut korumayı (garanti, anlaşmazlık çözümü, sigorta) açıkça
belirtmek, kullanıcının kısa vadeli cazibeye (ör. "komisyonsuz daha ucuz" teklifi) karşı akılcı
bir karşı-argümana sahip olmasını sağlıyor.

**Kaynak / güven notu:** **Doğrulandı**: platform dışı ödeme/iletişimin politika ihlali olduğu,
AirCover korumasının kaybedildiği ve dolandırıcılık uyarı işaretleri (banka/havale transferi,
kasa çeki, para havalesi, PDF/kağıt fatura) doğrudan fetch edilen
https://www.airbnb.com/help/article/199/paying-outside-airbnb sayfasından birebir alındı.
Mesajlaşma arayüzünde platform dışı iletişim girişiminin otomatik olarak tespit edilip
engellendiği/uyarıldığı mekanizmasının varlığı ise bu araştırmada doğrudan fetch edilerek
doğrulanmadı, genel gözlem/eğitim verisinden → **kısmen doğrulanmadı**.

---

## 8. Profil eksiksizliği sinyalleri (fotoğraf, biyografi, verifications listesi)

**Ne olduğu:** Hem host hem misafir profil sayfasında, gerçek bir kişi olduğunu ve platforma
"yatırım yapmış" olduğunu gösteren bir dizi tamamlanabilir alan: profil fotoğrafı, kısa bir
biyografi/"Hakkımda" bölümü (iş, okul, konuştuğu diller gibi alt alanlarla), ve bir
"doğrulamalar" (verifications) listesi (e-posta, telefon numarası, devlet kimliği gibi hangi
doğrulamaların tamamlandığının bir listesi). Bu bölümler genelde profilin görsel olarak üst
kısmında, doğrudan kimlik doğrulama rozetiyle yan yana gösteriliyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Boş veya minimal bir profil (fotoğrafsız, biyografisiz), karşı tarafta
"bu kişi platforma hiç yatırım yapmamış, belki de sahte/geçici bir hesap" izlenimi uyandırıyor;
tersine, dolu bir profil (fotoğraf + biyografi + birden fazla doğrulama), her ne kadar bu
bilgilerin hiçbiri tek başına "güvenilirlik" kanıtlamasa da, toplamda bir "bu gerçek, yerleşik
bir kullanıcı" izlenimi yaratıyor. Doğrulamaların ayrı bir liste olarak (tek bir rozet yerine)
gösterilmesi, kullanıcının "hangi güven katmanları var" sorusunu parça parça inceleyebilmesini
sağlıyor: sadece e-posta doğrulanmış biriyle hem e-posta hem telefon hem kimlik doğrulanmış
birini ayırt edebiliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcı profillerinin güven taşıdığı her
platformda (marketplace, sosyal ağ, freelance sitesi), profil eksiksizliğini tek bir ikili
(tamam/eksik) durum olarak değil, kademeli bir doğrulama listesi (e-posta > telefon > kimlik >
ödeme yöntemi gibi artan güven seviyeleri) olarak sunmak, kullanıcıya hem kendi profilini
geliştirmesi için somut bir yol haritası veriyor hem de karşı tarafa "bu kişi ne kadar
doğrulanmış" sorusuna kademeli bir cevap sağlıyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu maddedeki bilgiler tamamen
genel gözlem ve ikincil/üçüncü taraf host rehberi sitelerinin (igms.com, optimizemyairbnb.com)
WebSearch özetlerinden geliyor; bu sayfalar doğrudan fetch edilip birebir metinleri okunmadı,
Airbnb'nin kendi yardım merkezinden profil eksiksizliği/verifications listesinin tam biçimini
doğrulayan bir sayfa da bulunup fetch edilmedi. Bu madde diğerlerine kıyasla en zayıf kaynaklı
olanı.

---

## 9. Güvenlikle ilgili ilan bilgisi ifşası: güvenlik kamerası ve gürültü monitörü

**Ne olduğu:** İlan sayfasında "Guest safety" (Misafir güvenliği) başlığı altında, host'un
mülkünde bulunan gözetim/izleme cihazlarının ifşa edildiği bir bölüm. Doğrudan fetch edilen
help/article/3061'e göre: iç mekân güvenlik kameraları ve kayıt cihazları **tamamen yasak**
(gizli kameralar kesinlikle yasak; yatak odası, banyo, koridor, oturma odası veya misafir evinin
iç mekânında hiçbir kamera bulunamaz, kapalı olsa/bağlantısı kesilmiş olsa bile). Dış mekân
kameraları (ön bahçe, veranda, havuz, kapı zili kamerası) izinli ama host bu cihazın varlığını ve
genel konumunu ilanında açıklamak zorunda. Gürültü desibel monitörleri (ses seviyesini ölçen ama
sesi/konuşmayı kaydetmeyen cihazlar) yatak odası, banyo ve uyku alanları dışında iç mekânda
bulunabilir; bunların varlığı ifşa edilmeli ama tam konumu belirtilmek zorunda değil.

**Nerede görülür:** İkisi de; ilan sayfasında "Guest safety" bölümünde host'un doldurduğu
"Safety devices" alanı olarak.

**UX gerekçesi:** Bir misafirin fiziksel olarak içinde kalacağı bir mekânda gözetlenip
gözetlenmediğini bilmesi, mahremiyet ve güvenlik açısından temel bir bilgi; bunu isteğe bağlı bir
"sorarsan söylerim" modeline değil, ilan sayfasının kendisinde zorunlu bir ifşa alanına
dönüştürmek, misafirin bu bilgiyi aramak zorunda kalmadan (ya da hiç bilmeden) rezervasyon
yapmasını önlüyor. İç mekân kameralarının 2024'teki köklü politika değişikliğiyle **tamamen**
yasaklanması (önceden sadece ortak alanlarda izinliyken), platformun bu konuda zamanla daha katı
bir çizgiye geçtiğini gösteriyor; bu, "misafirin mahremiyeti, host'un gözetim ihtiyacından daha
öncelikli" şeklinde bir politika duruşu.

**Airbnb dışı bir uygulamaya uyarlama notu:** Misafirin/kullanıcının fiziksel olarak bir üçüncü
tarafın mülkünde bulunduğu her hizmette (kısa süreli kiralama, özel ders/randevu evde, co-living),
gözetim cihazlarının varlığını zorunlu ve yapılandırılmış bir ifşa alanında (serbest metin
açıklamasına gömülü değil, ayrı bir "güvenlik" bölümünde) göstermek, mahremiyet beklentisiyle
ilgili anlaşmazlıkları önden azaltır. İç mekân/dış mekân, ses/görüntü gibi net kategorik
ayrımlar koymak (Airbnb'nin yaptığı gibi), "hangi cihaz izinli hangisi değil" belirsizliğini
ortadan kaldırıyor.

**Kaynak / güven notu:** **Doğrulandı**: iç mekân kameralarının tamamen yasak olduğu (gizli
kameralar dahil, kapalı/bağlantısız cihazlar dahil), dış mekân kameralarının izinli ama ifşa
zorunlu olduğu, gürültü desibel monitörlerinin yatak odası/banyo/uyku alanı dışında izinli ve
ifşa zorunlu (ama konum zorunlu değil) olduğu, ve bu ifşanın ilan sayfasında "Guest safety"
bölümünde göründüğü, doğrudan fetch edilen https://www.airbnb.com/help/article/3061 sayfasından
birebir alındı. 2024 politika değişikliğinin tam tarihi ve önceki kuralın tam içeriği ise
WebSearch özetlerinde ikincil kaynaklardan (noiseaware.com, turno.com blog yazıları) görüldü,
birincil kaynaktan (news.airbnb.com'daki resmi duyuru) doğrudan fetch edilerek teyit edilmedi
→ bu alt-detay **kısmen doğrulanmadı**.

---

## 10. Topluluk standartları / ayrımcılık karşıtı politika temas noktaları

**Ne olduğu:** Airbnb'nin, tüm kullanıcıların (host ve misafir) platformu kullanabilmek için
kabul etmesi gereken iki politikası: "Community Commitment" (Topluluk Taahhüdü) ve
"Nondiscrimination Policy" (Ayrımcılık Karşıtı Politika). Doğrudan fetch edilen
help/article/1405'e göre Community Commitment, kullanıcıların birbirine "ırk, din, ulusal
köken, etnik köken, engellilik durumu, cinsiyet, cinsiyet kimliği, cinsel yönelim veya yaş
gözetmeksizin saygıyla davranmayı" taahhüt etmesini istiyor; Nondiscrimination Policy ise bunu
somutlaştırıp 14 korunan özelliğe (ırk, din, cinsiyet, yaş, engellilik, ailevi durum, medeni
durum, etnik köken, ulusal köken, cinsel yönelim, cinsiyet, cinsiyet kimliği, kast ve hamilelik)
göre ayrımcılığı yasaklıyor. Bu taahhüdü reddeden kullanıcıların hesabı otomatik olarak devre dışı
bırakılıyor; WebSearch özetine göre 2016'dan bu yana 2,5 milyondan fazla kişi bu taahhüdü kabul
etmediği için platforma erişimi engellenmiş ya da platformdan çıkarılmış.

**Nerede görülür:** İkisi de; hesap oluşturma/onboarding sürecinde bir onay adımı olarak, ayrıca
yardım merkezinde ayrı bir politika sayfası olarak.

**UX gerekçesi:** Airbnb'nin iş modeli, gerçek insanların gerçek evlerini/mekânlarını
paylaşmasına dayandığı için, ayrımcılık riski (bir hostun belirli bir ırk/etnik kökenden
misafiri reddetmesi gibi) platformun itibarını doğrudan tehdit eden, tekrar tekrar kamuoyunda
tartışılmış bir sorun (Airbnb'nin kendi "Project Lighthouse" girişimi de bu sorunu ölçmek için
kurulmuş). Bu taahhüdü bir "küçük yazı" (kullanım şartlarının derinlerine gömülü bir madde)
olarak değil, ayrı adlandırılmış (Community Commitment) ve reddedilmesi hesabın devre dışı
bırakılmasıyla sonuçlanan aktif bir onay adımı yapmak, platformun bu konuda "isteğe bağlı bir
öneri değil, kullanım şartı" mesajı vermesini sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** İki taraflı, insan-insana etkileşim içeren her
marketplace'te (ev paylaşımı, araç paylaşımı, hizmet pazaryeri), ayrımcılık karşıtı politikayı
kullanım şartlarının bir alt maddesi olarak gömmek yerine ayrı adlandırılmış, aktif onay
gerektiren bir taahhüt haline getirmek (ve reddedilmesinin somut bir sonucu -hesap devre dışı
kalma- olması), platformun bu konudaki duruşunu kullanıcıya açıkça ve erken iletir. Ayrımcılık
algısını ölçmek için kendi verisi üzerinde düzenli analiz yapan bir program (Project Lighthouse
benzeri) kurmak, politikanın sadece kağıt üzerinde kalmadığını, ölçülüp iyileştirildiğini
gösteriyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Community Commitment'ın tam metni (korunan
özellikler listesi dahil, ırk/din/ulusal köken/etnik köken/engellilik/cinsiyet/cinsiyet kimliği/
cinsel yönelim/yaş), Nondiscrimination Policy'nin 14 korunan özelliği ve ihlal durumunda
uygulanan yaptırımlar (eğitim, uyarı, ilan/hesap askıya alma) **doğrulandı**, doğrudan fetch
edilen https://www.airbnb.com/help/article/1405 sayfasından birebir alındı. "2016'dan bu yana
2,5 milyondan fazla kişinin erişiminin engellendiği" rakamı ve Project Lighthouse'un varlığı ise
WebSearch özetinde news.airbnb.com kaynaklarına atıfla görüldü; bu sayfayı (news.airbnb.com/
nondiscriminationupdate) doğrudan fetch etmeye çalıştım ama **403 Forbidden** hatası aldım,
dolayısıyla bu spesifik rakam ve Project Lighthouse detayları ikincil/özet düzeyinde kaldı →
**kısmen doğrulanmadı**.

---

## 11. Uyuşmazlık çözümü / "Get Help" giriş noktaları (Resolution Center)

**Ne olduğu:** Airbnb'nin, host-misafir arasında para veya kural anlaşmazlığı çıktığında
kullanılan "Resolution Center" (Çözüm Merkezi) adlı bir araç ile, daha genel "Get Help"/destek
başvurusu akışı. Doğrudan fetch edilen help/article/1542'ye göre kullanıcılar Airbnb'ye mesaj/
canlı sohbet üzerinden ("var olan bir sorunu takip edebilir veya 'Report a new issue' seçebilir")
ya da telefonla ulaşabiliyor; Resolution Center ise "orijinal ilanda kapsanmayan konular için
güvenli bir şekilde ödeme talep etmeyi veya göndermeyi" sağlıyor ve taraflar arasında
aracılı bir görüşme mekanizması işlevi görüyor. WebSearch özetlerine göre uygulama içinde bu akışa
erişim genelde bir mesaj dizisi içindeki "Details" > "Get Help" yolundan gerçekleşiyor.

**Nerede görülür:** İkisi de; mesajlaşma ekranı içinden erişilen bir alt-akış olarak (ayrı,
öne çıkan bir ana navigasyon sekmesi değil).

**UX gerekçesi:** Bir anlaşmazlık anında kullanıcının en büyük ihtiyacı, "bu sorunu nereye
anlatacağım" belirsizliğini hızlıca çözmek; Resolution Center'ı genel destek akışından ayrı,
parasal taleplerle sınırlı özel bir araç olarak tanımlamak, "para isteme/gönderme" gibi hassas ve
potansiyel olarak kötüye kullanılabilir bir işlevi (ör. dolandırıcılık riski) genel mesajlaşmadan
ayrı, izlenebilir/denetlenebilir bir kanala hapsediyor. Bunun mesajlaşma ekranının içine
gömülmüş olması (ayrı bir ana sekme değil), aracın çoğunlukla zaten var olan bir host-misafir
konuşmasının devamı olarak kullanılacağı varsayımını yansıtıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** İki taraflı ödeme/anlaşmazlık riski taşıyan her
marketplace'te, parasal taleplerin genel mesajlaşmadan ayrı, platform tarafından izlenebilen
ve gerektiğinde aracılık edilebilen özel bir kanaldan (kullanıcıların birbirine doğrudan
banka bilgisi paylaşmasından ayrı) geçmesi, hem dolandırıcılık riskini azaltır hem de anlaşmazlık
çıktığında platformun elinde somut bir kayıt/kanıt bulunmasını sağlar.

**Kaynak / güven notu:** Kısmen doğrulandı. Resolution Center'ın "orijinal ilanda kapsanmayan
konular için ödeme talep etme/gönderme" işlevi ve genel destek erişim yöntemleri (mesaj/sohbet,
telefon) doğrudan fetch edilen https://www.airbnb.com/help/article/1542 sayfasından geliyor.
Ancak uygulama içindeki tam navigasyon yolu ("mesaj > Details > Get Help > Go to details page")
WebSearch özetinde bir topluluk forumu gönderisinden (community.withairbnb.com, resmi olmayan bir
kullanıcı açıklaması) geldi, birincil kaynaktan doğrulanmadı → **kısmen doğrulanmadı**.

---

## 12. Yıldız puanlama enflasyonu: Airbnb'nin bilinen zayıflığı ve platformun buna yaklaşımı

**Ne olduğu:** Airbnb'nin 5 yıldızlı review sisteminde, puanların neredeyse tamamının skalanın en
üst ucunda ("mükemmele yakın") kümelenmesi olgusu; yani sistem teorik olarak 1-5 arası bir aralık
sunsa da fiilen kullanılan aralık çok daha dar ve yukarı çarpık. Doğrudan fetch edilen akademik
makaleye (arXiv 1701.01645, "Sharing Means Renting?: An Entire-marketplace Analysis of Airbnb")
göre "star-rating'lerde pozitif bir yanlılık var, bu da review metnindeki olumlu kelime
kullanımı yanlılığıyla güçleniyor" ve bu yanlılık, "zaten pozitif yanlılık gösterdiği bilinen
Yelp review'larından bile daha büyük". WebSearch özetlerinde (akademik makalenin tam metnine
erişilmeden) sıkça tekrarlanan bir iddiaya göre listelerin yaklaşık %86-90'ı 4.5 yıldız üzerinde
kümeleniyor.

**Nerede görülür:** İkisi de (puanların gösterildiği her yer: arama sonucu kartı, ilan sayfası,
review özeti).

**UX gerekçesi:** Bu enflasyonun kök nedeni, Airbnb'nin puanlamayı hem host performans
metriklerine (Superhost, Guest Favorite eşikleri 4.8 gibi yüksek bir eşiğe dayanıyor) hem de
arama sıralamasına doğrudan bağlaması: bir host için 4 yıldız, mutlak ölçekte "iyi" bir puan
olsa da platformun kendi eşiklerine göre pratikte bir "başarısızlık" gibi işliyor, bu da hem
host'ların misafirlerden yüksek puan almak için (bazen review sonrası doğrudan rica ederek)
baskı hissetmesine hem de misafirlerin "4 yıldız vermek hostu cezalandırır" düşüncesiyle gerçek
deneyimlerinden daha yüksek puan vermesine yol açıyor. Sonuç, review sisteminin ayırt edici
gücünün (bir 4.9 ile bir 4.7 arasındaki farkın anlamlı olup olmadığının) zayıflaması; puanlar
"filtre" olarak değil neredeyse "geçti/kaldı" (pass/fail) ikili bir sinyal olarak işliyor.
Airbnb'nin bu soruna 03 numaralı bölümde ele alınan altı kategorili puan kırılımı (temizlik,
doğruluk, check-in, iletişim, konum, değer) ve bu bölümde ele alınan Guest Favorite (sadece en
üst dilimdeki host/ilanları ayrıca öne çıkarma) gibi çözümlerle yaklaştığı görülüyor: genel puan
sıkışmışsa, alt kategori kırılımı ve ayrıca üst-dilim rozetleri (Guest Favorite, Superhost) ek
bir ayrıştırma katmanı sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** 5 yıldızlı (veya benzeri) bir puanlama sistemi
kuran her marketplace'te, eğer puanlama aynı zamanda satıcı/hizmet sağlayıcı için doğrudan bir
performans eşiği/ceza mekanizmasına bağlanıyorsa (ör. "4.7 altına düşerse hesabın kapanır"),
bu durumun puanları yukarı doğru sıkıştırma riski taşıdığı baştan öngörülmeli. Bu enflasyona
karşı mimari önlemler arasında: genel puanı alt kategori kırılımıyla desteklemek (böylece
"herkes 5 veriyor ama temizlik puanı düşük" gibi ayrıntı hâlâ görünür kalır), ve/veya üst dilimi
ayrıca öne çıkaran ikincil bir rozet sistemi (Airbnb'nin Guest Favorite'ı gibi) kurmak, tek bir
sıkışmış genel puana bağımlılığı azaltır.

**Kaynak / güven notu:** Kısmen doğrulandı. Pozitif yanlılığın var olduğu ve bunun Yelp'ten bile
büyük olduğu iddiası **doğrulandı**, doğrudan fetch edilen arXiv 1701.01645 makalesinin özetinden
(abstract) birebir alındı: "there is a bias toward positive ratings, amplified by a bias toward
using positive words in reviews" ve bu yanlılığın "greater than Yelp reviews" olduğu ifadeleri
sayfada geçiyor. Ancak makalenin tam metnindeki sayısal detaylara (ör. tam yüzde dağılımı)
erişilemedi, sadece özet okunabildi. "%86-90 civarı listelerin 4.5 üzerinde kümelendiği" rakamı
ise WebSearch özetinde farklı ikincil kaynaklardan (Hostaway blog, başka bir akademik çalışmanın
özeti) geldi, bu spesifik rakamlar doğrudan fetch edilerek teyit edilmedi →
**kısmen doğrulanmadı, rakamlar ikincil kaynaklı**. Airbnb'nin bu soruna "kategori kırılımı +
Guest Favorite" ile bilinçli olarak yaklaştığı yorumu ise Airbnb'nin resmi bir açıklaması değil,
bu araştırmacının kendi çıkarımı → açıkça **bu bir çıkarım, Airbnb'nin kendi ifadesi değil**.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip birincil/güçlü içerik olarak doğrulanan kaynaklar (Airbnb'nin kendi
  yardım merkezi sayfaları):** Guest Favorite kriterleri (help/article/3496), kimlik doğrulama
  süreci (help/article/1237), AirCover for Hosts kapsamı (help/article/3733, kısmi), platform
  dışı ödeme uyarısı (help/article/199), güvenlik kamerası/gürültü monitörü politikası
  (help/article/3061), host yanıt oranı/süresi (help/article/430), review'lara host yanıtı
  (help/article/32), Nondiscrimination Policy/Community Commitment (help/article/1405). Bu 8
  kaynak, 12 pattern'den 6'sını (madde 1[kısmen, 03'ten devralınan], 2, 3, 5, 7, 9) doğrudan ve
  güçlü şekilde destekliyor; 4'ünü (madde 4, 6, 10, 11) kısmen destekliyor.
- **Fetch edilip okunan ama akademik/ikincil kaynak:** arXiv 1701.01645 (review pozitif
  yanlılığı üzerine akademik makale, sadece özeti okunabildi, tam metin PDF'e erişilmedi).
- **Fetch denemesi yapılıp erişilemeyen kaynak:** news.airbnb.com/nondiscriminationupdate
  (403 Forbidden ile engellendi, sadece WebSearch özeti kullanılabildi).
- **Hiçbir birincil kaynakla doğrulanamayan, büyük ölçüde ikincil/eğitim verisinden yazılan
  madde:** profil eksiksizliği sinyalleri (madde 8) - sadece ikincil host rehberi sitelerinin
  arama özetlerine dayanıyor, hiçbir sayfa doğrudan fetch edilmedi.
- Toplamda 12 pattern'den **6 tanesi** doğrudan Airbnb birincil kaynağıyla güçlü doğrulandı,
  **5 tanesi** kısmen doğrulandı (gerçek kaynak fetch edildi ama ikincil kaynak, kısmi içerik
  veya alt-detay eksikliği var), **1 tanesi** (profil eksiksizliği) büyük ölçüde
  doğrulanmadı/eğitim verisinden. Canlı üründe sürekli politika ve A/B test değişikliği
  yaşandığından (ör. güvenlik kamerası politikasının 2024'te köklü değişimi), bu doküman
  "yerleşik/tekrar gözlemlenen pattern" çerçevesiyle okunmalı, "şu an birebir ekran görüntüsü"
  olarak değil.
