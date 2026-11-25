# Cisco Packet Tracer: FTP Aktarımı ve Dosya Bütünlüğü Kontrolü Labı

Bu projede `192.168.10.0/24` ağı üzerinde FTP dosya aktarımı, HTTP erişimi ve dosya boyutu/içerik değişimine dayalı bütünlük uyumsuzluğu senaryosu uygulanmıştır.

## Ağ Cihazları ve IP Adresleri
- **Backup-Server (`192.168.10.10`):** FTP sunucusu (`put` / `get` dosya işlemleri).
- **Hash-Log-Server (`192.168.10.11`):** HTTP servisi ve bütünlük kayıtlarının (`hash_log.txt`) tutulduğu sunucu.
- **Client-PC (`192.168.10.20`):** Rapor dosyalarının oluşturulduğu, güncellendiği ve FTP ile gönderildiği istemci.
- **Analyst-PC (`192.168.10.21`):** Web tarayıcısı üzerinden HTTP erişiminin kontrol edildiği istemci.

## Projede Yapılan İşlemler
1. **İlk Dosya Aktarımı ve Doğrulama:** `Client-PC` üzerinde `rapor1.txt` (`14` byte) oluşturuldu, `ftp 192.168.10.10` (`cisco` / `cisco`) ile sunucuya `put rapor1.txt` komutuyla gönderildi ve `get rapor1.txt` ile indirilerek `14` byte boyutu doğrulandı.
2. **HTTP Erişimi:** `Analyst-PC` tarayıcısından `http://192.168.10.11` adresine bağlanarak `Hash-Log-Server` web sayfasının yanıt verdiği test edildi.
3. **Bütünlük Uyumsuzluğu Senaryosu:** `Client-PC` üzerindeki `rapor1.txt` dosyasının içeriği değiştirilerek boyutu `14` byte'tan `27` byte'a çıkarıldı, `put rapor1.txt` ile sunucuya yeniden gönderildi ve `Hash-Log-Server` üzerindeki `hash_log.txt` dosyasına uyumsuzluk kaydı eklendi.
4. **Diğer Rapor Dosyaları (`rapor2.txt` ve `rapor3.txt`):** Yeni metin dosyaları oluşturuldu, FTP ile `Backup-Server` cihazına aktarıldı ve kayıtları `hash_log.txt` dosyasına işlendi.

## Karşılaşılan Hatalar ve Çözümleri
- **FTP Komut Hatası:** `get.rapor1.txt` şeklinde bitişik yazılan komut hata verdi; `get rapor1.txt` olarak boşluklu yazılınca dosya başarıyla indirildi.
- **Tarayıcı Bağlantı ve Adres Hataları:** URL kutusuna köşeli/yuvarlak parantezli bağlantı yapıştırılması veya `http://` ile çakışan ifadeler girilmesi `Protocol Not Supported`, `Host Name Unresolved` ve `Server Reset Connection` hatalarına yol açtı. Adres kutusu temizlenip doğrudan `192.168.10.11` girildiğinde sayfa sorunsuz açıldı.