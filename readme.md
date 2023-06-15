# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın. 1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
CEVAP :
SELECT ograd,ogrsoyad,atarih
FROM ogrenci,islem
WHERE ogrenci.ogrno= islem.ogrno

    2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

CEVAP :
SELECT kitapadi, turadi FROM kitap, tur
WHERE kitap.turno = tur.turno
AND (tur.turadi = 'Fıkra' OR tur.turadi = 'Hikaye')

    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

CEVAP :
SELECT ogrenci.ograd,ogrenci.ogrsoyad,ogrenci.ogrno,kitap.kitapadi FROM ogrenci,islem,kitap
WHERE sinif IN('10B','10C') AND
ogrenci.ogrno=islem.ogrno AND kitap.kitapno=islem.kitapno

    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

CEVAP :
SELECT ogrenci.ograd,ogrenci.ogrsoyad,islem.atarih FROM islem
INNER JOIN ogrenci ON ogrenci.ogrno=islem.ogrno

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

CEVAP :
SELECT k.kitapadi, t.turadi FROM kitap as k
INNER JOIN tur as t ON k.turno=t.turno
WHERE turadi='Hikaye' or turadi='Fıkra'

    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

CEVAP :
SELECT o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi FROM ogrenci as o
INNER JOIN islem as i ON o.ogrno=i.ogrno
INNER JOIN kitap as k ON i.kitapno=k.kitapno
WHERE o.sinif IN('10B','10C')
ORDER BY o.ograd

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

CEVAP :
SELECT o.ograd,o.ogrsoyad,i.atarih FROM ogrenci as o
LEFT JOIN islem as i ON o.ogrno=i.ogrno

    8) Kitap almayan öğrencileri listeleyin.

CEVAP :
SELECT o.ograd,o.ogrsoyad,i.islemno FROM ogrenci as o
LEFT JOIN islem as i ON o.ogrno=i.ogrno
WHERE i.islemno IS NULL
ORDER BY o.ograd

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

CEVAP :
SELECT k.kitapno,k.kitapadi,COUNT(i.kitapno) AS 'Alınma Sayısı' FROM kitap as k
INNER JOIN islem as i ON k.kitapno=i.kitapno
GROUP BY k.kitapno
ORDER BY k.kitapno ASC

    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

CEVAP :
SELECT k.kitapno,k.kitapadi,COUNT(i.kitapno) AS 'Alınma Sayısı' FROM kitap as k
LEFT JOIN islem as i ON k.kitapno=i.kitapno
GROUP BY k.kitapno
ORDER BY COUNT(i.kitapno) ASC

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

CEVAP :
SELECT o.ograd,o.ogrsoyad,k.kitapadi FROM ogrenci as o
INNER JOIN islem as i ON o.ogrno=i.ogrno
INNER JOIN kitap as k ON i.kitapno=k.kitapno

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

CEVAP :
SELECT o.ograd,o.ogrsoyad,k.kitapadi,CONCAT(y.yazarad,'',y.yazarsoyad) as 'Yazar AdSoyad' ,t.turadi,i.atarih FROM ogrenci as o
LEFT JOIN islem as i ON o.ogrno=i.ogrno
LEFT JOIN kitap as k ON i.kitapno=k.kitapno
LEFT JOIN yazar as y ON k.yazarno=y.yazarno
LEFT JOIN tur as t ON k.turno=t.turno

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

CEVAP :
SELECT CONCAT(o.ograd,'',o.ogrsoyad) AS 'Öğrenci', k.kitapadi FROM ogrenci as o
INNER JOIN islem as i ON o.ogrno=i.ogrno
INNER JOIN kitap as k ON i.kitapno=k.kitapno
WHERE o.sinif IN('10A','10B')
ORDER BY k.kitapadi

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

CEVAP :
SELECT AVG(kitap.sayfasayisi) AS 'Ortalama Sayfa Sayısı' FROM kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

CEVAP :
SELECT \* FROM kitap
WHERE sayfasayisi > (SELECT AVG(sayfasayisi) FROM kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

CEVAP :
SELECT COUNT(ogrno) AS 'Öğrenci Sayısı' FROM ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

CEVAP :
SELECT COUNT(ogrno) AS 'Toplam Sayı' FROM ogrenci

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

CEVAP :
SELECT COUNT(distinct ograd) AS 'İsim Sayısı' FROM ogrenci

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

CEVAP :
SELECT sayfasayisi FROM kitap
ORDER BY sayfasayisi DESC
LIMIT 1

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

CEVAP :
SELECT kitapadi,sayfasayisi FROM kitap
WHERE sayfasayisi= (SELECT MAX(sayfasayisi) FROM kitap)

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

CEVAP :
SELECT MIN(sayfasayisi) FROM kitap

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

CEVAP :
SELECT kitapadi,sayfasayisi FROM kitap
WHERE sayfasayisi = (SELECT MIN(sayfasayisi) FROM kitap)

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

CEVAP :
SELECT MAX(sayfasayisi) AS 'Dramdaki Max Sayfa Sayısı' FROM kitap as k
INNER JOIN tur as t ON k.turno=t.turno
WHERE t.turadi='Dram'

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

CEVAP :
SELECT SUM(k.sayfasayisi) AS 'Okunan Toplam Sayfa Sayısı' FROM ogrenci as o
INNER JOIN islem as i ON o.ogrno=i.ogrno
INNER JOIN kitap as k ON i.kitapno=k.kitapno
WHERE o.ogrno='15'

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

CEVAP :
SELECT COUNT(ograd) AS 'Öğrenci Sayısı',ograd FROM ogrenci as o
GROUP BY ograd

    26) Her sınıftaki öğrenci sayısını bulunuz.

CEVAP :
SELECT COUNT(ogrno) AS 'Öğrenci Sayısı',sinif FROM ogrenci
GROUP BY sinif

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

CEVAP :
SELECT COUNT(ogrno) AS 'Öğrenci Sayısı',cinsiyet FROM ogrenci
GROUP BY cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

CEVAP :
SELECT o.ograd,o.ogrsoyad,SUM(sayfasayisi) AS 'Sayfa Sayısı' FROM ogrenci as o
INNER JOIN islem as i ON o.ogrno=i.ogrno
INNER JOIN kitap as k ON i.kitapno=k.kitapno
GROUP BY o.ograd,o.ogrsoyad
ORDER BY 'Sayfa Sayısı' DESC

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

CEVAP :
SELECT COUNT(k.kitapno) AS 'Kitap Sayısı',o.ograd FROM ogrenci as o
INNER JOIN islem as i ON o.ogrno=i.OGRNO
INNER JOIN kitap as k ON i.kitapno=k.kitapno
GROUP BY o.ograd
ORDER BY 'Kitap Sayısı' DESC
