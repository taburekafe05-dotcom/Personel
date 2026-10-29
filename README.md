# Personel Takip (tablet uygulaması)

Personel kendi adına dokunur, **kendi şifresini** girer ve **Giriş / Çıkış** yapar.
Tüm veriler **tabletin kendi hafızasında** tutulur; internet gerekmez.

## Nasıl çalışır
- İlk açılışta **yönetici şifresi** belirlenir.
- Yönetici → **Personel** sekmesinden kişi ekler (ad + o kişiye özel şifre).
- Her gün için: bulunma süresinden **30 dk mola otomatik düşülür**, kalan net süre **8,5 saat** hedefle karşılaştırılır (artı/eksi fark gösterilir).
  Örnek: 08:30 giriş – 17:30 çıkış = 9 sa bulunma − 30 dk mola = 8 sa 30 dk net, fark 0.
- Unutulan çıkışlar "Çıkış yok" olarak işaretlenir; yönetici satıra dokunup düzeltir.
- Yönetici → **Kayıtlar**: aylık tablo, ay toplamı, CSV (Excel) dışa aktarma.
- Yönetici → **Ayarlar**: çalışma saati (8,5) ve mola (30 dk) değiştirilebilir; yedek al / geri yükle.
- Bir personel 5 kez yanlış şifre girerse 1 dakika kilitlenir. Şifreler düz metin değil, tuzlu özet (hash) olarak saklanır.

## APK üretme

### Yol 1 — Bilgisayarda hiçbir şey kurmadan (GitHub, ücretsiz)
1. https://github.com adresinde ücretsiz hesap açın, **New repository** ile boş bir depo oluşturun.
2. Bu klasörün içindekileri (`www`, `.github`, `package.json`, `capacitor.config.json` …) depoya yükleyin
   (**Add file → Upload files**; `.github` klasörünü de sürükleyin).
3. Depoda **Actions** sekmesi → **APK derle** → **Run workflow**. Yaklaşık 5–10 dakika sürer.
4. Bitince çalıştırmanın sayfasındaki **Artifacts → personel-takip-apk** dosyasını indirin, zip'ten `app-debug.apk` çıkar.
5. APK'yı tablete aktarın, açın. "Bilinmeyen kaynaklardan kuruluma izin ver" sorulursa izin verin.

### Yol 2 — Kendi bilgisayarınızda
Gerekenler: Node.js 20+, JDK 21, Android Studio (Android SDK için).
```
npm install
npx cap add android
npx cap sync android
cd android && ./gradlew assembleDebug
```
APK: `android/app/build/outputs/apk/debug/app-debug.apk`

## Önemli
- Veriler tablette saklanır: uygulamayı **silmek** veya **verilerini temizlemek** her şeyi siler. Ayarlar → **Yedek al** ile düzenli yedek alın.
- Yeni sürüm APK'yı eskisinin üstüne kurarak güncelleyebilirsiniz; veriler korunur (aynı imza anahtarıyla derlenmişse). Farklı bilgisayar/derlemeden gelen APK imzası farklı olabilir; bu durumda önce yedek alıp eski uygulamayı silin, yeniyi kurup yedeği geri yükleyin.
