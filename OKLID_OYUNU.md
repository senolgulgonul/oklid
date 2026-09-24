# Öklid Elementler — proje notu
*Şenol Gülgönül. v1.3 (2026-09-24). Her yayında sürüm artar (v1.1, v1.2…); sürüm numarası sayfanın kenar çubuğunda ve CSS başındaki yorumda yazar. Tek dosya: `index.html`. Kaynak: https://github.com/senolgulgonul/oklid · Yayın: https://senolgulgonul.github.io/oklid/ · Taslak artifact: https://claude.ai/artifact/PzZ1EosnP6RuV3DZHkEQb8*

## 1. Ne
Cetvel ve pergelle oynanan tarayıcı oyunu; Öklid'in Elementler'inin I. kitabındaki çizim önermelerini, kitabın sırasıyla, kitabın kurallarıyla çözdürür. Tek HTML dosyası, dış bağımlılık yalnız Google Fonts (Literata); telefon ve masaüstünde çalışır; hesap, sunucu, veri toplama yok.

## 2. Kurallar (oyunun ve Öklid'in)
- Cetvel: iki noktaya bas, ikisinden geçen doğru çizilir (ekranda boydan boya, uzatma gerekmez; ölçmez).
- Pergel: ilk nokta merkez, ikinci nokta çemberin üzerinden geçtiği nokta (yarıçap). Açıklık taşınamaz (3. postulat).
- Yeni nokta yalnız kesişimde doğar: çizgi ve çemberlerin kesişimleri gri belirir, ilk basış noktayı doğurur, ikinci basış seçer.
- Serbest nokta yok. Hamle sayısı yok. Hedef sağlanınca durum satırı "Tamamlandı" der (varsa yeni aracı söyler), sonuç yeşil çizilir.
- Kazanılan önerme sonraki seviyede tek hamlelik araç olur.

## 3. Seviyeler ve açılan araçlar
| Seviye | Önerme | Hedef | Açılan araç |
|---|---|---|---|
| 1 | I.1 | AB üzerine eşkenar üçgen | Eşkenar üçgen (E): iki nokta |
| 2 | I.2 | A'dan BC'ye eşit parça | Taşınmış pergel (T): merkez + parçanın iki ucu → çember |
| 3 | I.3 | AB'den CD'ye eşit parça kes | — |
| 4 | I.9 | ABC açısını ikiye böl | Açıortay (A): A, köşe B, C |
| 5 | I.10 | AB'nin orta noktası | Orta nokta (O): iki nokta |
| 6 | I.11 | Doğru üstündeki C'den dikme | — |
| 7 | I.12 | Doğru dışındaki C'den dikme | Dikme (K): doğrunun iki noktası + nokta |
| 8 | I.31 | C'den AB'ye paralel | Paralel (L): doğrunun iki noktası + nokta |
| 9 | I.46 | AB üzerine kare | Kare (Q): kenarın iki ucu + karşı tarafta kalacak nokta |
| 10 | II.14 | ABCD dikdörtgenine alanca eşit kare (kareleme) | — ; oyun burada biter |

I.47 (gelin şekli) çıkarıldı: teoremdir, problem değil; oyun yalnız çizim önermelerini oynatır. I.46 birinci kitabın son problemi olduğundan final ikinci kitabın karelemesidir.

Temel araçlar her seviyede: Cetvel (D), Pergel (P). Kilitli araçlar listede sönük görünür; n. seviyenin aracı n+1. seviyeden itibaren açıktır (kazanılmış olmasına bakılmaz, seviyeye bağlıdır). Baştan (R) seviyeyi ve o seviyenin ✓ işaretini sıfırlar. Seviye, üstteki açılır menüden ya da ‹ › oklarıyla seçilir; kazanılanlar ✓ ile işaretli. Tuşlar: Z geri al, R baştan, S sığdır, Esc seçimi temizle. Kaydırma: boş yerden sürükle; yaklaştırma: tekerlek ya da iki parmak.

## 4. Çözüm yolları (ipucu istenirse)
- **I.1:** A merkezli AB, B merkezli BA çemberleri; kesişime bas (C); cetvelle AC, BC.
- **I.2:** AB çiz; AB üzerine eşkenar üçgen (tepe D); pergel B–C, çemberin DB doğrusunu B'nin ötesinde kestiği yere bas (G); pergel D–G, çemberin DA doğrusunu A'nın ötesinde kestiği yere bas (L); cetvel A–L.
- **I.3:** taşınmış pergel A, C, D; çemberin AB'yi kestiği yere bas.
- **I.9:** pergel B–C, çemberin BA'yı kestiği yere bas (D); eşkenar üçgen D, C (tepe E); cetvel B–E.
- **I.10:** eşkenar üçgen A, B (tepe C); açıortay A, C, B; kesişime bas.
- **I.11:** pergel C–A, çemberin AB'yi öbür yanda kestiği yere bas (D); eşkenar üçgen A, D (tepe E); cetvel E–C.
- **I.12:** pergel C–A, çemberin AB'yi kestiği ikinci noktaya bas (D); orta nokta A, D (M); cetvel C–M.
- **I.31:** dikme A, B, C (d doğrusu); d'nin AB'yi kestiği yere bas (D); dikme C, D, C.
- **I.46:** dikme A, B, A; pergel A–B, dikmeyle kesişime bas (D); dikme A, B, B; pergel B–A, kesişime bas (E, D ile aynı taraf); cetvel D–E.
- **II.14:** pergel B–C, çemberin AB doğrusunu B'nin ötesinde kestiği yere bas; orta nokta A ile bu nokta; pergel orta nokta–A, çemberin BC doğrusunu C'nin ötesinde kestiği yere bas (H); kare B, H, A.

## 5. Kod yapısı (tek dosya)
- **Geometri:** `lineLineX`, `lineCircleX`, `circleCircleX`; tolerans `TOL = 1.5` (dünya birimi), açı toleransı `ANG = 0.02` rad. Çember `{t:'circle', c, r}` ya da `{t:'circle', c, rad}` (taşınmış pergel); doğru `{t:'line', a, b}`, `given:true` olanlar gri.
- **Dünya ve görünüm:** seviyeler `WW=1000 × WH=600` sabit dünyada tanımlı; `view={s,tx,ty}` ekrana sığdırır; çizgi kalınlıkları ekran pikselinde sabit (`k=1/view.s`).
- **Seviye tanımı:** `LEVELS[]` içinde `given()` (noktalar, oran koordinatı), isteğe bağlı `objs()` (verilen doğrular), `goal` (HTML), `check()` (sağlanınca çizilecek nesneyi döndürür: `tri | seg | pt | line | poly | polys`), `done` (mesaj).
- **Araçlar:** `TOOLS{}`; `unlock: n` → seviye indeksi ≥ n ise açık (yani n+1. seviyeden itibaren). Kazanımlar yalnız ✓ işareti için: `localStorage['oklid2-won']`, son seviye `localStorage['oklid2-level']` (anahtar adı I.47'nin çıkarılmasıyla değişti; eski kayıtlar sıfırlanır).
- **Hamle kaydı:** her hamle `history`'de tek kayıt `{objs, pts}`; geri alma hamlenin doğurduğu her şeyi (kare aracının iki köşesi dahil) birlikte siler, hamleden hemen önce doğmuş ve başka nesnede kullanılmayan noktayı da geri alır. Kesişim adayları `candCache`'te tutulur, objeler/noktalar değişince boşalır.
- **Etkileşim:** `pick()` önce noktalara, sonra kesişim adaylarına bakar; vuruş yarıçapı fare için 16, dokunma için 26 px; yeni doğan nokta `fresh` işaretli, ilk tıklama seçmez (ilk seferde toast uyarır); dikme/paralelde üçüncü seçim ilk ikisinden biri olabilir. Kazanınca hedef metninin altına "Sonraki seviye" düğmesi gelir (`#winNext`).
- **Kazanma çizimi:** `winObj`, `draw()` sonunda yeşil (`OK`); kare seviyelerinde dolgu.

## 6. Genişletme
- Yeni seviye: `LEVELS`'a nesne ekle; `check()` mevcut yardımcılarla yazılır (`hasSeg`, `onLine`, `isPerp`, `isPar`, `findSquare`).
- Adaylar: I.22 (üç parçadan üçgen), I.23 (açı kopyalama), I.42–45 (alan dönüştürme; II.14'ün Öklid'deki ön adımı), "imkânsız seviye": açıyı üçe böl (Wantzel 1837). I.47 istenirse epilog olarak (oyuncu çizmeden, kanıt şekli gösterilerek) eklenebilir.
- Yapılmamış: ses, ipucu düğmesi (kaldırıldı), hamle sayacı, çoklu dil, çözümü SVG olarak dışa aktarma.

## 7. Kanal bağı
Haber Bilim'in "Öklid'in oyunu: cetvel, pergel, beş kural" Short'u ve Öklid dünya turu dokümanıyla aynı malzemeden; animasyonlar `geo/anim.py` ile (I.1, I.2 cetvel-pergel, 25 fps). Yapılacak: artifact paylaşımı açılıp bağlantı kanal açıklamalarına eklenecek.
