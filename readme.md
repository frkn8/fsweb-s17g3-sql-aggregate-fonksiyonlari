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
	#ilk 3 soruyu join kullanmadan yazın.
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
 CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih
       FROM ogrenci, islem
       WHERE ogrenci.ogrno = islem.ogrno 
       ORDER BY ogrenci.ograd;

2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

CEVAP: SELECT kitap.kitapadi, tur.turadi
       FROM kitap, tur
       WHERE kitap.turno = tur.turno 
       AND tur.turadi IN ('Fıkra', 'Hikaye');

3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

CEVAP: SELECT ogrenci.ogrno, ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi
       FROM ogrenci, islem, kitap
       WHERE ogrenci.ogrno = islem.ogrno
       AND islem.kitapno = kitap.kitapno
       AND ogrenci.sinif IN ('10B', '10C')
       ORDER BY ogrenci.ogrno;

#join ile yazın
4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih
       FROM ogrenci
       INNER JOIN islem ON ogrenci.ogrno = islem.ogrno;

5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

CEVAP: SELECT kitap.kitapadi, tur.turadi
       FROM kitap
       INNER JOIN tur ON kitap.turno = tur.turno
       WHERE tur.turadi = 'Fıkra' OR tur.turadi = 'Hikaye' order by tur.turadi;

6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

CEVAP:  SELECT ogrenci.ogrno, ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi
        FROM ogrenci
        INNER JOIN islem ON ogrenci.ogrno = islem.ogrno
        INNER JOIN kitap ON islem.kitapno = kitap.kitapno
        WHERE ogrenci.sinif = '10B' OR ogrenci.sinif = '10C'
        ORDER BY ogrenci.ograd;

7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih
       FROM ogrenci
       LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno
       ORDER BY ogrenci.ograd;

8) Kitap almayan öğrencileri listeleyin.

CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad
       FROM ogrenci
       LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno
       WHERE islem.atarih IS NULL
       ORDER BY ogrenci.ograd;

9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

CEVAP: SELECT kitap.kitapno, kitap.kitapadi, 
       COUNT(islem.islemno) 
       FROM kitap
       LEFT JOIN islem ON kitap.kitapno = islem.kitapno
       GROUP BY kitap.kitapno, kitap.kitapadi
       ORDER BY kitap.kitapno ASC;

10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

CEVAP: SELECT kitap.kitapno, kitap.kitapadi, 
       COUNT(islem.islemno) 
       FROM kitap
       LEFT JOIN islem ON kitap.kitapno = islem.kitapno
       GROUP BY kitap.kitapno, kitap.kitapadi
       ORDER BY kitap.kitapno ASC;

11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi 
       FROM ogrenci
       INNER JOIN islem ON ogrenci.ogrno = islem.ogrno
       INNER JOIN kitap ON islem.kitapno = kitap.kitapno
       ORDER BY ogrenci.ograd;

12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi, yazar.yazarad,   yazar.yazarsoyad, tur.turadi, islem.atarih
       FROM ogrenci
       LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno
       LEFT JOIN kitap ON islem.kitapno = kitap.kitapno
       LEFT JOIN yazar ON kitap.kitapno = yazar.yazarno
       LEFT JOIN tur ON kitap.turno = tur.turno
       ORDER BY ogrenci.ograd;

13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad, 
       COUNT(islem.kitapno) as okudugu_kitap_sayisi
       FROM ogrenci
       LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno
       WHERE ogrenci.sinif = '10A' OR ogrenci.sinif = '10B'
       GROUP BY ogrenci.ograd, ogrenci.ogrsoyad;

14) Tüm kitapların ortalama sayfa sayısını bulunuz.
#AVG

CEVAP: SELECT AVG(sayfasayisi) FROM kitap;

15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

CEVAP: SELECT kitap.kitapadi, kitap.sayfasayisi 
       FROM kitap
       WHERE kitap.sayfasayisi > (SELECT AVG(sayfasayisi) FROM kitap) 
       ORDER BY kitap.sayfasayisi;

16) Öğrenci tablosundaki öğrenci sayısını gösterin

CEVAP: SELECT COUNT(ogrno) FROM ogrenci;

17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

CEVAP: SELECT COUNT(*) AS toplamsayi FROM ogrenci;

18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

CEVAP: SELECT COUNT(DISTINCT ograd) FROM ogrenci;

19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

CEVAP: SELECT MAX(sayfasayisi) FROM kitap;

20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

CEVAP: SELECT kitapadi, sayfasayisi FROM kitap
       WHERE sayfasayisi = (SELECT MAX(sayfasayisi) FROM kitap);

21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

CEVAP: SELECT MIN(sayfasayisi) FROM kitap;

22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

CEVAP: SELECT kitapadi, sayfasayisi FROM kitap
       WHERE sayfasayisi = (SELECT MIN(sayfasayisi) FROM kitap);

23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

CEVAP: SELECT MAX(sayfasayisi)
       FROM kitap
       INNER JOIN tur ON kitap.turno = tur.turno
       WHERE tur.turadi = 'Dram';

24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

CEVAP: SELECT SUM(kitap.sayfasayisi) 
       FROM ogrenci
       INNER JOIN islem ON ogrenci.ogrno = islem.ogrno
       INNER JOIN kitap ON islem.kitapno = kitap.kitapno
       WHERE ogrenci.ogrno = 15;

25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

CEVAP: SELECT ogrenci.ograd, COUNT(*) AS adet
       FROM ogrenci
       GROUP BY ogrenci.ograd;

26) Her sınıftaki öğrenci sayısını bulunuz.

CEVAP: SELECT sinif, COUNT(*) AS adet
       FROM ogrenci
       GROUP BY sinif
       ORDER BY adet;

27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

CEVAP: SELECT sinif, cinsiyet, COUNT(*) AS adet
       FROM ogrenci
       GROUP BY sinif, cinsiyet
       ORDER BY cinsiyet;


28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad, 
       SUM(kitap.sayfasayisi) AS toplamsayfasayisi
       FROM ogrenci
       INNER JOIN islem ON ogrenci.ogrno = islem.ogrno
       INNER JOIN kitap ON islem.kitapno = kitap.kitapno
       GROUP BY ogrenci.ograd, ogrenci.ogrsoyad 
       ORDER BY toplamsayfasayisi DESC;

29) Her öğrencinin okuduğu kitap sayısını getiriniz.

CEVAP: SELECT ogrenci.ograd, ogrenci.ogrsoyad, 
       COUNT(islem.kitapno) AS kitapsayisi
       FROM ogrenci
       LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno
       GROUP BY ogrenci.ograd, ogrenci.ogrsoyad;