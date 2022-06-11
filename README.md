# RasPrinter
Raspberry Pi Printer ve Samba Server Kurulumu

**Kablolu Bağlantılı Yazıcıların Kablosuz Bağlantı Sunucusu ile Sürdürülebilir Kullanımının Sağlanması**

![pi-printer](https://user-images.githubusercontent.com/107308122/173188764-3f867339-1203-423b-b5d1-a9c4a2a3e1fa.png)

Günümüzde birçok ev aleti, elektronik ofis aletleri (yazıcı vb.) gibi cihazların nesnelerin interneti alanının gelişmesiyle birlikte kablosuz hale gelmesi birçok insanın ellerinde aktif kullanılabilir ürünleri, sırf kablosuz bağlantıları olmadığından, değiştirmesine sebep olmaktadır. Kullanılabilir ve işlevini yerine getiren cihazların ömürlerini tamamlamadan değiştirilmesi sürdürülebilirlik ve sürdürülebilir olma konularına karşı olmakla beraber birey, aile, toplum ekonomisine zarar vermektedir.

Bu proje, bu tür cihazlardan biri olan kablo bağlantılı yazıcıların güncellenerek kablosuz bir sunucuya bağlanması ve böylelikle ev, ofis vb. ortamlarda kablosuz erişilebilirliğini sağlamayı hedeflemektedir.

**CUPS: Common Unix Print Server**

CUPS, Apple Inc. tarafından macOS® ve diğer UNIX® benzeri işletim sistemleri için geliştirilen standartlara dayalı, açık kaynaklı bir yazıcı sistemidir. Internet Yazdırma Protokolü'nü ("IPP") kullanır ve yazıcıları ve yazdırma işlerini yönetmek için System V ve Berkeley komut satırı arabirimleri, bir web arabirimi ve bir C API sağlar. Hem yerel (paralel, seri, USB) hem de ağa bağlı yazıcılara yazdırmayı destekler ve yazıcılar Internet üzerinden bile bir bilgisayardan diğerine paylaşılabilir!

Ek olarak, CUPS yazıcı yeteneklerini ve özelliklerini tanımlamak için PostScript Yazıcı Tanımı ("PPD") dosyalarını ve birçok dosya türünü dönüştürmek ve yazdırmak için çok çeşitli genel ve aygıta özgü programları kullanır. Birçok Dymo, EPSON, HP, Intellitech, OKIDATA ve Zebra yazıcıyı desteklemek için CUPS içerisinde örnek sürücüler de dahildir.

Bu proje dahilinde CUPS bizim yazıcı sunucumuz olarak çalışacaktır.

![linux_print1](https://user-images.githubusercontent.com/107308122/173192386-49ce7290-344e-48cb-b2f1-97d7972ede41.gif)


**SMB: SAMBA Server**

Samba, Linux ve Unix için standart Windows birlikte çalışabilirlik programıdır. Samba, GNU General Public License altında lisanslanmış Özgür Yazılımdır. 1992'den beri Samba, DOS ve Windows'un tüm sürümleri, OS/2, Linux ve diğerleri gibi SMB/CIFS protokolünü kullanan tüm istemciler için güvenli, istikrarlı ve hızlı dosya ve baskı hizmetleri sağlamıştır. Samba, Linux/Unix Sunucularını ve Masaüstlerini Active Directory ortamlarına sorunsuz bir şekilde entegre etmek için önemli bir bileşendir. Hem etki alanı denetleyicisi hem de normal etki alanı üyesi olarak işlev görebilir. 

Biz de bu projede Samba Server'ı CUPS ile kurduğumuz print server'ın Windows işletim sistemine sahip bilgisayarlar tarafından da görülmesini sağlamak için kullanacağız.

![samba1](https://user-images.githubusercontent.com/107308122/173192394-0ff1ff27-a6e4-44ad-b3fc-bc009844b541.png)

**Gerekli Diğer Kurulumlar**
- Raspberry PI OS kurulumu: Bu kısımda  **Debian Buster** tercih edildi.

Ek olarak; 
Bu tarz bir proje için Raspberry Pi cihazımızın Boot süresini hızlandırmamız verimlilik açısından faydalı olacaktır. Bütün kurulumları yaptıktan sonra ihtiyaç duyulmayan sistem servisleri kapatılarak bu süreden kazanç sağlanacaktır.
