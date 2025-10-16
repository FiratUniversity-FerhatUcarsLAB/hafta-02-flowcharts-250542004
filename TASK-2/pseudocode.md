Online Alışveriş Sepet Sistemi-Devamı için LLM tarafından hazırlanan pseudocode aşağıdadır:


BASLA

# 1. Kullanıcı Girişi
KULLANICI_GIRISI:
    KULLANICI_ADI ← input("Kullanıcı adını girin: ")
    SIFRE ← input("Şifreyi girin: ")
    EGER veritabanında KULLANICI_ADI ve SIFRE eşleşiyorsa ISE
        göster "Giriş başarılı"
    DEĞILSE ISE
        göster "Kullanıcı adı veya şifre hatalı"
        TEKRAR KULLANICI_GIRISI

# 2. Ürün Listeleme ve Sepete Ekleme
SEPET_YONETIMI:
    DONGU
        URUN_LISTESI ← veritabanından tüm ürünleri al
        göster URUN_LISTESI
        SECIM ← input("Sepete eklemek istediğiniz ürün ID'si veya çıkmak için 0: ")
        EGER SECIM = 0 ISE
            çıkış DONGU
        MIKTAR ← input("Miktarı girin: ")
        URUN ← veritabanından SECIM ile ürün al
        EGER URUN.stok ≥ MIKTAR ISE
            sepet.ekle(URUN, MIKTAR)
            göster "Ürün sepete eklendi"
        DEĞILSE ISE
            göster "Stok yetersiz"
    DONGU BITIR

# 3. İndirim Kodu Uygulama
INDIRIM_KODU:
    KOD ← input("İndirim kodu girin veya boş bırakın: ")
    EGER KOD ≠ "" ISE
        EGER kod veritabanında geçerliyse ISE
            sepet.toplam_fiyat ← sepet.toplam_fiyat * (1 - kod.indirim_orani)
            göster "İndirim uygulandı"
        DEĞILSE ISE
            göster "Geçersiz indirim kodu"

# 4. Kargo Hesaplama
KARGO_HESAP:
    KARGO_SECIM ← input("Kargo seçiminizi yapın: standart/express")
    TOPLAM_AGIRLIK ← 0
    DONGU URUN in sepet.urunler
        TOPLAM_AGIRLIK ← TOPLAM_AGIRLIK + URUN.agirlik * URUN.miktar
    DONGU BITIR
    KARGO_UCRETI ← kargo_fiyat_hesapla(TOPLAM_AGIRLIK, KARGO_SECIM)
    sepet.toplam_fiyat ← sepet.toplam_fiyat + KARGO_UCRETI
    göster "Kargo ücreti eklendi: ", KARGO_UCRETI

# 5. Ödeme İşlemleri
ODEME:
    ODENECEK_TUTAR ← sepet.toplam_fiyat
    KART_NO ← input("Kart numarasını girin: ")
    SON_KULLANMA_TARIHI ← input("Son kullanım tarihi: ")
    CVV ← input("CVV: ")
    EGER ödeme bilgileri geçerliyse ISE
        DONGU URUN in sepet.urunler
            URUN.stok ← URUN.stok - URUN.miktar
            veritabanını güncelle(URUN)
        DONGU BITIR
        göster "Ödeme başarılı, siparişiniz alındı"
    DEĞILSE ISE
        göster "Ödeme başarısız, bilgileri kontrol edin"

BITIR
