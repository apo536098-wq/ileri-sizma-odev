Frida ile Pratik Komutlar

Bu çalışmada Frida 17 kullanılarak C dili ile yazılmış ikili (binary) bir dosya üzerinde çalışma zamanlı dinamik analiz ve fonksiyon bypass işlemleri gerçekleştirilmiştir

## Ekran Görüntüleri

1. **Hazırlık ve Derleme:** `-rdynamic` bayrağı ile `hedef.c` dosyasının derlenmesi.
2. **Gözlem (İzleme):** `gozlem.js` betiği ile `kontrol` fonksiyonunun izlenmesi (`[gozlem] kontrol('yanlis') = 0`).
3. **Değiştirme (Hook / Bypass):** `degistir.js` betiği ile dönüş değerinin manipüle edilmesi (`Giris basarili`).

---

## Teknik Açıklama

`retval.replace(1)` Neden Programı "Giris basarili" Yaptı?

Frida, `Interceptor.attach` mekanizması ile `kontrol` fonksiyonunun bellekteki çalışma akışına kanca (hook) atar. Fonksiyon ana akışa geri dönmeden hemen önce tetiklenen `onLeave` bloğunda, CPU kaydedicisinde (register) tutulan gerçek dönüş değeri (`0`) `retval.replace(1)` komutuyla bellekte dinamik olarak `1` ile değiştirilmiştir. `hedef.c` içerisindeki `if (kontrol("yanlis"))` koşulu sıfırdan farklı olan `1` değerini `true` (doğru) olarak algıladığı için, fonksiyona hatalı parola gönderilmiş olmasına rağmen akış başarı koluna sapmış ve ekrana `"Giris basarili"` yazdırılmıştır.
