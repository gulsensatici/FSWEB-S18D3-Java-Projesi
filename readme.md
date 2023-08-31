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
    
	

	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
    CEVAP:
	select k.kitapadi, t.turadi from kitap as k, tur as t where k.turno= t.turno and t.turadi in('Fıkra', 'Hikaye')
	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
    CEVAP: 
	select  o.ograd, o.ogrsoyad, o.ogrno, k.kitapadi from ogrenci as o, islem as i , kitap as k
    where i.kitapno=k.kitapno and o.ogrno=i.ogrno
    and sinif in('10A' , '10B')

	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	CEVAP:
    select ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih 
    from ogrenci inner join
    islem on ogrenci.ogrno=islem.ogrno 
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	CEVAP:
    select tur.turadi,kitap.kitapadi 
    from kitap inner join tur on kitap.kitapno=tur.turno 
    where tur.turadi in('Fıkra','Hikaye')
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	CEVAP:
    select ogrenci.ograd, ogrenci.ogrsoyad, ogrenci.ogrno, kitap.kitapadi from ogrenci
    inner join islem on islem.ogrno= ogrenci.ogrno 
    inner join kitap on kitap.kitapno=islem.kitapno
    where ogrenci.sinif in('10B','10C') 
    order by ogrenci.ograd

	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
    CEVAP:
	select ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih from ogrenci
    left join islem on ogrenci.ogrno=islem.ogrno 
	
	8) Kitap almayan öğrencileri listeleyin.
	 CEVAP:
	select ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih from ogrenci
    left join islem on ogrenci.ogrno=islem.ogrno where islem.atarih is null
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	CEVAP:
    select kitap.kitapno, kitap.kitapadi, count(kitap.kitapno) from kitap 
    inner join islem on kitap.kitapno = islem.kitapno
    group by kitap.kitapadi,kitap.kitapno order by kitapno desc
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
    CEVAP:
    select kitap.kitapno, kitap.kitapadi, count(kitap.kitapno) as adet from kitap 
    left join islem on kitap.kitapno = islem.kitapno
    group by kitap.kitapadi,kitap.kitapno 
    order by adet asc

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
    CEVAP:
    select ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi from ogrenci 
    inner join islem on ogrenci.ogrno= islem.ogrno 
    inner join kitap on islem.kitapno=kitap.kitapno
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
    CEVAP:
    select ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi, yazar.yazarad, yazar.yazarsoyad, tur.turadi, islem.atarih from ogrenci 
    left join islem on ogrenci.ogrno= islem.ogrno 
    left join kitap on islem.kitapno=kitap.kitapno
    left join yazar on yazar.yazarno=kitap.yazarno
    left join tur on tur.turno=kitap.turno

	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	CEVAP:
    select ogrenci.ograd, ogrenci.ogrsoyad, count(islem.ogrno) as adet from ogrenci 
    inner join islem on ogrenci.ogrno=islem.ogrno
    where ogrenci.sinif in ('10A', '10B')
    group by ogrenci.ograd, ogrenci.ogrsoyad
   
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
    CEVAP:
    select avg(sayfasayisi) from kitap

	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	CEVAP:
    select kitap.kitapadi, kitap.sayfasayisi from kitap where
    (select avg(sayfasayisi) from kitap)<kitap.sayfasayisi
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
    CEVAP:
	select sum(ogrno) from ogrenci
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	CEVAP:
	select sum(ogrno) as toplamsayi from ogrenci
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	CEVAP:
    select count(ogrenci.ograd) from ogrenci

	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	CEVAP:
    select max(kitap.sayfasayisi) from kitap
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	CEVAP:
    select kitapadi,sayfasayisi from kitap where sayfasayisi=(select max(kitap.sayfasayisi) from kitap)
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	CEVAP:
    select sayfasayisi from kitap where sayfasayisi=(select min(kitap.sayfasayisi) from kitap)
	
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
    CEVAP:
    select kitapadi, sayfasayisi from kitap where sayfasayisi=(select min(kitap.sayfasayisi) from kitap)
	
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
    CEVAP:
    select max(sayfasayisi) from kitap
    inner join tur on kitap.turno=tur.turno where tur.turadi='Dram'
	
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
    CEVAP:
    select sum(sayfasayisi) from islem left join kitap on islem.kitapno= kitap.kitapno where islem.ogno=15
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
    CEVAP:!!! syntax hatası
    selcet ogrenci.ograd, count(ogrenci.ograd) as adet from ogrenci group by ogrenci.ograd order by adet desc
	
	26) Her sınıftaki öğrenci sayısını bulunuz.
    CEVAP:
    select ogrenci.sinif, count(ogrenci.ograd) from ogrenci as ogrenci group by ogrenci.sinif 
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	CEVAP:
    select sinif,cinsiyet, count(*) as ogrsayisi from ogrenci group by sinif,cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	CEVAP:
    select ogrenci.ograd, ogrenci.ogrsoyad, count(islem.kitapno) as 'kitapsayisi' from islem 
    inner join ogrenci on ogrenci.ogrno=islem.ogrno
    group by ogrenci.ogrno,ogrenci.ograd
    order by kitapsayisi desc
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
  