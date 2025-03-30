# Access_Control_List_Yapilandirmasi
Bilgi teknolojileri alanında ağ güvenliği, sistemlerin güvenli ve verimli çalışmasını sağlamak için kritik bir konudur. Özellikle belirli cihazların belirli hizmetlere erişimini düzenlemek, hem güvenliği artırır hem de ağ trafiğini kontrol altında tutmaya yardımcı olur. Bu amaçla, Genişletilmiş

Access Control List (Extended ACL) kullanarak ağ trafiğini filtrelemek yaygın bir uygulamadır.

Bu çalışmada, belirlenen kurallar çerçevesinde bir ACL yapılandırması gerçekleştirilecektir. Senaryomuzda:
✅ 192.168.1.0/24 ağı, 172.16.10.100 sunucusuna sadece ping (ICMP) atabilecek ve diğer tüm trafik engellenecektir.
✅ 192.168.2.0/24 ağı içindeki cihazlardan yalnızca 192.168.2.10'un FTP erişimi açık olacak, diğerleri engellenecektir.
✅ 172.16.2.20 cihazının 172.16.10.100 ile FTP bağlantısı kurması yasaklanacaktır.


Ağ güvenliğini sağlamak ve belirli cihazların belirli hizmetlere erişimini kontrol etmek için Genişletilmiş ACL (Extended Access Control List) kullanılır. Bu çalışmada, 192.168.2.10 cihazının FTP erişimini serbest bırakırken, 192.168.2.20 cihazının 172.16.10.100 FTP sunucusuna erişmesini engelleyecek bir ACL yapılandırması oluşturduk.

ACL 105 Yapılandırması
Aşağıdaki komutlar kullanılarak, belirlenen kurallar doğrultusunda bir Extended ACL (105 numaralı) oluşturulmuştur:

R1-gig 0/0  access-list 105 permit tcp host 192.168.1.10 host 172.16.10.100 eq 21 ( izin verildi)

R1-gig 0/0- access-list 105 deny tcp 192.168.1.0 0.0.0.255 host 172.16.10.100 eq ftp  (engelleme )

R1-gig 0/0- access-list 105 permit icmp any any ( any izin verme anlamı tasır ,ping atarız )

uygulama : interface fastEthernet 0/0

ip access-grup 105 in
Bu aşamada 192.168.2.0 network den sadece 192.168.2.10 ip adresi 172.16.10.100 e ftp bağlantısı gönderirken, 192.168.2.20 ftp bağlantısı yapamayacak.

#192.168.2.0 
access-list 106 deny tcp host 192.168.2.20 host 172.16.10.100 eq ftp
access-list 106 permit tcp host 192.168.2.10 host 172.16.10.100 eq ftp
access-list 106 permit icmp any any
interface GigabitEthernet0/1
ip access-group 106 in

192.168.1.0 networkü ftp yetkisi yok
ACL’nin Arayüze Uygulanması
Yukarıdaki ACL kurallarının çalışabilmesi için, ilgili router’ın GigabitEthernet 0/1 arayüzüne uygulanması gerekmektedir. Bunu aşağıdaki komutlarla gerçekleştiriyoruz:


Konfigürasyon Açıklaması
Permit TCP (192.168.2.10 — FTP erişimi açık): 192.168.2.10 cihazının 172.16.10.100 adresindeki FTP sunucusuna erişmesine izin verilir.
Deny TCP (192.168.2.20 — FTP erişimi engellendi): 192.168.2.20 cihazının FTP sunucusuna bağlanmasını engellemek için bir kural eklenir.
Permit ICMP Any Any: Ping paketlerine izin verilerek ağdaki cihazların erişim testleri yapmasına olanak sağlanır.
Permit IP Any Any: ACL’nin sonuna eklenen bu kural, kalan tüm trafiğe izin vererek gereksiz engellemelerin önüne geçer.

192.168.2.20 cihazı ftp bağlantısı açık
Sonuç
Bu yapılandırma sayesinde, 192.168.2.10 cihazı FTP sunucusuna erişebilirken, 192.168.2.20 cihazının erişimi engellenmiştir. Bununla birlikte, ICMP (ping) trafiğine izin verildiği için, ağdaki cihazlar diğer ağlarla iletişim kurmaya devam edebilir.

Bu yapılandırma, ağ erişim kontrolü konusunda esneklik sağlamanın yanı sıra, belirli cihazların belirli hizmetlere erişimini kısıtlayarak güvenliği artırmaktadır.
