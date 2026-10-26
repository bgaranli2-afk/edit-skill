---
name: xbear-motion-kurgu
description: XBEAR MEDIA'nın krem kanvas + editoryal kart stilinde (Seçkin Ayazlar Reels/YouTube) Remotion ile animasyonlu video kurgusu. Konuşan kafa videosuna motion graphics, altyazı, sayaç, kart, geçiş ve ses efekti eklerken; sahne planlarken; tasarım sayfası hazırlarken kullan.
---

# xbear-motion-kurgu

Konuşan kafa videosunu, **iki dünya** arasında gidip gelen editoryal motion kurgusuna çevirir:
koyu **A-roll** (yüz + altyazı) ↔ krem **kanvas** (açıklama, kanıt, sayılar). Kurgu Remotion'la yapılır.

**Altın kural:** Hiçbir sahne onay alınmadan video olarak render edilmez. Önce **tasarım sayfası** (PNG) gösterilir.

## 0. Dosyalar (kök: `~/Desktop/seckin_ayazlar/`)

| Ne | Nerede |
|---|---|
| Bu skill + indeks | `06_skill/xbear-motion-kurgu/SKILL.md`, `REFERANS_INDEKSI.md` |
| Stil özeti (ölçümler) | `05_stil_calismasi/STIL_OZETI.md` (yalnızca kural belirsizse oku) |
| Animasyon kartları | `05_stil_calismasi/animasyonlar/<tür>/<id>/kart.md` + `in/peak/out.png` |
| Marka (her şeyden önce gelir) | `02_marka/marka_notu.md`, `fontlar/`, `logolar/` |
| Remotion projesi | `remotion-studio/` → bileşenler `src/xbear/` |
| Transkript aracı | `remotion-studio/scripts/transcribe.sh <video> <çıktı> tr` → `<ad>.words.json` |
| Proje çıktıları | `07_projeler/<proje>/` (tasarım sayfaları, final MP4, sahne listesi) |
| Bileşen testleri | `05_stil_calismasi/bilesen_testleri/` |
| Ses efektleri | `03_sfx/<tür>/` (kaynak) → `scripts/sfx_esitle.py` → `public/sfx/` + `src/xbear/ses/kutuphane.ts`. Ölçüm: `05_stil_calismasi/SES_ANALIZI.md` |

Çakışmada sıra şöyledir: `marka_notu.md` > bu dosya > `STIL_OZETI.md`.

## 1. Stil kuralları

### Dünyalar
- **A-roll** (~%30): koyu stüdyo, yüz, altyazı. Neredeyse hiç altyazısız kalmaz.
- **Kanvas** (~%70): `#F5F2EF` zemin, hafif ızgara, yumuşak çapraz pencere ışığı (`<KremZemin>`).
- A-roll ↔ kanvas geçişi **her zaman sert kesmedir**. Kayma, silme ve çapraz geçiş yoktur.

### Renk (`tema.ts → RENK`)
| Rol | HEX |
|---|---|
| Kanvas / ızgara | `#F5F2EF` / `#ECE9E6` |
| Kart | `#FFFFFF` |
| Ana metin / ikincil | `#111111` / `#8A8A8A` |
| Vurgu mavi (UI / italik metin) | `#2782F5` / `#1E90E8` |
| Para yeşili | `#16A06B` |
| Uyarı kırmızısı (el çizimi, son tarih, olumsuz) | `#E35B63` |
| Ürün ışıması | `#864EC6` → `#3C296B` |
| A-roll siyahı / altyazı | `#030303` / `#FFFFFF` |

**Cümle başına en fazla bir vurgu rengi kullanılır:** mavi = ana fikir, yeşil = para, kırmızı = son tarih veya kayıp.

### Tipografi (`tema.ts → FONT, BOYUT`; 1080×1920 için)
- **Cümle / altyazı:** neo-grotesk sans (Instrument Sans 500). Kanvasta 84px, A-roll altyazısında 64px.
- **Vurgu ifadesi:** mavi *italik serif* (Instrument Serif Italic), sans boyutunun 1.3 katı. Cümlede **1 ifade, 2–4 kelime** olur; asla kalın veya altı çizili değildir.
- **Sayılar:** ağır italik grotesk (Inter Black Italic), tabular rakamlar. Asla serif kullanılmaz. Para ise yeşildir.
- **Etiket:** gri, aralıklı büyük harf (`letter-spacing .16em`).
- En fazla 2 satır yazılır. Metin ortalanır veya boş üçlüğe yaslanır, tam genişlik kullanılmaz. Türkçe karakterler doğru yazılır (ğ, ş, ı, İ).
- Marka fontu gelince yalnızca `src/xbear/fontlar.ts` değişir.

### Hareket (`tema.ts → KARE, EASE`)
| Öğe | Hareket | Süre |
|---|---|---|
| Kelime | opaklık 0→1 + bulanıklık 8→0, yerinde (satır kaymaz) | 5 kare, kelimenin söylendiği anda |
| Kart / obje girişi | ~60px yükselir + fade + bulanıklık 10→0 + ölçek .95→1, ease-out | 14 kare |
| İkon / rozet | ölçek .6→1, hafif aşma (spring) | 8 kare |
| Metin çıkışı | 40px yukarı süzülür + bulanıklaşır, ease-in | 10 kare |
| Kanvas bölüm değişimi | odak kaybı | 9 kare |
| Kart yana çıkışı | yatay hareket bulanıklığı | 8 kare |
| Kamera | kanvasta sürekli yavaş sürüklenme; A-roll'da anında yeniden kadraj (1.15–1.3×) | |
| Sayaç | ease-out sayma; para son değerde yeşile döner | 20–90 kare |

Her hareket **≤15 karede** yerine oturur. Tek izin verilen fazlalık hafif aşmadır.

### Ritim
- Ortalama ~6–8 saniyede yeni bir görsel fikir gelir (v01'de medyan 6,4 sn).
- Bir sahnenin içinde her 1–2 saniyede bir şey değişir: kelime, kart veya sayaç tiki.
- Dakikada ~5 sert kesme olur. A-roll bölümleri medyan ~8–9 saniye sürer.
- Kanca (ilk 3–5 sn): A-roll + altyazı + işaretli kritik sayı + bir ikon.

### Cümle → animasyon
| Cümle tipi | Ne gelir |
|---|---|
| Tarih / son tarih | Takvim ikonu + kırmızı tarih |
| Liste ("X, Y ve Z") | Her madde kendi kelimesinde bir kart + artan sayaç hapı |
| Para / gelir | Yeşil ağır italik sayı, çoğu zaman sayarak gelir |
| Sonuç / "insanlar dedi ki" | Kanıt kartları (~1 sn'de bir) |
| Fiyat hesabı | Satır satır kurulur, sonra çubuk karşılaştırma |
| Saklanan bilgi | Mozaik + "sonda açıklanacak" hapı |
| Ürün / araç adı | Ürün kartı veya uygulama ikonu |
| Seçim sorusu | A-roll üstünde iki buton |
| Duygusal / kişisel cümle | **Sadece A-roll + altyazı** |
| Harekete geçirici çağrı (CTA) | "Linke tıkla" + alttan mavi ışıma + buton tıklama |

### Yasaklar
- Neon, glitch, RGB kayması, film yanığı, ışık sızması, emoji, sticker
- Metinde gölge, konturlu metin, degrade metin, BÜYÜK HARF altyazı, kutulu/plakalı altyazı
- Dönme, 3D takla, lastik gibi esneyen easing
- Tam ekran renkli zemin: zemin ya krem kanvas ya da koyu A-roll'dur
- 2 satırdan fazla metin, cümlede birden fazla vurgu
- A-roll ↔ kanvas arasında kayma veya çapraz geçiş
- Güvenli alanın dışına taşmak: üst %8, alt %15, sağ %12 boş kalır. Altyazı yüzü kapatmaz.

### Ses (ölçüm: `SES_ANALIZI.md`)
| Olay | Ses | Not |
|---|---|---|
| A-roll ↔ kanvas kesmesi | `whoosh` | `Sahne.dunya` değişince `SahneDizisi` otomatik koyar (kesmelerin ~%60'ı) |
| Kart, sayı, hap girişi | `swish` | `KartGiris`, `UIKart`, `SayiSayaci` kendiliğinden çalar |
| Kartın yana çıkışı, odak kaybı | `whoosh` | Sonraki kesme whoosh'una 0,5 saniyeden yakınsa kapat (`cikisSesi={false}`) |
| Kanca reframe'i, payoff/açıklama, CTA | `boom` | Dakikada ~1. `ZoomIn ses="boom"` ya da `<SesEfekti tur="boom">` |
| Rozet / el çizimi / buton | `pop` / `kalem` / `klik` | |
| Gizli bilgi açılmadan önce | `riser` | Tepesi açıklama anına oturur |
| Sayaç | — | Tik yok (`tikSesi` varsayılan kapalı) |
| Para sayacı yeşile döndüğü an | `para` (ka-ching) | `SayiSayaci para` otomatik çalar (`paraSesi={false}` kapatır). Referansta yok, XBEAR tercihi |
- Sesin **tepesi görsel olaydan 2–3 kare sonra** gelir (`TEPE_GECIKME_KARE`). Bileşenler bunu otomatik hizalar.
- SFX tepesi konuşmanın **~10 dB altında** durur (`SEVIYE_DB`: swish -17, whoosh -14, boom -12 dBFS). Genel ayar için `<SesAyari kaydirmaDb={-3}>` kullanılır.
- Yasak: whoosh seli, her kelimeye pop, sayaç tiki, aynı dosyanın art arda çalması.
- Ham Epidemic dosyası doğrudan `03_sfx/<tür>/` klasörüne atılır. `sfx_esitle.py` içindeki varyantları ayırır (_v1, _v2…), sessizliği ve kuyruğu kırpar, normalize eder.

### Seçkin kanal kuralları (`02_marka/marka_notu.md`)
- Küfür kesilir. Sessiz bekleme ve senaryoya bakıp ezberleme anları kesilir. Kurgu çekim sırasını takip eder.
- Müzik yalnızca Epidemic Sound'dan seçilir, -15 dB (gürse -20 dB), yalnızca intro'da ve sessiz yerlerin altında.
- Renk düzeltme yapılmaz.

## 2. İş akışı

### Adım 1 — Plan
1. Transkript al: `remotion-studio/scripts/transcribe.sh <ham.mp4> 07_projeler/<proje>/ tr`.
2. Kesilecek yerleri işaretle: sessizlik, ezber, küfür.
3. Sahne listesini **tablo** olarak yaz ve kullanıcıya göster:
   `S01 | 00:00.0–00:04.8 | A-roll / kanvas | ne söyleniyor | ne gelir (cümle tipi) | bileşen(ler) | ses`.
4. Ritim kontrolü: ~6–8 saniyede bir yeni fikir olmalı, art arda üç A-roll sahnesi olmamalı.

### Adım 2 — Referans seçimi
- **Yalnızca `REFERANS_INDEKSI.md` okunur.** Her sahne için "ne zaman" ve anahtar kelimelere bakılıp **2–3 satır** seçilir.
- Sadece o satırların `kart.md` ve gerekirse `peak.png` dosyası açılır. `clip.mp4` açılmaz, klasör toplu yüklenmez.
- Sahne listesine `ref v01-17` gibi not düşülür.

### Adım 3 — Tasarım sayfası
1. Proje klasörü oluşturulur: `remotion-studio/src/projeler/<proje>/`.
   - `sahneler.tsx` → `Sahne[]` listesi (`id, ad, sure, kareler, not, dunya, Bilesen`).
   - Her sahnenin `kareler` dizisine 2–3 kritik kare girilir: giriş bitti, tepe, çıkıştan hemen önce.
   - `Kayit.tsx` iki kompozisyon kaydeder: `<Proje>-Video` (`SahneDizisi`) ve `<Proje>-Tasarim` (`TasarimSayfasi`). Ikisi de `Root.tsx`'e eklenir.
   - Tasarım kompozisyonu **`<Composition>`** olmalıdır (`<Still>` değil), `{...tasarimSayfasiBoyutu(sahneler)}` ile. Yoksa kareler 0'a sıkışır.
2. `03_sfx` değiştiyse önce sesleri eşitle: `python3 -I remotion-studio/scripts/sfx_esitle.py`. Betik, sentetik yer tutucu kullanan türleri listeler; bunu kullanıcıya söyle.
3. Ham video `remotion-studio/public/ham/<proje>/` içine konur, `<ARoll kaynak="ham/<proje>/x.mp4" baslangicSn={…} />` ile çağrılır.
4. Render: `npx remotion still <Proje>-Tasarim ../07_projeler/<proje>/tasarim_v1.png --frame=0`
5. PNG'ye kendin bak. Güvenli alan, taşma, 2 satır sınırı ve Türkçe karakterleri kontrol et. Sonra kullanıcıya göster.

### Adım 4 — Onay (zorunlu)
- Kullanıcı açıkça onay verene kadar `remotion render` **çalıştırılmaz**. Tek sahnelik deneme render da yapılmaz.
- Düzeltme istenirse tasarım sayfasının yeni sürümü (`tasarim_v2.png`) çıkarılır.

### Adım 5 — Render
- `npx remotion render <Proje>-Video ../07_projeler/<proje>/final_v1.mp4 --port=3199`
- Çıktıyı ffprobe ile doğrula: 1080×1920, 30 fps, beklenen süre, ses akışı var.
- `ffmpeg -i final.mp4 -af volumedetect -f null -` → max_volume ≤ -1 dB olmalı (kırpma olmamalı).
- Final videoyla birlikte sahne listesi teslim edilir: `zaman | tip | ekrandaki yazı | geçiş | referans`.

### Adım 6 — Revizyon
- Yalnızca istenen sahne değiştirilir. Önce o sahnenin tasarım sayfası (sadece değişen sahneler) gösterilir, onaydan sonra video yeniden render edilir.
- Dosyalar sürüm numarasıyla tutulur (`final_v2.mp4`). Eski sürümler silinmez.

## 3. Remotion yapısı ve bileşenler

```
remotion-studio/
  public/fonts/            Instrument Sans/Serif, Inter (OFL) → marka fontu gelince public/fonts/marka/
  public/ornek/            aroll_yer_tutucu.mp4 (yalnızca testler için)
  public/sfx/<tür>/        sfx_esitle.py'nin normalize ettiği sesler (elle dokunma)
  public/ham/<proje>/      ham A-roll (her proje için)
  src/Root.tsx             fontlariYukle() + tüm kompozisyonlar
  src/xbear/               ← yeniden kullanılan kütüphane, "./xbear"tan içe aktar
    tema.ts zamanlama.ts fontlar.ts TasarimSayfasi.tsx index.ts
    bilesenler/*.tsx
    ses/                   sesler.ts (seviye/hizalama kuralları), SesEfekti.tsx, kutuphane.ts (otomatik)
    ornekler/              her bileşenin örnek kullanımı (XB-* kompozisyonları)
  src/projeler/<proje>/    sahneler.tsx, Kayit.tsx, kelimeler.json
```

| Bileşen | Ne için | Örnek kullanım |
|---|---|---|
| `KremZemin` | Kanvas zemini + sürüklenme | `<KremZemin>…</KremZemin>` |
| `VurguMetni` | Kanvas cümlesi, kelime kelime; `[ ]` = vurgu, `\n` = satır | `<VurguMetni metin={"Şarj sadece 30 dakika,\n[rakiplerinden hızlı]"} isaret={{ifade:"30 dakika", tip:"altcizgi"}} cikisKaresi={108}/>` |
| `ElCizimi` | Kırmızı el çizimi elips/altçizgi (tek kritik ifade) | `VurguMetni.isaret` ile ya da `<ElCizimi tip="elips" baslangic={20}/>` |
| `SayiSayaci` | Büyük sayı sayma; `para` → yeşil | `<SayiSayaci etiket="BAŞLANGIÇ FİYATI" bas={39999} son={42999} sonEk=" ₺" para baslangic={12}/>` |
| `UIKart` | Beyaz yüzen kart veya `varyant="urun"` hero kart; rozet, etiket, ikon | `<UIKart varyant="urun" baslik="Nova X1 Pro" altBaslik="İNCELEME · 2026" rozet="+ YENİ"/>` |
| `Hap` | Etiket, sayaç (+3), son tarih, CTA | `<Hap metin="Kutudan çıkanlar" sayi="+3"/>` |
| `KartGiris` | Her objeye ortak giriş/çıkış (`yukari` / `yana`) | `<KartGiris baslangic={14} cikisKaresi={128} cikisTipi="yana">…</KartGiris>` |
| `AltYaziBandi` | A-roll altyazısı, words.json'dan, en fazla 2 satır | `<AltYaziBandi kelimeler={sahneKelimeleri(words, 12.4, 18)} vurgular={["30","gün"]}/>` |
| `ZoomIn` | `atlama` (anında reframe) · `punch` · `itme` · `suruklenme` | `<ZoomIn mod="atlama" kare={66} olcek={1.25} odak={{x:.5,y:.3}} ses="boom">…</ZoomIn>` |
| `ARoll` | Ham videoyu 9:16 kırpar, `baslangicSn` ile keser | `<ARoll kaynak="ham/p/x.mp4" baslangicSn={12.4}/>` |
| `Gecis` | `kesme` · `bulanikYukari` · `odakKaybi` · `yanaKayma` · `karartma`; içeriği sarar (zemini değil) | `<KremZemin><Gecis tip="odakKaybi" sure={120} giris={false}>…</Gecis></KremZemin>` |
| `SesEfekti` / `SesAyari` | Tek SFX (tür + kare; tepe hizalı, dosya döndürmeli) / genel aç-kapa, dB kaydırma | `<SesEfekti tur="boom" kare={66}/>` |
| `TasarimSayfasi` / `SahneDizisi` | Onay PNG'si / final dizilim + kesme sesleri | bkz. Adım 3 |

Zamanlama yardımcıları (`zamanlama.ts`): `kare(sn)`, `sahneKelimeleri(words, bas, son)`, `kelimeKaresi(kelimeler, "otuz")`.
Animasyonu **kelimeye bağla**: başlangıç karesi words.json'dan alınır, elle tahmin edilmez.

Bileşenler `useVideoConfig()`'dan yalnızca `fps` okur, ölçüler 1080×1920 sabittir. Böylece tasarım sayfasında da doğru görünürler.
Bir sahneye `sure` ve `cikisKaresi` açıkça verilir.

**16:9 / hibrit (Hibrit.tsx):** `KoyuCamZemin`, `CamKart`, `TakipEtiketi` (takipli nokta+çizgi+hap), `BolumEtiketi`, `UygulamaKarosu` (`gorsel` = gerçek logo), `TelefonCerceve`, `KilitKatmani`, `RotoKatmani`. Liste kartı: `projeler/duo/Parti2.tsx → ListeKarti + OzellikSatiri (var/yok/bilgi)`.
**Henüz yazılmadı:** `TarayiciCerceve`, `Stepper`, `Mozaik`.
İndekste karşılığı olan referanslar: v01-10/14/47 (telefon), v01-08/13/43 (tarayıcı), v01-26/55 (ikon), v01-51 (stepper), v01-04/58 (mozaik), v01-46 (buton), v01-56 (liste).

## 4. Ortam ve komutlar

- **Mac** (Apple M1 Pro): Node 24, ffmpeg 9, whisper.cpp + large-v3-turbo. Kurulum ve sürümler `KURULUM.md`'de, doğrulama için `kurulum_dogrula.command`.
- Oturumun kabuğu Mac değil de **Linux VM** ise:
  - `remotion-studio/node_modules` Mac'e özgüdür, VM'de `npm i` **çalıştırılmaz**.
  - Render için projeyi bulut çalışma alanına kopyala, oraya aynı sürümlerle `npm i` yap (`remotion@4.0.538`) ve Chromium ile render et (`--browser-executable=…/chrome-headless-shell`). Sonuçları klasöre geri yaz.
  - Whisper'ı ise kullanıcı Mac'te çalıştırır, ya da `.command` betiği hazırlanır.
  - `sfx_esitle.py` VM'de de çalışır (python3 + ffmpeg). Eski dosyaları silmez.
- Studio (canlı önizleme): `cd remotion-studio && npm run dev`. Port çakışırsa render'a `--port=3199` eklenir.
- Tip kontrolü: `npx tsc --noEmit`.
- Bileşen testleri: `npx remotion render XB-<Bileşen> ../05_stil_calismasi/bilesen_testleri/XB-<Bileşen>.mp4`
- Ses kontrolü: `npx remotion render XB-Ses ../05_stil_calismasi/bilesen_testleri/XB-Ses.mp4` (her türü sırayla çalar)
- Örnek tasarım sayfası: `npx remotion still XB-TasarimSayfasi ../05_stil_calismasi/bilesen_testleri/tasarim_sayfasi.png --frame=0`

## 5. Revize dersleri (Duo, Ekim 2026) — her projede uygula
**Logo ve ikon**
- Marka geçiyorsa **gerçek logo** kullanılır (kullanıcının verdiği PNG → `public/ham/<p>/ek/`; yoksa npm `simple-icons`). Harf, benzer ikon ya da renkli kare ile taklit edilmez.
- İkon anlamla birebir eşleşir. Kaynak `lucide-static` (beyaz çizgi, `public/ikon/<ad>.svg`), flaticon da olur. Aynı ikon farklı anlamda kullanılmaz:
  kamera→`camera` · diyafram→`aperture` · çocuk→`baby` · evcil hayvan→`paw-print` · cüzdan→`wallet` · para/pahalı→`banknote` · MagSafe→mıknatıs halkası · Apple ekosistemi→Apple logosu · kapak/katlanma ömrü→katlanır telefon · 3D baskı→`printer` · geri dönüşüm→`recycle`. Face ID ikonu sadece Face ID içindir.
- Bilgi taşımayan madde eklenmez ("tak diye tutuyor", "kaymıyor" gibi).

**Yerleşim ve zaman**
- Kanvas sahnelerinde içerik ekranda **tam ortalıdır**: dikey ve yatay, başlık ve gövde bir blok hâlinde.
- "Durdurup okuyun" tablosu ortalıdır ve **bir sonraki konu cümlesine kadar** ekranda kalır.
- Kişi eliyle bir yeri gösteriyorsa grafik **gösterdiği tarafa ve gösterdiği anda** gelir. El grafiğin önüne geçiyorsa roto yapılır (RVM, finalde 60 fps maske).
- Bölünmüş ekranda (multitasking) her etiket kendi uygulamasının yarısının üstündedir.
- Takip etiketinin noktası **söylenen parçanın tam üstündedir**: Touch ID = başparmağın üstündeki uzun tuş, ses tuşları = kenardaki iki tuşun arası. Optik akış kenarda kayar. 3–4 anahtar karede büyütülmüş ızgara üzerinde elle işaretlenir, aradaki kareler interpole edilir.
- Kişi kendi sosyal medya içeriğinden bahsederse (reel, test videosu): telefon çerçevesinde o içerik + platform logosu, gerekirse profil kartı + "Takip Et → Takiptesin".

**Kurgu ve gizlilik**
- Transkriptte art arda tekrar eden kelime veya ifade aranır ("gördüğünüz bir, gördüğünüz bir…"). Tekrarın biri sessiz aralıktan kesilir, seste 15 ms crossfade yapılır.
- Ekranda mesaj, kişi, not, şifre veya e-posta okunuyorsa **blur** uygulanır (final aşamasında bölge blur'u). Kullanıcı söylemese de yapılır.

**Final (4K60 yatay)**
- Grafikler katman olarak render edilir: `--props '{"katman":true}'` ile `ARoll` ve `RotoKatmani` şeffaf kalır. PNG dizisi → ffmpeg ile VP8 alfa, 1080p30 (Remotion'ın vp8 kodlaması çok yavaş).
- Birleştirme `07_projeler/<p>/final/final.py` ile yapılır: 1080→4K lanczos, katman overlay, roto maskesi (`tpad` ile parça boyuna doldurulur, yoksa bellek dolar), blur, kesim, x264 veryfast 30 Mbps, ~33 sn'lik parçalar halinde. Ardından `birlestir`. Mac VM'de komut başına 180 sn sınırı var: birleştirmeyi önce yerel diske yap, klasöre `dd` ile parça parça kopyala.
- Container → Mac dosya sınırı 30 MB'dır. Daha önce gönderilmiş bir yola yeniden göndermek eski dosyayı teslim edebilir: yeni yoldan gönder ve md5 ile doğrula.

## 5b. Revize dersleri (Watch Ultra 4, Ekim 2026) — her projede uygula
**Eski geçişler (mor bumper / silme)**
- Ham videoda editörün kendi geçişleri (mor "KUTU AÇILIŞI", "5 GÜN SONRA", "SAĞLIK VERİLERİ", "TEKNİK ÖZELLİKLER" vb.) seçilen stille çakışıyorsa **hepsi** bulunur ve değiştirilir; tek bir kare mor bile görünmez.
- Bulma: proxy'de mor piksel maskesi `(B>60)&(R>40)&(G<45)&(B>G+30)` kare kare taranır. Silmenin eğrisi çıkarılır: girişte morun üst kenarı, çıkışta alt kenarı (satır, saniye).
- Yerine `KremPerde` (`projeler/watch/Parti1.tsx`, `MOR_SILME`) konur. Perde bu eğriyi **%7 güvenlik payıyla** takip eder. Girişte bir sonraki karenin kenarı kullanılır, böylece 60 fps ara kareler de kapanır. Perdenin zamanı `sahneBas + kare/30` ile hesaplanır, yuvarlama kayması olmaz.
- Doğrulama: render edilen her karede aynı mor maskesi 0 çıkmalı. `kareler.mjs` ile pencere kare kare render edilip taranır.

**Kameraya bakmadığı anlar → stok**
- Pencere tahminle değil, yüz bölgesinin 0,2 sn adımlı kırpıntı ızgarasıyla bulunur: kafanın döndüğü ilk kare ve kameraya döndüğü ilk kare.
- Stok, kafa dönmeden **0,2–0,3 sn önce** tam opak olur ve kişi kameraya döndükten **sonra** çıkar. Geçiş 4 karedir. Seyirci bakmadığı anı hiç görmez.
- Stok yalnızca konuşulan ürünü gösterir (ör. Ultra 4; Series 12 karesi, İngilizce fiyat/UI ızgarası kullanılmaz). Klibin başındaki siyah kareler `kirp` ile atlanır. Köşeye "Görüntü: Apple" yazılır.

**Bilgi doğruluğu**
- Konuşmacı sürçerse ("Ultra 4'ler güncelleme almıyor" deyip Ultra 1'i kastetmek gibi) ekranda doğrusu yazılır.
- Resmî sayfada olmayan rakamlar kullanıcıya listelenir. Doğrulanamayan ve kritik olmayan değer (ör. kablo uzunluğu) ekrana konmaz.
- Transkriptte görünen tekrarlar ses enerji eğrisiyle doğrulanır. Whisper bir cümleyi sıkıştırıp iki kez yazabilir, gerçekte bir kez söylenmiş olabilir. Kesim sadece gerçekten tekrarlanan ifadede ve kelime arasından yapılır.

**Uzun "sabit panel" bölümü (teknik özellikler)**
- Bölüm boyunca sabit düzen kullanılır: krem kanvas, solda yuvarlak çerçevede A-roll, sağda beyaz panel. Panel sayfalara bölünür, her özellik söylendiği an satır olarak gelir ve sayfa bitene kadar kalır. Üstte adım şeridi olur, biten adıma tik gelir. Kapak, kanalın eski kartının yerine stok görüntüyle yapılır.
- Final kalitesi için çerçeve **hareketsizdir**. Katmanda kanvasa `clip-path: path(evenodd, …)` ile yuvarlak köşeli delik açılır (`katman` modunda çerçeve şeffaf). `final.py` `INSET` bu deliğin altına 60 fps kaynağı aynı dikdörtgene basar (cover ölçek + ortadan kırpma, sonda alfa fade). Kapak, çerçevenin üstünde söner.
- Opak/uzun katmanlar 30 MB sınırı yüzünden 300 karelik VP8 parçalara bölünür (`katman_seg.sh` → `SID_000.webm …`). `final.py` parçaları kendiliğinden sıraya koyar.

## 6. Teslim öncesi kontrol
- [ ] Tasarım sayfası onaylandı, onay mesajı tarihiyle not edildi.
- [ ] Her vurgu, sayaç ve kart kendi kelimesinde giriyor (words.json).
- [ ] Cümlede en fazla 1 vurgu, en fazla 2 satır. Para yeşil, son tarih kırmızı.
- [ ] A-roll ↔ kanvas sert kesme. Yasak efekt yok. Güvenli alan boş.
- [ ] Türkçe karakterler doğru. Küfür ve sessiz yerler kesildi.
- [ ] Sesler gerçek dosyalarla çalıyor (sentetik değil), iki whoosh üst üste binmiyor, kırpma yok.
- [ ] Logolar gerçek, ikonlar anlamla eşleşiyor, kanvas içerikleri ortalı, takip noktaları doğru parçada.
- [ ] Gizli bilgi (mesaj, not, şifre) blur'lu, tekrar eden ifade kesildi.
- [ ] Ham videodaki eski stil geçişlerin hepsi değişti, mor maskesi tüm karelerde 0.
- [ ] Stok pencereleri kare kare: kameraya bakmadığı tek kare bile görünmüyor.
- [ ] Çıktı formatı doğru (dikey 1080×1920 30 fps ya da yatay 4K 59.94 30 Mbps). Sahne listesi tablosu teslim edildi.
