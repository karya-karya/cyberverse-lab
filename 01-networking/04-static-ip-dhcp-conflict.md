# Lab 04 — Static IP, DHCP ve IP Çakışması

## 1. Amaç

Bu laboratuvarın amacı, statik IP adresi ile DHCP üzerinden otomatik IP adresi almayı karşılaştırmak ve aynı IP adresinin iki cihaza verilmesi durumunda oluşan çakışmayı gözlemlemektir.

## 2. Kullanılan Araçlar

- Cisco Packet Tracer
- 1 Router
- 1 Switch
- 2 PC
- Copper Straight-Through kabloları

## 3. Ağ Topolojisi

```text
PC-A (DHCP)
192.168.1.21
     |
     | FastEthernet0
     |
Switch-1
  Fa0/1 — PC-A
  Fa0/2 — Router
  Fa0/3 — PC-B
     |
     | GigabitEthernet0/0
     |
Router-1
192.168.1.1

PC-B (Static IP)
192.168.1.30
```

## 4. IP Yapılandırması

| Cihaz | IP Adresi | Atama Yöntemi |
|---|---|---|
| PC-A | 192.168.1.21 | DHCP |
| PC-B | 192.168.1.30 | Statik |
| Router G0/0 | 192.168.1.1 | Statik |

Alt ağ maskesi: `255.255.255.0`

Varsayılan ağ geçidi: `192.168.1.1`

## 5. Yapılan İşlemler ve Testler

### Test 1 — Farklı IP adresleriyle bağlantı

PC-A üzerinden aşağıdaki komut çalıştırıldı:

```text
ping 192.168.1.30
```

Sonuç:

- Gönderilen paket: 4
- Alınan yanıt: 4
- Paket kaybı: %0
- Ortalama yanıt süresi: 0 ms

**Değerlendirme:** PC-A ve PC-B farklı IP adresleriyle aynı alt ağda başarılı şekilde iletişim kurdu.

### Test 2 — IP adresi çakışması

PC-B'nin IP adresi `192.168.1.30` yerine `192.168.1.21` olarak değiştirilmeye çalışıldı.

Packet Tracer şu uyarıyı verdi:

`This address already used in network`

**Değerlendirme:** Simülatör, girilen IP adresinin ağda zaten kullanıldığını belirtti. Bu, aynı IP adresinin iki cihaza verilmesinin oluşturabileceği çakışma riskini gösterdi.

Uyarı sonrasında PC-B'nin IP adresi tekrar `192.168.1.30` olarak ayarlandı.

## 6. Öğrenilen Kavramlar

- **Static IP:** IP adresinin cihaz üzerinde elle yapılandırılmasıdır.
- **DHCP:** Cihazlara IP yapılandırmasını otomatik olarak sağlayan protokoldür.
- **IP Conflict:** Aynı IP adresinin aynı ağda birden fazla cihaza atanması durumudur.
- **Ping:** İki cihaz arasındaki IP bağlantısını test etmek için kullanılan araçtır.

## 7. Güvenlik ve Ağ Yönetimi Açısından Değerlendirme

Statik IP adresleri elle yönetildiğinde, DHCP havuzuyla çakışma riski oluşabilir. Bu riski azaltmak için statik adresler DHCP havuzunun dışında tutulabilir veya uygun DHCP rezervasyonları kullanılabilir.

## 8. Sonuç

Bu laboratuvarda DHCP ile otomatik IP ataması ve statik IP yapılandırması uygulandı. İki bilgisayar arasında başarılı bağlantı testi gerçekleştirildi ve IP çakışması girişiminde Packet Tracer uyarısı gözlemlendi.

**Not:** Gerçek bir IP çakışmasının iletişim üzerindeki etkileri ayrıca test edilmedi; gözlemlenen sonuç, Packet Tracer'ın adresin kullanımda olduğuna dair uyarısıdır.
