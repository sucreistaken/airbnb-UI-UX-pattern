# 10. Kişiselleştirme & Öneri Kartları

Bu bölüm, Airbnb'nin "sana özel" hissettirmeye çalışan tüm yüzeyleri kapsıyor: yaşam tarzı
temelli "Kategoriler" ızgarası (ve 2025'te bu ızgaranın büyük ölçüde kaldırılması), arama/gezinme
geçmişinin ana sayfa ve arama sıralamasını nasıl etkilediği, embedding tabanlı "benzer ilanlar"
önerisi, wishlist (kalp) sinyalinin bir öneri motoruna nasıl beslendiği, 2025-2026 "Summer
Release" döneminde tanıtılan AI destekli seyahat planlama özellikleri, misafir tarafı fiyat
kişiselleştirme mesajları, "Guest Favorite" rozetinin kalite sinyalini kişiselleştirilmiş biçimde
yüzeye çıkarması, geçmiş seyahate dayalı e-posta/bildirim yeniden-etkileşimi, host tarafı Smart
Pricing/Price Tips öneri araçları ve mevsimsel/özel gün temalı küratörlü koleksiyonlar.

Bu bölüm, projedeki diğer bölümlere kıyasla **doğrulama açısından en riskli** alanlardan biri;
kişiselleştirme/öneri motoru ve özellikle "AI" iddiaları tam olarak eğitim verisinin bayat veya
yanlış olabileceği bir alan, çünkü Airbnb bu yüzeyleri sık sık ve sessizce değiştiriyor (araştırma
sırasında bunun somut bir örneğine denk gelindi: 2022'de büyük bir lansmanla tanıtılan "Kategoriler"
ızgarası, 2025'in ortasında büyük ölçüde kaldırıldı, bkz. madde 1). Bu yüzden her maddede kaynak
notu özellikle titiz tutuldu ve WebSearch özetlerinden gelen ama doğrudan fetch edilerek teyit
edilemeyen hiçbir iddia kesin bir gerçek gibi sunulmadı.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: Airbnb'nin kendi yardım merkezinden
**recommendation systems (öneri sistemleri) açıklama sayfası** (help/article/4083, AB'nin Dijital
Hizmetler Yasası/DSA şeffaflık gerekliliğine karşılık geldiği düşünülen bir sayfa), **AI destekli
kişiselleştirme ve "Help improve AI-powered features" kapatma anahtarını açıklayan sayfa**
(help/article/4097), Smart Pricing sayfası (help/article/1168), fiyat/Price Tips sayfası
(help/article/474), Guest Favorites sayfası (help/article/3495); Airbnb Engineering'in kendi Medium
yayınından **"Personalizing Airbnb search by learning from the guest journey"** (Daochen Zha, 21
Temmuz 2026, transformer tabanlı sıra modeli) ve **"Listing Embeddings in Search Ranking"**
(Mihajlo Grbovic, 13 Mart 2018); Airbnb'nin 2022 Summer Release basın bülteninin hospitalitynet.org
üzerinden erişilebilen tam metni (Kategoriler lansmanı) ve Airbnb Tech Blog'un Kategoriler
yazısının başlangıç kısmı (predictiveanalyticsworld.com üzerinden, tam içerik kilitli). Bunların
dışında kalan pek çok iddia (Kategoriler'in 2025'te kaldırılması, "For You" sekmesi, 2025/2026
Summer Release AI özellikleri, Guest Favorite'in tam sıralama etkisi, misafir tarafı "bu tarihler
genelde ucuz" mesajı) sadece WebSearch özetlerinden geliyor, doğrudan fetch ile teyit edilemedi
(çoğu resmi Airbnb newsroom sayfası 403 Forbidden ile engellendi) ve ilgili maddelerde ayrıca
işaretlendi.

---

## 1. Kategoriler: yaşam tarzı temelli keşif ızgarası (ve 2025'teki büyük geri çekilmesi)

**Ne olduğu:** Airbnb, 2022 Summer Release'te ("2022 Yaz Sürümü") ana sayfa ve arama girişini
kökten değiştiren "Kategoriler" adlı bir özellik tanıttı: klasik "nereye gitmek istiyorsun"
sorusunu tersine çevirip "ne tür bir yerde kalmak istiyorsun" sorusuyla başlayan, OMG!, Amazing
pools (harika havuzlar), Treehouses (ağaç evler), Design (tasarım), Castles (kaleler), Caves
(mağaralar) gibi 56 kategoriden oluşan bir keşif ızgarası. Fetch edilen basın bültenine göre
kategoriler, 4 milyondan fazla benzersiz evi kapsıyor ve şu yöntemle oluşturuldu: önce makine
öğrenmesi milyonlarca ilanın başlıklarını, yazılı açıklamalarını, fotoğraf altyazılarını, host'un
girdiği yapısal verileri ve misafir review'larını analiz ediyor; ardından Airbnb'nin küratörlük
ekibi bu otomatik sınıflandırmayı elle gözden geçirip her kategori için öne çıkan fotoğrafı
seçiyor (ör. "Amazing Pools" kategorisindeki bir ilanın ilk fotoğrafının havuzu göstermesi
sağlanıyor). Ancak bu araştırma sırasında ortaya çıkan önemli bir gelişme: WebSearch özetlerine
göre (doğrudan fetch edilerek teyit edilemedi, ilgili haber kaynağı 403 ile engellendi) Airbnb bu
kategori tabanlı aramayı 2025'in nisan-mayıs döneminde büyük ölçüde kaldırdı; Experiences ve
Services (deneyimler ve hizmetler) sekmelerinin ana sayfaya eklenmesiyle yer açmak amacıyla,
onlarca kategoriden oluşan ızgara yerine üç ana sekmeli (Homes/Experiences/Services) ve geleneksel
filtrelere dayanan bir arayüze geçildiği, bunun host topluluğunda (özellikle niş kategorilerdeki
host'larda görünürlük kaybı şikayetleriyle) tartışmalı karşılandığı iddia ediliyor.

**Nerede görülür:** İkisi de; hem web hem mobil ana sayfa/arama girişinde (varken) yatay kaydırmalı
bir kategori şeridi olarak gösteriliyordu.

**UX gerekçesi:** Fetch edilen basın bülteninin kendi çerçevelemesi, "arz'ın hedefi belirlemesi,
hedefin arz'ı belirlemesi değil" mantığına dayanıyordu: kullanıcı önceden bir şehir/bölge adı
bilmek zorunda kalmadan, "havuzlu bir yer" veya "ağaç evi" gibi somut bir arzuyla arama
başlatabiliyordu; bu, özellikle "nereye gideceğimi bilmiyorum ama ne istediğimi biliyorum"
durumundaki kullanıcılar için klasik bir metin arama kutusundan daha düşük bilişsel yük
taşıyordu. Kategorilerin makine öğrenmesiyle oluşturulup insan gözden geçirmesiyle
cilalanması (madde, DLS bölümündeki "insan + ML" örüntüsüyle tutarlı, bkz.
`08-visual-design-system.md` madde 6), otomatik sınıflandırmanın ham haliyle tutarsız/yanıltıcı
kapak görselleri üretme riskini azaltıyordu. 2025'teki geri çekilmenin (doğrulanmamış) gerekçesi
ise farklı bir ürün stratejisi baskısı gibi görünüyor: ana sayfa gerçek dünya alanı, Experiences/
Services gibi yeni iş kollarına ayrılmak zorunda kalmış olabilir; bu, "kişiselleştirme/keşif
özelliği" ile "iş modeli genişlemesi" arasındaki gerilimin canlı bir örneği.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcının aradığı şeyi net bir kelimeyle
tanımlayamadığı ("ne istediğimi biliyorum ama nereden bulacağımı bilmiyorum") bir keşif
senaryosunda, klasik anahtar kelime aramasının önüne "somut, görsel, arzu-temelli" bir kategori
ızgarası koymak; arama kutusunu zorunlu ilk adım olmaktan çıkarabilir. Ama bu bölümün asıl
devredilebilir dersi, Kategoriler'in kendisinden çok onun akıbeti: büyük bir lansmanla tanıtılan
bir keşif özelliği bile, şirketin iş önceliği değiştiğinde (burada yeni gelir kollarına ana sayfa
alanı açma ihtiyacı) sessizce geri çekilebilir; bu yüzden "Airbnb şu an tam olarak böyle
yapıyor" diye kopyalanacak bir referans, yayın tarihi belirtilmeden asla kesin bir gerçek gibi
sunulmamalı.

**Kaynak / güven notu:** Kısmen doğrulandı, karma. Kategorilerin 2022 lansmanı, 56 sayısı, 4
milyon+ ev kapsamı ve ML+insan küratörlük süreci **doğrulandı**: doğrudan fetch edilen
hospitalitynet.org üzerindeki Airbnb 2022 Summer Release basın bülteni tam metninden birebir
alındı (bu, news.airbnb.com'un kendi sayfası 403 Forbidden ile engellendiği için republish edilen
bir kopyadan okundu, ama içerik Airbnb'nin kendi basın bülteni metni). Kategorilerin ML
mimarisiyle ilgili "Building Airbnb Categories with ML and Human-in-the-Loop" başlıklı Airbnb Tech
Blog yazısının varlığı ve kaynağı (Airbnb Engineering, Kasım 2022) doğrulandı ama tam teknik
içeriği (hangi model, hangi ağırlıklar) predictiveanalyticsworld.com üzerinden okunan kopyada
kilitliydi, Medium'daki orijinaline ayrıca ulaşılıp fetch edilmedi. **Kategorilerin 2025'te
kaldırılması/küçültülmesi iddiası ise doğrulanmadı, sadece WebSearch özetinden**: bu konudaki asıl
haber kaynağı (Skift, "Airbnb's Categories Were Meant to Reinvent the Search Box, Now They're
Gone") doğrudan fetch edilmeye çalışıldı ama 403 Forbidden ile engellendi, resmi bir Airbnb
newsroom açıklaması da bulunup fetch edilmedi; bu yüzden kaldırılmanın kesin tarihi, kapsamı ve
resmi gerekçesi bu araştırmada birincil kaynakla teyit edilemedi.

---

## 2. Kişiselleştirilmiş ana sayfa/arama sıralaması: geçmiş rezervasyon ve gezinme sinyali

**Ne olduğu:** Doğrudan fetch edilen Airbnb Engineering yazısına göre Airbnb, 2026'da guest'in
"tüm Airbnb ilişkisinin arkını" tek bir modelde kodlayan bir transformer tabanlı sıra (sequence)
modeli devreye aldı: **uzun-vadeli bir sıra** (7 yıla kadar geçmiş rezervasyon ve review verisi,
en fazla 80 olay) ve **kısa-vadeli bir sıra** (son 21 gündeki ilan görüntülemeleri, en fazla 200
olay) birlikte işleniyor. Bu iki sıra, misafirin "kim olduğu" (uzun vadeli zevk/tercih örüntüsü)
ile "şu an ne aradığı" (kısa vadeli, güncel niyet) bilgisini aynı anda modele veriyor. Ayrı olarak
doğrudan fetch edilen Airbnb yardım merkezi sayfasına göre (help/article/4097) Airbnb, arama
sonuçlarını "kalite, popülerlik, fiyat ve konum" gibi faktörlere göre sıralarken AI kullandığını,
ana sayfada "ilgili arama filtrelerini ve profil ilgi alanlarını" öne çıkardığını ve ilan
sayfasında "çıkarsanan tercihlere göre" olanak/özellik sıralamasını değiştirdiğini açıkça
belirtiyor; aynı sayfa kullanıcıya Hesap Ayarları > Gizlilik > "Help improve AI-powered features"
anahtarını kapatarak bu kişiselleştirmeyi (kısmen) devre dışı bırakma seçeneği sunuyor, bu kontrol
iOS, Android ve web'de aynı şekilde geçerli.

**Nerede görülür:** İkisi de; ana sayfadaki ilan/deneyim önerileri, arama sonuçlarının sıralanma
biçimi ve ilan detay sayfasındaki olanak sıralaması.

**UX gerekçesi:** Bir marketplace'te (Airbnb'nin kendi ifadesiyle bir "iki-taraflı pazar yeri")
kişiselleştirmenin temel vaadi, milyonlarca ilan arasından "bu kullanıcıya en alakalı" olanı öne
çekip arama sürtünmesini azaltmak. Uzun-vadeli ve kısa-vadeli sinyali ayrı ayrı modellemek önemli
bir tasarım kararı: sadece uzun-vadeli geçmişe (ör. "bu kullanıcı hep şehir merkezinde kalıyor")
bakmak güncel, o anki farklı bir niyeti (ör. "bu sefer sahilde bir yer arıyor") kaçırabilir; sadece
kısa-vadeli davranışa bakmak ise gürültülü/tek seferlik bir tıklamayı aşırı yorumlayabilir. İki
sırayı birlikte kullanmak, "bu kişi genelde ne sever" ile "şu an ne arıyor" arasında bir denge
kuruyor. Kullanıcıya AI kişiselleştirmesini kapatma seçeneği sunulması ise (help/article/4097),
kişiselleştirmenin bazı kullanıcılar için "yardımcı" değil "izleniyormuş" hissi yaratabileceğinin
zımni bir kabulü; bu kontrolün var olması, kişiselleştirmenin şeffaf ve geri döndürülebilir bir
tercih olarak sunulmasını sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir kişiselleştirme sistemi kurarken kullanıcı
geçmişini tek bir "genel profil" olarak değil, "uzun vadeli eğilim" ve "kısa vadeli/güncel niyet"
diye iki ayrı sinyal olarak modellemek, hem "beni tanıyor" hem "şu anki isteğimi anlıyor" hissini
aynı anda vermeyi kolaylaştırıyor. Ayrıca kişiselleştirmenin nerede/nasıl çalıştığını (hangi
sinyaller kullanılıyor) kullanıcıya açık bir ayarlar sayfasında anlatmak ve bunu kapatabilme
seçeneği sunmak, kişiselleştirmeyi "şeffaf bir hizmet" olarak konumlandırıyor; bu hem güven
inşa ediyor hem de (Airbnb'nin AB kullanıcıları için yaptığı gibi) düzenleyici şeffaflık
gerekliliklerine karşılık gelebiliyor.

**Kaynak / güven notu:** Güçlü doğrulandı, iki ayrı doğrudan fetch edilen kaynaktan. Transformer
tabanlı sıra modelinin mimarisi (uzun/kısa vadeli sıra, 7 yıl/80 olay ve 21 gün/200 olay üst
sınırları), doğrudan fetch edilen
https://medium.com/airbnb-engineering/personalizing-airbnb-search-by-learning-from-the-guest-journey-bcefd1915624
(Airbnb Engineering'in resmi Medium yayını, Daochen Zha, 21 Temmuz 2026) yazısından birebir
alındı; bu yazı çok yeni (bu araştırmanın yapıldığı tarihe göre sadece birkaç gün önce
yayınlanmış), dolayısıyla canlı üründe fiilen devrede olma ihtimali yüksek ama ekran kaydıyla
ayrıca doğrulanmadı. AI kişiselleştirmenin "kalite/popülerlik/fiyat/konum" faktörleri, ana
sayfa/ilan sayfası etkisi ve kapatma anahtarının varlığı, doğrudan fetch edilen
https://www.airbnb.com/help/article/4097 (Airbnb'nin kendi yardım merkezi sayfası) içeriğinden
birebir alındı. Bu maddenin ölçüm sonuçları (madde 9'da detaylandırılan %3,78 offline iyileşme,
+%0,55 rezervasyon, +%0,90 görüntüleme gibi rakamlar) da aynı doğrudan fetch edilen Medium
yazısından geliyor.

---

## 3. Embedding tabanlı "benzer ilanlar" carousel'i ve arama içi gerçek zamanlı kişiselleştirme

**Ne olduğu:** Doğrudan fetch edilen "Listing Embeddings in Search Ranking" yazısına göre Airbnb,
her ilanı 32 boyutlu bir vektör (embedding) olarak temsil ediyor; bu vektörler arama oturumu
verisinden öğreniliyor ve konum, fiyat, ilan tipi, mimari/stil gibi özellikleri kodluyor. İki somut
kullanım alanı var: (1) **"Benzer İlanlar" carousel'i**: bir ilanın vektörüne, aynı pazarda ve
benzer giriş/çıkış tarihlerine sahip diğer ilanlar arasından kosinüs benzerliğiyle en yakın
komşular (k-NN) bulunuyor; (2) **arama içi gerçek zamanlı kişiselleştirme**: arama sıralama
modeline iki yeni sinyal ekleniyor, **EmbClickSim** (adayın, kullanıcının son 2 hafta içinde
tıkladığı ilanlara embedding benzerliği) ve **EmbSkipSim** (adayın, kullanıcının yüksek sırada
görüp atladığı/tıklamadığı ilanlara benzerliği); bu iki sinyal, kullanıcının tıkladığı ilanlara
benzer ilanları yukarı, atladığı ilanlara benzer ilanları aşağı çekiyor.

**Nerede görülür:** İkisi de; en belirgin biçimde ilan detay sayfasındaki "Benzer İlanlar"
carousel'inde, ayrıca kullanıcı fark etmeden arama sonuçlarının sıralanma biçiminde.

**UX gerekçesi:** Doğrudan fetch edilen kaynağa göre "Benzer İlanlar" carousel'i, CTR'de (tıklama
oranı) %21 artış sağladı ve misafirlerin %4,9 daha fazlası nihai rezervasyonlarını bu carousel
üzerinden keşfetti; bu, "kullanıcı bir ilana baktıysa ama rezervasyon yapmadıysa, ona görsel/
konum/fiyat açısından yakın alternatifler göstermek" fikrinin somut bir iş değeri ürettiğini
gösteriyor - kullanıcı ilk gördüğü ilanı beğenmese bile arama sonuçlarına geri dönmek zorunda
kalmıyor. EmbClickSim/EmbSkipSim sinyalleri ise daha örtük bir kişiselleştirme: kullanıcı hiçbir
açık tercih belirtmeden (filtre seçmeden), sadece tıklama/atlama davranışıyla "bu tarz bir yeri
seviyorum/sevmiyorum" sinyali veriyor ve arama sonuçları bu sinyale göre gerçek zamanlı
(oturum içinde) yeniden şekilleniyor; bu, kullanıcının "arama sonuçları beni giderek daha iyi
anlıyor" hissini, hiçbir ek etkileşim gerektirmeden üretiyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir ürün kataloğunda (emlak, e-ticaret, iş ilanı)
"bu içeriğe benzer diğerleri" carousel'i kurarken, sadece kategori/etiket eşleşmesine değil,
öğrenilmiş bir embedding uzayındaki yakınlığa dayanmak (konum, fiyat, stil gibi çok boyutlu
benzerliği tek bir vektörde birleştirerek), daha ince taneli ve kullanıcının gerçekte neyi
"benzer" bulduğuna daha yakın öneriler üretebiliyor. Ayrıca kullanıcının negatif sinyalini de
(gördüğü ama tıklamadığı içerik) pozitif sinyal kadar ciddiye almak (EmbSkipSim mantığı),
"bu kullanıcı bunu istemiyor" bilgisini kaybetmemeyi sağlıyor; çoğu basit öneri sistemi sadece
tıklama/satın alma gibi pozitif sinyalleri kullanır, atlanan içeriği görmezden gelir.

**Kaynak / güven notu:** Güçlü doğrulandı. Embedding'lerin 32 boyutlu vektör tanımı, arama oturumu
verisinden öğrenilmesi, k-NN/kosinüs benzerliği ile "Benzer İlanlar" üretimi, %21 CTR artışı,
%4,9 ek rezervasyon-keşif rakamı, EmbClickSim/EmbSkipSim sinyallerinin tanımı ve son-2-hafta
tıklama penceresi, doğrudan fetch edilen
https://medium.com/airbnb-engineering/listing-embeddings-for-similar-listing-recommendations-and-real-time-personalization-in-search-601172f7603e
(Airbnb Engineering'in resmi Medium yayını, Mihajlo Grbovic, 13 Mart 2018) yazısından birebir
alındı. Bu, 2018 tarihli bir mühendislik anlatısı; bu spesifik embedding mimarisinin (32 boyut,
Word2Vec benzeri oturum-tabanlı öğrenme) 2026'daki canlı üründe hâlâ birebir aynı şekilde
çalıştığı ayrıca doğrulanmadı, madde 2'deki 2026 tarihli transformer modeli muhtemelen bu
sistemin üzerine inşa edilmiş veya onu kısmen ikame etmiş olabilir, ama bu iki sistemin (2018
embedding'leri ve 2026 transformer'ı) bugün tam olarak nasıl bir arada çalıştığı bu araştırmada
netleştirilemedi.

---

## 4. Wishlist (kalp) sinyalinin bir öneri motoruna beslenmesi

**Ne olduğu:** Doğrudan fetch edilen Airbnb yardım merkezi "recommendation systems" (öneri
sistemleri) sayfasına göre (help/article/4083), bir misafirin bir ilanı kalp/wishlist ile
kaydetmesi, sadece kişisel bir "sonra bakarım" listesi oluşturmuyor; aynı zamanda Airbnb'nin
resmi olarak tanımladığı öneri sistemlerine (ana sayfa, arama, önerilen destinasyonlar, review
sıralaması) giren sinyallerden biri haline geliyor. Sayfa, öneri sistemlerinin dikkate aldığı
15'ten fazla faktörü listeliyor; bunlar arasında kullanıcı bazlı faktörler (konum, dil ayarları,
gezinme geçmişi, geçmiş aramalar, rezervasyonlar, konaklama süresi, misafir sayısı, ne kadar
önceden rezervasyon yapıldığı), karşılaştırmalı faktörler (benzer kullanıcı profilleri,
mevsimsellik) ve ilan bazlı faktörler (kalite metrikleri, puanlar, review'lar, fiyat, popülerlik/
etkileşim seviyesi, müsaitlik, kullanıcıya yakınlık) yer alıyor. Sayfa, bu faktörlerin her birine
verilen ağırlığın değişebileceğini ama kullanıcıya faktör ağırlıklarını değiştirme veya öneri
sistemini tamamen kapatma seçeneği sunmadığını (madde 2'deki genel "AI kişiselleştirmesini kapat"
anahtarından ayrı olarak) belirtiyor.

**Nerede görülür:** İkisi de; wishlist'e eklenen bir ilanın kendisi, benzer stildeki başka
ilanların ana sayfada/aramada öne çıkması, ayrıca (topluluk gözlemine göre, ayrıca doğrulanmadı)
wishlist'lenen ilanların host tarafında "daha fazla etkileşim alan ilan" olarak arama sıralamasında
dolaylı bir avantaj kazanması.

**UX gerekçesi:** Kalp/wishlist ikonu Airbnb'de çok düşük sürtünmeli, tek dokunuşluk bir eylem
(bkz. `02-listing-card-browse.md`); bu düşük-sürtünme, kullanıcının "ilgi" sinyalini bir arama
filtresi doldurmaktan çok daha sık ve gönülsüzce vermesini sağlıyor. Bu sinyali sadece kişisel bir
liste olarak saklamak yerine öneri motoruna geri beslemek (madde 4083'teki resmi açıklamaya göre),
kullanıcının hiçbir ek çaba göstermeden ("bu tarz yerleri daha çok göster" diye bir ayar
aramadan) zaman içinde daha alakalı bir ana sayfa/arama deneyimi almasını sağlıyor. Kullanıcıya
bu faktörlerin ağırlığını değiştirme imkanı sunulmaması ise bir tasarım kısıtı olarak
görülebilir: şeffaflık (hangi faktörlerin kullanıldığını bilme) ile kontrol (bu faktörleri
ayarlayabilme) arasında Airbnb burada sadece ilkini, kısmen ikincisini (madde 2'deki genel
AI-kapatma anahtarı) sunuyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir "kaydet/favorile" eylemini sadece kullanıcıya
dönük bir liste olarak değil, aynı zamanda bir örtük tercih sinyali olarak ele almak (arama/ana
sayfa sıralamasına geri beslemek), kullanıcıdan ek bir kişiselleştirme eylemi istemeden
alakalılığı artırabiliyor. Ancak bu tür bir "arka planda çalışan" kişiselleştirme sistemi
kurulurken, hangi sinyallerin kullanıldığını (Airbnb'nin help/article/4083'te yaptığı gibi) açıkça
listelemek ve mümkünse kullanıcıya bunu kapatma/etkisini azaltma seçeneği sunmak, sistemin "gizli
gözetim" değil "beyan edilmiş bir hizmet" olarak algılanmasını sağlıyor.

**Kaynak / güven notu:** Güçlü doğrulandı. Öneri sistemlerinin resmi tanımı ("seçim, gösterim ve
sıralamayı etkileyen otomatik algoritma"), hangi yüzeylerde çalıştığı (ana sayfa, arama, önerilen
destinasyon, review sıralaması) ve listelenen 15+ faktör (gezinme geçmişi, geçmiş arama/
rezervasyon, benzer kullanıcı profilleri, ilan kalitesi/puan/review/fiyat/popülerlik/müsaitlik/
yakınlık, mevsimsellik, dil/konum), doğrudan fetch edilen https://www.airbnb.com/help/article/4083
(Airbnb'nin kendi yardım merkezi sayfası, muhtemelen AB Dijital Hizmetler Yasası Madde 27 şeffaflık
gerekliliğine karşılık geliyor) sayfasından birebir alındı. Ancak "wishlist'lenen ilanların host
tarafında arama sıralamasında dolaylı avantaj kazandığı" iddiası bu sayfada açıkça yazmıyor, bu
araştırmacının "popülerlik/etkileşim seviyesi" faktöründen yaptığı bir çıkarım, **doğrudan ifade
edilmedi** → bu spesifik alt-iddia doğrulanmadı.

---

## 5. "Aramana devam et" / son görüntülenen ilanların yeniden yüzeye çıkması

**Ne olduğu:** Genel e-ticaret/seyahat ürünlerinde yaygın olan bir örüntüye paralel olarak, bir
kullanıcının yarım bıraktığı bir arama (belirli tarih/konum/misafir sayısıyla yapılmış ama
rezervasyona dönüşmemiş bir arama) veya son görüntülediği ilanlar, kullanıcı siteye/uygulamaya
geri döndüğünde ana sayfada "aramana devam et" tarzı bir bölümde yeniden yüzeye çıkarılıyor. Bu
araştırmada, Airbnb'nin bu spesifik UI kalıbını (ayrı, adlandırılmış bir "Continue your search"
bölümü) bugünkü canlı üründe birebir bu şekilde sunduğuna dair doğrudan bir birincil kaynak
**bulunamadı**; ancak madde 4'te doğrulanan resmi öneri sistemleri sayfası (help/article/4083),
"gezinme geçmişi" ve "geçmiş aramalar"ın açıkça listelenen kişiselleştirme faktörleri arasında
olduğunu doğruluyor, bu da böyle bir yeniden-yüzeye-çıkarma mekanizmasının teknik olarak
mümkün/muhtemel olduğunu destekliyor, ama spesifik UI'ın (bölüm başlığı, konumu, tetikleyici
koşulları) kendisini doğrudan doğrulamıyor.

**Nerede görülür:** İkisi de (varsayım); tipik olarak ana sayfanın üst kısmında, kullanıcının en
son bıraktığı arama parametreleriyle (tarih, konum, misafir sayısı) önceden doldurulmuş bir kart
veya carousel olarak.

**UX gerekçesi:** Bir konaklama rezervasyonu genelde tek oturumda tamamlanmayan, günler/haftalar
süren bir karar sürecidir (birden fazla ilan karşılaştırma, aile/arkadaşlarla görüşme gibi ara
adımlar içerir); bu nedenle kullanıcının bir aramayı "kaybetmesi" (tarih/filtre kombinasyonunu
yeniden girmek zorunda kalması) yüksek bir sürtünme kaynağı. Yarım kalan bir aramayı otomatik
olarak hatırlayıp yeniden sunmak, kullanıcının "kaldığı yerden devam etme" maliyetini sıfıra
indiriyor; bu, uzun karar döngülü her üründe (emlak, iş ilanı, büyük bilet e-ticaret) tekrar
gözlemlenen, genel bir kişiselleştirme prensibi.

**Airbnb dışı bir uygulamaya uyarlama notu:** Karar döngüsü uzun olan (tek oturumda tamamlanmayan)
her üründe, kullanıcının yarım bıraktığı bir arama/filtre kombinasyonunu oturumlar arası hatırlayıp
geri döndüğünde göze çarpan bir yerde (genelde ana sayfanın en üstü) yeniden sunmak, düşük
maliyetli ama yüksek etkili bir kişiselleştirme yatırımı. Bu özellikle mobilde önemli: kullanıcı
mobilde bir aramayı yarıda bırakıp uygulamayı kapatma eğilimi masaüstünden daha yüksek, bu yüzden
"nereden kaldığını hatırlama" mobilde orantısız bir değer üretiyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu maddenin altında yatan genel
prensip (gezinme geçmişi ve geçmiş aramaların kişiselleştirme faktörü olması) madde 4'teki
doğrudan fetch edilen help/article/4083 sayfasıyla destekleniyor, ama "aramana devam et" adlı,
ayrı ve adlandırılmış bir UI bileşeninin Airbnb'nin canlı ürününde bugün tam olarak bu şekilde var
olduğu, bu araştırmada ne ekran görüntüsüyle ne birincil bir Airbnb kaynağıyla doğrulanabildi;
bu madde büyük ölçüde bu araştırmacının genel e-ticaret/seyahat ürünü gözleminden ve Airbnb'nin
resmi olarak doğruladığı "geçmiş arama" sinyalinden yapılan bir çıkarımdır, Airbnb'ye özgü
doğrudan bir kaynakla teyit edilmedi.

---

## 6. AI destekli seyahat planlama ve "her şeyi kapsayan uygulama": 2025-2026 Summer Release

**Ne olduğu:** WebSearch özetlerine göre (çoğu doğrudan fetch edilemedi, ilgili news.airbnb.com
sayfaları 403 Forbidden ile engellendi) Airbnb, 2025 Summer Release ile "Airbnb Services" (kişisel
şef, fotoğrafçı, masaj gibi hizmetler) ve yeniden tasarlanmış "Airbnb Experiences"i tanıttı; uygulama
konumunuza ve kiminle seyahat ettiğinize göre tamamlayıcı hizmet/deneyim önerileri sunuyor, yeni bir
"Trips" sekmesi tüm rezervasyonların görsel bir itinerary'sini (gezi planı) gösteriyor. 2026 Summer
Release ile (yine WebSearch özetinden, doğrudan fetch edilemedi) her ilan için review'ları
sentezleyip "kullanıcıların önemsediği şeyleri" öne çıkaran bir AI özet katmanı, konum/olanaklar
hakkında bağlam veren AI-üretimi ilan özetleri, grup mesajlaşma yerine geçmesi hedeflenen
kolektif bir itinerary oluşturucu ve genişletilmiş bir destek asistanı tanıtıldığı iddia ediliyor.
Ayrı olarak WebSearch'te "For You" adlı, kullanıcının seyahat planlama aşamasına (ör. hâlâ ilham
arıyor mu, yoksa belirli bir tarihe mi kilitlendi) göre uyarlanan kişiselleştirilmiş bir sekme/
akıştan bahsediliyor, ama bu sayfanın kendisi (news.airbnb.com/for-you-now-provides...) 403 ile
engellendiği için doğrudan okunamadı.

**Nerede görülür:** İkisi de (iddiaya göre); en çok mobil uygulamada vurgulanıyor gibi görünüyor
(Trips sekmesi, itinerary, AI özet kartları), ama web'de de karşılığı olduğu iddia ediliyor.

**UX gerekçesi (temkinli):** Eğer bu özellikler iddia edildiği gibi çalışıyorsa, altında yatan
mantık şu: Airbnb'nin ürünü artık sadece "bir yerde kalacak yer bulma" değil, "bir seyahati uçtan
uca planlama" olarak konumlandırılıyor; bu durumda kişiselleştirme de tek bir aramadan (nerede
kalayım) çok daha geniş bir bağlama (kimlerle, ne yapmak için, hangi hizmetlerle) yayılıyor. Review
sentezleme gibi bir AI katmanı, kullanıcının yüzlerce review'u tek tek okuma yükünü azaltıp
"bu kullanıcının önemsediği şeye" (ör. sessizlik, mutfak ekipmanı, aile dostu olması) odaklanmış
bir özet sunmayı hedefliyor gibi görünüyor; bu, kişiselleştirmenin "hangi ilanı göstereyim"
sorusundan "bu ilan hakkında hangi bilgiyi öne çıkarayım" sorusuna genişlemesi anlamına geliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir ürün "tek işlevli" (ör. sadece konaklama arama)
olmaktan "uçtan uca bir deneyimi planlama aracı" olmaya genişlerken, kişiselleştirmenin de aynı
oranda genişlemesi gerekiyor: artık sadece "hangi öğeyi göstereyim" değil "bu öğe hakkında hangi
bilgiyi, hangi sırayla göstereyim" sorusu da kişiselleştirme kapsamına giriyor (ör. review özeti,
AI-üretimi bağlam metni). Ancak bu maddenin en kritik dersi içerik değil metodolojik: burada
anlatılan hemen hemen her şey resmi kaynaktan doğrudan doğrulanamadı; bu tür hızlı değişen "en son
AI özelliği" iddialarını bir ürüne referans olarak kullanmadan önce, mutlaka canlı üründe veya
şirketin kendi güncel resmi kaynağında ayrıca teyit etmek gerekiyor.

**Kaynak / güven notu:** **Büyük ölçüde doğrulanmadı, eğitim verisi + ikincil kaynak (WebSearch
özeti) karışımı.** Bu maddenin hiçbir alt-iddiası doğrudan fetch edilerek teyit edilemedi:
news.airbnb.com'un 2025 Summer Release, 2026 Summer Release ve "For You" sayfalarının tümü fetch
denemesinde 403 Forbidden döndürdü, sadece bu sayfaları özetleyen üçüncü taraf haber/blog
sitelerinin (Hostaway, Gadget Hacks, Travel And Tour World, RentalScaleUp gibi) WebSearch
özetleri kullanılabildi. Ayrıca WebSearch'te bu konuda dolaşan bazı iddialar (ör. "2024 trial'da
kullanıcıların %30 daha memnun, %25 daha fazla rezervasyon tamamladığı", "Nisan 2026 Kullanım
Şartları güncellemesinin her host fiyatlama kararını ve tüm review arşivini eğitim için
kullanma hakkı güvence altına aldığı" gibi çok spesifik/iddialı rakamlar) düşük kaliteli, adı
belirsiz kaynaklardan (ör. bir kripto para haber sitesi) geldi ve **bu araştırmada hiçbir şekilde
doğrulanamadı, hatta kaynak güvenilirliği şüpheli**; bu rakamlar bu dokümana kasıtlı olarak
alınmadı, sadece varlığı burada şeffaflık için not ediliyor. Bu madde bütünüyle en düşük güven
seviyesindeki maddelerden biri olarak işaretleniyor.

---

## 7. Misafir tarafı fiyat kişiselleştirme mesajlaşması: "bu tarihler daha uygun olabilir"

**Ne olduğu:** Topluluk gözlemine ve genel ürün bilgisine göre (bu maddenin misafir-tarafı UI
metni doğrudan bir birincil kaynakla teyit edilemedi) Airbnb'nin arama/takvim arayüzü, seçilen
tarihlerin o bölge/ilan için nispeten pahalı olduğu durumlarda kullanıcıya yakın alternatif
tarihlerin daha ucuz olabileceğini ima eden bir mesaj/rozet gösterebiliyor (ör. "esnek tarihler"
aramasına yönlendirme veya belirli günlerin vurgulanması). Bunun host tarafındaki aynadaki
karşılığı doğrudan doğrulandı: host'un kendi takvim/fiyatlandırma arayüzünde, WebSearch
özetlerine göre (bu spesifik sayfa doğrudan fetch edilmedi) kırmızı bir nokta göstergesi "bu
tarihler için talep düşük veya fiyatınız benzer ilanlara göre yüksek" anlamına geliyor ve host'u
fiyat düşürmeye yönlendiriyor; bu, madde 10'da ele alınan Smart Pricing/Price Tips'in görsel bir
uzantısı.

**Nerede görülür:** İkisi de (varsayım, misafir tarafı UI metni için doğrudan teyit yok); host
tarafındaki takvim renk kodlaması (kırmızı nokta) ise daha güçlü bir gözlem temelinde.

**UX gerekçesi:** Fiyat, konaklama kararında en somut karşılaştırma ekseni; kullanıcıya "aynı yer,
farklı bir tarihte daha ucuz olabilir" bilgisini proaktif olarak vermek, kullanıcının kendi
başına tarih tarih deneme yapma yükünü azaltıyor ve aynı zamanda Airbnb'nin/host'un talebi
düşük dönemlere (doluluk oranını artıracak şekilde) doğru yönlendirmesine yardımcı oluyor; bu,
hem kullanıcı faydası (daha ucuz fiyat) hem platform/host faydasının (doluluk optimizasyonu)
aynı UI mesajında birleştiği bir örnek. Host tarafındaki kırmızı nokta göstergesinin mantığı da
simetrik: host'a "piyasa senin fiyatını kabul etmiyor" bilgisini soyut bir rapor yerine takvimin
üzerinde, tarih-spesifik bir görsel işaretle vermek, aksiyona geçme eşiğini düşürüyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Fiyatın zamana göre değiştiği her üründe (uçuş,
otel, etkinlik bileti), kullanıcıya sadece seçtiği tarihin fiyatını göstermek yerine, "yakın
tarihlerde daha ucuz" gibi karşılaştırmalı bir sinyali (ayrı bir arama yapmasına gerek kalmadan)
proaktif olarak sunmak, fiyat-duyarlı kullanıcılar için karar sürtünmesini azaltıyor. Host/satıcı
tarafında ise fiyat-talep uyumsuzluğunu soyut bir dashboard raporu yerine takvim üzerinde
tarih-spesifik bir görsel işaretle göstermek, aksiyon alma olasılığını artırıyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + ikincil kaynak özetinden.** Misafir
tarafında "bu tarihler daha ucuz" tarzı bir mesajın tam ifadesi ve görünüm koşulları bu araştırmada
hiçbir kaynakla (ne birincil ne WebSearch özeti) net biçimde teyit edilemedi; bu, araştırmacının
genel ürün bilgisinden gelen, doğrulanmamış bir gözlem. Host tarafındaki kırmızı nokta göstergesi
WebSearch özetinde (rentalrecon.com gibi üçüncü taraf bir host rehberi sitesinden) geçti ama bu
sayfa da doğrudan fetch edilip birebir doğrulanmadı. Price Tips ve Smart Pricing'in genel
mekanizması (madde 10) ise doğrudan fetch edilen Airbnb yardım merkezi sayfalarıyla güçlü
doğrulandı, bu maddenin en sağlam kısmı o kesişim.

---

## 8. "Guest Favorite" rozeti: kalite sinyalinin kişiselleştirilmiş biçimde yüzeye çıkarılması

**Ne olduğu:** Doğrudan fetch edilen Airbnb yardım merkezi sayfasına göre (help/article/3495)
"Guest Favorite" (Misafir Favorisi), her gün yeniden hesaplanan bir kalite rozeti: bir ilanın son
4 yılda en az 5 review'a (bunlardan en az 1'i son 2 yılda) sahip olması, genel yıldız puanı, alt
kategori puanları (check-in, temizlik, doğruluk, iletişim, konum, değer), host'un iptal oranı ve
kalite kaynaklı olay geçmişi gibi faktörlere göre değerlendiriliyor. En üst performans gösteren
evler altın kupa ikonu, altın rozet, ilan sayfasının en üstünde bir öne çıkarma ve review'ların
üzerinde bir etiketle gösteriliyor; sayfa yüzdelik dilim eşiklerinden (top %1, %5, %10) bahsediyor.
Sayfa ayrıca misafirlerin aramada "sadece Guest Favorite'leri göster" diye filtreleyebildiğini
belirtiyor, ama bu rozetin genel arama sıralama algoritmasını (filtre dışında, varsayılan
sıralamada) tam olarak nasıl etkilediğini açıkça anlatmıyor.

**Nerede görülür:** İkisi de; ilan kartlarında rozet, ilan detay sayfasında öne çıkarma/etiket,
arama filtrelerinde "Guest Favorite" seçeneği.

**UX gerekçesi:** Bu rozet, madde 4'teki genel öneri sistemi mantığıyla (ilan kalitesi, puan,
review bir faktör) doğrudan kesişen, ama onu kullanıcıya görünür, tek bir rozet olarak
"paketleyen" bir tasarım kararı. Ham veriye (yıldız puanı, review sayısı gibi çok sayıda ayrı
metrik) bakıp karar vermek kullanıcı için bilişsel yük yaratır; bunu tek bir rozete (kolayca
tanınabilen altın kupa) sıkıştırmak, "bu ilan güvenilir mi" sorusuna hızlı bir kısayol cevabı
sunuyor. Rozetin her gün yeniden hesaplanması (statik bir "bir kez kazanılan" ödül değil), kalite
düşerse rozetin kaybedilebileceği anlamına geliyor; bu, host'ları sürekli yüksek standardı korumaya
teşvik eden bir mekanizma. Kullanıcının bunu bir filtre olarak da kullanabilmesi (sadece Guest
Favorite'leri göster), rozeti pasif bir bilgi etiketinden aktif bir arama daraltma aracına
dönüştürüyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Çok sayıda ayrı kalite metriğini (puan, review
sayısı, iptal oranı, yanıt hızı gibi) kullanıcıya ham veri olarak sunmak yerine, bunları
şeffaf, açıklanmış kriterlerle tek bir rozete/etikete sıkıştırmak, kullanıcının karar verme
hızını artırıyor. Bu rozetin statik değil periyodik olarak yeniden hesaplanması (Airbnb'de günlük),
rozetin bir "bir kez kazanılıp sonsuza dek taşınan" ayrıcalık değil, sürekli hak edilmesi gereken
bir statü olduğunu hem kullanıcıya hem satıcıya/host'a iletiyor.

**Kaynak / güven notu:** Güçlü doğrulandı, kısmen. Rozetin kazanma kriterleri (min 5 review/4 yıl,
1 review/2 yıl, alt kategori puanları, iptal oranı, kalite olayları), günlük yeniden hesaplama,
yüzdelik dilim eşikleri (top %1/%5/%10) ve rozetin görsel sunumu (altın kupa/rozet/öne çıkarma/
etiket) ile "sadece Guest Favorite göster" filtresinin varlığı, doğrudan fetch edilen
https://www.airbnb.com/help/article/3495 (Airbnb'nin kendi yardım merkezi sayfası) içeriğinden
birebir alındı. Ancak bu rozetin **varsayılan (filtre uygulanmamış) arama sıralamasını ne kadar
etkilediği bu sayfada açıkça belirtilmiyor**; WebSearch özetlerinde üçüncü taraf host-optimizasyon
sitelerinde (igms.com, rankbreeze.com, hostex.io gibi) geçen "top %9", "son 10 review'un rolling
window'u" gibi ek iddialar ile "100'den fazla sinyal" gibi genel algoritma iddiaları **doğrudan
fetch edilip doğrulanmadı**, bu siteler Airbnb'nin resmi kaynağı değil, host danışmanlığı yapan
ticari bloglar; bu yüzden bu spesifik alt-detaylar ayrı işaretlendi.

---

## 9. Geçmiş seyahate dayalı yeniden-etkileşim: kişiselleştirilmiş e-posta/bildirim

**Ne olduğu:** Doğrudan fetch edilen Airbnb Engineering yazısına göre, madde 2'de anlatılan
transformer tabanlı sıra modeli sadece arama sıralamasına değil, **promosyon e-postalarına** da
uygulandı ve bu genelleme "email tıklama oranında +%5,04" gibi istatistiksel olarak anlamlı bir
iyileşme üretti. Bu, aynı kişiselleştirme modelinin (bir kullanıcının 7 yıllık rezervasyon/review
geçmişi + son 21 günlük gezinme davranışı) hem "şu an aktif olarak arama yapan" kullanıcıya hem
de "pasif, bir e-posta ile yeniden davet edilen" kullanıcıya aynı temel sinyalle hizmet ettiğini
gösteriyor; yani kişiselleştirme, ürünün tek bir yüzeyine (arama sonucu sayfası) hapsolmuyor,
kullanıcının Airbnb ile temas ettiği her kanala (arama, e-posta, muhtemelen push bildirimi de)
yayılan tek bir model üzerinden çalışıyor.

**Nerede görülür:** İkisi de; e-posta kanalı platformdan bağımsız olduğu için hem web hem mobil
kullanıcıyı aynı şekilde etkiliyor, ama e-postanın kendisi genelde mobilde (bildirim/gelen kutusu
üzerinden) tüketiliyor.

**UX gerekçesi:** Bir kullanıcı aktif olarak Airbnb'yi kullanmıyorken (arama yapmıyorken) onu
geri kazanmanın klasik yolu, ya genel/kitlesel bir promosyon e-postası (herkese aynı içerik) ya
da basit kural-tabanlı bir hatırlatma ("son baktığın ilan hâlâ müsait") göndermek. Aynı
kişiselleştirme modelini burada da kullanmak, e-postanın içeriğini (hangi ilanlar/destinasyonlar
öne çıkarılıyor) kullanıcının 7 yıllık geçmişine göre uyarlamayı mümkün kılıyor; bu, "genel bir
reklam" ile "bana özel hazırlanmış bir öneri" arasındaki farkı e-posta kanalına da taşıyor. %5,04
gibi somut bir tıklama artışı, bu kişiselleştirmenin sadece teorik değil, ölçülebilir bir iş
değeri ürettiğini gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir kişiselleştirme/öneri modeli geliştirildiğinde,
bunu sadece "aktif oturum" yüzeyine (arama sonucu, ana sayfa) hapsetmek yerine, kullanıcıyla
temas edilen pasif kanallara (e-posta, push bildirimi, re-engagement kampanyaları) da aynı
modeli uygulamak, mühendislik yatırımının getirisini çoğaltıyor; aynı sinyal setinin (geçmiş
davranış + güncel niyet) birden fazla kanalda yeniden kullanılabilir olması, her kanal için
ayrı bir kişiselleştirme sistemi kurma maliyetinden kaçınmayı sağlıyor.

**Kaynak / güven notu:** Güçlü doğrulandı. Aynı transformer modelinin promosyon e-postalarına
genellenmesi ve +%5,04 email tıklama iyileştirmesi rakamı, doğrudan fetch edilen
https://medium.com/airbnb-engineering/personalizing-airbnb-search-by-learning-from-the-guest-journey-bcefd1915624
yazısından birebir alındı (aynı kaynak madde 2'de de kullanıldı). Ancak bu maddenin "push
bildirimine de uygulandığı" kısmı **bu araştırmacının bir çıkarımı**, yazı özellikle "email"
kelimesini kullanıyor, push bildirimi ayrıca adı geçmiyor → bu alt-iddia doğrulanmadı.

---

## 10. Host tarafı öneri motoru: Smart Pricing ve Price Tips

**Ne olduğu:** Doğrudan fetch edilen iki ayrı Airbnb yardım merkezi sayfasına göre host'lara iki
farklı ama ilişkili fiyat-kişiselleştirme aracı sunuluyor. **Smart Pricing** (help/article/1168):
host'un ilanının ve bölgesinin "yüzlerce faktörünü" analiz edip nightly fiyatı talebe göre otomatik
olarak yukarı/aşağı ayarlayan bir sistem; host bir min/max fiyat aralığı belirliyor, sistem bu
aralık içinde çalışıyor, host istediği zaman belirli tarihler için özel fiyat girerek Smart
Pricing'i o tarihlerde geçersiz kılabiliyor. **Price Tips** (help/article/474): Smart Pricing'den
ayrı, host'un ilanının konumu/olanakları, kendi geçmiş rezervasyonları ve bölgedeki güncel
fiyatlara dayanan kişiselleştirilmiş fiyat önerileri; bu öneriler Smart Pricing zaten açıksa, yeterli
veri yoksa, fiyat zaten önerilen aralıktaysa veya tüm gecelere indirim uygulanıyorsa gösterilmiyor.

**Nerede görülür:** Sadece host tarafı, ama hem web host paneli hem host mobil uygulamasında
(Airbnb'nin host araçlarını her iki platformda da paralel tuttuğu genel örüntüyle tutarlı, bkz.
diğer bölümlerdeki host-tarafı pattern'ler).

**UX gerekçesi:** Bir host için "doğru fiyatı bulma" sorunu, genelde host'un kendi bölgesindeki
rakip ilanları manuel olarak takip etmesini gerektiren, sürekli bir emek. Smart Pricing bunu
tamamen otomatikleştirip host'un günlük manuel takibini ortadan kaldırıyor, ama host'a min/max
sınır tanıyarak "tamamen kontrolü kaybetme" korkusunu (ör. fiyatın çok düşmesi) hafifletiyor; bu,
otomasyonla kontrol arasında bir denge kurma örneği. Price Tips ise farklı bir tasarım felsefesi:
host'a fiyatı otomatik değiştirmiyor, sadece bir öneri gösteriyor ve son kararı host'a bırakıyor;
bu iki araç birlikte, "tam otomasyon isteyen host" ile "kontrolü elinde tutup sadece veri/öneri
isteyen host" arasındaki farklı ihtiyaç profillerine hizmet ediyor. Price Tips'in bazı koşullarda
(Smart Pricing zaten açıksa, yeterli veri yoksa) gösterilmemesi de dikkat çekici bir tasarım
kararı: sistem, çelişkili veya gürültülü bir öneri sunmak yerine hiç göstermemeyi tercih ediyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Fiyat/değer belirleme kararı satıcıya bırakılan her
iki-taraflı pazar yerinde (emlak, ikinci el eşya, hizmet ilanları), tek bir "otomatik fiyatlama"
aracı sunmak yerine, biri tam otomasyon (sistem karar veriyor, kullanıcı sadece sınır koyuyor)
diğeri yarı-otomasyon (sistem öneriyor, kullanıcı karar veriyor) olan iki ayrı aracı yan yana
sunmak, farklı risk toleransına sahip satıcı segmentlerine aynı anda hizmet edebiliyor. Öneri
motorunun, yeterli/güvenilir veri olmadığı durumlarda susmayı tercih etmesi (susma > kötü öneri),
genel olarak devredilebilir bir prensip.

**Kaynak / güven notu:** Güçlü doğrulandı. Smart Pricing'in "yüzlerce faktör" ile talebe göre
otomatik ayar yapması, min/max aralık mantığı, özel fiyatla geçersiz kılınabilmesi, hafta sonu
fiyatlandırması ve profesyonel host araçlarıyla çakışması, doğrudan fetch edilen
https://www.airbnb.com/help/article/1168 sayfasından birebir alındı. Price Tips'in tanımı (konum/
olanak/geçmiş rezervasyon/bölge fiyatına dayalı kişiselleştirilmiş öneri) ve gösterilmeme koşulları
(Smart Pricing açıkken, yeterli veri yokken, fiyat zaten aralıktayken, blanket indirim varken),
doğrudan fetch edilen https://www.airbnb.com/help/article/474 sayfasından birebir alındı. Her iki
sayfa da Airbnb'nin kendi resmi yardım merkezi içeriği, bu maddenin en güvenilir maddelerden biri
olmasını sağlıyor. "Yüzlerce faktör"ün tam listesi (hangi spesifik değişkenler) sayfada
detaylandırılmıyor, bu yüzden Smart Pricing'in iç modelinin tam mekanizması bir kara kutu olarak
kalıyor.

---

## 11. Mevsimsel/özel gün temalı küratörlü koleksiyonlar

**Ne olduğu:** Airbnb, dönemsel olarak (tatil sezonları, yılbaşı, özel anma günleri gibi)
Kategoriler mekanizmasının (madde 1) editoryal bir uzantısı olarak veya ayrı basın/pazarlama
içerikleri şeklinde küratörlü koleksiyonlar yayınlıyor gibi görünüyor; WebSearch'te bu araştırmada
görülen örnekler arasında "Instagram'da en beğenilen evler" (2023 yıl-sonu roundup'ı) ve "ABD'de
en çok wishlist'lenen ilanlar" gibi Airbnb newsroom içerikleri var. Ancak bu koleksiyonların ürün
içinde (ana sayfada, gerçek bir kullanıcı-yüzü kişiselleştirme yüzeyi olarak) nasıl sunulduğu,
kaç kullanıcıya, hangi tetikleyici koşullarla (mevsim, konum, geçmiş wishlist içeriği) gösterildiği
bu araştırmada netleştirilemedi; bulunan kaynaklar büyük ölçüde pazarlama/basın amaçlı roundup
içerikleri, üründeki gerçek bir kişiselleştirme mekanizmasının belgesi değil.

**Nerede görülür:** Belirsiz; büyük ölçüde web tabanlı newsroom/blog içeriği olarak gözlemlendi,
bunun ürün içi (ana sayfa/arama) bir karşılığı olup olmadığı bu araştırmada ayrıştırılamadı.

**UX gerekçesi (temkinli):** Eğer bu tür koleksiyonlar gerçekten ürün içinde (sadece pazarlama
içeriği olarak değil) sunuluyorsa, altında yatan mantık muhtemelen şu: yıl içindeki belirli
dönemlerde (yılbaşı, yaz tatili, sevgililer günü gibi) kullanıcıların arama niyeti kolektif olarak
değişiyor (ör. yılbaşında "kar" veya "aile için büyük ev" araması artıyor); bu dönemsel niyet
kaymasını önceden tahmin edip bir koleksiyon olarak paketlemek, bireysel kişiselleştirmenin
(madde 2-4) yakalayamayacağı "herkes için aynı anda geçerli olan mevsimsel bağlam"ı yakalıyor.
Ama bu tamamen bir çıkarım, Airbnb'nin kendi ürün ekibinin bu koleksiyonları nasıl konumlandırdığına
dair doğrudan bir açıklama bulunamadı.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bireysel davranış tabanlı kişiselleştirme
(kullanıcının kendi geçmişi) ile kolektif/mevsimsel kişiselleştirme (bu dönemde herkesin ilgisinin
kaydığı yön) birbirini tamamlayan iki farklı sinyal kaynağı; sadece bireysel geçmişe dayanan bir
sistem, "bu hafta herkes yılbaşı planı arıyor" gibi dönemsel bir kaymayı kaçırabilir. Ancak bu
maddenin zayıf kaynağı göz önüne alındığında, asıl devredilebilir ders şu: bir "mevsimsel
koleksiyon" özelliği kopyalanmadan önce onun gerçekten bir ürün/kişiselleştirme yüzeyi mi yoksa
sadece bir pazarlama/basın içeriği mi olduğu netleştirilmeli; ikisini birbirine karıştırmak
yanlış bir referans noktasına yol açar.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + zayıf ikincil kaynak özetinden.** Bu
maddede referans alınan tek somut buluntular ("Most liked homes on Airbnb's Instagram from 2023",
"Unveiling the most wishlisted listings across the US") news.airbnb.com üzerindeki gerçek başlıklar
olarak WebSearch'te göründü, ama bu sayfaların hiçbiri doğrudan fetch edilip okunmadı (haber/basın
sayfaları genelde bu araştırmada 403 ile engellendi) ve her ikisi de bir ürün/kişiselleştirme
özelliğinden çok yıl-sonu pazarlama roundup'ı gibi görünüyor. Bu, bu bölümdeki **en zayıf kaynaklı
madde**; dahil edilme nedeni sadece görevin kapsamındaki "mevsimsel/özel gün küratörlü
koleksiyonlar" maddesini tamamen atlamamak, ama okuyucu bu maddeyi diğerlerinden çok daha düşük
güvenle okumalı.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip birincil/güçlü içerik olarak doğrulanan kaynaklar:** Airbnb'nin kendi
  yardım merkezinden "recommendation systems" açıklama sayfası (help/article/4083), AI
  kişiselleştirme + kapatma anahtarı sayfası (help/article/4097), Smart Pricing (help/article/1168),
  Price Tips/nightly fiyatlandırma (help/article/474), Guest Favorites (help/article/3495); Airbnb
  Engineering'in Medium yayınından "Personalizing Airbnb search by learning from the guest journey"
  (2026) ve "Listing Embeddings in Search Ranking" (2018); Airbnb'nin 2022 Summer Release basın
  bülteninin hospitalitynet.org üzerinden okunan tam metni. Bu 8 kaynak, 11 pattern'den 6'sını
  (madde 2, 3, 4, 8, 9, 10) güçlü/doğrudan; 1'ini (madde 1) kısmen destekliyor.
- **Fetch denemesi yapılıp 403 Forbidden ile engellenen kaynaklar:** news.airbnb.com'un 2022, 2025
  ve 2026 Summer Release sayfaları, "For You" sekmesi duyurusu, Skift'in Kategoriler-kaldırma
  haberi. Bu, Airbnb'nin kendi newsroom alan adının bu araştırma boyunca sistematik olarak
  engellendiği anlamına geliyor; madde 1 (Kategoriler'in 2025'te kaldırılması) ve madde 6 (2025-2026
  AI özellikleri) bu yüzden yalnızca WebSearch özetleriyle sınırlı kaldı.
- **Hiçbir birincil kaynakla doğrulanamayan, tamamen eğitim verisi veya düşük kaliteli ikincil
  kaynak özetine dayanan maddeler:** "aramana devam et" UI'ı (madde 5), misafir tarafı "bu
  tarihler daha ucuz" mesajı (madde 7), mevsimsel/özel gün koleksiyonları (madde 11); madde 6'daki
  bazı alt-iddialar (özellikle rakamsal olanlar) düşük güvenilirlikli/şüpheli kaynaklardan geldiği
  için özellikle işaretlendi ve dokümana dahil edilmedi.
- Toplamda 11 pattern'den **6 tanesi** doğrudan Airbnb birincil kaynağıyla güçlü doğrulandı,
  **2 tanesi** (madde 1, 7) kısmen doğrulandı, **3 tanesi** (madde 5, 6, 11) büyük ölçüde
  doğrulanmadı/eğitim verisinden. Bu bölüm, projenin diğer bölümlerine kıyasla ortalama bir kaynak
  kalitesine sahip: bir yandan Airbnb'nin resmi öneri-sistemi şeffaflık sayfaları (muhtemelen AB
  düzenlemesi baskısıyla yayınlanmış) ve güncel mühendislik blog yazıları beklenmedik derecede
  zengin bir birincil kaynak seti sağladı, öte yandan tam olarak görevin en çok vurguladığı riskli
  alan (Kategoriler'in kaderi, en yeni AI özellikleri) neredeyse tamamen erişilemeyen newsroom
  sayfaları ve doğrulanamayan WebSearch özetlerine dayanmak zorunda kaldı. Bu, dokümanın en başında
  verilen uyarıyı somutlaştırıyor: kişiselleştirme/öneri alanı, Airbnb'nin sık ve sessizce
  değiştirdiği, "şu an tam olarak böyle" diye kesin konuşmanın en riskli olduğu alanlardan biri.
