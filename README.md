## Debugging Süreci ##
Sürece başlarken ilk olarak kodları inceledim bu süreçte ip uçlarına dikkat ettim.
Elimdeki kodların neler yapması gerektiğini not ettim ve ui kısmında incelerken bu almış olduğum notlara göre nasıl çalışması gerektiğini inceledim.
Ui kısmında incelerken ilk olarak sepete ekleme işlemlerine baktım. Burada fark ettiğim ilk şey stok  sayısından fazla sepete ürün ekleyebilmem olayıydı.
İlk düzeltmem gereken hatalardan biri buydu. 
İkinci olarak geçersiz kod girdiğimde ekrana birden fazla geçersiz kod yazdırabiliyor olmamdı
bu fatal bir hata değil ama kullanıcıya sürekli böyle bir hata görünmesi kötü bir durum
Üçüncü olarak doğru indrim kodu girildiğinde beklenen %10 indirim olması gerekirken %90 yapıyor olmasıydı.
Aynı zamanda kullanılan kodu tekrar yazar ise indirim zaten uygulandı yazmalı geçersiz kod yerine.
Dördüncü olarak sepete ürün ekledikçe ürün fiyatı çarpı miktar yapıp toplam fiyatı gösterecek kod da düzgün çalışmıyor. Onu da düzeltmeleyim.
Beşinci olarak sepetteki ürünleri sildiğimde yeniden stoğa eklenmemesiydi. Bunu da düzeltmemiz gerekiyor.
Altıncı olarak showError(message) fonksiyonunda hata mesajı gösterimi ekranda kalacağı süreden sonra tanımlanıyordu. Bu mantıksal bir hata 

Kodları Debuglama ve Düzeltme Aşaması 

## İlk hata için ##
const product = products.find(p => p.id === productId); 

if (product.stock <= quantity) { // < yerine <= kullanıldı

const existingItem = this.items.find(item => item.productId === productId); // Breakpoint 3

Bu satırlara dinleyici ekledim ve bu satırdaki kodların çıktılarını gözlemledim.

Birinci Breakpoint den productId değerini product objesini  products arrayının durumunu gözlemledim.
İkinci Breakpoint den product.stock değeri quantity değeri ve Karşılaştırma sonucunu inceledim.
Üçüncü Breakpoint den this.items arrayını sepetteki toplam ürün miktarını sepette ekli olan ürünlerin kaçar tane ekli oldugunu gözlemledim.


## İkinci hata ## 
bu hata için ekstra Breakpoint vs eklenmesine gerek yok çünkü hata çok açık += yapmak yerine = kullansak 
hata mesajları eklemek yerine mevcuttaki hata mesajını yazacak yani ne kadar kod gönderilirse gönderilsin tek hata gösterilecek 
ek olarak showMessage fonksiyonunda oldugu gibi 3 saniyelik bir zaman ekledim böylece mesak 3 saniye sonunda kaybolacak.

## Üçüncü Hata ##
Hata daha önce de belirttiğim gibi yanlış hesap edilip ekrana yazılmasıydı %10 indirim yapmak için 0.9 değeri ile çarpmalıyız.
Ve quantity değerini ekledik böylece eklenen ürün miktarını da hesaba kattık.

        else if(this.discountApplied){
            this.showError('İndirim zaten uygulandı!');
        }

kodunu da ekleyerek kullanılan koda  kullanıldı uyarısı atadık.

## Dördüncü Hata ##
Bu hatayı düzeltmek için 
<span>Birim Fiyat: ${item.price} TL (Toplam: ${item.price * item.quantity} TL)</span>
kodunu ekledim bu kodda birim fiyatına ek sepete eklenen ürün miktarı çarpı birim fiyatı şeklinde toplam fiyatını gösterdim.


## Beşinci Hata ##
Buradaki hata 
document.dispatchEvent(new Event('stockUpdate')); kodunun olmamasıydı ürünler siliyordu ancak mevcut stok bilgisi değişiyordu.
Bu kodu ekleyerek stogun güncellenmeisni sağladık.


## Altıncı Hata # 

    showError(message) {
        const errorElement = document.getElementById('error');
        if (errorElement) {
            errorElement.textContent = message + '\n';
            setTimeout(()=>{
                errorElement.textContent='';
            },3000)
         
        }
    }

Fonksiyonunda hata mesajı süre kısmından önce gösterildi. Fatal bir hata  değil ancak mantıksal bir hataydı.
