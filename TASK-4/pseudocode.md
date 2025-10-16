Universite Ders Kayıt Sistemi için LLM ile beraber hazırlanılan psuedocode aşağıdadır:


Başla
    Öğrenci girişi (öğrenci no + şifre)
    Eğer giriş başarılı ise
        Ders listesi = Tüm dersleri getir
        ToplamKredi = 0
        SeçilenDersler = []

        Her ders için dersListesi:
            Ders seçmek ister mi? (E/H)
            Eğer E ise
                Eğer kontenjan dolu ise
                    Uyarı: "Kontenjan dolu"
                Yoksa
                    Eğer ön koşullar alınmamış ise
                        Uyarı: "Ön koşul dersleri tamamlanmamış"
                    Yoksa
                        Eğer zaman çakışması varsa
                            Uyarı: "Zaman çakışması mevcut"
                        Yoksa
                            Eğer toplamKredi + ders.kredi > 35 ise
                                Uyarı: "Kredi limiti aşılıyor"
                            Yoksa
                                Eğer GPA < 2.5 ise
                                    Danışman onayı gerekli mi? (E/H)
                                    Eğer H ise
                                        Uyarı: "Danışman onayı gerekli"
                                    Yoksa
                                        SeçilenDersler += ders
                                        ToplamKredi += ders.kredi
                                Yoksa
                                    SeçilenDersler += ders
                                    ToplamKredi += ders.kredi
        Ders kayıt özetini göster
        Onaylıyor musunuz? (E/H)
        Eğer E ise
            Kayıt onaylandı
        Yoksa
            Kayıt iptal edildi
    Yoksa
        Uyarı: "Giriş başarısız"
Bitir
