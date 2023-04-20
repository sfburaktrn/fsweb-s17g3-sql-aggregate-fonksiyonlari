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
CEVAP: select ograd,ogrsoyad,atarih from ogrenci,islem
where ogrenci.ogrno=islem.ogrno;

    2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    CEVAP:  update tur set turadi=trim(turadi)
    		where turno != "aaaa";//Mysql'de trimledim.

    		select kitapadi,turadi from kitap,tur
    		where kitap.turno=tur.turno and turadi in ("Fıkra","Hikaye");

    3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

    CEVAP:  select ogrenci.ogrno,ograd,ogrsoyad,kitapadi from ogrenci,islem,kitap
    		where sinif in("10B","10C") and
    		ogrenci.ogrno=islem.ogrno and
    		islem.kitapno=kitap.kitapno;

    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    CEVAP:  select o.ograd,o.ogrsoyad,i.atarih from ogrenci as o
    		inner join islem as i on o.ogrno=i.ogrno;

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    CEVAP:  select k.kitapadi,t.turadi from kitap as k
    		inner join tur as t on k.turno=t.turno
    		where turadi in ("Hikaye","Fıkra");

    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    CEVAP:  update ogrenci set sinif=trim(sinif)
    		where ogrno != "aaaa";//mysql de trimledim.

    		select o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi from ogrenci as o
    		inner join islem as i on o.ogrno=i.ogrno
    		inner join kitap as k on i.kitapno=k.kitapno
    		where o.sinif in("10B","10C");

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    CEVAP:  select ograd,ogrsoyad,atarih from ogrenci as o
    		left join islem as i on o.ogrno=i.ogrno;


    8) Kitap almayan öğrencileri listeleyin.

    CEVAP:  select ograd,ogrsoyad,atarih from ogrenci as o
    		left join islem as i on o.ogrno=i.ogrno
    		where atarih is null;

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

    CEVAP:  select kitap.kitapno,kitap.kitapadi,count(islem.islemno) from kitap
    		left join islem on kitap.kitapno=islem.kitapno
    		group by kitap.kitapadi,kitap.kitapno
    		order by kitap.kitapno;

    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

CEVAP: select kitap.kitapno,kitap.kitapadi,count(islem.islemno) as "defa" from kitap
left join islem on kitap.kitapno=islem.kitapno
group by kitap.kitapadi,kitap.kitapno,islem.kitapno
order by defa;

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

    CEVAP:  select o.ograd,o.ogrsoyad,k.kitapadi from ogrenci as o
    		left join islem as i on o.ogrno=i.ogrno
    		left join kitap as k on i.kitapno=k.kitapno;

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

    CEVAP:  select ograd,ogrsoyad,kitapadi,yazarad,yazarsoyad,turadi,atarih from ogrenci as o
    		left join islem as i on o.ogrno=i.ogrno
    		left join kitap as k on i.kitapno=k.kitapno
    		left join yazar as y on y.yazarno=k.yazarno
    		left join tur as t on k.turno=t.turno;


    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

    CEVAP:  select ograd,ogrsoyad,count(islemno) as "Okunan Kitap sayısı" from ogrenci as o
    		left join islem as i on o.ogrno=i.ogrno
    		where o.sinif in ("10A","10B")
    		group by o.ogrno


    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

    CEVAP:  select avg(sayfasayisi) as "Ortalama sayfa sayısı" from kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

    CEVAP:  select kitapadi,sayfasayisi from kitap
    		where sayfasayisi > (select avg(sayfasayisi) from kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

    CEVAP:  select count(ogrno) from ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    CEVAP:  select count(ogrno) as "Toplam Sayı" from ogrenci

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

    CEVAP:  select count(distinct ograd) from ogrenci

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    CEVAP:  select max(sayfasayisi) from kitap

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    CEVAP:  select kitapadi,sayfasayisi from kitap
    		order by sayfasayisi desc
    		limit 1

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    CEVAP:  select min(sayfasayisi) from kitap

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    CEVAP:  select kitapadi,sayfasayisi from kitap
    		order by sayfasayisi
    		limit 1

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

    CEVAP:  select k.kitapadi,k.sayfasayisi,t.turadi from kitap as k
    		left join tur as t on k.turno=t.turno
    		where turadi="Dram"
    		order by k.sayfasayisi desc
    		limit 1

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

    CEVAP:  select sum(sayfasayisi),o.ogrno from ogrenci as o
    		left join islem as i on o.ogrno=i.ogrno
    		left join kitap as k on i.kitapno=k.kitapno
    		where o.ogrno=15

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

    CEVAP:  select ograd,count(ograd) from ogrenci
    		group by ograd

    26) Her sınıftaki öğrenci sayısını bulunuz.

    CEVAP:  select sinif,count(*) from ogrenci
    		group by sinif

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

    CEVAP:  select sinif,cinsiyet,count(cinsiyet) from ogrenci
    		group by sinif,cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

    CEVAP:  select o.ograd,o.ogrsoyad,sum(sayfasayisi) as toplamSayfaSayisi from ogrenci as o
    		inner join islem as i on o.ogrno=i.ogrno
    		inner join kitap as k on i.kitapno=k.kitapno
    		group by o.ogrno
    		order by toplamSayfaSayisi

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

    CEVAP:  select o.ograd,o.ogrsoyad,count(kitapadi) as toplamKitap from ogrenci as o
    		inner join islem as i on o.ogrno=i.ogrno
    		inner join kitap as k on i.kitapno=k.kitapno
    		group by o.ogrno
