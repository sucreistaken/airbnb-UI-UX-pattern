# 8. Görsel Tasarım Sistemi (Visual Design System / DLS)

Bu bölüm Airbnb'nin ürününü web ve mobilde tutarlı kılan görsel dilini kapsıyor: özel tipografi
(Airbnb Cereal), Rausch adlı mercan-kırmızı marka rengi ve bunun nötr-öncelikli bir arayüzde ne
kadar cimri kullanıldığı, spacing/grid sistemi, köşe yuvarlaklığı dili, ikon sistemi (2025'te
tanıtılan "Lava" formatı dahil), fotoğraf-öncelikli tasarım felsefesi, motion/micro-interaction
ilkeleri (deklaratif animasyon çerçevesi, kart-detay arası shared element transition), Airbnb'nin
kendi açtığı ve tüm sektöre yayılan Lottie animasyon kütüphanesi, buton/CTA hiyerarşisi, dark mode
durumu ve erişilebilirlik taahhüdü. Bu, resmi olarak "Design Language System (DLS)" adıyla anılan
ve Airbnb'nin 2016'dan beri kamuya en çok konuştuğu iç sistemlerden biri; buna rağmen DLS'in kendi
resmi/güncel bir genel-kullanıma-açık stil rehberi (token listesi, tam renk paleti, tam spacing
skalası) yok, dolayısıyla bu bölümdeki birçok piksel-seviyesi değer (tam renk kodları, tam spacing
adımları, tam radius değerleri) Airbnb'nin kendi resmi kaynağından değil, arayüzü tersine
mühendislikle inceleyen üçüncü taraf "design token extractor" siteleri ve topluluk gözleminden
geliyor; bu, diğer bölümlerdeki yardım merkezi sayfalarına kıyasla belirgin biçimde daha zayıf bir
kaynak katmanı ve her ilgili maddede ayrıca not edildi.

Araştırma sırasında fiilen fetch edilip okunan birincil/güçlü kaynaklar: Airbnb Design'ın kendi
Medium yayınından Karri Saarinen imzalı "Building a Visual Language: Behind the scenes of our
Airbnb design system" ve "Working Type" (Cereal'in tanıtım yazısı); Airbnb Engineering'in kendi
Medium yayınından "Introducing Lottie", "React Native at Airbnb: The Technology", "Motion
engineering at scale" (Cal Stephens) ve "Animations: Bringing the Host Passport to Life on iOS"
(Anne Lu); Airbnb'nin kendi yardım merkezinden erişilebilirlik sayfası (help/article/2166).
Bunların dışında kalan (renk hex kodları, spacing/radius tam değerleri, buton piksel
özellikleri, Lava ikon formatının teknik detayları, dark mode durumu) tamamen ikincil/üçüncü
taraf kaynaklardan ya da topluluk forumlarından geliyor ve ilgili maddelerde ayrıca işaretlendi.

---

## 1. Airbnb Cereal: özel tipografi, tek yazı ailesiyle tüm hiyerarşi

**Ne olduğu:** Airbnb, 2018'e kadar Helvetica Neue ve platform sistem fontlarının karışımını
kullanıyordu; bunun yerine Londra merkezli tip stüdyosu Dalton Maag ile yaklaşık 18 aylık bir
çalışmanın sonunda "Airbnb Cereal" adlı özel bir yazı tipi geliştirdi ve 15 Mayıs 2018'de
yayına aldı. Doğrudan fetch edilen "Working Type" yazısına göre ekip önce "insani, samimi,
davetkâr ve yaratıcı bir profesyonellik" gibi yön belirleyici kelimeler tanımladı; ardından
tasarımda küçük punto okunabilirliğini artıran daha geniş harf açıklıkları ve daha yüksek
x-height, farklı metin uzunlukları/cihaz çözünürlükleri gibi dinamik ortamlarda çalışabilme
ve Airbnb'nin Bélo logosundan ilham alan "a" ve "b" harfleri gibi marka-özgü ama abartısız
detaylar hedeflendi. Yazı 6 ağırlıkta geliyor (Light, Book, Medium, Bold, Extra Bold, Black) ve
web/iOS/Android'in üçünde de aynı ailenin kullanılması, tüm metin hiyerarşisinin (başlık, gövde,
etiket, buton metni) tek bir font ailesinin farklı ağırlıklarıyla kurulmasını sağlıyor.

**Nerede görülür:** İkisi de; web, iOS ve Android'de aynı Cereal ailesi kullanılıyor (yazının
kendi ifadesiyle platformlar arası tutarlılık amaçlanmış).

**UX gerekçesi:** Bir markanın büyüklüğüne (Airbnb gibi küresel, çok dilli, yüksek trafikli bir
üründe) ulaştığında hazır bir sistem fontuna güvenmenin iki riski var: (a) marka farklılaşması
sağlamaması (herkes aynı sistem fontunu kullanıyor), (b) küçük punto/karmaşık arayüz
yoğunluğunda okunabilirlik sorunları (Airbnb'nin önceki fontunun tam olarak yaşadığı sorun).
Özel bir tipografi yatırımı bu iki sorunu aynı anda çözüyor: marka kimliğini harf düzeyine kadar
indirip (a ve b harflerinde logoya gönderme) hem de mühendislik/okunabilirlik gereksinimlerini
(geniş x-height, farklı dil/uzunluklarla test) doğrudan tipin kendisine gömüyor. Tek bir ailenin
6 ağırlıkla tüm hiyerarşiyi taşıması, arayüzün farklı font aileleri karıştırmasından kaynaklanan
görsel gürültüyü de yapısal olarak engelliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Küçük/orta ölçekli bir ürün için özel tipografi
geliştirmek genelde maliyet-etkin değil, ama buradaki devredilebilir ilke şu: bir font seçilirken
"marka kişiliği" ve "farklı punto/dilde okunabilirlik" aynı karar sürecinde birlikte
değerlendirilmeli, sadece estetik tercihle seçilmemeli. Hazır bir font ailesi kullanan bir ürün
bile, hiyerarşiyi tek bir ailenin sınırlı ağırlık setiyle (ör. Regular/Medium/Bold gibi 3-4
ağırlık) kurup farklı font aileleri karıştırmaktan kaçınırsa, Cereal'in sağladığı tutarlılık
faydasının büyük kısmını, özel font geliştirme maliyeti olmadan elde edebilir.

**Kaynak / güven notu:** **Doğrulandı**: yazı tipinin adı, Dalton Maag ile iş birliği, ~18 aylık
geliştirme süreci, 15 Mayıs 2018 lansmanı, tasarım hedefleri (insani/samimi/profesyonel yön
kelimeleri, geniş aperture, yüksek x-height, Bélo'dan ilhamla "a"/"b" harfleri), 6 ağırlık ve
web/iOS/Android'de tek ailenin kullanılması, doğrudan fetch edilen
https://medium.com/airbnb-design/working-type-81294544608b (Karri Saarinen, Airbnb Design resmi
Medium yayını) yazısından birebir alındı. "11.000'den fazla ekran görüntüsüyle test" ve "2
milyon kullanıcıyla A/B test" rakamları da aynı doğrudan fetch edilen kaynaktan geliyor.

---

## 2. Rausch: tek, cimri kullanılan marka rengi; nötr-öncelikli arayüz

**Ne olduğu:** Airbnb'nin marka rengi "Rausch" (kurucuların ilk kiraladıkları San Francisco
sokağından adını alıyor), sıcak bir mercan-kırmızı. WebSearch özetlerinde görülen (ama ayrıca
doğrudan fetch edilerek doğrulanmayan) hex değerleri Rausch için tarihsel olarak #FF5A5F,
güncel canlı üründe ise daha canlı bir kırmızı-pembe olan #FF385C olarak geçiyor. Arayüzün geri
kalanı ise neredeyse tamamen nötr bir gri/siyah/beyaz skalası (metin için koyu antrasit ~#222222,
ikincil metin için orta gri); tekrarlanan topluluk gözlemine göre bu marka rengi sadece birincil
CTA butonu, arama çubuğu/orb'u, aktif sekme göstergesi, kalp/wishlist ikonu gibi az sayıda "asıl
eylem" noktasında kullanılıyor, dekoratif arka plan, kart kenarlığı veya büyük renk bloğu olarak
kullanılmıyor.

**Nerede görülür:** İkisi de; web ve mobilde aynı renk mantığı gözlemleniyor (birincil CTA'lar,
arama/kategori vurguları, kalp ikonu).

**UX gerekçesi:** Doğrudan fetch edilen Saarinen yazısındaki dört DLS ilkesinden biri "Iconic":
"work should speak boldly about design focus and decision making". Bir markanın rengini her
yerde kullanmak yerine sadece en kritik etkileşim noktalarında (birincil eylem, arama, kalp)
göstermek, bu rengi gördüğü her yerde kullanıcının "burada eyleme geçebilirim" diye
okumasını sağlıyor; renk bir dekorasyon değil bir sinyal haline geliyor. Arayüzün geri kalanının
nötr kalması iki şeyi aynı anda sağlıyor: (a) gerçek içerik olan fotoğrafların (mekân, deneyim
görselleri) renkle rekabet etmemesi, böylece fotoğraflar arayüzün en canlı/renkli unsuru olarak
öne çıkabiliyor, (b) tek bir vurgu rengi her yerde tekrarlandığında oluşabilecek "yorgunluk"
hissinin, rengin seyrek kullanılması sayesinde önlenmesi.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir marka rengi seçildiğinde bunu "nerede
KULLANILMAYACAĞI" listesiyle birlikte tanımlamak (ör. "sadece birincil CTA, aktif durum
göstergesi ve tek bir favori/kaydetme ikonu; büyük arka plan bloklarında, kart kenarlıklarında
veya ikincil butonlarda asla") renk sisteminin bir sinyal olarak kalmasını sağlar. Özellikle
görselin (ürün fotoğrafı, kullanıcı içeriği) ürünün merkezinde olduğu her platformda, arayüz
kromunu bilinçli olarak nötr tutmak, o görselin öne çıkmasına izin veriyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Rausch adının ve renk felsefesinin (tek, cimri
kullanılan vurgu rengi + nötr-öncelikli arayüz) genel çerçevesi, doğrudan fetch edilen Saarinen
DLS yazısındaki "Iconic" ilkesiyle ve o yazının genel "restraint" (kısıtlılık/cimrilik) diline
tutarlı, ancak o yazı rengin tam kullanım kurallarını (hangi bileşenlerde kullanılır/kullanılmaz)
madde madde vermiyor. Tam hex kodları (#FF5A5F tarihsel, #FF385C güncel), "sadece CTA/arama/kalp
ikonunda kullanılıyor" iddiası ve "arayüzün %0 renklilik" gibi kesin ifadeler, WebSearch
özetlerinde görülen üçüncü taraf design-token-extractor sitelerinden (superdesign.dev,
designmd.cc, mobbin.com gibi) geliyor, bu sayfalar doğrudan fetch edilip birebir doğrulanmadı
→ **doğrulanmadı, eğitim verisi + ikincil kaynak özetinden**.

---

## 3. Spacing/grid sistemi: yoğun bir marketplace için sık aralıklı ızgara

**Ne olduğu:** Topluluk gözlemine göre Airbnb arayüzü, çoğu SaaS ürününden daha sık aralıklı bir
spacing skalası kullanıyor; WebSearch özetlerinde geçen (ama doğrudan fetch edilerek
doğrulanmayan) bir analiz 4px tabanlı bir grid öneriyor (2, 4, 8, 12, 16, 24, 32px gibi adımlarla),
gerekçe olarak da ilan listesi/grid sayfalarının yüksek "kart yoğunluğu" ihtiyacını gösteriyor
(bir ekranda mümkün olduğunca çok ilan kartı sığdırmak, ama okunabilir kalmak). Aynı ikincil
kaynaklara göre büyük rezervasyon panellerinde 24px, olanak (amenity) satırlarında 16px, ilan
kartı meta verisinde ise 12px gibi daha sıkı iç boşluklar gözlemleniyor.

**Nerede görülür:** İkisi de; en belirgin biçimde arama sonuçları grid/liste görünümünde (kart
aralıkları) ve ilan detay sayfasındaki bölüm/satır boşluklarında.

**UX gerekçesi:** Airbnb'nin temel ürünü (bir arama sonucu sayfasında onlarca ilan kartı, her
kartta fotoğraf + başlık + fiyat + puan) doğası gereği yüksek bilgi yoğunluğu gerektiriyor; bu,
tipik bir SaaS panosundan (genelde az sayıda büyük bileşen) farklı bir spacing ihtiyacı yaratıyor.
Daha sık/küçük bir taban birimin (4px gibi) kullanılması, tasarımcılara "8px'in altına
inemem" kısıtlaması olmadan, kart içi meta veriyi (fiyat, puan, mesafe gibi küçük bilgi
parçacıklarını) daha sıkı paketleyebilme esnekliği veriyor; aynı zamanda büyük bölümler arası
(rezervasyon paneli gibi) daha geniş boşluklar (24px+) kullanılarak "bu yoğun ama kaotik değil"
hissi korunuyor. Yani spacing skalası tek bir sabit ritim değil, içeriğin yoğunluğuna göre kademeli
bir sistem olarak işliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Yüksek yoğunluklu bir liste/grid görünümü (ilan,
ürün, sonuç kartı) ile daha az sayıda büyük bilgi bloğu (checkout, form, panel) aynı üründe bir
arada bulunuyorsa, tek bir kaba spacing skalası (ör. sadece 8/16/24/32) her ikisine de
uymayabilir; daha ince taneli bir taban birim (4px gibi) tanımlayıp yoğun alanlarda küçük
adımları, ferah alanlarda büyük adımları kullanmak, aynı sistemin iki farklı yoğunluk ihtiyacını
karşılamasını sağlıyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + ikincil kaynak özetinden.** Bu maddenin
tamamı WebSearch özetlerinde görülen üçüncü taraf "design token extractor" sitelerinden
(superdesign.dev, designmd.cc, design-extractor.com) geliyor; bu siteler arayüzü tersine
mühendislikle inceleyip tahmini token listeleri üretiyor, Airbnb'nin kendisi tarafından
yayınlanmış değil. Hiçbiri doğrudan fetch edilip birebir metni okunmadı, sadece WebSearch'ün
ürettiği özetler kullanıldı. Bu bölümdeki en zayıf kaynaklı maddelerden biri.

---

## 4. Köşe yuvarlaklığı / şekil dili: keskin köşesiz, yumuşak bir arayüz

**Ne olduğu:** Topluluk gözlemine göre Airbnb arayüzünde neredeyse hiçbir bileşen keskin (0px)
köşeye sahip değil: butonlarda küçük bir radius (WebSearch özetlerinde ~8px), ilan kartlarında
orta büyüklükte bir radius (~12-14px, bazı kaynaklarda 12-20px aralığı), arama çubuğunda tam
"pill" (kapsül, radius 9999px/tam yuvarlak), kalp/wishlist ikonu ve navigasyon orb'larında tam
daire (%50 radius), kategori şeritlerinde ise daha büyük bir radius (~32px) gözlemleniyor. Bu
skalanın küçükten büyüğe (buton < kart < pill/daire) kademeli olarak artması, "önemli/etkileşimli
olan ne kadar yuvarlanıyor" gibi örtük bir hiyerarşi izlenimi veriyor.

**Nerede görülür:** İkisi de; en belirgin örnekler arama çubuğu (pill), kalp ikonu (tam daire),
ilan kartı köşeleri ve birincil CTA butonları.

**UX gerekçesi:** Yuvarlak köşeler, tasarımda genel olarak "yumuşak/dostane/tehditkâr olmayan"
bir izlenim yaratmakla ilişkilendiriliyor; Airbnb'nin "yabancının evinde kalma" gibi güven
gerektiren bir hizmette bu görsel dilin bilinçli bir tercih olduğu makul bir çıkarım (ama
Airbnb'nin kendisinin bunu bu şekilde açıkça gerekçelendirdiği bir kaynak bu araştırmada
bulunamadı). Radius'un bileşen türüne göre kademeli artması (buton en az, pill/daire en çok),
kullanıcının "bu ne kadar dokunsal/birincil bir eylem" sorusuna örtük bir görsel ipucu veriyor:
tam daire/pill şekiller (arama, kalp, orb) genelde tek-dokunuşluk, doğrudan ve sık kullanılan
eylemler, kartlar ise içerik konteynerları olarak orta düzey bir yumuşaklıkta kalıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir üründe köşe yuvarlaklığını rastgele/bileşen
bazında değil, "ne kadar sık/dokunsal etkileşim" ekseninde kademeli bir skala olarak tanımlamak
(ör. konteyner kartlar için küçük-orta radius, tek-dokunuşluk sık kullanılan eylem butonları için
tam pill/daire), görsel dile örtük bir hiyerarşi bilgisi kodluyor; kullanıcı bunu bilinçli olarak
fark etmese bile şekil dili tutarlı kaldığında ürün "düşünülmüş" hissettiriyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + ikincil kaynak özetinden.** Radius
değerleri (8px buton, 12-20px kart, tam pill arama çubuğu, tam daire kalp/orb, ~32px kategori
şeridi) tamamen WebSearch özetlerinde görülen üçüncü taraf design-token sitelerinden
(styles.refero.design, superdesign.dev, getdesign.md) geliyor, hiçbiri doğrudan fetch edilip
doğrulanmadı. "Yumuşak şekil = güven" gerekçesi ise Airbnb'nin kendi ifadesi değil, bu
araştırmacının genel tasarım prensiplerinden yaptığı bir çıkarım olarak işaretleniyor.

---

## 5. DLS'in temel çerçevesi: dört ilke ve "yaşayan organizma" komponent felsefesi

**Ne olduğu:** Doğrudan fetch edilen Karri Saarinen imzalı "Building a Visual Language" yazısına
göre Airbnb'nin Design Language System'i (DLS) dört temel ilke üzerine kuruldu: **Unified**
("her parça bütünün bir parçası, sistemin bütününe olumlu katkıda bulunmalı"), **Universal**
(ürünler küresel kullanıcıları karşılamalı ve erişilebilirliği önceliklendirmeli), **Iconic**
(çalışma, tasarım odağı ve kararları konusunda cesurca konuşmalı) ve **Conversational**
(motion, ürünleri net kullanıcı iletişimiyle hayata geçirir). Yazı ayrıca bileşenlerin klasik
"atomic design" mantığıyla (statik, sabit atomlar) değil, "bir arada var olabilen ve bağımsız
evrilebilen, yaşayan bir organizmanın parçaları" olarak tasarlandığını, sistemin önce iOS/Android
için kurulup sonra responsive prensiplerle tablet/web'e uyarlandığını ve "tüm bileşenlerin eşit
yaratılmadığını" (ör. satır/row gibi çok sık kullanılan bileşenlerin daha stratejik bir
pattern geliştirme süreci gerektirdiğini) anlatıyor.

**Nerede görülür:** İkisi de; DLS platform-agnostik bir sistem olarak tasarlandı, ilkeler hem
web hem iOS/Android bileşenlerine uygulanıyor.

**UX gerekçesi:** Bir şirket büyüdükçe (Airbnb'nin bu yazıyı yazdığı dönemde onlarca farklı takım
paralel olarak arayüz üretiyordu) tutarlılık kaybı riski büyüyor; dört ilkeyi (özellikle "Unified"
ve "Iconic") açıkça adlandırmak, farklı takımların "bu tasarım kararı sistemin bütününe
katkıda bulunuyor mu, yoksa kendi izole ihtiyacımızı mı çözüyor" sorusunu ortak bir dille
tartışabilmesini sağlıyor. Bileşenleri "atomik/sabit" değil "yaşayan organizma" olarak
kavramsallaştırmak, sistemin zamanla katılaşıp yeni ihtiyaçlara direnen bir kısıtlamaya
dönüşmesini önlemeyi hedefliyor; "her bileşen eşit değil" itirafı da (bazı bileşenlerin çok daha
fazla strateji/tekrar tasarım gerektirdiği) sistemin gerçekçi, aşırı-idealize edilmemiş bir
kendini değerlendirmesi.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir tasarım sistemi kurarken sadece bileşen
kütüphanesi (buton, kart, input) üretmek yerine, önce adlandırılmış, tartışılabilir ilkeler
(Airbnb'nin Unified/Universal/Iconic/Conversational'ı gibi) tanımlamak, ekiplerin gelecekteki
tasarım kararlarını bu ilkelere karşı test edebilmesini sağlıyor. Ayrıca sistemi "sabit atom seti"
değil "büyüyebilen/evrilen" bir çerçeve olarak kurmak, ürün büyüdükçe sistemin kırılganlaşmasını
azaltıyor; bazı bileşenlerin (en sık kullanılanların) diğerlerinden daha fazla özen/strateji
gerektirdiğini baştan kabul etmek de kaynak planlamasını gerçekçileştiriyor.

**Kaynak / güven notu:** **Doğrulandı**: dört ilke (Unified, Universal, Iconic, Conversational)
ve bunların birebir tanımları, "yaşayan organizma" komponent felsefesi, iOS/Android öncelikli
geliştirilip sonra tablet/web'e responsive uyarlanması, "her bileşen eşit yaratılmadı" itirafı,
doğrudan fetch edilen
https://medium.com/airbnb-design/building-a-visual-language-behind-the-scenes-of-our-airbnb-design-system-224748775e4e
(Airbnb Design'ın resmi Medium yayını, Karri Saarinen) yazısından birebir alındı. Yazı; tipografi,
renk, ikon, spacing ve bilgi mimarisini kapsayan bir "temel stil rehberi"nin var olduğunu
belirtiyor ama bu rehberin tam içeriğini (spesifik renk kodları, spacing adımları) paylaşmıyor,
dolayısıyla bu maddenin kendisi güçlü doğrulanmış olsa da, diğer maddelerdeki (2, 3, 4)
piksel-seviyesi detaylar bu yazıdan gelmiyor.

---

## 6. İkonografi: sade çizgi ikonlardan 2025'in "Lava" 3B animasyonlu ikon formatına

**Ne olduğu:** Airbnb uzun süre yalın, tek-renkli, ince çizgili (line icon) bir ikon dili
kullandı (kategori simgeleri, olanak ikonları, navigasyon ikonları gibi). WebSearch özetlerinde
görülen (doğrudan fetch edilerek doğrulanan bir teknik inceleme yazısına dayanan) bir gelişmeye
göre Airbnb, 2025 tarihli bir yeniden tasarımda "Lava" adını verdiği yeni bir ikon/animasyon
formatı tanıttı: saydam arka planlı, düşük dosya boyutlu, alfa kanallı bir video benzeri konteyner
içinde 3B render edilmiş, gölge/parlama gibi ışıklandırma efektleri taşıyan kısa animasyonlar.
Yazıya göre Lava, Lottie'nin (madde 8) sağlayamadığı 3B görsel zenginliği (ışıklandırma, gölgeleme,
perspektif) hedefliyor, ama tam etkileşimli 3B motorların (WebGL/Spline gibi) getirdiği
bellek/CPU/GPU yükünden kaçınıyor; tarayıcılar arası saydam video desteğinin tutarsız olması
(Safari'nin WebM desteklememesi gibi) nedeniyle Airbnb kendi özel decoder/player'ını geliştirip
web/iOS/Android'de "birebir aynı" görünüm sağlamayı hedefliyor.

**Nerede görülür:** İkisi de; kategori sekmeleri, olanak/özellik ikonları ve yeni nesil arayüz
elemanlarında (2025 sonrası) gözlemleniyor.

**UX gerekçesi:** Sade çizgi ikonlardan zengin, kısa-animasyonlu 3B ikonlara geçiş, ikonun sadece
"bir kategoriyi temsil eden pasif bir sembol" olmaktan çıkıp, kısa bir dikkat çekme/keyif anı
(micro-delight) üretmesini hedefliyor; bu, Saarinen'in DLS yazısındaki "Conversational" ilkesiyle
(motion'ın ürünleri net iletişimle hayata geçirmesi) örtüşen bir yön. Aynı zamanda saydam video
formatının platformlar arası tutarsızlığı çözmek için özel bir decoder geliştirilmesi, Airbnb'nin
görsel tutarlılığı (aynı ikonun iOS/Android/web'de birebir aynı görünmesi) mühendislik maliyetine
katlanarak bile önceliklendirdiğini gösteriyor; bu, Lottie'nin arkasındaki mantıkla (madde 8)
aynı: tasarımcının ürettiği zengin görsel, platform farklarına kurban edilmemeli.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir ürün ikon setini "durağan sembol" olmaktan
"kısa, anlamlı bir animasyon anı"na dönüştürmek isteyen ekipler için devredilebilir ilke şu:
zengin görsel format seçimi (3B render, saydam video gibi) platformlar arası tutarsızlık riski
taşıyorsa, bu riski platformun kendi decoder/player katmanını yazarak (Airbnb'nin Lottie ve
Lava'da yaptığı gibi) çözmek, tasarımcının vizyonunu platform kısıtlarına indirgemekten daha
sürdürülebilir bir yol. Ancak bu yatırım büyük mühendislik kaynağı gerektirdiğinden, küçük
ekipler için başlangıç noktası olarak zaten var olan açık kaynak Lottie'yi kullanmak (madde 8)
daha gerçekçi.

**Kaynak / güven notu:** Kısmen doğrulandı. Lava formatının teknik tanımı (saydam alfa kanallı
video benzeri konteyner, 3B render, Lottie'ye kıyasla farklandığı noktalar, platformlar arası
tutarlılık için özel decoder geliştirilmesi), doğrudan fetch edilen
https://medium.com/@waldobear002/airbnbs-new-lava-icon-format-a-technical-deep-dive-b2604626c7e0
yazısından geliyor, ancak bu **Airbnb'nin resmi bir yayını değil, bağımsız bir yazarın teknik
inceleme/reverse-engineering yazısı**; Airbnb'nin kendi resmi bir Lava duyurusu bu araştırmada
bulunup fetch edilmedi. Airbnb'nin önceki (Lava öncesi) sade çizgi ikon dili ise genel gözlem/
eğitim verisinden, ayrıca doğrulanmadı → bu madde bütünüyle **kısmen doğrulanmadı, tek ikincil
teknik kaynağa dayanıyor**.

---

## 7. Fotoğraf-öncelikli tasarım felsefesi: illüstrasyon/renk bloğu yerine gerçek, büyük fotoğraf

**Ne olduğu:** Airbnb arayüzünün baskın görsel unsuru neredeyse her zaman büyük, yüksek kaliteli
gerçek mekân/deneyim fotoğrafları; arayüz kromunun kendisi (madde 2'de anlatıldığı gibi) bilinçli
olarak nötr ve sade tutuluyor, boş alanlar illüstrasyon veya dekoratif renk bloğuyla değil büyük
oranda fotoğrafla dolduruluyor (ilan kartı görselleri, galeri, kategori kapak fotoğrafları, arama
sonucu arka planları). Airbnb'nin host'lara yönelik yardım/rehber içerikleri (bu maddede doğrudan
fetch edilmedi, sadece WebSearch özetlerinden görüldü) yüksek çözünürlük (min. 1024x683px), 3:2
en-boy oranı ve yatay çekim gibi somut fotoğraf gereksinimleri tanımlıyor; bu, fotoğrafın
platformun "ürünün kendisini gösterdiği" birincil kanal olarak ele alındığını gösteriyor.

**Nerede görülür:** İkisi de; en belirgin biçimde arama sonucu kartlarında (kartın çoğunluğu
fotoğraf), ilan detay sayfası galerisinde ve kategori/keşif sekmelerinin kapak görsellerinde.

**UX gerekçesi:** Airbnb'nin sattığı şey (bir mekânda, bir deneyimde geçirilecek zaman) doğası
gereği görsel ve duygusal bir karardır; kullanıcının bir ilanı "hayal edebilmesi" büyük ölçüde
fotoğrafın kalitesine ve boyutuna bağlı. Arayüz kromunu (renk, dekorasyon) bilinçli olarak
geri plana çekip fotoğrafa en büyük görsel alanı ve en yüksek kontrastı ayırmak (madde 2'deki
nötr-öncelik prensibiyle doğrudan bağlantılı), kullanıcının dikkatinin gerçek karar materyaline
(bu mekân nasıl görünüyor) gitmesini, arayüz süslemesine dağılmamasını sağlıyor. Bu aynı zamanda
illüstrasyon veya soyut ikonografiye kıyasla daha "dürüst" bir vaat: kullanıcı tam olarak neyi
göreceğinin fotoğrafını görüyor, stilize edilmiş bir temsilini değil.

**Airbnb dışı bir uygulamaya uyarlama notu:** Ürünün kendisi görsel/deneyimsel bir karar
gerektiren her platformda (emlak, seyahat, yemek, moda/e-ticaret), arayüz kromunu (renk,
dekorasyon, illüstrasyon) bilinçli olarak geri planda tutup en büyük ve en yüksek kaliteli görsel
alanı gerçek ürün/mekân fotoğrafına ayırmak, kullanıcının karar verme sürecini kısaltıyor.
Fotoğraf kalitesi için somut, ölçülebilir asgari standartlar (çözünürlük, en-boy oranı) tanımlamak,
bu felsefenin sadece bir tercih değil, platform genelinde uygulanan bir kalite kapısı olmasını
sağlıyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Fotoğrafın çözünürlük/en-boy oranı gereksinimleri
(1024x683px, 3:2 oran, yatay tercih) WebSearch özetlerinde host rehberi sitelerinden (smoobu.com,
airdna.co gibi ikincil kaynaklar) geldi, bu sayfalar doğrudan fetch edilip birebir doğrulanmadı.
"Fotoğraf-öncelikli, arayüz kromu nötr" genel felsefesi ise bu araştırmacının kendi gözlemi ve
madde 2/5'teki doğrulanmış kaynaklardan (Saarinen'in "Iconic" ilkesi, nötr renk paletinin genel
çerçevesi) yapılan bir çıkarım; Airbnb'nin kendisinin "fotoğrafı arayüzün önüne koyuyoruz" diye
açıkça yazdığı bir birincil kaynak bu araştırmada bulunamadı → **bu bir çıkarım, doğrudan
Airbnb ifadesi değil**.

---

## 8. Motion/micro-interaction ilkeleri: deklaratif animasyon çerçevesi ve shared element transition

**Ne olduğu:** Doğrudan fetch edilen "Motion engineering at scale" (Cal Stephens, Airbnb
Engineering) yazısına göre Airbnb iOS'ta, geleneksel "imperative" (elle, adım adım) UIView
animasyon koduna alternatif olarak **deklaratif bir geçiş animasyonu çerçevesi** geliştirdi:
mühendis sadece başlangıç durumu, bitiş durumu ve bir "geçiş tanımı" (transition definition)
belirtiyor, çerçevenin kendisi (genel bir `UIViewControllerAnimatedTransitioning` implementasyonu)
gerisini otomatik hallediyor. Bu çerçeve özellikle **shared element transition** (bir görselin bir
ekrandaki konumundan başka bir ekrandaki konumuna, boyutu/konumu akıcı şekilde değişerek
"aynı nesne" hissiyle geçmesi) için kullanılıyor; doğrudan fetch edilen "React Native at Airbnb"
yazısına göre bu, `<SharedElement>` adlı, Android ve iOS'ta native koda dayanan, hatta native ve
React Native ekranları arasında bile çalışabilen tek bir API'ye sarılmış. Doğrudan fetch edilen
"Bringing the Host Passport to Life" yazısı ise somut bir örnek veriyor: bir "host passport"
kartı arama sonuçlarındaki küçük halinden bir modal'a genişlerken, hem konum/boyut geçişi hem
kartın kendi iç 3B "sayfa çevirme" animasyonu senkronize ediliyor; bu senkronizasyon için özel
zamanlama eğrileri ve yay (spring) animasyonları ince ayarlanmış, geçiş bitişiyle kartın son
konumuna "tam zamanında" varması hedeflenmiş (erken/geç varış "sıçrama" veya "yavaşlık" hissi
yaratıyor).

**Nerede görülür:** İkisi de; iOS'ta native olarak, React Native ekranlarında da aynı
`<SharedElement>` API'siyle. En tipik kullanım örneği, bir ana sayfa/arama sonucu küçük
resminin ilan detay sayfasındaki tam genişlikte görsele dönüşmesi.

**UX gerekçesi:** Doğrudan fetch edilen kaynağa göre motive edici iki temel gerekçe var:
"akıcı geçişler kullanıcının bağlamı korumasına yardımcı oluyor" (bir kart tıklandığında ekran
aniden değişmek yerine, tıklanan görselin kendisinin genişleyerek detay sayfasına dönüşmesi,
kullanıcıya "az önce baktığım şey buydu" hissini kaybettirmiyor) ve "kısa animasyon flörtleri
uygulamayı canlı hissettiriyor". Deklaratif çerçevenin kendisi ayrı bir mühendislik gerekçesine
dayanıyor: özel geçişlerin daha önce "yüzlerce satır kırılgan, imperative kod" gerektirdiği,
bunu deklaratif bir API'ye indirgemenin hem daha az hataya açık olduğu hem de motion'ın daha
geniş bir mühendislik kitlesi tarafından benimsenmesini kolaylaştırdığı belirtiliyor; yani motion
kalitesi sadece bir tasarım kararı değil, motive edilen bir mühendislik-yatırımı sorunu olarak ele
alınıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir liste/kart görünümünden bir detay sayfasına
geçişte, ekranı aniden değiştirmek yerine tıklanan görselin kendisinin genişleyerek detay
görsele dönüşmesi (shared element transition), kullanıcının "az önce neye baktığımı"
hatırlamasını kolaylaştıran ucuz ama etkili bir bağlam-koruma tekniği. Bunu her ekran çiftinde
elle/imperative kodla yeniden yazmak yerine, "başlangıç durumu + bitiş durumu + geçiş tanımı"
alan tek, paylaşılan bir çerçeve/komponent (React Native/Compose/SwiftUI ekosistemlerinde bugün
hazır kütüphaneler de var) olarak soyutlamak, motion kalitesinin tek bir ekrana değil ürünün
tamamına yayılmasını kolaylaştırıyor.

**Kaynak / güven notu:** **Doğrulandı**: deklaratif animasyon çerçevesinin varlığı ve gerekçesi
(imperative kodun kırılganlığı, "bağlamı koruma" ve "uygulamayı canlı hissettirme" motivasyonları),
doğrudan fetch edilen https://medium.com/airbnb-engineering/motion-engineering-at-scale-5ffabfc878
yazısından birebir alındı. `<SharedElement>` API'sinin native/React Native arası paylaşımı,
doğrudan fetch edilen
https://medium.com/airbnb-engineering/react-native-at-airbnb-the-technology-dafd0b43838
yazısından geliyor. Host Passport'un sayfa-çevirme + shared element senkronizasyonu örneği,
doğrudan fetch edilen
https://medium.com/airbnb-engineering/animations-bringing-the-host-passport-to-life-on-ios-72856aea68a7
yazısından birebir alındı. Bu üç kaynak da Airbnb Engineering'in kendi resmi Medium yayını,
dolayısıyla bu bölümdeki en güçlü doğrulanmış madde. Genel bir "kart -> tam genişlik detay
görseli" shared element geçişinin bugünkü canlı üründe hâlâ birebir bu şekilde çalıştığı ayrıca
ekran kaydıyla doğrulanmadı; yazılar birkaç yıl öncesine ait mühendislik anlatıları.

---

## 9. Lottie: Airbnb'nin açtığı, sektör standardı haline gelen animasyon formatı

**Ne olduğu:** Doğrudan fetch edilen "Introducing Lottie" yazısına göre Lottie, Airbnb
Engineering'in açık kaynak olarak yayınladığı bir kütüphane: tasarımcıların After Effects'te
ürettiği animasyonlar, açık kaynak bir After Effects eklentisi olan **Bodymovin** ile JSON'a
export ediliyor, bu JSON dosyası da Lottie'nin iOS/Android/React Native/web kütüphaneleri
tarafından gerçek zamanlı olarak render ediliyor; yani animasyon, engineerin elle yeniden
kodlaması gereken bir şey değil, bir görsel varlık (asset) gibi native uygulamaya dahil ediliyor.
Yazıya göre öncesinde native uygulamalara karmaşık animasyon eklemenin iki kötü seçeneği vardı:
her ekran boyutu için ayrı, hacimli görsel dosyaları (GIF gibi) kullanmak ya da binlerce satır
kırılgan kod yazmak; bu maliyet çoğu ekibi animasyondan tamamen vazgeçirdiği için Lottie bu
sürtünmeyi ortadan kaldırmayı hedefliyor. Kütüphane ayrıca JSON'ın ağdan yüklenebilmesi (A/B test
için kullanışlı), sık kullanılan animasyonlar için önbellekleme, animasyonun bir gesture'a
(kaydırma gibi) bağlanabilmesi ve çalışma anında hız/renk gibi değerlerin değiştirilebilmesi gibi
özellikler sunuyor.

**Nerede görülür:** İkisi de; ayrıca web dahil (Lottie web render de destekliyor). Doğrudan fetch
edilen "React Native at Airbnb" yazısına göre Airbnb, Lottie'yi Android ve iOS'taki mevcut native
kütüphaneleri "sararak" (wrapping) React Native'de de çalışır hale getirdi.

**UX gerekçesi:** Bir tasarımcının After Effects'te ürettiği zengin bir animasyonun, mühendisin
elle yeniden kodlaması gerektiğinde iki şey kaybolur: (a) tasarımcının tam olarak istediği
zamanlama/eğri/detay genelde birebir yeniden üretilemez (yaklaşık bir "guesswork" ile taklit
edilir), (b) bu maliyet o kadar yüksektir ki çoğu ekip animasyonu baştan feragat eder. Lottie,
tasarım aracı (After Effects) ile üretim ortamı (native uygulama) arasındaki bu çeviri adımını
ortadan kaldırarak, tasarımcının ürettiği animasyonun **birebir** ekranda çıkmasını sağlıyor;
bu, tasarım-mühendislik işbirliğinde klasik bir "kayıpla çeviri" sorununu (design handoff
sürtünmesi) yapısal olarak çözüyor. JSON'ın ağdan yüklenebilmesi ayrıca animasyonu bir statik
varlık gibi değil, güncellenebilir/A-B test edilebilir bir içerik parçası haline getiriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Zengin motion tasarımı isteyen ama bunu her
platformda elle yeniden kodlamaya bütçesi olmayan her ekip için Lottie (bugün Airbnb dışında da
yaygın, açık kaynak, ücretsiz bir endüstri standardı) doğrudan kullanılabilir; özel bir "Airbnb
gibi format icat etme" ihtiyacı yok, zaten var olan bu kütüphaneyi benimsemek yeterli. Bu maddenin
asıl devredilebilir dersi teknik araç değil, prensip: tasarım aracındaki (After Effects, Figma
gibi) çıktının üretim ortamına "kayıpsız" aktarılabildiği bir format/araç zinciri kurmak, motion
kalitesinin tasarımcı vizyonuyla üretilen ürün arasında tutarlı kalmasını sağlıyor.

**Kaynak / güven notu:** **Doğrulandı**: Lottie'nin ne olduğu, Bodymovin ile After Effects->JSON
export akışı, iOS/Android/React Native/web'de gerçek zamanlı render, ağdan JSON yükleme/önbellek/
gesture-bağlama/çalışma anında değer değiştirme özellikleri, doğrudan fetch edilen
https://medium.com/airbnb-engineering/introducing-lottie-4ff4a0afac0e (Airbnb Engineering'in
resmi Medium yayını, Lottie'yi tanıtan orijinal yazı) yazısından birebir alındı. Lottie'nin
React Native'de mevcut native kütüphaneleri sararak çalıştırıldığı bilgisi de doğrudan fetch
edilen "React Native at Airbnb" yazısından geliyor. Lottie'nin bugün Airbnb dışında da (Facebook,
Google ve sayısız üçüncü taraf uygulamada) yaygın kullanıldığı iddiası ise genel
gözlem/eğitim verisinden, bu araştırmada ayrıca sayısal olarak doğrulanmadı.

---

## 10. Buton/CTA hiyerarşisi: cimri kullanılan dolu buton, geniş bir metin/ikincil buton katmanı

**Ne olduğu:** Topluluk gözlemine göre Airbnb arayüzünde birincil eylemler (Reserve/Rezervasyon
Yap, Ara, Devam Et gibi) genelde dolu (filled), yuvarlak/pill köşeli, ya marka rengi (Rausch,
madde 2) ya da koyu/siyah zeminli tek bir buton stiliyle gösteriliyor; ikincil eylemler ise
çoğunlukla dolu bir zemin almadan, sadece metin (text button) veya ince bir dış çizgi (outline)
ile gösteriliyor. WebSearch özetlerinde görülen bir üçüncü taraf analiz, birincil CTA için
~8px radius, tek bir dolu renk ve orta-ağır (medium/semibold) bir Cereal ağırlığı; ikincil eylemler
için ise dolu zemin olmadan, çoğunlukla koyu metin rengiyle gösterilen "yumuşak konteynerli" bir
stil öneriyor.

**Nerede görülür:** İkisi de; en belirgin örnek ilan detay sayfasındaki sticky rezervasyon
kutusunda "Reserve" butonunun dolu/vurgulu, yanındaki "Ask a question" gibi ikincil eylemlerin
metin-only olması; checkout akışında ise adım geçişlerinde benzer bir örüntü gözlemleniyor.

**UX gerekçesi:** Bir sayfada birden fazla eylem varsa (rezervasyon yap, host'a soru sor,
paylaş, kaydet), bunların hepsini aynı görsel ağırlıkla göstermek kullanıcıyı "hangisi asıl
amaç" konusunda tereddütte bırakır. Tek bir eylemi (genelde en yüksek iş değeri olan: rezervasyon
tamamlama) dolu/renkli/pill bir buton olarak işaretleyip geri kalan her şeyi metin/outline gibi
görsel olarak "geri planda" tutmak, sayfanın "birincil eylem hiyerarşisi"ni netleştiriyor; bu,
madde 2'deki "rengin cimri kullanımı" prensibiyle doğrudan tutarlı, çünkü marka renginin/dolu
zeminin en fazla bir-iki butonda görünmesi onu daha da belirgin kılıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir ekranda birden fazla eylem varsa, "bu sayfanın
tek bir asıl amacı ne" sorusunu netleştirip sadece o eylemi dolu/yüksek kontrastlı bir buton
olarak işaretlemek, diğer tüm eylemleri (paylaş, iptal, daha fazla bilgi gibi) metin veya outline
gibi düşük görsel ağırlıklı bir stille göstermek, kullanıcının karar yorgunluğunu azaltıyor.
Birden fazla dolu/renkli buton aynı ekranda yan yana durduğunda bu hiyerarşi kaybolur; bu yüzden
"dolu buton" stilini bilinçli olarak nadir/kıt bir kaynak gibi kullanmak önemli.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisi + ikincil kaynak özetinden.** Bu maddenin
somut piksel/renk detayları (8px radius, tam hex kodları, ağırlık isimleri) WebSearch
özetlerinde görülen üçüncü taraf design-token sitelerinden (designmd.cc, design-extractor.com)
geliyor, doğrudan fetch edilip doğrulanmadı. "Birincil dolu buton, cimri kullanım + geniş bir
metin/ikincil buton katmanı" genel örüntüsü ise bu araştırmacının kendi ürün gözleminden
(ekran görüntüsü/canlı ürün üzerinden ayrıca bu araştırmada teyit edilmedi) ve madde 2'deki
doğrulanmış "cimri renk kullanımı" ilkesinden yapılan bir çıkarım.

---

## 11. Dark mode: mobil uygulamalarda resmi destek yok, tekrarlanan bir kullanıcı talebi

**Ne olduğu:** WebSearch'te görülen çok sayıda Airbnb topluluk forumu (community.withairbnb.com)
gönderisine göre Airbnb'nin native iOS ve Android uygulamaları, cihazın sistem genelinde dark
mode'u olsa bile (Android'de yaklaşık 6 yıldır, iOS'ta yaklaşık 3 yıldır mevcut olan bir
platform özelliği), kalıcı olarak beyaz/açık temada kalıyor; resmi bir dark mode ayarı
sunmuyor. Kullanıcılar bu forumlarda tekrar tekrar (yıllar içinde birden fazla ayrı başlıkta)
gece kullanımında göz yorgunluğu gibi gerekçelerle dark mode talep ediyor. Bazı kullanıcılar
Android'in sistem genelindeki "force dark" gibi işletim sistemi seviyesinde zorlama
özelliklerini bir geçici çözüm olarak kullanabiliyor; web tarafında ise tarayıcı seviyesinde
(ör. Chrome'un kendi sayfa-zorla-karartma özelliği) benzer geçici çözümler mümkün, ama bunların
hiçbiri Airbnb'nin kendi resmi bir dark mode implementasyonu değil.

**Nerede görülür:** İkisi de (mobil native uygulamalar ve web, resmi bir dark mode sunmama
durumu ikisi için de geçerli görünüyor); tarayıcı/OS seviyesinde zorlama geçici çözümleri ayrı
bir konu.

**UX gerekçesi:** Airbnb'nin dark mode'u önceliklendirmemesinin resmi bir gerekçesi bu
araştırmada bulunamadı, ama makul bir çıkarım: Airbnb'nin görsel kimliği büyük ölçüde
gerçek fotoğrafların (madde 7) yüksek kaliteli, doğru renkte gösterilmesine dayanıyor; bir
dark mode implementasyonu sadece arayüz kromunu (metin, arka plan, buton) değil, binlerce
farklı host tarafından yüklenen, farklı ışık koşullarında çekilmiş fotoğrafın koyu bir zemin
üzerinde nasıl algılanacağını da yeniden düşünmeyi gerektirir; bu, salt renk paleti tersine
çevirmekten daha karmaşık bir tasarım problemi olabilir. Ancak bu sadece bir yorum, Airbnb'nin
kendi resmi bir açıklaması değil.

**Airbnb dışı bir uygulamaya uyarlama notu:** Ürünün görsel kimliği büyük ölçüde kullanıcı
üretimi/üçüncü taraf fotoğraflara dayanıyorsa (emlak, seyahat, e-ticaret, yemek), dark mode
kararını sadece bir renk paleti tersine çevirme işi olarak değil, "bu fotoğraflar koyu bir
arka plan üzerinde nasıl görünecek, kontrast/okunabilirlik korunacak mı" sorusunu da kapsayan
ayrı bir tasarım problemi olarak ele almak gerekiyor. Kullanıcı talebinin (Airbnb örneğinde
yıllardır süren, tekrarlanan forum talepleri) yüksek olması, bir özelliğin uygulanma
maliyetinin/karmaşıklığının onu geciktirebileceğinin de bir hatırlatıcısı.

**Kaynak / güven notu:** Kısmen doğrulandı. Airbnb'nin native uygulamalarında resmi dark mode
desteği olmadığı iddiası, Airbnb'nin kendi resmi bir sayfasından değil, **çok sayıda ama resmi
olmayan kullanıcı forumu gönderisinden** (community.withairbnb.com, birden fazla ayrı başlık,
yıllar içinde tekrarlanan aynı talep) geliyor; bu sayfalar doğrudan fetch edilip birebir
okunmadı, sadece WebSearch özetleri kullanıldı. Çoklu ve tutarlı kaynak olması (aynı iddiayı
yıllar içinde birden fazla bağımsız forum başlığı doğruluyor) güven düzeyini tek bir kaynağa
göre artırıyor, ama yine de **Airbnb'nin kendi resmi bir "dark mode yok" açıklaması bu
araştırmada bulunup fetch edilmedi**, dolayısıyla kısmen doğrulanmış olarak işaretleniyor.
"Dark mode'un fotoğraf kontrastı sorunuyla ilişkisi" gerekçesi ise açıkça **bu araştırmacının
kendi çıkarımı, Airbnb'nin ifadesi değil**.

---

## 12. Erişilebilirlik taahhüdü: çapraz-fonksiyonel ekip, WCAG standardı, VoiceOver/TalkBack testi

**Ne olduğu:** Doğrudan fetch edilen Airbnb yardım merkezi sayfasına (help/article/2166) göre
Airbnb, dijital erişilebilirlik taahhüdünü üç şekilde tanımlıyor: (1) mühendis, tasarımcı ve
teknik program yöneticilerinden oluşan **çapraz-fonksiyonel, özel bir ekip** ("herkesin
kullanabileceği ürünler inşa etmeye odaklanan"), (2) **Web Content Accessibility Guidelines
(WCAG)**'de belirlenen geliştirme standartlarına uyma taahhüdü (sayfa, spesifik bir uygunluk
seviyesini, ör. "AA", açıkça belirtmiyor), (3) erişilebilirlik ihtiyacı olan kişilerle
doğrudan **araştırma** yapılması ve bu kişilerin "uzmanlığı ve yaşanmış deneyiminin" ürün
geliştirmeyi etkilemesi. Sayfa ayrıca "Airbnb'yi daha erişilebilir kılmaya devam ediyoruz"
diyerek bunun tamamlanmış değil süregelen bir çalışma olduğunu belirtiyor. WebSearch özetlerinde
ayrıca (bu sayfadan değil, başka bir ikincil kaynaktan) VoiceOver (masaüstü Safari/macOS, mobil
Safari/iOS ve native uygulama) ve TalkBack (mobil Chrome/Android ve native uygulama) ile düzenli
test yapıldığı ve WCAG 2.1 AA seviyesi ile Avrupa Erişilebilirlik Yasası'na uyum hedeflendiği
iddia ediliyor; bu spesifik detay doğrudan fetch edilen birincil kaynakta görülmedi.

**Nerede görülür:** İkisi de; hem web hem native mobil uygulamalar için geçerli bir kurumsal
taahhüt olarak sunuluyor.

**UX gerekçesi:** Erişilebilirlik taahhüdünü somut bir ekip yapısına (çapraz-fonksiyonel, özel
atanmış) ve dışsal bir standarda (WCAG) bağlamak, bunu "iyi niyetli ama ölçülemez bir söz"
olmaktan çıkarıp, en azından prensipte denetlenebilir bir sorumluluk haline getiriyor. Doğrudan
erişilebilirlik ihtiyacı olan kullanıcılarla araştırma yapılmasının ayrıca belirtilmesi, ekibin
sadece standart-uyumluluğu (checklist mantığı) değil, gerçek kullanıcı deneyimini de ölçüt
aldığını ima ediyor; standart uyumluluğu (WCAG) genelde teknik doğruluğu garanti eder ama
kullanılabilirliği garanti etmez, bu boşluğu doğrudan kullanıcı araştırmasıyla kapatmaya
çalışmak daha bütüncül bir yaklaşım.

**Airbnb dışı bir uygulamaya uyarlama notu:** Erişilebilirlik taahhüdünü kurumsal bir sayfada
duyururken, sadece bir standart adı (WCAG, ADA gibi) anmak yerine, bunu hangi somut yapının
(özel, çapraz-fonksiyonel bir ekip) sürdürdüğünü ve standart-uyumluluğunun ötesinde gerçek
kullanıcı araştırmasıyla (erişilebilirlik ihtiyacı olan kişilerle doğrudan çalışarak) nasıl
desteklendiğini belirtmek, taahhüdü daha inandırıcı kılıyor. Ayrıca hangi ekran okuyucu/
platform kombinasyonlarının (VoiceOver+Safari, TalkBack+Chrome gibi) somut olarak test
edildiğini açıkça listelemek (Airbnb'nin ikincil kaynaklarda görülen ama birincil kaynakta teyit
edilemeyen pratiği), "erişilebilirlik" gibi soyut bir sözü somut bir test matrisine indirgiyor.

**Kaynak / güven notu:** Kısmen doğrulandı. Çapraz-fonksiyonel ekip yapısı, WCAG standardına
uyma taahhüdü (spesifik seviye belirtilmeden), erişilebilirlik ihtiyacı olan kullanıcılarla
araştırma yapılması ve bunun "devam eden bir çalışma" olarak çerçevelenmesi **doğrulandı**,
doğrudan fetch edilen https://www.airbnb.com/help/article/2166 sayfasından birebir alındı.
Ancak WebSearch özetlerinde görülen "WCAG 2.1 Level AA" spesifik uygunluk seviyesi ve
"Avrupa Erişilebilirlik Yasası'na uyum" iddiası, ile VoiceOver/TalkBack'in hangi tarayıcı/
cihaz kombinasyonlarında test edildiğine dair detay, **doğrudan fetch edilen birincil kaynakta
görülmedi**, sadece WebSearch'ün ikincil bir özetinde geçti (muhtemelen airbnb.com/accessibility
gibi başka bir sayfadan); bu sayfayı ayrıca fetch etmeye çalıştım ama **403 Forbidden** hatası
aldım → bu spesifik alt-detaylar **kısmen doğrulanmadı**.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip birincil/güçlü içerik olarak doğrulanan kaynaklar (Airbnb'nin kendi
  Medium yayınları ve yardım merkezi):** "Building a Visual Language" (Karri Saarinen, DLS'in
  dört ilkesi), "Working Type" (Cereal tipografisi), "Introducing Lottie", "React Native at
  Airbnb: The Technology", "Motion engineering at scale" (Cal Stephens), "Animations: Bringing
  the Host Passport to Life on iOS" (Anne Lu), erişilebilirlik yardım merkezi sayfası
  (help/article/2166). Bu 7 kaynak, 12 pattern'den 4'ünü (madde 1, 5, 8, 9) güçlü ve doğrudan;
  2'sini (madde 8'in shared element kısmı, madde 12) kısmen destekliyor.
- **Fetch edilip okunan ama Airbnb'nin resmi yayını olmayan (bağımsız/ikincil teknik) kaynak:**
  Lava ikon formatı üzerine bir teknik inceleme yazısı (waldobear002, Medium) - detaylı ve
  teknik olarak tutarlı ama Airbnb'nin kendi resmi duyurusu değil.
- **Fetch denemesi yapılıp erişilemeyen kaynak:** airbnb.com/d/accessibility (403 Forbidden,
  sadece WebSearch özeti kullanılabildi).
- **Hiçbir birincil kaynakla doğrulanamayan, büyük ölçüde üçüncü taraf "design token
  extractor" sitelerinin WebSearch özetlerine dayanan maddeler:** renk hex kodları ve tam
  kullanım kuralları (madde 2), spacing/grid tam değerleri (madde 3), köşe yuvarlaklığı/radius
  değerleri (madde 4), buton/CTA piksel detayları (madde 10). Bu dört madde, bu araştırmada
  hiçbir noktada doğrudan fetch edilerek doğrulanmadı; Airbnb'nin kendi resmi, genel-kullanıma
  açık bir tasarım token dokümantasyonu (Material Design'ın Google'da olduğu gibi) bulunamadı.
  Dark mode durumu (madde 11) ise çok sayıda ama resmi olmayan kullanıcı forumu gönderisine
  dayanıyor, tutarlı ama birincil değil.
- Toplamda 12 pattern'den **4 tanesi** doğrudan Airbnb birincil kaynağıyla güçlü doğrulandı,
  **4 tanesi** kısmen doğrulandı (gerçek kaynak fetch edildi ama ikincil kaynak, kısmi içerik
  veya bağımsız teknik blog), **4 tanesi** (renk hex/kullanım kuralları, spacing, radius,
  buton piksel detayları) büyük ölçüde doğrulanmadı/eğitim verisi + ikincil kaynak özetinden.
  Bu bölüm, diğer bölümlere (özellikle 05-trust-safety-signals) kıyasla daha zayıf bir birincil
  kaynak tabanına sahip; bunun nedeni Airbnb'nin görsel tasarım sistemini yardım merkezi gibi
  halka açık, resmi bir dokümantasyon kanalında değil, büyük ölçüde zaman içinde dağınık
  mühendislik/tasarım blog yazılarında paylaşmış olması ve tam bir güncel stil rehberinin (token
  listesi) hiç kamuya açık şekilde yayınlanmamış olması. Canlı ürün sürekli değiştiğinden (2025
  Lava ikon geçişi gibi), bu doküman da "şu an tam olarak böyle" değil "tekrar gözlemlenen/
  belgelenen yerleşik pattern" çerçevesiyle okunmalı.
