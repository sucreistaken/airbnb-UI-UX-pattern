# 6. Mesajlaşma & Bildirimler (Messaging & Notifications UX)

Bu bölüm Airbnb'de host ve misafirin, bir rezervasyon var olmadan önce başlayabilen bir soru-cevap
akışından, rezervasyon süresince süren otomatik/manuel karma bir mesaj dizisine, oradan da
platformun push/e-posta/SMS kanallarıyla kullanıcıyı geri çağırdığı bildirim katmanına kadar uzanan
iletişim yığınını kapsıyor: rezervasyon öncesi "Contact host" akışı, mesaj dizisi arayüzü (ek
gönderme, host'lar için hazır şablon/quick reply, otomatik çeviri), rezervasyona bağlı otomatik
sistem mesajları (check-in bilgisi, checkout hatırlatması), bildirim kanalı x kategori matrisi,
okundu bilgisi ve yazıyor göstergesinin (typing indicator) durumu, gelen kutusu görünümü/filtreleri,
host'lara yönelik 24 saatlik yanıt süresi baskısı ve son olarak Airbnb'nin **yapmadığı** bir şey:
wishlist/fiyat düşüşü bildirimi. Diğer bölümlerdeki gibi burada da "şu an tam olarak böyle" değil,
tekrar tekrar belgelenen yerleşik pattern'ler anlatılıyor; mesajlaşma özellikle sık güncellenen bir
alan (AI quick reply önerileri, çeviri motoru gibi özellikler 2022-2024 arası eklendi), bu yüzden
detaylar zamanla değişebilir.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: Airbnb'nin kendi yardım merkezi
sayfalarından mesajlaşma genel akışı (help/article/145), misafirin host'u ne zaman mesajlayabileceği
(help/article/124), host yanıt vermezse ne yapılacağı (help/article/88), host'un rezervasyon talebine
yanıt süresi (help/article/75), bildirim kanalları/kategorileri (help/article/14), gelen kutusu
yönetimi ve read receipts (help/article/3558), fotoğraf/video ek gönderme (help/article/3759) ve
zamanlanmış quick reply/otomatik mesaj kurulumu (help/article/2897); ayrıca Airbnb'nin kendi
mühendislik blogundan (Medium, Airbnb Tech Blog) "Messaging Sync" makalesi ve "Promotions and
Communications Platform (OMNI)" makalesi doğrudan fetch edilip okundu. Host quick replies'in tam
kullanıcı akışını anlatan Airbnb Resource Center sayfaları (resources/hosting-homes/a/using-quick-
replies...) ve inbox read-receipts sayfası (resources/hosting-homes/a/inbox-read-receipts-586)
fetch denemesinde 403 ile engellendi, bu ikisi için sadece WebSearch özeti kullanılabildi.
news.airbnb.com'daki Translation Engine duyurusu da 403 ile engellendi.

---

## 1. Rezervasyon öncesi soru sorma: "Contact host" mesaj akışı

**Ne olduğu:** Misafirin, bir rezervasyon yapmadan önce host'a soru sormasını sağlayan mesajlaşma
akışı. Doğrudan fetch edilen help/article/124'e göre misafirler "booking'den önce her zaman" host'a
mesaj gönderebiliyor; bu genelde ilanla ilgili detayları netleştirmek veya özelleştirme talebi için
kullanılıyor. Instant Book olmayan ilanlarda ise rezervasyon **isteği** gönderirken misafirin "neden
seyahat ettiği ve ne zaman check-in yapacağı hakkında kısa bir mesaj" eklemesi isteniyor; bu,
tamamen serbest bir "soru sorma" değil, rezervasyon akışına gömülü yarı-zorunlu bir mesaj alanı.

**Nerede görülür:** İkisi de (ilan sayfasındaki "Contact host" linki hem web hem mobil uygulamada
aynı konumda, host bölümünün yakınında).

**UX gerekçesi:** Bir konaklama rezervasyonu, çoğu e-ticaret işleminden farklı olarak yüksek
belirsizlik taşıyor (evcil hayvan kabul ediliyor mu, geç check-in mümkün mü, otopark var mı gibi
ilanın yapılandırılmış alanlarında yer almayan sorular). Bu soruları rezervasyon **öncesinde**
sorabilme imkânı, misafirin parasını taahhüt etmeden belirsizliği azaltmasını sağlıyor; bu da hem
misafir tarafında iptal/anlaşmazlık riskini düşürüyor hem de host tarafında "yanlış beklentiyle
gelen misafir" sorununu önden filtreliyor. Instant Book olmayan ilanlarda mesaj alanının rezervasyon
isteğine gömülmesi ise farklı bir amaca hizmet ediyor: host'a "bu bir bot/spam talebi değil, gerçek
bir insan" sinyali vererek host'un 24 saatlik pencerede hızlı karar vermesini kolaylaştırıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Envanterin standart olmadığı (her ilan/hizmetin kendi
özel koşulları olduğu) her marketplace'te (kısa süreli kiralama, ikinci el eşya, freelance hizmet),
rezervasyon/satın alma öncesi bir soru-cevap kanalı sunmak, alıcının riskini taahhütten önce
azaltmasını sağlar. Onaylanmamış bir talebe kısa bir bağlam mesajı eklemeyi zorunlu/teşvik edilen
bir alan yapmak (tamamen serbest bırakmak yerine), satıcının talebi hızlı değerlendirmesini
kolaylaştırıyor ve spam/düşük niyetli taleplerin görünürlüğünü azaltıyor.

**Kaynak / güven notu:** **Doğrulandı.** Misafirin booking öncesi, sonrası ve trip sonrası
mesajlaşabildiği, Instant Book olmayan ilanlarda rezervasyon isteğine "neden seyahat ediyorsunuz,
ne zaman check-in yapacaksınız" mesajının eklenmesi gerektiği, doğrudan fetch edilen
https://www.airbnb.com/help/article/124 sayfasından birebir alındı. "Contact host" linkinin ilan
sayfasındaki tam piksel konumu ise ekran görüntüsüyle ayrıca doğrulanmadı, genel gözlemden.

---

## 2. Mesaj hız sınırlaması: 24 saatte 25 mesaj / saatte 10 mesaj

**Ne olduğu:** Airbnb, host ve misafirler arasındaki mesajlaşmaya sert bir sayısal tavan koyuyor.
Doğrudan fetch edilen help/article/145'e göre kullanıcılar "24 saatlik herhangi bir periyotta 25
mesaj veya saatte 10 mesaj" ile sınırlı. Aynı sayfaya göre gönderilen bir mesaj 15 dakika içinde
düzenlenebiliyor, 24 saate kadar geri çekilebiliyor (unsend); ayrıca kullanıcı kendi hesabından
kendine mesaj gönderemiyor.

**Nerede görülür:** İkisi de; sınırlama arka planda uygulanıyor, kullanıcıya genelde sadece limite
ulaşıldığında bir hata/uyarı olarak görünüyor (bu spesifik hata mesajının tam UI biçimi kaynaklarda
belirtilmedi).

**UX gerekçesi:** Açık uçlu bir mesajlaşma kanalı, hem spam/dolandırıcılık (bir kötü niyetli
hesabın çok sayıda kullanıcıya toplu mesaj göndermesi) hem de tacizin (bir tarafın diğerini mesaj
bombardımanına tutması) aracı olabilir. Sabit ve düşük bir sayısal tavan (saatte 10, günde 25),
normal bir host-misafir konuşmasının ihtiyaç duyacağından fazlasını zaten karşılıyor ama kötüye
kullanım senaryolarını pratik olarak imkânsızlaştırıyor; bu, "kötüye kullanımı algoritmik olarak
tespit et" gibi karmaşık bir sisteme güvenmek yerine basit bir hız sınırlamasıyla riski önden
azaltan bir tasarım kararı. 15 dakikalık düzenleme ve 24 saatlik geri çekme penceresi ise insan
hatasına (yanlış yazılan bir tarih, yanlışlıkla gönderilen bir mesaj) tolerans tanıyor, ama bu
pencereyi kısıtlı tutarak mesajlaşma geçmişinin (ör. bir anlaşmazlıkta kanıt olarak) büyük ölçüde
kalıcı kalmasını sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Yabancılar arası mesajlaşmanın taciz/spam riski
taşıdığı her platformda (marketplace, arkadaşlık uygulaması, iş ilanı sitesi), sabit ve düşük bir
saatlik/günlük mesaj tavanı koymak, karmaşık bir kötüye kullanım tespit sistemi kurmadan önce
uygulanabilecek ucuz ve etkili bir ilk savunma hattı. Mesaj düzenleme/geri çekme penceresini kısa
tutmak (Airbnb'nin 15 dakika/24 saat gibi), kullanıcı dostu esneklik ile mesajlaşma geçmişinin
anlaşmazlık çözümünde güvenilir bir kayıt olarak kalması arasında bir denge kuruyor.

**Kaynak / güven notu:** **Doğrulandı.** Mesaj limitleri (25/24 saat, 10/saat), 15 dakikalık
düzenleme penceresi, 24 saatlik geri çekme penceresi ve kendine mesaj gönderilemediği bilgisi
doğrudan fetch edilen https://www.airbnb.com/help/article/145 sayfasından birebir alındı. Limite
ulaşıldığında kullanıcıya gösterilen tam hata mesajı/UI biçimi doğrulanmadı.

---

## 3. Rezervasyon bazlı grup thread (host + misafir + co-host + co-traveler tek dizide)

**Ne olduğu:** Her rezervasyon, otomatik olarak kendi mesaj dizisini oluşturuyor ve bu dizide
"tüm rezervasyon sahipleri, birlikte seyahat edenler (co-traveler), host ve co-host'lar"
birbirine bağlanabiliyor; yani mesajlaşma kişi bazlı değil **rezervasyon bazlı** organize ediliyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Bir konaklama rezervasyonunda taraf sayısı ikiden fazla olabiliyor (birden fazla
misafir, host'un yanında bir co-host); mesajlaşmayı kişi-kişi (1:1 DM) yerine rezervasyon etrafında
gruplamak, bu çok taraflı koordinasyonu (ör. "check-in saatini kim teyit edecek" sorusuna hem
host'un hem co-host'un görebileceği tek bir yerde cevap verilmesi) tek bir yerde topluyor;
aksi halde her taraf ayrı ayrı bilgilendirilmesi gereken, senkronizasyonu zor, dağınık 1:1
konuşmalar oluşurdu. Bu tasarım kararı aynı zamanda mesaj dizisinin doğal olarak bir rezervasyona
"ait" olmasını sağlıyor, bu da hem otomatik sistem mesajlarının (check-in bilgisi gibi) doğru
rezervasyona bağlanmasını hem de anlaşmazlık durumunda "bu konuşma hangi rezervasyonla ilgili"
sorusunun hiç sorulmamasını sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Birden fazla tarafın (ana kullanıcı + yetkilendirdiği
ikincil kullanıcılar, ör. asistan/ekip üyesi) aynı işlem etrafında koordine olması gereken her
platformda (randevu/rezervasyon sistemleri, çok taraflı hizmet pazaryerleri), mesajlaşmayı kişi
kimliği yerine işlem/rezervasyon kimliği etrafında gruplamak, çok taraflı koordinasyonu basitleştirir
ve otomatik sistem mesajlarının doğru bağlama oturmasını garanti eder.

**Kaynak / güven notu:** **Doğrulandı**, ana mekanizma için: "her rezervasyon otomatik olarak tüm
bookers, co-traveler, host ve co-host'ların bağlanabildiği bir grup thread oluşturur" ifadesi
doğrudan fetch edilen https://www.airbnb.com/help/article/145 sayfasından birebir alındı. Ancak bu
projenin görev tanımında istenen "thread'in en üstünde rezervasyona dair bir bağlam kartı (context
card: tarihler, ilan fotoğrafı, konum özeti)" görseli hiçbir kaynaktan doğrudan doğrulanamadı; bu
**tamamen genel gözlem/eğitim verisinden** bir gözlem, Airbnb'nin kendi belgelediği bir UI detayı
değil → **doğrulanmadı, eğitim verisinden**.

---

## 4. Ekler: fotoğraf/video gönderme, sadece onaylanmış rezervasyondan sonra

**Ne olduğu:** Mesaj dizisi içinde dosya eki (fotoğraf, video) gönderme özelliği. Doğrudan fetch
edilen help/article/3759'a göre bu özellik **yalnızca rezervasyon onaylandıktan sonra** kullanılabiliyor
(booking öncesi mesajlaşmada eklenemiyor); videolar en fazla 60 saniye uzunluğunda olabiliyor;
gönderilen tüm medya, rezervasyonun "Details" sayfasındaki ayrı bir "Gallery" bölümünde toplu
olarak görüntülenebiliyor. Masaüstünde akış: Messages > ilgili thread > "Add +" > "Add photo or
video" > gözden geçir > gönder.

**Nerede görülür:** İkisi de; masaüstü adımları doğrudan doğrulandı, mobil adımlar kaynakta
detaylandırılmadı ama özelliğin iOS/Android'de de var olduğu belirtiliyor.

**UX gerekçesi:** Ek gönderimini rezervasyon onayından **önce** değil **sonra** açmak, iki amaca
hizmet ediyor: birincisi güvenlik, rezervasyon öncesi tanımadığı biriyle dosya alışverişini
sınırlamak dolandırıcılık/kötüye kullanım yüzeyini azaltıyor; ikincisi ürün mantığı, ek gönderiminin
gerçek kullanım senaryosu genelde check-in sonrası pratik koordinasyon (ör. host'un "kapı kodu
resmi", misafirin "ısıtıcı bozuk, işte fotoğrafı" paylaşımı), bu da zaten onaylı bir rezervasyon
bağlamında anlamlı. Tüm medyanın ayrı bir "Gallery" bölümünde toplanması, mesaj dizisinin
kronolojik akışını kirletmeden (kaydırarak eski fotoğrafları aramak zorunda kalmadan) paylaşılan
görsellere hızlı erişim sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Dosya/medya paylaşımının anlamlı olduğu her iki
taraflı işlem platformunda (kiralama, ikinci el satış, servis randevusu), bu özelliği rezervasyon/
işlem onayından önceki döneme değil sonrasına kısıtlamak, tanımadık kullanıcılar arası dosya
paylaşımı riskini azaltan ucuz bir güvenlik önlemi. Paylaşılan medyayı mesaj akışından ayrı bir
galeri görünümünde toplamak, uzun bir konuşma geçmişinde belirli bir fotoğrafı arama sürtünmesini
ortadan kaldırıyor.

**Kaynak / güven notu:** **Doğrulandı.** Ek gönderiminin sadece onaylı rezervasyon sonrası
kullanılabildiği, 60 saniyelik video sınırı, medyanın "Gallery" bölümünde toplandığı ve masaüstü
adım adım akış, doğrudan fetch edilen https://www.airbnb.com/help/article/3759 sayfasından birebir
alındı. Fotoğraf dosya boyutu sınırı belirtilmemiş; mobil uygulamadaki tam buton/ikon yerleşimi de
doğrulanmadı.

---

## 5. Host'lar için Quick Replies (hazır şablon yanıtlar) + AI destekli öneri

**Ne olduğu:** Host'ların, sık sorulan sorulara (wifi şifresi, otopark, check-in saati gibi) hızlı
yanıt verebilmesi için önceden yazdığı veya Airbnb'nin sunduğu şablonları düzenleyip kaydettiği bir
"Quick Replies" sistemi. WebSearch özetine göre bu şablonlar host'un ilan/rezervasyon bilgisinden
(misafirin adı, check-in tarihi gibi) otomatik doldurulan yer tutucular (placeholder) içerebiliyor;
Messages sekmesinde "+" ikonuna basıp "Send quick reply" seçilerek anında gönderiliyor. Aynı özet,
Messages sekmesinin yapay zeka kullanarak misafirin sorusunu anlayıp konuşma içinde (sadece host'a
görünen) bir quick reply önerisi otomatik sunduğunu belirtiyor.

**Nerede görülür:** İkisi de (host tarafı); misafir tarafında görünen sadece gönderilen mesajın
kendisi, "bu bir şablon" olduğuna dair bir etiket misafire gösterilip gösterilmediği kaynaklarda
belirtilmedi.

**UX gerekçesi:** Host'un yanıt hızı hem Superhost uygunluğunu hem arama sıralamasını etkilediği
için (bu projenin 05 numaralı bölümünde help/article/430 üzerinden doğrulandığı gibi), host'a
tekrar eden soruları tek tıkla yanıtlama imkânı vermek, düşük çabayla yüksek yanıt hızı sağlanmasını
mümkün kılıyor; bu da hem host'un günlük iş yükünü azaltıyor hem de misafirin "hızlı yanıt alma"
beklentisini karşılıyor. AI'ın soruyu anlayıp otomatik şablon önermesi, host'un doğru şablonu
manuel arayıp bulma adımını ortadan kaldırarak yanıt süresini (response time metriğini doğrudan
etkileyen bir değişkeni) daha da kısaltıyor; önerinin sadece host'a görünmesi (misafire değil),
host'a düzenleme/reddetme özgürlüğü tanıyarak yapay/robotik bir yanıtın kontrolsüz gitmesini
engelliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Yanıt hızının performans metriğine bağlı olduğu her
müşteri hizmetleri/marketplace mesajlaşma sisteminde (destek hattı, satıcı-alıcı mesajlaşması),
sık sorulan sorular için düzenlenebilir şablonlar sunmak ve bunu bir adım ileri götürüp gelen
mesajı otomatik sınıflandırıp en uygun şablonu öneren bir yapay zeka katmanı eklemek, yanıt hızını
insan çabasını artırmadan yükseltir. Önerinin gönderen tarafa özel kalması (karşı tarafa "bu bir
bot önerisiydi" görünmemesi), otomasyonun insan kontrolünü ortadan kaldırmadan hızı artırmasını
sağlıyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Quick Replies'in var olduğu, placeholder ile
kişiselleştirildiği, "+" > "Send quick reply" akışı ve AI'ın soruyu anlayıp öneri sunduğu bilgisi
WebSearch'ün Airbnb Resource Center sayfalarından (resources/hosting-homes/a/using-quick-replies-
to-save-time) ürettiği özetten geliyor; bu sayfaları doğrudan fetch etmeye çalıştım ama ikisi de
(hem -73 hem -747 sürümü) **403 Forbidden** ile engellendi, dolayısıyla birebir metin doğrudan
görülemedi → **kısmen doğrulanmadı, ikincil özet üzerinden**. Misafir tarafında bir şablonun
"bu otomatik bir yanıt" olarak etiketlenip etiketlenmediği hiçbir kaynaktan doğrulanmadı →
**doğrulanmadı, eğitim verisinden**.

---

## 6. Zamanlanmış otomatik sistem mesajları: check-in bilgisi, checkout hatırlatması, "Scheduled by" etiketi

**Ne olduğu:** Host'ların, belirli tetikleyicilere (yeni rezervasyon, check-in öncesi, checkout
öncesi) bağlı olarak otomatik gönderilecek mesajlar tanımlayabildiği bir sistem. Doğrudan fetch
edilen help/article/2897'ye göre host Messages > Settings > Quick Replies > Create yolunu izleyip
bir şablon oluşturuyor, listesini seçiyor ve "Scheduling" adımında ne zaman gönderileceğini
belirliyor. Üç tetikleyici WebSearch özetinde adlandırıldı: yeni rezervasyon sonrası teşekkür
mesajı, check-in öncesi (tripten "birkaç gün önce" yön tarifi ve ev kuralları içeren hatırlatma) ve
checkout öncesi (tripin bitişinden "sabah önce" hatırlatma). Doğrudan fetch edilen kaynağa göre
host mesaj dizisinde "her rezervasyon için gönderilen her mesajın tam bir zaman çizelgesini"
inceleyebiliyor; otomatik gönderilen mesajlar "Scheduled by" etiketiyle işaretleniyor ve host bu
mesajları gönderilmeden önce atlayabiliyor, düzenleyebiliyor veya erken gönderebiliyor.

**Nerede görülür:** İkisi de (host tarafı kurulum; misafir tarafı sadece gelen mesajı görüyor, ayrı
bir bölümde doğrudan doğrulanan kaynağa göre check-in bilgisi ayrıca check-in'den 48 saat önce
misafire otomatik olarak açılıyor, WebSearch özetine göre ayrıca tripten 3 gün önce e-posta olarak
da tekrarlanıyor).

**UX gerekçesi:** Host-misafir iletişiminin büyük bir kısmı (wifi bilgisi, check-in yönergesi,
checkout saati hatırlatması) tekrar eden ve kişiye özel olmayan bilgi aktarımı; bunu otomasyona
bağlamak host'un manuel emek harcamadan tutarlı ve zamanında bilgi vermesini sağlıyor, bu da
dolaylı olarak host'un "yanıt hızı" metriğini de destekliyor (mesaj hiç host'un müdahalesini
beklemeden zamanında gidiyor). "Scheduled by" etiketinin thread içinde ayrıca görünür olması,
misafire (ve daha sonra host'a) hangi mesajın gerçek zamanlı insan yanıtı, hangisinin önceden
programlanmış otomatik bir mesaj olduğunu şeffaf şekilde ayırt ettiriyor; bu şeffaflık,
otomasyonun "sahte kişisel iletişim" izlenimi vermesini (ve misafirin gerçek bir yanıt beklerken
boşuna beklemesini) engelliyor. Host'a mesajı son anda atlama/düzenleme hakkı tanınması ise
otomasyonun bağlamdan kopuk çalışmasını (ör. iptal edilmiş bir rezervasyona hâlâ hatırlatma
gitmesi gibi hataları) önlemek için bir güvenlik supabı.

**Airbnb dışı bir uygulamaya uyarlama notu:** Tekrar eden, kişiye özel olmayan ama zamanlaması
kritik bilgi aktarımı gerektiren her hizmet akışında (randevu hatırlatması, teslimat bilgisi,
check-in yönergesi), bu iletişimi tetikleyici bazlı (rezervasyon anı, X gün önce, X saat önce)
otomatikleştirmek tutarlılığı artırırken insan emeğini azaltır. Otomatik gönderilen her mesajı
konuşma akışında açıkça "otomatik/zamanlanmış" olarak etiketlemek, kullanıcının "bu gerçek bir
insan mı yazdı" belirsizliğini ortadan kaldırıyor ve otomasyona olan güveni artırıyor; gönderen
tarafa son anda müdahale/iptal hakkı tanımak da bağlamdan kopan otomatik mesaj hatalarını
önlüyor.

**Kaynak / güven notu:** **Doğrulandı**, ana mekanizma için: zamanlanmış quick reply kurulum akışı,
"Scheduled by" etiketi, host'un mesajı atlama/düzenleme/erken gönderme hakkı ve "her rezervasyon
için tam mesaj zaman çizelgesi" görüntüleme özelliği doğrudan fetch edilen
https://www.airbnb.com/help/article/2897 sayfasından birebir alındı. Üç tetikleyicinin (yeni
rezervasyon, check-in öncesi, checkout öncesi) tam adlandırması ve check-in bilgisinin misafire
48 saat önce otomatik açılması, 3 gün önce e-posta tekrarı gibi zamanlama detayları ise WebSearch
özetinde ikincil kaynaklardan (Hospitable blog yazıları) geldi, birincil kaynaktan birebir teyit
edilmedi → **kısmen doğrulanmadı**.

---

## 7. Otomatik çeviri motoru (Translation Engine): buton yok, mesaj kendiliğinden çevriliyor

**Ne olduğu:** Airbnb'nin, farklı dillerdeki host ve misafirlerin mesajlaşabilmesi için geliştirdiği
"Translation Engine" adlı bir makine çevirisi katmanı. WebSearch özetine göre bu sistem 2022 yazında
review'lardan mesajlaşmaya genişletildi; önceki "tıkla ve çevir" (click-to-translate) butonunu
kaldırıp, mesajları kullanıcının tercih ettiği dile **otomatik olarak** çeviriyor; kullanıcı bir
şey yapmadan, mesajı açtığında zaten kendi dilinde görüyor. Aynı özete göre platform genelinde
12 aylık dönemde 1,3 milyardan fazla mesaj gönderiliyor (günde 3,5 milyondan fazla, saniyede 40'tan
fazla), bu da çeviri sisteminin ölçeğini gösteriyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Bir "tıkla ve çevir" butonu, çeviriye ihtiyacı olan kullanıcıya ekstra bir adım
(mesajı fark et, çevrilmemiş olduğunu anla, butona bas) yüklüyor; bunu tamamen otomatikleştirmek
bu sürtünmeyi sıfıra indiriyor ve kullanıcının "acaba host beni anladı mı" belirsizliğini
azaltıyor çünkü her iki taraf da mesajı kendi dilinde, hiç ekstra adım atmadan görüyor. Bu, uluslararası
bir marketplace'te dil bariyerinin rezervasyon/host-misafir güvenini doğrudan etkileyen bir sorun
olduğunu ima ediyor; çeviriyi "isteğe bağlı bir özellik" değil "varsayılan, görünmez bir altyapı"
haline getirmek, dil farkının kullanıcı deneyiminde hissedilir bir engel olmaktan çıkmasını
sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcı tabanı birden fazla dile yayılan her
mesajlaşma ürününde (uluslararası marketplace, destek hattı, topluluk forumu), çeviriyi kullanıcının
tetiklediği isteğe bağlı bir buton yerine varsayılan ve görünmez bir katman olarak tasarlamak,
dil bariyerinin gündelik kullanımda fark edilmeyecek kadar küçülmesini sağlar. Böyle bir sistemi
kurarken kullanıcıya orijinal metni de (bir "show original" linkiyle) erişilebilir tutmak, çeviri
hatası şüphesi durumunda bir geri dönüş yolu sağlıyor (bu detay Airbnb için ayrıca doğrulanmadı).

**Kaynak / güven notu:** Kısmen doğrulandı. Translation Engine'in varlığı, "tıkla ve çevir"
butonunun kaldırılıp otomatik çeviriye geçildiği ve mesajlaşmaya 2022 yazında genişletildiği bilgisi
WebSearch özetinde ikincil bir kaynaktan (multilingual.com, sektör haber sitesi) geldi; birincil
kaynak olan https://news.airbnb.com/translation-engine-launches-reviews-after-expanding-to-messages-
this-summer/ sayfasına fetch denemesi yapıldı ama **403 Forbidden** ile engellendi → **kısmen
doğrulanmadı, ikincil kaynak**. Ölçek rakamları (1,3 milyar mesaj/yıl, saniyede 40 mesaj) da aynı
ikincil kaynaktan, ayrıca doğrulanmadı.

---

## 8. Bildirim kanalı x kategori matrisi: push / e-posta / SMS / telefon araması, 5 kategori

**Ne olduğu:** Airbnb'nin bildirim tercihleri sayfası, kullanıcıya iki boyutlu bir kontrol sunuyor:
hangi **kanaldan** (telefon araması, SMS/metin, push bildirimi, e-posta) ve hangi **kategoriden**
bildirim almak istediği. Doğrudan fetch edilen help/article/14'e göre kategoriler şöyle: Messages
(host/misafir iletişimi), Reminders and suggestions (rezervasyon güncellemeleri, seyahat ipuçları),
Promotions and tips (kupon, anket, ürün haberleri), Policy and community (yerel paylaşım
yasaları, savunuculuk) ve Account support (güvenlik, hukuki, müşteri hizmetleri). Bazı bildirimler
(rezervasyonlar, hesap aktivitesi, hukuki güncellemeler, güvenlik uyarıları, müşteri hizmetleri
talepleri) kapatılamıyor.

**Nerede görülür:** İkisi de; ayarlar hesap/profil menüsünde "Notifications" bölümünde.

**UX gerekçesi:** Bildirimleri tek bir "aç/kapa" anahtarına indirgemek yerine kanal x kategori
matrisi olarak sunmak, kullanıcının "bu bilgiyi almak istiyorum ama sadece e-posta ile, push
istemiyorum" gibi ince ayarlı tercihler yapmasına izin veriyor; bu granülerlik, bildirim
yorgunluğunun (notification fatigue) en büyük nedenlerinden biri olan "her şeyi ya tamamen aç ya
tamamen kapat" ikiliğini ortadan kaldırıyor. Belirli kategorilerin (rezervasyon, güvenlik,
hesap) kapatılamaz olması ise ürün tarafında bilinçli bir sınır: kullanıcı deneyimi kişiselleştirmesi
ile platformun operasyonel/yasal sorumluluğu (ör. bir rezervasyon iptalini bildirmemek hukuki
risk taşır) arasında net bir ayrım çiziyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Çok kanallı bildirim gönderen her üründe (e-ticaret,
SaaS, sosyal platform), tercih ekranını tek boyutlu bir liste yerine kanal x kategori matrisi
olarak tasarlamak kullanıcıya çok daha ince kontrol veriyor ve bildirim yorgunluğunu azaltıyor.
Hangi kategorilerin (güvenlik, hesap, yasal/operasyonel) kullanıcı tercihinden bağımsız olarak
her zaman gönderileceğini baştan net bir şekilde ayırmak, hem kullanıcı beklentisini yönetiyor hem
de platformun operasyonel/yasal yükümlülüklerini karşılamasını garantiliyor.

**Kaynak / güven notu:** **Doğrulandı.** Dört kanal (telefon, SMS, push, e-posta), beş kategori
(Messages, Reminders and suggestions, Promotions and tips, Policy and community, Account support)
ve kapatılamayan bildirim türleri (rezervasyon, hesap, hukuki, güvenlik, müşteri hizmetleri),
doğrudan fetch edilen https://www.airbnb.com/help/article/14 sayfasından birebir alındı. Ancak
sayfa her kategori için her kanalın **ayrı ayrı** açılıp kapatılabildiğini birebir teyit etmiyor
(ör. "Messages kategorisini sadece push'tan kapat, e-postadan açık bırak" gibi ince bir kombinasyonun
mümkün olup olmadığı net değil) → bu granülerlik iddiası **kısmen doğrulanmadı**, sayfanın genel
diliyle tutarlı ama birebir kanıtlanmadı.

---

## 9. Read receipts (okundu bilgisi): opt-out edilebilir, typing indicator yok

**Ne olduğu:** Mesaj dizisinde, gönderilen bir mesajın karşı taraf tarafından görüntülenip
görüntülenmediğini gösteren bir "okundu" sinyali (read receipt). Doğrudan fetch edilen
help/article/3558'e göre "read receipts, karşı tarafın mesajınızı görüp görmediğini gösterir,
meğer ki kendi hesap ayarlarında bu özelliği kapatmış olsunlar." Yani özellik varsayılan olarak
açık ama kullanıcı tarafından kapatılabilir (opt-out). Hiçbir kaynakta bir "yazıyor..." (typing
indicator) göstergesinden bahsedilmiyor; bunun bilinçli bir tasarım kararı mı yoksa basitçe
araştırılan kaynaklarda belirtilmemiş bir detay mı olduğu net değil.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Read receipts'in opt-out (varsayılan açık, isteyen kapatabilir) olması, çoğu
kullanıcı için "mesajım görüldü mü" belirsizliğini gidermenin faydasının, azınlık bir kullanıcı
kitlesinin mahremiyet endişesinden (ör. bir mesajı görüp kasıtlı geç yanıtlamak istemesi) daha
ağır bastığı varsayımını yansıtıyor; ama yine de bir kaçış yolu (kapatma seçeneği) bırakılması,
bu varsayımın herkese dayatılmadığını gösteriyor. Typing indicator'ın (rakip mesajlaşma
uygulamalarında, ör. iMessage/WhatsApp'ta yaygın olan bir özellik) görünmemesi ya da en azından
hiçbir resmi kaynakta belgelenmemesi, Airbnb'nin mesajlaşmasının bir "gerçek zamanlı sohbet" değil
daha çok "asenkron, rezervasyon bağlamlı iletişim" olarak tasarlandığını düşündürüyor; host-misafir
iletişiminde tarafların genelde aynı anda ekran başında olmadığı (farklı saat dilimleri, host'un
günlük hayatı) düşünülürse, anlık "yazıyor..." göstergesinin değeri zaten düşük olabilir.

**Airbnb dışı bir uygulamaya uyarlama notu:** Asenkron, genelde farklı zaman dilimlerinde çalışan
taraflar arası mesajlaşma tasarlayan her platformda (marketplace, uzaktan hizmet koordinasyonu),
read receipts'i varsayılan açık ama opt-out edilebilir yapmak çoğu kullanıcının faydasını
korurken azınlığın mahremiyet tercihine yer bırakıyor. Typing indicator gibi "gerçek zamanlı
sohbet" sinyallerini eklemeden önce, kullanım bağlamının gerçekten eşzamanlı bir sohbet mi yoksa
asenkron bir koordinasyon mu olduğunu sorgulamak gerekir; ikincisi için bu özelliğin katma değeri
düşük olabilir.

**Kaynak / güven notu:** Kısmen doğrulandı. Read receipts'in var olduğu ve kullanıcı ayarlarından
kapatılabildiği bilgisi, doğrudan fetch edilen https://www.airbnb.com/help/article/3558 sayfasından
birebir alındı ("unless they've turned off this feature in their account settings" ifadesi sayfada
geçiyor). Read receipts'i ayrıca ve daha detaylı anlatan resources/hosting-homes/a/inbox-read-
receipts-586 sayfasına fetch denemesi **403 Forbidden** ile engellendi. Typing indicator'ın
**bulunmadığı** iddiası ise "kaynaklarda hiç bahsedilmemiş olması"na dayanıyor, yani bir yokluk
ispatı değil; bu madde **doğrulanmadı, eğitim verisinden/dolaylı çıkarım** olarak işaretlenmeli.

---

## 10. Host yanıt süresi baskısı: 24 saatlik pencere, guest tarafına "sabırlı ol" mesajı

**Ne olduğu:** Airbnb, host'un bir rezervasyon **talebine** (Instant Book olmayan ilanlarda) yanıt
vermesi için 24 saatlik bir pencere tanımlıyor; bu süre dolarsa talep otomatik olarak sona eriyor
ve misafirden ücret alınmıyor. Doğrudan fetch edilen help/article/75'e göre "büyük çoğunluk 12 saat
içinde yanıt veriyor" ve "tüm taleplerin yarısından fazlası bir saat içinde kabul ediliyor" gibi
sosyal kanıt (social proof) içeren ifadelerle misafire bekleme süresi konusunda güven veriliyor.
Genel **mesajlar** için (rezervasyon talebi dışında) doğrudan fetch edilen help/article/88'e göre
resmi bir "hostların 24 saat içinde yanıt vermesi bekleniyor" çerçevesi var, ama aynı sayfa "çoğu
host birkaç saat içinde yanıt veriyor, zaman dilimi ve internet erişimi eksikliği süreci
yavaşlatabilir" diyerek gerçek davranışın daha hızlı olduğunu, 24 saatin bir "tavan" (üst sınır),
tipik davranış değil, olduğunu ima ediyor. Doğrulanan hiçbir kaynakta guest'e yönelik görünür bir
geri sayım sayacı (countdown timer) bulunmuyor.

**Nerede görülür:** İkisi de; 24 saatlik kural ve "çoğu host daha hızlı yanıtlıyor" çerçevesi
öncelikle yardım merkezi metninde ifade ediliyor, uygulama içi UI'da bunun görsel karşılığı
(ör. "Usually responds within an hour" gibi bir rozet) doğrudan doğrulanmadı.

**UX gerekçesi:** 24 saatlik resmi bir SLA (hizmet seviyesi taahhüdü) tanımlamak, misafire "sonsuza
kadar beklemeyeceğim" güvencesi veren somut bir üst sınır sağlıyor; ama bu sınırı doğrudan
söylemek yerine "çoğu host daha hızlı yanıtlıyor / yarısından fazlası bir saat içinde kabul ediyor"
gibi istatistiksel çerçeveleme eklemek, misafirin beklentisini "24 saat normaldir" değil "24 saat
en kötü senaryodur, genelde çok daha hızlı olur" yönünde kalibre ediyor; bu, aynı temel kuralı
(24 saat) hem host'a esneklik tanıyan hem misafiri endişelendirmeyen bir dille sunmanın bir yolu.
Host tarafında ise bu 24 saatlik pencere aynı zamanda (bu projenin 05 numaralı bölümünde
doğrulandığı gibi) Superhost uygunluğunu ve arama sıralamasını etkileyen bir performans metriğine
bağlı; yani host için bu sadece "nazik bir beklenti" değil, somut bir davranışsal baskı unsuru.
Görünür bir geri sayım sayacının olmaması ise muhtemelen bilinçli: bir sayaç, host'u "acil" bir
şekilde baskı altında hissettirip düşük kaliteli/aceleyle yazılmış yanıtlara yol açabilir; ifadeyi
metinsel bir çerçeve (24 saat + istatistik) olarak tutmak bu baskıyı yumuşatıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** İki taraflı bir onay/yanıt döngüsü içeren her
marketplace'te (iş başvurusu, hizmet randevusu, satıcı-alıcı mesajlaşması), resmi bir üst sınır
(ör. "24 saat içinde yanıt gerekir") tanımlamak net bir güvence sağlarken, bunu doğrudan bir "üst
sınır" olarak değil güven verici bir istatistikle ("çoğu kullanıcı çok daha hızlı yanıt veriyor")
birlikte sunmak, beklentiyi olumlu yönde kalibre ediyor. Bu üst sınırı host/satıcı tarafında bir
performans metriğine (rozet, sıralama) bağlamak davranışsal baskı yaratırken, misafir tarafında
görünür bir geri sayım sayacından kaçınmak, bu baskının host'u aceleci/düşük kaliteli yanıtlara
itmesini önlüyor.

**Kaynak / güven notu:** **Doğrulandı.** Rezervasyon talebi için 24 saatlik pencere, "büyük
çoğunluğun 12 saat içinde yanıt verdiği" ve "yarısından fazlasının bir saat içinde kabul edildiği"
istatistikleri doğrudan fetch edilen https://www.airbnb.com/help/article/75 sayfasından birebir
alındı. Genel mesajlar için "24 saat içinde yanıt beklenir" çerçevesi ve "çoğu host birkaç saat
içinde yanıt veriyor" ifadesi doğrudan fetch edilen https://www.airbnb.com/help/article/88
sayfasından birebir alındı; aynı sayfa açıkça "dokümantasyon, misafire gösterilen bir görsel
geri sayım sayacı veya performans rozetinden bahsetmiyor" şeklinde not düşüyor → sayaç/rozetin
**yokluğu** iddiası da kaynağın kendi ifadesine dayanıyor, sağlam. Uygulama içindeki "usually
responds within an hour" tarzı bir UI rozetinin varlığı/yokluğu ise ayrıca doğrulanmadı,
**doğrulanmadı, eğitim verisinden**.

---

## 11. Birleşik gelen kutusu (unified inbox) + hızlı filtreler (Hosting/Traveling/Support, Unread, Trip stage, Starred)

**Ne olduğu:** Airbnb'nin mesajlaşma arayüzü, host ve misafir hesaplarını ayrı ayrı gezmek yerine
tüm konuşmaları tek bir gelen kutusunda birleştiriyor. Doğrudan fetch edilen help/article/3558'e
göre bir "All" görünümü "tüm misafir, host ve destek mesajlarını tek bir yerde birleştiriyor",
bu da kullanıcının hosting ve traveling hesapları arasında geçiş yapma ihtiyacını ortadan
kaldırıyor. Kullanıcılar "Hosting, Traveling veya Support" hızlı filtrelerini uygulayabiliyor;
Hosting filtresi içinde ayrıca "Unread" (sadece okunmamışlar), "Trip stage" (rezervasyon talebi /
yaklaşan rezervasyon / şu an ağırlanıyor / geçmiş rezervasyon), "Listings" (birden fazla ilanı olan
host için ilana göre) ve "Starred" (yıldızlanmış mesajlar) alt filtreleri mevcut. Arama kutusu da
gerçek zamanlı olarak, uygulanan hızlı filtreleri dikkate alarak sonuç veriyor.

**Nerede görülür:** İkisi de.

**UX gerekçesi:** Airbnb'de aynı kullanıcı hem host hem misafir olabiliyor (bir ev sahibi kendi
seyahatlerinde misafir de oluyor); mesajlaşmayı hesap rolüne göre ayrı tutmak yerine tek bir gelen
kutusunda birleştirip rol bazlı filtreleme sunmak, bu çift rollü kullanıcı için "hangi hesaba
geçmem gerekiyor" sürtünmesini ortadan kaldırıyor. "Trip stage" filtresinin ayrı bir kategori
olması özellikle host'lar için önemli: birden fazla rezervasyonu aynı anda yöneten bir host için
"hangi mesajlar hâlâ karar bekleyen bir talep, hangileri zaten onaylanmış bir rezervasyonun
koordinasyonu" ayrımı, günlük iş akışının en temel önceliklendirme sorusu; bu filtre olmadan
host'un talepleri (24 saatlik pencereye tabi, aciliyeti yüksek) diğer daha az acil mesajlar
arasında kaybolma riski taşırdı.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcıların birden fazla rolde (alıcı + satıcı,
misafir + ev sahibi) bulunabildiği her iki taraflı platformda, mesajlaşmayı role göre ayrı
uygulamalara/sekmelere bölmek yerine birleşik bir gelen kutusunda toplayıp üstüne rol bazlı filtre
eklemek, rol geçişindeki sürtünmeyi azaltır. Zaman baskısı olan bir alt kategoriyi (ör. "yanıt
bekleyen talepler") ayrı bir filtre olarak öne çıkarmak, kullanıcının en acil işleri diğer daha az
zaman-kritik mesajlar arasında kaybetmesini önlüyor.

**Kaynak / güven notu:** **Doğrulandı.** "All" birleşik görünüm, Hosting/Traveling/Support hızlı
filtreleri, Hosting altındaki Unread/Trip stage/Listings/Starred alt filtreleri ve gerçek zamanlı
arama (filtreleri dikkate alarak) doğrudan fetch edilen https://www.airbnb.com/help/article/3558
sayfasından birebir alındı. Trip stage filtresinin tam adlandırdığı dört alt durumun (reservation
requests, upcoming reservations, currently hosting, past reservations) ekran görüntüsüyle tam
görsel yerleşimi ayrıca doğrulanmadı.

---

## 12. Wishlist/fiyat düşüşü bildirimi: Airbnb'nin native olarak SUNMADIĞI bir özellik

**Ne olduğu:** Bu maddenin geri kalanından farklı olarak burada "Airbnb'nin yaptığı" değil,
**yapmadığı görünen** bir şey belgeleniyor: kullanıcının wishlist'ine eklediği bir ilanın fiyatı
düştüğünde veya müsaitlik durumu değiştiğinde otomatik bir push/e-posta bildirimi. Bu proje
kapsamında yapılan araştırmada Airbnb'nin kendi yardım merkezinde böyle bir özelliğe dair hiçbir
madde bulunamadı; bunun yerine bu boşluğu dolduran bağımsız üçüncü taraf servisler (Alertstays,
StayWatch, Visualping gibi) ortaya çıkmış durumda, bunlar kullanıcının belirli bir ilanı veya
arama kriterini takip edip fiyat/müsaitlik değişince harici bir bildirim gönderiyor.

**Nerede görülür:** Airbnb'nin kendisinde yok (araştırmada bulunamadı); üçüncü taraf web/mobil
araçlarında var.

**UX gerekçesi:** Bunun neden native olarak sunulmadığına dair Airbnb'nin kendi bir açıklaması
bulunamadı, ama olası bir yorum şu: Airbnb'nin iş modeli e-ticaretteki gibi "aynı ürünü daha ucuza
bekleme" davranışını teşvik etmekle doğrudan uyumlu olmayabilir, çünkü konaklama fiyatları büyük
ölçüde talebe (occupancy) bağlı dinamik fiyatlandırmayla belirleniyor ve host'un fiyatı düşürmesi
genelde "doluluk düşük" sinyali taşıyor; bunu bir "fırsat" olarak öne çıkarıp misafiri bekletmek,
host'un gelirini optimize etmeye çalışan platformun kendi teşvik yapısıyla gerilebilir. Bu boşluğun
üçüncü taraf araçlarca doldurulmuş olması, kullanıcı tarafında gerçek bir talep olduğunu gösteriyor,
ama Airbnb'nin bunu bilinçli olarak native yapmamayı seçtiği mi yoksa henüz önceliklendirmediği mi
net değil; bu bölüm bu belirsizliği açıkça bırakıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Dinamik fiyatlandırma kullanan bir marketplace
tasarlarken, "favori/wishlist" özelliğini fiyat takip bildirimiyle birleştirip birleştirmemek
bilinçli bir ürün kararı olmalı: e-ticarette (statik envanter, indirim kültürü) bu özellik dönüşümü
artırabilirken, arz tarafının (host/satıcı) fiyat düşüşünü "kötü sinyal" olarak okuyabileceği bir
marketplace'te bu özellik platform-taraf teşvikleriyle çelişebilir. Bu boşluğu üçüncü taraf
araçların doldurması, kullanıcı talebinin var olduğunun dolaylı bir kanıtı; bu tür bir sinyali
görmezden gelmek yerine en azından bilinçli bir "bunu yapmıyoruz çünkü X" kararı olarak
belgelemek daha sağlıklı.

**Kaynak / güven notu:** Bu madde **doğrulanmadı, eğitim verisinden** olarak değil, tam tersi bir
şekilde ele alınmalı: WebSearch ile özellikle bu özelliğin Airbnb'de var olup olmadığı arandı ve
Airbnb'nin kendi yardım merkezinde/resource center'ında **hiçbir doğrulayıcı kaynak bulunamadı**;
bulunan tek şey üçüncü taraf araçların (Alertstays, StayWatch, Visualping, PageCrawl) varlığıydı.
Yani bu "özelliğin yokluğu" iddiası, negatif bir sonucun (arama sonucunda bulunamama) temkinli bir
yorumu; Airbnb'nin bu özelliği hiç sunmadığının kesin bir kanıtı değil, sadece bu araştırmada
görünür/belgelenmiş bir native özellik bulunamadığı anlamına geliyor.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip birincil/güçlü içerik olarak doğrulanan kaynaklar (Airbnb'nin kendi
  yardım merkezi sayfaları):** mesajlaşma genel akışı ve limitleri (help/article/145), booking
  öncesi/sonrası mesajlaşma (help/article/124), host yanıt vermezse ne yapılacağı (help/article/88),
  rezervasyon talebi yanıt süresi (help/article/75), bildirim kanalı/kategori matrisi
  (help/article/14), gelen kutusu yönetimi/filtreler/read receipts (help/article/3558), fotoğraf/
  video ek gönderme (help/article/3759) ve zamanlanmış otomatik mesajlar (help/article/2897). Bu 8
  kaynak, 12 pattern'den 6'sını (madde 1, 2, 4, 8 [kısmen], 10, 11) doğrudan ve güçlü şekilde
  destekliyor.
- **Doğrudan fetch edilip birincil/güçlü mühendislik içeriği olarak okunan kaynaklar:** Airbnb Tech
  Blog'daki "Messaging Sync" makalesi (mobil senkronizasyon mimarisi, +200%/+96% inbox ziyaret artışı
  gibi somut rakamlar) ve "Promotions and Communications Platform (OMNI)" makalesi (kanal
  yönlendirme, teslimat gecikmesi <30 saniye); bu ikisi ana pattern maddelerine doğrudan
  madde olarak girmedi ama arka plan/mimari bağlam için kullanıldı ve gerçekten fetch edilip
  okundu.
- **Fetch denemesi yapılıp erişilemeyen kaynaklar (403 Forbidden):** Airbnb Resource Center'daki
  Quick Replies sayfaları (resources/hosting-homes/a/using-quick-replies-to-save-time-73 ve -747),
  inbox read-receipts sayfası (resources/hosting-homes/a/inbox-read-receipts-586) ve
  news.airbnb.com'daki Translation Engine duyurusu. Bu üçü için sadece WebSearch özeti
  kullanılabildi, madde 5, 7 ve 9'un bir kısmı bu yüzden "kısmen doğrulanmadı" olarak işaretli.
- **Hiçbir birincil kaynakla doğrulanamayan, büyük ölçüde genel gözlem/çıkarımdan yazılan
  maddeler:** thread'in üstündeki rezervasyon bağlam kartının tam görseli (madde 3'ün bir
  parçası), typing indicator'ın yokluğu (madde 9'un bir parçası, negatif kanıt) ve wishlist fiyat
  düşüşü bildiriminin native olarak sunulmaması (madde 12, yine negatif kanıt).
- Toplamda 12 pattern'den **6 tanesi** doğrudan Airbnb birincil kaynağıyla güçlü doğrulandı,
  **5 tanesi** kısmen doğrulandı (kaynak fetch edildi ama ikincil özet, kısmi içerik veya 403
  engeliyle sınırlı kaldı), **1 tanesi** (wishlist fiyat bildirimi) doğası gereği bir "yokluk"
  iddiası olduğu için normal "doğrulandı/doğrulanmadı" ölçeğine tam oturmuyor, ayrı olarak
  belirtildi. Mesajlaşma alanı özellikle hızlı değişen bir alan olduğundan (AI destekli quick
  reply önerisi, otomatik çeviri motoru gibi özellikler son birkaç yılda eklendi), bu doküman da
  diğerleri gibi "şu an birebir ekran görüntüsü" değil "yerleşik/tekrar gözlemlenen pattern"
  çerçevesiyle okunmalı.
