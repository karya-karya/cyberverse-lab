# Lab 02 — Router ile İki Farklı Ağı Birbirine Bağlama

## 1. Amaç

İki farklı IPv4 ağını bir router aracılığıyla birbirine bağlamak, temel router yapılandırmasını öğrenmek ve ağlar arası iletişimi test etmek.

## 2. Kullanılan Araçlar

- Cisco Packet Tracer
- Cisco 2911 Router
- 2 adet Cisco 2960 Switch
- 2 adet PC
- Copper Straight-Through kablolar

## 3. Ağ Topolojisi

`PC-A — Switch-1 — Router-1 — Switch-2 — PC-B`

Router, iki farklı IP ağı arasında paket yönlendirmek için kullanılmıştır.

## 4. IP Adresleme Planı

| Cihaz | Arayüz | IP Adresi | Alt Ağ Maskesi | Varsayılan Ağ Geçidi |
|---|---|---|---|---|
| PC-A | FastEthernet0 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| Router-1 | GigabitEthernet0/0 | 192.168.1.1 | 255.255.255.0 | — |
| Router-1 | GigabitEthernet0/1 | 192.168.2.1 | 255.255.255.0 | — |
| PC-B | FastEthernet0 | 192.168.2.10 | 255.255.255.0 | 192.168.2.1 |

## 5. Router Yapılandırması

Kullanılan temel Cisco IOS komutları:

```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
end
show ip interface brief
```

Her iki arayüz de `up/up` durumunda gözlemlenmiştir.

## 6. Yönlendirme Tablosu

`show ip route` komutuyla router'ın yönlendirme tablosu incelenmiştir.

- `C`: Doğrudan bağlı ağ (Connected)
- `L`: Router'ın kendi arayüz IP adresi (Local)
- `192.168.1.0/24`: GigabitEthernet0/0 üzerinden erişilen ağ
- `192.168.2.0/24`: GigabitEthernet0/1 üzerinden erişilen ağ

Varsayılan rota tanımlanmamıştır. Bu laboratuvarda iki yerel ağ arasındaki iletişim için varsayılan rota gerekli değildir.

## 7. Bağlantı Testleri

PC-A üzerinden PC-B'ye:

```text
ping 192.168.2.10
```

Sonraki testte 4 paket gönderilmiş, 4 yanıt alınmış ve paket kaybı gözlemlenmemiştir (`0% loss`).

PC-B üzerinden PC-A'ya:

```text
ping 192.168.1.10
```

4 paket gönderilmiş, 4 yanıt alınmış ve paket kaybı gözlemlenmemiştir (`0% loss`).

## 8. Sonuç

Bu laboratuvarda iki farklı IPv4 alt ağı oluşturulmuş ve Cisco 2911 router'ın iki arayüzü yapılandırılmıştır. Yönlendirme tablosu incelenmiş, iki ağ arasında çift yönlü iletişim ping testleriyle doğrulanmıştır.

## 9. Öğrenilen Kavramlar

- IPv4 adresleme ve `/24` alt ağ maskesi
- Varsayılan ağ geçidi (Default Gateway)
- Router arayüz yapılandırması
- `no shutdown` komutunun işlevi
- Yönlendirme tablosu (Routing Table)
- Doğrudan bağlı ağlar (Connected Routes)
- ICMP Echo Request ve Echo Reply üzerinden ping testi

**Not:** Başarılı ping testleri bu simülasyonda IP bağlantısını doğrular; uygulama servislerinin çalıştığını veya ağın güvenli olduğunu tek başına kanıtlamaz.
