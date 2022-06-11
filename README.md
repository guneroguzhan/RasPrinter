# RasPrinter
## Kablolu Bağlantılı Yazıcıların Kablosuz Bağlantı Sunucusu ile Sürdürülebilir Kullanımının Sağlanması

**Raspberry Pi Printer ve Samba Server Kurulumu**

![pi-printer](https://user-images.githubusercontent.com/107308122/173188764-3f867339-1203-423b-b5d1-a9c4a2a3e1fa.png)

Günümüzde birçok ev aleti, elektronik ofis aletleri (yazıcı vb.) gibi cihazların nesnelerin interneti alanının gelişmesiyle birlikte kablosuz hale gelmesi birçok insanın ellerinde aktif kullanılabilir ürünleri, sırf kablosuz bağlantıları olmadığından, değiştirmesine sebep olmaktadır. Kullanılabilir ve işlevini yerine getiren cihazların ömürlerini tamamlamadan değiştirilmesi sürdürülebilirlik ve sürdürülebilir olma konularına karşı olmakla beraber birey, aile, toplum ekonomisine zarar vermektedir.

Bu proje, bu tür cihazlardan biri olan kablo bağlantılı yazıcıların güncellenerek kablosuz bir sunucuya bağlanması ve böylelikle ev, ofis vb. ortamlarda kablosuz erişilebilirliğini sağlamayı hedeflemektedir.

**CUPS: Common Unix Print Server**

CUPS, Apple Inc. tarafından macOS® ve diğer UNIX® benzeri işletim sistemleri için geliştirilen standartlara dayalı, açık kaynaklı bir yazıcı sistemidir. Internet Yazdırma Protokolü'nü ("IPP") kullanır ve yazıcıları ve yazdırma işlerini yönetmek için System V ve Berkeley komut satırı arabirimleri, bir web arabirimi ve bir C API sağlar. Hem yerel (paralel, seri, USB) hem de ağa bağlı yazıcılara yazdırmayı destekler ve yazıcılar Internet üzerinden bile bir bilgisayardan diğerine paylaşılabilir!

Ek olarak, CUPS yazıcı yeteneklerini ve özelliklerini tanımlamak için PostScript Yazıcı Tanımı ("PPD") dosyalarını ve birçok dosya türünü dönüştürmek ve yazdırmak için çok çeşitli genel ve aygıta özgü programları kullanır. Birçok Dymo, EPSON, HP, Intellitech, OKIDATA ve Zebra yazıcıyı desteklemek için CUPS içerisinde örnek sürücüler de dahildir.

Bu proje dahilinde CUPS bizim yazıcı sunucumuz olarak çalışacaktır.

![linux_print1](https://user-images.githubusercontent.com/107308122/173192386-49ce7290-344e-48cb-b2f1-97d7972ede41.gif)

***CUPS Kurulum adımları;***

```
sudo apt update
sudo apt-get install cups
```
>Bu kısımda güncelleme olup olmadığını denetledikten sonra CUPS kurulumu için talepte bulunmuş olduk 
-------------------------------------------------------------------------
```
sudo usermod -a -G lpadmin pi
```
>Bu kısımda pi kullanıcısını admin olarak tanımladık
-------------------------------------------------------------------------
```
sudo cupsctl --remote-any
```
>Bu kısımda CUPS'ı remote bağlantı için erişilebilir yaptık
-------------------------------------------------------------------------
```
sudo etc/init.d/cups restart
```
>Ve son olarak CUPS'ı yeniden başlatarak bütün güncel ayarlarımızın çalışmasını sağladık
-------------------------------------------------------------------------

**SMB: SAMBA Server**

Samba, Linux ve Unix için standart Windows birlikte çalışabilirlik programıdır. Samba, GNU General Public License altında lisanslanmış Özgür Yazılımdır. 1992'den beri Samba, DOS ve Windows'un tüm sürümleri, OS/2, Linux ve diğerleri gibi SMB/CIFS protokolünü kullanan tüm istemciler için güvenli, istikrarlı ve hızlı dosya ve baskı hizmetleri sağlamıştır. Samba, Linux/Unix Sunucularını ve Masaüstlerini Active Directory ortamlarına sorunsuz bir şekilde entegre etmek için önemli bir bileşendir. Hem etki alanı denetleyicisi hem de normal etki alanı üyesi olarak işlev görebilir. 

Biz de bu projede Samba Server'ı CUPS ile kurduğumuz print server'ın Windows işletim sistemine sahip bilgisayarlar tarafından da görülmesini sağlamak için kullanacağız.

```
sudo apt-get install samba
```
>Bu kısımda Samba Server kurulumu için talepte bulunmuş olduk 
-------------------------------------------------------------------------
```
sudo nano etc/samba/smb.conf
```
>Bu kısımda smb.conf dosyası açılır ve içerisinde yer alan **[printers]** bölümündeki **guest ok = no -> yes** ile **read only = yes -> no** ile değiştirilerek kaydedilir 
-------------------------------------------------------------------------
```
sudo systemctl restar smbd
```
>Ve son olarak Samba Server'ı yeniden başlatarak bütün güncel ayarlarımızın çalışmasını sağladık
-------------------------------------------------------------------------

![samba1](https://user-images.githubusercontent.com/107308122/173192394-0ff1ff27-a6e4-44ad-b3fc-bc009844b541.png)

**Gerekli Diğer Kurulumlar**
- Raspberry PI OS kurulumu: Bu kısımda  **Debian Buster** tercih edildi.
- Ek olarak; 
Bu tarz bir proje için Raspberry Pi cihazımızın Boot süresini hızlandırmamız verimlilik açısından faydalı olacaktır. Bütün kurulumları yaptıktan sonra ihtiyaç duyulmayan sistem servisleri kapatılarak bu süreden kazanç sağlanacaktır.


```
systemd-analyze
```
>Bu komut Raspberry cihazımızın Startup süresinin, **kernel + userspace** olacak şekilde, ne kadar olduğunu gösterecektir.
>*Benim sistemim için bu komut **"Startup finished in 5.185s (kernel) + 23.775s (userspace) = 28.961s graphical target reached after 23.702s in userspace"** olarak cevap dönmüştür* 
-------------------------------------------------------------------------


```
systemd-analyze blame
```
>Bu komut Raspberry cihazımızın Startup sürecinde servislerin her birinin ne kadar süre kullanımı olduğunu gösterecektir.
>Bu liste içerisinden projemiz için ihtiyaç duymadığımız servisleri seçerek kapatabiliriz.
-------------------------------------------------------------------------
```
sudo nano /boot/config.txt
```
>Bu kısımda config.txt dosyası açılır ve içerisinde yer alan **uncomment to overclock the arm. 700Mhz is the default** yazan bölümünde **over_voltage = 2, arm_freq=1000, boot_delay=0** olacak şekilde değiştirilerek kaydedilir.
-------------------------------------------------------------------------
```
sudo nano /etc/dhcpcd.conf
```
>Bu kısımda dhcpcd.conf dosyası açılır ve içerisinde en alttaki boş kısma aşağıdakiler yazılarak kaydedilir.
```
noarp
ipv4only
noipv6
```
>*Bütün bu işlemler sonucunda **"Startup finished in 4.343s (kernel) + 19.888s (userspace) = 24.232s graphical target reached after 19.786s in userspace"** olarak cevap dönmüştür*
>Özet olarak basit bir iki işlemle bile total boot süresini **4.729s** azaltmış olduk.
-------------------------------------------------------------------------
