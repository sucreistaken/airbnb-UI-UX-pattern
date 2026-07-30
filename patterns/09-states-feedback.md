# 9. Boş/Hata/Yükleme Durumları ve Feedback (States & Feedback)

Bu bölüm, Airbnb'nin web ve mobil ürününde bir kullanıcının "beklediği içerik henüz orada değil"
ya da "bir şey ters gitti" hissiyle karşılaştığı tüm anları kapsıyor: skeleton/shimmer yükleme
ekranları ve arama sonuçlarının kademeli (staged) yüklenmesi, boş arama sonucu / boş wishlist /
boş gelen kutusu / boş seyahatler gibi "içerik yok" durumları, ağ hatası ve çevrimdışı durum
yönetimi, form doğrulama hataları (inline vs banner, tarih aralığı geçersizliği), toast/snackbar
onay bildirimleri, kalp-dolma gibi mikro-animasyonlar, 404/kaldırılmış ilan sayfası, rate-limit
mesajlaşması ve mobilde pull-to-refresh. Bu konu kümesi, önceki bölümlere (özellikle
02-listing-card-browse ve 06-messaging-notifications) kıyasla Airbnb'nin kendi resmi
kaynaklarında (yardım merkezi, mühendislik blogu) belirgin biçimde daha az doğrudan belgeleniyor:
Airbnb bir "boş sepet ekranımız şöyle tasarlanmıştır" diye bir tasarım yazısı yayınlamamış. Bu
yüzden bu bölümde iki farklı kaynak katmanı iç içe kullanıldı: (a) Airbnb'nin kendisiyle doğrudan
ilgili, fiilen fetch edilmiş kaynaklar (Airbnb Engineering'in HTTP streaming yazısı, Airbnb'nin
kendi açık kaynak `react-dates` deposundaki bir issue, Airbnb yardım merkezi wishlist sayfası),
ve (b) bu konunun genel endüstri prensiplerini tarif eden, doğrudan fetch edilmiş ama
Airbnb'ye özgü olmayan otorite kaynaklar (NN/g'nin skeleton screens ve error message
guidelines yazıları, Baymard'ın inline form validation ve zero-results page araştırmaları,
pull-to-refresh'in Wikipedia üzerinden doğrulanan tarihi). Bu iki katman her maddede ayrı ayrı
işaretlendi; bir Baymard/NN/g bulgusunun "Airbnb da muhtemelen buna benzer davranıyor" biçiminde
bir çıkarım olduğu, Airbnb'nin kendi ağzından doğrulanmış bir olgu olmadığı her seferinde açıkça
belirtiliyor.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: Airbnb Engineering'in Medium
yayınından "Improving Performance with HTTP Streaming"; Airbnb'nin kendi GitHub deposu
`airbnb/react-dates` üzerindeki #1978 numaralı issue; Airbnb yardım merkezinden wishlist sayfası
(help/article/1236); NN/g'nin "Skeleton Screens 101" ve "Error-Message Guidelines" yazıları;
Baymard Institute'ün "Usability Testing of Inline Form Validation" ve "5 Proven UX Strategies
For 'No Results' Pages" yazıları; GoodUI'nin Airbnb kalp ikonu A/B testi üzerine "leak" yazısı;
Codelevate'in Airbnb'nin 2025 "Lava" ikon formatı üzerine teknik rehberi; UserTesting'in empty
state'ler üzerine blog yazısı; Wikipedia'nın pull-to-refresh tarihçesi maddesi. Bunların dışında
kalan (Airbnb'nin gerçek boş gelen kutusu ekranı, gerçek ağ hatası kopyası, gerçek 404 sayfası
kopyası, gerçek toast metni, rate-limit mesajı) `airbnb.com/rooms/...` gibi canlı sayfalara
doğrudan erişim denemesi 403 Forbidden ile engellendiği için doğrulanamadı; bu maddeler ilgili
yerlerde açıkça "doğrulanmadı" olarak işaretlendi.

---

## 1. Skeleton loading ekranları ve shimmer (parıltı) efekti

**Ne olduğu:** Sayfa/kart içeriği henüz gelmemişken, boş bir alan ya da dönen bir spinner yerine,
gelecek içeriğin taslak/iskelet halini (gri dikdörtgen bloklar: fotoğraf alanı, başlık satırı,
fiyat satırı) gösterip bu bloklar üzerinde soldan sağa hareket eden parlak bir "shimmer" (parıltı
dalgası) animasyonu oynatmak. Topluluk gözlemine göre Airbnb, arama sonucu kartları başta olmak
üzere hemen her dinamik ekran biriminde bu tekniği kullanıyor; iskelet kartların boyutu ve
aralığı, gerçek kart yüklendiğinde "zıplama" hissi yaratmayacak şekilde gerçek karta birebir
yakın tutuluyor.

**Nerede görülür:** İkisi de; arama sonuçları grid/liste görünümü, ilan detay sayfası (galeri,
host bilgisi kartları) ve mesajlaşma gelen kutusu gibi liste tabanlı her ekranda.

**UX gerekçesi:** Doğrudan fetch edilen NN/g "Skeleton Screens 101" yazısına göre iskelet
ekranlar "sayfanın kademeli olarak nihai formuna dönüştüğü" illüzyonunu yaratıyor ve kullanıcının
"bombardımana tutulmadan önce sayfa yapısının zihinsel modelini kurmasına" yardımcı oluyor; yazı
ayrıca soldan sağa hareket eden dalga/shimmer benzeri hareketin, sabit nabız (pulse) gibi
alternatiflere kıyasla daha kısa süreymiş gibi algılandığını belirtiyor (DoorDash örneğiyle).
Aynı yazı bir sınır da koyuyor: iskelet ekranlar 2-10 saniye arası yüklenen sayfalarda anlamlı,
1 saniyeden kısa yüklenen sayfalarda gereksiz. Airbnb'nin arama sonucu sayfası gibi onlarca
kartın aynı anda, değişken hızda geldiği bir ekranda, her kartın kendi iskeletiyle doldurulması,
kullanıcının "kaç sonuç var, sayfa nasıl bir düzende" sorusuna spinner'dan çok daha erken cevap
veriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir liste/grid görünümünün yüklenme süresi 1-10
saniye aralığındaysa (tam ekran spinner'ın "bekliyorum" hissi yarattığı, ama anlık de olmayan bir
aralık), boş ekran veya merkezi spinner yerine, gerçek içerik kartının boyut/aralığını birebir
taklit eden iskelet kartlar + soldan sağa shimmer hareketi kullanmak, algılanan bekleme süresini
kısaltıyor. Kritik detay: iskelet kart boyutu gerçek karttan farklıysa, içerik geldiğinde oluşan
"sıçrama" bu faydanın çoğunu geri alıyor; bu yüzden iskelet ile gerçek bileşen arasındaki
piksel uyumu bilinçli bir tasarım kararı olmalı.

**Kaynak / güven notu:** Kısmen doğrulandı. NN/g'nin doğrudan fetch edilen "Skeleton Screens 101"
yazısı (https://www.nngroup.com/articles/skeleton-screens/), shimmer/soldan-sağa hareketin
algılanan performansa etkisi, 2-10 saniyelik kullanım aralığı ve "zihinsel model kurma"
gerekçesini genel bir prensip olarak doğruluyor, ancak bu yazı Airbnb'yi hiç örnek olarak
anmıyor (yazıda geçen örnekler LinkedIn, Headspace, DoorDash, NBC). Airbnb'nin arama sonucu
kartlarında fiilen skeleton+shimmer kullandığı iddiası, bu araştırmada WebSearch özetlerinde
görülen ikincil kaynaklardan (topluluk gözlemi, ekran görüntüsü paylaşan tasarım blogları) geliyor,
doğrudan fetch edilip Airbnb'nin kendi ağzından ya da ekran kaydıyla ayrıca doğrulanmadı
→ **genel prensip doğrulandı, Airbnb'ye özgü uygulama detayı doğrulanmadı/eğitim verisinden**.

---

## 2. Kademeli/staged yükleme: arama sonuçları sayfası için HTTP streaming

**Ne olduğu:** Doğrudan fetch edilen Airbnb Engineering yazısına göre Airbnb, günde 75 milyondan
fazla aramanın geldiği arama sonucu sayfasında, klasik "sunucu tüm yanıtı arka planda oluşturup
sonra tek seferde gönderir" (buffered) yaklaşımı yerine **HTTP streaming** kullanıyor: yanıt üç
parçaya bölünüyor. "Early chunk" (hızlı hesaplanan font/CSS/JS gibi varlıklar) en önce gönderiliyor,
"body chunk" (sayfa iskeleti) ardından, veri sorgularına bağımlı "deferred data chunk" (sunucudan
gelen gerçek JSON veri) ise hazır olduğu anda ayrı bir parça olarak akıtılıyor. Tarayıcı tarafında
bir `MutationObserver` bu geciktirilmiş veri parçasının geldiğini yakalayıp veriyi uygulamanın
kendi veri deposuna, sanki normal bir ağ isteği tamamlanmış gibi enjekte ediyor. Bunu mümkün kılmak
için Airbnb, NGINX'te yanıt tamponlamayı (response buffering) ve haproxy yük dengeleyicisinde
Nagle algoritmasını kapatmak gibi altyapı seviyesinde değişiklikler yapmış.

**Nerede görülür:** Öncelikle web; yazı özellikle arama sonucu sayfası ve ana sayfa dahil sunucu
taraflı render edilen (server-side rendered) sayfalardan bahsediyor.

**UX gerekçesi:** Doğrudan fetch edilen yazıya göre bu değişiklik, test edilen her sayfada
(ana sayfa dahil) First Contentful Paint (FCP) metriğinde sabit ~100ms'lik bir iyileşme
sağlamış. Sunucu tarafı render ile veri çekme işleminin, klasik yaklaşımda birbirini beklemesi
(sıralı, "network waterfall") yerine paralel çalışması, kullanıcının ekranda "bir şeyler görmeye
başlaması" ile "gerçek veri gelmesi" arasındaki gecikmeyi kısaltıyor; sayfa iskeleti (madde 1'deki
skeleton kartlarla dolu bir düzen) daha erken göründüğü için kullanıcı verinin gelişini "kademeli
doluş" olarak deneyimliyor, tek bir uzun beyaz ekran beklemesi yerine. Bu, madde 1'deki skeleton
screen tekniğinin arkasındaki altyapısal/mühendislik gerekçesi olarak okunabilir: skeleton'ın
"kademeli dolma" hissini verebilmesi için, sunucunun da veriyi gerçekten kademeli
gönderebiliyor olması gerekiyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Yüksek trafikli, veri-yoğun bir sayfada (arama
sonucu, feed, dashboard) klasik "tüm sayfayı arka planda oluştur, sonra gönder" modeli yerine,
sayfa iskeletini/statik varlıkları hemen, veri bağımlı kısımları ayrı bir akış (chunk) olarak
göndermek, kullanıcı tarafında algılanan performansı, gerçek toplam yükleme süresini
değiştirmeden iyileştirebiliyor. Bu yatırımın maliyeti düşük değil (altyapı seviyesinde
buffering/Nagle gibi ayarların değişmesi gerekebiliyor), bu yüzden özellikle yüksek trafikli,
tek bir sayfanın milyonlarca kez yüklendiği ürünlerde (arama, feed) anlamlı bir kazanç/maliyet
oranı sunuyor; düşük trafikli sayfalarda bu mühendislik yatırımı gerekmeyebilir.

**Kaynak / güven notu:** **Doğrulandı.** Üç aşamalı chunk stratejisi (early/body/deferred data),
`MutationObserver` ile deferred data'nın yakalanması, FCP'de ~100ms'lik sabit iyileşme, NGINX
buffering ve haproxy Nagle algoritmasının kapatılması, doğrudan fetch edilen
https://medium.com/airbnb-engineering/improving-performance-with-http-streaming-ba9e72c66408
(Airbnb Engineering'in resmi Medium yayını) yazısından birebir alındı. Günde 75 milyon arama
rakamı da aynı doğrudan fetch edilen kaynaktan geliyor. Yazı skeleton/shimmer görsel tekniğinden
(madde 1) açıkça bahsetmiyor, bu ikisinin birlikte çalıştığı iddiası (madde 1'in UX
gerekçesindeki son cümle) bu araştırmacının kendi çıkarımı, doğrudan Airbnb ifadesi değil.

---

## 3. Boş arama sonucu durumu: sıfır sonuç mesajı ve filtre kaldırma önerisi

**Ne olduğu:** Bir arama/filtre kombinasyonu hiçbir ilanla eşleşmediğinde, boş bir sayfa yerine
"bu arama için sonuç bulunamadı" gibi bir mesaj ve kullanıcının aramayı kurtarabileceği somut bir
sonraki adım gösterilmesi (örneğin aktif filtrelerden birini kaldırma önerisi, tarihleri esnetme,
yakın bölgeleri gösterme). WebSearch özetlerinde görülen (ama doğrudan fetch edilerek Airbnb'ye
özgü olarak doğrulanmayan) bir gözleme göre Airbnb, filtre panelinde bu sorunu önleyici tarafta da
ele alıyor: bir filtre/değer seçildiğinde kaç sonuç döneceğinin sinyalini girdi seviyesinde önceden
veriyor (ör. bir seçenek 0 sonuca düşürecekse bunu seçilmeden önce hissettirmeye çalışıyor).

**Nerede görülür:** İkisi de; en belirgin biçimde filtre panelinin uygulanmasının hemen ardından
gösterilen arama sonucu grid/liste görünümünde.

**UX gerekçesi:** Doğrudan fetch edilen Baymard "5 Proven UX Strategies For 'No Results' Pages"
araştırmasına göre e-ticaret sitelerinin **yaklaşık %50'si**, sıfır sonuç durumundan kurtarma için
etkili bir yol sunmuyor. Aynı araştırma, "yazımınızı kontrol edin" veya "daha genel terimler
deneyin" gibi genel tavsiyelerin kulağa yardımcı gelse de **nadiren işe yaradığını**, kullanıcıların
bu tür talimatları çoğunlukla atladığını ve somut bir yol göstermediğini belirtiyor; bunun yerine
somut kurtarma yolları (ilgili kategoriler, filtre gevşetilmiş ürün listesi, destek kanalı, popüler
ürünler) genel talimat metninden daha iyi performans gösteriyor. Bu çerçeve Airbnb'nin arama
sonucu bağlamına uyarlandığında: "sonuç yok" yazan pasif bir metin yerine, hangi filtrenin
kaldırılırsa sonuç döneceğini gösteren somut, tıklanabilir bir öneri (ör. "fiyat filtresini kaldır,
12 sonuç görün" gibi), kullanıcıyı aramayı baştan kurmaya değil, tek bir küçük düzeltmeye
yönlendiriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Sıfır sonuç ekranını tasarlarken genel/talimat
niteliğinde metin ("aramanızı kontrol edin") yazmak yerine, hangi spesifik filtrenin/kriterin
kaldırılmasının kaç sonuç döndüreceğini önceden hesaplayıp bunu tek tıkla uygulanabilir bir öneri
olarak sunmak (Baymard'ın "ilgili kategori/filtre gevşetilmiş liste" bulgusuyla uyumlu), kullanıcı
kaybını somut biçimde azaltıyor. Mümkünse bu sorunu daha da geriye, filtre seçilirken (seçim 0
sonuca düşürecekse bunu önceden sinyallemek) taşımak, "önce önle" ilkesinin "sonra kurtar"
ilkesinden daha güçlü olduğunu gösteriyor.

**Kaynak / güven notu:** Kısmen doğrulandı. **%50 istatistiği ve "genel tavsiye işe yaramıyor,
somut kurtarma yolu işe yarıyor" bulgusu doğrulandı**, doğrudan fetch edilen
https://baymard.com/blog/no-results-page yazısından birebir alındı; ancak bu araştırma
**e-ticaret siteleri geneli için**, Airbnb'ye özgü bir vaka çalışması değil, Airbnb bu yazıda
örnek olarak anılmıyor. Airbnb'nin filtre panelinde "seçim 0 sonuca düşürürse önceden sinyalleme"
davranışı ve gerçek "sonuç bulunamadı" ekranının tam kopyası/tasarımı bu araştırmada doğrudan
fetch edilip doğrulanmadı, sadece WebSearch özetlerinde görülen ikincil kaynaklardan geliyor
→ **bu kısım doğrulanmadı, eğitim verisi + ikincil kaynak özetinden**.

---

## 4. Boş wishlist (kaydedilenler) durumu

**Ne olduğu:** Hiç ilan kaydedilmemiş bir wishlist sekmesi açıldığında, boş bir liste yerine
bağlamsal bir görsel (WebSearch özetlerinde görülen bir gözleme göre bavul içinde bir kalp
illüstrasyonu) ve kullanıcıyı ilan kaydetmeye teşvik eden kısa bir açıklama metni gösterilmesi.
Doğrudan fetch edilen Airbnb yardım merkezi sayfasına göre wishlist mekaniğinin kendisi şöyle
işliyor: bir ilan kartındaki kalbe dokunulduğunda ilan mevcut bir wishlist'e eklenebiliyor ya da
yeni bir wishlist oluşturulabiliyor, aynı aramadan gelen birden fazla ilan otomatik olarak aynı
wishlist'te gruplanıyor, her wishlist'te en fazla 100 ilan olabiliyor ve orijinal arama tarihleri
kayıtla birlikte saklanıyor; wishlist'ler yeniden adlandırılabiliyor/silinebiliyor ve işbirlikçi
(collaborative) modda birden fazla kişi ilanlara oy verip not ekleyebiliyor.

**Nerede görülür:** İkisi de; wishlist sekmesi/profilin "Saved" bölümü web ve mobilde aynı işlevi
görüyor.

**UX gerekçesi:** Bir wishlist özelliğinin kendisi kullanıcıya "bir şey biriktir" davetidir; hiç
kullanılmamış bir wishlist'in boş, açıklamasız bir liste olarak gösterilmesi, özelliğin ne işe
yaradığını yeni kullanıcıya öğretmeyen bir fırsat kaybı. Bağlamsal bir illüstrasyon (bavul + kalp
gibi, wishlist'in "seyahat planlama" ve "kaydetme" işlevlerini aynı görselde birleştiren bir
metafor) ve kısa bir teşvik metni, boş durumu bir "hata" gibi değil bir "henüz başlamadın" daveti
gibi çerçeveliyor. Wishlist mekaniğinin kendisinin zengin olması (100 ilana kadar, işbirlikçi oy
verme, otomatik gruplama), boş durumun sadece "boş" değil, "bu özelliğin gerçekte ne kadar
zengin olduğunu henüz keşfetmedin" hissini de taşıyabilmesi için bir fırsat.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir "kaydet/favorile" özelliğinin boş durumunu
tasarlarken, jenerik bir "burada hiçbir şey yok" mesajı yerine, özelliğin asıl değerini
(ör. birden fazla kişiyle paylaşılabilir olması, otomatik gruplama gibi) ima eden bağlamsal bir
görsel + kısa açıklama kullanmak, boş durumu bir öğretim anına çeviriyor. Kaydetme özelliği
zenginse (paylaşım, işbirliği, sınır/limit gibi) bu zenginliğin bir kısmını boş durumun
metninde ipucu olarak vermek, kullanıcının özelliği daha erken keşfetmesini sağlıyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Wishlist mekaniğinin kendisi (kalp ile kaydetme,
100 ilan limiti, otomatik gruplama, yeniden adlandırma/silme, işbirlikçi oy verme/not ekleme,
paylaşılabilir link), doğrudan fetch edilen https://www.airbnb.com/help/article/1236 (Airbnb'nin
kendi yardım merkezi) sayfasından birebir doğrulandı, ancak **bu sayfa boş wishlist durumunun
kendisini (görsel, metin) hiç anmıyor**. Boş durumdaki "bavul içinde kalp" illüstrasyonu ve teşvik
metni iddiası, WebSearch özetlerinde görülen ikincil kaynaklardan (Pinterest/Muzli ekran görüntüsü
koleksiyonları) geliyor, bu sayfalar doğrudan fetch edilip birebir doğrulanmadı → **bu kısım
doğrulanmadı, eğitim verisi + ikincil kaynak özetinden**.

---

## 5. Boş gelen kutusu / "henüz mesaj yok" durumu

**Ne olduğu:** Hiç mesaj alışverişi olmamış bir kullanıcının Mesajlar (Inbox) sekmesini açtığında
karşılaştığı, mesajlaşmanın ne için kullanılacağını (host'a soru sorma, rezervasyon sonrası
iletişim gibi) kısaca anlatan bir boş durum ekranı.

**Nerede görülür:** İkisi de; mesajlaşma sekmesi web ve mobilde aynı işlevi görüyor
(06-messaging-notifications.md bölümünde mesajlaşma akışının kendisi, limitleri ve bildirim
davranışı ayrıntılı işlendi; bu madde özellikle "hiç mesaj yokken ekran ne gösteriyor" sorusuna
odaklanıyor).

**UX gerekçesi:** Bu araştırmada Airbnb'nin gerçek boş gelen kutusu ekranının tasarımına dair
doğrudan bir kaynak bulunamadı; WebSearch sonuçları bunun yerine büyük ölçüde "gelen kutum boş
görünüyor ama mesaj almışım" gibi **bir hata/bug şikayeti** temalı topluluk forumu gönderileriyle
doldu (ör. yanlış tarayıcı, önbellek sorunları, senkronizasyon gecikmesi). Bu, ilginç bir dolaylı
gözlem sunuyor: bir mesajlaşma ürününde "boş" durum ile "hatalı/senkronize olmamış" durumun
kullanıcı tarafından ayırt edilmesi zor olabiliyor, çünkü ikisi de aynı görsel sonucu (boş liste)
üretiyor; bu yüzden gerçekten boş bir gelen kutusu ile geçici bir yükleme/senkronizasyon
gecikmesini görsel olarak (ör. bir yükleniyor göstergesiyle) ayırt etmek önemli bir tasarım
gereksinimi olabilir, ama bu Airbnb'nin kendi ifadesi değil, bu araştırmacının forum
gözleminden çıkardığı bir sonuç.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir mesajlaşma/gelen kutusu özelliğinde "gerçekten
boş" ile "henüz yüklenmedi/senkronize olmadı" durumlarını görsel olarak kesin biçimde ayırmak
(ör. boş durumdan önce kısa bir yükleme göstergesi göstermek, senkronizasyon başarısız olduğunda
ayrı bir hata durumu göstermek), kullanıcıların "mesajım kayboldu mu" diye endişelenmesini
önlüyor. Airbnb'nin forumlarındaki tekrarlanan "boş görünüyor ama mesajım var" şikayetleri, bu
ayrımın net yapılmadığında ne kadar güven kaybına yol açabileceğinin dolaylı bir kanıtı.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + ikincil kaynak özetinden.** Bu maddenin
hem "boş gelen kutusu nasıl görünür" iddiası hem de "boş ile senkronize-olmamış karışıyor"
çıkarımı, Airbnb'nin resmi bir kaynağıyla değil, community.withairbnb.com üzerindeki resmi
olmayan kullanıcı forumu gönderilerinin WebSearch özetleriyle destekleniyor; bu sayfalar doğrudan
fetch edilip birebir okunmadı. Bu, bölümdeki en zayıf kaynaklı maddelerden biri.

---

## 6. Boş seyahatler (Trips) durumu: yaklaşan seyahat yok, arama teşviki

**Ne olduğu:** Hiç aktif rezervasyonu olmayan bir kullanıcının "Trips" (Seyahatlerim) sekmesini
açtığında, boş bir liste yerine bu bölümün ne işe yaradığını kısaca anlatan bir metin ve
kullanıcıyı arama/keşfe yönlendiren, göz alıcı, belirgin bir CTA butonu gösterilmesi.

**Nerede görülür:** İkisi de; Trips sekmesi hem web hem mobil alt navigasyonda benzer bir yapıda.

**UX gerekçesi:** Doğrudan fetch edilen UserTesting blog yazısına göre Airbnb'nin bu boş durumu
"çok az miktarda metinle" üç şeyi aynı anda başarıyor: bu bölümün ne için olduğunu (planlanmış
seyahatleri görüntülemek), neden boş olduğunu (henüz bir seyahat rezerve edilmemiş) açıklamak ve
kullanıcıyı "yeni bir yer keşfetmek için aramaya" teşvik eden büyük, renkli, belirgin bir buton
sunmak. Bu üçlü yapı (ne/neden/sonraki adım), boş bir ekranı sadece bir "henüz veri yok" bilgisi
olarak değil, yeni kullanıcıyı ürünün asıl işlevine (arama, rezervasyon) geri yönlendiren aktif bir
onboarding anı olarak kullanıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir "geçmiş/aktif işlemler" listesi (sipariş
geçmişi, randevu listesi, rezervasyon listesi gibi) boşken, sadece "henüz kaydınız yok" yazıp
bırakmak yerine, üç unsuru (bu bölüm ne işe yarar, neden şu an boş, hangi eylemle doldurulur)
tek bir kısa metin + belirgin CTA içinde birleştirmek, boş durumu ürünün ana değer döngüsüne
(arama, keşif, ilk işlem) geri bağlıyor. Bu özellikle yeni kullanıcılar için, ürünün bu bölümünün
ne işe yaradığını henüz bilmedikleri bir öğretim fırsatı.

**Kaynak / güven notu:** Kısmen doğrulandı. Doğrudan fetch edilen
https://www.usertesting.com/blog/empty-states yazısı, Airbnb'nin Trips boş durumunu somut olarak
örnek gösteriyor ve "az metin + ne/neden/CTA" yapısını doğruluyor, ancak yazı **tam buton
metnini veya birebir kopyayı vermiyor**, sadece genel yaklaşımı anlatıyor; bu yazının kendisi de
Airbnb'nin resmi bir yayını değil, üçüncü taraf bir UX blogunun vaka gözlemi. Ekranın tam güncel
görünümü bu araştırmada ayrıca ekran kaydıyla doğrulanmadı → **kısmen doğrulandı, ikincil kaynak
gözleminden, tam kopya doğrulanmadı**.

---

## 7. Ağ hatası / çevrimdışı durum yönetimi

**Ne olduğu:** Kullanıcının bağlantısı kesildiğinde veya bir API isteği başarısız olduğunda
uygulamanın verdiği tepki: bir hata mesajı, "tekrar dene" butonu, önbelleğe alınmış (cached)
içeriğin gösterilip gösterilmediği gibi davranışlar.

**Nerede görülür:** İkisi de; ancak bu araştırmada bulunan kanıtların neredeyse tamamı mobil
uygulama forumu şikayetlerinden geliyor.

**UX gerekçesi:** Bu araştırmada Airbnb'nin resmi olarak belgelediği bir çevrimdışı/ağ hatası
tasarım felsefesi bulunamadı. WebSearch sonuçları, kullanıcıların "Inbox network request failed,
please try again" gibi bir hata metniyle karşılaştığını ve bunun çözümü için genelde uygulamayı
yeniden başlatma, hücresel veri/Wi-Fi arası geçiş yapma, önbelleği temizleme, uygulamayı yeniden
kurma gibi **kullanıcı tarafı geçici çözümlerin** topluluk forumlarında paylaşıldığını gösteriyor;
bu, hata mesajının kendisinin (NN/g'nin madde 8'de aktarılan ilkelerine göre olması gerektiği gibi)
kullanıcıya "ne yapmalıyım" konusunda yeterince somut bir yönlendirme sağlamadığının dolaylı bir
göstergesi olabilir, çünkü kullanıcılar çözümü uygulamanın kendisinden değil birbirlerinden
öğreniyor gibi görünüyor. Bu, tamamen bir çıkarım; Airbnb'nin kendi hata mesajı metninin tam
kopyası ve bunun arkasındaki tasarım niyeti bu araştırmada doğrulanamadı.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir ağ hatası mesajı yalnızca "istek başarısız
oldu, tekrar deneyin" demekle kalmayıp (NN/g'nin madde 8'deki "yapıcı çözüm öner" ilkesiyle
tutarlı biçimde), olası nedenin ne olabileceğine dair somut bir ipucu (ör. "bağlantınızı kontrol
edin" veya otomatik olarak bir yeniden deneme sayacı göstermek) sunmalı; kullanıcıların bir hatayı
çözmek için resmi kaynak yerine topluluk forumuna gitmek zorunda kalması, hata mesajının kendi
başına yetersiz olduğunun bir işareti olabilir.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + ikincil kaynak özetinden.** Bu madde
tamamen community.withairbnb.com forum gönderilerinin WebSearch özetlerine dayanıyor; bu sayfalar
doğrudan fetch edilip birebir okunmadı, Airbnb'nin resmi bir çevrimdışı/hata yönetimi
dokümantasyonu bu araştırmada bulunamadı. Bölümdeki en zayıf kaynaklı maddelerden biri.

---

## 8. Form doğrulama hataları: inline (alan-altı) vs banner/toast yaklaşımı

**Ne olduğu:** Bir form alanına (ör. ödeme bilgisi, e-posta, telefon numarası) geçersiz bir değer
girildiğinde hatanın nasıl gösterileceği: hatayı doğrudan ilgili alanın altında (inline), tüm
formun üstünde toplu bir banner'da, ya da geçici bir toast/snackbar ile mi bildirmek gerektiği
sorusu.

**Nerede görülür:** İkisi de; bu madde Airbnb'ye özgü bir vaka çalışması değil, form doğrulama
üzerine genel, doğrudan fetch edilmiş bir endüstri araştırmasına dayanıyor ve Airbnb'nin checkout
akışındaki (04-booking-checkout.md'de ayrıntılı işlenen) form alanlarına genel bir çerçeve olarak
uyarlanıyor.

**UX gerekçesi:** Doğrudan fetch edilen Baymard "Usability Testing of Inline Form Validation"
araştırmasına göre e-ticaret sitelerinin **%31'i hiç inline doğrulama sunmuyor, %4'ü ise bunu
yanlış uyguluyor**. Araştırma, doğrulamanın **ne zaman** tetiklendiğinin kritik olduğunu
gösteriyor: kullanıcı bir alanı doldururken erken/anlık hata göstermek (ör. her tuş vuruşunda)
rahatsız edici (bir katılımcının aktarılan sözü: "Neden e-posta adresimin yanlış olduğunu
söylüyorsun, henüz doldurmayı bitirmedim ki!"); bunun yerine alan **terk edildiğinde** (blur)
kontrol yapılması, ama hata **düzeltilirken** anlık/tuş-vuruşu seviyesinde kaybolması öneriliyor.
Araştırma ayrıca pozitif doğrulamanın (bir alan doğru doldurulduğunda yeşil bir onay ikonu
gösterme) kullanıcıda "ilerleme/başarı hissi" yarattığını belirtiyor. Bu ilke bir toast/banner
yaklaşımıyla karşılaştırıldığında: hatayı formun en üstünde toplu bir listede göstermek,
kullanıcının hangi alanın hatalı olduğunu bulmak için sayfayı taraması gereken bir ek adım
yaratıyor; alanın hemen altında gösterilen inline hata bu aramayı ortadan kaldırıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Uzun formlarda (ödeme, kayıt, rezervasyon)
hata mesajlarını toplu bir banner yerine ilgili alanın hemen altında göstermek, kullanıcının
hatayı bulma süresini kısaltıyor; ancak bunu **ne zaman** tetikleyeceğinize dikkat etmek gerekiyor:
alan doldurulurken erken uyarı vermek yerine alan terk edildiğinde kontrol edip, düzeltme
yapıldığında hatayı anında (tuş vuruşu seviyesinde) kaldırmak, hem gereksiz rahatsızlığı önlüyor
hem de düzeltmenin işe yaradığını anında teyit ediyor. Toast/snackbar bu bağlamda hata mesajı
için değil (çünkü toast geçicidir ve kullanıcı formu doldururken kaybolabilir, madde 10'da
anlatılan onay bildirimleri için daha uygun), doğrulama hataları için ise kalıcı, alanla ilişkili
inline mesaj daha uygun.

**Kaynak / güven notu:** Kısmen doğrulandı. **%31/%4 istatistiği, doğrulama zamanlaması
(blur'da kontrol, tuş vuruşunda anlık düzeltme kaldırma) ve pozitif doğrulamanın etkisi
doğrulandı**, doğrudan fetch edilen https://baymard.com/blog/inline-form-validation yazısından
birebir alındı; ayrıca NN/g'nin doğrudan fetch edilen
https://www.nngroup.com/articles/error-message-guidelines/ yazısı da "hatayı kaynağa yakın göster"
ve "önem derecesine göre banner/toast/modal seç" ilkelerini destekliyor. Ancak **bu iki kaynak da
Airbnb'ye özgü değil, genel e-ticaret/UX araştırması**; Airbnb'nin kendi checkout formunun
gerçekte inline mi banner mı kullandığı bu araştırmada doğrudan fetch edilip doğrulanmadı
→ **genel prensip güçlü doğrulandı, Airbnb'nin kendi uygulaması doğrulanmadı**.

---

## 9. Tarih seçici (date picker) geçersiz aralık mesajlaşması

**Ne olduğu:** Kullanıcı bir check-in/check-out tarih aralığı seçerken, seçilen giriş tarihi
çıkış tarihinden sonra geliyorsa (ya da tersi) bu geçersiz durumun takvimde nasıl
yansıtılacağı/düzeltileceği sorunu.

**Nerede görülür:** İkisi de; en belirgin biçimde arama çubuğundaki tarih seçici ve rezervasyon
akışındaki tarih değiştirme ekranında (01-discovery-search.md ve 04-booking-checkout.md'de tarih
seçicinin genel davranışı ayrıca işlendi; bu madde özellikle geçersiz aralık durumuna odaklanıyor).

**UX gerekçesi:** Airbnb'nin kendi açık kaynak `react-dates` kütüphanesindeki (bu, Airbnb
Engineering'in halka açık bir yazılım projesi, doğrudan bir tasarım kararı belgesi değil, ama
kütüphanenin davranışı ürünün gerçek karar problemini yansıtıyor) bir issue'ya göre, kütüphane
tutarsız bir davranış sergiliyor: kullanıcı **çıkış tarihini** düzenlerken giriş tarihinden önceki
bir tarih seçerse, takvim görsel olarak güncellenip yeni bir aralık gösteriyor (giriş tarihi
otomatik olarak yeniden konumlandırılıyor); ama kullanıcı **giriş tarihini** düzenlerken çıkış
tarihinden sonraki bir tarih seçerse, giriş kutusu güncelleniyor ama takvim bunun geçersiz bir
aralık olduğunu göstermiyor ve buna göre güncellenmiyor. Bu asimetri, tarih aralığı gibi
"iki uçlu, birbirine bağımlı" bir girdi türünde, hangi ucun düzenlendiğine bağlı olarak farklı
geri bildirim davranışının kolayca gözden kaçabileceğini, ve bu tutarsızlığın kullanıcıya
(sonunda geçersiz bir aralığın sunucuya gönderilmesi ya da sessizce "tek günlük" bir aralığa
indirgenmesi gibi) belirsiz bir sonuç bırakabileceğini gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** İki uçlu (başlangıç/bitiş) bir tarih aralığı
seçicisi tasarlarken, her iki ucun da düzenlenmesi durumunda **simetrik** bir geçersiz-aralık
davranışı tanımlamak kritik: hangi tarih değiştirilirse değiştirilsin (giriş ya da çıkış), diğer
ucun bu yeni seçime göre nasıl tepki vereceği (otomatik düzeltme mi, engelleme mi, uyarı mı)
önceden, tek bir kural olarak belirlenmeli. Sadece bir yönü (ör. sadece çıkış tarihi
değiştirildiğinde) ele alıp diğerini (giriş tarihi değiştirildiğinde) belirsiz bırakmak, kullanıcı
arayüzünün "bazen çalışan bazen çalışmayan" bir izlenim vermesine yol açıyor.

**Kaynak / güven notu:** **Doğrulandı** (bir mühendislik/davranış problemi olarak): bu issue'nun
içeriği (asimetrik davranış, hangi durumda takvimin güncellenip hangi durumda güncellenmediği,
kullanıcının önerdiği düzeltme), doğrudan fetch edilen
https://github.com/airbnb/react-dates/issues/1978 (Airbnb'nin kendi açık kaynak GitHub deposu)
sayfasından birebir alındı. Ancak bu, **Airbnb'nin canlı ürününde bugün fiilen bu davranışın
sergilendiği anlamına gelmiyor**: `react-dates` eski bir açık kaynak kütüphane, Airbnb'nin
kendi ürünündeki güncel tarih seçici bu kütüphaneden farklı/güncellenmiş olabilir, issue'nun
kendisi de bir "bildirilen sorun" (kesin olarak çözüldüğü teyit edilmedi). Dolayısıyla "bu tür bir
asimetri riski var, tasarımda buna dikkat edilmeli" dersi doğrulanmış, ama "Airbnb'nin bugünkü
canlı ürünü tam olarak böyle davranıyor" iddiası doğrulanmadı.

---

## 10. Toast/snackbar ile onay bildirimleri (ör. "wishlist'e eklendi")

**Ne olduğu:** Kullanıcı bir eylem tamamladığında (bir ilanı kaydetme, bir mesajı gönderme gibi)
ekranın kalıcı bir parçası olmayan, birkaç saniye görünüp kendiliğinden kaybolan kısa bir bildirim
(toast/snackbar) gösterilmesi; genelde ekranın alt kısmında, "X wishlist'ine eklendi" gibi kısa bir
onay metniyle.

**Nerede görülür:** İkisi de; en tipik tetikleyici an, bir ilan kartındaki kalp ikonuna
dokunulduğu, ilanın bir wishlist'e eklendiği an.

**UX gerekçesi:** Bir "kaydetme" gibi düşük riskli, geri alınabilir eylemde kullanıcıya kalıcı bir
onay ekranı (ör. bir modal) göstermek gereksiz bir sürtünme yaratır; kullanıcı zaten aynı ekranda
kalıp taramaya devam etmek istiyor. Geçici bir toast, "eylemin gerçekleştiğini" teyit edip
kullanıcının akışını kesmeden kendiliğinden kayboluyor; bu, NN/g'nin madde 8'de aktarılan
"önem derecesine göre mesaj türü seç" ilkesiyle tutarlı bir örnek: küçük/geri alınabilir bir
onay için toast, büyük/geri alınamaz bir hata için modal.

**Airbnb dışı bir uygulamaya uyarlama notu:** Düşük riskli, sık tekrarlanan bir onay eylemi
(kaydetme, favorileme, "okundu" işaretleme gibi) için kalıcı bir onay ekranı yerine kısa süreli,
kendiliğinden kaybolan bir toast/snackbar kullanmak, kullanıcının akışını kesmeden eylemin
gerçekleştiğini teyit ediyor. Toast'ın süresi ve konumu (genelde ekranın alt kısmı, birincil
navigasyonu engellemeyecek biçimde) tutarlı tutulmalı ki kullanıcı zamanla "bu kısa mesajları
görmezden gelebilirim, önemli değiller" güvenini geliştirebilsin.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + ikincil kaynak özetinden.** Bu araştırmada
Airbnb'nin "wishlist'e eklendi" toast'unun gerçek metnini veya varlığını doğrudan doğrulayan
resmi bir kaynak bulunamadı; doğrudan fetch edilen wishlist yardım merkezi sayfası
(help/article/1236, madde 4) böyle bir bildirimden hiç bahsetmiyor. WebSearch sonuçlarında
görülen Figma topluluk forumu gönderileri, tasarımcıların Airbnb'nin wishlist akışını Figma'da
prototiplerken "eklendi" toast'u ve kalp dolma animasyonunu aynı anda tetiklemeye çalıştığını
gösteriyor, ama bunlar **Airbnb'nin gerçek ürününün açıklaması değil, üçüncü taraf tasarımcıların
Airbnb'yi taklit etme girişimleri**; bu araştırmada Airbnb'nin canlı ürünündeki gerçek toast
davranışı ekran kaydıyla ayrıca doğrulanmadı.

---

## 11. Başarı/onay mikro-animasyonu: wishlist kaydında kalp dolma efekti

**Ne olduğu:** Bir ilan kartındaki kalp ikonuna dokunulduğunda, ikonun sadece anlık olarak
dolu/boş durumu değiştirmesi değil, kısa bir animasyonla (büyüyüp küçülme, "nabız atma" gibi)
bu değişimi vurgulaması.

**Nerede görülür:** İkisi de; en belirgin örnek ilan kartı/detay sayfasındaki kalp ikonu, ama
08-visual-design-system.md'de işlenen 2025 "Lava" ikon formatının bir parçası olarak da
tanımlanıyor.

**UX gerekçesi:** Doğrudan fetch edilen bir teknik rehbere göre Airbnb'nin 2025'te tanıttığı
"Lava" ikon/animasyon formatı kütüphanesinde, doğrudan "Favorite Heart Pulse" (Favori Kalp
Nabzı) adı verilen, örnek olarak listelenen bir animasyon var: "iki kez genişleyip küçülen,
'kaydedildi' içeriğine dikkat çeken bir kalp". Bu, madde 8 (08-visual-design-system.md)'de
işlenen Saarinen'in DLS "Conversational" ilkesiyle (motion'ın ürünleri net kullanıcı
iletişimiyle hayata geçirmesi) doğrudan örtüşüyor: bir durum değişikliğini (kaydedilmemiş ->
kaydedilmiş) sadece renk/dolgu değişimiyle değil, kısa bir hareketle de işaretlemek, kullanıcının
bu değişimi gözden kaçırma ihtimalini azaltıyor ve eyleme küçük bir "keyif" (micro-delight)
anı ekliyor. Ayrıca doğrudan fetch edilen GoodUI'nin Airbnb kalp ikonu yerleşimi A/B testi
üzerine yazısı, kalp ikonunun fotoğraf üzerinden kart kenarına taşınmasının (muhtemelen) hem
Fitts Yasası'na göre dokunma kolaylığını hem de yanlışlıkla ilan detayına tıklama riskinin
azalmasını hedeflediğini öne sürüyor; bu, kalp etkileşiminin (ve onunla birlikte gelen
animasyonun) Airbnb için tek seferlik bir detay değil, tekrar tekrar test edilip iyileştirilen
bir etkileşim noktası olduğunu gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir "kaydet/favorile" gibi ikili (açık/kapalı)
durum değişikliği taşıyan bir ikon için, sadece renk/dolgu değişimine güvenmek yerine kısa bir
(genelde 200-600ms aralığında, "iki kez genişleyip küçülme" gibi) vurgu animasyonu eklemek,
durumun değiştiğini kullanıcıya daha güvenilir biçimde iletiyor; özellikle hızlı kaydırma/tarama
davranışında (kullanıcı ekrana tam odaklanmıyor olabilir) bu tür bir hareket ipucu, sadece statik
bir renk değişiminden daha fazla dikkat çekiyor. Bu ikonun yerleşimini (fotoğrafın üzerinde mi,
kartın kenarında mı) test etmek de ayrı bir optimizasyon fırsatı: dokunma kolaylığı ile yanlışlıkla
tetiklenen eylemler arasındaki denge, tek seferlik bir tasarım kararı değil, tekrarlı test
gerektiren bir konu.

**Kaynak / güven notu:** Kısmen doğrulandı. "Favorite Heart Pulse" adının ve "iki kez genişleyip
küçülen kalp" tanımının doğrudan fetch edilen
https://www.codelevate.com/blog/how-to-create-airbnb-lava-style-icons---step-by-step-guide-2025
yazısından geldiği doğrulandı, ancak **bu, Airbnb'nin resmi bir yayını değil, bağımsız bir
üçüncü taraf teknik/eğitim rehberi**; yazı bu animasyonu "Lava formatının neler yapabileceğine
dair kavramsal bir örnek" olarak sunuyor, bu spesifik animasyonun Airbnb'nin canlı ürününde
birebir bu adla/bu şekilde kullanıldığını doğrulayan resmi bir Airbnb kaynağı bulunamadı.
GoodUI'nin kalp ikonu yerleşimi A/B testi yazısı da (https://goodui.org/leaks/airbnb-a-b-tests-and-detects-a-better-placement-for-saving-properties/)
doğrudan fetch edildi, ancak yazının kendisi "gerçek test verisine erişimimiz yok, bunlar kendi
ileri sürdüğümüz hipotezler" diyerek **kendi bulgusunun ters-mühendislik/varsayım olduğunu**
açıkça belirtiyor → bu madde bütünüyle **kısmen doğrulanmadı, ikincil/bağımsız teknik kaynaklara
dayanıyor, Airbnb'nin resmi bir açıklaması yok**.

---

## 12. 404 / kaldırılmış (artık mevcut olmayan) ilan sayfası

**Ne olduğu:** Bir kullanıcı artık listelenmeyen (host tarafından kaldırılmış, "snooze" edilmiş,
Airbnb tarafından yayından kaldırılmış) bir ilanın linkine tıkladığında karşılaştığı sayfa/durum.

**Nerede görülür:** İkisi de; en tipik senaryo bir ilanın eski bir arama/paylaşım linki, sosyal
medya paylaşımı ya da arama motoru sonucu üzerinden ziyaret edilmesi.

**UX gerekçesi:** WebSearch sonuçlarında görülen çok sayıda topluluk forumu gönderisine göre
"This listing is no longer available" (Bu ilan artık mevcut değil) mesajı ya da ana sayfaya
otomatik yönlendirme, ilanın "snooze" edilmiş, yayından kaldırılmış (unlisted), yeni
oluşturulup henüz açılmamış, ya da Airbnb'nin (çok sık misafir talebini reddeden host'lar gibi
gerekçelerle) gizlediği durumlarda ortaya çıkıyor. Bir ilanın kaybolma nedeninin host'un kendi
eylemi (kasıtlı kaldırma) ile platformun idari bir kararı (politika ihlali, düşük yanıt oranı gibi)
arasında değişebilmesi, "artık mevcut değil" gibi tek, nötr bir mesajın hem ziyaretçi hem host
için farklı anlamlara gelebileceğini gösteriyor; ziyaretçi için bu basitçe "arama sonucunu
kaybettim" anlamına gelirken, host için bu potansiyel bir gelir kaybı sinyali.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir içerik sayfası (ilan, ürün, profil) kaldırıldığında,
"bulunamadı" gibi tek bir jenerik mesaj yerine, mümkünse kullanıcıyı **benzer/ilgili içeriğe**
yönlendiren bir kurtarma yolu sunmak (bu, madde 3'teki zero-results page ilkeleriyle doğrudan
aynı mantık: jenerik "yok" mesajı değil, somut bir sonraki adım). Airbnb bağlamında bu, aynı
bölgedeki benzer ilanları göstermek ya da kullanıcıyı arama sayfasına, önceki arama kriterleriyle
geri yönlendirmek şeklinde olabilir.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + ikincil kaynak özetinden.** Bu maddenin
tamamı community.withairbnb.com ve airhostsforum.com üzerindeki resmi olmayan kullanıcı/host
forumu gönderilerinin WebSearch özetlerine dayanıyor. Gerçek 404/kaldırılmış ilan sayfasının
birebir kopyasını görmek için `airbnb.com/rooms/999999999999` gibi geçersiz bir URL'e doğrudan
erişim denendi, ancak **403 Forbidden hatası alındı** (Airbnb'nin bot/otomatik erişim engelleme
sistemi); bu yüzden sayfanın gerçek metni, tasarımı ve varsa sunduğu kurtarma yolları bu
araştırmada doğrulanamadı.

---

## 13. Rate-limit / "çok fazla istek" mesajlaşması

**Ne olduğu:** Bir kullanıcının kısa sürede çok fazla istek göndermesi (ör. hızlı ardışık arama,
mesaj gönderme, giriş denemesi) durumunda platformun bunu geçici olarak sınırlaması ve
kullanıcıya bunu nasıl ilettiği.

**Nerede görülür:** Bilinmiyor; bu araştırmada Airbnb'ye özgü bir rate-limit mesajı/davranışı
bulunamadı.

**UX gerekçesi:** Bu maddede dürüstçe belirtilmesi gereken şey şu: geniş bir arama (Airbnb'nin
kendi yardım merkezi, mühendislik blogu ve WebSearch genelinde) yapılmasına rağmen, Airbnb'nin
kullanıcıya dönük, belgelenmiş bir "çok fazla istek" veya "rate limit" mesajı/deneyimi bu
araştırmada bulunamadı. Bulunan sonuçlar tamamen genel HTTP 429 durum kodu açıklamaları
(MDN, Postman, HubSpot gibi genel teknik kaynaklar) ile sınırlı kaldı; bunlar Airbnb'ye özgü
hiçbir bilgi içermiyor. Bu, ya Airbnb'nin böyle bir durumu kullanıcıya hiç göstermediği (arka
planda sessizce kuyruğa alma/geciktirme gibi bir strateji izlediği), ya da bu deneyimin
yeterince nadir/az belgelendiği için kamuya açık kaynaklarda iz bırakmadığı anlamına gelebilir;
ama bu tamamen bir spekülasyon.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir rate-limit durumu kullanıcıya gösterilecekse,
genel HTTP 429 mantığından (Retry-After header, üstel geri çekilme/exponential backoff) ilham
alarak, kullanıcıya "ne zaman tekrar deneyebileceğine dair somut bir süre" vermek (ör. "30 saniye
sonra tekrar deneyin" gibi), NN/g'nin madde 8'deki "yapıcı çözüm öner" ilkesiyle tutarlı bir
uygulama olur. Ancak devredilebilir asıl ders şu: yüksek trafikli bir tüketici ürününde
rate-limit'i kullanıcıya hiç göstermeden (arka planda kuyruklama, debounce, önbellekleme gibi
tekniklerle) tamamen görünmez kılmak, en iyi kullanıcı deneyimi olabilir; bu maddenin Airbnb için
kamuya açık kaynaklarda hiç iz bırakmaması, bunun bilinçli bir "kullanıcıya hiç göstermeme"
stratejisinin bir işareti olabileceğine dair zayıf bir dolaylı ipucu.

**Kaynak / güven notu:** **Doğrulanmadı, bulgu yokluğu olarak işaretlendi.** Bu madde için hem
Airbnb yardım merkezi hem mühendislik blogu hem de genel WebSearch'te doğrudan bir kaynak
bulunamadı; bulunan tek sonuçlar Airbnb'ye özgü olmayan genel HTTP 429 teknik açıklamalarıydı
(MDN, Postman, SiteGround gibi). Bu maddenin "Airbnb'nin rate-limit'i sessizce ele aldığı" yönündeki
yorumu tamamen bu araştırmacının spekülasyonu, hiçbir kaynakla desteklenmiyor
→ **doğrulanmadı, eğitim verisinden bile değil, açık bir bulgu boşluğu**.

---

## 14. Pull-to-refresh: mobil listede aşağı çekerek yenileme

**Ne olduğu:** Mobil bir liste görünümünde (arama sonuçları, gelen kutusu, trips listesi gibi)
kullanıcının parmağıyla listeyi aşağı doğru çekip bırakması, bir yenileme göstergesinin
(genelde dönen bir spinner) belirmesi ve liste içeriğinin en güncel veriyle yeniden yüklenmesi.

**Nerede görülür:** Sadece mobil; web'de bu gesture'ın doğal bir karşılığı yok (web'de genelde
manuel bir "yenile" butonu ya da otomatik polling kullanılıyor).

**UX gerekçesi:** Doğrudan fetch edilen Wikipedia maddesine göre bu pattern, 2008'de Loren
Brichter'ın Twitter istemcisi Tweetie için icat ettiği bir gesture'a dayanıyor; Brichter'ın
motivasyonu ekranın en değerli bölgesini (üst köşe, navigasyon alanı) statik bir "yenile"
butonuna feda etmemekti ("Hepsi bir yer bulup bir yenileme butonunu bir yere sıkıştırmak zorunda
kaldı... o, navigasyon için en değerli gayrimenkuldü" şeklinde aktarılan bir açıklamasına göre).
Brichter, resmi kullanılabilirlik testi yapmadan, tek bir öğleden sonra iki iterasyon deneyerek
eşiği (ne kadar çekmenin "yeterince zor ama yeterince kolay" hissettiğini) elle ayarladı. Pattern
o kadar yaygınlaştı ki (2015'te Chrome'a opsiyonel, 2019'da Chrome 75 ile zorunlu, 2020-2021'de
Firefox'a eklendi) Wikipedia'nın belirttiği gibi "kullanıcılar bunun mobil uygulama deneyiminin
bir parçası olmasını örtük olarak bekliyor" hale geldi; bu evrensel beklenti, bir liste görünümünde
bu gesture'ın **olmaması**nın bile kendi başına bir eksiklik gibi hissedilmesine yol açabiliyor.
Airbnb'nin arama sonuçları, gelen kutusu ve trips gibi liste tabanlı mobil ekranlarında bu
gesture'ın (topluluk gözlemine göre) standart biçimde bulunması, bu evrensel beklentiyi
karşılamanın bir sonucu olarak okunabilir.

**Airbnb dışı bir uygulamaya uyarlama notu:** Mobil bir liste görünümü sunan her uygulama için,
pull-to-refresh artık bir "özellik" değil, kullanıcının varsayılan olarak beklediği bir temel
etkileşim; bunu uygulamamak, kullanıcıya "bu liste güncel mi, nasıl yenileyeceğim" belirsizliği
bırakıyor. Brichter'ın orijinal tasarım sürecinden (resmi test olmadan, elle "doğru hissettiren"
eşiği bulma) çıkarılabilecek ders: bu gesture'ın eşiği (ne kadar çekmek gerektiği, ne zaman
yenilemenin tetiklendiği) çok ince bir dokunsal ayar meselesi, bu yüzden hazır platform
bileşenlerini (iOS/Android'in kendi native pull-to-refresh implementasyonları) kullanmak, bu
inceliği sıfırdan yeniden icat etmekten daha güvenilir bir sonuç veriyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Pull-to-refresh'in **Loren Brichter ve Tweetie
kökeni, motivasyonu, geliştirme süreci ve platformlar arası yaygınlaşma zaman çizelgesi
(2015 Chrome, 2019 Chrome 75, 2020-2021 Firefox) doğrulandı**, doğrudan fetch edilen
https://en.wikipedia.org/wiki/Pull-to-refresh sayfasından birebir alındı; bu, pattern'in genel
endüstri tarihi için güçlü bir kaynak. Ancak **Airbnb'nin kendi mobil uygulamasında bu gesture'ı
hangi ekranlarda, tam olarak nasıl bir görsel/dokunsal ayarla uyguladığı bu araştırmada doğrudan
fetch edilip doğrulanmadı**; bu kısım genel mobil UX gözlemi/eğitim verisinden bir çıkarım
→ **pattern'in kökeni/tarihi güçlü doğrulandı, Airbnb'nin spesifik uygulaması doğrulanmadı**.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip Airbnb'nin kendi kaynağıyla güçlü doğrulanan maddeler:** madde 2
  (HTTP streaming, Airbnb Engineering Medium), madde 9 (react-dates geçersiz aralık davranışı,
  Airbnb'nin kendi GitHub deposu, ama "bugün canlı üründe böyle mi" ayrıca doğrulanmadı), madde 4
  (wishlist mekaniğinin kendisi, Airbnb yardım merkezi, ama boş durumun kendisi değil).
- **Genel endüstri/otorite kaynağıyla güçlü doğrulanan ama Airbnb'ye özgü olmayan maddeler:**
  madde 1 (NN/g skeleton screens), madde 3 ve kısmen 12 (Baymard zero-results/no-results page),
  madde 8 (Baymard inline form validation + NN/g error message guidelines), madde 14
  (Wikipedia pull-to-refresh tarihi). Bu dört madde güçlü doğrulanmış genel prensipler sunuyor,
  ancak Airbnb'nin bu prensipleri birebir nasıl uyguladığı ayrıca doğrulanmadı.
- **Kısmen doğrulanan, bağımsız/ikincil teknik kaynaklara dayanan maddeler:** madde 6
  (UserTesting blog, Airbnb'yi örnek gösteriyor ama tam kopya yok), madde 11 (Codelevate Lava
  rehberi + GoodUI A/B test yazısı, ikisi de üçüncü taraf ve GoodUI kendi bulgusunun spekülasyon
  olduğunu itiraf ediyor).
- **Büyük ölçüde ya da tamamen doğrulanmayan maddeler:** madde 5 (boş gelen kutusu), madde 7
  (ağ hatası/çevrimdışı), madde 10 (toast/snackbar metni), madde 12'nin sayfa kopyası kısmı
  (404/kaldırılmış ilan), madde 13 (rate-limit, açık bulgu boşluğu). Bu beş madde, Airbnb'nin
  gerçek ürün deneyimini fiilen ekran kaydıyla veya resmi bir kaynakla görmeden yazıldı;
  community.withairbnb.com forum gönderilerinin WebSearch özetlerine ya da genel eğitim verisi
  çıkarımına dayanıyor.
- `airbnb.com/rooms/999999999999` gibi geçersiz bir ilan URL'ine doğrudan erişim denemesi 403
  Forbidden ile engellendi (Airbnb'nin bot koruma sistemi); bu, gerçek 404 sayfası kopyasının,
  gerçek hata mesajlarının ve gerçek toast metinlerinin bu araştırmada doğrudan gözlemlenmesini
  engelleyen tekrarlayan bir kısıt oldu. Bu bölüm, önceki bölümlere (özellikle Airbnb'nin yardım
  merkezinde bolca belgelediği 04-booking-checkout ve 05-trust-safety-signals) kıyasla belirgin
  biçimde daha zayıf bir birincil kaynak tabanına sahip; bunun temel nedeni, "boş/hata durumları"
  konusunun Airbnb'nin kendi resmi dokümantasyon kanallarında (yardım merkezi, mühendislik blogu)
  nadiren doğrudan ele alınan bir konu olması. Bu doküman da diğerleri gibi "şu an tam olarak
  böyle" değil "tekrar gözlemlenen/kısmen belgelenen yerleşik pattern" çerçevesiyle okunmalı.
