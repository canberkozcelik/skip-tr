# skip-tr

## Memoization  
Programlamada kullanılan bir optimizasyon tekniği olan memoization, kısaca masraflı -işlem süresi uzun sayılabilecek- bir fonksiyona sahip bir programın aynı input değeri gelmesi durumuna karşın sonucu önbellekte tutması, cachelemesi için geliştirilmiş bir tekniktir.  
Çoğu dilde kullanılan bu teknik isim veya şekil değiştirebilir. Örneğin, python ile ilgili olanların dilinden konuşacak olursak bu tekniği decorator özelliği sağlar. Buna aynı zamanda incremental computation da denir.

Bu tekniği kullanan dillerin iki önemli kusuru olduğundan bahsedilir:  
- Programın input değeri değişirse sonucu veya sonuçları dinleyen ya da belirli sonuçlara artık ihtiyacı olmayan dinleyenler (observer) için bütün bağılların (dependency) yeniden hesaplanması.  
- Programın artımlı (incremental) olma özelliğinin unit bazında kalması.  
Skip bu geleneksel memoization tekniği bir adım daha öne taşıyarak sadece fonksiyon sonucunu cachelemek ile kalmıyor, ayrıca demand-computation graph adı verilen bir yapıda hangi parametrelerin hangi dinleyicilerde hangi sırada kullanıldığını tutuyor ve buna bağlı olarak ön belleğe alınmış değerin geçersiz olma durumuna bağlı olarak yeniden hesaplama yapabiliyor.
Bu yapı aslında Adopton adı verilen bir yazılım diline ait bir özellik ve Skip bu özelliği multi-version concurrency control adı verilen bir mekanizma ile genişleterek hafıza kullanımından kazanım sağlıyor (reclaiming memory).  
 
## Side Effect Management
Bir Skip programı iki katmandan oluşur:  
* Memoizing ve okuma işlemlerinin gerçekleştiği Core.  
* Cell adı verilen değişebilir (mutable) objelerin değişikliğinin gerçekleştiği Mutator.  
Core fonksiyonu Cell’deki değeri okuyup sonucu çıkardıktan sonra runtime yukarıda bahsetmiş olduğumuz graph yapısında bu Cell değeri ile fonksiyon sonucu olan memoized değeri bağlar. Böylece Mutator, Cell’e yeni bir değer atadığında runtime bütün memoized değerleri saptayabilir.  

## Safe Parallelism
Yukarıda bahsetmiş olduğum side effect saptanması sayesinde Skip async/await syntax kullanımları ile ergonomik bir asenkron hesaplama desteği sağlar.
