Hastane Randevu ve Tahlil Sistemi için LLM ile beraber hazırlanılan pseudocode aşağıdadır:


ALGORITMA HastaneSistemi

BASLA
          TEKRARLA
          YAZ "=== HASTANE SİSTEMİ ANA MENÜ ==="
          YAZ "1. Randevu Sistemi"
          YAZ "2. Tahlil Sonucu Görüntüleme"
          YAZ "3. Çıkış"
          SECIM = OKU

          // =========================
          // 1. Randevu Sistemi
          // =========================
          EĞER SECIM = 1
            // Kimlik doğrulama
            YAZ "Lütfen TC Kimlik Numaranızı giriniz:"
            KIMLIK_NO = OKU
            EĞER KimlikDogruMu(KIMLIK_NO) = HAYIR
                YAZ "Kimlik doğrulama başarısız! Ana menüye dönülüyor."
                DEVAM
            SONRA
                YAZ "Kimlik doğrulama başarılı."

            // Poliklinik seçimi
            POLIKLINIKLER = ["Dahiliye", "Kardiyoloji", "Ortopedi", "Göz", "Çocuk"]
            YAZ "Poliklinik seçenekleri:"
            FOR i = 1 TO POLIKLINIKLER.UZUNLUK
                YAZ i, ". ", POLIKLINIKLER[i]
            POLIKLINIK_SECIM = OKU
            SEÇILEN_POLIKLINIK = POLIKLINIKLER[POLIKLINIK_SECIM]
            YAZ "Seçilen poliklinik: ", SEÇILEN_POLIKLINIK

            // Doktor listesi gösterme
            DOKTORLAR = DoktorListele(SEÇILEN_POLIKLINIK)
            YAZ "Bu poliklinikteki doktorlar:"
            FOR i = 1 TO DOKTORLAR.UZUNLUK
                YAZ i, ". Dr. ", DOKTORLAR[i]
            DOKTOR_SECIM = OKU
            SEÇILEN_DOKTOR = DOKTORLAR[DOKTOR_SECIM]
            YAZ "Seçilen doktor: Dr. ", SEÇILEN_DOKTOR

            // Uygun saatleri gösterme
            SAATLER = UygunSaatleriGoster(SEÇILEN_DOKTOR)
            YAZ "Uygun randevu saatleri:"
            FOR i = 1 TO SAATLER.UZUNLUK
                YAZ i, ". ", SAATLER[i]
            SAAT_SECIM = OKU
            SEÇILEN_SAAT = SAATLER[SAAT_SECIM]
            YAZ "Seçilen saat: ", SEÇILEN_SAAT

            // Randevu onaylama ve SMS gönderme
            YAZ "Randevunuzu onaylamak istiyor musunuz? (EVET/HAYIR)"
            ONAY = OKU
            EĞER ONAY = "EVET"
                RandevuKaydet(KIMLIK_NO, SEÇILEN_POLIKLINIK, SEÇILEN_DOKTOR, SEÇILEN_SAAT)
                YAZ "Randevunuz başarıyla kaydedildi."
                GONDERSMS(KIMLIK_NO, "Randevunuz Dr. " + SEÇILEN_DOKTOR +
                          " ile " + SEÇILEN_POLIKLINIK + " polikliniğinde " + SEÇILEN_SAAT + " saatinde onaylandı.")
            DEĞILSE
                YAZ "Randevu iptal edildi."

        // =========================
        // 2. Tahlil Sonucu Görüntüleme Sistemi
        // =========================
        DEĞILSE EĞER SECIM = 2
            // Kimlik doğrulama
            YAZ "Lütfen TC Kimlik Numaranızı giriniz:"
            KIMLIK_NO = OKU
            EĞER KimlikDogruMu(KIMLIK_NO) = HAYIR
                YAZ "Kimlik doğrulama başarısız! Ana menüye dönülüyor."
                DEVAM
            SONRA
                YAZ "Kimlik doğrulama başarılı."

            // Tahlil varlığı kontrolü
            EĞER TahlilVarMi(KIMLIK_NO) = HAYIR
                YAZ "Herhangi bir tahlil sonucu bulunmamaktadır."
                DEVAM
            SONRA
                YAZ "Tahlil sonuçlarınız bulundu."

            // Sonucun hazır olup olmadığı kontrolü
            EĞER SonucHazirMi(KIMLIK_NO) = HAYIR
                YAZ "Tahlil sonucunuz henüz hazırlanmadı. Lütfen daha sonra tekrar kontrol ediniz."
                DEVAM
            SONRA
                YAZ "Tahlil sonucunuz hazır."

            // Sonucu görüntüleme
            YAZ "Tahlil sonucunu görüntülemek ister misiniz? (EVET/HAYIR)"
            SECIM2 = OKU
            EĞER SECIM2 = "EVET"
                GosterSonuc(KIMLIK_NO)
                YAZ "Tahlil sonucunuz ekranda görüntülendi."

                // PDF indirme
                YAZ "Sonucunuzu PDF olarak indirmek ister misiniz? (EVET/HAYIR)"
                PDF_SECIM = OKU
                EĞER PDF_SECIM = "EVET"
                    PDFIndir(KIMLIK_NO)
                    YAZ "Tahlil sonucunuz PDF olarak indirildi."
                DEĞILSE
                    YAZ "PDF indirme işlemi iptal edildi."
            DEĞILSE
                YAZ "Tahlil görüntüleme işlemi iptal edildi."

        // =========================
        // 3. Çıkış
        // =========================
        DEĞILSE EĞER SECIM = 3
            YAZ "Sistemden çıkılıyor. İyi günler!"
            DUR

        // Geçersiz seçim
        DEĞILSE
            YAZ "Geçersiz seçim! Lütfen tekrar deneyiniz."
    TEKRARLA
SON
