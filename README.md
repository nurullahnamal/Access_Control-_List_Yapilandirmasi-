# Access_Control_List_Yapilandirmasi
Bilgi teknolojileri alanında ağ güvenliği, sistemlerin güvenli ve verimli çalışmasını sağlamak için kritik bir konudur. Özellikle belirli cihazların belirli hizmetlere erişimini düzenlemek, hem güvenliği artırır hem de ağ trafiğini kontrol altında tutmaya yardımcı olur. Bu amaçla, Genişletilmiş

Access Control List (Extended ACL) kullanarak ağ trafiğini filtrelemek yaygın bir uygulamadır.<br>
 ![image alt](https://github.com/nurullahnamal/Access_Control_List_Yapilandirmasi/blob/main/Geni%C5%9Fletilmi%C5%9F%20ACL%20yap%C4%B1land%C4%B1rmas%C4%B1.png)
Bu çalışmada, belirlenen kurallar çerçevesinde bir ACL yapılandırması gerçekleştirilecektir. Senaryomuzda: <br>
✅ 192.168.1.0/24 ağı, 172.16.10.100 sunucusuna sadece ping (ICMP) atabilecek ve diğer tüm trafik engellenecektir. <br>
✅ 192.168.2.0/24 ağı içindeki cihazlardan yalnızca 192.168.2.10'un FTP erişimi açık olacak, diğerleri engellenecektir. <br>
✅ 172.16.2.20 cihazının 172.16.10.100 ile FTP bağlantısı kurması yasaklanacaktır. <br>
![image alt](https://github.com/nurullahnamal/Access_Control_List_Yapilandirmasi/blob/main/192.168.1.10%20cihaz%C4%B1%20ping%20172.16.10.100%20.gif)

Ağ güvenliğini sağlamak ve belirli cihazların belirli hizmetlere erişimini kontrol etmek için Genişletilmiş ACL (Extended Access Control List) kullanılır.<br>
Bu çalışmada, 192.168.2.10 cihazının FTP erişimini serbest bırakırken, 192.168.2.20 cihazının 172.16.10.100 FTP sunucusuna erişmesini engelleyecek bir ACL yapılandırması oluşturduk.<br>
![image alt](https://github.com/nurullahnamal/Access_Control_List_Yapilandirmasi/blob/main/192.168.2.10%20to%20ftp%20172.16.10.100.gif) <br>
ACL 105 Yapılandırması<br>
Aşağıdaki komutlar kullanılarak, belirlenen kurallar doğrultusunda bir Extended ACL (105 numaralı) oluşturulmuştur: <br>

R1-gig 0/0  access-list 105 permit tcp host 192.168.1.10 host 172.16.10.100 eq 21 ( izin verildi) <br>

R1-gig 0/0- access-list 105 deny tcp 192.168.1.0 0.0.0.255 host 172.16.10.100 eq ftp  (engelleme ) <br>

R1-gig 0/0- access-list 105 permit icmp any any ( any izin verme anlamı tasır ,ping atarız ) <br>

uygulama : interface fastEthernet 0/0 <br>

ip access-grup 105 in <br>
Bu aşamada 192.168.2.0 network den sadece 192.168.2.10 ip adresi 172.16.10.100 e ftp bağlantısı gönderirken, 192.168.2.20 ftp bağlantısı yapamayacak. <br>

![image alt](https://github.com/nurullahnamal/Access_Control_List_Yapilandirmasi/blob/main/192.168.2.20%20to%20ftp%20172.16.10.100.gif)
 
#192.168.2.0  <br>
access-list 106 deny tcp host 192.168.2.20 host 172.16.10.100 eq ftp <br>
access-list 106 permit tcp host 192.168.2.10 host 172.16.10.100 eq ftp <br>
access-list 106 permit icmp any any <br>
interface GigabitEthernet0/1  <br>
ip access-group 106 in <br>

192.168.1.0 networkü ftp yetkisi yok <br>
ACL’nin Arayüze Uygulanması <br>
Yukarıdaki ACL kurallarının çalışabilmesi için, ilgili router’ın GigabitEthernet 0/1 arayüzüne uygulanması gerekmektedir. Bunu aşağıdaki komutlarla gerçekleştiriyoruz: <br>

![image alt]()
Konfigürasyon Açıklaması <br>
Permit TCP (192.168.2.10 — FTP erişimi açık): 192.168.2.10 cihazının 172.16.10.100 adresindeki FTP sunucusuna erişmesine izin verilir. <br>
Deny TCP (192.168.2.20 — FTP erişimi engellendi): 192.168.2.20 cihazının FTP sunucusuna bağlanmasını engellemek için bir kural eklenir. <br>
Permit ICMP Any Any: Ping paketlerine izin verilerek ağdaki cihazların erişim testleri yapmasına olanak sağlanır. <br>
Permit IP Any Any: ACL’nin sonuna eklenen bu kural, kalan tüm trafiğe izin vererek gereksiz engellemelerin önüne geçer. <br>

192.168.2.20 cihazı ftp bağlantısı açık <br>
Sonuç <br>
Bu yapılandırma sayesinde, 192.168.2.10 cihazı FTP sunucusuna erişebilirken, 192.168.2.20 cihazının erişimi engellenmiştir.  <br>
Bununla birlikte, ICMP (ping) trafiğine izin verildiği için, ağdaki cihazlar diğer ağlarla iletişim kurmaya devam edebilir. <br>

Bu yapılandırma, ağ erişim kontrolü konusunda esneklik sağlamanın yanı sıra, belirli cihazların belirli hizmetlere erişimini kısıtlayarak güvenliği artırmaktadır. <br>
