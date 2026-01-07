# Deney Sonu Teslimatı

Sistem Programlama ve Veri Yapıları bakış açısıyla veri tabanlarındaki performansı öne çıkaran hususlar nelerdir?

Aşağıda kutucuk (checkbox) ile gösterilen maddelerden en az birini seçtiğiniz açık kaynak kodlu bir VT kaynak kodları üzerinde göstererek açıklayınız. Açıklama bölümüne kısaca metninizi yazıp, kod üzerinde gösterim videonuzun linkini en altta belirtilen kutucuğa yerleştiriniz.

- [X]  Seçtiğiniz konu/konuları bu şekilde işaretleyiniz. **!**
    
---

# 1. Sistem Perspektifi (Operating System, Disk, Input/Output)

### Disk Erişimi

- [X]  **Blok bazlı disk erişimi** → block_id + offset
- [ ]  Rastgele erişim

### VT için Page (Sayfa) Anlamı

- [X]  VT hangisini kullanır? **Satır/ Sayfa** okuması

---

### Buffer Pool

- [X]  Veritabanları, Sık kullanılan sayfaları bellekte (RAM) kopyalar mı (caching) ?

- [X]  LRU / CLOCK gibi algoritmaları
- [X]  Diske yapılan I/O nasıl minimize ederler?

# 2. Veri Yapıları Perspektifi

- [X]  B+ Tree Veri Yapıları VT' lerde nasıl kullanılır?
- [ ]  VT' lerde hangi veri yapıları hangi amaçlarla kullanılır?
- [ ]  Clustered vs Non-Clustered Index Kavramı
- [ ]  InnoDB satırı diskte nasıl durur?
- [ ]  LSM-tree (LevelDB, RocksDB) farkı
- [ ]  PostgreSQL heap + index ayrımı

DB diske yazarken:

- [X]  WAL (Write Ahead Log) İlkesi
- [ ]  Log disk (fsync vs write) sistem çağrıları farkı

---

# Özet Tablo

| Kavram      | Bellek          | Disk / DB      |
| ----------- | --------------- | -------------- |
| Adresleme   | Pointer         | Page + Offset  |
| Hız         | O(1)            | Page IO        |
| PK          | Yok             | Index anahtarı |
| Veri yapısı | Array / Pointer | B+Tree         |
| Cache       | CPU cache       | Buffer Pool    |

---

# Video [Linki](https://www.youtube.com/watch?v=Nw1OvCtKPII&t=2635s) 
Ekran kaydı. 2-3 dk. açık kaynak V.T. kodu üzerinde konunun gösterimi. Video kendini tanıtma ile başlamalıdır (Numara, İsim, Soyisim, Teknik İlgi Alanları). 

---

# Açıklama (Ort. 600 kelime)

Bu çalışmada, veritabanı sistemlerinin performansını etkileyen temel unsurlar sistem programlama ve veri yapıları bakış açısıyla ele alınmıştır. Özellikle disk erişimi, page kavramı, buffer pool mekanizması, indeks yapıları ve WAL prensibi incelenmiştir. Föy-4 kapsamında gerçekleştirilen SQL sorguları ve indeksleme işlemleri, bu kavramların pratikte nasıl performans avantajı sağladığını gözlemlemek açısından bir temel oluşturmuştur.

Disk erişimi, veritabanı sistemlerinde en maliyetli işlemlerden biridir. Geleneksel sabit diskler ve hatta SSD’ler, RAM’e kıyasla oldukça yüksek gecikme sürelerine sahiptir. Bu nedenle modern veritabanı sistemleri diske doğrudan satır bazlı erişim yapmak yerine page bazlı erişim kullanır. Disk üzerindeki veriler, belirli boyutlardaki sayfalar halinde organize edilir ve erişimler block_id ve offset mantığıyla gerçekleştirilir. Bu yaklaşım, diskten yapılan I/O işlemlerinin sayısını azaltarak performansı artırır.

Veritabanları, satır bazlı okuma yapmak yerine sayfa bazlı okuma yapar. Bunun temel nedeni, diskten tek bir satır okumak yerine bir sayfa içerisindeki birden fazla satırı tek seferde belleğe almanın daha verimli olmasıdır. Bu sayede aynı sayfa üzerindeki verilere yapılan sonraki erişimler disk yerine bellekten karşılanır. PostgreSQL gibi veritabanlarında bu yapı açıkça görülmekte ve tüm erişim mantığı sayfa temelli olarak kurgulanmaktadır.

Buffer Pool, veritabanlarının performansını doğrudan etkileyen en önemli bileşenlerden biridir. Buffer pool, diskten okunan sayfaların RAM içerisinde tutulduğu alandır. Sık kullanılan sayfalar bellekte saklanarak, aynı veriye tekrar erişildiğinde disk I/O yapılmasının önüne geçilir. Bu durum, Föy-4’te yapılan sorgularda indeksli sorguların çok daha hızlı çalışmasının temel nedenlerinden biridir.

Buffer pool içerisinde hangi sayfaların bellekte tutulacağı ve hangilerinin bellekten çıkarılacağı belirli algoritmalarla yönetilir. En yaygın kullanılan yaklaşımlar LRU (Least Recently Used) ve CLOCK benzeri algoritmalardır. Bu algoritmalar, yakın zamanda kullanılan sayfaların gelecekte tekrar kullanılma ihtimalinin daha yüksek olduğu varsayımına dayanır. Böylece buffer pool kapasitesi en verimli şekilde kullanılarak disk erişimi minimize edilir.

Veri yapıları perspektifinden bakıldığında, B+ Tree yapısı veritabanı indekslerinin temelini oluşturur. B+ Tree, disk tabanlı sistemler için optimize edilmiş bir ağaç yapısıdır ve dengeli olması sayesinde arama, ekleme ve silme işlemlerini logaritmik zamanda gerçekleştirebilir. Ayrıca yaprak düğümlerin ardışık olarak disk üzerinde tutulması, aralık sorgularında yüksek performans sağlar. Föy-4’te kullanılan indekslerin arka planında bu yapı yer almaktadır.

Veritabanı sistemlerinde veri güvenliği ve tutarlılığı da performans kadar önemlidir. Bu noktada Write Ahead Log (WAL) prensibi devreye girer. WAL yaklaşımında, veriye ait değişiklikler önce log dosyasına yazılır, ardından asıl veri sayfaları güncellenir. Bu sayede sistem çökmesi gibi durumlarda log dosyaları kullanılarak veritabanı tutarlı bir duruma geri döndürülebilir. WAL aynı zamanda sıralı disk yazımı yaptığı için rastgele disk yazımlarına kıyasla performans avantajı da sağlar.

Sonuç olarak, veritabanı sistemlerinin yüksek performanslı çalışabilmesi; disk erişimini minimize eden sayfa bazlı yapı, buffer pool ile etkin bellek kullanımı, B+ Tree gibi disk dostu veri yapıları ve WAL gibi güvenli yazma mekanizmalarının birlikte çalışmasıyla mümkün olmaktadır. Bu çalışma kapsamında ele alınan kavramlar, veritabanı sistemlerinin yalnızca kullanıcı seviyesinde değil, sistem ve veri yapıları düzeyinde de anlaşılmasının önemini ortaya koymaktadır.


## VT Üzerinde Gösterilen Kaynak Kodları

Açıklama [Linki](https://...) \
Açıklama [Linki](https://...) \
Açıklama [Linki](https://...) \
... \
...
