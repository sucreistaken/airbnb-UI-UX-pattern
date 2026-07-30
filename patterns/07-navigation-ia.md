# 7. Navigasyon & Bilgi Mimarisi (Navigation & Information Architecture)

Bu bölüm Airbnb'nin kullanıcıyı platform içinde nasıl gezdirdiğini kapsıyor: web'deki üst
header (logo, arama pili, "Airbnb your home" host CTA'sı, globe/dil-para birimi seçici,
kullanıcı menüsü), mobildeki alt sekme çubuğu (Explore/Wishlists/Trips/Inbox/Profile),
misafir modu ile host modu arasında geçiş paterni ("switch to hosting"), arama çubuğunun
hamburger menü yerine platformun ana navigasyon unsuru olması, mobilde filtre/tarih
seçici/misafir sayacının yeni bir ekran yerine bottom sheet olarak açılması, mobil geri
navigasyon/gesture paternleri, breadcrumb'sız navigasyon felsefesi, bildirim/e-postadan
uygulama içi spesifik bir ekrana derin bağlantı (deep link), sekme çubuğundaki bildirim
noktası/badge kullanımı ve ana header'ın altında ikinci bir katman olarak duran kategori
sekmeleri. Diğer bölümlerdeki gibi burada da "şu an tam olarak böyle" değil, tekrar tekrar
gözlemlenen/belgelenen yerleşik pattern'ler anlatılıyor; Airbnb sürekli A/B test yapan canlı
bir ürün olduğu için (ör. arama deneyimi ve kategori sekmeleri yıllar içinde birkaç kez
köklü şekilde değişti) detaylar zamanla farklılaşabilir.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: Airbnb'nin kendi yardım merkezi
sayfalarından misafir/host modu geçişi (help/article/3546) ve wishlist kaydetme/gezinme yolu
(help/article/338), bildirim tipleri ve kanalları (help/article/14), Airbnb'nin kendi açık
kaynak Android kütüphanesi DeepLinkDispatch'in GitHub README'si, Airbnb Design ekibinin
Medium'daki Design Language System (DLS) yazısı ve NN/g'nin genel bottom sheet rehberi.
Mobil alt sekme çubuğunun tam güncel isim seti, "Airbnb your home" başlığının header'daki
tam konumu, globe ikonunun tam davranışı ve kategori sekmelerinin tam ikon seti gibi bazı
detaylar sadece WebSearch özetlerinden (ikincil kaynak, doğrudan fetch edilip birebir
doğrulanmadı) geliyor; bunlar ilgili maddelerde açıkça "kısmen doğrulanmadı" veya
"doğrulanmadı, eğitim verisinden" olarak işaretlendi.

---

## 1. Arama-öncelikli bilgi mimarisi: hamburger menü yerine arama pili ana navigasyon unsuru

**Ne olduğu:** Airbnb'nin hem web'de hem mobilde ana giriş ekranının en belirgin, en büyük
etkileşim alanı üç çizgili bir hamburger menü değil, kendisi büyük bir pil/kapsül şeklindeki
arama çubuğı ("Nereye gidelim?" / "Start your search"). WebSearch özetlerine göre (bir UX
teardown makalesinden, doğrudan fetch edilerek birebir doğrulanmadı) Airbnb hamburger menüyü
yıllar önce kaldırmış ve "bir daha geri dönmemiş"; bunun yerine tüm birincil eylemler
(arama, kategori gezintisi, filtre) doğrudan görünür yüzeyler olarak sunuluyor. Ayrıca
fiilen fetch edilen bir mobil arama yeniden tasarımı vaka çalışmasına göre tasarım ekibi
ana sayfayı "asıl işlevi olan aramaya" odaklanacak şekilde sadeleştirmiş ve arama akışını
"destinasyon, kategori, tarihler" gibi tek seferde bir soruya odaklanan adımlara bölmüş.

**Nerede görülür:** İkisi de. Web'de header'ın ortasında/altında yatay bir pil; mobilde ana
sekmenin (Explore) en üstünde, ekranın büyük bir kısmını kaplayan bir arama kartı/pil olarak.

**UX gerekçesi:** Bir hamburger menü, navigasyon seçeneklerini görünmez kılıp keşfedilebilirliği
düşürür (kullanıcı üç çizgiye tıklamadan neyin arkasında ne olduğunu bilemez); oysa Airbnb'nin
temel kullanıcı niyeti neredeyse her oturumda aynı: "bir yer bul ve rezerve et". Ana eylemi
gizli bir menünün arkasına değil, ekranın en göze çarpan yerine, kendi başına büyük bir
bileşen olarak koymak, "IA'nın kendisini arama etrafında kurma" felsefesini yansıtıyor: alt
sekmeler (Wishlists, Trips, Inbox, Profile) ikincil, arama ise birincil ve varsayılan durum.
Bu, klasik "hamburger'de her şeyi eşit ölçüde göm" yaklaşımının tam tersi; bir işlevi
(arama) diğerlerinden kasıtlı olarak çok daha büyük ve erişilebilir yaparak IA'da açık bir
hiyerarşi kuruyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcı niyetinin oturumların büyük
çoğunluğunda tek ve öngörülebilir olduğu her uygulamada (seyahat, e-ticaret arama motoru,
iş ilanı platformu), o tek niyeti hamburger menünün arkasına gömülü bir seçenek değil,
ana ekranın en büyük, en görünür bileşeni yapmak, kullanıcının "ilk ne yapacağım" sorusunu
tasarım kararıyla baştan cevaplıyor. Arama akışını "tek soru, tek adım" (destinasyon,
sonra tarih, sonra kişi sayısı) şeklinde bölmek, uzun bir formu doldurma hissini azaltıyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Mobil arama yeniden tasarımı vaka çalışmasından
("ana sayfanın asıl işlevi olan aramaya odaklanması", "tek seferde bir soru: destinasyon,
kategori, tarihler" ifadeleri) doğrudan fetch edilen
https://medium.com/@fangyuandong/how-i-redesign-airbnbs-in-app-searching-experience-8be1c7bf3e92
sayfasından **doğrulandı**, ama bu makale resmi bir Airbnb kaynağı değil, bağımsız bir
tasarımcının kendi yeniden tasarım denemesi/analizi. "Airbnb hamburger menüyü kaldırdı ve
bir daha dönmedi" iddiası ise WebSearch özetinde ikincil bir Medium makalesinden
("The End of the Hamburger Menu") geldi, bu makale ayrıca fetch edilip birebir okunmadı →
**kısmen doğrulanmadı, ikincil kaynak**. Arama pilinin web'de tam piksel/konum yerleşimi
genel gözlemden, ekran görüntüsüyle ayrıca doğrulanmadı.

---

## 2. Web header: "Airbnb your home" host CTA butonu

**Ne olduğu:** Web header'ının sağ tarafında, kullanıcı menüsünden ayrı, kendi başına duran
bir metin bağlantısı/buton: "Airbnb your home" (bazı bölgelerde "Become a host" olarak da
geçtiği WebSearch özetlerinde görülüyor). Bu, misafir olarak gezinen bir kullanıcıyı,
henüz host olmamışsa host olmaya, zaten host'sa kendi ilan yönetim paneline yönlendiren
sabit bir giriş noktası.

**Nerede görülür:** Ağırlıklı olarak web (header'da her zaman görünür bir öğe olarak);
mobilde bu eylem WebSearch özetlerine göre ayrı bir header CTA'sı değil, profil sekmesi
içindeki "Hosting" girişi/hold-to-switch etkileşimi üzerinden sunuluyor (bkz. madde 6).

**UX gerekçesi:** Airbnb'nin iş modeli iki taraflı bir pazaryeri (misafir + host) olduğu
için, "host olma" eylemi tesadüfi bir alt sayfa değil, platformun her sayfasında (header
kalıcı olduğu için) sürekli görünür bir büyüme kanalı. Bunu bağımsız bir buton olarak
(kullanıcı menüsünün içine gömmeden) header'a koymak, host olmayı "gizli bir seçenek" değil
"herkesin görebileceği bir davet" haline getiriyor; bu, platformun arz tarafını (host
sayısını) büyütmeye verdiği stratejik önceliği IA seviyesinde somutlaştırıyor. Bu buton
misafir modundayken de görünmeye devam ediyor, yani "sen zaten misafirsin, host olmayı
düşünmen gerekmez" gibi bir kapı kapatma yok; bu iki rolün platformda birbirini
dışlamadığını, aynı hesabın hem misafir hem host olabileceğini IA seviyesinde ima ediyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Talep (misafir/alıcı) ve arz (host/satıcı)
taraflarını aynı platformda barındıran her iki taraflı marketplace'te (ikinci el satış,
hizmet pazaryeri, kısa dönem kiralama), arz tarafına geçiş davetini kullanıcı menüsünün
içine gömülü bir seçenek olarak değil, header'da her zaman görünen ayrı bir CTA olarak
tutmak, arz büyümesini pasif değil aktif bir kanal haline getiriyor. Bu CTA'nın hem
misafir/alıcı modundayken hem de zaten satıcı olan kullanıcılar için görünmeye devam etmesi
(bağlama göre metni "sat/host ol" ile "ilanlarını yönet" arasında değiştirerek), tek bir
header öğesinin iki farklı kullanıcı durumuna hizmet etmesini sağlıyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** "Airbnb your home" ifadesinin
header'da bağımsız bir CTA olarak var olduğu ve host olma davetini temsil ettiği bilgisi
WebSearch özetinde bir instapage.com blog yazısından ("Airbnb your home' in its header
navigation" ifadesi) geldi; bu sayfa doğrudan fetch edilip birebir doğrulanmadı.
airbnb.com/host/homes sayfasını doğrudan fetch etme denemesi **403 Forbidden** hatasıyla
engellendi, dolayısıyla header'ın güncel tam metni bu araştırmada birincil kaynaktan teyit
edilemedi. Bölgeye/dile göre metnin "Airbnb your home" ile "Become a host" arasında
değişebileceği de sadece genel gözlem, doğrulanmadı.

---

## 3. Web header: globe ikonu (dil / para birimi seçici)

**Ne olduğu:** Header'ın sağ üst köşesinde, kullanıcı menüsünün hemen yanında duran bir
dünya/globe ikonu; tıklandığında bir dropdown/modal açılıp kullanıcının arayüz dilini ve
görüntülenen para birimini ayrı ayrı seçmesine izin veriyor.

**Nerede görülür:** İkisi de; web'de header'da sabit bir ikon, mobilde WebSearch özetlerine
göre benzer bir işlev profil/ayarlar menüsü içinde bir "Countries & regions" ya da
dil/para birimi seçim ekranı olarak sunuluyor (uygulamanın kendi ekran görüntüsüyle bu
araştırmada ayrıca doğrulanmadı).

**UX gerekçesi:** Airbnb küresel bir platform olduğu için dil ve para birimi tercihleri
kullanıcının coğrafi konumundan (IP/cihaz ayarı) otomatik tahmin edilse de, bu tahminin
her zaman doğru olmayacağı biliniyor (ör. seyahat eden, VPN kullanan ya da farklı bir
dilde arayüz tercih eden kullanıcı). Bunu ayrı, tek bir simgeyle (globe) her zaman erişilebilir
kılmak, kullanıcının "otomatik tahmin yanlışsa nasıl düzeltirim" sorusuna, ayarların derinlerine
gitmeden, header'dan tek tıkla bir cevap veriyor. Dil ve para biriminin **aynı** kontrol
noktasından yönetilmesi (iki ayrı menü yerine) bunların kullanıcı zihninde "yerelleştirme"
adlı tek bir kavramsal grup olarak ele alındığını gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Birden fazla ülkede/dilde hizmet veren her
platformda, dil ve para birimi tercihini iki ayrı ayarlar sayfasına dağıtmak yerine tek bir
görünür ikon/kontrol noktasında birleştirmek, kullanıcının yerelleştirme ile ilgili tüm
kararlarını tek bir zihinsel modelde ele almasını sağlıyor. Bu kontrolü header'da her zaman
erişilebilir tutmak (ayarların derinlerine gömmemek), otomatik coğrafi tahminin yanlış
olduğu durumlarda sürtünmesiz bir düzeltme yolu sunuyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Globe ikonunun header'da "profil fotoğrafının
yanında, sağ üstte" konumlandığı ve tıklandığında para birimi/dil seçimi sunduğu bilgisi
WebSearch özetinde houst.com ve wise.com blog yazılarından geldi; bu sayfalar doğrudan
fetch edilip birebir doğrulanmadı → **kısmen doğrulanmadı, ikincil kaynak**. Mobil
uygulamadaki karşılığının tam konumu ve adı bu araştırmada hiç doğrulanmadı, tamamen genel
gözlem/eğitim verisinden.

---

## 4. Web header: kullanıcı menüsü dropdown (avatar + genel menü kombinasyonu)

**Ne olduğu:** Header'ın en sağında, kullanıcı giriş yapmışsa profil fotoğrafı/avatarı,
yapmamışsa jenerik bir kullanıcı ikonuyla birlikte küçük bir hamburger (üç çizgi) ikonunun
**bir arada** kullanıldığı bir buton; tıklandığında Wishlists, Trips, hesap ayarları,
yardım merkezi, çıkış yap gibi bağlantıları içeren bir dropdown açılıyor. Doğrudan fetch
edilen help/article/338'e göre masaüstünde wishlist'lere erişim de bu menü üzerinden:
"Menu > Wishlists" yolu.

**Nerede görülür:** Ağırlıklı olarak web (mobilde bu işlevlerin çoğu ayrı sekmelere,
Wishlists ve Profile'a dağıtılmış durumda, bkz. madde 5).

**UX gerekçesi:** Bu, "hamburger menüyü tamamen kaldırdı" iddiasının biraz nüanslı olduğu
bir nokta: Airbnb hamburger ikonunu **ana** navigasyon aracı olarak kullanmıyor (madde 1),
ama onu ikincil, daha az sık kullanılan hesap/ayar işlevleri için (kullanıcı menüsü
içinde) hâlâ tutuyor. Bu ayrım önemli: sık kullanılan, birincil eylemler (arama, gezinme)
her zaman görünür kalırken, seyrek kullanılan işlevler (hesap ayarları, yardım, çıkış)
gizli bir menüde toplanıyor. Bu, hamburger menünün "kötü" olmadığını, sadece **yanlış
içerik** için (ör. ana navigasyon) kullanıldığında sorunlu olduğunu; seyrek kullanılan
ikincil işlevler için hâlâ makul bir sıkıştırma aracı olduğunu gösteriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir uygulamada hangi işlevlerin "her zaman
görünür" hangilerinin "gizli menüde" olacağına karar verirken, ayrım "hamburger var mı yok
mu" değil "bu işlev kullanıcının her oturumda yapacağı birincil eylem mi, yoksa seyrek
başvurduğu ikincil bir ayar/hesap işlevi mi" sorusuna dayanmalı. Birincil eylemler (arama,
gezinme, ana içerik) her zaman görünür bileşenler olarak header'da kalmalı; hesap ayarları,
yardım, çıkış gibi seyrek işlevler rahatlıkla bir dropdown/hamburger'e gömülebilir.

**Kaynak / güven notu:** Kısmen doğrulandı. "Menu > Wishlists" navigasyon yolunun masaüstünde
gerçekten bu şekilde adlandırıldığı **doğrulandı**, doğrudan fetch edilen
https://www.airbnb.com/help/article/338/how-do-i-save-a-favorite-experience-or-place-to-stay
sayfasında bu ifade birebir geçiyor ("On desktop, access wishlists by clicking Menu >
Wishlists"). Ancak bu menünün avatar ile hamburger ikonunun birlikte tek bir buton
oluşturduğu görsel biçimi, ve menü içindeki tam bağlantı listesi (hesap ayarları, yardım
merkezi, çıkış gibi) genel gözlemden, ekran görüntüsüyle ayrıca doğrulanmadı →
**kısmen doğrulanmadı**.

---

## 5. Mobil alt sekme çubuğu (bottom tab bar): Explore / Wishlists / Trips / Inbox / Profile

**Ne olduğu:** Mobil uygulamanın ekranın en altında sabit duran, beş sekmeden oluşan ana
navigasyon çubuğu: Explore (keşif/arama), Wishlists (kaydedilenler), Trips (rezervasyonlar),
Inbox (mesajlar) ve Profile (hesap). Her sekme küçük bir ikon ve altında kısa bir metin
etiketiyle birlikte gösteriliyor (sadece ikon değil, ikon+etiket kombinasyonu); aktif sekme
genelde dolu/koyu bir ikon rengiyle, pasif sekmeler daha soluk bir renkle ayrışıyor.

**Nerede görülür:** Sadece mobil (native iOS/Android uygulaması); web'de bu beş işlev
header ve ayrı sayfalar arasında dağıtılmış durumda, tek bir sabit alt çubuk yok.

**UX gerekçesi:** Beş sekmelik bir alt tab bar, mobilde iyi bilinen bir "IA'yı düzleştirme"
tekniği: kullanıcının uygulamanın en üst düzey beş bölümü arasında (her biri kendi
bağımsız navigasyon yığınına sahip) tek dokunuşla, her zaman aynı fiziksel konumdan geçiş
yapmasını sağlıyor. İkon+etiket kombinasyonunun tercih edilmesi (sadece ikon değil),
ikonların tek başına genelde belirsiz kalabildiği (ör. bir kalp ikonu "Wishlists" mi
"Favoriler" mi olduğunu her kullanıcıya aynı netlikte anlatmayabilir) bir alanda okunabilirlik
riskini azaltıyor; metin etiketi ikonun anlamını netleştiren bir güvenlik ağı işlevi
görüyor. Sekmelerin sırası da anlamlı: Explore en solda (varsayılan/giriş durumu),
Profile en sağda (en "kişisel", en az sık ziyaret edilen); Trips ve Inbox ise rezervasyon
öncesi/sonrası döngünün ortasında yer alıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Beş veya daha az üst düzey bölümü olan her
mobil uygulamada, bu bölümleri hamburger menü ardına gömmek yerine sabit bir alt tab bar'a
yerleştirmek, her bölüme erişimi tek dokunuşa indiriyor ve kullanıcının "şu an
uygulamanın neresindeyim" sorusunu her an görsel olarak cevaplıyor (aktif sekmenin
vurgulanması sayesinde). İkon+etiket kombinasyonunu (sadece ikon değil) tercih etmek,
özellikle marka-özgü veya soyut ikonlarda (genel bir "ev" veya "kalp" ikonundan daha
belirsiz olabilecek özel ikonlarda) yanlış anlaşılma riskini azaltır.

**Kaynak / güven notu:** Kısmen doğrulandı. Tab isimlerinin (Explore, Wishlists, Trips,
Inbox, Profile) güncel sekme seti olduğu bilgisi WebSearch özetinde ikincil bir kaynaktan
("all these options are just a tap away, letting you browse listings, manage your saved
destinations, and keep track of confirmed stays" özet cümlesiyle) geldi; bu, Airbnb'nin
kendi resmi bir sayfası değil, ikincil bir özet kaynağı, doğrudan fetch edilip birebir
doğrulanmadı → **kısmen doğrulanmadı, ikincil kaynak**. Wishlists sekmesinin varlığı ve
"kaydedilen ilanlar" işlevi dolaylı olarak doğrudan fetch edilen help/article/338'den
destekleniyor (aynı işlev web'de "Menu > Wishlists" olarak anılıyor), ama bu kaynak mobil
tab bar'ın kendisini betimlemiyor. İkon+etiket görsel stili, aktif/pasif renk ayrımı ve
sekme sırasının tam mantığı bu araştırmada ekran görüntüsüyle doğrulanmadı, genel
gözlem/eğitim verisinden.

---

## 6. Misafir modu (Traveling) ile Host modu arasında geçiş paterni

**Ne olduğu:** Aynı Airbnb hesabının iki ayrı "mod"u var: Traveling (misafir/gezgin modu)
ve Hosting (host modu); kullanıcı bu iki mod arasında açık bir geçiş eylemiyle geçiyor,
otomatik veya arka planda değil. Doğrudan fetch edilen help/article/3546'ya göre uygulamada
"Profile'ı basılı tutup Hosting'e dokunarak" host moduna, "Menu'yü basılı tutup
Traveling'e dokunarak" misafir moduna geçiliyor; web'de ise profil fotoğrafının yanındaki
"Switch to hosting" / "Switch to traveling" bağlantısıyla aynı geçiş yapılıyor.

**Nerede görülür:** İkisi de; ancak etkileşim biçimi platforma göre farklı: mobilde bir
"basılı tutma" (long-press/hold) gesture'ı, web'de ise tek tıklamalık bir metin bağlantısı.

**UX gerekçesi:** Airbnb'nin aynı kişinin hem misafir hem host olabildiği (ve sıkça ikisini
birden yaptığı) bir platform olması, IA açısından bir zorluk yaratıyor: misafir modunun
navigasyonu (arama, rezervasyonlarım) ile host modunun navigasyonu (ilanlarım, takvim,
gelen rezervasyon talepleri, kazanç) neredeyse hiç örtüşmeyen, tamamen farklı iki menü
seti gerektiriyor. Bunları tek bir birleşik menüde her zaman yan yana göstermek yerine
**ayrı iki mod** olarak tanımlamak (ve kullanıcının bilinçli bir eylemle aralarında
geçmesini istemek), her modun kendi ekranını "gürültüsüz" tutuyor: host modundayken misafir
arama arayüzü, misafir modundayken host takvimi ekranı görünmüyor. Mobilde bu geçişin bir
"basılı tutma" gesture'ı gerektirmesi (tek dokunuş değil), bunun sık yapılan sıradan bir
eylem değil, bilinçli bir "bağlam değiştirme" kararı olduğunu kullanıcıya gesture'ın
kendisiyle işaret ediyor; yanlışlıkla, tek bir yanlış dokunuşla moddan çıkılmasını
zorlaştırıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Aynı kullanıcının platformda iki farklı,
büyük ölçüde örtüşmeyen rolü (alıcı/satıcı, öğrenci/eğitmen, misafir/host) üstlenebildiği
her uygulamada, bu iki rolün navigasyonunu tek bir birleşik (ve dolayısıyla kalabalık)
menüde göstermek yerine ayrı "mod"lar olarak tanımlayıp aralarında bilinçli bir geçiş eylemi
istemek, her modun arayüzünü kendi bağlamına özgü ve sade tutuyor. Bu geçişi (özellikle
mobilde) sıradan bir tek-dokunuştan biraz daha "ağır" bir gesture (basılı tutma gibi)
yapmak, yanlışlıkla mod değiştirmeyi zorlaştırarak kullanıcı hatasını azaltıyor.

**Kaynak / güven notu:** **Doğrulandı.** Mobil uygulamada "Profile'ı basılı tutup Hosting'e
dokunma" ve "Menu'yü basılı tutup Traveling'e dokunma" adımları, web'de "profil fotoğrafının
yanında Switch to hosting / Switch to traveling" bağlantıları, doğrudan fetch edilen
https://www.airbnb.com/help/article/3546 sayfasından birebir alındı. Sayfa ayrıca hesabın
mod durumunun rezervasyon yönetimi ile misafir iletişimi (hosting modu) ile kişisel
seyahat/deneyim yönetimi (traveling modu) arasındaki işlevsel ayrımı da doğruluyor.

---

## 7. Bottom sheet kullanımı: filtreler, tarih seçici, misafir sayacı yeni ekran açmıyor

**Ne olduğu:** Mobilde arama akışının üç ana alt adımı (filtre seçimi, tarih seçici takvimi,
misafir sayısı sayacı) ayrı, tam ekran bir sayfaya geçiş yapmadan, mevcut ekranın altından
yukarı doğru kayan bir panel (bottom sheet) olarak açılıyor. Doğrudan fetch edilen NN/g
makalesine göre bir bottom sheet "mobil bir cihazın alt kenarına sabitlenmiş, ek detay
veya eylem gösteren bir overlay"; bu, "arka plandaki ana bilgiye kullanıcının hâlâ ihtiyaç
duyabileceği" durumlar için özellikle uygun bir pattern.

**Nerede görülür:** Ağırlıklı olarak mobil (native uygulama ve mobil web); masaüstünde
aynı işlevler genelde bir dropdown panel veya modal pencere olarak açılıyor, tam bir bottom
sheet değil (ekranın alt kenarına değil, tetikleyici elemanın hemen altına/üstüne konumlanan
bir panel).

**UX gerekçesi:** Filtre, tarih ve misafir sayısı seçimleri, kullanıcının "arama sonucu
bağlamını" hiç kaybetmeden yapması gereken, sık tekrarlanan, kısa süreli etkileşimler;
bunlar için her seferinde yeni bir tam ekran sayfası açmak (ve geri gitmek) hem gereksiz bir
navigasyon derinliği ekliyor hem de kullanıcının "az önce nerede kalmıştım" hissini
bozuyor. NN/g'nin doğrudan fetch edilen makalesine göre bottom sheet'ler tam olarak bu
senaryo için tasarlanmış: "progressive disclosure" (aşamalı bilgi açma) sağlarken
arka plandaki içeriğin görünürlüğünü koruyor. Aynı kaynağa göre bottom sheet'lerin
**sayfa navigasyonunun yerine kullanılmaması gerektiği** de açıkça belirtiliyor ("bir
e-ticaret ürün detay sayfasını bir sheet içinde göstermeyin" örneğiyle); Airbnb'nin
filtre/tarih/misafir sayacı gibi kısa, tek-amaçlı, "seç ve kapat" etkileşimleri için bottom
sheet kullanıp, ilan detay sayfası gibi uzun/gezinilebilir içerikler için tam ekran
sayfalara geçmesi, bu ayrımla tutarlı: bottom sheet kısa kararlar için, tam ekran sayfa
uzun içerik/gezinme için.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kullanıcının ana bağlamı (arama sonucu,
liste, form) kaybetmeden hızlıca bir seçim yapması gereken her mobil akışta (filtre, tarih,
miktar/adet seçici, sıralama seçenekleri), bunu yeni bir tam ekran sayfa yerine bir bottom
sheet olarak sunmak navigasyon derinliğini azaltıyor ve "az önce nerede kalmıştım"
hissini koruyor. NN/g'nin uyardığı gibi bu pattern'i kısa, tek-amaçlı etkileşimlerle
sınırlı tutmak önemli: uzun, kendi içinde gezinilmesi gereken içerikleri (ör. bir ürün
detay sayfası) bir sheet'e sıkıştırmak, kullanıcı deneyimini kötüleştirebiliyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Bottom sheet'in genel tanımı, "progressive
disclosure" gerekçesi ve "sayfa navigasyonu yerine kullanılmamalı" uyarısı **doğrulandı**,
doğrudan fetch edilen https://www.nngroup.com/articles/bottom-sheet/ sayfasından birebir
alındı; ancak bu, Airbnb'ye özgü bir kaynak değil, genel bir mobil UX pattern rehberi.
Airbnb'nin filtre/tarih seçici/misafir sayacı akışlarının fiilen bottom sheet olarak
uygulandığı bilgisi, bu araştırmada Airbnb'nin kendi bir kaynağından (yardım merkezi,
mühendislik blogu) doğrudan fetch edilerek doğrulanmadı; bu proje kapsamındaki 01 numaralı
bölümde (discovery-search) benzer bir gözlem genel deneyim/eğitim verisinden aktarılmıştı,
burada da aynı statüde: **doğrulanmadı, eğitim verisinden/genel gözlem**.

---

## 8. Mobil geri navigasyon ve gesture paternleri: swipe back, modal dismiss

**Ne olduğu:** Mobil uygulama içinde bir alt ekrandan (ilan detayı, filtre sayfası, host
profili) bir üst ekrana dönmenin iki temel yolu var: platformun kendi native geri
mekanizması (iOS'ta ekranın solundan sağa kaydırma swipe gesture'ı, Android'de sistem geri
tuşu/gesture'ı) ve bottom sheet/modal pencerelerde aşağı doğru kaydırarak (swipe-down) veya
açık bir "kapat" (X) butonuna dokunarak kapatma.

**Nerede görülür:** Sadece mobil (native uygulama); web'de bu karşılığı tarayıcının kendi
"geri" tuşu ve URL değişimi üstleniyor.

**UX gerekçesi:** WebSearch özetlerinde toplanan genel mobil UX kaynaklarına göre (Airbnb'ye
özgü değil, platform genelinde geçerli bir prensip) bir bottom sheet veya modalin hem
swipe-down gesture'ı hem sistemin native "geri" mekanizmasıyla hem de açık bir kapat
butonuyla kapatılabilmesi gerekiyor; sadece gesture'a güvenmek, gesture'ı bilmeyen veya
motor becerisi kısıtlı kullanıcıları dışarıda bırakıyor. Bu üç yöntemin (gesture, sistem geri,
açık buton) aynı anda desteklenmesi, "kullanıcı bu ekrandan nasıl çıkacağını farklı
şekillerde tahmin edebilir, hangisini denerse denesin çalışmalı" ilkesini yansıtıyor;
bu bir tür yedeklilik (redundancy) tasarımı: platformun kendi işletim sistemi
konvansiyonlarına (iOS'ta swipe-back, Android'de sistem geri) güvenmek, kullanıcının
zaten yıllardır alışkın olduğu bir kas hafızasını yeniden öğretmeye çalışmak yerine
kullanmak anlamına geliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Her mobil uygulamada, özel bir "geri" davranışı
icat etmek yerine platformun (iOS/Android) yerleşik geri gesture ve tuş konvansiyonlarını
desteklemek, kullanıcının uygulamalar arasında taşıdığı kas hafızasını (muscle memory)
kullanır ve öğrenme eğrisini sıfıra indirir. Bottom sheet/modal gibi geçici overlay'lerde
en az üç çıkış yolunu (gesture, sistem geri tuşu, görünür kapat butonu) aynı anda açık
tutmak, farklı kullanıcı gruplarının (gesture'a alışkın olanlar, olmayanlar, erişilebilirlik
ihtiyacı olanlar) hepsine hizmet ediyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden / genel platform prensipleri.**
Bu maddedeki bilgiler Airbnb'ye özgü bir kaynaktan (yardım merkezi, mühendislik blogu,
ekran görüntüsü) doğrulanmadı; genel mobil UX kaynaklarının (NN/g'nin doğrudan fetch edilen
bottom sheet makalesindeki "support the Back button for dismissal" tavsiyesi hariç, ki bu
kısım **doğrulandı**) WebSearch özetlerinden ve platformun (iOS/Android) genel bilinen
davranışlarından derlendi. Airbnb'nin spesifik ekranlarında bu üç yöntemin (swipe, sistem
geri, X butonu) hepsinin birden desteklenip desteklenmediği bu araştırmada teyit edilmedi.

---

## 9. Breadcrumb'sız navigasyon felsefesi

**Ne olduğu:** Airbnb'nin ne web'de ne mobilde klasik bir breadcrumb (ör. "Anasayfa >
İspanya > Barselona > İlan #123" şeklinde tıklanabilir bir konum izi) kullanmaması; bunun
yerine konum bilgisi arama pilinin/başlığının içine gömülü (ör. "Barselona, İspanya" arama
kutusunda) veya geri navigasyonu tek adımlık bir "geri" ok/gesture'ıyla (madde 8) sağlanıyor.

**Nerede görülür:** İkisi de; hiçbir ana akışta (arama sonucu, ilan detay, checkout)
çok seviyeli bir breadcrumb izine rastlanmıyor.

**UX gerekçesi:** Breadcrumb'lar klasik olarak derin, hiyerarşik bir kategori ağacında
("Elektronik > Telefon > Aksesuar > Kılıf" gibi) kullanıcının "ağaçta neredeyim" sorusunu
cevaplamak için var; Airbnb'nin IA'sı ise bu tarz derin bir hiyerarşi değil, büyük ölçüde
**düz** (arama sonucu -> ilan detay -> checkout gibi, 2-3 seviyeyi geçmeyen) bir yapı.
Konum bilgisi bir hiyerarşi seviyesi olarak değil, aramanın kendisinin bir **parametresi**
olarak modelleniyor (kullanıcı "İspanya"dan "Barselona"ya konum değiştirdiğinde bu bir
breadcrumb tıklaması değil, arama kutusunu düzenleme eylemi). Bu, breadcrumb'ın gerektirdiği
"burada kaç seviye derindeyim" zihinsel modelini kullanıcıya hiç yüklemiyor; onun yerine
"her zaman aramanın/geçmiş adımın bir tık gerisindeyim" daha basit bir modelle
yetiniyor. Bu tasarım tercihi doğrudan madde 1'deki (arama-öncelikli IA) felsefeyle
tutarlı: IA'nın omurgası bir kategori ağacı değil arama olduğu için, o ağacı temsil eden
bir gezinme aracına (breadcrumb) da ihtiyaç kalmıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Ürün/içerik kataloğunun derin, sabit bir
kategori hiyerarşisi yerine büyük ölçüde bir arama/filtre motoruyla keşfedildiği her
platformda (seyahat, ilan siteleri, bazı e-ticaret dikeyleri), klasik bir breadcrumb yerine
"mevcut arama/filtre durumunu" görünür ve düzenlenebilir tutmak (arama çubuğunda,
filtre çipleri olarak) yeterli olabiliyor; bu, kullanıcıya "ağaçta nerede olduğunu"
değil "şu an neyi aradığını" gösteriyor, ki bu genelde arama-öncelikli bir üründe daha
doğru soru. Ancak gerçekten derin, sabit bir kategori ağacı olan platformlarda (ör. büyük
bir elektronik e-ticaret sitesi) breadcrumb hâlâ değerli olabilir; bu felsefe her platforma
körü körüne uygulanmamalı.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu madde, Airbnb'nin herhangi
bir resmi kaynağından doğrudan alınmış bir ifade değil; bu araştırmacının, madde 1 ve
madde 5'teki doğrulanmış gözlemlerden (arama-öncelikli IA, düz sekme yapısı) ve genel
ürün gözleminden (Airbnb'nin ana akışlarında breadcrumb bulunmaması) çıkardığı bir
sentez/yorum. Bu açıkça **bir çıkarım**, Airbnb'nin kendi ifade ettiği bir tasarım
prensibi değil; tek bir kaynaktan da doğrulanmadı.

---

## 10. Bildirim/e-postadan uygulama içi spesifik bir ekrana derin bağlantı (deep linking)

**Ne olduğu:** Kullanıcı bir push bildirimine, e-postaya veya SMS'e dokunduğunda, uygulama
sadece açılmıyor; kullanıcı doğrudan ilgili ekrana (ör. belirli bir mesaj dizisi, belirli
bir rezervasyonun Trips detay sayfası) yönlendiriliyor. Doğrudan fetch edilen GitHub
README'sine göre Airbnb bunun için kendi açık kaynak Android kütüphanesini
(DeepLinkDispatch) geliştirmiş: "annotation tabanlı bir API ile uygulama deep link'lerini
tanımlamak" için; bir Activity `@DeepLink("example://example.com/deepLink/{id}")` gibi bir
URI kalıbıyla işaretleniyor, kütüphane gelen bir deep link'i bu kalıpla eşleştirip
parametreleriyle (ör. `{id}`) birlikte doğru ekrana yönlendiriyor.

**Nerede görülür:** Sadece mobil (native uygulama); bildirimden/e-postadan gelen tıklama
web'de ise doğrudan ilgili URL'e yönlendiren klasik bir hyperlink olarak çalışıyor, ayrı bir
"deep link" mekanizmasına ihtiyaç duymuyor.

**UX gerekçesi:** Bir bildirimin amacı kullanıcıyı belirli bir bilgiye ("host'un mesajına",
"rezervasyon onayına") olabildiğince az adımda ulaştırmak; bildirime dokunduğunda kullanıcıyı
uygulamanın ana ekranına (Explore) düşürüp oradan tekrar Inbox'a, tekrar ilgili mesaj
dizisine gitmesini istemek, bildirimin sağladığı "bağlamı" boşa harcıyor. Doğrudan fetch
edilen kaynağa göre DeepLinkDispatch'in **annotation tabanlı, deklaratif** bir API sunması
(her Activity'nin kendi URI kalıbını kendi tanımlaması, merkezi bir dev if/else zincirine
ihtiyaç duyulmaması), mühendislik tarafında da bu derin bağlantı setinin ölçeklenebilir
kalmasını sağlıyor; bildirim türü arttıkça (yeni mesaj, rezervasyon onayı, review hatırlatması,
fiyat değişikliği) her biri kendi hedef ekranına kendi annotation'ıyla bağlanabiliyor,
merkezi bir yönlendirme dosyasının şişmesi gerekmiyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bildirim/e-posta gönderen her mobil
uygulamada, bildirime dokunmanın kullanıcıyı jenerik bir ana ekrana değil, bildirimin
konusuyla ilgili tam ekrana (mesaj dizisi, sipariş detayı, randevu sayfası) götürmesi,
bildirimin sağladığı bağlamsal avantajı koruyor. Mühendislik tarafında bunu her ekranın
kendi URI kalıbını deklaratif olarak tanımladığı bir sistemle (Airbnb'nin
DeepLinkDispatch'i gibi bir annotation/route-mapping yaklaşımıyla) kurmak, yeni bildirim
türleri eklendikçe merkezi bir yönlendirme kod bloğunun karmaşıklaşmasını önlüyor.

**Kaynak / güven notu:** Kısmen doğrulandı. DeepLinkDispatch'in var olduğu, Airbnb'nin
kendi açık kaynak GitHub deposu olduğu, annotation tabanlı API sunduğu ve `@DeepLink`
örneğiyle bir Activity'nin belirli bir URI kalıbına ve parametreye (`{id}`) nasıl
bağlandığı **doğrulandı**, doğrudan fetch edilen
https://github.com/airbnb/DeepLinkDispatch/blob/master/README.md sayfasından birebir
alındı; bu, Airbnb'nin kendi mühendislik ekibinin ürettiği gerçek bir teknik artefakt,
ikincil bir yorum değil. Ancak bu kütüphanenin bildirim/e-posta tıklamalarında **fiilen**
kullanıldığı, hangi bildirim türlerinin hangi ekrana yönlendirdiği gibi ürün-seviyesi
detaylar bu README'de yok; bu bağlantı WebSearch özetinde ikincil bir kaynaktan (mockingly.ai
mülakat sorusu sayfası) çıkarımsal olarak kuruldu → **kısmen doğrulanmadı, kütüphanenin
varlığı doğrulandı ama ürün akışındaki fiili kullanımı doğrudan teyit edilmedi**.

---

## 11. Sekme çubuğunda badge / bildirim noktası kullanımı

**Ne olduğu:** Mobil alt tab bar'daki Inbox (ve bazen Trips) sekmesinin üzerinde, okunmamış
mesaj veya yeni bir güncelleme olduğunda beliren küçük, genelde kırmızı bir nokta veya
sayı içeren rozet (badge).

**Nerede görülür:** Sadece mobil (native uygulamanın alt tab bar'ı); web'de karşılığı
genelde header'daki mesaj/bildirim ikonunun üzerinde benzer bir küçük sayı rozeti olarak
görülüyor (bu araştırmada ayrıca doğrulanmadı).

**UX gerekçesi:** Beş sekmelik bir tab bar'da kullanıcı her sekmeyi düzenli aralıklarla
kontrol etmeyecektir; badge, "şu an hangi sekmede senin dikkatini bekleyen yeni bir şey
var" sorusunu, kullanıcı o sekmeye hiç girmeden, pasif bir görsel sinyalle cevaplıyor. Bunun
sadece Inbox gibi zaman-duyarlı, kullanıcı eylemi gerektirebilecek içerikler için
kullanılması (ör. Explore veya Wishlists sekmesinde sürekli bir badge olmaması) önemli:
badge'in gücü, onu **seyrek ve anlamlı** tutmakla korunuyor; her sekmede sürekli bir badge
olması, sinyalin "her şey önemli, o zaman hiçbir şey önemli değil" şeklinde anlamını
yitirmesine yol açardı.

**Airbnb dışı bir uygulamaya uyarlama notu:** Alt tab bar kullanan her mobil uygulamada,
badge/bildirim noktasını sadece kullanıcının gerçekten eyleme geçmesi gerekebilecek,
zaman-duyarlı içerik taşıyan sekmelerde (mesajlar, onay bekleyen talepler) kullanmak, ve
bunu keşif/gezinme gibi eylemsiz sekmelerde (Explore benzeri) hiç kullanmamak, badge'in
"dikkat gerektiren bir şey var" sinyalinin güvenilirliğini koruyor. Badge'i her sekmede
gösteren veya çok sık tetiklenen bir tasarım, kullanıcıların zamanla badge'i görmezden
gelmeyi öğrenmesine (banner blindness'ın bir varyantı) yol açabilir.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu maddenin tamamı genel
mobil UX gözlemine ve bu tarz badge sistemlerinin sektör genelinde nasıl çalıştığına dair
bilinen prensiplere dayanıyor; Airbnb'nin kendi bir kaynağından (yardım merkezi, mühendislik
blogu, ekran görüntüsü) bu araştırmada doğrudan fetch edilerek doğrulanmadı. Bildirim
kanalları ve tiplerinin genel listesi (Mesajlar, Hatırlatmalar, Promosyonlar, Politika/
topluluk, Hesap desteği) doğrudan fetch edilen help/article/14'ten **doğrulandı**, ama bu
sayfa badge/tab bar görsel davranışından hiç bahsetmiyor; badge'in kendisi bu sayfadan
**doğrulanmadı**.

---

## 12. Kategori sekmeleri: ana header'ın altında ikinci bir navigasyon katmanı

**Ne olduğu:** Web'de ve mobilde ana arama pilinin/header'ının hemen altında, yatay
kaydırılabilir bir ikon+etiket sekme sırası (ör. "Icons", "Amazing views", "Tiny homes",
"OMG!" gibi Airbnb'nin kendi adlandırdığı konaklama kategorileri); bu sekmeler ana IA
hiyerarşisinde header'dan sonra gelen, arama sonucu listesini filtreleyen ikincil bir
katman olarak duruyor.

**Nerede görülür:** İkisi de; web'de header'ın hemen altında, mobilde arama sonucu
ekranının en üstünde, arama pilinin altında yatay bir şerit olarak.

**UX gerekçesi:** Bu kategori sekmeleri, madde 1'deki arama-öncelikli felsefeyle ilk bakışta
çelişiyor gibi görünebilir (bunlar bir tür kategori gezintisi, arama değil), ama aslında
tamamlayıcı bir rol oynuyor: **birincil** eylem hâlâ arama (belirli bir yer/tarih için),
kategori sekmeleri ise arama sonucu **içinde** ikincil bir keşif/filtreleme katmanı. WebSearch
özetlerinde toplanan bir vaka analizine göre Airbnb bir dönem kategorileri kaldırmış,
sonra geri getirmiş; bu, kategorilerin "rezervasyon değerini artırdığı ama sürtünme de
eklediği" bulgusuna işaret ediyor (ikincil kaynaktan, doğrudan doğrulanmadı). Kategori
sekmelerinin ana header'ın **altında**, ayrı ama bitişik bir katman olarak durması,
IA'da net bir hiyerarşi kuruyor: "önce nereye/ne zaman" (arama), "sonra ne tür bir yer"
(kategori) sırasıyla, kullanıcıyı önce zorunlu parametreleri (konum, tarih) girmeye,
sonra isteğe bağlı bir keşif katmanına yönlendiriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Arama-öncelikli bir IA'da (madde 1) dahi,
kullanıcının "spesifik bir şey aramak" ile "sadece göz atmak/ilham almak" arasında gidip
gelebildiği her platformda, ana arama çubuğunun hemen altına yatay kaydırılabilir bir
kategori/tema sekmesi eklemek, arama-öncelikli IA'yı bozmadan bir keşif katmanı ekleyebiliyor.
Bu katmanın ana arama unsurunun **yerine** değil, onun **altında ikincil bir uzantısı**
olarak konumlanması önemli: kategori sekmeleri birincil navigasyon hiyerarşisini
devralırsa, arama-öncelikli felsefe zayıflar.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden / ikincil kaynak.** Kategori
sekmelerinin varlığı ve genel görsel biçimi (header'ın altında yatay ikon+etiket şeridi)
genel ürün gözleminden geliyor; WebSearch özetlerinde bulunan iki Medium makalesi
(kategori sekmelerinin "basitlik için kaldırılıp sonra geri getirildiği" ve "rezervasyon
değerini artırdığı ama sürtünme eklediği" iddiaları dahil) doğrudan fetch edilip birebir
doğrulanmadı, sadece WebSearch özet cümleleri okundu → **kısmen doğrulanmadı, ikincil
kaynak**. "Icons", "Amazing views" gibi tam kategori adları da bu araştırmada birincil
kaynaktan teyit edilmedi.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip birincil/güçlü içerik olarak doğrulanan kaynaklar:** misafir/host
  modu geçişi (Airbnb yardım merkezi, help/article/3546), wishlist kaydetme ve "Menu >
  Wishlists" navigasyon yolu (help/article/338), bildirim tipleri/kanalları (help/article/14,
  ama tıklama/deep-link davranışı bu sayfada yok), NN/g'nin genel bottom sheet rehberi
  (Airbnb'ye özgü değil ama doğrudan fetch edildi ve genel prensip olarak alıntılandı),
  Airbnb'nin kendi açık kaynak GitHub deposu DeepLinkDispatch'in README'si (gerçek bir
  mühendislik artefaktı), Airbnb Design ekibinin Medium'daki DLS yazısı (genel ama gerçek
  birincil kaynak). Bu 6 kaynak, 12 pattern'den 3'ünü (madde 6, 7'nin bir kısmı, 10'un bir
  kısmı) doğrudan ve güçlü şekilde destekliyor.
- **Kısmen doğrulanan (gerçek kaynak fetch edildi ama Airbnb'ye özgü değil, ya da ikincil
  kaynaktan geldi, ya da kaynağın sadece bir kısmı ürün detayını destekliyor):** madde 1
  (arama-öncelikli IA, bir bağımsız tasarımcının vaka çalışmasından kısmen), madde 4
  (kullanıcı menüsü, wishlist navigasyon yolu doğrulandı ama görsel biçim doğrulanmadı),
  madde 5 (tab bar isimleri ikincil kaynaktan), madde 10 (kütüphanenin varlığı doğrulandı,
  ürün akışındaki fiili kullanımı doğrulanmadı), madde 12 (kategori sekmeleri, iki Medium
  makalesinin özetinden).
- **Büyük ölçüde doğrulanmadı, eğitim verisinden/genel gözlem:** madde 2 ("Airbnb your
  home" CTA'sı; airbnb.com/host/homes sayfası fetch denemesinde 403 ile engellendi),
  madde 3 (globe ikonu davranışı), madde 8 (swipe-back/modal dismiss gesture'ları, genel
  platform konvansiyonu), madde 9 (breadcrumb'sız felsefe, bu araştırmacının kendi
  çıkarımı), madde 11 (badge/bildirim noktası kullanımı).
- Toplamda 12 pattern'den **2 tanesi** (madde 6, ve kısmen madde 10) doğrudan Airbnb
  birincil kaynağıyla güçlü doğrulandı, **6 tanesi** kısmen doğrulandı (gerçek kaynak
  fetch edildi ama ikincil kaynak, Airbnb'ye özgü olmayan genel prensip, veya kısmi içerik),
  **5 tanesi** (madde 2, 3, 8, 9, 11) büyük ölçüde doğrulanmadı/eğitim verisinden. Bu
  bölüm, önceki bölümlere (03, 05, 06) kıyasla Airbnb'nin kendi yardım merkezinde
  navigasyon/IA konusuna doğrudan değinen sayfaların daha az olması nedeniyle görece daha
  zayıf birincil kaynaklı; navigasyonun görsel/etkileşimsel detayları büyük ölçüde ürünün
  kendisinin ekran görüntüsüyle doğrulanması gerektiren, bu araştırmada erişilemeyen bir
  alan. Canlı üründe navigasyon ve kategori sekmeleri sık A/B test edilen alanlar olduğundan
  (ör. kategorilerin kaldırılıp geri getirilmesi), bu doküman "yerleşik/tekrar gözlemlenen
  pattern" çerçevesiyle okunmalı, "şu an birebir ekran görüntüsü" olarak değil.
