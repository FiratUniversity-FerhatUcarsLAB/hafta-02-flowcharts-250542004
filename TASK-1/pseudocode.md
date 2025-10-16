ATM para çekme sisteminin basit bir pseudocode'u aşağıdadır:

BAŞLA

// --- Başlangıç değişkenleri (hesap sahibi/ATM bilgileri) ---
hesap_PIN <- 1234
hesap_bakiye <- 8500         // örnek bakiye (TL)
gunluk_limit <- 3000         // hesap için günlük çekim limiti (TL)
gunluk_cekilen <- 0          // o gün içinde çekilmiş toplam (TL)
maks_pin_hakki <- 3

// --- Kullanıcı ATM'ye gelir ---
DONGÜ (true)  // Genel işlem döngüsü: kullanıcı işlem yapmayı tekrar edebilir
    YAZ "Kartınızı takınız veya kart bilgisi giriniz..."
    // PIN doğrulama
    deneme <- 0
    pin_kabul <- HAYIR

    DONGÜ (deneme < maks_pin_hakki)
        YAZ "Lütfen PIN'inizi giriniz:"
        OKU girilen_PIN

        EĞER (girilen_PIN = hesap_PIN) İSE
            pin_kabul <- EVET
            YAZ "PIN doğrulandı."
            DONGÜDEN_CIK  // PIN döngüsünden çık
        DEĞİLSE
            deneme <- deneme + 1
            kalan <- maks_pin_hakki - deneme
            EĞER (kalan > 0) İSE
                YAZ "Yanlış PIN. Kalan hak:" , kalan
            DEĞİLSE
                YAZ "PIN haklarınızı tükettiniz. Kart bloke edildi."
                YAZ "Lütfen bankanız ile iletişime geçin."
                // İşlem sonlandırılır; karta elveda
                DONGÜDEN_CIK  // PIN döngüsünden çık (burada program ana döngüden de çıkacak)
            Fİ
        Fİ
    SON_DONGÜ

    EĞER (pin_kabul = HAYIR) İSE
        // PIN doğrulanamadıysa ana döngüyü sonlandır
        YAZ "İşlem sonlandırılıyor."
        DONGÜDEN_CIK  // Genel döngüden çık -> BAŞLA sonu
    Fİ

    // PIN onaylandıktan sonra işlem menüsü
    islemler_devam <- EVET

    DONGÜ (islemler_devam = EVET)
        YAZ "Seçiniz: 1) Para Çek  2) Bakiye Sorgula  3) Çıkış"
        OKU secim

        EĞER (secim = 1) İSE
            // Para çekme akışı
            YAZ "Çekmek istediğiniz tutarı giriniz (20 TL katları):"
            OKU tutar

            // Tutarın pozitif ve 20'nin katı olup olmadığı
            EĞER (tutar <= 0) İSE
                YAZ "Geçersiz tutar. Lütfen pozitif bir tutar girin."
                DONGÜDEN_ATLA  // işlem menüsüne geri dön
            Fİ

            EĞER (tutar MOD 20 <> 0) İSE
                YAZ "Tutar 20 TL'nin katı olmalıdır. Lütfen yeniden deneyin."
                DONGÜDEN_ATLA
            Fİ

            // Günlük limit kontrolü
            EĞER (gunluk_cekilen + tutar > gunluk_limit) İSE
                YAZ "Günlük limit aşılıyor. Kalan günlük limitiniz:" , (gunluk_limit - gunluk_cekilen)
                DONGÜDEN_ATLA
            Fİ

            // Bakiye kontrolü
            EĞER (tutar > hesap_bakiye) İSE
                YAZ "Yetersiz bakiye. Mevcut bakiyeniz:" , hesap_bakiye
                DONGÜDEN_ATLA
            Fİ

            // Onay isteği (isteğe bağlı, butona bas veya E/H)
            YAZ "Çekmek istediğiniz tutar:" , tutar , "TL. Onaylıyor musunuz? (E/H)"
            OKU onay
            EĞER (onay = "E" VEYA onay = "e") İSE
                // İşlemi gerçekleştir
                hesap_bakiye <- hesap_bakiye - tutar
                gunluk_cekilen <- gunluk_cekilen + tutar
                YAZ "İşlem başarılı. Lütfen paranızı alınız."
                YAZ "Kalan bakiyeniz:" , hesap_bakiye
            DEĞİLSE
                YAZ "İşlem iptal edildi."
            Fİ

            // İşlem tekrarı seçeneği
            YAZ "Başka işlem yapmak istiyor musunuz? (E/H)"
            OKU devam_tercihi
            EĞER (devam_tercihi = "E" VEYA devam_tercihi = "e") İSE
                islemler_devam <- EVET
            DEĞİLSE
                islemler_devam <- HAYIR
            Fİ

        DEĞİLSE EĞER (secim = 2) İSE
            // Bakiye göster
            YAZ "Mevcut bakiyeniz:" , hesap_bakiye
            YAZ "Bugün çektiğiniz toplam:" , gunluk_cekilen , "  Günlük limitiniz:" , gunluk_limit
            YAZ "Başka işlem yapmak istiyor musunuz? (E/H)"
            OKU devam_tercihi
            EĞER (devam_tercihi = "E" VEYA devam_tercihi = "e") İSE
                islemler_devam <- EVET
            DEĞİLSE
                islemler_devam <- HAYIR
            Fİ

        DEĞİLSE EĞER (secim = 3) İSE
            YAZ "İşlem sonlandırılıyor. Kartınızı alınız."
            islemler_devam <- HAYIR
        DEĞİLSE
            YAZ "Geçersiz seçim. Lütfen 1, 2 veya 3 giriniz."
            // Menüye geri dön
        Fİ

    SON_DONGÜ  // islemler_devam döngüsü sonu

    // Kartı çıkartma / oturumu kapatma
    YAZ "Oturum kapatıldı. Güle güle."
    // Eğer kullanıcı başka bir işlemle geri dönmek isterse ana DONGÜ devam eder; burada DONGÜden çıkmak için:
    YAZ "Yeni müşteri için 'Y' tuşlayın, yoksa herhangi bir tuşa basın."
    OKU yeni_musteri
    EĞER (yeni_musteri = "Y" VEYA yeni_musteri = "y") İSE
        DONGÜDEN_ATLA  // ana döngünün başına dön (yeni müşteri)
    DEĞİLSE
        DONGÜDEN_CIK  // ana döngüden çık -> BAŞLA sonu
    Fİ

SON_DONGÜ  // Genel işlem döngüsü sonu

YAZ "ATM kapatılıyor."

SON
