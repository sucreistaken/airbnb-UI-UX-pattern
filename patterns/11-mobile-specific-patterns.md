# 11. Mobile-Only Pattern'ler (Mobile-Specific Patterns)

Diğer bölümlerin çoğu (07-navigation-ia, 09-states-feedback dahil) web ve mobili yan yana ele
alırken, bu bölüm kasıtlı olarak sadece native mobil uygulamada (iOS/Android) var olan ya da
web'de anlamlı biçimde farklı çalışan pattern'lere odaklanıyor: kart/foto kaydırma gestureları,
haptic (dokunsal) geri bildirim, bottom sheet derinliği/istifleme, native paylaşım sayfası,
kamera/foto kütüphanesi entegrasyonu, biyometrik oturum açma, native harita SDK'sı, push bildirim
izni zamanlaması, ana ekran widget'ı, çevrimdışı/düşük bağlantı davranışı, mağaza değerlendirme
istemi zamanlaması ve Airbnb'nin React Native'i önce benimseyip sonra terk etmesinin mimari
mirası. Projenin kullanıcısı mobil tarafın diğer bölümlerde gölgede kalmaması gerektiğini
birden fazla kez vurguladığı için, bu bölüm bilinçli bir sentez turu: web karşılığı olmayan ya
da web'den kökten farklı çalışan davranışlara odaklanıyor, web ile ortak olan genel mobil
navigasyon pattern'leri (alt tab bar, arama-öncelikli IA gibi) zaten 07 numaralı bölümde işlendi,
burada tekrarlanmıyor.

Araştırma sırasında fiilen fetch edilip okunan kaynaklar: Airbnb Engineering'in Medium
yayınından "React Native at Airbnb" ve "Sunsetting React Native" (mimari hikaye), "AirMapView:
A View Abstraction for Maps on Android" (native harita SDK stratejisi); Airbnb yardım
merkezinden hesaba giriş (help/article/3530, Face ID/Touch ID), iki faktörlü kimlik doğrulama
(help/article/2842, cihaz biyometrisi), kimlik doğrulama (help/article/1237, selfie/belge
fotoğrafı) ve yüz tanıma teknolojisi (help/article/3564, biyometrik şablon); bağımsız tasarımcı
Arlen McCluskey'nin Airbnb foto galerisi yeniden tasarımı vaka çalışması (swipe davranışı
araştırması); ve iClarified'ın 2015 tarihli Airbnb uygulama güncellemesi haberi (çevrimdışı
erişim özelliğinin duyurulması). Bunların dışında kalan pek çok madde (haptic feedback'in
spesifik kullanım noktaları, native paylaşım sayfası, widget varlığı, push bildirim izni
zamanlaması, mağaza değerlendirme istemi zamanlaması, host foto yükleme akışının kamera-only
kısıtlaması, mesaj arşivleme swipe gesture'ı) bu araştırmada Airbnb'nin resmi bir kaynağından
doğrudan fetch edilerek doğrulanamadı; WebSearch özetlerinde ikincil kaynaklardan görüldü ya da
hiç kaynak bulunamadı, ve ilgili maddelerde bu açıkça "doğrulanmadı" olarak işaretlendi. Önemli
bir metodolojik not: Airbnb'nin resmi App Store sayfası (apps.apple.com) da doğrudan fetch
edildi ve widget, share sheet, Face ID gibi native özelliklere dair uygulama açıklamasında hiçbir
açık referans bulunamadı; bu, "özellik yok" anlamına gelmiyor (App Store açıklamaları genelde
böyle teknik detayları listelemez), sadece bu araştırma yönteminin bu soruyu kesin olarak
cevaplayamadığı anlamına geliyor.

---

## 1. React Native'in benimsenip sonra terk edilmesi: iOS/Android tutarlılığının mimari kökeni

**Ne olduğu:** Airbnb 2016'da mobil uygulamasının büyük bir kısmını React Native ile yazmaya
karar verdi; amaç iOS ve Android için ürün kodunu iki kere değil bir kere yazmaktı. Doğrudan
fetch edilen "React Native at Airbnb" yazısına göre bu kararın arkasındaki asıl motivasyon
kaynak kısıtlıydı: "yeterli mobil mühendisimiz yoktu" hedeflere ulaşmak için. React Native
sayesinde "Experiences" gibi tamamen yeni bir iş kolu ve düzinelerce özellik (review'lardan
hediye kartlarına) native mühendislik kapasitesi yetersizken geliştirilebildi. Ancak 2018'de
doğrudan fetch edilen "Sunsetting React Native" yazısına göre Airbnb bu kararı tersine çevirdi:
"çeşitli teknik ve organizasyonel sorunlar" nedeniyle tüm yeni React Native özelliklerini
durdurdu ve en yüksek trafikli ekranları native'e geri taşımaya başladı. Yazıya göre asıl
sorun şuydu: uygulamanın "sadece küçük bir yüzdesi" gerçekten React Native'di, geri kalanı için
"büyük miktarda köprüleme (bridging) altyapısı" gerekiyordu; bu da Airbnb'yi iki platform yerine
"üç platformu birden desteklemek" durumunda bıraktı (native iOS, native Android, React Native).

**Nerede görülür:** İkisi de (iOS ve Android); bu madde bir kullanıcı arayüzü pattern'i değil,
bugün iOS ve Android uygulamalarının neden bazı ekranlarda birebir aynı, bazılarında farklı
davrandığının mimari/tarihsel açıklaması. Bu bölümdeki diğer maddelerin (native harita SDK'sı,
biyometrik oturum açma gibi) her platformda "native" olarak uygulanmasının arka planında da
aynı 2018 sonrası "native öncelikli" mühendislik kültürü yatıyor.

**UX gerekçesi:** Bir ürünün iOS ve Android sürümlerinin ne kadar "tutarlı" hissettirdiği,
büyük ölçüde hangi mimari yaklaşımın (paylaşılan kod tabanı vs iki ayrı native kod tabanı)
seçildiğine bağlı. React Native teorik olarak tutarlılığı artırması beklenen bir araçtı (tek
kod tabanı, iki platformda çalışır), ama doğrudan fetch edilen yazıya göre pratikte "async
first render" gibi teknik kısıtlar kaliteyi hedeflenen seviyede tutmayı zorlaştırdı ve
geliştirici deneyimi "karışık bir sonuç" verdi (build süreleri iyileşti, hata ayıklama
kötüleşti). Airbnb'nin bunu terk edip native'e dönmesi, "aynı görünüp aynı hissetmek" hedefinin
paylaşılan kod yerine platforma özgü ama ortak bir tasarım dili (Design Language System, bkz.
08-visual-design-system.md) ile de sağlanabileceğini gösteren bir örnek: tutarlılık kod
paylaşımından değil, ortak tasarım kurallarından (renk, tipografi, motion ilkeleri, bileşen
davranışı) gelebiliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kısıtlı mobil mühendislik kapasitesiyle hem iOS
hem Android'de hızlı özellik teslim etmek isteyen her ekip için, cross-platform bir framework
(React Native, Flutter gibi) cazip bir kısayol olabilir, ama Airbnb'nin deneyimi şunu gösteriyor:
bu kısayolun maliyeti zamanla "üç kod tabanını bakımda tutma" riskine dönüşebilir (native
altyapı + cross-platform katman + bunlar arasındaki köprü). Bir ekip bu yolu seçerse, "ne kadarı
gerçekten paylaşılan kod, ne kadarı köprüleme/native modül" oranını erken ve düzenli olarak
ölçmesi, Airbnb'nin 2 yıl sonra fark ettiği "sadece küçük bir yüzde gerçekten paylaşılıyor"
sürprizini önceden yakalayabilir. Platformlar arası tutarlılığı kod paylaşımı yerine ortak bir
tasarım sistemiyle (bileşen kütüphanesi, motion ilkeleri) sağlamak, daha az kırılgan bir
alternatif olabilir.

**Kaynak / güven notu:** **Doğrulandı.** Hem 2016 benimseme kararının gerekçeleri (mühendis
kıtlığı, dört hedef: hız/kalite/tek seferde yazma/geliştirici deneyimi, Experiences iş
kolunun React Native ile inşası) hem de 2018 terk kararının gerekçeleri ("çeşitli teknik ve
organizasyonel sorunlar", async first render zorluğu, "sadece küçük bir yüzde React Native",
"üç platform desteği", karışık geliştirici deneyimi), doğrudan fetch edilen
https://medium.com/airbnb-engineering/react-native-at-airbnb-f95aa460be1c ve
https://medium.com/airbnb-engineering/sunsetting-react-native-1868ba28e30a (Airbnb Engineering'in
resmi Medium yayını, Gabriel Peal imzalı) yazılarından birebir alındı. Bu, bu bölümdeki en güçlü
birincil kaynaklı madde. Yazıların 2018 tarihli olduğu ve Airbnb'nin bugünkü (2026) mimarisinin
o zamandan beri değişmiş olabileceği (ör. daha yeni bir cross-platform denemesi başlatılmış
olabilir) bu araştırmada ayrıca güncellenmedi; "native'e dönüş" kararının 2018'den bugüne
sürdüğü varsayımı doğrulanmadı, sadece o tarihteki karar doğrulandı.

---

## 2. Biyometrik oturum açma: parola yerine Face ID / Touch ID ile hesaba giriş

**Ne olduğu:** Doğrudan fetch edilen Airbnb yardım merkezi sayfasına göre, "Sign in with Apple"
akışının bir parçası olarak Airbnb kullanıcıya parola girmek yerine Face ID veya Touch ID ile
giriş yapma seçeneği sunuyor: "Instead of entering a password, this feature lets you use Face ID
or Touch ID to log in." Ancak bu özellik **sadece yeni hesaplar için** kullanılabiliyor, mevcut
eski hesaplara sonradan eklenmiyor.

**Nerede görülür:** iOS (Face ID/Touch ID, Apple'ın "Sign in with Apple" çerçevesi üzerinden);
Android tarafında karşılığının (fingerprint/biyometrik donanım üzerinden benzer bir akış)
var olup olmadığı bu araştırmada doğrudan doğrulanmadı, ama madde 3'teki iki faktörlü
doğrulama sayfası "yüz tanıma, parmak izi veya şifre" ifadesiyle bunun platform-agnostik
(hem iOS hem Android cihaz biyometrisi) bir kavram olarak tasarlandığını ima ediyor.

**UX gerekçesi:** Parola girmek, özellikle mobil bir klavyede, sürtünmeli bir eylem: kullanıcı
parolasını hatırlamalı, doğru yazmalı, otomatik doldurma çalışmazsa manuel girmeli. Cihazın
zaten donanımsal olarak sunduğu biyometrik kimlik doğrulamayı (Face ID/Touch ID) uygulamanın
kendi oturum açma akışına bağlamak, kullanıcıya "cihazını nasıl açıyorsan hesabını da öyle aç"
tutarlılığı sunuyor; bu, öğrenilmesi gereken yeni bir eylem değil, zaten günde onlarca kez
tekrarlanan bir kas hafızasının (telefonu yüzle/parmakla açma) yeniden kullanılması. Bunun
"sadece yeni hesaplar" ile sınırlı olması, muhtemelen geriye dönük bir güvenlik/uyumluluk
kısıtı (eski hesapların bu akışa güvenli biçimde bağlanması için ek bir doğrulama adımı
gerekebilir) ile ilgili, ama bu sayfanın kendisinde açıkça gerekçelendirilmiyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Sık kullanılan bir mobil uygulamada, kullanıcıyı
her oturum açışında parola yazmaya zorlamak yerine cihazın kendi biyometrik donanımına (Face
ID/Touch ID/parmak izi) bağlı bir oturum açma seçeneği sunmak, sürtünmeyi azaltıyor ve
(cihaz üreticisinin güvenlik standartlarına güvenildiği için) güvenliği de zayıflatmıyor. Bunu
"sadece yeni hesaplar" gibi bir kısıtla sınırlı tutmak istemiyorsanız, mevcut kullanıcılara
bu özelliği sonradan bir kerelik ek bir doğrulama adımıyla (ör. mevcut parolayla bir kez giriş
yapıp sonra biyometriye geçiş) açmak, hem güvenliği koruyor hem de eski kullanıcıları
dışarıda bırakmıyor.

**Kaynak / güven notu:** **Doğrulandı** (özelliğin varlığı ve "sadece yeni hesaplar" kısıtı
için). "Instead of entering a password, this feature lets you use Face ID or Touch ID to log
in. It's only available for new accounts." ifadesi doğrudan fetch edilen
https://www.airbnb.com/help/article/3530 (Airbnb'nin kendi yardım merkezi) sayfasından birebir
alındı. Ancak bu özelliğin Android tarafındaki tam karşılığı, kullanıcı arayüzünün görsel akışı
(ekran görüntüsü) ve "neden sadece yeni hesaplar" sorusunun gerekçesi bu araştırmada
doğrulanmadı.

---

## 3. İki faktörlü kimlik doğrulamada cihaz biyometrisiyle yeniden kimlik doğrulama

**Ne olduğu:** Doğrudan fetch edilen Airbnb yardım merkezi sayfasına göre, bir kullanıcı iki
faktörlü kimlik doğrulamayı (2FA) kurduktan sonra, sonraki girişlerde "cihazınızı açtığınız
aynı yöntemle" (yüz tanıma, parmak izi veya şifre) kimlik doğrulaması yapabiliyor: "you may be
able to authenticate your login in the same way you unlock your device, whether that's with
face recognition, your fingerprint, or passcode." Ancak bu, **yeni bir cihazdan ilk kez** giriş
yapılıyorsa çalışmıyor; o durumda kullanıcı SMS/telefon üzerinden gelen bir doğrulama kodu ve
PIN kullanmak zorunda kalıyor.

**Nerede görülür:** İkisi de (iOS Face ID/Touch ID, Android parmak izi/yüz tanıma); bu, madde
2'deki "Sign in with Apple" özelinden farklı olarak, Avrupa Ekonomik Alanı'ndaki host'lar için
zorunlu olan Strong Customer Authentication (SCA) uyumlu genel 2FA akışının bir parçası.

**UX gerekçesi:** İki faktörlü doğrulama, güvenlik için ek bir adım ekliyor, ama bu ek adımın
**her girişte** SMS kodu beklemek anlamına gelmesi kullanıcı deneyimini ciddi biçimde
yavaşlatabilir. Cihazın zaten güvenilir kabul edilen kendi biyometrik doğrulamasını ikinci
faktör olarak kabul etmek (cihaz zaten kilitliyken ve kullanıcı onu açabiliyorsa, bu "sen busun"
sinyalinin bir kanıtı sayılıyor), güvenlik seviyesini düşürmeden tekrarlayan sürtünmeyi ortadan
kaldırıyor. Bunun "yeni cihaz" durumunda çalışmaması mantıklı bir güvenlik sınırı: yeni bir
cihazın biyometrik sensörü, o cihazın gerçek sahibinin hesap sahibiyle aynı kişi olduğunu henüz
kanıtlamıyor, bu yüzden orada daha güçlü bir kanal (SMS + PIN, hesap sahibinin telefon
numarasına bağlı) devreye giriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** İki faktörlü doğrulama uygulayan her mobil
uygulamada, "güvenilir cihaz" kavramını tanımlayıp bu cihazlarda ikinci faktörü cihazın kendi
biyometrisiyle (SMS kodu beklemeden) sağlamak, güvenliği SMS'in tek başına sunduğundan daha
zayıflatmadan (biyometri zaten cihaz kilidinin arkasında) tekrarlayan sürtünmeyi ortadan
kaldırıyor. Bu güveni sadece **daha önce doğrulanmış cihazlarla** sınırlı tutmak (yeni cihazda
güçlü kanala geri dönmek) kritik bir güvenlik sınırı: biyometrik kısayol, cihaz kimliğinin
kendisi zaten önceden doğrulanmışsa güvenli, yeni/bilinmeyen bir cihazda değil.

**Kaynak / güven notu:** **Doğrulandı.** Cihaz biyometrisiyle yeniden doğrulama ifadesi
("face recognition, your fingerprint, or passcode") ve yeni cihazda bu kısayolun çalışmadığı
("If you're logging in from a new device for the first time, this feature won't be
available") kısıtı, doğrudan fetch edilen https://www.airbnb.com/help/article/2842 sayfasından
birebir alındı; aynı sayfa SCA/EEA host zorunluluğunu da doğruluyor. Bu akışın Android'deki
tam görsel/etkileşimsel karşılığı (ekran görüntüsü) ayrıca doğrulanmadı.

---

## 4. Kimlik doğrulama akışı: hükümet kimliği + canlı selfie ile yüz eşleştirme

**Ne olduğu:** Doğrudan fetch edilen iki Airbnb yardım merkezi sayfasına göre, kimlik
doğrulama süreci kullanıcıdan iki fotoğraf istiyor: bir hükümet kimliği (ehliyet, pasaport,
ulusal kimlik kartı, vb., çift taraflıysa her iki yüz de) ve bir "canlı" selfie ("it must be
taken of you at the moment of submission and not be a previous photo of you", yani galeriden
eski bir fotoğraf yüklenemiyor, o an kamerayla çekilmesi gerekiyor). Kimlik belgesinin süresi
dolmamış ve orijinal (fotokopi/PDF/dijital kopya değil) olması gerekiyor. Sistem bu iki
fotoğraftan "benzersiz bir yüz tanımlayıcısı" (unique facial identifier) üretip otomatik olarak
eşleştirmeyi deniyor; eşleşme otomatik olarak sağlanamazsa bir insan inceleme ekibi manuel
olarak karşılaştırıyor.

**Nerede görülür:** İkisi de (iOS/Android kamera erişimi gerektiren native akış); doğrudan
fetch edilen sayfaya göre "Airbnb app or your browser may need permission to access your
camera", yani hem mobil uygulama hem mobil web bu kamera izin isteğini tetikleyebiliyor, ama
native uygulamada bu, işletim sisteminin kendi kamera izin diyaloğu (iOS'ta "Airbnb, fotoğraf
çekmek için kameranıza erişmek istiyor" gibi bir sistem promptu) üzerinden gerçekleşiyor.

**UX gerekçesi:** Bir kimlik doğrulama sisteminin en kritik tasarım kararı, "gerçekten o anda,
gerçekten o kişi mi çekiyor" sorusunu nasıl cevapladığı. Selfiyi galeriden değil sadece o an
kamerayla çektirmek (eski bir fotoğrafın kullanılmasını engellemek), sahte kimlik doğrulama
girişimlerine karşı temel bir savunma katmanı: kullanıcı arayüzünün kendisi (kameraya
"galeriden seç" seçeneği sunmaması) bu güvenlik kısıtını, kullanıcıya ayrı bir kural olarak
anlatmadan, doğrudan etkileşim tasarımıyla dayatıyor. Otomatik yüz eşleştirmenin başarısız
olduğu durumda insan incelemeye düşmesi (tek bir "reddedildi" sonucu yerine), algoritmanın
kusurlu olabileceğini (yardım merkezi sayfasının kendi ifadesiyle "no facial matching process
is entirely accurate") kabul eden bir tasarım: kullanıcı aydınlatma koşulları veya görünüm
değişikliği yüzünden haksız yere reddedilmiyor, bir insan ikinci bir bakış atıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Kimlik doğrulama gerektiren her uygulamada
(finans, kısa dönem kiralama, ikinci el pazaryeri), selfiyi galeriden yükletmek yerine sadece
canlı kamerayla çektirmek, sahtecilik riskini önemli ölçüde azaltıyor; bu kısıtı kullanıcıya
bir metin kuralı olarak değil, arayüzün kendisinden "galeriden seç" seçeneğini kaldırarak
dayatmak daha güvenilir. Otomatik biyometrik eşleştirmenin bir "hayır" durumunda kullanıcıyı
tamamen reddetmek yerine bir insan inceleme katmanına düşürmesi, algoritmanın yanlış negatif
verme ihtimaline karşı bir güvenlik ağı sağlıyor; bu, özellikle düşük ışık/kamera kalitesi
gibi kullanıcının kontrolünde olmayan faktörlerin mağduriyet yaratmaması için önemli.

**Kaynak / güven notu:** **Doğrulandı.** Selfie'nin "o an çekilmesi gerektiği, önceki bir
fotoğraf olamayacağı" kısıtı, kimlik belgesi gereksinimleri (tür, çift taraf, orijinal/süresi
dolmamış olma şartı) ve kamera izni gerekliliği, doğrudan fetch edilen
https://www.airbnb.com/help/article/1237 sayfasından birebir alındı. Otomatik yüz
eşleştirmenin "benzersiz yüz tanımlayıcısı" ürettiği, eşleşme sağlanamazsa insan incelemesine
düştüğü ve bu tanımlayıcının eşleştirme bitince silindiği (kötüye kullanımı önleme amaçlı ayrı
bir biyometrik şablonun ise bazı bölgelerde 1 yıla kadar saklandığı), doğrudan fetch edilen
https://www.airbnb.com/help/article/3564 sayfasından birebir alındı. Bu, bölümdeki en güçlü
birincil kaynaklı maddelerden biri, iki ayrı resmi Airbnb yardım merkezi sayfasıyla çapraz
doğrulanmış durumda.

---

## 5. Native harita entegrasyonu: AirMapView ve Google Maps SDK + WebView yedeği

**Ne olduğu:** Doğrudan fetch edilen Airbnb Engineering yazısına göre Airbnb, Android
tarafında haritaları göstermek için kendi açık kaynak kütüphanesi AirMapView'ı geliştirmiş:
"bir view soyutlaması, Google Play Servisleri olan ve olmayan cihazlar için etkileşimli
haritalar sağlıyor." Kütüphane varsayılan olarak native Google Maps V2'yi (bir Fragment olarak)
kullanıyor, ama cihazda Google Play Servisleri yoksa (yazıya göre bazı ülkelerde cihazların
çoğunluğu Play Servisleri olmadan satılıyor) otomatik olarak bir WebView tabanlı haritaya
(JavaScript köprüsüyle etkileşim sağlayan) düşüyor. Geliştiriciye sunulan API her iki durumda
da aynı, yani bir ekran hangi haritanın kullanıldığını bilmeden aynı kodu yazabiliyor.

**Nerede görülür:** Öncelikle Android (yazı özellikle Android'e odaklanıyor); iOS tarafında
Apple'ın kendi MapKit/Apple Maps çerçevesi kullanılıyor olması genel platform konvansiyonu
gereği beklenir, ama bu araştırmada iOS tarafı için ayrı bir Airbnb kaynağı doğrudan fetch
edilip doğrulanmadı.

**UX gerekçesi:** Bir seyahat platformu için harita, arama sonucu ekranının ayrılmaz bir
parçası (bkz. 01-discovery-search.md); ama "internet genelinde en yaygın interaktif harita
SDK'sı" (Google Play Servisleri) her cihazda mevcut değil. Yazının belirttiği gibi "gerçekten
uluslararasılaştırılmış bir deneyim sunmak için" Airbnb haritayı bir kısım kullanıcı için
tamamen devre dışı bırakmak yerine, performansı biraz daha düşük (yazıya göre "biraz daha
kötü" ama kullanılabilir) bir WebView yedeğiyle **her cihazda** çalışır hale getirmeyi tercih
etti. Bu, "native her zaman en iyisidir" ilkesinin bile evrensel erişilebilirlik önünde geri
adım atabileceği bir örnek: ana öncelik en performanslı deneyim değil, hiç kimseyi haritasız
bırakmamak.

**Airbnb dışı bir uygulamaya uyarlama notu:** Konum/harita özelliğinin ürünün merkezinde
olduğu her uygulamada (seyahat, teslimat, emlak), tek bir harita SDK'sına (ör. sadece Google
Play Servisleri'ne) bağımlı kalmak, o SDK'nın mevcut olmadığı pazarlarda (bazı bölgelerde
Huawei gibi Play Servisleri içermeyen cihazlar yaygın) kullanıcıları tamamen dışarıda
bırakabilir. Birincil native SDK ile daha az performanslı ama evrensel bir yedek (WebView
tabanlı harita gibi) arasında otomatik geçiş yapan bir soyutlama katmanı kurmak, "bazı
pazarlarda hiç çalışmıyor" riskini "bazı pazarlarda biraz daha yavaş çalışıyor" riskine
indiriyor.

**Kaynak / güven notu:** **Doğrulandı.** AirMapView'ın amacı ("Google Play Servisleri olan ve
olmayan cihazlar için etkileşimli haritalar"), varsayılan olarak native Google Maps V2
kullanıp Play Servisleri yoksa WebView'a düşme mekanizması, ve bunun gerekçesi (bazı
ülkelerde cihazların çoğunluğunun Play Servisleri olmadan satılması), doğrudan fetch edilen
https://medium.com/airbnb-engineering/airmapview-a-view-abstraction-for-maps-on-android-4b7175a760ac
(Airbnb Engineering'in resmi Medium yayını) yazısından birebir alındı; bu, Airbnb'nin kendi
gerçek bir mühendislik artefaktı (açık kaynak kütüphane), ikincil bir yorum değil. Ancak bu
yazı 2015 civarı yayınlanmış eski bir kaynak; Airbnb'nin bugünkü (2026) harita altyapısının
hala aynı yaklaşımı kullanıp kullanmadığı bu araştırmada güncellenmedi. iOS tarafındaki
karşılığı (Apple Maps/MapKit kullanımı) bu araştırmada Airbnb'nin kendi bir kaynağından hiç
doğrulanmadı, sadece genel platform konvansiyonundan bir varsayım
→ **Android tarafı güçlü doğrulandı, iOS tarafı doğrulanmadı/eğitim verisinden varsayım.**

---

## 6. Bottom sheet derinliği: harita üzerinde sürüklenebilir, katmanlı panel istifleme

**Ne olduğu:** Mobil arama sonucu ekranında, arka planda tam ekran bir harita dururken,
önünde yukarı/aşağı sürüklenebilir bir liste paneli (bottom sheet) yer alıyor; bu panel
kullanıcı bir ilana dokunduğunda ya da bir filtre açtığında, mevcut sheet'in **üzerine**
başka bir sheet gelecek şekilde derinlik kazanabiliyor (07-navigation-ia.md'de işlenen
tekil bottom sheet kullanımından farklı olarak, burada odak birden fazla sheet'in aynı anda
üst üste **istiflenmesi**). WebSearch'te görülen (Apple'ın kendi geliştirici forumundan) bir
tartışmaya göre bu, Apple'ın kendi Maps ve Find My uygulamalarının da kullandığı, "yeni
içeriğin sağdan değil **alttan** geldiği" bir navigasyon hiyerarşisi biçimi: klasik "push"
navigasyonunun (yeni ekran sağdan kayarak gelir) yerini, sheet'lerin alttan gelip üst üste
yığılması alıyor.

**Nerede görülür:** Sadece mobil (native uygulama); web'de harita ve liste yan yana, sabit iki
panel olarak durur, bir sheet'in bir başka sheet'in üzerine "istiflenmesi" kavramı web'in
pencere/scroll modeline uymuyor.

**UX gerekçesi:** Harita + liste ekranı gibi "arka planı hiç kaybetmeden derinlemesine
gezinme" gerektiren bir yapıda, her yeni detay seviyesi (bir ilana dokunma, bir filtre açma)
için tam ekran bir sayfaya geçmek kullanıcıyı haritadan koparır. Sheet'leri birbirinin üzerine
istiflemek, her katmanın bir öncekinin üstünde, ama altındaki bağlamı (harita, önceki liste)
kısmen görünür/erişilebilir tutarak açılmasını sağlıyor; kullanıcı "en üstteki sheet'i kapat"
eylemiyle bir önceki katmana geri dönebiliyor, bu da her adımın kendi geri butonuna/gesture'ına
sahip, sığ ama derin hissedilen bir navigasyon modeli yaratıyor. Bu, klasik "push stack"
navigasyonunun (her ekran öncekini tamamen kaplar) aksine, alttaki katmanın bir kısmının
(genelde bir tutamaç/handle çizgisi ve biraz da içerik) her zaman kısmen görünür kalmasıyla
"nereden geldiğimi" sorusuna sürekli bir görsel ipucu veriyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir harita (ya da başka bir "her zaman arka
planda kalması gereken" ana görsel) üzerinde çok seviyeli bir detay gezintisi (liste -> ilan
detayı -> alt detay) gerektiren her mobil uygulamada, her seviyeyi ayrı bir tam ekran sayfa
yerine üst üste istiflenen bottom sheet'ler olarak tasarlamak, kullanıcının ana bağlamdan
(harita) hiç kopmamasını sağlıyor. Bunun karmaşıklığı, kaç seviye derinliğe kadar istiflemenin
mantıklı olduğuna dair bir sınır koymayı gerektiriyor: sınırsız derinlikte sheet üstüne sheet
açmak, sonunda "kaç kapat'a basmam gerekiyor" karmaşasına dönüşebilir.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden / ikincil kaynak.** Bu maddedeki
"harita + sürüklenebilir liste" pattern'inin genel varlığı ve Apple'ın Maps/Find My
uygulamalarında sheet istiflemesinin "alttan gelen içerik" mantığı, WebSearch'te görülen
Apple Developer Forums'taki bir tartışma başlığından ("Stacked-sheets vs. nav-stack-in-a-sheet
for navigation hierarchies on top of a map") geliyor, bu sayfa doğrudan fetch edilip birebir
okunmadı. Airbnb'nin kendi arama sonucu ekranının fiilen bu şekilde çok seviyeli sheet
istiflemesi kullandığı bilgisi bu araştırmada Airbnb'nin resmi bir kaynağından hiç
doğrulanmadı; bu, genel mobil UX gözleminden ve "Airbnb'ninki de haritalı-liste uygulamalarının
yaygın bir varyantı" çıkarımından geliyor.

---

## 7. Kart/foto kaydırma (swipe) gestureları: Tinder tarzı ilan kaydırma YOK, ama foto galerisi ve mesaj arşivlemede swipe var

**Ne olduğu:** Bu maddenin ilk ve en önemli kısmı bir **yokluk** iddiası: bu araştırmada
Airbnb'nin arama sonuçlarını Tinder tarzı (bir ilanı sağa "beğen"/sola "geç" diye tam ekran
kart kaydırarak) tüketmeye izin veren bir mod kullandığına dair **hiçbir kanıt bulunamadı**.
WebSearch'te hem Airbnb'nin kendi arayüzünün arama-öncelikli (07-navigation-ia.md madde 1)
olduğu hem de emlak/kiralama sektöründe "Tinder-ification" adı verilen swipe-tüketim
denemelerinin (ör. Brickunderground'ın haberine göre bazı kiralama uygulamalarının böyle bir
mod denediği) kullanıcı testlerinde başarısız olduğu ("tek bir fotoğrafa bakıp sola/sağa karar
vermek işe yaramadı, kullanıcılar neredeyse her ilana dokunup detaya girdi, bu da swipe'ın
faydasını ortadan kaldırdı") görüldü. Bu, Airbnb'nin böyle bir modu **hiç denemediği** anlamına
gelmiyor (denemiş olabilir), ama bugünkü, yaygın olarak bilinen/belgelenen deneyiminin bu
olmadığı, bunun yerine iki **farklı, dar kapsamlı** swipe kullanımı olduğu görülüyor: (1) ilan
foto galerisinde yatay swipe ile fotoğraflar arasında gezinme, (2) mesajlaşma gelen kutusunda
bir mesaj dizisini swipe ile arşivleme.

**Nerede görülür:** İkisi de (iOS/Android); foto galerisi swipe'ı web'de de fare/klavye ile
bir karşılığa sahip (ok tuşları/tıklama ile ilerleme), ama dokunmatik swipe doğası gereği
sadece mobilde (native uygulama ve mobil web) var. Mesaj arşivleme swipe'ı ise native
uygulamaya özgü bir liste-satırı gesture'ı (web'de karşılığı muhtemelen bir buton/ikon).

**UX gerekçesi:** Bağımsız tasarımcı Arlen McCluskey'nin doğrudan fetch edilen foto galerisi
yeniden tasarım vaka çalışmasına göre, kullanıcı testlerinde katılımcılar "tam genişlikteki
görsellerde, ok veya nokta gibi hiçbir arayüz ipucu olmadan bile reflekssel olarak sola veya
sağa kaydırıyor". Bu, swipe'ın foto galerisi bağlamında **öğrenilmesi gereken bir özellik değil,
zaten var olan bir dokunsal beklenti** olduğunu gösteriyor; tasarımın işi burada swipe'ı icat
etmek değil, onu engellememek (tam genişlik görsel, kaydırılabilir bir yüzey sunmak). Bunun
"tüm ilanı sola/sağa kaydırarak reddet/kaydet" fikrinden temel farkı: foto galerisinde swipe
**aynı ilanın içinde daha fazla bilgiye** götürüyor (ilerleme), Tinder modelinde ise swipe
**bir karar** anlamına geliyor (ilanı bir daha görmeyeceksin). Sektördeki başarısız swipe-tüketim
denemelerinin gösterdiği gibi, bir konaklama ilanı gibi çok değişkenli, yüksek riskli bir karar
(genellikle yüzlerce dolarlık bir rezervasyon) tek bir fotoğrafa bakarak ikili bir kararla
(beğen/geç) indirgenemeyecek kadar karmaşık; kullanıcılar zaten detaya girmek istiyor, bu da
swipe-karar modelinin verimliliğini ortadan kaldırıyor. Mesaj arşivleme swipe'ı ise tamamen
farklı bir kategori: burada swipe bir "karar" değil bir "eylem kısayolu" (arşivle), ve e-posta
istemcilerinden (Gmail, Mail.app) yıllardır tanıdık bir liste-satırı konvansiyonunu tekrar
kullanıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Yüksek riskli/çok değişkenli bir karar
gerektiren bir ürün (konaklama, araç, iş ilanı, ciddi bir satın alma) için Tinder tarzı ikili
swipe-karar modelini kopyalamadan önce, sektördeki başarısız denemelerin dersini dikkate almak
gerekiyor: swipe-karar modeli düşük riskli, tek değişkenli kararlar (bir fotoğrafı beğenme,
bir haberi geç) için işe yarıyor, çok değişkenli kararlar için kullanıcıyı detaya zorluyor ve
swipe'ın kendisini gereksiz kılıyor. Buna karşılık, foto galerisi gibi "aynı içeriğin içinde
ilerleme" bağlamında swipe'ı desteklemek neredeyse risksiz bir kazanç: kullanıcılar zaten bunu
bekliyor, tam genişlikte bir görsel yüzeyi swipe'a açık bırakmak yeterli. Liste satırlarında
(mesaj, bildirim, sipariş listesi) swipe-to-archive/swipe-to-delete gibi kısayolları e-posta
istemcilerinin yerleşik konvansiyonuyla tutarlı tutmak (sola kaydır = arşivle/sil gibi genel
beklentiler), kullanıcıya yeni bir gesture öğretme yükü bindirmiyor.

**Kaynak / güven notu:** Kısmen doğrulandı, karma. "Airbnb'nin Tinder tarzı bir ilan
kaydırma/karar modu kullanmadığı" iddiası **doğrudan pozitif bir Airbnb kaynağıyla
kanıtlanamaz** (bir şeyin yokluğunu ispatlamak zor), ama bu araştırmada hem Airbnb'nin genel
arama-öncelikli tasarım felsefesine dair kanıtlar (07-navigation-ia.md) hem de sektördeki
swipe-tüketim denemelerinin neden başarısız olduğuna dair WebSearch özetleri (Brickunderground
haberi, doğrudan fetch edilmedi) bu yokluğu destekliyor durumda → **doğrulanmadı ama tutarlı
dolaylı kanıt var, kesin negatif kanıt değil**. Foto galerisinde reflekssel swipe davranışı
("Participants reflexively swipe left or right... even without UI affordances") doğrudan fetch
edilen https://www.arlenmccluskey.com/projects/airbnb-photo-viewer sayfasından birebir alındı,
ama bu bağımsız bir tasarımcının kendi vaka çalışması, Airbnb'nin resmi bir yayını değil, ve
sayfa Airbnb'nin **canlı ürününün** birebir bugünkü davranışını değil bir yeniden tasarım
önerisini/araştırmasını anlatıyor → **kısmen doğrulandı, ikincil/bağımsız kaynak**. Mesaj
arşivleme için "swipe ile arşivle" iddiası, bu araştırmada Airbnb'nin resmi yardım merkezi
sayfalarından (help/article/145, help/article/2899) doğrudan fetch edilerek **doğrulanamadı**
(bu sayfalar arşivleme özelliğinden bahsediyor ama swipe gesture'ını hiç anmıyor); bu iddia
sadece WebSearch özetlerinde görülen ikincil kaynaklardan geliyor → **doğrulanmadı, ikincil
kaynak özetinden**.

---

## 8. Native paylaşım sayfası (share sheet) ile ilan paylaşma

**Ne olduğu:** Bir ilan detay sayfasındaki "paylaş" butonuna dokunulduğunda, uygulamanın
kendi özel bir paylaşım arayüzü çizmek yerine işletim sisteminin native paylaşım sayfasını
(iOS'ta UIActivityViewController, Android'de Intent.ACTION_SEND ile tetiklenen paylaşım
menüsü) açması; bu, kullanıcıya cihazında kurulu olan tüm uygulamalara (Mesajlar, WhatsApp,
e-posta, notlar) tek bir standart arayüzden paylaşım yapma imkanı veriyor.

**Nerede görülür:** Sadece mobil (native uygulama); web'de bu işlevin karşılığı genelde
ayrı bir "kopyala/sosyal medya ikonları" paneli ya da tarayıcının kendi (varsa) paylaşım
API'si.

**UX gerekçesi:** Bir uygulamanın kendi özel paylaşım arayüzünü (ör. sadece Facebook/Twitter/
e-posta ikonlarından oluşan sabit bir liste) çizmesi yerine işletim sisteminin native paylaşım
sayfasına devretmesi, iki önemli avantaj sağlıyor: (1) kullanıcının hangi uygulamalara
paylaşım yapmak istediği zamanla değişir (bugün WhatsApp, yarın farklı bir mesajlaşma
uygulaması); native share sheet bu listeyi işletim sistemi seviyesinde otomatik günceller,
uygulamanın kendisinin her yeni popüler uygulama için ayrı bir entegrasyon yazmasına gerek
kalmaz. (2) kullanıcı zaten diğer uygulamalardan (fotoğraflar, Safari, notlar) bu paylaşım
arayüzüne aşina; yeniden öğrenmesi gereken bir arayüz değil. Bu, madde 1'deki "native-öncelikli"
mimari kültürüyle de tutarlı: platformun kendi standart bileşenlerine güvenmek, özel bir
tekerlek icat etmekten daha az bakım yükü ve daha tutarlı bir kullanıcı deneyimi sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir içerik parçasını (ürün, ilan, makale)
paylaşılabilir kılan her mobil uygulamada, kendi özel paylaşım arayüzünü sabit bir uygulama
listesiyle sınırlı tutmak yerine işletim sisteminin native paylaşım sayfasına devretmek,
kullanıcının cihazında kurulu olan (ve zamanla değişen) tüm uygulamalara erişimi tek bir
standart, tanıdık arayüzden sağlıyor; bu hem mühendislik bakımını azaltıyor hem kullanıcı için
öğrenme eğrisini sıfırlıyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu araştırmada Airbnb'nin ilan
paylaşım akışının native share sheet kullandığını doğrudan doğrulayan resmi bir Airbnb kaynağı
(yardım merkezi, mühendislik blogu, ekran kaydı) bulunamadı; WebSearch'te bu konuda bulunan
sonuçlar Airbnb topluluk forumundaki ("sharing listings with friends", "How do I share my
listing on Facebook") kullanıcı sorularıyla sınırlı kaldı, bunlar da paylaşım mekanizmasının
teknik detayını (native share sheet mi özel bir panel mi) belirtmiyor. Bu madde, iOS/Android'in
"paylaş" ikonu olan hemen her uygulamada bu native bileşenin fiilen kullanıldığına dair genel
platform gözleminden ve Airbnb'nin de bu konvansiyonu takip ettiği varsayımından geliyor,
Airbnb'ye özgü olarak doğrulanmadı.

---

## 9. Host foto yükleme akışı: kamera vs galeri, konum-bağlı foto doğrulama

**Ne olduğu:** Bir host, yeni bir ilan oluştururken ya da mevcut ilan fotoğraflarını
güncellerken telefonun galerisinden (kütüphaneden) mevcut fotoğrafları seçip yükleyebiliyor.
Ancak WebSearch özetlerinde görülen ikincil kaynaklara göre, Airbnb'nin **fotoğraf doğruluğu**
(photo accuracy) inceleme sürecinde (ilanın gerçekte anlatılan gibi göründüğünü teyit etmek
için) bu kural tersine dönüyor: host'un ilanı **fiziksel olarak ziyaret edip** doğrulama
fotoğraflarını **o an, uygulamanın kendi kamera özelliğiyle** çekmesi gerekiyor; galeriden
mevcut bir fotoğraf bu inceleme için yüklenemiyor.

**Nerede görülür:** İkisi de (native kamera/galeri erişimi gereken akış); normal ilan
fotoğrafı ekleme büyük olasılıkla hem galeri hem kamera seçeneği sunuyor (genel mobil
konvansiyon), doğrulama akışı ise (WebSearch özetine göre) sadece kamerayla sınırlı.

**UX gerekçesi:** Bir ilanın normal fotoğraflarını yüklerken galeriye izin vermek mantıklı:
host muhtemelen ilanını profesyonel bir fotoğrafçıyla ya da farklı bir zamanda çekmiş
olabilir, bu fotoğrafları galerisinden seçip yüklemesi gerekiyor. Ama "bu ilan hala anlatıldığı
gibi mi" sorusunu cevaplayan bir doğrulama akışında, galeriden seçim izni vermek doğrulamanın
amacını tamamen boşa çıkarır: host, yıllar önce çekilmiş eski (ve artık gerçeği yansıtmayan)
bir fotoğrafı yeniden "doğrulama" olarak gönderebilir. Sadece o anda, uygulamanın kendi
kamerasıyla çektirmek (madde 4'teki kimlik doğrulama selfie'sindeki "canlı çekim" mantığıyla
birebir aynı ilke), doğrulamanın gerçekten **o anki** durumu yansıttığını arayüz seviyesinde
garanti ediyor; bu, kullanıcıya "lütfen eski fotoğraf göndermeyin" diye bir kural yazmak yerine,
arayüzden o seçeneği kaldırarak zorunlu kılınan bir bütünlük kontrolü.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir platformda "bu içerik/durum şu an gerçekten
böyle mi" sorusunu cevaplayan herhangi bir doğrulama akışında (ürün durumu doğrulama, hasar
tespiti, teslimat kanıtı, konum doğrulama), galeriden eski bir dosya seçme seçeneğini tamamen
kaldırıp sadece o anki canlı kamera çekimini kabul etmek, doğrulamanın bütünlüğünü bir kural
metniyle değil arayüz kısıtıyla sağlıyor. Bu ayrımı (normal içerik yükleme: galeri+kamera,
doğrulama: sadece kamera) net biçimde iki farklı akış olarak tasarlamak önemli; ikisini
karıştırmak (doğrulamada da galeriye izin vermek) doğrulamanın güvenilirliğini baştan zayıflatır.

**Kaynak / güven notu:** **Doğrulanmadı, ikincil kaynak özetinden.** Bu maddenin çekirdek
iddiası ("doğrulama fotoğrafları için kamera zorunlu, galeri yasak, host ilanı fiziksel olarak
ziyaret etmeli") bu araştırmada Airbnb'nin resmi bir yardım merkezi sayfasından doğrudan fetch
edilerek **doğrulanamadı**; bu bilgi WebSearch özetlerinde görülen üçüncü taraf host-araçları
sitelerinden (ör. Guesty for Hosts, rentalrecon.com tarzı kaynaklar) geldi, bu sayfalar
doğrudan fetch edilip birebir okunmadı, ve doğrudan fetch edilen airbnb.com/help/article/3034
sayfası bu konuyu (kimlik doğrulama selfie'siyle karıştırılmaması gereken, ayrı bir "ilan
fotoğraf doğruluğu" süreci) hiç anmıyor. Bu madde bu bölümdeki en zayıf kaynaklı iddialardan
biri; yine de madde 4'teki "canlı çekim zorunluluğu" ilkesiyle tutarlı olması (aynı tasarım
mantığının host tarafında da tekrarlanmış olması makul bir varsayım), bunu tamamen
temelsiz bir spekülasyondan ayırıyor, ama kesinlik iddia edilmiyor.

---

## 10. Push bildirim izni isteme zamanlaması ve çerçevelemesi

**Ne olduğu:** Bir mobil uygulamanın, işletim sisteminin kendi bildirim izni diyaloğunu
(iOS'ta "Airbnb Bildirimler göndermek istiyor" sistem promptu, Android'de benzeri) **ne zaman**
tetiklediği sorusu: uygulama ilk açıldığında hemen mi, yoksa kullanıcı bildirimlerin neye
yarayacağını (bir mesaj alacağını, bir rezervasyon onayı geleceğini) deneyimledikten sonraki
bir anda mı.

**Nerede görülür:** Sadece mobil (native uygulama); web'de tarayıcı bildirimleri ayrı bir
mekanizma ve bu araştırmada Airbnb'nin web bildirim izni akışı ayrıca incelenmedi.

**UX gerekçesi:** iOS ve Android'in kendi sistem düzeyinde bildirim izni diyaloğu, kullanıcıya
**sadece bir kez** (ya da reddedilirse ancak ayarlardan manuel açılarak) sorulabiliyor; bu
"tek atışlık" doğası, iznin **ne zaman** istendiğini kritik bir tasarım kararı haline
getiriyor. Genel mobil UX prensibi (permission priming olarak bilinen, WebSearch'te görülen
UserOnboard kaynağının da değindiği bir kavram), sistem izin diyaloğunu uygulama ilk
açıldığında hemen göstermek yerine, kullanıcının bildirimin **neden** faydalı olacağını
anladığı bir bağlamda (ör. bir mesaj gönderdikten hemen sonra, "host'un cevabını kaçırma"
gibi bir gerekçeyle) tetiklemeyi öneriyor; çünkü kullanıcı "evet" ya da "hayır" kararını bir
kere veriyor ve bu kararı sistem düzeyinde tekrar sormak kolay değil, ilk izlenim bu yüzden
büyük ağırlık taşıyor. Bir rezervasyon/mesajlaşma platformu için bildirimler (host cevabı,
rezervasyon onayı, check-in hatırlatması) ürünün değerinin önemli bir parçası olduğundan,
bu izni erken ve bağlamsız bir şekilde isteyip reddedilme riskini göze almak yerine, kullanıcı
bunun neden işine yarayacağını hissettiği bir anda istemek daha yüksek kabul oranı sağlıyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Push bildirim izni isteyen her mobil uygulamada,
sistem izin diyaloğunu uygulamanın en ilk ekranında, herhangi bir bağlam olmadan tetiklemek
yerine, kullanıcının bildirimin somut faydasını (bir mesaj, bir hatırlatma, bir onay) az önce
deneyimlediği ya da deneyimlemek üzere olduğu bir ana ertelemek, kabul oranını artırıyor. Bazı
ekipler bunu bir "yumuşak" (soft) izin isteğiyle (uygulamanın kendi ekranında "bildirimleri aç"
diye bir buton, gerçek sistem diyaloğunu sadece kullanıcı bu butona bastığında tetiklemek)
birleştirerek, sistem diyaloğunun "tek atışlık" riskini kullanıcı zaten "evet" demeye hazır
olduğu bir ana kadar erteliyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu maddede Airbnb'nin push
bildirim iznini fiilen **ne zaman ve nasıl bir çerçeveyle** istediğine dair Airbnb'nin kendi
bir kaynağından (yardım merkezi, mühendislik blogu, ekran kaydı) doğrudan doğrulama bu
araştırmada bulunamadı; doğrudan fetch edilen https://www.airbnb.com/help/article/14
(bildirim kanalları/tipleri, 06 ve 07 numaralı bölümlerde de kullanıldı) bildirimlerin **türlerini**
(Mesajlar, Hatırlatmalar, Promosyonlar, vb.) doğruluyor ama izin isteme **zamanlamasından** hiç
bahsetmiyor. Bu maddenin UX gerekçesi kısmı genel "permission priming" endüstri prensibine
(WebSearch özetinde görülen UserOnboard kaynağı, doğrudan fetch edilmedi) ve platformların
(iOS/Android) genel bilinen izin API kısıtlarına dayanıyor, Airbnb'nin kendi spesifik
uygulaması doğrulanmadı.

---

## 11. Ana ekran widget'ı: bu araştırmada Airbnb'ye ait bir native widget bulunamadı

**Ne olduğu:** iOS'ta WidgetKit, Android'de App Widgets çerçevesi üzerinden bir uygulamanın
ana ekrana (veya iOS'ta kilit ekranına) yerleştirilebilen, uygulamayı açmadan bilgi gösteren
küçük bir arayüz paneli (ör. yaklaşan bir seyahatin geri sayımı, bir rezervasyonun check-in
tarihi). Bu maddenin ana bulgusu **olumsuz**: bu araştırmada, resmi App Store sayfası dahil
hiçbir kaynakta, Airbnb'nin resmi olarak yayınladığı bir ana ekran widget'ının varlığına dair
kanıt bulunamadı.

**Nerede görülür:** Bilinmiyor/muhtemelen yok. Doğrudan fetch edilen Airbnb App Store sayfası
(apps.apple.com), uygulama açıklamasında widget'a dair hiçbir referans içermiyor; WebSearch'te
"Airbnb iOS widget WidgetKit" gibi sorgular sadece üçüncü taraf, resmi olmayan "Airbnb ikon
paketi/ev ekranı özelleştirme" sitelerini (WidgetClub gibi, bunlar Airbnb'nin **uygulama
ikonunu** özelleştirmek için ikon paketleri satan, Airbnb'nin kendi ürünüyle ilgisi olmayan
üçüncü taraf servisler) ve Dribbble'da bağımsız tasarımcıların "Airbnb iOS Widget" adlı
**konsept/hayali tasarım** çalışmalarını döndürdü, resmi bir Airbnb widget'ı değil.

**UX gerekçesi:** Bir seyahat/rezervasyon uygulaması için "yaklaşan seyahatin geri sayımı" ya
da "check-in bilgisi" gibi bir widget, teorik olarak mantıklı bir özellik adayı: kullanıcı
sık sık "seyahatime kaç gün kaldı" sorusunu uygulamayı açmadan cevaplamak isteyebilir. Bu
özelliğin (bulunabildiği kadarıyla) resmi olarak var olmaması birkaç şekilde yorumlanabilir:
belki widget'lar Airbnb'nin kullanım sıklığı modeline göre (çoğu kullanıcı yılda birkaç kez
seyahat rezervasyonu yapıyor, günlük bir "check" alışkanlığı değil) öncelik sırasında düşük
kalmış olabilir, ya da mühendislik kaynakları başka özelliklere (ör. madde 1'deki React
Native mimarisi geçişleri gibi büyük altyapı projelerine) ayrılmış olabilir. Ama bu tamamen
bir spekülasyon; bu maddenin dürüst değeri, "araştırıldı ve bulunamadı" bilgisinin kendisi.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir widget özelliği eklemeden önce, ürünün
kullanım sıklığı modelini (kullanıcı günde kaç kez "hızlıca bakmak" isteyebilir) değerlendirmek
faydalı: widget'lar en çok günlük/sık kontrol edilen bilgiler için (hava durumu, adım sayacı,
takvim) değer katıyor; yılda birkaç kez tekrarlanan bir eylem (seyahat rezervasyonu) için
widget yatırımı, mühendislik maliyetine kıyasla düşük bir kullanım sıklığıyla karşılaşabilir.
Bu, "her platform özelliği her ürüne uygulanmalı" varsayımının sorgulanması gereken bir örnek.

**Kaynak / güven notu:** **Doğrulanmadı, bulgu yokluğu olarak işaretlendi.** Airbnb'nin App
Store sayfası (https://apps.apple.com/us/app/airbnb/id401626263) doğrudan fetch edildi ve
widget'a dair hiçbir referans bulunamadı; ancak App Store açıklamaları genelde bu düzeyde
teknik/platform detayı listelemiyor, dolayısıyla bu **kesin bir "widget yok" kanıtı değil**,
sadece bu spesifik kaynakta bulunamadığı anlamına geliyor. WebSearch'te bulunan tüm sonuçlar
ya Airbnb'nin ürünüyle ilgisiz üçüncü taraf ikon/widget özelleştirme siteleri ya da bağımsız
tasarımcıların hayali konsept çalışmalarıydı, resmi bir Airbnb kaynağı (mühendislik blogu,
App Store "What's New" güncelleme notu, basın bülteni) bu konuda hiç bulunamadı. Bu madde
diğerlerinden farklı bir tür: burada iddia edilen şey bir pattern'in varlığı değil, bulunamamış
olması; okuyucu bunu "Airbnb'de widget kesinlikle yok" diye değil "bu araştırma yöntemiyle
bulunamadı" diye okumalı.

---

## 12. Çevrimdışı / düşük bağlantı durumunda önceden görüntülenen içeriğe erişim

**Ne olduğu:** Bir kullanıcının interneti olmadığında (uçakta, kırsal bir bölgede, veri
paketi bittiğinde) uygulamanın daha önce görüntülediği ilanlara, wishlist'lere, rezervasyon
detaylarına ve mesajlara erişip erişemediği sorusu. Doğrudan fetch edilen bir 2015 tarihli
teknoloji basını haberine göre Airbnb bir dönem bu tam olarak bu özelliği duyurmuştu: "Artık
daha önce görüntülediğiniz herhangi bir ilana, Wish List'e, rezervasyon detayına ve mesaja
erişmek için çevrimiçi olmanıza gerek yok" ("Now, you don't [need] to be online to access any
listings, Wish Lists, reservation details, and messages that you've previously viewed").
Ancak bu araştırmada bulunan daha güncel WebSearch sonuçları (Airbnb topluluk forumu
şikayetleri özetinden), bugünkü uygulamada çevrimdışıyken Trips (rezervasyonlar) bölümüne
girildiğinde kullanıcıların "sinyal yok" hatasıyla karşılaştığını ve bu bölümü kullanamadığını
öne sürüyor.

**Nerede görülür:** Sadece mobil (native uygulama; çevrimdışı önbellekleme kavramı web'de
farklı çalışır, tarayıcı sekmesi kapatılırsa önbellek genelde kaybolur).

**UX gerekçesi:** Bir seyahat uygulamasının en kritik anı, tam olarak kullanıcının interneti
olmadığı anlar olabilir: yurt dışında veri paketi olmadan, bir havalimanında Wi-Fi'a
bağlanmadan önce, rezervasyon onay kodunu/adresi görmesi gerektiği an. 2015'teki özelliğin
gerekçesi muhtemelen tam olarak buydu: rezervasyon detayları gibi "seyahat sırasında en çok
ihtiyaç duyulan" bilgiyi, bağlantı olmasa bile önbellekten sunmak. Bugünkü forum
şikayetlerinin işaret ettiği olası gerileme (Trips bölümünün artık çevrimdışı çalışmaması),
eğer doğruysa, ürünün zamanla (belki güvenlik/güncel veri gereksinimleri, belki mimari
değişiklikler yüzünden) bu özellikten uzaklaşmış olabileceğini gösteriyor; bu, "bir özelliğin
bir noktada var olduğu doğrulanmış olsa bile, yıllar sonra hala aynı şekilde çalıştığı
varsayılamaz" ilkesinin somut bir örneği (bu proje genelinde tekrarlanan bir uyarı).

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir seyahat/rezervasyon uygulamasında,
kullanıcının en çok bağlantısız kaldığı anların (uçakta, yurt dışında, kırsalda) aynı zamanda
en çok bilgiye ihtiyaç duyduğu anlar olduğunu kabul edip, en azından rezervasyon onay
kodu/adres/check-in talimatları gibi kritik, değişmeyen bilgiyi cihazda önbellekleyip
çevrimdışı erişilebilir tutmak, kullanıcı güvenini önemli ölçüde artırıyor. Bu özelliği bir
kere kurup unutmamak da önemli: mimari değişiklikler zamanla önbellekleme davranışını
(fark edilmeden) bozabilir; düzenli olarak "uçak modunda uygulama hala işe yarıyor mu" testi
yapmak, bu tür sessiz gerilemeleri yakalıyor.

**Kaynak / güven notu:** Kısmen doğrulandı, karma. 2015 tarihli özelliğin duyurulduğu ve tam
metni ("listings, Wish Lists, reservation details, and messages that you've previously
viewed"), doğrudan fetch edilen
https://www.iclarified.com/46670/airbnb-app-gets-improved-navigation-offline-access-to-previously-viewed-listings-more
sayfasından alındı; ancak bu, Airbnb'nin resmi bir kaynağı değil, üçüncü taraf bir teknoloji
haber sitesi (muhtemelen Airbnb'nin o zamanki resmi duyurusunu/basın bültenini aktarıyor, ama
bu araştırmada Airbnb'nin kendi orijinal duyurusu ayrıca doğrulanmadı), ve **11 yıl önceki bir
özellik**, bugünkü davranışı garanti etmiyor. Bugünkü Trips bölümünün çevrimdışı çalışmadığı
iddiası ise sadece WebSearch özetlerinde görülen topluluk forumu şikayetlerinden geliyor, bu
sayfalar doğrudan fetch edilip birebir okunmadı → **her iki uçtaki iddia da (eski özellik +
bugünkü olası gerileme) ikincil kaynaktan, birbirini doğrudan doğrulayan/çürüten resmi bir
Airbnb kaynağı bu araştırmada bulunamadı**.

---

## 13. App Store / Play Store değerlendirme (rating) istemi zamanlaması

**Ne olduğu:** Bir mobil uygulamanın, kullanıcıdan uygulama mağazasında (App Store/Play
Store) bir yıldız değerlendirmesi/yorum bırakmasını istediği native sistem promptu (iOS'ta
Apple'ın kendi StoreKit çerçevesindeki `SKStoreReviewController`/`requestReview()` API'si).
Bu maddenin sorusu, Airbnb'nin bu istemi uygulama içinde **hangi anda** (ilk açılışta mı,
başarılı bir rezervasyon sonrasında mı, bir seyahat tamamlandıktan sonra mı) tetiklediği.

**Nerede görülür:** Sadece mobil (native uygulama; bu API'ler platform mağazalarına özgü,
web'de "bizi değerlendirin" gibi bir kavramın native karşılığı yok).

**UX gerekçesi:** Apple'ın kendi StoreKit kısıtı, bir uygulamanın bu promptu **365 günde en
fazla 3 kez** gösterebilmesine izin veriyor (WebSearch'te görülen genel StoreKit
dokümantasyonu özetlerine göre), yani her tetikleme değerli ve sınırlı bir kaynak. Genel
endüstri prensibi (Apple'ın kendi geliştirici tavsiyeleri de dahil, WebSearch özetlerinden),
bu istemi kullanıcının **olumlu bir deneyim yaşadığı** bir anın hemen ardından (ör. bir görevi
başarıyla tamamladıktan sonra) göstermek, rastgele/bağlamsız bir anda (ör. uygulama her
açıldığında) göstermekten çok daha yüksek olumlu değerlendirme oranı sağlıyor; kötü bir anda
(ör. bir hata mesajından hemen sonra) sorulan bir değerlendirme istemi, tam tersi etki
yaratıp öfkeli bir 1 yıldız yorumunu tetikleyebilir. Bir seyahat uygulaması için mantıklı
adaylar, bir rezervasyonun başarıyla tamamlanması ya da bir seyahatin sorunsuz bitmesi gibi
anlar olurdu, ama bu Airbnb'nin kendi ifade ettiği bir tasarım kararı değil, bu araştırmacının
genel prensipten yaptığı bir çıkarım.

**Airbnb dışı bir uygulamaya uyarlama notu:** Uygulama mağazası değerlendirme istemini
kullanan her mobil uygulamada, bu istemi rastgele bir anda değil, kullanıcının az önce
olumlu bir sonuç aldığı somut bir an (bir işlemi tamamlama, bir hedefe ulaşma) ile
ilişkilendirmek, sistemin sınırlı tetikleme hakkını (365 günde birkaç kez) en yüksek olumlu
dönüş ihtimaliyle kullanmak anlamına geliyor. Kötü bir deneyimin (hata, şikayet, iptal)
hemen ardından bu istemi hiç göstermemek, düşük puanlı bir yorumu proaktif olarak önlüyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu maddede Airbnb'nin
değerlendirme istemini **fiilen ne zaman** tetiklediğine dair Airbnb'nin kendi bir
kaynağından (mühendislik blogu, yardım merkezi, ekran kaydı) bu araştırmada hiçbir doğrulama
bulunamadı; WebSearch sonuçları Airbnb'nin App Store'daki genel puanı (4,8/5) ve rezervasyon
sonrası **host'a yönelik misafir yorumu** akışı (bu, mağaza değerlendirmesiyle karıştırılmamalı,
tamamen ayrı bir ürün özelliği, 05-trust-safety-signals.md'de işlendi) hakkında bilgi verdi,
ama uygulama mağazası rating promptunun zamanlaması hakkında hiçbir şey söylemedi. Bu maddenin
UX gerekçesi kısmı tamamen Apple'ın genel StoreKit kısıtları ve endüstri genelinde bilinen
"olumlu an sonrası sor" prensibine dayanıyor, Airbnb'ye özgü değil.

---

## 14. Haptic (dokunsal) geri bildirim kullanımı: tarih seçimi, buton dokunuşları, pull-to-refresh

**Ne olduğu:** iOS'un Taptic Engine'i ya da Android'in titreşim motoru üzerinden, bir
dokunmatik etkileşimin (bir tarihi takvimde seçme, bir butona basma, pull-to-refresh
gesture'ının eşiğine ulaşma) görsel geri bildirime ek olarak kısa, hafif bir titreşimle de
onaylanması.

**Nerede görülür:** İkisi de (iOS Taptic Engine/Core Haptics, Android'in kendi titreşim
API'leri); doğası gereği tamamen dokunmatik bir donanım özelliği olduğu için web'de hiçbir
karşılığı yok.

**UX gerekçesi:** Bir dokunmatik ekranda, kullanıcının parmağı ekranın kendisini
"hissetmiyor" (fiziksel bir buton gibi tıklama/basınç geri bildirimi yok); haptic feedback bu
eksikliği donanım seviyesinde telafi ediyor. Bir tarih seçicide her güne dokunulduğunda ince
bir titreşim, kullanıcıya "dokunuşun kayıt edildiğini" görsel değişiklikten (rengin dolması)
bağımsız, ek bir duyusal kanaldan onaylıyor; bu özellikle hızlı, art arda dokunmalarda (bir
tarih aralığını hızla taramak gibi) faydalı, çünkü göz her dokunuşu takip edemeyebilir ama
parmak hisseder. Pull-to-refresh'te (09-states-feedback.md madde 14'te tarihi işlenen bir
pattern) eşiğe ulaşıldığında (yenilemenin tetikleneceği an) bir haptic "tık" vermek,
kullanıcıya "yeterince çektin, bırakabilirsin" sinyalini parmağı ekrandan hiç kaldırmadan,
gözünü de spinner'dan ayırmadan veriyor. Bu tür ince, kısa (genelde 10-50 milisaniye) haptic
darbeler, aşırı kullanıldığında ("her dokunuşta titreşim") rahatsız edici ve pil tüketici hale
gelebiliyor; bu yüzden genel platform rehberlerinde (Apple'ın kendi haptic tasarım
rehberlerinde) bu geri bildirimin **seçici ve anlamlı anlarla** sınırlı tutulması öneriliyor.

**Airbnb dışı bir uygulamaya uyarlama notu:** Bir mobil uygulamada, kullanıcının kritik bir
eşiği geçtiği anları (bir seçimi onaylama, bir gesture'ın tetikleneceği eşiğe ulaşma, bir
sınırlayıcı değere gelme) sadece görsel bir değişiklikle değil, kısa bir haptic darbeyle de
işaretlemek, özellikle hızlı/art arda etkileşimlerde (bir listeyi hızla kaydırma, bir aralık
seçme) kullanıcının gözünü ekrandan ayırmadan bile "bir şey oldu" bilgisini alabilmesini
sağlıyor. Bunu her dokunuşta değil, sadece anlamlı durum değişikliklerinde (bir eşiğe ulaşma,
bir seçimi kaydetme, bir hata/uyarı anı) tetiklemek, bu geri bildirimin "gürültü" haline
gelip anlamını yitirmesini (madde 07-navigation-ia.md'deki tab bar badge mantığıyla aynı ilke:
seyrek ve anlamlı tutmak) önlüyor.

**Kaynak / güven notu:** **Doğrulanmadı, eğitim verisinden.** Bu araştırmada Airbnb'nin kendi
uygulamasında haptic feedback'i hangi spesifik anlarda (tarih seçimi, buton, pull-to-refresh)
kullandığına dair Airbnb'nin resmi bir kaynağından (mühendislik blogu, yardım merkezi, ekran
kaydı/video) hiçbir doğrulama bulunamadı; WebSearch sonuçları genel iOS/Android haptic
feedback API dokümantasyonuna (Apple Developer, çeşitli Medium tutorial'ları) yönlendirdi,
bunlardan hiçbiri Airbnb'yi örnek olarak anmıyordu. Bu madde tamamen genel mobil UX
prensiplerine ve "bu düzeyde cilalı bir tüketici uygulamasının muhtemelen haptic feedback
kullandığı" varsayımına dayanıyor; Airbnb'nin fiilen bunu yapıp yapmadığı, hangi ekranlarda
yaptığı bu araştırmada teyit edilmedi.

---

## Genel gözlem: kaynak kalitesi özeti

- **Doğrudan fetch edilip Airbnb'nin kendi kaynağıyla güçlü doğrulanan maddeler:** madde 1
  (React Native benimseme/terk kararı, iki ayrı Airbnb Engineering yazısı), madde 2 (Face
  ID/Touch ID ile giriş, help/article/3530), madde 3 (2FA'da cihaz biyometrisi,
  help/article/2842), madde 4 (kimlik doğrulama selfie/belge/yüz eşleştirme, iki ayrı yardım
  merkezi sayfası help/article/1237 ve help/article/3564), madde 5'in Android kısmı
  (AirMapView, Airbnb Engineering). Bu 5 madde (6 kaynak), bu bölümün en sağlam birincil
  kaynaklı çekirdeğini oluşturuyor; özellikle madde 1 ve 4 iki ayrı bağımsız Airbnb kaynağıyla
  çapraz doğrulanmış durumda.
- **Kısmen doğrulanan (gerçek kaynak fetch edildi ama Airbnb'ye özgü olmayan genel prensip,
  bağımsız/ikincil kaynak, ya da iddia sadece kısmen destekleniyor):** madde 5'in iOS kısmı
  (Apple Maps varsayımı doğrulanmadı), madde 6 (bottom sheet istifleme, Apple forumundan
  ikincil özet), madde 7 (foto galerisi swipe'ı bağımsız tasarımcı vaka çalışmasından
  doğrulandı ama Airbnb'nin resmi kaynağı değil; Tinder-tarzı swipe'ın yokluğu dolaylı kanıtla
  destekleniyor ama kesin kanıtlanamıyor), madde 12 (2015 çevrimdışı özelliği üçüncü taraf
  teknoloji haberinden, bugünkü davranışı doğrulanmadı).
- **Büyük ölçüde ya da tamamen doğrulanmayan maddeler:** madde 8 (native share sheet), madde 9
  (host foto doğrulama kamera-only kısıtı), madde 10 (push bildirim izni zamanlaması), madde 11
  (widget, açık bulgu boşluğu olarak işaretlendi), madde 13 (mağaza değerlendirme istemi
  zamanlaması), madde 14 (haptic feedback). Bu 6 madde, Airbnb'nin resmi kaynaklarında (yardım
  merkezi, mühendislik blogu, App Store sayfası) doğrudan ele alınmayan, büyük ölçüde genel
  mobil platform konvansiyonlarından ve "bu düzeyde olgun bir tüketici uygulamasının muhtemelen
  böyle yaptığı" varsayımından yazıldı.
- Toplamda 14 pattern'den **5 tanesi** doğrudan Airbnb birincil kaynağıyla (6 ayrı gerçek
  kaynak: 2 Airbnb Engineering Medium yazısı, 4 Airbnb yardım merkezi sayfası, 1 Airbnb
  Engineering harita yazısı) güçlü doğrulandı, **4 tanesi** kısmen doğrulandı (gerçek ama
  ikincil/bağımsız kaynak ya da dolaylı kanıt), **6 tanesi** (madde 8, 9, 10, 11, 13, 14)
  büyük ölçüde ya da tamamen doğrulanmadı/eğitim verisinden. Bu oran, önceki bölümlerin
  çoğuna (özellikle madde 4 ve madde 5'in trust-safety odaklı içeriğiyle örtüşen kimlik
  doğrulama/biyometri konularının Airbnb'nin yardım merkezinde nispeten iyi belgelenmiş
  olması sayesinde) kıyasla dengeli; ama native mobil özelliklerin (widget, haptic, share
  sheet, rating prompt zamanlaması) çoğu Airbnb'nin kendi resmi kanallarında hiç
  belgelenmiyor, çünkü bunlar kullanıcıya yönelik yardım makaleleri değil, ince mühendislik/
  tasarım detayları; bu bölüm de diğerleri gibi "şu an tam olarak böyle" değil "tekrar
  gözlemlenen/kısmen belgelenen yerleşik pattern, bazı maddelerde açık bulgu boşluğuyla
  birlikte" çerçevesiyle okunmalı. Apple App Store sayfasının doğrudan fetch edilip widget/
  share sheet/biyometri için hiçbir açık referans bulunamaması da bu bölüme özgü, önceki
  bölümlerde karşılaşılmayan bir doğrulama kısıtıydı: bu tür ince native entegrasyon
  detayları, kullanıcıya dönük pazarlama/yardım metinlerinde nadiren açıkça anılıyor.
