# 2. İlan Kartı & Liste/Grid Görünümü (Listing Card & Browse)

Bu bölüm Airbnb'nin arama sonuçları ve keşif ekranlarındaki temel yapı taşını, ilan kartını
(listing card) ve bu kartların bir arada dizildiği grid/liste görünümünü kapsar: kart anatomisi,
fotoğraf carousel'i, wishlist kalp ikonu, fiyat gösterimi mantığı, rating/rozet yerleşimi, grid
responsive davranışı, yükleme durumları ve infinite scroll/pagination tercihi. Airbnb sürekli
A/B test yapan canlı bir ürün olduğu için burada "şu an tam olarak böyle" değil, tekrar tekrar
gözlemlenmiş/belgelenmiş yerleşik pattern'ler anlatılıyor.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: TechCrunch'ın Airbnb toplam fiyat
duyurusu haberi, Hospitable'ın Guest Favorite rozeti analizi, Baymard Institute'ın "2 Key Design
Principles for Product Listing Information" ve "Responsive Upscaling" makaleleri, Nielsen Norman
Group'un "Skeleton Screens 101" makalesi, Airbnb'nin kendi yardım merkezi sayfaları
(wishlist/koleksiyonlar ve arama sıralaması), GoodUI'nin Airbnb infinite scroll "leak" analizi,
Airbnb'nin kendi açık kaynak GitHub deposu (infinity.js), ve Prototypr'da yayınlanan bağımsız bir
kalp ikonu kullanılabilirlik deneyi. Ayrıca üçüncü taraf bir "reverse-engineered" tasarım sistemi
dökümü (VoltAgent/awesome-design-md, GitHub'da barındırılan bir DESIGN.md) fetch edildi; bu kaynak
Airbnb'nin resmi bir yayını DEĞİL, birinin Airbnb arayüzünü inceleyip tasarım tokenlarını tahmin
ederek yazdığı bir doküman olduğu için ilgili maddelerde ayrıca ve açıkça "doğrulanmadı" diye
işaretlendi; yalnızca somut/spesifik bir görsel referans noktası sağladığı için kullanıldı, kesin
gerçek olarak okunmamalı.

---

## 1. Fotoğraf öncelikli kart anatomisi (genel yapı)

**Ne olduğu:** İlan kartının temel iskeleti: üstte köşeleri yuvarlatılmış, 1:1'e yakın oranlı bir
fotoğraf alanı; bu alanın üzerine binen iki kalıcı katman (sol üstte "Guest Favorite" gibi bir
rozet, sağ üstte kalp/wishlist ikonu); fotoğrafın altında ise 4-5 satırlık kısa bir meta bloğu:
başlık/konum, tarih veya mesafe bilgisi (soluk/ikincil renk), ve sağa hizalı gecelik fiyat. Kart
bir bütün olarak tıklanabilir, ama carousel oku/nokta gibi alt bileşenler kendi tıklamalarını
üstlenip sayfa geçişini tetiklemeden görsel değiştirebiliyor.

**Nerede görülür:** İkisi de; web'de grid hücresi, mobilde ise dikey akan liste veya yatay
kaydırmalı "öneri şeridi" içindeki tekil kart olarak aynı iskelet tekrarlanıyor.

**UX gerekçesi:** Baymard Institute'ın liste öğesi (list item) tasarımı üzerine araştırması, aynı
tür ürünler/ilanlar için bilgi elemanlarının (isim, tip, özellik) tutarlı biçimde ve birbirinden
görsel olarak ayrışacak şekilde (yazı tipi farkı, boşluk, kalın/renk vurgusu) sunulması gerektiğini,
aksi halde kullanıcıların ürünleri kıyaslayamadığını ve uygun seçenek olmadığını varsayıp siteyi
terk edebildiğini gösteriyor. Airbnb kartındaki sabit iskelet (her zaman aynı sırada: görsel, rozet,
kalp, başlık, fiyat) bu ilkeyle örtüşüyor: kullanıcı yüzlerce kartı hızlıca tararken gözü hep aynı
yere bakarak fiyatı/başlığı bulabiliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Herhangi bir marketplace/e-ticaret/ilan listeleme
ürününde kart şablonunu (görsel + rozet + aksiyon ikonu + başlık + fiyat) tüm kategori/tip
varyasyonlarında birebir aynı sırada ve aynı tipografik hiyerarşide sabitlemek, kullanıcının
sonuçları hızlı tarayabilmesi için kritik. Bir kategori diğerinden farklı bir alan gösteriyorsa
(ör. bazı ürünlerde stok bilgisi bazılarında yok) bu boşluğun kartın genel oranını bozmayacak
şekilde ele alınması gerekir.

**Kaynak / güven notu:** Karma. Genel ilke (tutarlı bilgi elemanları, görsel ayrışma) Baymard'ın
"2 Key Design Principles for Product Listing Information" makalesinden fiilen fetch edilip
doğrulandı (https://baymard.com/blog/list-item-design-ecommerce), ama bu makale Airbnb'ye özel
değil, genel e-ticaret araştırması. Airbnb kartının birebir iskeleti (1:1 görsel oranı, rozet
sol üst, kalp sağ üst, 4-5 satır meta) üçüncü taraf reverse-engineered bir dökümden geliyor
(github.com/VoltAgent/awesome-design-md, DESIGN.md dosyası, fetch edildi ama Airbnb'nin resmi
kaynağı değil) → **kısmen doğrulanmadı: genel ilke güçlü kaynaklı, Airbnb'ye özgü detaylar
doğrulanmamış üçüncü taraf yorumu**.

---

## 2. Fotoğraf carousel'i: web'de hover ile ilerleme, mobilde swipe

**Ne olduğu:** Kart üzerindeki tek bir kapak fotoğrafının aslında bir dizinin ilk karesi olması;
web'de fare imleci kartın üzerine gelip sağ/sol yarısına hareket ettirildiğinde sonraki/önceki
fotoğrafın önizlemesi gösterilmesi (fotoğraf üstündeki küçük ok ikonlarına tıklanarak da aynı
şey yapılabiliyor), mobilde ise parmakla yatay kaydırma (swipe) ile aynı geçişin yapılması.
Fotoğrafın altında/üstünde küçük nokta (dot) göstergeleri, kaçıncı fotoğrafta olunduğunu ve
toplam kaç fotoğraf olduğunu gösteriyor.

**Nerede görülür:** İkisi de, ama giriş yöntemi farklı: web = hover + ok tıklama, mobil = swipe
jesti. Dot göstergeleri her iki platformda da ortak.

**UX gerekçesi:** Kart, ilanın tüm detay sayfasına gitmeden önce "ikinci bir izlenim" fırsatı
sunuyor: kullanıcı kapak fotoğrafını beğenmese bile ikinci/üçüncü fotoğrafı görüp ilgisini
sürdürebiliyor, bu da detay sayfasına tıklama (click-through) olasılığını artırıyor. Ok/dot
tıklamasının kartın ana linkini tetiklememesi (yani "fotoğraf gezinme" ile "ilana git" eylemlerinin
birbirinden ayrıştırılması) kullanıcının kazara istemediği sayfaya gitmesini engelliyor; bu ayrım
olmasaydı her fotoğraf denemesi kullanıcıyı listeden koparıp geri gitmeye zorlardı.

**Airbnb dışı bir uygulamaya uyarlama notu:** Görsel ağırlıklı herhangi bir kart bileşeninde
(emlak, moda/giyim, restoran, araç) tek kapak fotoğrafı yerine kartın kendisinde birkaç fotoğrafı
gezilebilir hale getirmek, kullanıcıyı detay sayfasına gitmeden önce ürünü/ilanı daha iyi
değerlendirmesini sağlar. Kritik teknik detay: gezinme kontrollerinin (ok, nokta) tıklama alanı
kartın ana link alanından `stopPropagation` ile ayrılmalı, aksi halde her fotoğraf denemesi
istenmeyen sayfa geçişine yol açar.

**Kaynak / güven notu:** **Doğrulanmadı, çoğunlukla üçüncü taraf yorumu.** Carousel'in varlığı ve
genel davranışı (hover/swipe, dot göstergesi, ok tıklamasının linki tetiklemediği) WebSearch
sonuçlarında birden fazla ikincil kaynakta (GitHub üzerindeki reverse-engineered DESIGN.md,
CodeSandbox örneği, Webflow forumu, Airbnb'nin kendi açık kaynak `epoxy` kütüphanesinin wiki
sayfası) tutarlı şekilde tarif ediliyor, ama bunların hiçbiri Airbnb'nin kendi tasarım ekibinin
birincil açıklaması değil; `epoxy` kütüphanesi Android tarafında genel bir carousel bileşeni
sağlıyor olsa da bunun doğrudan ilan kartı carousel'iyle bire bir eşleştiğini teyit edemedim.

---

## 3. Kapak fotoğrafı seçimi ve olası kişiselleştirme

**Ne olduğu:** İlanın ilk 5 fotoğrafından hangisinin kart üzerinde "kapak" (kullanıcının ilk
göreceği kare) olarak gösterileceğinin seçimi. Klasik yaklaşımda host bu sırayı elle belirliyor
(en güçlü fotoğrafı birinci sıraya sürüklüyor); ancak yakın dönemde bazı üçüncü taraf
kaynaklara göre bu seçim artık tamamen host kontrolünde değil, bir algoritmanın arama niyetine
göre ilk 5 fotoğraf arasından "en çok dönüşüm getireceğini tahmin ettiği" fotoğrafı
kişiselleştirerek her kullanıcıya farklı bir kapak gösterdiği iddia ediliyor.

**Nerede görülür:** İkisi de; kart üzerindeki kapak fotoğrafı olarak.

**UX gerekçesi:** Grid/liste görünümünde kullanıcı saniyeler içinde onlarca karttan geçiyor;
kapak fotoğrafı pratikte "tıklama kararının" en büyük tek değişkeni. Sabit bir kapak yerine
arama bağlamına (ör. "aile tatili" araması yapan kullanıcıya geniş oturma alanı fotoğrafı, "romantik
kaçamak" araması yapana yatak odası fotoğrafı) göre kişiselleştirilmiş bir kapak göstermek, aynı
ilanın farklı kullanıcı segmentlerine en alakalı görseliyle "satış" yapmasını sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Çoklu görseli olan her ürün/ilan kartında (emlak,
e-ticaret, iş ilanı fotoğrafları), kapak görselini sabit host seçimiyle sınırlamak yerine arama
sorgusu/kullanıcı segmentiyle eşleştirilmiş bir kapak seçim modeli denemek, tıklama oranını
artırabilir; ama bu, host/satıcının "kendi vitrinini kontrol edememesi" şikayetine yol açabileceği
için şeffaflık (hangi fotoğrafın neden seçildiğinin görülebilmesi) ile dengelenmeli.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden + tek ikincil kaynak.** "Host'un ilk
fotoğrafı elle seçmesi" kısmı genel, yaygın bilinen bir Airbnb hosting pratiği (birden fazla
host rehberi sitesinde tutarlı). Ancak "AI'ın arama niyetine göre kapak fotoğrafını
kişiselleştirdiği" iddiası yalnızca WebSearch sonucunda görülen tek bir ikincil kaynaktan
(airroi.com blog yazısı) geliyor; bu sayfayı fetch edip tam metnini okumadım, Airbnb'nin resmi
bir duyurusuna veya birincil kaynağa da rastlamadım. Bu maddeyi temkinli okumak gerekir, iddia
doğrulanmadı.

---

## 4. Wishlist kalp ikonu: konum, tasarım ve etiket sorunu

**Ne olduğu:** Kartın sağ üst köşesinde duran, dairesel/dokunma alanı geniş tutulmuş kalp
biçimli bir ikon; boş/dolu değil, dış hat (outline) ve dolu (filled, marka rengiyle) olmak üzere
iki durumu var. Dokunulduğunda ilan bir wishlist'e (koleksiyona) kaydediliyor; ikon anında dolu
hale geçip görsel geri bildirim veriyor.

**Nerede görülür:** İkisi de; kartın üstünde sabit pozisyonda.

**UX gerekçesi:** Airbnb bu ikonu önceden bir yıldız (star) olarak kullanıyordu; yıldızdan kalbe
geçildiğinde kullanım/etkileşimde belirgin bir artış gözlemlendiğine dair yaygın bir anlatı var
(yıldız "değerlendirme/puanlama" çağrışımı yaparken kalp "beğenme/biriktirme" çağrışımı yapıyor,
bu da daha güçlü bir duygusal bağ kuruyor). Ancak ikon tek başına her zaman yeterince açık
değil: bağımsız bir kullanılabilirlik deneyinde, sadece kalp ikonu gösterilen kullanıcıların
ekranda amaçsızca dokunup dolaştıktan sonra kalbi keşfettiği, kalbin yanına kısa bir metin
etiketi eklendiğinde ise kullanıcıların doğrudan ve tereddütsüz doğru elemente dokunduğu
gözlemlendi.

**Airbnb dışı bir uygulamaya uyarlama notu:** "Kaydet/favorile" eylemi için tek başına bir ikon
kullanmak (kalp, yer imi, yıldız fark etmez) yeni kullanıcılar için her zaman kendiliğinden
anlaşılır olmayabilir; özellikle ilk kullanım deneyiminde (onboarding) veya düşük dijital
okuryazarlığa sahip kullanıcı kitlelerinde ikonun yanına kısa bir metin etiketi eklemek veya
ilk birkaç görüntülemede bir tooltip/coach-mark göstermek, keşfedilebilirliği artırır.

**Kaynak / güven notu:** Karma. Kalp ikonunun sağ üstte, dairesel ve iki durumlu (outline/filled)
olduğu üçüncü taraf reverse-engineered bir dökümden geliyor (yukarıdaki gibi, doğrulanmadı).
Etiket/keşfedilebilirlik deneyi ise Prototypr'da yayınlanan bağımsız bir kullanılabilirlik
testi yazısından fiilen fetch edilip okundu
(https://blog.prototypr.io/does-the-heart-on-airbnb-need-a-label-dba7d1d10f8c): yazar 40
kullanıcıyı iki gruba ayırıp iPhone prototipleriyle test etmiş, etiketsiz tasarımda kullanıcıların
"ekranda amaçsızca dokunduğunu", etiketli tasarımda ise doğrudan kalbe dokunduğunu raporluyor
→ bu spesifik deney **doğrulandı**, ama bu Airbnb'nin resmi bir testi değil, bağımsız bir
tasarımcının kendi prototipiyle yaptığı küçük ölçekli bir kullanılabilirlik çalışması. Yıldızdan
kalbe geçişte "%30 etkileşim artışı" iddiası ise yalnızca bir WebSearch özetinde geçti, birincil
kaynağı fetch edilemedi → bu rakam **doğrulanmadı, eğitim verisinden/ikincil kaynak snippet'i**.

---

## 5. Fiyat gösterimi mantığı: gecelik fiyat vs. "vergiler hariç toplam fiyat"

**Ne olduğu:** Kart üzerinde gösterilen fiyatın neyi temsil ettiği zaman içinde değişti: önceleri
sade bir gecelik fiyat gösteriliyordu; 2022 sonunda ABD'de ve devamında 200'den fazla ülkede,
kullanıcının isteğe bağlı olarak açabileceği bir "toplam fiyatı göster" toggle'ı eklendi (bu
toggle 2023'te fiyat filtresine, haritaya ve ilan sayfalarına da yayıldı); Nisan 2025'te ise
Airbnb bu toggle'ı tamamen kaldırıp, tüm kullanıcılar için "vergiler hariç, diğer tüm ücretler
dahil toplam fiyatı" varsayılan gösterim haline getirdi ve kartın/sonuç listesinin yanına
"Fiyatlara tüm ücretler dahildir" tarzı bir açıklama etiketi ekledi.

**Nerede görülür:** İkisi de; arama sonuçları grid'i, harita pin'leri ve fiyat filtresi dahil
tüm fiyat gösterim noktalarında tutarlı biçimde uygulanıyor.

**UX gerekçesi:** Yalnızca gecelik/temel fiyatı gösterip temizlik ücreti, hizmet bedeli gibi
kalemleri sepete/checkout'a kadar saklamak, kullanıcıda "sürpriz ücret" (drip pricing) hissi
yaratıp güven kaybına yol açıyordu; bu, misafir şikayetlerinin önemli bir kaynağıydı. Toggle'ın
17 milyondan fazla kullanıcı tarafından tercih edilmiş olması, kullanıcıların büyük bir kısmının
zaten şeffaf/toplam fiyatı görmek istediğini gösterdi; bu da Airbnb'nin isteğe bağlı bir
özelliği varsayılan davranışa çevirme kararını destekleyen bir sinyal oldu. Kartlar üzerinde
doğru/nihai fiyata en yakın rakamı erken göstermek, kullanıcının farklı ilanları adil biçimde
kıyaslayabilmesini sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Ek ücret/vergi/hizmet bedeli içeren her
üründe (uçuş, etkinlik bileti, e-ticaret kargo ücreti) liste/kart görünümünde mümkün olan en
gerçekçi "nihai fiyata yakın" rakamı erken göstermek, checkout'a kadar sürpriz ücret biriktirmekten
daha güvenilir bir deneyim yaratır. İsteğe bağlı bir toggle olarak başlatıp kullanım oranını
ölçmek, bunu varsayılan hale getirip getirmeme kararını veri ile desteklemenin düşük riskli bir
yolu.

**Kaynak / güven notu:** **Doğrulandı.** TechCrunch'ın 21 Nisan 2025 tarihli haberi fiilen fetch
edilip okundu (https://techcrunch.com/2025/04/21/airbnb-will-automatically-display-total-price-to-all-users/):
toggle'ın 2022'de ABD'de tanıtıldığı, 2023'te fiyat filtresi/harita/ilan sayfalarına yayıldığı,
17 milyon kullanıcı tarafından kullanıldığı ve Nisan 2025'te varsayılan/zorunlu hale getirilip
toggle'ın kaldırıldığı bu kaynakta doğrudan geçiyor. Ayrıca aynı konuyu doğrulayan Skift ve
Yahoo Finance başlıkları WebSearch sonuçlarında görüldü (bunlar ayrıca fetch edilmedi, ama
TechCrunch'la tutarlı).

---

## 6. Guest Favorite ve Superhost rozetleri: kart üzerinde konum ve anlam farkı

**Ne olduğu:** Kartın fotoğrafı üzerine binen, sol üstte yer alan beyaz/pill biçimli bir rozet:
"Guest Favorite" (Misafir Favorisi). Bu rozet, Superhost rozetinden farklı bir katmanda çalışıyor:
Superhost host hesabı seviyesinde, üç ayda bir değerlendirilen bir statü iken; Guest Favorite tekil
ilan seviyesinde, günlük olarak güncellenen, makine öğrenmesi/yapay zeka araçlarıyla hesaplanan bir
puanlama. Rozetin kazanılması için ilanın ortalama 4.9 üstü puana, check-in kolaylığı/temizlik/açıklama
doğruluğu/iletişim/konum/değer kategorilerinde güçlü performansa, düşük iptal oranına, en az 5
misafir değerlendirmesine sahip olması ve kalite kaynaklı müşteri hizmeti şikayetlerinin ortalama
%1 gibi çok düşük seviyede kalması gerekiyor.

**Nerede görülür:** İkisi de; arama sonucu kartlarında (grid görünümünde) ve ilan detay
sayfasında.

**UX gerekçesi:** Onlarca benzer kart arasında hızlı taranırken, kalite sinyalini metne
(yüksek puan, çok review) değil tek bakışta anlaşılır bir görsel işarete indirgemek karar verme
hızını artırıyor. Rozetin ilan seviyesinde ve günlük güncellenmesi, host'un genel itibarından
bağımsız olarak "bu belirli ilan şu anda gerçekten iyi performans gösteriyor mu" sorusuna cevap
veriyor; bu da host seviyesindeki (üç ayda bir güncellenen, daha yavaş tepki veren) Superhost
rozetinin veremediği bir tazelik/güncellik sinyali sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Hem satıcı/host seviyesinde hem de ürün/ilan
seviyesinde ayrı güven sinyalleri olan her marketplace'te (e-ticaret satıcı rozetleri + ürün
rozetleri gibi), bu iki katmanı görsel olarak ayırmak ve ürün seviyesindeki rozeti daha sık
güncellemek, kullanıcıya güncel/dinamik bir kalite sinyali verir. Rozet kriterlerinin (puan eşiği,
minimum review sayısı, düşük şikayet oranı) şeffaf biçimde yayınlanması, rozetin manipülasyona
kapalı ve güvenilir algılanmasını destekler.

**Kaynak / güven notu:** Karma. Rozetin kriterleri (4.9 üstü puan, kategori bazlı performans,
%1 şikayet oranı, en az 5 review, günlük güncelleme, host seviyesi vs. ilan seviyesi farkı)
Hospitable'ın "Airbnb Guest Favorite Badge" makalesinden fiilen fetch edilip doğrulandı
(https://hospitable.com/airbnb-guest-favorite-badge); bu bir Airbnb resmi kaynağı değil ama
konu üzerine host topluluğuna yönelik detaylı bir üçüncü taraf analiz. Rozetin kartta "sol üst,
beyaz pill" biçiminde göründüğü bilgisi ise reverse-engineered DESIGN.md dökümünden geliyor,
doğrulanmadı. Top %1/%5/%10 kademe rozetlerinin ayrı görsel varyantları olduğu iddiası da
yalnızca bir WebSearch snippet'inden (thepointsguy.com) geldi, fetch edilmedi →
**kısmen doğrulanmadı**.

---

## 7. Rating/review sayısının kartta görünme eşiği

**Ne olduğu:** Bir ilanın ortalama puanının kart üzerinde (başlığın yanında, genelde bir yıldız
ikonu + ondalıklı puan olarak) gösterilebilmesi için ilanın önce belirli bir minimum
değerlendirme sayısına ulaşması gerekiyor. Airbnb'nin kendi yardım merkezi sayfasına göre bu
eşik en az 3 misafir değerlendirmesi; bu eşiğe ulaşana kadar kartta puan/yıldız alanı boş
kalıyor veya hiç gösterilmiyor.

**Nerede görülür:** İkisi de; hem arama sonucu kartında hem ilan sayfasının başlık bölümünde.

**UX gerekçesi:** Tek bir değerlendirmeye (özellikle çok düşük veya çok yüksek bir puana)
dayanarak ortalama göstermek istatistiksel olarak yanıltıcı olur ve yeni ilanları haksız yere
cezalandırabilir veya kayırabilir; 3 değerlendirmelik bir minimum eşik, gösterilen puanın en
azından biraz örneklem büyüklüğüne sahip olmasını garanti ediyor. Bu aynı zamanda yeni host'lar
için bir "soğuk başlangıç" (cold start) sorununu de yönetiyor: ilk birkaç misafir puan
göstermeden rezervasyon yapmaya ikna edilmeli, bu da arama sıralaması/kart tasarımındaki diğer
sinyallerin (fotoğraf kalitesi, fiyat, konum) bu dönemde daha ağır basmasını gerektiriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcı review'larına dayalı puanlama gösteren
her üründe (e-ticaret, hizmet marketplace'i, uygulama mağazası) minimum bir review eşiği
belirlemeden ortalama puan göstermek yanıltıcı olabilir; eşik altındaki ürünler için puan yerine
"Yeni" rozeti veya boş alan göstermek, hem istatistiksel dürüstlüğü korur hem de yeni
satıcı/ürünlerin görünürlüğünü tamamen yok etmez.

**Kaynak / güven notu:** **Kısmen doğrulandı.** "En az 3 misafir puanladığında ortalama puanın
başlığın yanında, arama sonuçlarında ve ilanda gösterildiği" ifadesi WebSearch sonucunda
airbnb.com/help/article/1257 ("Ratings for homes") sayfasına atıfla görüldü, ancak bu spesifik
sayfayı ayrıca fetch etmedim; bunun yerine airbnb.com/help/article/39 ("How search results
work") sayfasını fetch ettim ve bu sayfa rating/review'ların sıralama algoritmasında (kalite
sinyali olarak) önemli olduğunu doğruluyor, ama kartın görsel yerleşimini tarif etmiyor →
3-değerlendirme eşiği doğrudan birincil kaynaktan teyit edilmedi, ikincil kaynak özetine
dayanıyor.

---

## 8. Grid layout & responsive breakpoint davranışı

**Ne olduğu:** Aynı kart bileşeninin ekran genişliğine göre farklı sayıda sütunda dizilmesi:
büyük masaüstü ekranlarında dört kart yan yana ("4-up"), tablet/orta genişlikte iki kart yan
yana ("2-up"), mobilde ise kartların tek sütunda alt alta dizilmesi ("1-up", genelde dikey akan
liste veya yatay kaydırmalı şeritler halinde). Kart oranı (1:1'e yakın görsel + sabit meta
yüksekliği) sütun sayısı değişse de korunuyor.

**Nerede görülür:** Ağırlıklı olarak web (responsive breakpoint geçişleri); mobil uygulamada
zaten tek sütun/yatay şerit sabit.

**UX gerekçesi:** Baymard Institute'ın büyük ekran/responsive tasarım araştırmasına göre, ekran
genişledikçe ürün listelerine ek sütun eklemek (aynı kartı büyütmek yerine) kullanıcının aynı
anda gördüğü seçenek sayısını artırıp genel bakışı (overview) güçlendiriyor; ama bu araştırma
sütun sayısının kontrolsüz artmamasını, genelde ürün tipine göre 5-8 sütunla sınırlı tutulmasını
öneriyor, aksi halde kullanıcı "bilgi denizinde" kayboluyor. Airbnb'nin göreli olarak
mütevazı sütun sayısı (masaüstünde 4), kartların görsel-ağırlıklı doğası (fotoğraf carousel'i,
rozet, meta bloğu) düşünüldüğünde bu ilkeyle uyumlu: her kart kendi başına dikkat çekmesi gereken
görsel-yoğun bir birim olduğu için Wayfair gibi ürün-yoğun sitelere kıyasla daha az sütun tercih
ediliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Görsel ağırlıklı kart tasarımı kullanan (emlak,
moda, restoran, seyahat) ürünlerde büyük ekranlarda sütun sayısını agresif biçimde artırmak
yerine, Baymard'ın önerdiği üst sınır aralığında (yaklaşık 5-8 sütun, ürün tipine göre) kalmak
ve kart oranını sütun sayısı değişse de sabit tutmak, taranabilirliği korur. Metin-ağırlıklı,
karşılaştırma odaklı ürünlerde (ör. teknik ürün spesifikasyonları) daha fazla sütuna izin
verilebilir.

**Kaynak / güven notu:** Karma. Genel ilke (büyük ekranda sütun ekleme, 5-8 sütun üst sınırı)
Baymard'ın "Responsive Upscaling" makalesinden fiilen fetch edilip doğrulandı
(https://baymard.com/blog/responsive-upscaling); ama bu makale açıkça Airbnb'yi değil
Wayfair'i örnek alıyor, Airbnb'ye özel bir ifade içermiyor. Airbnb'nin kendi breakpoint
sayıları (masaüstü 4-up, tablet 2-up, mobil 1-up, belirli piksel aralıklarıyla) reverse-engineered
DESIGN.md dökümünden geliyor, doğrulanmadı → **kısmen doğrulanmadı, genel ilke güçlü kaynaklı +
Airbnb'ye özgü sayılar doğrulanmamış**.

---

## 9. Görsel yükleme durumu: skeleton screen (iskelet ekran)

**Ne olduğu:** Kart fotoğrafları henüz yüklenmeden önce, nihai görselin kapladığı alanın aynısını
kaplayan gri/nötr renkli bir placeholder blok (skeleton) gösterilmesi; metin alanları (başlık,
fiyat) için de benzer şekilde kısa gri dikdörtgenler kullanılması. İçerik geldikçe bu bloklar
gerçek görsel/metinle yer değiştiriyor.

**Nerede görülür:** İkisi de; grid ilk yüklendiğinde veya kaydırma sırasında yeni kartlar
render edilirken.

**UX gerekçesi:** Nielsen Norman Group'un araştırmasına göre skeleton screen'ler özellikle
1-10 saniye arası süren tam sayfa/bölüm yüklemelerinde tercih edilmeli; sayfanın nihai
düzenini önceden ima eden gri bloklar, kullanıcının "sistem çalışıyor ve şu şekilde
dolacak" mental modelini erken kurmasını sağlayarak algılanan bekleme süresini kısaltıyor.
Akademik bir çalışma (Mejtoft ve ark., 2018) skeleton screen kullanan sayfaların algılanan hız
ve gezinme kolaylığı açısından daha yüksek puan aldığını gösteriyor. NN/g ayrıca 1 saniyeden
kısa yüklemelerde skeleton screen kullanmamayı öneriyor çünkü çok hızlı beliren/kaybolan bir
iskelet, kullanıcıda "yetişemedim" hissi yaratıp rahatsız edici olabiliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Görsel ağırlıklı herhangi bir grid/liste
görünümünde (ürün kataloğu, sosyal medya akışı, ilan listesi), görsellerin yüklenme süresi
1 saniyeyi geçiyorsa boş beyaz alan veya dönen bir spinner yerine kartın nihai oranını taklit
eden gri skeleton blokları kullanmak, algılanan performansı artırır. 1 saniyenin altındaki
yüklemelerde skeleton eklemek gereksiz ve rahatsız edici olabilir, bu eşiğe dikkat edilmeli.

**Kaynak / güven notu:** **Doğrulandı (genel ilke), Airbnb'ye özgü uygulanışı doğrulanmadı.**
NN/g'nin "Skeleton Screens 101" makalesi fiilen fetch edilip okundu
(https://www.nngroup.com/articles/skeleton-screens/): 1-10 saniye aralığı önerisi, 10 saniye
üstünde progress bar'a geçiş önerisi ve Mejtoft ve ark. 2018 çalışmasına atıf bu kaynakta
doğrudan geçiyor. Ancak bu makale Airbnb'yi örnek almıyor (Headspace örneği kullanıyor);
Airbnb'nin kartlarında birebir gri skeleton blok kullandığı görsel olarak doğrudan gözlemlenmedi,
bu madde genel NN/g ilkesinin Airbnb'nin muhtemel/tipik bir grid uygulamasına genellenmesinden
ibaret → Airbnb'ye özgü kısım **doğrulanmadı, eğitim verisinden genelleme**.

---

## 10. Infinite scroll vs. "Show more" / sayfalama tartışması

**Ne olduğu:** Arama sonuçlarının sonuna gelindiğinde yeni ilanların nasıl yüklendiği: klasik
sayfalama (1, 2, 3... sayfa numaraları) yerine ya kullanıcı kaydırmaya devam ettikçe otomatik
olarak yeni kartların eklendiği "infinite scroll", ya da her N sonuçtan sonra kullanıcının
manuel tıklaması gereken bir "Show more" (Daha fazla göster) butonu. Airbnb'nin bu iki yaklaşım
arasında zaman içinde A/B testleri yaptığı biliniyor: bir varyantta 40 sonuç/sayfa + "Show More"
butonu, diğer varyantta kesintisiz/infinite scroll test edilmiş.

**Nerede görülür:** İkisi de; ama mekanik olarak web ve mobilde farklı: mobilde native
uygulamalarda infinite scroll neredeyse standart beklenti, web'de ise sayfalama/"show more"
ile infinite scroll arasında tarihsel olarak gidip gelinmiş.

**UX gerekçesi:** Infinite scroll, kullanıcının "sayfa değiştirme" gibi ek bir karar/eylem
adımı olmadan taramaya devam etmesini sağlayarak sürtünmeyi azaltıyor gibi görünse de, literatürde
tartışmalı bir pattern: Etsy'nin 2012'deki kendi infinite scroll deneyinde görüntülenen ürün
sayısının %50, satışların ise yaklaşık %23 düştüğü rapor edilmiş (footer'a ve "sonraki adım"
hissine ulaşamama, geri dönüşte kaydırma pozisyonunu kaybetme gibi nedenlerle). Airbnb'nin kendi
mühendislik ekibi ise bu sorunu teknik tarafta ciddiye almış: performans/DOM şişmesi sorununu
çözmek için `infinity.js` adlı, iOS'taki `UITableView` mantığından esinlenen bir liste
sanallaştırma (virtualization, sadece görünen öğeleri render etme) kütüphanesi geliştirip açık
kaynak yayınlamışlar; bu, infinite scroll'un yalnızca bir tasarım kararı değil aynı zamanda ciddi
bir mühendislik/performans problemi olduğunu gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Uzun sonuç listesi olan her üründe infinite
scroll'u seçmeden önce iki riski birlikte değerlendirmek gerekir: (a) ürün/UX riski, kullanıcının
"nereye kadar geldiğini" ve footer/alt bilgiyi kaybetmesi, geri dönüşte kaydırma konumunun
korunup korunmadığı; (b) mühendislik riski, DOM'a sınırsız öğe eklemenin performans/bellek
sorunlarına yol açması, bu yüzden gerçek bir liste sanallaştırma (yalnızca görünen öğeleri
render etme) stratejisi olmadan infinite scroll'a girişmemek. Alternatif olarak "Show more"
butonuyla infinite scroll'un hibrit bir versiyonu (belirli bir toplu grup yüklenir, sonra buton
görünür) iki yaklaşımın da bazı avantajlarını taşıyabilir.

**Kaynak / güven notu:** Karma. Airbnb'nin Haziran/Temmuz 2022'de 40 sonuç/sayfa + "Show More"
ile infinite scroll'u A/B test ettiği bilgisi GoodUI'nin "leak" analizinden fiilen fetch edilip
okundu (https://goodui.org/leaks/airbnb-retests-the-infamous-infinite-scroll/); bu bir "leak"
yani üçüncü tarafın canlı üründe gözlemlediği bir değişikliği yorumladığı bir kaynak, Airbnb'nin
resmi açıklaması değil, ve yazarın kendisi de nihai sonuç/dönüşüm verisinin paylaşılmadığını
belirtiyor. `infinity.js` kütüphanesinin varlığı, amacı (DOM sanallaştırma, UITableView'dan
esinlenme) ve 2018'de arşivlendiği bilgisi Airbnb'nin kendi GitHub deposundan fiilen fetch edilip
**doğrulandı** (https://github.com/airbnb/infinity) → bu, Airbnb'nin birincil/resmi bir kaynağı.
Etsy'nin %50/%23 rakamları ise GoodUI sayfasının kendi içinde Etsy'ye atıfla aktarılıyor, ayrıca
doğrulanmadı, ikinci elden bir aktarım olarak okunmalı.

---

## 11. Kart hover state (yalnızca web)

**Ne olduğu:** Fare imleci bir karta geldiğinde kartın hafifçe yükselmiş/öne çıkmış hissi
vermesi için uygulanan ince bir gölge (box-shadow) katmanı; kartın kendisi büyümüyor veya
kaymıyor, sadece çok hafif bir "elevation" (yükseklik) efektiyle görsel olarak öne çıkıyor.
Aynı anda, yukarıda anlatıldığı gibi carousel oklarının/dot'larının görünür hale gelmesi de
genelde bu hover anına denk geliyor.

**Nerede görülür:** Yalnızca web (dokunmatik ekranlarda "hover" kavramı fiziksel olarak
karşılık bulmadığı için mobilde bu davranış yok; mobilde eşdeğeri dokunma anındaki hafif bir
"press" durumu olabilir).

**UX gerekçesi:** Grid içinde onlarca özdeş boyutlu kart yan yana dururken, hover anında hafif
bir gölge/yükselme kullanıcıya "şu an hangi kartla etkileşime geçtiğini" fark ettirip fare
imlecinin konumunu görsel olarak teyit ediyor; bu, özellikle yoğun grid düzenlerinde "hangi
kart hangi tıklamaya karşılık geliyor" belirsizliğini azaltan standart bir mikro-etkileşim
prensibi. Efektin abartılı değil (kartın boyutu/konumu değişmiyor, sadece gölge) ince tutulması,
grid'in genel düzeninin hover sırasında bozulmamasını, komşu kartların yer değiştirmemesini
sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Grid tabanlı herhangi bir web arayüzünde
(e-ticaret, portföy, dosya gezgini) kart/öğe hover durumları için boyut değiştirmek yerine
ince bir gölge/yükselme kullanmak, grid'in düzenini bozmadan etkileşim geri bildirimi verir.
Mobil karşılığı tasarlanırken hover yerine dokunma anındaki (touch-down) kısa bir görsel geri
bildirime (hafif opaklık azalması gibi) geçilmeli, hover'ın birebir mobile taşınması mümkün
olmadığı unutulmamalı.

**Kaynak / güven notu:** **Doğrulanmadı, üçüncü taraf yorumu.** Bu davranış ve spesifik
box-shadow değerleri, yukarıda bahsedilen reverse-engineered DESIGN.md dökümünden ve WebSearch
özetlerinden geliyor; dökümün kendisi de bu alanı "genel hover politikası nedeniyle resmi
olarak belgelenmemiş" diye işaretliyor. Yani bu, Airbnb'nin kendi tasarım sisteminin resmi bir
açıklaması değil, birinin tarayıcıda inceleyip çıkardığı bir gözlem; genel prensip (ince
gölge/elevation ile hover geri bildirimi) yaygın bir web tasarım pratiği olduğu için makul, ama
Airbnb'ye özgü kesin gerçek olarak okunmamalı.

---

## 12. "Save to wishlist" (koleksiyona kaydetme) akışının kart üzerinden başlaması

**Ne olduğu:** Kalp ikonuna dokunmanın tek bir eylem değil, küçük bir akışın giriş noktası
olması: ilk dokunuşta ilan otomatik olarak "varsayılan" bir wishlist'e (genelde aynı arama
oturumundan kaydedilen diğer ilanlarla aynı koleksiyona) ekleniyor; kullanıcı isterse "Change"
(Değiştir) diyerek ilanı başka bir koleksiyona taşıyabiliyor veya yeni bir koleksiyon adı
girip oluşturabiliyor. Her koleksiyon en fazla 100 ilan tutabiliyor, kaydedilen ilanlar
aramada seçilen tarihlerle birlikte saklanıyor ve daha sonra güncellenebiliyor. Koleksiyonlar
arkadaşlarla paylaşılabiliyor/işbirlikli hale getirilebiliyor: davet edilen kişiler not
ekleyebiliyor, ilanlara oy verebiliyor, önerilen tarihleri/misafir sayısını değiştirebiliyor;
işbirlikçi olmayan kişilerle ise yalnızca görüntüleme (view-only) linki paylaşılabiliyor.

**Nerede görülür:** İkisi de; giriş noktası (kalp ikonu) kart üzerinde, akışın geri kalanı
(koleksiyon seçimi/oluşturma, işbirliği) ayrı bir ekran/panel üzerinde devam ediyor.

**UX gerekçesi:** Kaydetme eylemini tek dokunuşla tamamlayıp (otomatik gruplama) organizasyon
kararını (hangi koleksiyona, kiminle paylaşılacak) isteğe bağlı, sonraya bırakılabilir bir adım
haline getirmek, kullanıcının kart üzerinde "hızlı kaydet" ile "detaylı organize et" ihtiyaçlarını
aynı anda karşılamasını sağlıyor: aceleyle tarayan bir kullanıcı tek dokunuşla kaydedip devam
edebiliyor, planlama yapan bir kullanıcı ise aynı ikondan başlayıp arkadaşlarıyla işbirlikli bir
seyahat planına dönüştürebiliyor. Bu, tek bir mikro-etkileşimin (kalbe dokunma) hem düşük hem
yüksek niyetli kullanıcı için doğru maliyette bir giriş noktası olmasını sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** "Kaydet/favorile" eylemi olan her üründe
(e-ticaret, emlak, iş ilanı, içerik platformu), kaydetme eylemini varsayılan/otomatik bir
koleksiyona anında tamamlayıp, organizasyon/paylaşım kararını sonraki, isteğe bağlı bir adıma
ertelemek, tek dokunuşluk hız ile daha derin planlama ihtiyacını aynı bileşende buluşturur.
Koleksiyonların paylaşılabilir ve işbirlikli olması, özellikle grup halinde karar verilen
satın alımlarda (seyahat, ev eşyası, düğün organizasyonu) ürünü tek kullanıcılık bir araçtan
çok kullanıcılı bir karar verme aracına dönüştürür.

**Kaynak / güven notu:** **Doğrulandı.** Airbnb'nin kendi yardım merkezi sayfası fiilen fetch
edilip okundu (https://www.airbnb.com/help/article/1236, "Use wishlists to save listings"):
kalbe dokunarak kaydetme, aynı aramadan kaydedilen ilanların otomatik olarak aynı wishlist'e
eklenmesi ve "Change" ile taşınabilmesi, koleksiyon başına 100 ilan sınırı, tarihlerin
saklanıp güncellenebilmesi, işbirlikçilerin not ekleme/oy verme/tarih-misafir sayısı değiştirme
yetkisi ve view-only paylaşım linki bu kaynakta doğrudan tarif ediliyor. Bu, bölümdeki en güçlü
şekilde doğrulanmış maddelerden biri; Airbnb'nin kendi birincil kaynağından geliyor.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip içeriği doğrulanan kaynaklar (11 adet):** TechCrunch'ın toplam fiyat
  haberi, Hospitable'ın Guest Favorite analizi, Baymard'ın "List Item Design" makalesi, Baymard'ın
  "Responsive Upscaling" makalesi, NN/g'nin "Skeleton Screens 101" makalesi, Airbnb yardım
  merkezinin "Use wishlists" sayfası, Airbnb yardım merkezinin "How search results work" sayfası,
  GoodUI'nin infinite scroll leak analizi, Airbnb'nin kendi GitHub deposu (infinity.js),
  Prototypr'daki kalp ikonu kullanılabilirlik deneyi, ve (referans/karşılaştırma amaçlı, resmi
  olmayan) VoltAgent/awesome-design-md reverse-engineered DESIGN.md dökümü.
- Bunlardan **6 tanesi Airbnb'nin kendi birincil kaynağı veya güçlü/doğrudan haber kaynağı**
  (TechCrunch, iki Airbnb yardım merkezi sayfası, Airbnb'nin kendi GitHub deposu); **2 tanesi
  Airbnb'ye özgü olmayan genel UX araştırması** (iki Baymard makalesi, NN/g makalesi) ve Airbnb'ye
  yalnızca eğitim verisi/genelleme yoluyla bağlandı; **2 tanesi bağımsız üçüncü taraf
  analiz/deney** (GoodUI, Prototypr) ve **1 tanesi kesin olarak Airbnb'nin resmi kaynağı
  olmayan, reverse-engineered bir doküman** (DESIGN.md) olarak işaretlendi.
- Yukarıdaki 12 pattern'den **3 tanesi** (madde 5: fiyat toggle, madde 10'un infinity.js kısmı,
  madde 12: wishlist akışı) doğrudan Airbnb'nin kendi birincil kaynağıyla güçlü biçimde
  doğrulandı; geri kalanı "karma" (bir kısmı doğrulandı bir kısmı doğrulanmadı) ya da
  "doğrulanmadı, eğitim verisinden/üçüncü taraf yorumu" olarak işaretlendi. Canlı üründe sürekli
  A/B testler yapıldığından bu doküman "şu an tam olarak böyle" değil "yerleşik/tekrar
  gözlemlenen pattern" çerçevesiyle okunmalı.
