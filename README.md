Proje Adı: javafx-blockchain-smartcontract-simulator

Proje Açıklaması: Bu proje kapsamında, Java tabanlı bir Blockchain simülatörüne, işlemlerin hem gönderen hem 
de alıcı tarafından onaylanması gerektiği güvenlik artırıcı bir mekanizma entegre edeceksiniz. 
Java Core bilgilerinizle yapmanız beklenmektedir.

Soru 1: İki Aşamalı İşlem Onay Sistemi (Çift Taraflı Transfer Onayı) 
Senaryo Tanımı: 
Blokzinciri üzerinde yapılan her transfer işleminin, sadece gönderen tarafından değil, aynı zamanda alıcı tarafından da onaylanması gerektiği bir sistem geliştirilecektir. İşlem, yalnızca her iki taraf da onay verdiyse zincire eklenecektir. 

Beklenen Özellikler ve Teknik Gereklilikler: 

1. Transaction sınıfına şu alanlar eklenecek: 
   o boolean fromApproved = false; // Gönderen onayı 
   o boolean toApproved = false; // Alıcı onayı 

2. SmartContract sınıfına şu yöntemler eklenecek: 
   o public void approveTransaction(String walletAddress, String transactionId) 
   → Bu metod üzerinden, kullanıcı kendi adresiyle ilgili olan işlemi onaylayacak. 
   o Yalnızca fromApproved == true && toApproved == true ise pendingTransactions 
   listesinden alınarak bloğa yazılacak. 

3. MainTest.java üzerinde bu iki taraflı onay süreci adım adım simüle edilmeli: 
   o admin cüzdanından kullanıcıya para gönder. 
   o her iki taraf da onay vermeden işlem blok haline getirilmemeli. 
   o onaylar sonrası blok oluşumu test edilmeli.

4. Konsol çıktısında şu bilgiler detaylı yazılmalı: 
   o Onay verilmeden önce bekleyen işlemler. 
   o Onay sırası ve ilgili wallet bilgileri. 
   o Hangi işlem ne zaman blok haline geldi, blok hash bilgisi.

5. Onay verilmezse işlem zaman aşımına uğrasın ve iptal edilsin (timestamp + expiry)

Soru 2: İşlem Limiti ve Kara Liste Sistemi (Anti-Fraud Algoritması) 
Senaryo Tanımı: 
Kötü niyetli kullanıcıların sistemde çok sık işlem yaparak spam oluşturmasını engellemek 
amacıyla aşağıdaki kurallar uygulanmalıdır.
Beklenen Özellikler ve Teknik Gereklilikler:

1. _05_Wallet sınıfına ek alanlar: 
   o int transactionCount; 
   o boolean blacklisted; // Kara liste kontrolü
2. SmartContract sınıfına şu kontroller entegre edilecek: 
   o Bir cüzdan günde maksimum 5 işlem yapabilsin (gerçek zamanlı değilse 
   simülasyon üzerinden işlem sırası baz alınabilir) 
   o submitTransaction() içinde kara liste kontrolü yapılacak: if(wallet.isBlacklisted()) 
   return;
3. Eğer bir cüzdan sınırı aşarsa: 
   o blacklisted true yapılacak 
   o Admin onayı gelmeden tekrar işlem gönderemeyecek
4. Blockchain sınıfında audit() fonksiyonu tanımlanacak: 
   o Tüm bloklardaki işlemleri tarar, kara listedeki cüzdanlardan gelenleri 
   işaretler/loglar
5. MainTest içinde: 
   o 1 cüzdanla 6 işlem yapılmaya çalışılır, 6.sında hata/log düşer. 
   o Admin blacklisted alanını resetleyerek yeniden işlem yapmasına izin verir.

![](C:\Users\tugba\AppData\Roaming\marktext\images\2025-07-06-15-47-19-image.png)