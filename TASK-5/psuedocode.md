Akıllı Ev Güvenlik Sistemi ile ilgili LLM yardımı ile yazılan psuedocode aşağıdadır:



# Görev 5: Akıllı Ev Güvenlik Sistemi – Pseudocode

```pseudocode
BASLA
    // 1. Sistem başlatılır ve aktif/pasif durumu kontrol edilir
    SISTEM_AKTIF = TRUE
    YAZ "Akıllı Ev Güvenlik Sistemi Başlatıldı."

    // 2. Sonsuz ana döngü – sistem 7/24 çalışır
    TEKRARLA (SÜREKLİ)
        // 2a. Sensör okuma döngüsü
        YAZ "Sensörler kontrol ediliyor..."
        HAREKET_DETECTED = HAREKET_SENSÖRÜ_OKU()
        KAPI_PENCERE_DETECTED = KAPI_PENCERE_SENSÖRÜ_OKU()

        // 2b. Kamera aktivasyonu
        EĞER HAREKET_DETECTED = TRUE VEYA KAPI_PENCERE_DETECTED = TRUE
            KAMERA_AKTİF_ET()
            YAZ "Kamera aktifleştirildi."

        // 2c. Yanlış alarm kontrolü (Ev sahibi evde mi?)
        EVDE_Mİ = EV_SENSÖRÜ_OKU()
        EĞER EVDE_Mİ = TRUE
            YAZ "Ev sahibi evde, alarm tetiklenmedi."
            ALARM_DUR = TRUE  // Alarm durdurulur
        DEĞİLSE
            ALARM_DUR = FALSE  // Alarm aktif olabilir

        // 2d. Alarm seviyesi belirleme
        EĞER HAREKET_DETECTED = TRUE VE KAPI_PENCERE_DETECTED = TRUE
            ALARM_SEVİYESİ = 3  // Yüksek
        DEĞİLSE EĞER HAREKET_DETECTED = TRUE VEYA KAPI_PENCERE_DETECTED = TRUE
            ALARM_SEVİYESİ = 2  // Orta
        DEĞİLSE
            ALARM_SEVİYESİ = 1  // Düşük / Tehdit yok

        // 2e. Alarm ve bildirim gönderme
        EĞER ALARM_DUR = FALSE VE ALARM_SEVİYESİ > 1
            BILDIRIM_GONDER(SMS)
            BILDIRIM_GONDER(EMAIL)
            BILDIRIM_GONDER(APP)
            YAZ "Bildirim gönderildi. Alarm seviyesi: ", ALARM_SEVİYESİ

        // 2f. Alarm sıfırlama veya devam ettirme
        ALARM_KOMUT = ALARM_DURUMU_KONTROL_ET() // Kullanıcıdan sıfırlama komutu gelirse
        EĞER ALARM_KOMUT = "SIFIRLA"
            ALARM_SEVİYESİ = 1
            YAZ "Alarm sıfırlandı."

        // 2g. Döngü bekleme – sensör güncellemeleri için kısa süre
        BEKLE(5 saniye)
BİTİR
