# Lab 03 — DHCP ile Otomatik IP Adresi Alma

## 1. Amaç

Bu laboratuvarın amacı, bir router üzerinde DHCP (Dynamic Host Configuration Protocol) yapılandırmak ve bir bilgisayarın IP adresi, alt ağ maskesi, varsayılan ağ geçidi ve DNS adresini otomatik olarak almasını sağlamaktır.

## 2. Ağ Topolojisi

```text
PC-A
  |
  | Copper Straight-Through
  |
Switch-1
  |
  | Copper Straight-Through
  |
Router-1
```

### Fiziksel bağlantılar

- PC-A FastEthernet0 → Switch-1 FastEthernet0/1
- Switch-1 FastEthernet0/2 → Router-1 GigabitEthernet0/0

## 3. IP Yapılandırması

| Cihaz | Arayüz | IP Adresi | Alt Ağ Maskesi |
|---|---|---|---|
| PC-A | FastEthernet0 | 192.168.1.21 | 255.255.255.0 |
| Router-1 | GigabitEthernet0/0 | 192.168.1.1 | 255.255.255.0 |

PC-A için otomatik olarak atanan diğer bilgiler:

- Varsayılan ağ geçidi: `192.168.1.1`
- DNS sunucusu: `8.8.8.8`

## 4. DHCP Yapılandırması

Router üzerinde kullanılan komutlar:

```text
enable
configure terminal
ip dhcp excluded-address 192.168.1.1 192.168.1.20
ip dhcp pool LAN-POOL
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
end
```

### Yapılandırmanın açıklaması

- `ip dhcp excluded-address`: Belirtilen IP adreslerinin DHCP tarafından dağıtılmasını engeller.
- `ip dhcp pool LAN-POOL`: LAN-POOL isimli DHCP havuzunu oluşturur.
- `network`: DHCP havuzunun hizmet vereceği ağı belirler.
- `default-router`: İstemcilere varsayılan ağ geçidini bildirir.
- `dns-server`: İstemcilere DNS sunucusunu bildirir.

`192.168.1.1` ile `192.168.1.20` arasındaki adresler hariç tutulduğu için ilk uygun adres olarak `192.168.1.21` atanmıştır.

## 5. Testler ve Sonuçlar

### DHCP adres kontrolü

PC-A üzerinde Desktop → IP Configuration bölümünden DHCP seçildi.

Atanan bilgiler:

- IP Address: `192.168.1.21`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.1.1`
- DNS Server: `8.8.8.8`

### Router DHCP binding kontrolü

Komut:

```text
show ip dhcp binding
```

Çıktıda `192.168.1.21` adresinin PC-A'nın `0006.2AD5.95C8` MAC adresiyle eşleştirildiği görüldü.

### Bağlantı testi

PC-A üzerinden çalıştırılan komut:

```text
ping 192.168.1.1
```

Sonuç:

- Gönderilen paket: 4
- Alınan yanıt: 4
- Paket kaybı: %0

Bu sonuç, PC-A ile router arasındaki bağlantının başarılı olduğunu göstermektedir.

## 6. Sonuç

Bu laboratuvarda router DHCP sunucusu olarak yapılandırılmış ve PC-A'nın ağ bilgilerini otomatik olarak alması sağlanmıştır. DHCP binding ve ping testleri, IP atamasını ve router ile bağlantıyı doğrulamıştır.

Not: DNS adresinin atanması doğrulanmıştır; bu laboratuvarda internet erişimi veya DNS çözümlemesi ayrıca test edilmemiştir.
