# Kız Kulesi — three.js gerçekçi deniz sahnesi

Üsküdar açıklarında Kız Kulesi'nin altın saat (gün batımı) ışığında işlendiği, tarayıcıda
çalışan tek dosyalık bir WebGL sahnesi. Kurulum, derleme veya sunucu gerekmez; `index.html`
dosyası tarayıcıda açıldığında sahne kendiliğinden yüklenir.

Yayın adresi: https://jilazem.github.io/kiz-kulesi/

## Sahne nedir

- Gerstner dalga modeliyle hareket eden, yansıma/parıltı ve köpük detayı içeren deniz yüzeyi.
- Kule, çevresindeki kayalık ada, iskele ve çevre siluetleri prosedürel geometriyle üretilir.
- Altın saat atmosferi: alçak güneş, sıcak tonlu gökyüzü, gölge ve hacim hissi veren bloom.
- Martılar ve sahne içi hareketli öğeler animasyon döngüsünde yer alır.
- Ekran görüntüleri `ekran/` altında: shot-7, shot-8, shot-9, shot-10.

## Kontroller

| Girdi | İşlev |
|---|---|
| Sol tuş sürükle | OrbitControls ile sahneyi döndürme |
| Tekerlek | Yakınlaşma / uzaklaşma (zoom) |
| W A S D / ok tuşları | Uçan kamera ile ilerleme |
| Shift | Uçan kamerada hızlanma |
| Q / E | Alçalma / yükselme |
| F | Serbest bakış (pointer lock) aç/kapat |
| Esc | Serbest bakıştan çıkış |
| Sağ üst panel | "Kalite: Yüksek/Düşük", "Bloom: Açık/Kapalı", "Serbest Bakış (F)" düğmeleri |

Kalite anahtarı sağ üstteki **Kalite** düğmesidir. "Düşük" seçildiğinde piksel oranı 1'e iner,
gölge haritası, SSAO ve bloom kapanır, deniz ve ada çözünürlüğü azaltılır. Aynı panelden bloom
ayrıca tek başına kapatılabilir. Sol altta ms/kare, çizim çağrısı ve üçgen sayısı göstergesi vardır.

## Teknik notlar

- Motor: three.js 0.160.0 (jsDelivr CDN üzerinden ES modül import map ile yüklenir; internet bağlantısı gerekir).
- Deniz: Gerstner dalga toplamı ile vertex shader deformasyonu; `uDetail` uniform'ı kaliteye bağlıdır.
- Son işlem hattı: RenderPass → SSAOPass (kernel 0.85) → UnrealBloomPass (0.42, eşik 1.06) → OutputPass/FXAA.
- Geometri: 236 mesh (bunun 145'i kule grubunda). Yüksek kalitede ölçülen kare başına
  466 çizim çağrısı ve ~281.000 üçgen (M4 / ANGLE Metal, 2× DPR sınırı 1.75).
- Sahne, hata ayıklama için `window.__sahne` nesnesini yayımlar: `bilgi()`, `meshSay()`, `kuleMeshSay()`,
  `kameraAyarla()`, `kaliteAyarla()`, `bloomAyarla()`, `olcum()`, `hatalar`.
- Tek dosya, harici görsel/doku bağımlılığı yok; tüm dokular prosedürel/canvas ile üretilir.

## Gereksinimler

- WebGL2 destekli güncel tarayıcı (Chrome, Edge, Firefox, Safari). WebGL2 yoksa sahne açılmaz.
- Donanım hızlandırmalı GPU önerilir; düşük kalite modu zayıf donanım için ayrılmıştır.
- CDN erişimi (three.js modülleri için).

## Yerel çalıştırma

```
git clone git@github.com:Jilazem/kiz-kulesi.git
cd kiz-kulesi
open index.html          # macOS
```

Not: ES modül import map kullanıldığı için doğrudan dosya olarak açmak sorunsuz çalışır;
katı CORS politikası olan tarayıcılarda `python3 -m http.server` ile servis etmek güvenli yoldur.
